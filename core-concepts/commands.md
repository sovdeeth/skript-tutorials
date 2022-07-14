# Commands

### Custom Commands in Skript

Skript allows you to easily create custom commands in your scripts, like the following:

`# A simple "broadcast" command for broadcasting the text argument.`\
`# This is accessible only to users with the "skript.example.broadcast" permission.`\
``\
`command /broadcast <text>:`\
&#x20;   `permission: skript.example.broadcast`\
&#x20;   `description: Broadcasts a message to everyone including console.`\
&#x20;   `trigger:`\
&#x20;       `broadcast arg-text`

That's a simple example, but you can do much more complex things with custom commands! That "/broadcast" command only shows a few of the options you have available when creating a custom command.

### Command Pattern

Here's the full list of features that you can use in your commands. They're all optional, except for the trigger section. We'll explain each one individually below.

`command /<command name> <arguments>:`\
&#x20;   `aliases:             # alternate command names`\
&#x20;   `executable by:       # players or console`\
&#x20;   `usage:               # the message that explains how to use the command`\
&#x20;   `description:         # the command's description`\
&#x20;   `permission:          # the permissions needed to use the command`\
&#x20;   `permission message:  # the message sent to users without the correct permissions`\
&#x20;   `cooldown:            # a timespan, during which the command can't be used again`\
&#x20;   `cooldown message:    # the message sent when the command is used again too fast`\
&#x20;   `cooldown bypass:     # the permission necessary to bypass the cooldown`\
&#x20;   `cooldown storage:    # what variable to store the cooldown in`\
&#x20;   `trigger:`\
&#x20;       `# The code to run goes here.`\


### Command Name

The command name is what comes immediately after `command`. It can consist of any characters you want, except for space. Additionally, the / in front of the command is optional. This means `command /broadcast` and `command broadcast` are the same. Here are a few examples:

command /test-command:\
&#x20;   trigger:\
&#x20;       broadcast "Command: /test-command"\
\
command nickname:\
&#x20;   trigger:\
&#x20;       broadcast "Command: /nickname"\
\
command //set!:\
&#x20;   trigger:\
&#x20;       broadcast "Command: //set!"\


### Arguments

