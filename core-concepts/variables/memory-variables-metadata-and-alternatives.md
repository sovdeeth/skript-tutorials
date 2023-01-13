# Memory Variables, Metadata, and Alternatives

While our standard global and local variables are versatile tools and are perfectly capable on their own, there are situations where we may want to consider different ways of storing information. We have plenty of options, especially if we include addons, but I want to focus on a few main ones. Namely, Memory (or Ram) Variables and Metadata.

## Memory Variables

Memory variables are very easy to wrap your head around. Simply, they're just global variables with one difference. They don't get saved when the server stops. This means, of course, you don't get to save data over long-term, but you still get the benefits of the global scope. Plus, since they're not saved, they're also a lot gentler on the server.

These are extremely useful for when you need to transfer information between triggers or over time, but you don't really need to keep it around for the long term. Whenever you're using global variables, ask yourself if you really need to save this information over restarts. If you don't, try using a memory variable.

Now, I haven't explained how to actually write a memory variable yet, and this is because it's not a default part of Skript. You have to specifically enable them in your `config.sk` file, and they can take whatever form you give them. The general convention is to make all variables that start with `-` memory variables, eg: `{-variable}, {-list::*}`. You can do this by opening your `config.sk` file and scrolling down to line 310 or so, where you should see the following:

```yaml
default:
	# The default "database" is a simple text file, with each variable on a separate line and the variable's name, type, and value separated by commas.
	# This is the last database in this list to catch all variables that have not been saved anywhere else.
	# You can modify this database freely, but make sure to know what you're doing if you don't want to loose any variables.
	
	type: CSV
	
	pattern: .*
	
	file: ./plugins/Skript/variables.csv
	
	backup interval: 2 hours
	
	# PS: If you don't want some variables to be saved in any database (e.g. variables that contain an %entity% which usually despawn when the server is shut down)
	# you can modify the last database's pattern to not match all variables, e.g. use '(?!x_).*' to match all variables that don't start with 'x_'.
	# Be very cautious when doing this however as unsaved variables cannot be recovered after the server has been stopped.
	# I recommend to use a single character to denote unsaved variables (similar to local variables' '_'), e.g. '-', in which case the last database's pattern should be '(?!-).*'.
```

The last four lines of comments describe how to enable memory variables. In simple terms, you just replace the `pattern:` line with a new regex pattern, one that excludes variables that start with `-`. As you can see in the comments, this pattern is  `(?!-).*`.&#x20;

However, know that you can set this pattern to whatever you want, which also means your memory variables can follow whatever format you want, as long as you set the pattern up properly. I advise sticking with convention and using `-`.

## Metadata

Metadata serves a similar role to memory variables, in that it's global and it doesn't stick around over restarts. It has one major difference, though, and it's that metadata is tied to a specific "metadata holder". These holders can be a few different things, but importantly players, entities, and blocks are all metadata holders.

This makes metadata ideal for temporary information tied to some entity or block, as you don't have to worry about giving it a unique name for each player or holder. You can see the syntax [here](https://docs.skriptlang.org/expressions.html#ExprMetadata).

For example, let's use metadata to make an arrow explode when it hits something:

```applescript
on projectile hit:
    if projectile is an arrow:
        create a fake explosion at projectile    
```

This makes explosions at every arrow, so let's make it only happen to arrows fired from a specifically named bow.

```applescript
on shoot:
    if projectile is an arrow:
        if name of shooter's tool is "Explosive Bow":
            set metadata tag "explosive" of projectile to true

on projectile hit:
    if projectile is an arrow:
        if metadata tag "explosive" of projectile is true:
            create a fake explosion at projectile  
```

Now we're transferring information from the `shoot` event directly to the `projectile hit` event without using a global variable, and without running into any issues with multiple arrows being fired, and even without worrying about filling up our `variables.csv` file with useless info!

If we, instead, chose to check the shooter's tool when the projectile lands, we'd run into the issue of the shooter maybe changing tools while the arrow's in flight. With this solution, all the relevant info is stored in the arrow itself and nothing relies on the shooter, which makes our solution much less likely to run into bugs and edge cases.

## Alternatives

There are many other ways to deal with information and data in Skript. Addons allow you to write and read from files, or store data in YML or JSON formats, or even communicate with SQL and MongoDB databases. You could even choose to store data in the NBT of entities, or in an NBT file. All of these approaches have their times and places, their pros and cons. Ultimately, it's up to you to do research as to what's best for you.

That said, here are some general rules:

* Try to store the least amount of data possible for the long term. No matter your storage solution, it'll always be faster and easier if you store less information.
  * In this vein, try to make use of temporary structures like local variables, metadata, or memory variables.&#x20;
* When you're using a relatively slow method of storing data (databases, reading/writing files, etc), try to load all the relevant information **only when you need it**. When a player joins, for example, try to copy all their information from your long-term solution into a temporary, fast format like memory variables. Then, when they leave, write the updated information back to the slower, long-term storage.
* Prioritize usability and readability over performance. If a high-performance solution makes your code a nightmare to read and maintain, consider not making that sacrifice. Losing a few milliseconds per tick is not worth it if it saves you hours of extra work in the future when you need to modify your code.
