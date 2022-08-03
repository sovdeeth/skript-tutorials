# Global and Local

Variables, as mentioned previously, have two main "scopes" they can have. They can be global, where any script, any file, any event or function or command can access them. Or, they can be local, where only the event/function/command that created them can access them.&#x20;

```bash
on load:
    set {a-global-variable} to "hello, world"
    set {_a-local-variable} to "Hey!"

    # this broadcasts "hello, world" and "Hey!"    
    broadcast {a-global-variable}
    broadcast {_a-local-variable}
    
command /test:
    trigger:
        # this broadcasts "hello, world", but not "Hey!"   
        broadcast {a-global-variable}
        broadcast {_a-local-variable}
    
```

As seen above, the global variable is the same no matter where it's accessed. The local variable, though, only is set to `"Hey!"` in the event it was defined in, the `on load`. In the command, it's not set to anything yet. This might seem annoying, but it's actually one of the strengths of local variables.&#x20;

Say you have an event that includes some sort of wait. Let's say, a command that draws particles for 5 seconds. If you were to use a global variable to store the location, you'd need to somehow make the name of the variable unique for every single time the command is called, otherwise the particles move somewhere else if the command is called again within the 5 seconds.&#x20;

```bash
# bad code:
command /particles:
    trigger:
        set {location} to player's location
        loop 100 times:
            show smoke at {location}
            wait 1 tick
```

However, if we use `{_location}`, a local variable, we don't have to worry about this. Each time the command is called, the location variable is only usable inside that instance of the command. This means each time the command is called, it has its own unique location stored without needing to have a unique variable name.

```bash
# good code:
command /particles:
    trigger:
        set {_location} to player's location
        loop 100 times:
            show smoke at {_location}
            wait 1 tick
```

### When to Use Global vs Local

