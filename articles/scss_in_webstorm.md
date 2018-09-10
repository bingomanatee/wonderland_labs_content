Using [SCSS](http://sass-lang.com/) in [Webstorm](http://www.jetbrains.com/webstorm/) is useful ... with a bit of work. Otherwise, you have to use Scout, and that's a pain; in innumerable cases I have forgotten to start Scout and wondered why my CSS was not working properly. Also, using the technique below, your project will work on any deployed system right away without external dependencies, for any WebStorm user. 

## SCSS?

SCSS is the most recent dialect of [SASS](http://sass-lang.com/). SCSS (Syntactically Awesome Style Sheets) allows you to do three things that clear up CSS very well:

1. nest your style sheet designations
2. Abstract values into variables
3. Compute values by using math or functions on these variables

#### A typical SCSS file. 

Here is a sample SCSS file. Note how the `$icon_width` is multiplied to reuse the icon sprite image. If for instance the proportions of your icon grid changed (as indeed it did for me) you will only have to change one value. 

``` scss

$icon_width: -60px;
$icon_button_width: 30px;

#thing_editor {

  .ui {
    width: 500px;
    padding: 0px;
    margin: 0px;
    float: left;

    td {
      text-align: center;
      vertical-align: middle;
      padding: 0;
      margin: 0;
    }

    table,
    tr {
      margin: 0;
      padding: 0;
      border: none;
    }

    .draw_buttons button {
      width: $icon_button_width;
      height: $icon_button_width;
      background-image: url(/img/admin/nerds/create_game/draw_icons.png);
      background-position: left top;
      background-repeat: no-repeat;
      -webkit-appearance: none;
      border: none;
      background-color: transparent;
    }

    .draw_buttons {
      button.rectangle {
        background-position: left top;
      }
      button.rectangle:hover {
        background-position: left $icon_width;
      }
      button.rectangle.active {
        background-position: left $icon_width * 2;
      }

      button.circle {
        background-position: $icon_width top;
      }
      button.circle:hover {
        background-position: $icon_width  $icon_width;
      }
      button.circle.active {
        background-position: $icon_width  $icon_width * 2;
      }

      button.triangle {
        background-position: $icon_width * 2 top;
      }
      button.triangle:hover {
        background-position: $icon_width * 2 $icon_width;
      }
      button.triangle.active {
        background-position: $icon_width * 2 $icon_width * 2
      }

      button.polygon {
        background-position:  $icon_width * 3 top;
      }
      button.polygon:hover {
        background-position:  $icon_width * 3 $icon_width;
      }
      button.polygon.active {
        background-position:  $icon_width * 3  $icon_width * 2 ;
      }
      button.remove {
        background-position: $icon_width * 4 top;
      }
      button.remove:hover {
        background-position: $icon_width * 4 $icon_width;
      }
      button.remove.active {
        background-position: $icon_width * 4 $icon_width * 2;
      }
      button.move_up {
        background-position: $icon_width * 5 top;
      }
      button.move_up:hover {
        background-position: $icon_width * 5 $icon_width;
      }
      button.move_up.active {
        background-position: $icon_width * 5 $icon_width * 2;
      }
      button.move_down {
        background-position: $icon_width * 6 top;
      }
      button.move_down:hover {
        background-position: $icon_width * 6 $icon_width;
      }
      button.move_down.active {
        background-position: $icon_width * 6 $icon_width * 2;
      }
    }

    .color {
      width: 9em;
    }
    .color input {
      width: 8.5em;
    }
  }

  .btn-toolbar-header {
    margin-top: 10px;
    margin-bottom: 3px;
  }

  .thing_canvas {
    width: 400px;
    height: 400px;
    border: 1px solid black;
    background-color: white;
  }

  .details {
    float: left;
    width: 300px;
    margin-right: 1em;
  }

  .details .control-label {
    width: 80px;
  }

  .details.form-horizontal .controls {
    margin-left: 90px;
  }

}
```

#### Resulting CSS

here is the generated CSS that results.

``` css
#thing_editor .ui {
  width: 500px;
  padding: 0px;
  margin: 0px;
  float: left; }
  #thing_editor .ui td {
    text-align: center;
    vertical-align: middle;
    padding: 0;
    margin: 0; }
  #thing_editor .ui table, #thing_editor .ui tr {
    margin: 0;
    padding: 0;
    border: none; }
  #thing_editor .ui .draw_buttons button {
    width: 30px;
    height: 30px;
    background-image: url(/img/admin/nerds/create_game/draw_icons.png);
    background-position: left top;
    background-repeat: no-repeat;
    -webkit-appearance: none;
    border: none;
    background-color: transparent; }
  #thing_editor .ui .draw_buttons button.rectangle {
    background-position: left top; }
  #thing_editor .ui .draw_buttons button.rectangle:hover {
    background-position: left -60px; }
  #thing_editor .ui .draw_buttons button.rectangle.active {
    background-position: left -120px; }
  #thing_editor .ui .draw_buttons button.circle {
    background-position: -60px top; }
  #thing_editor .ui .draw_buttons button.circle:hover {
    background-position: -60px -60px; }
  #thing_editor .ui .draw_buttons button.circle.active {
    background-position: -60px -120px; }
  #thing_editor .ui .draw_buttons button.triangle {
    background-position: -120px top; }
  #thing_editor .ui .draw_buttons button.triangle:hover {
    background-position: -120px -60px; }
  #thing_editor .ui .draw_buttons button.triangle.active {
    background-position: -120px -120px; }
  #thing_editor .ui .draw_buttons button.polygon {
    background-position: -180px top; }
  #thing_editor .ui .draw_buttons button.polygon:hover {
    background-position: -180px -60px; }
  #thing_editor .ui .draw_buttons button.polygon.active {
    background-position: -180px -120px; }
  #thing_editor .ui .draw_buttons button.remove {
    background-position: -240px top; }
  #thing_editor .ui .draw_buttons button.remove:hover {
    background-position: -240px -60px; }
  #thing_editor .ui .draw_buttons button.remove.active {
    background-position: -240px -120px; }
  #thing_editor .ui .draw_buttons button.move_up {
    background-position: -300px top; }
  #thing_editor .ui .draw_buttons button.move_up:hover {
    background-position: -300px -60px; }
  #thing_editor .ui .draw_buttons button.move_up.active {
    background-position: -300px -120px; }
  #thing_editor .ui .draw_buttons button.move_down {
    background-position: -360px top; }
  #thing_editor .ui .draw_buttons button.move_down:hover {
    background-position: -360px -60px; }
  #thing_editor .ui .draw_buttons button.move_down.active {
    background-position: -360px -120px; }
  #thing_editor .ui .color {
    width: 9em; }
  #thing_editor .ui .color input {
    width: 8.5em; }
#thing_editor .btn-toolbar-header {
  margin-top: 10px;
  margin-bottom: 3px; }
#thing_editor .thing_canvas {
  width: 400px;
  height: 400px;
  border: 1px solid black;
  background-color: white; }
#thing_editor .details {
  float: left;
  width: 300px;
  margin-right: 1em; }
#thing_editor .details .control-label {
  width: 80px; }
#thing_editor .details.form-horizontal .controls {
  margin-left: 90px; }
```

### SCSS vs. SASS

SASS uses the DRY style of replacing braces with intents /whitespace. This is not very good in my mind. 
SCSS uses CSS-style braces(`{..}`); essentially you can paste in normal CSS into a SCSS file and it will compile. This makes adopting SCSS around public domain/open source CSS files MUCH easier. 

## Using SCSS in WebStorm

The WebStorm Documentation covers SCSS and LESS in the same documentation and does so incompletely. 

Here is what I did to get my SCSS to autocompile. 

1. **install scss**. I did this with node. Unlike the documentation, i installed `node-sass`. (`npm install sass` doesn't install the proper module). Note that despite the name, `node-sass` will compile either `*.sass` or `*.scss` files. You do NOT have to install node-sass into your working directory as long as you install it somewhere you can find in the next steps. 

2. **Start a watch process**. a pop-up dialog will start this process, but won't necessarily properly prepare your environment. I would suggest using the values below to create your own file watcher.

![watch_scss.png](/blog_image/watch_scss.png)

3. **create a file with a suffix `*.scss` in your watched directory

4. **change it and watch the result in a `*.css` file. 

### Some notes on the watcher


#### The Argument field

Arguments to the node binary

``` bash
--no-cache --update $FileDir$/$FileName$  $FileDir$/$FileNameWithoutExtension$
```

is fundamentally different than what you will get when automatically configuring SCSS using the WebStorm "Wizard". The wizard will put colons in the list; that may be appropriate for the SCSS/SASS "gem" but is not how node expects its arguments to be fed.

#### The Program field

The program field links to a file that is, again, different from what the WebStorm documentation describes. (this is probably because the node module changed API/profile due to development.) You will e linking to a script hidden in the `bin` folder of the module. 

## Show Console dropdown

This is a personal preference, but I set the Show Console dropdown to "Always" so I can "hear it crunch".