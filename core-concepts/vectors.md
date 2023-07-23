# Vectors

Vectors are extremely useful tools when working with things in 3d space, whether you need offsets, angles, velocity, and more. They're most commonly used in Skript when working with velocities and particles, but vectors can do a lot more than just that.

Vectors can be thought of as an arrow in 3D space, with some length and direction that it points in. Notably, they do **not** have a location associated with them. This is one of many ways to conceptualize a vector, but it's the easiest to picture for many people.

To help with explanations and examples, we will be comparing and visualizing them as arrows in a 3D grid, but remember that this is one representation!

{% hint style="danger" %}
**We highly recommend not skipping sections of this tutorial.**

&#x20;Terms and concepts tend to build off of previous ones, and if you skip ahead you may find yourself a little lost and confused.
{% endhint %}

***

## Basic Vector Creation

Before we create a vector, we need to first understand what they are composed of. Vectors in Skript are composed of only 3 numbers, defined as X, Y, and Z.

The values define the "offset" in that axis, similar to how a location defines your "offset" from the center of the world on each axis. Although looking similar to locations, they **are not** the same and do not behave the same.

To give an example, the location 5, 10, -5 means you're 5 blocks from the origin (0,0,0) in the positive x direction, 10 blocks in positive y, and 5 in the negative z direction. A vector of 5, 10, and -5 means the same thing, but it has no specific origin. It is **not** tied to the world's origin, or any specific origin.&#x20;

The easiest way to create a vector in Skript is to literally define the XYZ values all at once, which can be either done with the `vector` function or the `vector` expression:

```vb

# Using the vector function
set {_v} to vector(0, 1, 0)

# Using the vector expression
set {_v} to vector 0, 1, 0

```

{% hint style="info" %}
It does not matter which method you use; they produce the same result. I personally stick to using the function method for consistency's sake.
{% endhint %}

If you were to visualize the vector that was created using our arrow analogy, it would look something like:

![An image of an arrow pointing straight up](../media/vectors/straightup.png)

The light blue arrow is the vector we just created. It's pointing straight up because we set the X and Z values to 0 and just the Y is 1.

In case the concept of looking at vectors from their individual components ( XYZ ) has not quite set in, here's a visualization of how the vector changes based on its components:&#x20;

![A vector being created from separate components](../media/vectors/comp.gif)

The aqua arrow is the resulting vector and the red, green and blue arrows are the XYZ components. Feel free to ignore the length for now, we will get to that in more detail.

There are other ways to create vectors, but we'll stick with this method for simplicity. The others will be introduced later.

***

## Basic Arithmetic

Just like with normal numbers, you can perform arithmetic ( math ) with vectors just as easily, the only difference is that you need to double the sign, ie `*` becomes `**.`

For the sake of example, we will be using the vector function to define the vectors here, but these could also be variables or other vector expressions like `player's velocity`.

```vb

set {_v} to vector(1, 2, 3) ++ vector(4, 5, 6)
# {_v} will be vector(5, 7, 9)

set {_v} to vector(1, 2, 3) -- vector(4, 5, 6)
# {_v} will be vector(-3, -3, -3)

set {_v} to vector(1, 2, 3) ** vector(4, 5, 6)
# {_v} will be vector(4, 10, 18)

set {_v} to vector(1, 2, 3) // vector(4, 5, 6)
# {_v} will be vector(0.25, 0.4, 0.5)

```

{% hint style="warning" %}
Currently the docs show that you are able to perform arithmetic with a vector and a scalar ( a single number ), which is no longer true.&#x20;

If you need to perform such operation, then simply replace the number with a vector where all 3 components are that number. For example, instead of `vector(1, 2, 3) * 5`, you would do `vector(1, 2, 3) ** vector(5, 5, 5)`
{% endhint %}

***

## Length

Along with having an XYZ, vectors also have a length, which is calculated from its XYZ components - it's simply done using the euclidean distance formula: `sqrt((x)^2 + (y)^2 + (z)^2)`.

You can literally think of the length as the length of the "arrow", from the base to the tip. Combining the concept of length and the XYZ components, we can see how the length changes as the XYZ components are changed, and in turn how the "arrow" looks:

![A gif showing the behavior of XYZ components being changed](../media/vectors/lenandxyz.gif)

The syntax for length is as follows:

```vb

[the] (vector|standard|normal) length[s] of %vectors%
%vectors%'[s] (vector|standard|normal) length[s]

```

And an example of getting the length:

```vb

set {_len} to vector length of vector(1, 2, 3)

```

The expression also allows you to modify the length, which will modify the components accordingly to meet that length:

```vb

set {_v} to vector(1, 2, 3)
send "vector: %{_v}%"
send "length: %vector length of {_v}%"

set vector length of {_v} to 10
send "vector: %{_v}%"
send "length: %vector length of {_v}%"

```

And if you run that code, you'll see a result similar to this:

```vb

vector: x: 1, y: 2, z: 3
length: 3.74

vector: x: 2.67, y: 5.34, z: 8.01
length: 10

```

***

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

***

## Yaw and Pitch

Last thing before we dive into the interesting stuff is yaw and pitch.

Yaw and pitch are values in degrees that define your view direction in the world. Yaw is your horizontal rotation ( left and right, like on a compass) and pitch is your vertical rotation ( up and down ), together they allow us to describe any looking direction with 2 numbers.

![An image showing a sphere outlining the difference between yaw and pitch](../media/vectors/yawpitch.png)

Yaw extends from 0 to 180, and then from -180 back to 0. South is 0, and 180/-180 is north. You can also use 0 to 360, where -90 is the same as 270.

Pitch extends from -90 to 90. 90 is straight down, -90 is straight up and 0 is straight forward.

