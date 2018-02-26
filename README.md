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
    - [Completion and Abandonmnt](#completion-and-abandonment)
3. [Notes on Design Decisions](#notes-on-design-decisions)
    - [Player death](#player-death)
    - [Forewarning of death](#forewarning-of-death)
    - [Tedious content](#tedious-content)
    - [Alternatives to saving](#alternatives-to-saving)

Introduction
------------
Cheese Quest is a text-based adventure game, or more commonly referred to as interactive fiction, inspired by many of the games published by Infocom in the 1980s.
While it works and has most of its critical bugs taken care of, this game is a clear example of what **not to do** when developing a game or any program for that manner.

With that in consideration, this repository mainly serves to document what I've done in the past for my own personal use rather than to show others what I am capable of.
Think of it as a cautionary tale of novice programming gone wrong.
Maybe later down the road I can look back at this and see how much I've learned over the years.

The game world spans over 100 rooms, and takes approximately 2 hour to complete.

Development History
-------------------

Cheese Quest started development in 2016 while I was in my 3rd year majoring Geophysics at the University of Calgary.
I had no prior programming knowledge or experience, and was taking an introduction to computer science course for non-computer science majors (CPSC 217) as required for my degree.

#### Learning about standard input and output
As soon as I learned that I could receive input from the keyboard and output text to the screen, I knew I wanted to create a simple IF game.
Infocom's Hitchhiker's Guide to the Galaxy IF was a big part of my childhood and a game I really came to appreciate because the whole concept of a game based on a typing in commands seemed so cool and different from any other games I've played.

At first, Cheese Quest was just going to be a couple of rooms with some puzzles.
The implementation wasn't particularly well thought-out since (A) I only intended it to be something small and (B) I was completely new to programming as a whole.
As a result, the code was essentially a giant series of if/elif/else statements encased in a single multi-thousand line while-loop, where problems were repeatedly solved with ad hoc band-aid fixes with little code reusability.
Sticking with this approach would ultimately lead to the downfall and eventual abandonment of the project.

#### Learning about functions
While I knew what functions were at the inception of Cheese Quest's development, I don't believe the concept of using functions to reuse code entirely sunk in until quite some time.
I did make a couple utility functions (just to reverse a string which I only use once), and some functions that were just to print text.
Despite that, I essentially used functions to represent the game's current menu state: in the main menu, in a game over prompt, or in the game itself.
Looking back at it now as I write this, this might have caused some stack overflow issues if the player goes back and forth between menus enough times, but that was a concept entirely foreign to me at the time.

#### Learning about classes
Later on in development when I decided I was going to increase the scope of the game, I wasn't entirely sure how I was going to store the data of all the rooms and items.
I was already doing some of it through global variables, and some through local variables within the `main()` function.
I did some research and learned about classes and objects, although I didn't figure out about the whole paradigm of object-oriented programming until my 2nd Computer Science course (which I didn't take until later on when most of the game was already complete).

As a result, only a few components of the game were converted to classes, being Inventory, Room, and Stat (player statistics).
Part of me wanted to convert everything into classes, almost discovering object-oriented programming on my own while being blissfully unaware of it already being a thing, but I figured I might as well spend that time adding more content instead.

#### Learning about saving games to files
Yeah... I never figured this out during the game's development, which is why the game does not have a formal save/load game system but instead uses spells ([more details on that here](#alternatives-to-saving)).

This problem is one of the central motivators that prompted me to start development on [Cheese Quest 2](https://github.com/EvanQuan/CheeseQuest2).

#### Completion and Abandonment
Shortly after starting my second Computer Science course (CPSC 219) and learning about object-oriented programming, it quickly dawned on me just how terribly designed my initial approach to the game was.
I kind of already knew that the code was terrible early on since I had to move around the file through 4 or 5 different locations if I wanted to add a new item or room, but it never really bothered me since the game was of an intially small scope.
I completed the game to the point that it was actually winnable, and continued changing and adding content afterwards due to feedback.
This was all the while the idea of creating a new version of the game from scratch lurked in the back of my mind.
Eventually, I began to understand that if I were to increase the game's scope further that I would have to make some drastic changes to the code if I were to continue working on it.

After learning about creating GUIs through Java's javax.swing and java.awt packages, I finally decided I would remake Cheese Quest in Java.
I liked Java's approach to object-oriented programming better than Python's, and while I'm probably in the minority here, I also liked its overly verbose syntax as well.
While I understood that it would take some time to build the game's foundation from the the ground up before I could actually start adding any real content, it didn't really bother me.
After all, I never intended to share this game to anyone beyond maybe 2 or 3 other people, and I took this more as a learning experience rather than anything else.
I abandoned any serious development of the Python version although I occationally made small patches afterwards.

*Moral of the story: Creating a game with a foundation designed with little to no Computer Science knowledge and building upon it until it grows into an unmaintainable monstrosity is probably not a good idea. Who would have guessed?*

## Notes on Design Decisions
*I made some notes while developing the game. They were initially in their own file, but I thought I'd put them here just to share.
**They contain spoilers.***

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
