# Conditionals

Conditions mean to do something if something else passes some kind of check. Traditionally the check is if something can be narrowed down to true or false answer, like a closed ended question. For example, "Do you have a pen on you?". That can be answered with a simple yes or no (true or false).

Conditions can take form in a few different ways in Skript which will be outlined below with their own pros and cons.

## Dedicated Conditions

Conditions in Skript are pretty straight forward, most things you may want to check have their own dedicated condition. Some other things you may want to check might not have a dedicated condition so you will have to utilize a "generic" condition with an expression. For example let's say that you wanted to check if a player is allowed to fly, you have two options:

```
if player can fly
```

or

```
if player's fly mode is true
```

The first uses the dedicated condition in Skript (notice how it has no `is`, `=`, `contains` etc etc.), the second uses the `fly mode` expression with a "generic" condition, in this case `is/is equal to`.

## Generic Conditions

Generic conditions are used when a dedicated condition does not exist or you have a more specific thing you check for. Things like `is / is not`, `= / !=`, `is between / is not between`, `> / <` are all generic, they just simply check two objects and can be used in the same way as normal:

```
if player's balance < 20
```

## If Statements

The traditional way conditions are used is just by doing

```
if <condition>:
    send "hello"
```

where `<condition>` is the "question" you are asking. Once the condition passes it will run the code inside the section and if there is any code below the section that will be ran too. Keep note of how the indentation changes!

If the condition does not pass, then the code inside the section will be skipped and the code below will be ran.

## Else If

Now what if you need to do something if a condition does not succeed and perform another check? Then you can utilize an `else if`, it operates just like a normal `if`:

```
if <condition>:
    send "hello"
else if <other condition>:
    send "bye"
```

Notice how the `else` is at the same indentation level as the top `if`. If the first condition passes then the code within the first section will run and the code in the second section will be skipped. Now if the first condition does not pass, then the reverse applies; the first section is skipped and the second section is ran.

## Else

If you need to perform a catch-all, whether you don't know all the possible outputs or you just need to catch everything that does not pass, `else` comes in handy. It can either be applied after an `if` or after an `else if`:

```
if <condition>:
    send "hello"
else:
    send ":("
```

or

```
if <condition>:
    send "hello"
else if <other condition>:
    send "bye"
else:
    send ":("
```

As you can likely guess, the `else` will only fire if the previous condition(s) did not pass, it behaves as a catch-all. Keep in mind that since `else` are a catch-all, you __cannot__ chain them like so:

```
if <condition>:
    send "hello"
else:
    send ":("
else:
    send ":)"
```

## Inline Ifs

Inline ifs are a neat thing, but are generally discouraged due to them making code harder to read at a glace and can be easily overlooked. The basis of inline ifs are that you don't include the leading `if` and the ending colon, therefor you dont indent the subsequent lines to make a section:

For example,

```
if player is op:
    send "hello operator"
```

can become

```
player is op
send "hello operator"
```

As there is no section to separate distinct conditions, all code below the condition is ran if it passes and skipped if it does not, which also allows you to chain conditions without indenting:

```
player is op
player is flying
player's gamemode is survival
send "hey no cheating!"
```

Keep note that there is no `else if` or `else` options with this method.

As you can probably see, it can make code harder to read quickly, typically recommended only when golfing.

## Do If

The only purpose of the `do if` effect is to run an effect if a condition passes all in one line, an example being:

```
send "hello" if distance between player and {spawn} <= 10
```

Notice how there is no indentation differences, colons, and how the effect comes first and then the condition.

Keep note that there is no `else if` or `else` options with this method.

## Ternary Operators

Ternary is similar to the `do if` effect, but it's an expression, it's designed to return something if a condition passes, and if it does not pass, then it returns something else. A simple but straight forward example looks something like:

```
send "hello player" if player is not op, else "hello admin" 
```

If it seems a bit hard to understand, let's highlight the expression itself:

```
"hello player" if player is not op, else "hello admin" 
```

This is basically saying, return the text `hello player` if the player is op, and if they are not op (the condition fails), then return `hello admin`.

Ternary can be used to return any object or sets of objects, not just limited to strings:

```
<object> if <condition>, else <object>
```

### Common Use Case

A quite common use case for ternaries in Skript is to toggle a boolean variable, ie if a variable is set to `true`, then set it to `false` and vise versa:

```
set {myVariable} to true if {myVariable} is false, else false
```

See if you can understand how and why that works.
