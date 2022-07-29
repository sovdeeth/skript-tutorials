# Debugging

Debugging is a vital skill to learn for any programmer. Your code will break, there will be bugs, and you will not understand how to fix it without debugging. Knowing how to debug well will save you hours, as well as save the time of anyone you're asking for help. This page also includes a list of [common mishaps and points of confusion](debugging.md#undefined) at the bottom, so you know what to avoid doing.

Debugging is a catch-all term for the process of figuring out what is causing an error or a bug and fixing it, hence the name. Modern IDEs for larger programming languages have really cool and fancy debugging tools built in, but us Skripters have to deal with manual debugging. Here are the main strategies:

### Avoid Long Lines

Skript's error system is pretty helpful, most of the time. However, it can get confused when there's a lot of stuff going on in a single line. Plus, it doesn't often tell you what part of the line is the problem, just that the whole line has some sort of error. For example, the following line returns an error: `can't understand this condition/effect`.&#x20;

```tcl
    set {_item} to a golden shovel named "&bHello &cWorld!" of unbreaking 5 with lore "Remaining usages: &c%{_usages}%", "&7Click to use!", "", "To use this item, right click on a block.", "&7You currently have %{_usages}% usages", "", "", "Price: &c$%{_price}%"
```

It's probably hard to read with all the scrolling, but think of that as just another downside of long lines. Anyway, there's something wrong with it but we have no clue what. The syntax looks fine at first glace, there's no glaring errors like misspellings. Maybe it's the quotation marks. Maybe we missed a comma somewhere. Maybe golden shovel isn't the right alias. Who knows?

We can go above figuring this out in two ways. First, we could just remove parts of the line until it no longer errors, and we've found our culprit. This is fine, but it means you'll still have a long line at the end of it. A more sustainable approach is to split this over multiple lines, like so:

```tcl
    set {_item} to a golden shovel named "&bHello &cWorld!" of unbreaking 5 
    set {_lore::1} to "Remaining usages: &c%{_usages}%"
    set {_lore::2} to "&7Click to use!"
    set {_lore::3} to ""
    set {_lore::4} to "To use this item, right click on a block."
    set {_lore::5} to "&7You currently have %{_usages}% usages"
    set {_lore::6} to ""
    set {_lore::7} to ""
    set {_lore::8} to "Price: &c$%{_price}%"
    set lore of {_item} to {_lore::*}
```

Firstly, this is a lot easier to read. We can even see how the lore gets laid out on the item now. But we also know now that the error is in just the first line, because Skript now tells us that it `Can't understand this expression: 'a golden shovel named "&bHello &cWorld!" of unbreaking 5`.&#x20;

This narrows things down. Some of you may have spotted the error by now, which is good! You can fix it and stop debugging at this point. If you haven't, we can keep going.

```bash
    set {_item} to a golden shovel named "&bHello &cWorld!"
    enchant {_item} with unbreaking 5
    set {_lore::1} to "Remaining usages: &c%{_usages}%"
    set {_lore::2} to "&7Click to use!"
    set {_lore::3} to ""
    set {_lore::4} to "To use this item, right click on a block."
    set {_lore::5} to "&7You currently have %{_usages}% usages"
    set {_lore::6} to ""
    set {_lore::7} to ""
    set {_lore::8} to "Price: &c$%{_price}%"
    set lore of {_item} to {_lore::*}
```

Now we don't get any errors, so the problem must have come from the enchantments. It looked fine though, we were using unbreaking 5, didn't have spelling errors, so what was the problem? Let's check the docs. [https://docs.skriptlang.org/classes.html#itemtype](https://docs.skriptlang.org/classes.html#itemtype)

You will notice that the enchantments are supposed to come directly after the item alias, so we should have written the following:

```tcl
    set {_item} to a golden shovel of unbreaking 5 named "&bHello &cWorld!"
    set {_lore::1} to "Remaining usages: &c%{_usages}%"
    set {_lore::2} to "&7Click to use!"
    set {_lore::3} to ""
    set {_lore::4} to "To use this item, right click on a block."
    set {_lore::5} to "&7You currently have %{_usages}% usages"
    set {_lore::6} to ""
    set {_lore::7} to ""
    set {_lore::8} to "Price: &c$%{_price}%"
    set lore of {_item} to {_lore::*}
```

And voila, it works! This is the essence of debugging, narrowing down the problem until you've found the culprit, and then figuring out how to fix it. Let's move on to another common situation.

### Broadcast, Broadcast, Broadcast

Nearly every bit of code you write will have some sort of variable or if statement in it. It's really easy to assume these things just work, because the variable's the same throughout, the if statement makes sense to you, or some other reason. Oftentimes, though, the variable goes off track somewhere and isn't what you expect it to be on the other end. Or maybe code that should be running isn't running, because an if statement you thought was fool-proof isn't passing. It's these situations where broadcasting is always helpful.

```bash
# one with lots of ifs
```

```bash
# one with a variable that goes haywire
```

### Re-read and Rewrite Your Code

Talk about mentally stepping through your code

Talk about writing clear code that has limited points of error, like limiting use of options, making things clear and easy to read, labelling things with comments and good var names, etc.

Using () to cut down on parsing errors

### Common Points of Confusion

* Item comparison, items amount/name/enchants, etc
* Local vs Global
* Apostrophes in function inputs

### Other Useful Things

* effect commands
* setting up your own debug syntax/functions
