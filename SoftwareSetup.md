---
title: Software Setup
parent: Setup
has_children: false
nav_order: 1
---


# How to Set Up: Software

### What do I need?

- Java version 9 or higher
- IDE (Eclipse and VSCode both work, but this tutorial assumes you're using Eclipse)
- MiniLib and sample robot code -- [Download latest version here](https://github.com/HHS-Team670/MustangMinibots/releases)

### Getting Started

- Create a new Eclipse project
- [From here, download 3 files: frc.zip, MiniLibJ.jar, and pi4j-core.jar.](https://github.com/HHS-Team670/MustangMinibots/releases)
- Add MiniLibJ.jar and pi4j-core.jar to your project folder: simply dragging them in should work. These contain the libraries you'll need, so **make sure they do not end up in  /src/ !** 
- Extract the files from frc.zip. You get a folder called frc, with another folder called team670 right inside it. Add the entire frc folder to /src/ in your Eclipse project: this is sample robot code that you'll be working with.
- You should be seeing a ton of errors in the code you just added. This is because that code requires some libraries which we haven't 
made accessible! To fix this, add MiniLibJ.jar and pi4j-core.jar to your project build path like this:

![setup](https://github.com/670Mustangs/minibots/blob/master/images/addjar.JPG?raw=true)


- Now, hopefully there are no more errors popping up in the code. Your project should look like this:

![final](https://github.com/670Mustangs/minibots/blob/master/images/sample.JPG?raw=true)


### Next Steps

At this point, you're ready to start writing code for the minibots -- but you still need to get your code onto the bots to run them!
To do that, first we need to export our code into a runnable JAR file...






