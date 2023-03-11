# The Basics

This page will walk you through creating your first script. If this is too slow for you, feel free to jump ahead to the [Core Concepts](broken-reference/) section. This is just to get you comfortable with the very basics of Skript.

### Creating the Script File

First things first, we need somewhere to write our code. Inside the `plugins/Skript/scripts` folder, create a new text file. Name it `first-script.sk`. The `.sk` tells Skript that this is a script and that it needs to pay attention to it. You can name Skript files whatever you want, as long as they end in `.sk`. Just don't use `-` to start the name, as Skript uses that to denote disabled scripts: `-example.sk`.

{% hint style="info" %}
#### What program should I use to write Skript code?

Any text editor works fine for writing Skript. Some, like Atom, Sublime Text, and Visual Studio Code have themes or extensions that highlight Skript's syntax to make it easier to read.
{% endhint %}

### Writing a Simple Command

Let's start with a simple command, /food. We don't want our players to go hungry, so when they run the command we'll fill up their hunger bar for them. We start by writing out the syntax for a new command. If you want to know more about this, you can skip to [Custom Commands](../core-concepts/commands.md), but most of it isn't necessary yet.

```applescript
# here's our command, just a basic /food.
command /food:
    trigger:
        # this is where the code will go
```

So we have two lines so far, the first one telling Skript that we're making a new command, and that it's named `/food`. The lines with `#` are comments and are just there for explanation; they don't affect the code. Now we need to tell Skript what the command does, which is what `trigger` does. There are other things that commands can have, which you can look at the core concepts page for.

{% hint style="info" %}
#### Indentation

You might also notice we've indented the `trigger:`. This is a really important concept in Skript. Indenting tells Skript what belongs to what. Everything indented after the `command` line belongs to that command, and if you un-indent, Skript thinks it is going to be something else. **The basic rule of thumb is to indent every time you see a colon (`:`)**, but there's more information on indentation [here](../core-concepts/indentation/).
{% endhint %}

So, `trigger` is responsible for holding all the code that runs when the command is called by a player. Let's add some code for it to hold!&#x20;

We want a code that fills the player's hunger bar, but we don't know what to write. We can consult the docs for some help. If you'll indulge me, head to [https://docs.skriptlang.org/](https://docs.skriptlang.org/), click on `Expressions`, and search for "Food" or "Hunger".&#x20;

{% hint style="warning" %}
This may seem annoying, or frivolous, but please actually visit the docs. They're your best tool for finding what you need when you have questions.
{% endhint %}

You should see one result, called Food Level. If you look at the patterns, you can see that `player`, `food`, and `level` all show up in there. It might be a bit hard to read, which is why we have a [syntax reading tutorial](../syntax-types/reading-syntax.md), but it's pretty simple once you know how. For now, we'll just look at the example.

```applescript
set the player's food level to 10
```

Before we get ahead of ourselves, though, we need to talk about `the player`. In commands, the person who executes the command can be referred to as `player`, or `sender`. However, we don't always have `player` to help us out. Sometimes we'll have to get a player from a variable (explained later), or from an event value:

```applescript
set {_player-variable}'s food level to 10
set the food level of event-player to 10
```

But I digress. Let's put this line into our command:

```applescript
command /food:
    trigger:
        set the player's food level to 10
```

### Making the Command Better

Perfect! A simple, easy food command. It doesn't seem very fun though. Let's give the food to the players instead, so they have something to actually eat. See if you can use the docs to find what `effect` we need to give items to players.

Spoilers: it's `give`.

```applescript
command /food:
    trigger:
        give 2 steak to player
```

Much better. Let's add some class into this command though, and tell the player that they were given some food. Bonus points for using the docs to find the effect used to send messages.

```applescript
command /food:
    trigger:
        give 2 steak to player
        send "You received some food!" to player
```

Technically, we could just write `send "You received some food!"` and Skript would know who we meant to send it to, but it's good practice to be explicit in what you're doing when writing code.

{% hint style="info" %}
#### Note on Using Text in Skript

