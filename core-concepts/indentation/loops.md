# Loops

Loops are fantastic tools for anyone to have in their skill set. Loops allow you to repeat code, create countdowns, draw things, look for certain entities or items, and much more. They, along with conditionals, are your bread and butter for controlling how your program flows.

In essence, loops repeat code. They have a block of code inside them and they run it, and then run it again, and again, until they're done with their job. The details about how many times they run it (and what changes between each iteration) depend on the type of the loop.

## Types of Loops

Skript, like most languages, has two main types of loops: _for_ loops and _while_ loops. Each has their own use case.

### For Loops

For loops are often just called "loops", but I'll be calling them for loops to ensure no one gets them confused with while loops. They look like this:

```
loop something:
    # do something
```

The _something_ will be some sort of list. It could be `integers from 0 to 4`, or `all players`, or even `blocks in radius 10 of player`. There's even a special expression intended to be used in loops: `x times`. This will loop the numbers from 1 to x, or in the following example, 1 to 3.

```
loop 3 times:
    broadcast "loop!"
```

Loops will run the code inside them once per item in their list. So for `3 times`, which has `1, 2, 3`, it'll run the code 3 times.&#x20;

However, it'd be really nice if we could, say, access what number it's currently on. This is where `loop-value` comes in. When you're looping something, you can get the current thing (number, player, whatever) by using `loop-value`.&#x20;

```
loop all players:
    send "hey!" to loop-value
```

{% hint style="info" %}
If you're looping a specific expression, like `all players`, you can use `loop-type` as another version:

```
loop all players:
    send "hey!" to loop-player
```
{% endhint %}

#### Looping Over Lists

Loops go hand in hand with lists, as every loop needs a list of something to loop over. The `%number% times` expression actually creates a list of numbers, so `set {_x::*} to 3 times` is equivalent to `set {_x::*} to 1, 2, and 3`. Use this knowledge wisely.&#x20;

This means that variable lists can also be looped, and they come with some special features.

```applescript
set {_my-list::1} to "hey"
set {_my-list::2} to "how"
set {_my-list::are} to "you"

loop {_my-list::*}:
    broadcast "%loop-index%: %loop-value%"
    # this prints the following:
    # 1: hey
    # 2: how
    # are: you
```

Really, just one special feature, which is `loop-index`. This lets you access the index of the current element of the list, just like how `loop-value` gives you the value.

### While Loops

While loops are the for loop's more temperamental cousin. They keep looping as long as a condition is true, which makes them great for repeating things over time, doing a specific action a dynamic number of times, or crashing your server.

{% hint style="danger" %}
Since while loops will not stop until the condition fails, they can run infinitely, potentially crashing your server. Make sure your while loop **will always exit** **or add a wait to it.** Adding a wait stops the while loop until the delay is done, allowing your server to actually run.

```applescript
# crashes your server
while true is true:
    send "hi"

# doesn't crash
while {_i} < 100:
    add 1 to {_i}

# doesn't crash
while true is true:
    send "hi"
    wait 1 tick
```
{% endhint %}

While loops are most often used to do periodic work while a condition is true, say, to do something every 5 ticks while a player is online. However, you do need to be careful that you can stop a while loop at will, since **reloading a script will not stop a running while loop**.

{% hint style="warning" %}
While loops will not stop when you reload the script that they are in. They will doggedly run until their condition is no longer true. This is a good reason to either use a periodical event (when appropriate) or to add a special case to abort a loop.

```applescript
while player is connected:
    send "hi"
    if {abort-while-loop} is true:
        set {abort-while-loop} to false
        exit loop
    wait 10 seconds

...

command abort-loop:
    trigger:
        set {abort-while-loop} to true
```
{% endhint %}

### Do While

Like while loops, but they check the condition at the end of the loop, rather than the start:

```applescript
# bad, normal while loop
add 1 to {_test}
while {check::%{_test}%} is set:
    add 1 to {_test}
    
# cool, epic do while loop
do while {check::%{_test}%} is set:
    add 1 to {_test}
```

## Loop-specific Syntax

As briefly referenced in the while loop section, loops have special syntaxes that can be used to control them.  These syntaxes are critical to making efficient and readable code with loops. These come in two main categories, the information syntaxes and the control syntaxes.

### Informational

