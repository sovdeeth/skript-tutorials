# Functions

## What Is a Function?

A function is a bit of code, like you'd put in a command or an event. Unlike commands or events, though, functions don't start running because of something happening in the game. A command might run because someone did `/help`, or an event might run because someone ate a piece of steak, but function run **only when you tell them to.**

For example, let's look at a basic function:

```applescript
function test():
    broadcast "hello world!"
```

This is called a **function definition**. It's composed of the function's name and its code,  as well as some other optional stuff we'll get into later. This function is named "test" and all it does is broadcast "hello world!" when it runs. However,  there's nothing to make it run yet. Let's run it every time a player joins:

```applescript
on join:
    test()
```

Now, whenever someone joins, Skript sees `test()` and runs our function, broadcasting "hello world!". The act of telling Skript to run a function is named a **function call**. We just called the function test, or ran it, by using its function call.

Function can be called from anywhere, and at (nearly) any time. You can use them across files! The only restriction is using them in `on load` events. Be careful there. You can also restrict a function to work only in 1 file by using `local`, which we'll get to later.

{% hint style="info" %}
This might seem a little, well, useless. Why come up with this whole function thing when you can just write the code when you want it? Why couldn't we just write the following?

```applescript
on join:
    broadcast "hello world!"
```

And you'd be right. These two scripts behave exactly the same. In general, you can always just take the code from a function, plop it right into the place you're calling it from with some minor changes, and have it work the same.&#x20;

The main benefits come from when you're using the same code in multiple different locations. Functions allow you to only write that code once.
{% endhint %}

## Function Definitions

Before we move on to the more advanced uses of functions, let's go over how to write one.

```applescript
function functionName():
```

This is a basic function, like we saw before. The only thing that changes here is the `fuctionName`. The first character must be an alphabetic character, eg a letter, but the following characters can include digits and underscores as well. Common practices are to name functions in `camelCase` or `snake_case`.

However, this is rather limiting. What if we want to do something to an object? Say we have a function called `giveTenApples()`. We don't know who to give the apples to! This is where **parameters** come into play.

### Parameters

Parameters are used to give extra information to a function, and look like this:

```applescript
function functionName(parameterName: parameterType = defaultValue):
```

They can be named just like variables, so nearly anything you want. Avoid `:` and `*`, though, they can mess things up. The `parameterType` is the type of the parameter, surprisingly enough. This can be any type you want, but should be pretty specific. Is it a `player`, or a `number`, or maybe even a `inventory`. You can allow lists by making the type plural. If you're not sure what type to use, or you want to allow multiple types, use `object`.

That's a lot, so here's a small example:

```applescript
function giveTenApples(who: player):
    give 10 apples to {_who}
```

Here, we have a parameter named `who` with a type of `player`. We can then use that parameter in our function code, as if it were a local variable: `{_who}`. If the type had been `players`, we would use `{_who::*}`, as it's now a list.

To call this function, we now have to give it the player argument it asks for:

```applescript
on join:
    giveTenApples(event-player)
```

See how the parameter value just goes inside the ()? If you have multiple parameters, you just add them on like a list:

```applescript
on join:
    # a random function that take a player and a number list
    giveTenApples2(event-player, (10, 20, 30)) 
    
function giveTenApples2(player: player, number-list: numbers):
    # imagine code here
```

{% hint style="warning" %}
Note that I used `()` around the number list. This is so that Skript doesn't get confused and think that `10, 20, 30` are all different parameters.&#x20;

If you're ever experiencing errors or weird bugs with your parameters, try making sure they're surrounded with `()`, it can solve a lot of issues.
{% endhint %}

But we skipped over something earlier. We can give parameters **default values**, too.

```applescript
function giveApples(who: player, amount: number = 10):
    give {_amount} of apples to {_who}
```

Now we have a parameter for the amount of apples we're giving to the player. But we don't always want to have to specify how many we're giving, sometimes we want to be lazy. This is where default values come in. If we don't give the function a number when we call it, the function will automatically use `10` instead:

```applescript
on join:
    # gives 10 apples
    giveApples(event-player)
    
    # gives 20 apples
    giveApples(event-player, 20)
```

This is useful when you have a parameter you might need to specify sometimes, but most times it's the same value.

### Return Values

We've looked at functions as ways to simply run code, but one of the most powerful aspects of functions is their ability to return values back to the code they were called from. Here's an example:

```applescript
on join:
    give player customItem((name of player), golden apple)
    
function customItem(name: string, base-item: item) :: item:
    set {_item} to {_base-item} named {_name}
    set lore of {_item} to "&fWelcome back!"  
    return {_item}
```

This would give a golden apple with the player's name and some custom lore to anyone who joins.

There's a few things to pay attention too. First, notice that now we're putting the function call in the `give` effect. It's no longer on its own line. This means we're using it as an `expression`, not an `effect`. Functions are one of, if not the only, syntax in Skript with this property.&#x20;

Secondly, notice how the function definition has changed. We now have this `:: type:` bit on the end, which tells Skript what type the function is going to return. Again, if you make this plural you can return a list.

