Cheese Quest: The Plight of Kashkaval
====================================

Table of Contents
-----------------
1. [Introduction](#introduction)
2. [Development History](#development-history)
    - [Input/Output](#learning-about-standard-input-and-output)
    - [Functions](#learning-about-functions)
    - [Classes](#learning-about-classes)
    - [Saving](#learning-about-saving-games-to-files)
3. [Notes on Design Decisions](#notes-on-design-decisions)
    - [Player death](#player-death)
    - [Forewarning of death](#forewarning-of-death)
    - [Tedious content](#tedious-content)
    - [Alternatives to saving](#alternatives-to-saving)

Introduction
------------
Cheese Quest is a text-based adventure game, or more commonly referred to as Interactive Fiction, inspired by many of the games published by Infocom in the 1980s.
It started development in 2016 while I was in my 3rd year of Geophysics at the University of Calgary before I officially enrolled in Computer Science.
While it works and has most of its critical bugs taken care of, **it is a clear example of what not to do**.

With that in consideration, this repository is more of a showcase documenting what I've done in the past than anything else;
That, and a way I can embarrass myself to the world.
Maybe later down the road I can look back and see how much I've learned over the years.

The game world spans over 100 rooms, and takes approximately 2 hour to complete.

Development History
-------------------

Cheese Quest started development in 2016 while I was in my 3rd year majoring Geophysics at the University of Calgary, before I officially enrolled in Computer Science.
I began working on it shortly after I started taking my first intro to computer science course for non-computer science majors (CPSC 217).

As a result, the code was essentially a giant series of if/elif/else statements encased in a single multi-thousand line while-loop, where problems were repeatedly solved with ad hoc band-aid fixes with little code reusability.
#### Learning about standard input and output
As soon as I learned that I could receive input from the keyboard and output text to the screen, I knew I wanted to create a simple IF game.
Infocom's Hitchhiker's Guide to the Galaxy IF released in 1984 was a big part of my childhood and a game I really came to appreciate because the whole concept of a game based on a typing in commands seemed so cool and different from any other games I've played.

#### Learning about functions
While I knew what functions were at the inception of Cheese Quest's development, I don't believe the concept of using functions to reuse code entirely sunk in until quite some time.
I did make a couple utility functions (just to reverse a string which I only use once), and some functions that were just to print text.
Despite that, I essentially used functions to represent the game's current menu state: in the main menu, in a game over prompt, or in the game. So in some sense, the menu functions acted as "macro while loops".

Looking back at it now as I write this, this may potentially cause some stack overflow issues if the player goes back and forth between menus enough times, but that was a concept entirely foreign to me at the time.

#### Learning about classes
At some point in development I wasn't entirely sure how I was going to store all the data in the game.
I was already doing some of it through global variables, and some through local variables within the `main()` function.
I did some research and learned about classes and objects, although I didn't figure out about the whole paradigm of object-oriented programming until my 2nd Computer Science course (which I didn't take until later on when most of the game was already complete).

As a result, only a few components of the game were converted to classes, being Inventory, Room, and Stat (player statistics).
Part of me wanted to convert more components of the game's code into classes (also sort of discovering object-oriented programming on my own while being blissfully unaware of it already being a thing), but I figured I might as well spend that time adding more content.

#### Learning about saving games to files
Yeah... I never figured this out during the game's development, which is why the game does not have a formal save/load game system but instead uses spells (more details under design decisions).
It probably would have helped if the game's state was actually contained as an object that could be saved as a file instead of just being a while-loop changing a bunch of local and global variables.

This problem is one of the central reasons that prompted me to start development on [Cheese Quest 2](https://github.com/EvanQuan/CheeseQuest2).

## Notes on Design Decisions
*I made some notes while developing this game. I thought I'd put them here just for historical documentation purposes. **They contain spoilers** since they were notes for myself and not for anyone else:*

Many of the design decisions were a work-around to the lack of implemented save feature and the problems that arise from it.
### Player death
Throughout the game there are various opportunities for the player to die, and due to the lack of saving, results in the entire game resetting. This results in multiple problems that need to be accounted for.
### Forewarning of death
Later game deaths are far more punishing than earlier ones since more content needs to be repeated to return back to the latest game state prior to death. This can be a problem if situations of possible death are not probably communicated to the player. These later deaths can be frustrating or confusing if opportunities to avoid them are not clear or forgiving enough. They no longer feel like punishments for player mistakes, but rather punishments out of nowhere.

To solve this, situations that can cause death deaths are more obvious in later stages, or if triggered, do not immediately result in death, and allow the player to recover if prepared properly. For example:
- The dying from starvation within the jail cell does not give the player many turns to prevent it, especially when the player is not familiar with the controls and what the player is capable of doing. This is fine because it is at the very beginning of the game and no progress is lost upon death.
- The guardian at the Temple harms the player unless the player makes the mistake of incorrectly answering the riddle. The player is also not immediately killed (unless they use a demon spell), but instead injured, meaning that players who prepared and have bandages in their inventory can heal the wound and continue playing.
- The creature in the mines gives over 20 turns of notice when it has appeared, and the player at any part of the cave has enough turns to leave the cave to safety. The creature also injures the player the turn before killing them, allowing for an extra turn of escape.
### Tedious content
Certain game puzzles or moments in the game, while interesting at first, may become tedious or annoying if constantly repeated. This may be mainly due to the player knowing what they want to do and how to do it, but requiring multiple turns to complete the action. Therefore, these aspects of the game should be shifted to later on as to decrease this repetition. For example:
- Escaping the jail cell required multiple steps, which needed to be done in a timely manner or the player would be killed from starvation. Instead of shifting the content to later on (which cannot happen as this is the beginning of the game), once the player successfully escapes, any subsequent deaths results in all the objects necessary to escape already in the player inventory, and the mysterious figure does not appear.
- The vault was originally in the jail corridor, right outside the jail cell. The vault code changed between every life, and each number needed to be set individually. It was later moved to the entrance of the Mount Magna.
### Alternatives to saving
For a game that takes approximately 2 hours to beat without a save feature, there should be some sort of checkpoint system when important points in the game are completed, so they do not need to be repeated. Spells are generally rewarded after certain game events which can be used to bypass that respective event. In turn, this makes many spells only useful if the player dies. To help with this, the spells and their effects are saved between deaths so they do not need to be memorized, although they are lost between different game sessions.