Notice that all the text we want to send is surrounded by `"`. Surrounding the text with `"` is important because it tells Skript that the stuff inside isn't code, it's just some text that shouldn't really be bothered with. Text that's just text and isn't part of the actual code is referred to as a `string` in programming terminology. In Skript you'll hear it referred to as `text` and `string` interchangeably.

Note that there are some rules surrounding text that might be confusing at first. If you want to use `"`, `#`, or `%` in a string, you have to type two of them in a row. This is because Skript uses these symbols for important things, and typing two in a row tells Skript to just treat it as one, normal, non-code character.

```applescript
send "this is a ""test"" of percent signs (%%) and hash tags (##)" to player

# this sends: 
# this is a "test" of percent signs (%) and hash tags (#)
# to the player
```

You might encounter strings with code inside them, surrounded by `%` signs. This will be explained a little later in this tutorial, but it's basically just telling Skript that there's actually some code to run here, and the result of that code will end up in the string.

```applescript
send "5 + 10 = %5 + 10%" to player

# this sends:
# 5 + 10 = 15
# to the player
```

You can read more about the things you can and can't do with strings [here](../core-concepts/text.md).
{% endhint %}

### If/Else

Now let's get fancy and make it so operators get golden apples instead. We can check whether a player is an op with the following condition:

```applescript
if player is an op:
```

Conditions are syntax elements that are essentially yes/no questions, `player is an op`, `block at player is dirt`, `name of player's tool is "hello!"`. We use these inside if statements to control what code gets run:

```applescript
command /food:
    trigger:
        if player is an op:
            give 2 golden apples to player
            send "You received some food!" to player
        else:
            give 2 steak to player
            send "You received some food!" to player
```

See how the indentation shows what it supposed to run? If the player is an op, we know to run that indented code after the `if`. If they aren't, we can jump straight to the `else` and run that code instead. This is called an `if/else`, and is a basic concept across nearly every programming language. It's the most basic way of controlling what code gets run, called `control flow` in programming jargon.

### Bringing in Variables

You may have noticed that we have some duplicate code going on here. Generally, you don't want to ever write the same (or very similar) code twice. It makes your code long, harder to read, and a lot harder to maintain, because if you want to change the food message, you now have to change it in two places, or possibly more if you added other if/elses.

Let's make this better. First, we can just move the messages outside of the if/else. They're the same in both sections, so we only need one version:

```applescript
command /food:
    trigger:
        if player is an op:
            give 2 golden apples to player
        else:
            give 2 steak to player
        send "You received some food!" to player
```

Un-indenting the send pulls it out of the `else`, meaning it will be run whether the player is op or not. The give is a bit more complicated. Honestly, this code is fine, it's very little duplication and wouldn't be that bad. But let's keep going for the sake of teaching you about variables.

Variables are extremely useful. They basically store data for you so you can keep using it again and again. They come in two main types, global and local variables. Global variables can be used anywhere in any script, while local variables can only be used in the command/event they were defined in. They're surrounded by `{}` and can be named pretty much whatever you want. Variables that start with `_`, like `{_local}`, are local variables. Everything else is global.

```applescript
command /food:
    trigger:
        if player is an op:
            set {_item} to 2 golden apples
        else:
            set {_item} to 2 steak
        give {_item} to player
        send "You received some food!" to player
```

Here, we've set the different items that the player will get to the `{_item}` variable. We can then use that after the `if` to give the right item to the player. If they're an op, `{_item}` will be set to 2 golden apples, otherwise it'll be 2 steak.

Variables are explained further [here](../core-concepts/variables/), in the [Core Concepts](broken-reference/) section.

### Expressions in Text

Since we have our food in a convenient variable now, let's also tell the player exactly what food they received. We can put the result of any expression or variable into text using `%` signs.

```applescript
command /food:
    trigger:
        if player is an op:
            set {_item} to 2 golden apples
        else:
            set {_item} to 2 steak
        give {_item} to player
        send "You received %{_item}%!" to player
```