[Arguments](https://www.sovdee.com/expressions.html#ExprArgument) follow the command name, and are separated by spaces.\
You can make arguments optional by surrounding them with square brackets `[]`, like this: `command /kill [all entities]`.\
However, the real power in arguments comes from type arguments. These allow a command to take in a [type](https://www.sovdee.com/classes.html), like `command /kill <player>`. Type arguments must be surrounded by angle brackets `<>` and can also be made optional by surrounding them with square brackets: `[<player>]`.\
The argument can then be referenced in the trigger section by `arg-1` or `argument 1` or a [number of other ways](https://www.sovdee.com/expressions.html#ExprArgument). You can also name type variables like so: `<name:type>`, which can then be refenced as a local variable: `{_name}`. Here's the full pattern for arguments:

\<name:type=%default value%>

Arguments can be used in a lot of different ways, so I'll provide some examples ranging from the simplest possible to more complex uses.

\# this command can be run by "/test" or by "/test command".\
command /test \[command]:\
&#x20;   trigger:\
&#x20;       broadcast "Command: /test or /test command"\
\
\# sends the name of the given player. "of" is optional.\
command /name \[of] \<player>:\
&#x20;   trigger:\
&#x20;       send "Player's name: %arg-1%, %argument 1%, or %player-argument%" to player # here 'player' refers to the player executing the command\
\
\# heals the player in the argument. If no player is given, it will heal the sender.\
command /heal \[\<player=%player%>]:\
&#x20;   trigger:\
&#x20;       heal argument 1\
\
\# this can be used like /kill zombies, /kill creepers and animals in radius 100 or /kill monsters in the radius 6.\
\# the radius will default to 20 if isn't entered.\
command /kill \<entity types> \[in \[the] radius \<number = 20>]:\
&#x20;   executable by: players # this will be explained later on\
&#x20;   trigger:\
&#x20;       loop entities in radius arg-2 of player:\
&#x20;           if loop-entity is arg-1:\
&#x20;               kill loop-entity\
\
\# broadcasts the text given in the command. If no text is given, it will broadcast the default value of "default text".\
\# note that the text is referenced as {\_text input}, rather than arg-1 or similar.\
command /broadcast \[\<text input:text="default text">]:\
&#x20;   trigger:\
&#x20;       broadcast {\_text input}\


### Executable by

Who can execute this command. The options are `players`, `console`, or `players and console`.

command /shutdown:\
&#x20;   executable by: console\
&#x20;   trigger:\
&#x20;       shutdown the server\


### Aliases

Aliases are alternate names for your command. For example, the command `/teleport` could have an alias `/tp`. Like in the command name, the forward slash (`/`) is optional.

\# this command can be run by "/teleport" or by "/tp".\
command /teleport \<number> \<number> \<number>:\
&#x20;   executable by: players\
&#x20;   aliases: tp\
&#x20;   trigger:\
&#x20;       teleport player to location(arg-1, arg-2, arg-3)\


### Description

The description of the command. Other plugins can get/show this with `/help`, like `/help teleport`.

### Permissions

The permissions required to execute this command. The message sent to players without the proper permissions can be customized with the `permission message:` field.

command /shutdown:\
&#x20;   permission: server.admin\
&#x20;   permission message: Only admins can shut down the server!\
&#x20;   trigger:\
&#x20;       shutdown the server\


### Cooldowns

This field takes a timespan that the player must wait out before executing the command again. The cooldown can be canceled with `cancel the cooldown` ([documentation here](https://www.sovdee.com/effects.html#EffCancelCooldown)). Like with the permissions, you can change the default cooldown message with the `cooldown message:` field. The remaining time of the cooldown can be displayed with `%remaining time%` Additionally, you can store the cooldown in a variable with `cooldown storage:`, in order to store the cooldown even when the server restarts.

command /vote:\
&#x20;   executable by: players\
&#x20;   cooldown: 1 day\
&#x20;   cooldown message: You can only vote once a day!\
&#x20;   cooldown storage: {vote::cooldown::%uuid of player%\}\
&#x20;   trigger:\
&#x20;       add 1 to {vote::players::%uuid of player%\}\


### The Trigger Section

This section is where all the code the command should run is located. I'm sure you're familiar with how it works from the previous examples, but in case you're still unsure, some more examples of commands will be displayed here. You can see these example commands and more in the `/plugins/Skript/scripts/-examples/commands.sk` file in your server.

command /item \<items>:\
&#x20;   description: Give yourself some items.\
&#x20;   usage: /item\
&#x20;   aliases: /i\
&#x20;   executable by: players\
&#x20;   permission: skript.example.item\
&#x20;   cooldown: 30 seconds\
&#x20;   cooldown message: You need to wait %remaining time% to use this command again.\
&#x20;   cooldown bypass: skript.example.cooldown.bypass\
&#x20;   trigger:\
&#x20;       if player has permission "skript.example.item.all":\
&#x20;           give arguments to player\
&#x20;       else:\
&#x20;           loop arguments:\
&#x20;               if (TNT, bedrock, obsidian, mob spawner, lava, lava bucket) does not contain loop-item:\
&#x20;                   give loop-item to player\
&#x20;               else:\
&#x20;                   send "%loop-item% is blacklisted and cannot be spawned." to player\
command /home \<text> \[\<text>]:\
&#x20;   description: Set, delete or teleport to your home.\
&#x20;   usage: /home set/remove \<name>, /home \<name>\
&#x20;   permission: skript.example.home\
&#x20;   executable by: players\
&#x20;   trigger:\
&#x20;       if arg-1 is "set":\
&#x20;           if arg-2 is set:\
&#x20;               set {homes::%uuid of player%::%arg-2%\} to player's location\
&#x20;               send "Set your home \<green>%arg-2%\<reset> to \<grey>%location of player%\<reset>" to player\
&#x20;           else:\
&#x20;               send "You must specify a name for this home." to player\
&#x20;       else if arg-1 is "remove":\
&#x20;           if arg-2 is set:\
&#x20;               delete {homes::%uuid of player%::%arg-2%\}\
&#x20;               send "Deleted your home \<green>%arg-2%\<reset>" to player\
&#x20;           else:\
&#x20;               send "You must specify the name of this home." to player\
&#x20;       else if arg-2 is set:\
&#x20;           send "Correct usage: /home set/remove \<name>" to player\
&#x20;       else if {homes::%uuid of player%::%arg-1%\} is set:\
&#x20;           teleport player to {homes::%uuid of player%::%arg-1%\}\
&#x20;       else:\
&#x20;           send "You have no home named \<green>%arg-1%\<reset>." to player

Guide written by [sovde](https://github.com/sovdeeth), with parts used with permission from [blueyescat's tutorial](https://skripthub.net/tutorials/10) on skripthub.
