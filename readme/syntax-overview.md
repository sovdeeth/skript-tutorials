# Syntax Overview

Syntax is a word that means the set or rules or conventions that define a language. In our case with Skript, syntaxes are various things you can write to make Skript do different things. For example, `player` is a syntax that means either the player entity type, or the player involved in the event, depending on when and where it is used.

Skript has a few main types of syntaxes:

* Top-level syntaxes like events, commands, function definitions, and the option and variables sections.
* Sections and conditions, syntaxes that require some indentation.
* Effects, expressions, literals and similar syntaxes that make up the fundamental bits of a line of Skript code.

I will warn you, this is a very long page. Feel free to use the right-side bar to skip the the sections you are most interested in.

## Crash Course in Reading Syntax

You may have looked at the Skript docs and been very confused on how to turn that long string of words and symbols into actual code. The docs can be a bit daunting if you don't know the rules for how to read them. Here's a little crash course, but if you want more detail, head over to [Reading Syntax](../syntax-types/reading-syntax.md).

```applescript
(message|send [message[s]]) %objects% [to %players/console%] [from %player%]
```

The Send effect, seen above, is a pretty ubiquitous syntax in Skript, so we'll use it as our example. The first thing to recognize are the square brackets `[]`. These denote optional syntax, things you don't have to include if you don't want to. We can omit the `[to %players/console%] [from %player%]` section entirely and still have it work.

The second big thing are the parts where you can choose one or another. These look like `(choice 1|choice 2)`. You can use choice 1 or choice 2, but not both. In the Send effect, this means we can either use `message` or `send [message[s]]`, but not both together.

Finally, note the use of `%`. Anything enclosed in percent signs denotes some blank spot left for us to fill in. Here, `%objects%` means we can put whatever kind of information we want there. `%player%` means we need to put a player there, and `%players/console%` means we can put multiple players and/or the console in that spot. Let's see a few ways we could write the syntax out:

```applescript
message "test" to player
send message "Hey there!" to player and console
send {_a} and {_b} and "c" to all players
message {_something} to player from {_another player} 
```

Hopefully this helps you see how the syntax in the docs can be turned into actual Skript code.

## Top-Level Syntaxes

Top level syntaxes are things you would write to start out a script:

```applescript
# an event
on place of cobblestone:
    # run code

# a command
command /broadcast <text>:
    # Fields and the trigger section
    
# a function
function name(p: player) :: text:
    # run code
```

You don't need to know exactly how to use all of these, they'll be explained in greater detail later. **The important part is that none of them are indented.** All top-level syntaxes are just that, top-level. They're never indented. They contain every other bit of code inside of them. If you ever get the error `"All code must be put inside an event"`, you've encountered this principle.

## Fundamental Syntaxes

Top-level syntaxes are pretty straightforward, just write an event, or a command, or whatever. Everything else is a little more complicated, but we'll break it down to the simple bits. The basic structure of a line of Skript code consists of two main elements: Effects and Expressions.

### Effects

Effects are as their name suggests. They **effect change** on things, like a verb. In fact, nearly every effect is the verb of the "sentence" it creates. If you look at the Skript documentation you'll see the Effect tab and all the possible syntaxes you can use. Things like `teleport %entities% to %location%`, or `send %objects% to %players/console%`.&#x20;

Note that you can only have **one** effect on each line. There is no way to have any line with more than one effect.