The `%%` tell Skript to pay attention and read the stuff inside as code instead of just as text. This means we can just put `{_item}` in there and "2 steaks" or "2 golden apples" will automatically be sent to the player. `%%` can also be used in variable names, like`{variable::%player's uuid}`, which is a good way to make global variables specific to one player. Global and local variables are explained [here](../core-concepts/variables/global-and-local.md).

{% hint style="warning" %}
However, %% should only ever be used in those two situations: inside a string, or inside a variable. You should not be using it outside of that.

```applescript
# BAD
send "test" to %player%
set %player's tool% to stone

# GOOD
send "%event-item%" to player
set {_tool-text} to "%player's tool%"
```
{% endhint %}

### Using Events

We've been using a command all this time, but events are another extremely useful way to trigger code in Skript. Events are things that happen when certain, well, events happen in the game. Say the player jumps. There's an event for that. Say a furnace burns a piece of coal. There's an event for that. Say a sheep regrows its wool. There's an event for that. You can see all the events at [https://docs.skriptlang.org/events.html](https://docs.skriptlang.org/events.html). You can also learn more about them in the[ Events page](../syntax-types/events.md).

Events are kind of like commands in that they're never indented. Commands and events always start all the way to the left. Since we've been giving players food, let's use an `on consume` event:

```applescript
on consume:
    # code here
```

Notice that we didn't need to use `trigger` here. This is because commands can have a lot of other attributes, like permissions, cooldowns, and aliases, while events are just events. There's no need for the trigger section to differentiate.

We can also make this event a bit more specific. Let's say we want to give the player regeneration when they eat rotten flesh, to encourage cannibalism. We can use this, instead:

```applescript
on consume of rotten flesh:
    # code here
```

Now it'll only trigger when someone eats rotten flesh, rather then when they eat any item. Note that this is nearly identical to using an if statement at the start of the event:

```applescript
on consume:
    if event-item is rotten flesh:
        # code here
```

{% hint style="info" %}
### Event Values

See `event-item`? This is what's called an `event-value`. These are used to get information about the event. They follow the format of `[event-]%type%`, where type is, well, the type. If you want to know the event-values of an event, find it on the [SkriptHub](https://skripthub.net/docs/) or [SkUnity ](https://docs.skunity.com/syntax/)docs.

Here, we're getting the `event-item`, which is the item being consumed. The `consume` event also has `event-player`, too, which we'll use as `player`.
{% endhint %}

Let's use the first one, it's cleaner and actually runs slightly better. Anyway, we need to give the player regeneration. You probably already know about `/effect give`, and we could use that if we wanted. But we should stick to Skript syntax as much as possible. After a quick search on the docs, we find the following:

```applescript
apply swiftness 2 to the player


# for those curious about the /effect give version:
execute console command "/effect give %player% regeneration"
```

Pretty simple. Let's just change that to `regeneration 1` and add it to our code. We'll also give it a duration of 5 seconds.

```applescript
on consume of rotten flesh:
    apply regeneration 1 to the player for 5 seconds
```

Perfect! Now we get regen while eating flesh. If you eat multiple in a row though, the effect's duration starts to stack. This may be something you want, and you can keep it like this. If you want the timer to never go above 5 seconds, though, you need to add `replacing the existing effect`

```applescript
on consume of rotten flesh:
    apply regeneration 1 to the player for 5 seconds replacing the existing effect
```

### Conclusion

Congrats on writing your first script! This was only a small part of what you can do with Skript, but I hope it helped you get your feet wet. To learn more, check out the [Core Concepts](broken-reference/) pages for more in-depth tutorials. You should also become familiar with the [official docs](https://docs.skriptlang.org/index.html), or with the [SkriptHub docs](https://skripthub.net/docs/) if you prefer those. They'll help you tremendously.

Happy Skripting!

{% hint style="danger" %}
If you had trouble following along, or didn't like this tutorial for some reason, please either message me on Discord at Sovde#0001, or open an issue on the github repository for this site.
{% endhint %}
