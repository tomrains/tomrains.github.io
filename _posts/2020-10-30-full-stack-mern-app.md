---
layout: post
title: Full-Stack MERN App
subtitle: React, Express, React, and Node create a titillating party ganme
comments: true
---

## So what is this app, anyway?
My [full-stack MERN app](https://secret-wildwood-99621.herokuapp.com/) is based on an old-school pen and paper activity called the Story Game. This digital version lets you:  
* Create outrageous, hilarious stories with friends
* Play either remotely or together (with your faces in your smartphones)
You'll need at least one friend to help you test the app out. If you want to learn more about how I built it and its technical capabilities, read on.

## How did you make the app?
I wanted to build a full-stack app. I already knew and loved React, so I opted for a MERN stack app.

## How did you get the idea for this app?
I've always loved the pen and paper version of the Story Game. I don't laugh easily, but this game often has me crying with laughter at the Kafkaesque stories that come out of it. Sometimes though, it's hard to read what people wrote, and I've learned some people don't really like writing with pen and paper, so I decided to make an app version, especially since I knew it would require a backend.

## What are the rules of the game?
First, you begin writing the first 135 to 150 characters of a story. You can't write more than 150. Then, you submit your story. Once all players have submitted, you will receive a story someone else has written, but you'll only be able to see the last few words they wrote. You'll have to continue the thread of the story based on this small amount of context. You'll do this for however many rounds the host chose (3, 5, 7, or 9). Once you've submitted for the last round, you'll receive a story written by several players. Hopefully, it makes you and everyone else laugh.

## Can you give me a full, exhaustive/exhausting breakdown of the technologies you used to make the app?
I'm so glad you asked. The answer is _yes._ If you don't want to read this, scroll down super far. There's one more question after this.

Let's go through the letters of MERN to outline what the app can do and why.

**M** _(MongoDB/Mongoose)_
Because the Story Game involves several people communicating with each other on different devices, there needs to be a backend. The word "backend" strikes fear in the hearts of many coders teaching themselves. It was definitely frustrating to learn at first, and it took a lot of research to figure out, but it doesn't need to be so scary. MongoDB is learnable, and Mongoose makes it even better. The hardest, most hair-pulling part for me was installing and setting up MongoDB. 

I read once in an article that it's helpful when building a full-stack app to think of it as two separate apps: the frontend and the backend. I found this helpful, so that's how I'll discuss this backend for now.

MongoDB is a backend database. Mongoose creates these things called "models," which are basically shortcuts to creating templates for things you'll make over and over again. For example, I know there will be multiple players I need to store in the backend, so I might as well create a template that makes it easy for players to be created. What I _don't_ need to do is say "Okay, what do I need to create a player. A name ... and I guess a number ... and I guess a unique ID ..." No. I only do that once, when I make the model. Still not making sense? I'll talk a bit more about what models I made below.
* **Player:** The Player model I created is stored within the Game model. The player model has the player's name, avatar and player number. This is different from the player ID, which Mongoose creates automatically. The ID is helpful for confirming a player's identity, while the player number (0 for the host, 1 for the next player, 2 for the next, and so on) is useful for story passing logic.
* **Game:** When a player makes a request for a game to be created, Mongoose sets up a game in the backend using the Game model. This includes a code, rounds, currentRound, gameStarted, game_completed, players, originalPlayers, deletedPlayersNumbers, removablePlayers, storyBeginningsNotReturned, playerDeleted boolean, storiesSubmitted, storiesReturned, storyTexts, and doesGameExist
* **Story:** This was originally created to store inside the Player model, until I realized it made more sense to store in the Game model. Coding is all about being flexible and moving on from an idea you had previously that doesn't make sense anymore (some might call this being agile). You might have to change tack if you plan ahead before jumping into writing code. This is something I still struggle with, partially because banging out the code feels like working, and the planning doesn't (but it is!).

## Can you show me how to make an app?
Yes. Just contact me via email or phone. I love helping people learning to code.
- Email: tomrainswrites@gmail.com
- Phone: (918) 933-9899
