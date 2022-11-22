# Conditionals

Conditions are ways to control what code in your program executes and what doesn't. In other words, they allow you to do something if another thing passes some kind of check. Traditionally the check is something can be narrowed down to true or false answer, like a yes or no question. For example, "Do you have a pen on you?". That can be answered with a simple yes or no (true or false).

Conditions can take a few different forms in Skript, each of which will be outlined below with their own pros and cons.

## Dedicated Conditions

Conditions in Skript are pretty straight forward: most things you may want to check have their own dedicated condition. Some others might not have a dedicated condition, and instead use a kind of  "generic" condition with an expression. For example, let's say that you wanted to check if a player is allowed to fly. You have two options:

```tcl
if player can fly
```

or

```tcl
if player's fly mode is true
```

The first uses the dedicated condition in Skript (notice how it has no `is`, `=`, `contains` etc etc.); the second uses the `fly mode` expression with a "generic" condition, in this case `is/is equal to`.

## Generic Conditions

Generic conditions are used when a dedicated condition does not exist or you have a more specific thing you check for. Things like `is / is not`, `= / !=`, `is between / is not between`, `> / <` are all generic, they just simply check two objects and can be used in the same way as normal:

```tcl
if player's balance < 20
```

You can see all of the various generic condition syntaxes [here, under the Comparison condition](https://docs.skriptlang.org/conditions.html#CondCompare).:&#x20;

## If Statements

The traditional way conditions are used is the following:

```tcl
if <condition>:
    send "hello"
    
# example:
if player's y-coordinate is greater than 10:
    send "yay!"
```

Here,`<condition>` is the "question" you are asking. If the condition passes it will run the code inside the section, then continue on running the code after the if section. Keep note of how the indentation changes!

If the condition does not pass, then the code inside the section will be skipped and the code below will be ran.

## Else If

What if you want to check some other condition if your first condition fails? Say maybe your check for whether the player's y-coordinate is greater than 10 didn't pass, but now you want to check if it's less than -20. Then you can utilize an `else if`. It operates just like a normal `if`:

```tcl
if <condition>:
    send "hello"
else if <other condition>:
    send "bye"
    
# example:
if player's y-coordinate is greater than 10:
    send "yay!"
else if player's y-coordinate is less than -20:
    send "whoa!"
```

Notice how the `else` is at the same indentation level as the top `if`. If the first condition passes then the code within the first section will run and the code in the second section will be skipped. Now if the first condition does not pass, then the reverse applies; the first section is skipped and the second section is ran.

## Else

If you need to perform a catch-all, perhaps when you don't know all the possible outputs or you just need to catch everything that does not pass, `else` comes in handy. It can either be applied after an `if` or after an `else if`:

```tcl
if <condition>:
    send "hello"
else:
    send ":("
```

or

```tcl
if <condition>:
    send "hello"
else if <other condition>:
    send "bye"
else:
    send ":("
    
# example:
if player's y-coordinate is greater than 10:
    send "yay!"
else if player's y-coordinate is less than -20:
    send "whoa!"
else:
    send "aww :("
```

As you can likely guess, the `else` will only fire if the previous condition(s) did not pass, it behaves as a catch-all. Keep in mind that since `else` are a catch-all, you **cannot** chain them like so:

```tcl
if <condition>:
    send "hello"
else:
    send ":("
else:
    send ":)"
```

## Inline Ifs

Inline ifs are a neat thing, but are generally discouraged as they make code harder to read at a glace and can be easily overlooked. Inline ifs are created by not including the leading `if` and the ending colon, which means you also don't indent the subsequent lines:

For example,

```tcl
if player is op:
    send "hello operator"
```

can become

```tcl
player is op
send "hello operator"
```

Note that inline ifs will skip all of the following code in their section. When they're placed at the root level of an event or command, they will effectively stop all execution if they don't pass. If placed at the root level of a loop, they'll skip to the next value like `continue` does if they don't pass. &#x20;

&#x20;Inline ifs also allows you to chain conditions without indenting, like the following:

```tcl
player is op
player is flying
player's gamemode is survival
send "hey no cheating!"
```

Keep note that there is no way to use `else if` or `else` with inline ifs.

As you can probably see, it can make code harder to read -especially if used often- but can be useful to limit the amount of code that runs in loops or events.

## Do If

The only purpose of the `do if` effect is to run an effect if a condition passes all in one line, an example being:

```tcl
send "hello" if distance between player and {spawn} <= 10
```

Notice how there is no indentation differences, colons, and how the effect comes first and then the condition.

Keep note that there is no `else if` or `else` options with this method.

## Ternary Operators

Ternaries are similar to the `do if` effect, but they're expressions instead. They're designed to return one thing if a condition passes and a different thing if it does not pass. A simple but straight forward example looks something like:

```tcl
send "hello player" if player is not op, else "hello admin" 
```

If it seems a bit hard to understand, let's highlight the expression itself:

```tcl
"hello player" if player is not op, else "hello admin" 
```

This is saying the following:&#x20;

Return the text `hello player` if the player is op.\
If they are not op (the condition fails), then return `hello admin`.

Ternary can be used to return any object or sets of objects, not just limited to strings:

```tcl
<object> if <condition>, else <object>
```

### Common Use Case

A quite common use case for ternaries in Skript is to toggle a boolean variable, ie if a variable is set to `true`, then set it to `false` and vise versa:

```tcl
set {myVariable} to true if {myVariable} is false, else false
```

See if you can understand how and why that works.
