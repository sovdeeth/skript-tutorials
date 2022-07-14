# The Basics

This page will walk you through creating your first script. If this is too slow for you, feel free to jump ahead to the [Core Concepts](<../README (1).md>) section. This is just to get you comfortable with the very basics of Skript.

### Creating the Script File

First things first, we need somewhere to write our code. Inside the `plugins/Skript/scripts` folder, create a new text file. Name it `first-script.sk`. The `.sk` tells Skript that this is a script, and is important. You can name Skript files whatever you want, as long as they end in `.sk`. Just don't use `-` to start the name, as Skript uses that to denote disabled scripts: `-example.sk`.

Now you've got an empty file to play around in. You can write Skript in whatever editor you want, Notepad, Sublime, etc. Personally, I use Visual Studio Code as there are some nice formatting extensions for it. It's up to you.

### Writing a Simple Command

Let's start with a simple command, /food. We don't want our players to go hungry, so when they run the command we'll fill up their hunger bar for them. We start by writing out the syntax for a new command. If you want to know more about this, you can skip to [Custom Commands](../core-concepts/commands.md), but most of it isn't necessary yet.

```bash
# here's our command, just a basic /food.
command /food:
    trigger:
        # this is where the code will go
```

So we have two lines so far, the first one telling Skript that we're making a new command, and that it's named `/food`. Now we need to tell Skript what the command does, which is what `trigger` does. There are other things that commands can have, which you can look at the core concepts page for.&#x20;

You might also notice we've indented the `trigger:`. This is a really important concept in Skript. Indenting tells Skript what belongs to what. Everything indented after the `command` line belongs to that command, and if you un-indent, Skript thinks it is going to be something else. **The basic rule of thumb is to indent every time you see a colon (`:`)**, but there's more information on indentation [here](../core-concepts/indentation.md).

