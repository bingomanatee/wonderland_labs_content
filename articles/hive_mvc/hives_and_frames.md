![frames900.png](/blog_image/frames700.png)

Hive-MVC has three tiers of organizations: ?[Actions](actions), Hives and Frames. 

1. **?[Actions](actions)**, covered in detail elsewhere, respond to a subset of URLs. This can equate to a single URL, or a set of URLS that tie into a single REST endpoint. 
2. **Hives** are collections of Actions. Their purpose is to group actions, and to provide a set of common default configurations for Actions. They may also have their own set of ?[Resources](resources), but generally resources are better grouped at the Frame level.
3. **Frames** are collections of Hives that accomplish a single function, such as  providing a blog, or adding a support functionality such as flash messages or layout mixins. 

In classical MVC terminology, Hives fulfill the role of collections, and frames fulfill the role of modules.

Hives and Frames are largely used internally to organize actions. They can be accessed off of the ?[Apiary](apiary) through `apiary.Frame.list` and `apiary.Hive.list`.

## Running a subset of frames

Both for testing and for "stealth developing" frames you can run a subset of frames. In your app.js file, 

``` javascript
var log_file = path.resolve(__dirname, '../actions.log');

server.listen(app.get('port'), function () {
    var apiary = mvc.Apiary({log_file: log_file, action_handler_failsafe_time: 3000}, __dirname + '/frames');
    apiary.set_config('frame_filter', ['mock_twitter', 'hive_layout', 'hive_messages', 'main']);
    console.log('initializing apiary for port %s', PORT);
    apiary.init(function () {
        console.log('serving');
        apiary.serve(app, server);
    });
});
``` 
the `frame_filter` configuration will limit the frames that are loaded in bootup. 

This is an especially useful way to insulate frames during testing, and to get a handle on frame interdependencies.