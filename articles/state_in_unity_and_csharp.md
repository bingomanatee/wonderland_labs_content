Tracking the state of things in code is significant in any project and in games, there are many contexts in which state of a gameObject (or the application as a whole) needs to be tracked and in which changes of state must trigger other downstream changes. 

## What is state?

A classic example is tracking the state of a guard in a stealth/shooter game. 

* Initially guard bots typically follow a pathfinding routine to go from point to point on patrol.
* If they hear a noise they are put into "suspicious mode" for a period (or permanently). 
* Depending on the nature of the game, after a few minutes of fruitless search, the guard may return to their patrol. 
* If they get direct visual contact with the player's avatar they go into attack mode and/or "run to the alarm" mode to summon more enemies. 
* At some point, if the player kills/disables the guard they enter a terminal mode and become a rag-doll roadbump. In almost every game at some point to keep the piles of bodies down, the guard will then enter into a final state and get destroyed. 

Each of these states implies a broad set of behaviors regarding the guards' moving, targeting of player, attempt to find an alarm switch (because guards are never given walkie talkies, which are expensive), or desire to lie in a puddle of their own blood. 

More importantly, transitioning between one state and another sometimes requires other actions to be triggered, depending on what both the previous state was and what the state was changed to. For instance, when a guard transitions from search (back) to pathfinding, you might ask the guard to issue a "I guess that was nothing" animation.

## State in C#

C# is a very powerful flexible language and while you can [create a formal state engine](http://www.dofactory.com/net/state-design-pattern) its easier to make (and probably maintain) an "Ad Hoc" state machine by combining enums and properties. 

If we were to model the above pattern in a C#/Unity MonoBehaviour it might look like this: 

``` C#

public enum GuardState {
  Asleep,
  Patrolling,
  Searching,
  Attacking,
  RunningForAlarm,
  Dead
}

public class Guard : MonoBehaviour {
   public GuardState initialState = GuardState.Patrolling; 
   // something you can set in the editor to give each guard a custom initial state. 
   // in this way the start state doesn't trigger any transitional conditions. 

   public GuardState state {
     set {
       switch (_state) { the previous state of the guard
        case GuardState.Patrol: 
           if (value  == GuardState.Searching) PlayShrugAnimation();
           GoToNextWaypoint();
         break;
        
       } // end switch


     _state = value;

    } // end set
         get {
           return _state;
       } // end get

   } // end state property

   void Start() {
    _state = initialState;

    }

   void Update() {
      switch state {
           case GuardState.Patrol: 
              Patrol();
           break;

           case GuardState.Search:
              SearchForPlayer();
           break;
           // and so on.

       } // end state

    } // end update

// not shown: the behavior methods like SearchForPlayer() etc. 
} // end Mono

`````

The way C# treats properties gives you simultaneous access to the previous value of the property (previous state) and the value that the property is being changed to (the keyword `value`) which delivers a great opportunity to manage transitions between states without a huge overhead of class-driven abstraction code. 

As shown here the core language constructs of enum's and the get/set pattern of [C# properties](http://msdn.microsoft.com/en-us/library/aa288470%28v%3Dvs.71%29.aspx) give you all the power you need to control state transitions and reflection. 

The only thing that might be considered absent in this pattern is the ability to enforce only specific transitions -- i.e., there may be combinations of patterns you don't want to allow. You can create state transition masks by using Get and Set methods (classic OOP) for your state and keeping its state property private. 

``` C#

// same class as above .... assume state is no longer public

bool SetState(GuardState newState) {
  if (AllowStateChange(state, newState)){
    state = newState; // triggering all of the above consequences
  } else {
    return false;
  }
}

bool AllowStateChange(GuardState fromState, GuardState toState) {
  if (fromState == GuardState.Dead) return false. // not mostly dead....
  
  if (fromState == GuardState.Searching) {
    switch (toState) {
     // all the legal transitional states from searching
      case GuardState.Dead:
         return true;
      break;
      case GuardState.Attacking:
         return true;
      break;
      case GuardState.Patrolling:
         return true;
      break;
      case GuardState.RunningForAlarm:
         return true;
      break;
     // ban the other ones. 
      default: 
           return false;

    }


  }

  return true;
}

GuardState GetState(){ return state; }

```

You may not always need to constrain state changes to a fixed number of combinations but when you do, the above pattern is a pretty minimal way to get a little more control over what state changes can or cannot be allowed.