So trigger is responsible for holding all the code that runs when the command is called by a player. Let's add a line that fills up a player's hunger bar. We can consult the docs for this one. Head to [https://docs.skriptlang.org/](https://docs.skriptlang.org/), click on `Expressions`,  and search for "Food" or "Hunger".&#x20;

You should see one result, called Food Level. If you look at the patterns, you can see that `player`, `food`, and `level` all show up in there.  It might be a bit hard to read, which is why we have a [syntax reading tutorial](../syntax-types/reading-syntax.md), but it's pretty simple once you know how. For now, we'll just look at the example.&#x20;

```bash
set the player's food level to 10
```

We can also do some addition and subtraction really easily:

```bash
add 2 to the player's food level
subtract 3 from the player's food level
```

Before we get ahead of ourselves, though, we need to talk about `the player`. In commands, the person who executes the command can be referred to as `player`, or `sender`. However, as we'll see later on, we don't always have `player` to help us out. Sometimes we'll have to get a player from a variable (explained later), or from an event value:

```bash
set {_player-variable}'s food level to 10
set the food level of event-player to 10
```

But I digress. Let's put this line into our command:

```bash
command /food:
    trigger:
        set the player's food level to 10
```

### Making the Command Better

Perfect! A simple, easy food command. It doesn't seem very fun though. Let's give the food to the players instead, so they have something to actually eat. See if you can use the docs to find what `effect` we need to give items to players.

Spoilers: it's `give`:

```bash
command /food:
    trigger:
        give 2 steak to player
```

Much better. Let's add some class into this command though, and tell the player that they were given some food. Bonus points for using the docs to find the effect used to send messages.

```bash
command /food:
    trigger:
        give 2 steak to player
        send "You receieved some food!" to player
```

Technically, we could just write `send "You received some food!"` and Skript would know who we meant to send it to, but it's good practice to be explicit in what you're doing when writing code.

### If/Else

Now let's get fancy and make it so operators get golden apples instead. We can check whether a player is an op with the following condition:

```bash
if player is an op:
```

Conditions are syntax elements that are essentially yes/no questions, `player is an op`, `block at player is dirt`, `name of player's tool is "hello!"`. We use these inside if statements to control what code gets run:

```bash
command /food:
    trigger:
        if player is an op:
            give 2 golden apples to player
            send "You receieved some food!" to player
        else:
            give 2 steak to player
            send "You receieved some food!" to player
```

See how the indentation shows what it supposed to run? If the player is an op, we know to run that indented code after the `if`. If they aren't, we can jump straight to the `else` and run that code instead. This is called an `if/else`, and is a basic concept across nearly every programming language. It's the most basic way of controlling what code gets run, called `control flow` in programming jargon.

### Bringing in Variables

You may have noticed that we have some duplicate code going on here. Generally, you don't want to ever write the same (or very similar) code twice. It makes your code long, harder to read, and a lot harder to maintain, because if you want to change the food message, you now have to change it in two places, or possibly more if you added other if/elses.

Let's make this better. First, we can just move the messages outside of the if/else. They're the same in both sections, so we only need one version:

```bash
command /food:
    trigger:
        if player is an op:
            give 2 golden apples to player
        else:
            give 2 steak to player
        send "You receieved some food!" to player
```

Un-indenting the send pulls it out of the `else`, meaning it will be run whether the player is op or not. The give is a bit more complicated. Honestly, this code is fine, it's very little duplication and wouldn't be that bad. But let's keep going for the sake of teaching you about variables.

Variables are extremely useful. They basically store data for you so you can keep using it again and again. They come in two main types, global and local variables. Global variables can be used anywhere in any script, while local variables can only be used in the command/event they were defined in. They're surrounded by `{}` and can be named pretty much whatever you want. Variables that start with `_`, like `{_local}`, are local variables. Everything else is global.

```bash
command /food:
    trigger:
        if player is an op:
            set {_item} to 2 golden apples
        else:
            set {_item} to 2 steak
        give {_item} to player
        send "You receieved some food!" to player
```

Here, we've set the different items that the player will get to the `{_item}` variable. We can then use that after the `if` to give the right item to the player. If they're an op, `{_item}` will be set to 2 golden apples, otherwise it'll be 2 steak.&#x20;

Variables are explained further [here](../core-concepts/variables/), in the [Core Concepts](<../README (1).md>) section.

### Using Events

We've been using a command all this time, but events are another extremely useful way to trigger code in Skript. Events are things that happen when certain, well, events happen in the game. Say the player jumps. There's an event for that. Say a furnace burns a piece of coal. There's an event for that. Say a sheep regrows its wool. There's an event for that. You can see all the events at [https://docs.skriptlang.org/events.html](https://docs.skriptlang.org/events.html). You can also learn more about them in the[ Events page](../syntax-types/events.md).

Events are kind of like commands in that they're never indented. Commands and events always start all the way to the left. Since we've been giving players food, let's use an `on consume` event:

```bash
on consume:
    # code here
```

Notice that we didn't need to use `trigger` here. This is because commands can have a lot of other attributes, like permissions, cooldowns, and aliases, while events are just events. There's no need for the trigger section to differentiate.

We can also make this event a bit more specific. Let's say we want to give the player regeneration when they eat rotten flesh, to encourage cannibalism. We can use this, instead:

```bash
on consume of rotten flesh:
    # code here
```

Now it'll only trigger when someone eats rotten flesh, rather then when they eat any item. Note that this is nearly identical to using an if statement at the start of the event:

```bash
on consume:
    if event-item is rotten flesh:
        # code here
```

Let's use the first one, it's cleaner and actually runs slightly better. Anyway, we need to give the player regeneration. You probably already know about `/effect give`, and we could use that if we wanted. But we should stick to Skript syntax as much as possible. After a quick search on the docs, we find the following:

```bash
apply swiftness 2 to the player


# for those curious about the /effect give version:
execute console command "/effect give %player% regeneration"
```

Pretty simple. Let's just change that to `regeneration 1` and add it to our code. We'll also give it a duration of 5 seconds.

```bash
on consume of rotten flesh:
    apply regeneration 1 to the player for 5 seconds
```

Perfect! Now we get regen while eating flesh. If you eat multiple in a row though, the effect starts to stack in time. This may be something you like, and you can keep it like this. If you want the timer to never go above 5 seconds, though, you need to add `replacing the existing effect`

```bash
on consume of rotten flesh:
    apply regeneration 1 to the player for 5 seconds replacing the existing effect
```

### Conclusion

Congrats on writing your first script! This was only a small part of what you can do with Skript, but I hope it helped you get your feet wet. To learn more, check out the [Core Concepts](<../README (1).md>) pages for more in-depth tutorials. You should also become familiar with the official docs, or with SkriptHub docs if you prefer those. They'll help you tremendously.&#x20;

If you had trouble following along, or didn't like this tutorial for some reason, please either message me on Discord at Sovde#0001, or open an issue on the github repository for this site.

Happy Skripting!
