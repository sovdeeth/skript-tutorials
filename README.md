---
description: An entry point to Skript.
---

# Introduction

### Welcome!

This gitbook contains a crash course in Skript programming, so if you're here to learn, you're in the right place. This page contains the very basic overview of Skript, including installation and some basic ideas on how Skript works.&#x20;

We'll walk you through writing your first script, and hopefully get you more confident in using the language. After that, you can explore the rest of this site for more in-depth explanations and higher-level concepts. To get started, just click the `Installation` link at the bottom of this page.

For those of you who are already familiar with Skript, go ahead and use the navigation table on the left to jump to what you're interested in. If you're interested in helping out, the github repo is linked on the right. Pull requests are welcome!

{% hint style="warning" %}
**Note:** These tutorials are for the current version of the SkriptLang Skript fork (2.8.X) and are not guaranteed to be correct for other versions or forks. This site does not include any tutorials for addons, either.
{% endhint %}

## Installation

Installing Skript is no different from any other plugin. You download the latest jar file, which can be found at [https://github.com/SkriptLang/Skript/releases/latest](https://github.com/SkriptLang/Skript/releases/latest).&#x20;

{% hint style="warning" %}
You should ensure your server meets the requirements for running Skript:

* A Spigot, Paper, or Paper fork server jar. Spigot is the absolute minimum and Skript includes some features that are only accessible when using Paper or a fork of Paper.
* Minecraft Version. Skript 2.8 works for versions 1.13+.&#x20;
  * 1.8 support can be found [here](https://github.com/Matocolotoe/Skript-1.8/releases), but is not official, nor recommended, nor guaranteed to work.
  * 1.9-1.12 support is limited to old versions of Skript that do not receive updates (2.6.4)&#x20;
{% endhint %}

After you've downloaded the jar file, put it in your `plugins` folder in your server directory. Restart the server and Skript should generate the folder `plugins/Skript`.&#x20;

Inside should be some files, `config.sk`, and the folder `scripts`.&#x20;

{% hint style="info" %}
The `scripts` folder should have a folder of example scripts inside it. These are very useful for understanding how scripts are written.

You can take some time to read through them, or you can move directly on to making your first script. Click `The Basics` below to move on.
{% endhint %}
