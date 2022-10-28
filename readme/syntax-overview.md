# Syntax Overview

Syntax is a word that means the set or rules or conventions that define a language. In our case with Skript, syntaxes are various things you can write to make Skript do different things. For example, `player` is a syntax that means either the player entity type, or the player involved in the event, depending on when and where it is used.

Skript has a few main types of syntaxes:

* Top-level syntaxes like events, commands, function definitions, and the option and variables sections.
* Other syntaxes, like effects, expressions, types, function calls, variables, and anything else you'd use in normal code.

### Top-Level Syntaxes

Top level syntaxes are things you would write to start out a script:

```bash
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

### Other Syntaxes

Top-level syntaxes are pretty straightforward, just write an event, or a command, or whatever. Everything else is a little more complicated, but we'll break it down to the simple bits. The basic structure of a line of Skript code consists of two main elements: Effects and Expressions.

#### Effects

Effects are as their name suggests. They **effect change** on things, like a verb. In fact, nearly every effect is the verb of the "sentence" it creates. If you look at the Skript documentation you'll see the Effect tab and all the possible syntaxes you can use. Things like `teleport %entities% to %location%`, or `send %objects% to %players/console%`.&#x20;

Note that you can only have **one** effect on each line. There is no way to have any line with more than one effect.

Think of most Effects like a jigsaw puzzle with a few pieces missing (some already have all the pieces they need, but that's rare). The docs tell you what pieces are already there (`send`, `to`) and what the shape of the missing pieces looks like (`objects`, `players/console`). These are **types**, and they tell you what you can and can't put in the blanks. Anything can go into an `object` slot, but text can't go into a `player` slot. If the type is plural, it means you can put multiple things into that slot, like a list.

#### Expressions

We just went over the basics of Effects and we found that we need to fill in some blanks. What do we put in place of `objects`, in place of `players/console`? This is what Expressions do.&#x20;

Expressions are syntaxes that **represent something**, filling the role of a noun in the "sentence". Again, if you look in the Expressions tab on the Skript documentation you will be able to see all the available expressions.

Expressions can be further broken down into some subcategories, which we'll talk about in more detail in the Syntax Types page. For now, just know that there are simple expressions and combined expressions. Simple expressions rely on no other expressions, they're completed puzzles. Combined expressions are more like effects in that they require some more puzzle pieces to be completed. Here are some examples:

```bash
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

We should also briefly touch on Literals and Aliases, which are fancy names for pretty simple concepts. Literals are just the literal value of something, written out. For example, `"Hello"` is a text literal: It's text, and it's written out literally. It's not created through some expression, like `join "he" and "llo"`. Likewise, any number you write out is a literal, and same with writing out entity types like `creeper` or `skeleton`.&#x20;

Aliases are similar, but mainly for items. Aliases make it easier to write out item names, because Minecraft items aren't always named well behind the scenes. Some examples of aliases include: `diamond sword`, `oak leaves`, and (surprisingly) `netherite chestplate of unbreaking 3`. That last one is a little out of place, but it's good to remember that when you're making enchanted objects like that, the enchantments have to come immediately after the name of the item.

### Examples

Anyway, let's get back on track with some examples. I'm going to write some basic lines of Skript, and then identify the effects and expressions in each one.

```bash
# Example 1: giving an item to a player in a command
command /give-food <player>:
    trigger:
        give 5 steak to arg-1
```

This is a pretty short line, so there isn't too much to dissect. Firstly, we have our top-level syntax, which is a command. We won't be going in to the details of those here. Inside that command, we have this line of code: `give 5 steak to arg-1`. This consists of 1 effect, an alias, and a simple expression.&#x20;

The effect, as always, begins our line. Here we're using the Change effect, a very common effect. Specifically, we're using the `give` syntax of the effect:

```bash
(add|give) %objects% to %~objects%
```

The `~` here just means that that expression cannot be a literal, but we'll talk more about that in the Syntax Types page. Anyway, we can see that this means our first `objects` is going to be the `5 steak` and our second one is going to be `arg-1`.&#x20;

`arg-1` is the Argument expression, which is a simple expression that refers to the argument of our command.&#x20;

`5 steak` is an alias for an item, rather self explanatory. 5 steaks.

Here's a slightly more complex example:

```bash
# Example 2: sending the player's name and their tool's type when they right click.
on right click:
    send "%player's name% and %type of player's tool%" to all players
```

Again, we have a top-level element, this time a right-click event. Inside this event, we have a `send` effect.&#x20;

```bash
(message|send [message[s]]) %objects% [to %players/console%] [from %player%]
```

You can see we're not using the very last bit, `from %player%`, as it's optional and we don't need it here. It's also evident that the first `objects` refers to the text, and the second one refers to `all players`.

`all players` is simply all the players that are online on the server. This is a simple expression.

Our text, however, is a little more complicated. We're using `%` symbols, which is Skript's way of inserting the values of expressions into text. The first one is `player's name`, which is a combined expression:

```bash
%offline players/entities/blocks/item types/inventories/slots/gamerules%'[s] (name[s]|(display|nick|chat|custom)[ ]name[s])
# our specific use:
%offline players/entities%'[s] name[s]
```

So we have the simple expression `player`, or `event-player`, put into the combined expression to make `player's name`.

Likewise for the player's tool. It's the simple expression `player` put into a combined expression to form `player's tool`. This expression though, is taken one step further and we use it in another combined expression to make `type of player's tool`.&#x20;

Then we can take all of these values, put them into a text, and send it off to all the players on the server.

### Conclusion

I want to stress that you do not need to do this kind of analysis when you're writing code. It's good to know how effects and expressions work together and how you can combine them to make bigger things happen. However, most of your code writing should just be taking things from the docs and putting them together to make it work. You don't need to stress about what's a literal, what's an alias, etc. Just know how to fit expressions into effects and you're pretty much golden.

If you are curious about the more advanced technicalities in how Skript works, you can read the pages in Syntax Types.
