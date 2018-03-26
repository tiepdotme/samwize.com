---
layout: page
title: "Apple Coding Style"
permalink: /development/apple-coding-style/
---

This coding style is for iOS, macOS, tvOS, watchOS, and whatever OS Apple adds to the family. 

For simplicity let's just call it Apple Coding Style. But we could have call it Xcode & Swift Coding Style, because the IDE and language plays a big part.


## Project Structure

Organize your project like how you clean and organize your room. Put things where they belong to, so that you can find them when needed.

**Project Structure** is made up the Xcode's groups and files.

A group is a container, and you can have smaller containers in it (subgroup).

The files are placed in these containers.

Many developers will have a traditional structure. If they use MVC architecture, they have the groups "Models", "Views" and "ViewControllers".

But no, your project structure should not reflect the architecture you use. Why?

What if you use multiple architectures? How would you mix in MVVM or VIPER?

As Uncle Bob say, an accounting software should shout ACCOUNTING when you look at the project structure (he call that "project architecture").

## Directory Structure

There is a secondary way of organizing.

**Directory Structure** are the actual folders and files. 

Many developers keep the Project and Directory structures in sync. But this is not a good use.

Directory structure should organize by the type of files you have: swift, storyboard, xib, resources, etc

As a project will have many swift files, you can further organize into the common type: View Controllers, Models, Managers, etc

How is this good?

We change Project Structure frequently, moving the groups, renaming it etc. That can be done without affecting the directory structure, and therefore in your git history, no change.

Putting the same type of files together will also help in the internal representation of localization efforts. When you localize a storyboard, Xcode will create a directory for each language (eg "fr.lproj" for French). By placing all storyboards in the same directory, they share the same eg. "fr.lproj".

## Keep close first

Keep related code as close as possible. Move them further when that code is being reused by others eg. moving to "common".

But when you first write any code, keep it close to where it is being used.

## Naming is hard

There are only 2 hard problems with computer science:

1. Names
2. Naming