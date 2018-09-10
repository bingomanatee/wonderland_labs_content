There is a default logger embedded with Apiary and implemented via [node-logger](https://github.com/quirkey/node-logger)) that responds to emitted log messages. 

In order to enable logging, set the apiary's `log_file` configuration property to a valid (complete) file path. The file will be created as necessary or appended to with all logging messages. 

The logger responds to `emit('log', 'info|error|debug...', 'message)` which can be called from an ?[Action](actions) or the ?[apiary](Apiary) itself. 

The logger will log messages at the start and end point of all action handlers. Also, ?[long handlers](long_action_failsafe) will be noted with errors.

The actions' emit('log'...) listener is an indirect way of calling apiary.emit('log'... ) so if you want to plug in a listener to log messages all you need to do is to listen for 'log' messages on the Apiary instance.