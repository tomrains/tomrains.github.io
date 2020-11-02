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

### M 
**_(MongoDB/Mongoose)_**

Because the Story Game involves several people communicating with each other on different devices, there needs to be a backend. The word "backend" strikes fear in the hearts of many coders teaching themselves. It was definitely frustrating to learn at first, and it took a lot of research to figure out, but it doesn't need to be so scary. MongoDB is learnable, and Mongoose makes it even better. The hardest, most hair-pulling part for me was installing and setting up MongoDB. 

I read once in an article that it's helpful when building a full-stack app to think of it as two separate apps: the frontend and the backend. I found this helpful, so that's how I'll discuss this backend for now.

MongoDB is a backend database. Mongoose creates these things called "models," which are basically shortcuts to creating templates for things you'll make over and over again. For example, I know there will be multiple players I need to store in the backend, so I might as well create a template that makes it easy for players to be created. What I _don't_ need to do is say "Okay, what do I need to create a player. A name ... and I guess a number ... and I guess a unique ID ..." No. I only do that once, when I make the model. Still not making sense? I'll talk a bit more about what models I made below.
* **Player:** The Player model I created is stored within the Game model. The player model has the player's name, avatar and player number. This is different from the player ID, which Mongoose creates automatically. The ID is helpful for confirming a player's identity, while the player number (0 for the host, 1 for the next player, 2 for the next, and so on) is useful for story passing logic.
* **Game:** When a player makes a request for a game to be created, Mongoose sets up a game in the backend using the Game model. This includes a code, rounds, currentRound, gameStarted, game_completed, players, originalPlayers, deletedPlayersNumbers, removablePlayers, storyBeginningsNotReturned, playerDeleted boolean, storiesSubmitted, storiesReturned, storyTexts, and doesGameExist
* **Story:** This was originally created to store inside the Player model, until I realized it made more sense to store in the Game model. Coding is all about being flexible and moving on from an idea you had previously that doesn't make sense anymore (some might call this being agile). You might have to change tack if you plan ahead before jumping into writing code. This is something I still struggle with, partially because banging out the code feels like working, and the planning doesn't (but it is!).

### E
**_(Express)_**

Note: If you literally do not care about how Express works and don't want to hear me explain it, and you just want to know what Express did for me in this app, scroll down to In-depth discussion of what each Express route in the game has/does. **(link this later)**

Okay, so MongoDB and Mongoose help you hold information in the backend. But how do we talk to the backend from the frontend? This is where Express comes in. I like to think of Mongo as the big guy in the back who has his head down at his desk and doesn't like to be disturbed. He doesn't want just anyone sauntering into his office and making a request. For that reason, he has an extremely strict secretary you must speak to first. Why do I say she's strict?

Well, this secretary, let's call her Anna, she has strict protocol you must adhere to. Want to ask the boss a question? It must follow exact guidelines, or Anna rejects it. Need the boss to update some data? Follow the strict protocol, or Anna, again, will completely reject your request. She'll tell you why (that's the good part), but the request won't go through to the big guy.

Okay, enough metaphor. Let's talk about how Express actually works in this app. From the frontend (the players on their phones), several different types of requests can be made. The first is a GET request, exactly what it sounds like. You are GETTING information from the backend. You don't need the big boss to update anything, you just won't to know what's up. I'll talk about these first, because they're the simplest.

When a player joins the waiting room, the frontend is programmed to periodically make GET requests to the backend asking what other players have joined the game. Now, remember when I told you how strict Anna is? If you are trying to make this exact request, you MUST ask it in the following way:
* axios.get(`api/players/${this.props.gameId}/player`) (_Axios is a tool that helps us make requests_)

What does this mean exactly? It means that if you don't include the exact formula when requesting this information, you'll be rejected. Or, even worse, you'll get wrong info if Anna thinks you're actually asking for something other than you're asking. In other words, you must be requesting from the players route, and including the gameId and the word "player", in that order. Anna, or in this case Express, first goes to the route "players", then looks for a matching request protocol. Once she finds the matching one listed above, goes through the code and does precisely what it tells her to do. In this case, she notes the gameID, let's say it's "YOLO," and asks the big boss (Mongo) to look into game YOLO. Then she tells him he needs to loop through YOLO's player array and prepare that into a format the requester can understand. Once the big boss has a list of all the players in the game YOLO, Anna sends the requester a response back. If there's an error along the way, she exits from the tasks completely and sends the requester an error message.

#### A note on routes
Anna has a lot of Express paths she has to keep track of. For that reason, she has them separate into tidy piles on her desks. One is for requests related to players, another for games, one for stories, and a final one exclusively for when a player joins a game. If you haven't realized, Anna is really just me. The point of having these different piles (called routes when they're used in Express code) is a way to keep your code clean and find what you need fast. I have as few as one or as many as four request options within each route. This just helps me stay organized and find the route I want quickly - it isn't strictly necessary. But imagine Anna looking through all the papers just to find one particular type of route requests - it's easier to have 4 piles.

#### In-depth discussion of what each Express route in the game has/does
This part is me trying to show off what I made Express do. If you don't care about the level of technical detail, skip to the next question.
**Games**
* When a player visits the home page, a function in componentDidMount automatically creates a new game in the backend with basic info, including the game code and link.
* The host can update the number of rounds in a game.
* The host starts the game. This creates arrays in the backend that track who has submitted, who hasn't, and what the submitted stories are. This also lets the backend know the game has started and to not accept any new players.

**Players**
* Adds host information and additional game information to the game. This also returns a unique ID to the host, which the host uses for the rest of the game when submitting stories.
* Adds a non-host player to the game. Non-hosts are added to an array of removablePlayers (hosts are able to remove players if they bail mid-game so the game can keep running). Non-host players are also given a unique ID to submit stories later on.
* Fetch all players that have signed up for a game. This is called every 3 seconds in the Waiting Room to see who's joined. This is especially helpful for the host when deciding it's time to start the game.
* Delete a  player from the game. Only the host may do this. This is helpful if a player has to bail mid-game. This functionality finds the player's ID in the backend and deletes them from the players array (their information is still maintained in an array called originalPlayers). The story the player was working on is noted, because this disrupts the story passing. The player's stories for the current and future rounds are changed to "PLAYER BAILED," and the backend looks for this when determining story passing logic and when sending the final compiled stories out. It also marks all story returns for the player as "true" so that the backend doesn't wait on that player to submit a story anymore. The player's ID is also stored in the backend in a list of deleted players. Once there are 2 players left in the game, the host isn't allowed to delete anyone.

**Stories**
* Player submits their story lines for the round. These are stored in the backend. If 2 players submit simultaneously and one is unable to save due to version errors, that player receives an error and tries again
* After submitting their story, the player's front-end checks every few seconds to see if everyone else has finished. If people are still working, the backend sends information on who is still working. If they are done, then the front end makes a request for a new story from the back end.
* The player makes a request for a new story from the backend. This takes into account the round and if players have been deleted and how this affects story passing.
* After the last round is complete, the player makes a final request for a full story, which consists of several story lines, written by different people, chained together to make one final bizarre, comedic masterpiece. If players have been deleted, the backend logic makes sure everyone still gets back a full, completed story

**Join**
* Player attempts to join the game. If the game doesn't exist or it's already mid-game, the player is given an error message and can return to the homepage. If successful, the player receives relevant game info to store in the front end and is directed to the Waiting Room.

## Can you show me how to make an app?
Yes. Just contact me via email or phone. I love helping people learning to code.
- Email: tomrainswrites@gmail.com
- Phone: (918) 933-9899
