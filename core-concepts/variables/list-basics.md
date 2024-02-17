# List Basics

Lists are extra powerful variables. Instead of just a single name referring to a single value, lists can refer to multiple values, they can be looped, added to, cleared, and treated as if they're much more than just a single value. These properties make lists extremely useful for store data that changes per player, for data that requires a bunch of know locations, like warps, homes, or generator locations. Plus, when you're done with the data you can just delete it all at once!

## Values, Indices, and `::*`

Let's start by introducing some definitions, so we're all on the same page. Lists, in Skript, are variables that use the `::` separator. In most cases, it'll look like `{_my-list::*}`. This `::*` means "get all the values of the list `_my-list`". Values are the things you store in your list. These could be numbers, locations, text, or any combination. Each value in a list also has a corresponding name, an index. You can use this index to access that value specifically. By default, the indices of a list will be from 1 to however long the list is. Here's an example:

```applescript
set {list::*} to "hey", "how", "are", "you" and 55

# the following is not code, it's just a representation of the list
list::1 -> "hey"
list::2 -> "how"
list::3 -> "are"
list::4 -> "you"
list::5 -> 55
```

Using these indices, we can get a value separately from all the other values, by naming it specifically:

```applescript
send {list::3} to player
# Player receives "are"
```

We can also set the index of a value to whatever we want. This is especially useful for storing information that's attributed to something else, like a player's last death location:

```applescript
on death of player:
    set {last-death::%uuid of victim%} to event-location
    
# send the player their specific death location
command /last-death:
    executable by: players
    trigger:
        send {last-death::%uuid of sender%} to sender
```

Using lists like this makes it simple to see all the last death locations at once (`{last-death::*}`), and as you'll see in a second, makes it really easy for us to reset everything if we decide this information is no longer useful.

Lists are also very useful for sending information to a certain group of people:

```applescript
send "Hello" to {admins::*}
```

Or sending a bunch of things all at once:

```applescript
set {_info::*} to player's name, player's level, player's location
add player's tool to {_info::*}
send {_info::*} to player
```

Of course, these same properties extend to other effects too. You can equip a player with a list of armors, give them a list of items, kill a list of entities, and much more.

## Looping, Adding, Clearing, and Removing

This is where the major power of lists comes into play. Lists can be easily modified by adding and removing values, and they can be looped to provide easy access to all the values.

We have the normal changers:

```applescript
# adding adds an element, or value, to the list
# its index will be the first available number (1, 2, 3, etc)
add "hello" to {_list::*}

# removing removes the first matching value
remove "hey" from {_list::*}

# delete or clear can delete all the values of a list, or just one.
delete {_list::*}
clear {_list::*}
delete {_list::1}
clear {_list::1}

# and of course we can set
set {_list::*} to 1, 2, 3, 4 and 5
set {_list::3} to 10
```

In a similar vein, there's also a condition called `contains` which will allow you to check if a value is contained somewhere in a list:

```applescript
if {_list::*} contains "hello":
```

We can also loop lists, which opens up even more possibilities:

```applescript
set {_list::*} to "hey", "how", "are", "you" and 55
loop {_list::*}:
    send "%loop-index% - %loop-value%" to player
```

In this code, we loop through a list, sending the index and the value each time. Our output will look something like this:

```applescript
1 - hey
2 - how
3 - are
4 - you
5 - 55
```

This can be really useful for things like leaderboards, for knowing what elements exist in your list, and much more. If you want to see some more advanced uses of lists, ways to best optimize them, as well as some of the quirks and trickier aspects of using them, check out the [Lists page](../lists/).
