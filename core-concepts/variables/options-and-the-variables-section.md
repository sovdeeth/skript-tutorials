# Options and the Variables Section

The Options section and the Variables section are two powerful tools for making your scripts easier to read and write, but both can be easily mis-used and can actively make your scripts worse by increasing parse times, hiding bugs, and making code hard to read. We'll go over how each section works, the benefits, and the downsides of each, as well as how to maintain a good middle-ground.

## Options

Options, for those of you who have programmed in C, are like macros. For those of you who haven't, they're keywords that represent bits of code. In the options section, you say&#x20;

```applescript
options:
    error-msg: You've encountered an error!
```

and then you can use `{@error-msg}` in your code. It will be replaced with the exact code you put in the options section, unlike a variable set to the same string.

```applescript
set {_a} to "{@error-msg}"

send "{@error-msg}"
```

This is double-edged sword. Notice that even though we put the `{@error-msg}` directly in quotes, it'll still be replaced by the text in the options section. However, this can go wrong in a few ways. What if we put quotes in the option section too? Suddenly we have the following in our code, but we can't actually see it, because it's hidden behind an option.

<pre class="language-bash"><code class="lang-bash">send "{@error-msg}"
# could become
<strong>send ""You've encountered an error!""</strong></code></pre>

Be careful with options, then can help you cut down the amount of code you actually write, but they can also help obscure bugs and errors. It's also good to be wary of options in terms of parse times. I've seen people put items into options because they were tired of writing them out, or copy-pasting them.&#x20;

```applescript
options:
    item-1: diamond sword of sharpness 3 named "hey" with lore "no", "yes", and "maybe so" 
    
# later on...
set slot 10 of {_gui} to {@item-1}
```

This is bad practice. First, it obscures how long the line actually is, and can lead to people writing what looks to be short lines but are actually really long and cause the parser to struggle to figure out what they mean. Second, the parser has to parse the code again every single time you use the option. It's doing a lot more work for no good reason.

If you put the item into a global variable instead, it avoids both these issues:

```applescript
on load:
    set {item-1} to diamond sword of sharpness 3 named "hey" with lore "no", "yes", and "maybe so"
    
 
# later on...
set slot 10 of {_gui} to {item-1} 
```

For you, it's nearly the same. But for the parser, it's wayyy better. **In general, try to use global variables before using options**. Options should be reserved for when they're truly needed, or for small roles like the prefixes of variable names.

## Variables Section

Many new Skripters get the wrong impression about the variables section. Many think that you have to put the names of your global variables here in order for them to work. This is not true. The variables section is really just a glorified `on load` event. Here's how it works.

```applescript
variables:
    {test-var} = 10
    {test-var-2::%player%} = 10
```

Essentially, the variables section defines default values for a variable. If {test-var} isn't already set, the variables section will set it to 10 when the script is loaded. That's it. Nothing special.

However, it is a little special for variables with types in their names, like `{test-var-2::%player%}`. Here, the variable section will give a default value of 10 to _every_ element of {`test-var-2::*}` that has a player index. If you have a new player join, and you want to get the value of `{test-var-2::%your new player%}`, the variables section will make sure that it's 10, rather than nothing.

This is a useful feature, but in my opinion the variables section is mostly useless. I've gotten by for four years now without ever using it, even though I've known of its existence for 3 of those years. Setting values in `on load` or `on first join` works just as well, and can help avoid misinterpretations of what variables are set to what. Remember, **the variable section only sets the variable if it isn't already set**.&#x20;