`loop-value` and `loop-index` give you information on what's currently being looped. `loop-value` is the value, and will always be present. `loop-index` is only available when looping variable lists, and gives the index of the element in the list. As previously mentioned, you can also use `loop-%type%` when looping specific types, like `loop-number` when looping `integers between 0 and 10`.

```applescript
set {_my-list::1} to "hey"
set {_my-list::2} to "how"
set {_my-list::are} to "you"

loop {_my-list::*}:
    broadcast "%loop-index%: %loop-value%"
    # this prints the following:
    # 1: hey
    # 2: how
    # are: you
```

`loop-iteration` is useful for keeping track of what loop you're on. This can also be accessed with `loop-counter`.

```applescript
# loop the first 25 elements of {_list::*}
loop {_list::*}:
    if loop-iteration > 25:
        stop
```

### Control

`continue` is for when you want to skip the rest of the loop and _continue_ to the next element.

```applescript
# broadcast the square of all even numbers between 1 and arg 1:
loop integers between 1 and arg 1:
    # skip odd numbers 
    # (mod() gives the remainder, so dividing odd numbers by 2 gives a remainder of 1)
    if mod(loop-number, 2) == 1:
        continue
        
    broadcast loop-number * loop-number
```

`exit loop` is for, well, exiting loops. This is useful for preventing runaway while loops, as we've seen earlier, or just exiting once you find what you need.

```applescript
# searching for {_needle} in {_haystack::*}:
loop {_haystack::*}:
    if loop-value is {_needle}:
        set {_index} to loop-index
        exit loop # exit early, since the rest of the list doesn't matter.
broadcast "Found needle at %{_index}%!"
```

## When to Use Loops

{% hint style="info" %}
This is a more of the in-the-weeds performance comparison, so if you're just here to learn about loops, you can skip this.
{% endhint %}

Often, people will compare the performance of these two patterns:

```applescript
every 10 ticks:
    loop all players:
        # do something

on join:
    while player is connected:
        # do something
        wait 10 ticks
```

Before you keep reading, try to think about what the actual difference here is!

I'll wait.

Don't worry, I'll still be here.

Alright.

So, behind the scenes, there's a thing called a Scheduler. This is in charge of scheduling when things run on your server. When we make an `every x` periodical event, Skript tells the scheduler to run this thing every x time. This is pretty simple and straightforward, which means it's very easy for Skript to come back later and tell the scheduler to stop running it. The scheduler just gives Skript a number to call if it wants the task ended.

The downside here is that it runs the `do something` for every player, all at once. If you have a lot of players, or a lot of work to do per player, this can sometimes result in small lag spikes every time the code runs.

While loops, meanwhile, are a bit more complicated. Skript has to run the code within the loop to determine what the next behavior is, so when it hits the `wait 10 ticks`, it tells the scheduler "hey, can you restart this code in 10 ticks?" and the scheduler does just that. The downside, though, is that Skript doesn't really have control over the code anymore. Since every single iteration of the loop creates a new scheduled task, Skript doesn't know what number to call to stop the loop. This means that the while loop itself is the only thing that can stop it, which it does by failing the condition and not running the code inside itself, therefore not running the `wait`. **All this to say, `/sk reload` will not stop while loops.**

However, it also comes with some benefits. Since players don't usually all join at the exact same time, the while loops running for each individual player are going to trigger at slightly different times. This helps solve our problem with `every x`, where we were getting lag spikes.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
Note that we didn't change the amount of work that we were doing, we just spread that work out over multiple ticks.
{% endhint %}

This means the `on join, while online` pattern is good for spreading work out, but comes with the downsides of a) not stopping with a reload, and b) potentially starting multiple loops for one player if you're not careful.&#x20;

It also means that for short waits, like 1 to 5 ticks, the benefits of spreading work out will be very small, and a periodical event will be your better bet. While loops also can't address sustained lag, only lag spikes.

{% hint style="info" %}
TL/DR:

**every x, loop all players:**

* simple to set up
* safe
* reload-friendly
* can cause lag spikes if the work done is too much to do in one tick

**on join, while online:**

* can spread work over multiple ticks if the delay is long enough ( >5 ticks)
* not reload-friendly
* needs extra work to make safe (prevent multiple loops from running at once for one player)

**conclusion:**

prefer using `every x, loop all players` when you are working with fast timings or smaller amounts of work.&#x20;

prefer `on join, while online` when you are working with slower timings or larger amounts of work (updating scoreboards is a good example, you only need to do this at most once a second.)
{% endhint %}