Think of most Effects like a jigsaw puzzle with a few pieces missing (some already have all the pieces they need, but that's rare). The docs tell you what pieces are already there (`send`, `to`) and what the shape of the missing pieces looks like (`objects`, `players/console`). These are **types**, and they tell you what you can and can't put in the blanks. Anything can go into an `object` slot, but text can't go into a `player` slot. If the type is plural, it means you can put multiple things into that slot, like a list.

### Expressions

We just went over the basics of Effects and we found that we need to fill in some blanks. What do we put in place of `objects`, in place of `players/console`? This is what Expressions do.&#x20;

Expressions are syntaxes that **represent something**, filling the role of a noun in the "sentence". Again, if you look in the Expressions tab on the Skript documentation you will be able to see all the available expressions.

Expressions can be further broken down into some subcategories, which we'll talk about in more detail in the Syntax Types page. For now, just know that there are simple expressions and combined expressions. Simple expressions rely on no other expressions, they're completed puzzles. Combined expressions are more like effects in that they require some more puzzle pieces to be completed. Here are some examples:

```applescript
# simple expressions
the attacker
the console
event-player # or just 'player'
{_any-variable} # variables are *technically* different but who cares.

# combined expressions
the name of %item%
%player%'s level
the block at %location%
```

We should also briefly touch on Literals and Aliases, which are fancy names for pretty simple concepts. Literals are just the literal value of something, written out. For example, `"Hello"` is a text literal: It's text, and it's written out literally. It's not created through some expression, like `join "he" and "llo"` or `"the answer is %10 + 5%"`. Likewise, any number you write out is a literal, and same with writing out entity types like `creeper` or `skeleton`.&#x20;

Aliases are similar, but mainly for items. Aliases make it easier to write out item names, because Minecraft items aren't always named well behind the scenes. Some examples of aliases include: `diamond sword`, `oak leaves`, and (surprisingly) `netherite chestplate of unbreaking 3`. That last one is a little out of place, but it's good to remember that when you're making enchanted objects like that, the enchantments have to come immediately after the name of the item.

### Examples

Anyway, let's get back on track with some examples. I'm going to write some basic lines of Skript, and then identify the effects and expressions in each one.

```applescript
# Example 1: giving an item to a player in a command
command /give-food <player>:
    trigger:
        give 5 steak to arg-1
```

This is a pretty short line, so there isn't too much to dissect. Firstly, we have our top-level syntax, which is a command. We won't be going in to the details of those here. Inside that command, we have this line of code: `give 5 steak to arg-1`. This consists of 1 effect, an alias, and a simple expression.&#x20;

The effect, as always, begins our line. Here we're using the Change effect, a very common effect. Specifically, we're using the `give` syntax of the effect:

```applescript
(add|give) %objects% to %~objects%
```

The `~` here just means that that expression cannot be a literal, but we'll talk more about that in the Syntax Types page. Anyway, we can see that this means our first `objects` is going to be the `5 steak` and our second one is going to be `arg-1`.&#x20;

`arg-1` is the Argument expression, which is a simple expression that refers to the argument of our command.&#x20;

`5 steak` is an alias for an item, rather self explanatory. 5 steaks.

Here's a slightly more complex example:

```applescript
# Example 2: sending the player's name and their tool's type when they right click.
on right click:
    send "%player's name% and %type of player's tool%" to all players
```

Again, we have a top-level element, this time a right-click event. Inside this event, we have a `send` effect.&#x20;

```applescript
(message|send [message[s]]) %objects% [to %players/console%] [from %player%]
```

You can see we're not using the very last bit, `from %player%`, as it's optional and we don't need it here. It's also evident that the first `objects` refers to the text, and the second one refers to `all players`.

`all players` is simply all the players that are online on the server. This is a simple expression.

Our text, however, is a little more complicated. We're using `%` symbols, which is Skript's way of inserting the values of expressions into text. The first one is `player's name`, which is a combined expression:

```applescript
%offline players/entities/blocks/item types/inventories/slots/gamerules%'[s] (name[s]|(display|nick|chat|custom)[ ]name[s])
# our specific use:
%offline players/entities%'[s] name[s]
```

So we have the simple expression `player`, or `event-player`, put into the combined expression to make `player's name`.

Likewise for the player's tool. It's the simple expression `player` put into a combined expression to form `player's tool`. This expression though, is taken one step further and we use it in another combined expression to make `type of player's tool`.&#x20;

Then we can take all of these values, put them into a text, and send it off to all the players on the server.

## Conditions and Sections

### Conditions

Conditions are what you think of as **if statements**. Conditions can compare two things or they can just look at a property of something, like `if player is alive:`. We call this second kind of condition a _property condition_, and they tend to be simpler to use than other conditions.

Conditions don't technically have to be part of an if statement, but it's rare that you see them outside of that usage. Conditions can be written on their own, where they act as gatekeepers for your code. If they don't pass (i.e. they are false), then the code after won't run. This is generally called an _inline condition_. I'll put some examples below:

```applescript
# a normal if statement using a condition:
if level of player >= 5:
    broadcast "yay!"

# an inline if statement
level of player >= 5
broadcast "yay!"

# an exotic use: "do if" statement
broadcast "yay!" if level of player >= 5

# another slightly exotic use: a ternary operator
broadcast "yay!" if level of player >= 5 else "aww."
```

If you want to learn more about these various uses, check out [Conditionals](../core-concepts/indentation/conditionals.md).

### Sections

Notice that in the first example above, we had to use a colon (`:`) and indent the next line of code. When this happens, we call it a _section_. Sections are smaller, well, sections of your whole program. In the code below, notice how we have two levels of indentation, and thus two sections.

```applescript
if player's health >= 5:
    spawn a zombie at player:
        set the zombie's helmet to an iron helmet
        set the zombie's display name to "BOO!"
    send "BOO!" to the player
```

Our first section is the if statement. If the player's health is less than 5, we just skip the entire section of code and move on. However, if it's greater or equal to 5, we'll go into that section.

We then have the spawn section. This section is pretty neat, because it allows you to modify the entity that you're spawning before it officially spawns. It has the special property of being able to refer to the spawning entity with the word `entity` or in this case, `zombie`. No need for `last spawned entity`.&#x20;

After we finish running the spawn section, we'll pop back out to the if statement's section and finish running the `send` effect. That's the last bit of code, so we stop there.

## Conclusion

I hope this has giving you a bit more grounding in the terms and meaning behind how Skript's syntaxes work and how they're named and categorized.&#x20;

I want to stress that you do not need to do this kind of analysis when you're writing code. It's good to know how different syntactic elements work together and how you can combine them to make bigger things happen. However, most of your code writing should just be taking things from the docs and putting them together to make it work. You don't need to stress about what's a literal, what's an alias, etc. Just know that effects are different from expressions and that conditions go in if statements, and you're pretty much golden.

If you are curious about the more advanced technicalities in how Skript works, you can read the pages in Syntax Types.
