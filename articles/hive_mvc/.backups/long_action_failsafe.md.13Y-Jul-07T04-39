There is a timeout for each ?[action script handler](action_scripts) that will pre-emptively return an error if an action takes too long. 

Actions can take too long for any of many reasons: 

1. The action was badly scripted and its handler ended without calling the callback
2. A long task legitimately needs extra time to accomplish its activity, such as writing or searching a long file. 
3. An external process, such as a network or a database, fails badly/loops
4. Directly routing via the `context.$res`'s handlers

If any of these situations occur an error will be sent back to the response.

Note -- the timeout will not actually halt execution of the long process. If the process does eventually execute, its effects will be real -- they just won't be returned to the browser. 

If your application has consistently long processes, consider putting them in a separate unix process. 

## Adjusting the failsafe time

An action that has an actual need for a longer execution cycle can be given more time to execute by adding a `handler_failsafe_time` property to its' configuration, with an integer value -- number of milliseconds to wait before sending a message.

The timeout can be adjusted globally by setting an ?[Apiary's](apiary) `action_handler_failsafe_time` property.