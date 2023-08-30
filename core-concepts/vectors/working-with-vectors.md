---
description: How do I use vectors?
---

# Working with Vectors

Vectors have tremendous use in Skript, but they're not all that useful if you can't actually use them to interact with the actual world. This is where **Location Vector Offset** comes in to play.

## Location Offsets

Now that you understand the basic concepts of vectors, we should now mention the ability to offset locations with vectors. This should help solidify the idea of vectors, as this is something you can play with in game.

Although vectors are not locations, but rather an "arrow" or "offset" in space, in Skript we can offset locations by vectors to get a new location with the given expression(s):

```vb

%location% offset by [[the] vectors] %vectors%
%location%[ ]~[~][ ]%vectors%

```

And an example looks like:

```vb

set {_l} to location of player ~ vector(0, 1, 0)

```

The code takes the location of the player and offsets it by the vector `vector(0, 1, 0)`, which just returns the same location but 1 meter higher.

The expression isn't black magic; all it does is take the XYZ of the location and adds the XYZ of the vector to it.&#x20;

```applescript
# Assume the player is standing at 3.4, 63, -15.7
set {_l} to location of player ~ vector(0, 1, 0)
# We take 3.4 + 0, 63 + 1, and -15.7 + 0 to get
# the final location of 3.4, 64, -15.7
```

Feel free to try and play around with this expression. For example, try and teleport yourself to a location offset by a vector, or set a block at a location offset by one. This expression is a really good start to playing around with vectors before moving onto more detailed things.

{% hint style="info" %}
Offsetting is one of the only and most common way of dealing with both locations and vectors. Get used to seeing it a lot.&#x20;
{% endhint %}

## Practical Example: Particles

One of the most common uses of vectors in Skript is to create particle effects. And one of the most useful shapes to create with particles is a line. And it just so happens that vectors are _really_ good at making lines.

At this point, you should be familiar with vectors having length and direction. Well, lines also have lengths and directions, don't they? If we have a vector pointing in the right direction, we can use it to make a line!

The idea here is to draw a bunch of particles along a certain line, ideally between two points that we know. To do this, we're going to need a bunch of locations along that line where we can spawn our particles. We can get these locations by starting at one end of the line and taking small steps towards the other end of the line, but to do that we need to know what direction to move in. This is where our vector comes in.

Since we have two locations, we can use the `vector from %location% to %location%` expression. We'll call the first point `{_start}` and the other point `{_end}`.&#x20;

```applescript
set {_vector} to vector from {_start} to {_end}
```

Now, `{_vector}` is pointing from start to end, but it's the entire length of the line. If we use it as is, we'll only have a particle at the start and the end. Not much of a line.

So to get our small steps, we need to make our vector small too:

```applescript
set vector length of {_vector} to 0.2
```

This sets the length of our vector to 0.2 meters, which means five particles per block. This should be plenty. Now we just need to start at `{_start}` and use our vector to move towards `{_end}`:

```applescript
# draw a particle at {_start}
...
# move start along the line, using our vector
set {_start} to {_start} ~ {_vector}
```

If we repeat this enough times, we reach `{_end}` and have drawn the whole line. Of course, the tricky part is knowing exactly how many times we need to move `{_start}` to reach the end. That challenge is left to you to figure out.

Here's a gif showing how this process works, in steps. The light blue is our vector.

<figure><img src="../../.gitbook/assets/ParticleLine.gif" alt=""><figcaption><p>An animation of how to draw a line using a vector.</p></figcaption></figure>

***

## Miscellaneous Syntax

Here's some extra syntaxes that you may find useful while working with vectors.

### **Normalization**

Normalizing a vector just means to take the length of it and make it `1`, but still keep the rest of the components proportional. When doing this, the vector can be referred to as a unit vector.

In Skript you can normalize a vector in a couple ways, first by using the included `normalized` expression:

```vb

normalize[d] %vector%
%vector% normalized

```

And then of course giving it a vector:

```vb

set {_v} to vector(1, 2, 3)
set {_v2} to normaized {_v}

```

And the other option, would be to just set the length of the vector to 1 ourselves:

```vb

set {_v} to vector(1, 2, 3)
set {_v2} to {_v}
set vector length of {_v2} to 1

```

Either way will result in the same, `{_v2}` will be around `vector(0.26, 0.53, 0.8)`

Normalizing may seem like a useless thing to perform, but it has great uses. Typically in math we like to deal with nice and clean vectors, which normalizing lets us do easily. Towards the end of this wiki we will discuss use cases and examples involving vectors, where some will utilize normalization.

### Rotate Vector Around XYZ

We already know how to rotate a vector by changing its yaw and pitch, but there are other ways as well. This effect allows you to rotate a vector around one of the 3 main axes, X, Y, or Z.

This can be very useful when rotating shapes or trying to get a certain direction from a player.

```applescript
rotate %vectors% around (x|y|z)(-| )axis by %number% [degrees]
```

```applescript
set {_v} to vector(1, 1, 1)
rotate {_v} around x axis by 90
# v is now (1, -1, 1)
rotate {_v} around y-axis by 45
# v is now (1.41, -1, 0)
rotate {_v} around z-axis by 180 degrees
# v is now (1.41, 1, 0)
```

### **Squared Length**

This expression does the exact same as the normal `vector length` expression, but it leaves out the final `sqrt` function, leaving the final result to be the length squared - hence the name.

This expression is most useful when you need to check if a vector is longer than another vector and you need the extra performance ( square roots are considered an intensive operation ).

```vb

[the] squared length[s] of %vectors%
%vectors%'[s] squared length[s]

```

```vb

set {_v} to vector(1, 2, 3)
set {_len} to vector length of {_v}
set {_sqrlen} to squared length of {_v}

```

As expected, `{_len}` will result in around `3.74` and `{_sqrlen}` will result in around `14`.

###
