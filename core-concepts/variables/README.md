# Variables

Variables are a key component in any useful programming language, and Skript is no exception. They allow you to store data, move data around, make code readable, and much more.

At their core, variables are a label for information. If you've taken algebra in math, you probably understand the concept. Instead of using an actual number, you use `x` or `y` as a stand-in for an unknown or changing number. This is the same concept as variables in programming. Variables are representations, or stand-ins, of unknown or changing data.

### Variables in Skript

In Skript, variables are represented by a variable name contained within two curly braces, like so:

```applescript
set {variable} to 1
set {_variable} to 2
set {hey, what's going on} to "not much, what about you?"
```

Variables can have nearly any name you like. They have have spaces, dashes, symbols, whatever. You can put the whole Bee Movie script into a variable name, if you so choose. There are a few special ways to name variables though.

By default, variables are `global`, which means they can be seen and changed by any part of your script. Often we want variables to exist only in a small section of script, which can be achieved by `local` variables (those that begin with `_`, like `{_variable}`). We'll get further into this difference in the Global and Local page.

### Using Variables

Let's use a very simple example to start with. We have a command that does a small math equation and outputs the answer. However, we have a section of the equation that repeats a few times. Let's make it easier to deal with using variables.

```applescript
command /math <number>:
    trigger:
        # a basic quadratic equation
        send "%10 * (7 * arg-1 - 5)^2 + 3 * (7 * arg-1 - 5) + 4%"
        
        # using variables to clean up
        set {_x} to 7 * arg-1 - 5
        send "%10 * {_x}^2 + 3 * {_x} + 4%"        
```

That's much easier to read! Plus, it means if we need {\_x} again later in the command, we have it ready to use, without having to retype it. And if we want to change `7` to `6`, we only have to change 1 number instead of 2 or more. Be careful with using too many variables, though. If overused, they can make code more cluttered and messy than necessary:

```applescript
         # questionable overuse of variables
         set {_a} to 10
         set {_b} to 3
         set {_c} to 4
         set {_x} to 7 * arg-1 - 5
         send "%{_a} * {_x}^2 + {_b} * {_x} + {_c}%"  
```

### Using Variables to Store Data

Global variables in particular are great for storing and transferring data. Here we'll use a very basic `/home` command system to demonstrate.

```applescript
command /sethome:
    trigger:
        set {home} to player's location
        
command /home:
    trigger:
        teleport player to {home} 
```

Perfect, right? We save the player's location to a variable, and then when they want to go home, we can just use the variable to look up the current location the home is set to.

Well, this is only partially correct. You see, since global variables are, in fact, global, there's only one of them at any time. So if someone else comes along and does `/sethome`, the `{home}` variable now has their home location, and if you use `/home`, you're getting teleported to their home instead!

To solve this, we need to make the variable unique for each player. The best method to do this is to use the player's uuid as part of the variable name. If you use just `player` or `player's name`, you run the risk of the data no longer being useful when the player changes their name. This is why it's always recommended to use uuids, or enable the config option `use player UUIDs in variable names`, which is explained [here](../../auxiliary-guides/useful-config-options.md).

```applescript
command /sethome:
    trigger:
        set {home::%player's uuid%} to player's location
        
command /home:
    trigger:
        teleport player to {home::%player's uuid%} 
```

{% hint style="warning" %}
#### Note on Using Expressions in Variable Names

Notice that in the home example, we're using the expression `player's uuid` in the variable name. Remember, we can't just write that directly in the variable name. Skript would think we literally mean "player's uuid" instead of replacing it with the uuid we actually want.

So we make sure to surround it with `%`, so that Skript know it's an expression, not a literal bit of text.

```applescript
# Correct!
{variable::%player's uuid%} -> {variable::0841f144-e999-42a2-a83e-9ceaf8732de4}

# Incorrect :(
{variable::player's uuid} -> {variable::player's uuid}
```
{% endhint %}

Now each player has a unique home variable that we can get using their uuid. Note the use of `::` in the variable name. This is used to create list variables, which are explained [here](list-basics.md), with a more in- depth explanation [here](../../unfinished/lists/). The main thing to understand is that using `::` means we have much more power over the variable. We can delete all homes at once, we can easily see all the homes that are set, and much more.

```applescript
# clear all homes
clear {home::*}

# broadcast all homes
broadcast {home::*}
```

That's all for the basics. The next section, [Global and Local](global-and-local.md), will explain more about the differences between global variables and local ones, when you should use them, and what benefits each has.
