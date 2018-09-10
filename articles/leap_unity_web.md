I am working with the [Leap Motion Controller](https://www.leapmotion.com/) and have come across a "Soft area" in the API. There is not a direct bridge between the Unity Web Player and the Leap Motion controller. This has to do with the fact that the Unity web player is "Sandboxed" to prevent it access to larger hardware, including the Leap. 

Fortunately there is a way around this. 

The [Leap.js library](http://js.leapmotion.com/api_guide) can communicate with the Leap Motion controller, as a web socket. The [documentation](http://js.leapmotion.com/api_guide) describes in detail the API that JS uses to access the Leap. 

## The basic work flow

We will be establishing four points of communication between the Unity Player context and javascript. each will have two functions -- a "listen" and a "say" function. 

These methods will be fed by a Leap.Controller event listener, and will help to limit the flow of information to Unity to that which the Unity player has the processing time to render. 

![leap-unity-web.png](/blog_image/leap-unity-web.png)

## A few words of warning: 

1. The JS API is NOT an exact match to the Unity API. Notably absent are "Frontmost" helpers; you'll have to find the frontmost hand, and it's frontmost finger, yourself. 
2. You will be sending text (or json as text) between the two systems -- javascript and unity. This may become a bit of a bottleneck if large amounts of information are sent back and forth frequently so work to only send back and forth what you need. 
3. Sending more information to Unity than it can handle due to its render cycles is counter productive so we'll need a "handshaking" mechanism between the two environments. 
4. The units and coordinate spaces are different between Unity and Leap.js; specifically, the Z space seems to be reversed. 

## The basic back and forth: Unity and Javascript talking to each other

The interaction between the unity web app and javascript is [well documented in unity](http://docs.unity3d.com/Documentation/Manual/UnityWebPlayerandbrowsercommunication.html). 

In short: 

* **Unity calls javascript** in a 'jsonp-like' method -- you pass it the name of a (global) javascript function and a single argument (string) to pass into that method. 
* **javascript calls Unity** in a similar fashion. It passes a **string** to the **method** of an **object**.

### The Javascript end of the communication

The following script is a javascript script for listenig to and talking to Unity.

``` javascript

function SaySomethingToUnity(message) {
   u.getUnity().SendMessage("LeapManager", "ListenWeb", message);
  // @param LeapManager the name of my Leap script in my application
}

// called by Unity engine
function HearSomethingFromUnity(says) {
   console.log('unity says ', says);
}

```


### The Unity C# scripts

The script below is the unity script for talking to and from Javascript, in C#. 

``` c#

	
	public static void TellWeb (string s)
	{
		//Debug.Log ("TellWeb " + s);
		Application.ExternalCall ("HearSomethingFromUnity", s);	
	}

	public void ListenWeb (string s)
	{
		try {
			JSONObject j = new JSONObject (s);
			AccessData (j, "__root");
		} catch (UnityException ex) {
			TellWeb ("OOPS!" + ex.ToString ());
		}
		ProcessFingers ();
	}

       private void AccessData ( JSONObject j, string context ) { // more later
       }

      private void ProcessFingers(){ // more later
      }

\\ ...
}

```

this is making the presumption that you are sending data back and forth in JSON format. This is not a requirement -- you could use a custom format, CSV, or whatever you want, but JSON is fairly standard so I am running with it. [JSONObject](http://wiki.unity3d.com/index.php?title=JSONObject) is a popular CODEC for JSON in Unity/C#. Note that the inline code file in the wiki page is the one that works in Unity's "Flavor" of C#; use it, not the JSONObject file in Google Code.

## Handling the difference between Unity's FPS and Leaps FPS

Why are we listening to Unity? First off for debugging -- we want confirmation that our data is being received and decoded by our unity scripts. Secondly, because we want the Javascript to "Sleep" when Unity is in a data digestion cycle. the Javascript Leap player can receive hundreds of frames a second; the Unity player can realistically render 30 frames a second. This means that we will either be buffering 10-20 frames in javascript, or just listening to the last one every 10 to 20 frames. 

### Teaching Unity and Javascript to Shake Hands

The simplest way to throttle Javascript is to have a local "state" flag that we flip on when we send a message, then flip off when we receive feedback from Unity. 

* When Javascript sends a command to Unity , we will set a local variable, `unity_working`, to true.
* When unity has received and finished processing the message it will say `frame_done` back. Javascript, on hearing this will set `unity_working` to false. 
* When `unity_working` is set to true, Javascript will not harass the Unity player with data it doesn't have the bandwidth to process. 

Our javascripts now look like this: 

#### javascripts, revised with state. 

``` javascript

var unity_working = false;
function SaySomethingToUnity(message) {
    unity_working = true;
    u.getUnity().SendMessage("LeapManager", "ListenWeb", message);
}

// called by Unity engine
function HearSomethingFromUnity(says) {
    console.log('unity says ', says);
    if (says == 'frame done') {
        unity_working = false;
    }
}

```

## Preparing Frame data for transmission

You could hypothetically transmit every piece of frame data to Unity in a complex block; however it is likely that the information we want to transmit is much smaller, so why force Unity (and yourself) to decompile an entire JSON frame when you have a much simpler dataset to send?

In my case I just wanted the fingers' location; not the location of pointers (sticks and such) or the rotation angle of the fingers, or anything about the palms. Also I wanted to send the bounding box -- given by the InteractionBox property of the frame. 

So here is my "Packing function" in javascript, as well as the general event stuff for Leap:

#### Packing a frame for Unity

``` javsacript

(function () {
    var controller = new Leap.Controller();
    console.log('controller made', controller);
    controller.on('error', function (err) {
        console.log('leap error: ', err);
    })
    controller.on('connect', function () {
        console.log('connection made');
    });
    var errr = false;
    var startTime = new Date().getTime();
    var incTime = startTime;
    var sps = 0;
    controller.on('frame', function (frame) {
        if (unity_working || errr) return;
        if (frame.valid) {
            var out = {hands: []}

            for (var h = 0; h < frame.hands.length; ++h) {

                var hand = frame.hands[h];

                if (hand.valid) {
                    var out_hand = []; // an array of fingers.

                    for (var f = 0; f < hand.fingers.length; ++f) {
                        var finger = hand.fingers[f];
                        if (finger.valid) {
                            var stp = finger.stabilizedTipPosition; // an array of three floats
                            out_hand.push(stp);
                        }
                    }
                    if (out_hand.length) {
                        out.hands.push(out_hand);
                    }
                }
            }
            if (!out.hands.length) return; // hands are not in front of the controller

            try {
                var ib = frame.interactionBox;

                out.width = ib.width;
                out.height = ib.height;
                out.depth = ib.depth;
                var send = new Date();

                SaySomethingToUnity(JSON.stringify(out));
            } catch (e) {
                console.log('error:', out, e);
                errr = e;
            }
        }
    });
    controller.connect();
})(window);

```

The end resulting structure is something like this:

``` json
{
  "width": 300,
  "height" 200",
  "depth": 400",
  "hands": [
      [
         [1.2, -1.3, 20.5], 
         [..],
         [..]
      ],
      [ ...]
   ]
}
```

Hands are an array of hands (usually 0..2), which are an array of fingers (0..5) which are an array of coordinates(3). This highly compressed burst should be a very short string, hopefully improving thorughput. For a larger (more long term) project I'd probably use more readable format like

``` json
{
  position: {
    "width": 300,
    "height" 200",
    "depth": 400"},
  hands: {
    id: "h1",
    fingers: [
     {
        id: "f1",
        position: {x: 1.2, y: -1.3, z: 20.5}
     }, 
     {..} .. {..}
     ]
   },  
   { ... (another hand}
  ]
}

```

Note we are sending a **string** of JSON data, not a raw object. As mentioned before, this is a choice I made as a developer -- you can use whatever string-based encoding you want. 

### Decoding the JSON in Unity

Decoding JSON in Unity is a lot like the "Bad old days" of SAX - processing; we are not automatically given the context of each node as we crawl them recursively, so I try to add some back in by including the name of the parent property in my C# JSON node crawler.

The JSONObject class is a custom class; it was adopted for use in Unity [on the Unity Wiki](http://wiki.unity3d.com/index.php?title=JSONObject). I have put my copy of that code [As A Gist](https://gist.github.com/bingomanatee/7325780). 

This is the JSON parsing code in LeapManager.&ast;

``` c#

	// This code is expanded from the JSONObject documentation in the Unity wiki. 
        // I have added a context parameter that tells you for instance which property
        // contained the leaf value that the obj is representing. Since arrays are anonymous
        // i infer context from the array nesting. 

	void accessData (JSONObject obj, string context)
	{
		
		if (context == "root") {
			fingers = new List<Vector3> ();
		}
		
		switch (obj.type) {
		case JSONObject.Type.OBJECT:
			for (int i = 0; i < obj.list.Count; i++) {
				string key = (string)obj.keys [i];
				JSONObject j = (JSONObject)obj.list [i];
				accessData (j, key);
			}
			break;
			
		case JSONObject.Type.ARRAY:
			// inferring structure from nested array
			foreach (JSONObject j in obj.list) {
				if (context == "hands") {
					accessData (j, "hand");
				} else if (context == "hand") {
					accessData (j, "finger");
				} else if (context == "finger") {
					accessData (j, "finger.value");
				} else {
					accessData (j, context);
				}
			}
			break;
			
		case JSONObject.Type.STRING:
			Debug.Log (obj.str);
			break;
			
		case JSONObject.Type.NUMBER:
			JSONNumber (obj, context);
			break;
			
		case JSONObject.Type.BOOL:
			//Debug.Log (obj.b);
			break;
			
		case JSONObject.Type.NULL:
		//	Debug.Log ("NULL");
			break;
 
		}
	}
	
        // separating out the processing of numeric values. 
        // note this function kicks in both for the interactionBox properties 
        // of the root object and for the deeply nested finger values. 
        // no effort is made to preserve which finger came from which hand
        // as we are simply seeking out the frontmost finger. 

	private void JSONNumber (JSONObject obj, string context)
	{
		//	TellWeb ("NUMBER context: " + context);
		
		if (context == "width") {
			IB_WIDTH = obj.n;
		} else if (context == "height") {
			IB_HEIGHT = obj.n;
		} else if (context == "depth") {
			IB_DEPTH = obj.n;
		} else {	
			//	TellWeb ("finger value: " + context);
			switch (finger_count) {
			case 0:
				current_finger.x = obj.n;
				break;
							
			case 1: 
				current_finger.y = obj.n;
				break;
							
			case 2: 
				current_finger.z = obj.n;
				fingers.Add (current_finger);
				current_finger = new Vector3 (0, 0, 0);
				++finger_count;
				break;
			} // end finger count switch
			++finger_count;
		}

		//	Debug.Log (obj.n);
	}

        // we are assuming every message coming in is a JSON string
        // and further more, is a fingers collection object. 

	public void ListenWeb (string s)
	{
		try {
			fingers.Clear ();
			finger_count = 0;
			JSONObject j = new JSONObject (s);
			accessData (j, "root");
		} catch (UnityException ex) {
			TellWeb ("OOPS!" + ex.ToString ());
		}

                // this (not shown) method takes the fingers list and 
                // uses it -- and the interaction box -- to move a cursor
                // in three-space. 

		ProcessFingers ();
	}

```

## Areas for improvement

In reflection there is no reason to re-broadcast the interactionBox with each frame -- sending it once is enough. 

This is a fairly "rough" script and could probably be refined for a more involved application. It is likely
that more than one message type would be present, which you'd probably want to identify with a `msg_type` parameter. 

For instance I'd like to send the interactionBox in a "box" type message, and send fingers in a "finger" type message. 

Also, there is no effort being made to reflect the fact that a user might (intentionally or accidentally) plug the controller in or out of his machine during the runtime of the application. 

I'd like to create a system that allows you to design an app for both web and native interaction; that is, to allow unity to listen to a controller where it exists in unity, or where it doesn't exist, ask for it from the javascript. It might be a good idea for the active message originator to be the Unity Engine -- i.e., the unity engine asks for some specific information -- hands, fingers, palm position -- and the javascript responds with that information. 

## The full source Gists:

#### LeapManager.cs

This is the Unity C# script that listens for web messages.

<script src="https://gist.github.com/bingomanatee/7325680.js"></script>

#### leapToUnity.js

This script is the "Gateway" between the leap controller's web socket stream and my custom Unity application.

<script src="https://gist.github.com/bingomanatee/7325719.js"></script>

#### unity.js

This script is the result of the unity "Export to web" wizard, with my gateway javascript/unity listen functions tossed in. 

<script src="https://gist.github.com/bingomanatee/7325735.js"></script>

-------

&ast; note there is nothing magical about the name `LeapManager` -- you can call your recipient script target whatever you want. In a more involved application you may even want to send messages to more than one manager script.