{% hint style="info" %}
For those interested in math, yaw and pitch are used in the [**spherical coordinate system**](https://en.wikipedia.org/wiki/Spherical\_coordinate\_system) along with radius, or length, to determine a point in 3d space. It's essentially drawing a sphere in space with a certain radius, and then using the two angles to select a single point on its surface.

Our first method, with XYZ, was your typical **Cartesian** coordinate system.&#x20;

The spherical coordinate system uses yaw, or polar angle; pitch, or azimuthal angle; and length, or radius/radial distance.

These two can be easily converted back and forth using vectors, which is one of the most useful aspects of vectors in Skript.
{% endhint %}

***

## Other Ways of Creating Vectors

Now that we got the introductory information out of the way, we can move onto the cooler stuff with vectors.

Creating vectors using the `vector()` function is cool and all, but it can be seen as limited and difficult to use for some applications. Luckily, Skript comes with a few other expressions that greatly help us.

### **Vector From Yaw and Pitch**

Expanding upon the concept of yaw and pitch, we can use them to create a vector that's pointing to that specific direction.

The expression looks like:

```vb

[a] [new] vector (from|with) yaw %number% and pitch %number%

```

Here's an example of creating a vector that points south and a little upwards:

```vb

set {_v} to vector from yaw 0 and pitch -15

```

If you were to visualize the vector with the setup from the yaw and pitch section, this is what you would see:

![An image showing a sphere representing yaw and pitch](../media/vectors/vectoryawandpitch.gif)

_Note: Keep in mind that this expression will always return a vector with a length of 1_

### **Vector from Radius, Yaw, and Pitch**

This expression is identical to the previous, the only difference is that it allows you to adjust the length of the vector along with the yaw and pitch at the same time. &#x20;

{% hint style="warning" %}
Remember, vectors created with this syntax are no different from vectors created with the XYZ or cylindrical syntaxes. The name only refers to the way they're created.
{% endhint %}

The syntax looks like:

```vb

[new] spherical vector [(from|with)] [radius] %number%, [yaw] %number%(,| and) [pitch] %number%

```

And a simple example:

```vb

set {_v} to spherical vector radius 2, yaw 22.5, pitch 45

```

Now with this expression, it includes the term `spherical`, which just describes how the vector is being created, essentially a vector is just being made that goes from the center of a sphere to the outer edge.

{% hint style="info" %}
For those that read the previous math note, this is because it uses the **spherical** coordinate system.
{% endhint %}

### **Vector from Radius, Yaw, and Height**

Unlike the previous two expressions, this expression creates a vector using a cylindrical shape, where the vector is made from the center of a cylinder to a point on the side at a specific height.

The syntax is as follows:

```vb

[a] [new] cylindrical vector [(from|with)] [radius] %number%, [yaw] %number%(,| and) [height] %number%

```

Now of course, similar setup to the other expressions:

```vb

set {_v} to cylindrical vector radius 2, yaw 45, height 5

```

And if you were to visualize the created vector:

![An image showing a vector being made from the center to the side of a cylinder](../media/vectors/cylvector.gif)

{% hint style="info" %}
For those that read the previous math note\[s], it's called cylindrical because it uses the **cylindrical** or **polar** coordinate system.
{% endhint %}

### **Vector from Location**

This is a bit of a niche but simple expression, but it has uses so it will be discussed anyway.

All this expression does is return a vector with the same components as the input location:

```vb

[the] vector (of|from|to) %location%
%location%'s vector

```

And some examples:

```vb

set {_v} to vector from location of player

set {_v} to vector from location(0, 64, 0)

```

In the second example, the returned vector will become `vector(0, 64, 0)`.

And as you may have guessed, this does not make the returned value a location, it will be a vector. Which means that the other data ( such as world, yaw, and pitch ) will be lost.

### **Location from Vector**

Again this is a bit of a niche but simple expression, but it has uses so it will be discussed anyway.

This may come off as no surprise, but this expression does the opposite of the previous, it will take the components of the vector, a world, and optionally a yaw and pitch and turn it into a location:

```vb

%vector% [to location] [in] %world% 
location (from|of) %vector% [(from|in)] %world% 
%vector% [to location] [in] %world% with yaw %number% and pitch %number% 
location (from|of) %vector% [(in|from)] %world% with yaw %number% and pitch %number% 

```

```vb

set {_l} to vector(2, 4, 6) to location in world "world"

set {_myLoc} to location from vector(7, 8, 9) in world("world_the_end") with yaw 45 and pitch -90

```

If the yaw and pitch are not included, then both will be defaulted to 0.

### **Vector Between Locations**

This expression simply gets the vector from one location to another. In other words, can be thought of just subtracting the components from the first location by the second.

The returned vector "points" from the first location to the second, and the length being the distance between the two locations.

```vb

[the] vector (from|between) %location% (to|and) %location%

```

And can be used like:

```vb

set {_v} to vector between player and location(100, 16, 200)

```

If it still seems confusing, you can implement what this expression does by getting the vectors from both locations and then subtracting them:

```vb

set {_v1} to vector from location(0, 64, 0)

set {_v2} to vector from location(100, 32, -20)

set {_v3} to {_v2} -- {_v1}

```

But it's still recommended to use the built in expression for this, both for performance and cleanliness reasons.

### **Entity Velocity**

The velocity of something represents its speed and direction in meters per tick, which in Skript is represented as a vector ( yay ). Currently in Minecraft only entities have velocities.

```vb

[the] velocit(y|ies) of %entities%
%entities%'[s] velocit(y|ies)

```

Along with being able to get the velocity of an entity, we are able to modify it:

```vb

set {_v} to velocity of player
set {_v2} to {_v} ** vector(2, 2, 2) ++ vector(1, 2, -3)
set velocity of player to {_v2}

```

{% hint style="warning" %}
Having the vector `vector(1, 0, 0)` for example, as your velocity does not mean that you will be travelling exactly 1 meter per tick in the X direction, as the game has other calculations it performs when moving the entity, such as gravity and drag.
{% endhint %}

***

## Miscellaneous Syntax

A list of syntax that doesn't really fit into another category above nicely or by itself, but is still useful to include and discuss.

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

### **Random Vector**

This expression, as the same suggests, returns a random vector. More specifically, it returns a random unit vector where each component is a random number between `-1` and `1`, this expression is preferred when you need random vectors instead of generating the numbers yourself as the components.

```vb

[a] random vector

```

And the the code is as simple as you can get...

```vb

set {_v} to a random vector

```

As expected, every time that code is ran, `{_v}` will be a different vector every time.

{% hint style="danger" %}
In 2.6.4, this expression isn't fully random. The 3 random numbers have a bias towards pointing to the corners of a cube, like towards 1, 1, 1. This is fixed in 2.7.
{% endhint %}

## Slightly Advanced Syntax

The concepts below are typically deemed as a little outside of the basics of vectors, but shouldn't be too complex if you understood everything so far.

### **Angle Between Vectors**

### **Rotate Vector Around XYZ**

### **Rotate Vector Around Vector**

### **Vector Dot Product**

### **Vector Cross Product**
