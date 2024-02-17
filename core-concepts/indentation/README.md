# Indentation and Program Flow

Like any programming language, Skript needs some way of recognizing when code is meant to be 'within' other code, like what code a command should run, or what code should be run if an if statement succeeds. Many languages solve this with brackets, like `{}` or `()`. Skript, like Python, decides to solve this with indentation to keep the code you write cleaner and less cluttered.

Indentation is a catch-all term for how far from the left margin your text starts. In Skript, we achieve this with either spaces or tabs, like so:

```
I'm not indented.
    I've been indented by 1 tab.
  Only used two spaces were used for me.
          A whole ten spaces were used to indent me! 
```

## Basic rules of indentation

Skript requires that for each structure you create (ie: an event, a command, a function), you stick to a consistent pattern of indentation. This means every line in that structure has to follow the same rules. If the first line uses 2 spaces for a single level of indentation, the next line can't use a tab, or 3 spaces. Apart from that, though, you can choose whatever amount of spaces OR tabs (not both!) that you want.

```
# valid indentation:
Not indented
    Indented by one tab
        Indented by two tabs!
        
# valid indentation:
Not indented
   Indented by three spaces
      Indented by six spaces

# invalid indentation
Not indented
    Indented by one tab
      Indented by a tab and two spaces (BAD)
```

The standard practice is to use 2 spaces, 4 spaces, or 1 tab as your indentation. Editors like VSCode have tools to automatically indent or convert indentation for you if you have issues.

### When to indent?

Indentation is required whenever you want code to fall under the control of something else. Let's say you have an event, `on chat`, that you want to run code inside of. You write the event itself with no indentation, but any code inside of it has to follow with a layer of indentation:

```applescript
on chat:
    # indent your code by one layer
    set message to ">> %message% <<"
```

For commands it's much the same, but we have the first level taken up by the entries like aliases, cooldown, description, and the trigger. So we have to indent a second time below the trigger to tell Skript that we want the code to be attached to the trigger, specifically.

```applescript
command /test:
    # the entries are indented once
    aliases: t
    description: a test command
    trigger:
        # but then our code is indented a second time to put it under the trigger.
        broadcast "test"
```

This follows suit for things like loops, if statements, spawn sections, anything that can have code "inside" it:

```applescript
on join:
    # indent to put code in the event
    set {_uuid} to player's uuid
    if {banned::%{_uuid}%} is set:
        # indent a second time to put code in the if statement
        kick player
    # go back to one level of indentation to show that this code isn't in the if statement
    loop all players:
        # indent again to enter the loop
        if {quiet-mode::%loop-player's uuid%} is set:
            # indent again to enter the if
            continue
        # go back one to exit the if statement
        send "%player% has joined." to loop-player
    # go back one to exit the loop
    set {last-seen::%player's uuid%} to now
```

If you're ever uncertain, **a good rule of thumb is to indent one more time whenever you end a line with a colon** (`:`).