Finally, notice the new syntax at the end of the function: `return {_item}`. This is how we tell the function what value it should return. In this case, it's `{_item}`. Return will also stop execution of the function there, like `stop` does.&#x20;

{% hint style="warning" %}
Note that you cannot use `wait` in a function that returns a value. It has to return it instantly, without delay.
{% endhint %}

### Local Functions

The final piece of the puzzle is whether the function is **local or global**. By default, functions are always global. This means you put the function definition in one script file, and you can call the function from any other script! It'll always do the same thing. Sometimes, however, you'll want your function to only be used by 1 script file. Perhaps you want to avoid naming conflicts, where you might accidentally have two functions named `subtract()`.&#x20;

This is where **local** functions come into play. By putting `local` in front of your function definition, you can make it so that only _this_ script file can use that function. Any other script file won't even know that it exists. &#x20;

```applescript
[local] function functionName(...)...
```

{% hint style="warning" %}
If there's a global function of the same name, your local function will **always** be prioritized over the global version. If that's a bit confusing, here's an example:

```applescript
# ----------------
# in script-1.sk
local function test():
    broadcast "local!"

on join:
    test()
# ----------------

# ----------------
# in script-2.sk
function test():
    broadcast "global!"

on quit:
    test()
# ----------------
```

When a player joins, the `join` event in `script-1.sk` runs. This calls the **local** function `test()`, which broadcasts `"local!"`.&#x20;

When a player quits, the `quit` event in `script-2.sk` runs. This can't see the local version of `test()`, so it calls the **global** `test()`, which broadcasts `"global!"`.
{% endhint %}

## Full Definition

Now that we've explained all the parts, let's show the whole function at once:

```applescript
[local] function functionName(parameterName: parameterType = defaultValue) :: returnType:
```

Hopefully by now you know all these parts, but let's recap:

* **\[local]**: Whether this function is local or global. (global is default)
* **functionName:** the name of the function, starts with a letter and can use letters, numbers, and underscores
* **parameterName:** Optional. The name of the parameter, follows the same rules as variable names. Will be accessible as a local variable of the same name in the function.
  * **parameterType:** Required if you have a parameter. Tells Skript what type the parameter will be. Use `object` if you're unsure.
  * **defaultValue:** Optional. Allows the function calls to omit this parameter, and Skript will automatically use this value instead.
* **returnType:** Optional, only use if you intend to return something. Again, use `object` if you're unsure. Remember, functions that return values can't use `wait`.

And calling a function looks like this:

```applescript
functionName()
functionName({_parameter1}, {_parameter2})
```

## When Should I Use a Function?

Now that we know _how_ to use a function, let's look at when and why.

### Eliminating Duplicate Code

If you have two or more places where you've found yourself copying code and making minor tweaks, that might be a good spot to use a function. For example, let's look at a teleportation function:

```applescript
function fancyTeleport(entity: entity, destination: location):
    play sound "block.beacon.activate" at {_entity}
    wait 1.85 seconds
    play sound "entity.enderman.teleport" at {_entity}
    teleport {_entity} to {_destination}
```

With this function, we can use the code wherever we're teleporting things:

```applescript
command /spawn:
    trigger:
        fancyTeleport(player, spawn of player's world)

command /warp <text>:
    trigger:
        fancyTeleport(player, {warp::%arg 1%})
```

And if we want to change the sounds, or the delay, we only have to change it in one spot!

### Running Delayed Code Separately

Sometimes you'll find yourself wanting to start a countdown, or maybe a loop with a delay, but not want to stop doing whatever else you're doing. Function are great for this:

```applescript
command startGame:
    trigger:
        broadcast "Starting Game"
        # these function calls start code with delays, but the code after
        # them runs immediately! pretty useful!
        startTimer()
        startCountdown()
        set {seeker} to player
        set {hiders::*} to all players where [input is not player]

function startCountdown():
    loop reversed 10 times:
        broadcast "Starting in %loop-number%"
        wait 1 second
    broadcast "Game has started!

function startTimer():
    wait 10 minutes
    broadcast "Game has ended!"
```

### Organization and Readability

Often, function are just nice to make your code easier and clearer to read. They move chunks of code out of large, busy areas and isolate it so it's easier to see what's happening.

Functions also come with names, which are nice labels for your code so you can clearly see what it's doing.

```applescript
on load:
    loadMinigames()
    loadSurvival()
    loadPrisons()
    resetMines()
    # etc...
```

## Conclusion

To wrap up, functions are very useful tools to have in your belt. They're powerful, versatile, and make your code way more organized and easy to maintain. You can even explore fancier topics, like recursion, if you're brave enough.

I hope now you've got a grasp on how functions work, at least enough to go and experiment on your own. All the tutorials in the world can't teach you what good old trial and error can.

{% hint style="danger" %}
If you didn't like anything about this tutorial, or found it hard to understand, message me on Discord at Sovde#0001, or tag me in the SkUnity discord.
{% endhint %}
