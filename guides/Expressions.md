# Expressions

These are just a few examples of Expressions that can be very powerful when utilized correctly, and can potentially simplify a setup that doesn't use Expressions significantly.
Please keep in mind that only Actions/Feedbacks that explicitly state they can utilize Expressions can this be used, which is usually limited to Internal functions as Companion doesn't yet support modules using Expressions. Also while Expressions may resemble JavaScript, they NOT, so do not expect something that works in JS to work in Expressions.
Full documentation for Expressions can be found in the 'Getting Started' guide within the Companion Web UI, under the 'Secondary Admin Controls' > 'Expressions' section.

- [Ternary Operator](#ternary-operator)
- [Time Functions](#time-functions)
- [Min & Max Functions](#min--max-functions)
- [Nested Variables](#nested-variables)
- [Template Literal](#template-literal)
- [Modulus Operator](#modulus-operator)

## Ternary Operator

A Ternary Operator takes the form of `CONDITION ? TRUE : FALSE`, and is a way to perform simple if-else logic all within a single line.

It is made up of 3 parts, a `CONDITION`, which is evaluated and expected to return a truthy or falsy value followed by a Question Mark `?`. After this is section to be executed if the condition is evaluated to be truth, followed by a Colon `:` which separates it from to be executed if the condition is evaluated to be falsy.

Example: If you needed a certain colour dependant on what team is currently selected you could use `$(custom:team) === 'Team 1' ? '#FF0000' : '#0000FF'` which would check the `$(custom:team)` variable, if it equals `Team 1` it return the Hex value for Red, if it's anything other than `Team 1` it would return the Hex value for Blue. Another example is if you wanted return text based on if a match has started or not you could use `$(custom:match_time) === '00:00' ? 'Stopped' : 'Started'`.

## Time Functions

Companion provides several utility functions to transform time from seconds or milliseconds into a timestamp, and the reverse. This can be incredibly useful when you want to compare 2 times, such as `$(internal:time_hms)` which is the current time of the system running Companion in hours:minutes:seconds, and some time in the future to create a countdown.

Example: if you wanted to create a countdown to 21:00, you could use the Expression `timestampToSeconds('21:00:00') - timestampToSeconds($(internal:time_hms))` to get the time difference in seconds between the target time and the current time, and then use `secondsToTimestamp` to turn that back into a timestamp, such as `secondsToTimestamp(timestampToSeconds('21:00:00') - timestampToSeconds($(internal:time_hms)))`.

This will create a countdown to that target time, or if that target time has passed it'll start with a minus symbol as it counts so indicate time passed since then.

Because long expressions can be difficult to read, it's possible to create temporary variables used only within the Expression field and use multiple lines for readability. So the previous example can also be written as:

```
now = timestampToSeconds($(internal:time_hms))
target = timestampToSeconds('21:00:00')
secondsToTimestamp(target - now)
```

This will have the same result, but is much more readable and maintainable should you need to make changes in the future, which can be very valuable in situations where a Companion setup is used by multiple people.

Some times when working with time you may or may not want milliseconds, or omit hours, this is where the second value of `secondsToTimestamp` or `msToTimestamp` come in. For example `msToTimestamp('1234567', 'mm:ss')` will show `20:34`, but using `msToTimestamp('1234567', 'mm:ss.S')` will show `20:34.5`.

## Min & Max Functions

One common task is to increasing or decreasing a value, but keeping it within a certain range. For example if you have a Volume value that needs to be 0 to 100, having buttons that just add/subtract 1 could result in the number going negative, or above 100, so using the `min` and `max` functions can be used to keep within that range.

Example: `min($(custom:volume) + 1, 100)` will return the lowest value between the volume + 1, and 100. So if the current `$(custom:volume)` is 51, this will return `51` as `$(custom:volume) + 1` is lower than 100. If you keep pressing the button until `$(custom:volume)` is 100, then `$(custom:volume) + 1` would be 101 and the minimum value would be `100` so it would return 100 and never go above that.

The opposite is also possible, where `max($(custom:volume) - 1, 0)` will subtract 1 from the volume, and if it goes negative then `0` will be returned as that is always higher than a negative value.

This same operation can also be achieved with the previously mentioned Ternary Operator, for example `$(custom:volume) + 1 <= 100 ? $(custom:volume) + 1 : 100` checks if the volume + 1 is less than or equal to 100, and if so returns that, else it returns 100. The choice of using Ternary Operators or `min` and `max` functions comes down to your individual use case and what gives the most readable Expression.

## Nested Variables

Some times it can be beneficial to have one variable nested within another. For example in my vMix module there are a lot of variables generated with the `input_X_PROPERTY` structure, such as `input_vt1_remaining`, `input_vt1_duration`, etc... If you was to have many of these variables on different buttons you can monitor and control the state of an input, and have play/pause buttons that target the input `vt1`. The downside to this is that you will use a lot of buttons, potentially even needing multiple pages to control each input needed, alternative you can use just a single page using nested variables and a few buttons to change what is being nested.

Example: On a button text you can use the Expression `parseVariables('$(vmix:input_$(custom:target_input)_remaining)')` and it will display the time remaining of whatever `$(custom:target_input)` is referencing, which could be `vt1`, or at the press of a button that variable could change to `vt2`, and now every single button has gone from showing the state and controlling `vt1`, to controlling `vt2`.

This opens the possibility of functionality such as using a rotary dial on a StreamDeck+ to cycle through inputs, or using just 1 page showing buttons for input selection, and 1 page to control whatever is the selected input, rather than needing dozens of pages. Another use case is when integrating with Google Sheets, where rather than hard coding a cell variable such as `players_A1`, `players_A2` etc... instead you could use `parseVariables('$(sheets:0_players_$(custom:column)$(custom:row))')` would now allow you to easily change which Column and Row are being referenced by changing the custom variables.

## Template Literal

Template Literals (Template Strings) are a type of text string that provides a convenient way to create a text string and insert dynamic data such as using Variables, or performing Expression functions. Whilst this is not the only way to achieve this, template literals are often the most compact and clear way to write such strings of text.

Template Literals are created by starting and ending the string with backticks `` ` ``, for example `` `This is a Template Literal` ``. Expressions can be used within a Template Literal by surrounding it with `${}`, such as `` `The answer to 1 + 2 is: ${1 + 2}` `` which would result in `The answer to 1 + 2 is: 3`. Note that unlike Variables in Companion which use `$` followed by parenthesis `()`, Template Literal uses `$` followed by braces `{}` to evaluate the content inside.

An example of where this can be useful is to generate filenames for recordings, such as `` `recording_${$(internal:date_iso)}_camera_${$(custom:camera)}.mp4` `` which could result in `recording_2025-09-03_camera_1.mp4`.

## Modulus Operator

The Modulus Operator `%` is returns the remainder of a division operation, for example `4 % 3` would return 1. Where this becomes useful within Companion is when you want to increment a number and when it reaches some maximum value cycle back around to 0.

Example increment a value:
If you're setting a variable with an expression such as `$(custom:selection)` you can use `$(custom:selection) + 1 % 5` so that each time it is run it'll increase the variable by 1, until it hits 5 where it'll rollover back to 0, so it will cycle through the numbers 0 to 4 over and over.

Example cycling through cameras:

```
cameras = [
  'Cam1',
  'Cam2',
  'Cam3',
  'Cam4',
  'Wide',
  'Crowd'
]

nextSelection = ($(custom:selection) + 1) % length(cameras)

nextSelection

```

In this example, we have an array of a set of cameras (which here is written in the Expression but could be stored in a variable to more easily use in multiple places). Every press increments the next selection value by 1 until it reaches the length of the cameras array before returning to 0, and then that value is returned to be saved into the variable.
