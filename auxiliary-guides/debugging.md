# Debugging

Debugging is a vital skill to learn for any programmer. Your code will break, there will be bugs, and you will not understand how to fix it without debugging. Knowing how to debug well will save you hours, as well as save the time of anyone you're asking for help. You should also visit the [SkUnity wiki page on debugging](https://forums.skunity.com/wiki/debug/), it has a lot of useful examples, tips and tricks, and other resources.

Debugging is a catch-all term for the process of figuring out what is causing an error or a bug and fixing it, hence the name. Modern IDEs for larger programming languages have really cool and fancy debugging tools built in, but us Skripters have to deal with manual debugging. Here are the main strategies:

## Avoid Long Lines

Skript's error system is pretty helpful, most of the time. However, it can get confused when there's a lot of stuff going on in a single line. Plus, it doesn't often tell you what part of the line is the problem, just that the whole line has some sort of error. For example, the following line returns an error: `can't understand this condition/effect`.&#x20;

```tcl
set {_item} to a golden shovel named "&bHello &cWorld!" of unbreaking 5 with lore "Remaining usages: &c%{_usages}%", "&7Click to use!", "", "To use this item, right click on a block.", "&7You currently have %{_usages}% usages", "", "", "Price: &c$%{_price}%"
```

It's probably hard to read with all the scrolling, but think of that as just another downside of long lines. There's something wrong with it, but we have no clue what. The syntax looks fine at first glace, there's no glaring errors like misspellings. Maybe it's the quotation marks. Maybe we missed a comma somewhere. Maybe golden shovel isn't the right alias. Who knows?

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

## Broadcast, Broadcast, Broadcast

Nearly every bit of code you write will have some sort of variable or if statement in it. It's really easy to assume these things just work, because the variable's the same throughout, the if statement makes sense to you, or some other reason. Oftentimes, though, the variable goes off track somewhere and isn't what you expect it to be on the other end. Or maybe code that should be running isn't running, because an if statement you thought was fool-proof isn't passing. It's these situations where broadcasting is always helpful.

Here we have a command that has a bunch of different possible uses. We'll be specifically looking at `/kit give Ultra Dinnerbone`, or giving the kit `Ultra` to `Dinnerbone`.

{% code lineNumbers="true" %}
```bash
# A command to manage kits and give them to players
command kit <text> [<text>] [<player>]:
    trigger:
        if arg-1 is "give":
            if arg-2 is "Ultra":
                if {kits-unlocked::%player%::*} contains "Ultra": 
                    if arg-3 is set:
                        give {kit::Ultra::*} to arg-3
```
{% endcode %}

The problem is, whenever we run the command, nothing happens! Obviously, something's broken, the question is what?

Our first strategy should be to find out where our code stops working. We can do this by putting broadcasts after every if statement to see which ones work:

{% code lineNumbers="true" %}
```bash
# A command to manage kits and give them to players
command kit <text> [<text>] [<player>]:
    trigger:
        if arg-1 is "give":
            broadcast arg-1
            if arg-2 is "Ultra":
                broadcast arg-2
                if {kits-unlocked::%player%::*} contains "Ultra": 
                    broadcast "unlocked kits: %{kits-unlocked::%player%::*}%" 
                    if arg-3 is set:
                        broadcast arg-3
                        give {kit::Ultra::*} to arg-3
```
{% endcode %}

Note that we didn't just broadcast the same thing every time, we also broadcasted something important, the arguments or the unlocked kits. Now it'll tell us where it stops working, and it'll also help us know exactly what the values of our arguments are. When we run the command, we get this output:

<pre class="language-bash"><code class="lang-bash"><strong>[21:15:15 INFO]: /kit give Ultra Sahvde
</strong>[21:15:15 INFO]: give
[21:15:15 INFO]: Ultra</code></pre>

Ok, now we know that we never reach line 9, because `unlocked kits:` was never broadcast. This means we have a problem with Line 8.&#x20;

```bash
if {kits-unlocked::%player%::*} contains "Ultra": 
```

Notice anything? We mistakenly used `player` instead of `arg-3`, so it was checking what kits we've unlocked, not Dinnerbone. When we switch out `player` for `arg-3`, the command works and the items are given to Dinnerbone.

Now this was a bit of a contrived example, but I hope it points out the utlitity of knowing where the error is, and narrowing down the possibilities until you find it. This same technique would work if we had forgotten to unlock the kit for Dinnerbone. We'd just go one more step and broadcast their unlocked kits, see that `Ultra` is nowhere to be found, and then realise we hadn't unlocked it for them yet.

## Reread and Rewrite Your Code

Skript unfortunately lacks any sort of built-in debugging tools, as it isn't exactly super widely known. This means we don't have easy access to tool that let us step through our code line by line. Instead, we have to rely on our own perception, reading our own code, putting in broadcasts and testing things line by line. It's always a good idea, when you're confused, to sit down and run through your code line-by-line, making sure you can prove to yourself that it works.

Another aspect of debugging is making sure bugs are less likely to happen in the future. This is a much more nebulous topic and what works for me may not work for you. However, here are a few things you can to to make your code easier to debug, easier to read, and easier to maintain:

* **Avoid extensive use of options.** Options can be powerful tools, but they can also cause a lot of issues. Options are directly replaced with their values before the code is ever parsed. It's like Skript does a ctrl-f find and replace on your script. This can cause unseen errors to pop up, since when you're looking at your code, it makes perfect sense. But once the options are replaced, that code literally changes, and maybe there's now a comma out of place, or one too many quotation marks, or some other hard-to-find error. Options can be powerful, but they make your code hard to read and hard to debug.
* **Use lots of short lines.** This has been mentioned before, but always write more, shorter lines versus fewer, longer lines. It may seem counter-intuitive, but it make your parsing time shorter and makes your script run faster. It also allows the parser to find your error more easily and tell you exactly what's wrong.
* **Use descriptive variable names (and add comments).** The easier your code is to understand, the easier a time you will have in 4 weeks when some small bug pops up and you have to go back and fix something. You'll have forgotten a lot of what you wrote, and having clear names and comments to fall back on helps you get up to speed a lot faster.
* **Use parentheses to help the parser.** Often, the Skript parser isn't exactly sure what you mean. If you surround your expressions in `()`, it tells the parser that everything inside there is one expression and it doesn't need to worry about where it starts or stops, it just trusts you. This lets you be a lot more specific about what your code does, cuts down on parsing times, and avoids weird parsing errors that make no sense to you.

## More Information and Tips

This is not an exhaustive guide to debugging, but it should get your well on your way. If you want to know more,  including extra tips about how to help you debug, check out the [SkUnity wiki page on debugging here](https://forums.skunity.com/wiki/debug/)!
