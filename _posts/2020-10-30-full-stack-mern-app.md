---
layout: post
title: Full-Stack MERN App
subtitle: MongoDB, Express, React, and Node join forces to create a titillating party game
comments: true
---

## So what is this app, anyway?
My <a href="https://secret-wildwood-99621.herokuapp.com/" target="_blank">full-stack MERN app</a> is based on an old-school pen and paper activity called the Story Game. This digital version lets you:  
* Create outrageous, hilarious stories with friends
* Play remotely or together (with your faces in your smartphones).  

Grab at least one friend to help you test the app. To learn more about how I built the app, read on.

## How did you get the idea for this app?
I've always loved the pen and paper version of the Story Game. It often has me crying with laughter at the Kafkaesque stories that come out of it. The pen and paper version has its drawbacks, though. Some people don't really like writing with pen and paper, other people have difficult-to-read handwriting, and sometimes no one has pen and paper on them. For these reasons, I decided to make an app version of the game, especially since I knew it would require a backend. I had no idea how to set up a backend but was willing to figure it out.

## What are the rules of the game?
Begin by writing the first 135 to 150 characters of a story. Then, you submit your story. Once all players have submitted, you'll receive a story someone else has written, but you'll only be able to see the last few words they wrote. Continue the thread of the story based on this small amount of context. Once you've submitted for the last round, you receive a story co-written by several players. Hopefully, it makes you and everyone else laugh.

## How did you make the app?
I wanted to build a full-stack app. I already knew and loved React, so I opted for a MERN stack app. First, I designed an overall architecture for the app. Then, I built the front-end React components. Next, I learned how Express and MongoDB and Mongoose tied in to the overall picture, creating a backend. Then came much, much debugging. _**For a more in-depth explanation,**_ check out my long answer below.

## Can you give me a full, exhaustive/exhausting breakdown of the technologies you used to make the app?
I'm so glad you asked. The answer is _yes._ If you don't want to read this, scroll down super far. There's one more question after this.

Let's go through the letters of MERN to outline what the app can do and why.

### M 
**_(MongoDB/Mongoose)_**

#### What are MongoDB and Mongoose?
MongoDB and Mongoose are backend tools. MongoDB is a backend database. Mongoose creates these things called "models," which are basically creating templates for data you'll make over and over again. 

#### Why does this app need a backend?
Because the Story Game involves several people communicating with each other on different devices, there needs to be a backend. The word "backend" strikes fear in the hearts of many self-taught coders, including me circa 2019. It was definitely frustrating to learn at first, and it took a lot of research to figure out, but it doesn't need to be scary. MongoDB is learnable, and Mongoose makes it even better. The hardest, most hair-pulling part for me was installing and setting up MongoDB. 

#### How I used MongoDB and Mongoose
I created 3 different models for the Story Game. Here's a breakdown of each one:
* **Game:** When a player makes a request for a game to be created, Mongoose sets up a game in the backend using the Game model. This includes the following information slots all ready to go: game code, number of rounds, current round, whether the game has started, list of current players, list of all player who joined (including deleted players), deleted player information, which players can be deleted, which stories have been disrupted due to players leaving, whether any players have left, which stories have been submitted for which rounds, which stories have been returned for which rounds, the texts of the stories, and whether or not the game exists (surprise, it always does). _That's a lot of information!_ Mongoose lets me choose the type of input, too, whether it be a boolean, string, array, number, etc.
* **Player:** The Player model I created is stored within the Game model. The player model has the player's name, avatar and player number. This is different from the player ID, which MongoDB creates automatically. The ID is helpful for confirming a player's identity, while the player number (0 for the host, 1 for the next player, 2 for the next, and so on) is useful for story passing logic.
* **Story:** This was originally created to store inside the Player model, until I realized it made more sense to store in the Game model. Coding is all about being flexible and moving on from an idea you had previously that doesn't make sense anymore (some might call this being agile). You might have to change tack more if you don't plan ahead before jumping into writing code. This is something I still struggle with, partially because banging out the code feels like working, and the planning doesn't (even though it is!).

Mongoose creates these useful models, and MongoDB stores all the information. This is key to allowing players to communicate with each other.

### E
**_(Express)_**

#### How does Express work?
 MongoDB and Mongoose help you hold information in the backend. But how do we talk to the backend from the frontend? This is where Express comes in. 
 
#### Extended metaphor time (Is Express MongoDB's secretary?)
Think of it this way: MongoDB is the big guy in the back who has his head down at his desk and doesn't like to be disturbed. For that reason, he has a strict, tough-as-nails secretary you must speak to first. Why do I say she's strict?

Well, this secretary, let's call her Anna, has strict protocol you must adhere to. Want to ask the boss a question? You must follow exact guidelines, or Anna rejects the question. Need the boss to update some data? Follow the strict protocol, or Anna, again, will completely reject your request. She'll tell you why (that's the good part), but the request won't go through to the big guy.

In this app, players can make several different types of requests. The most basic is a GET request, exactly what it sounds like. You are GETTING information from the backend. You don't need the big boss to update anything, you just want to know what's up. I'll talk about these first, because they're the simplest.

When a player joins the waiting room, the frontend is programmed to periodically make GET requests to the backend asking what other players have joined the game. Now, remember when I told you how strict Anna is? If you are trying to make this exact request, you MUST ask it in the following way:
* ``axios.get(`api/players/${this.props.gameId`}/player)`` _(Axios is a tool that helps us make requests)_

What does this mean exactly? It means that if you don't follow the exact formula when requesting this information, you'll be rejected. Or, even worse, you'll get wrong info if Anna thinks you're actually asking for something other than you're what you're asking for. In other words, you must make a GET request to the players route, you must include the gameId (this.props.gameId pulls it from React), and you must include the word "player", in that order. Anna, or in this case Express, first goes to the route "players", then looks for a matching request protocol. Once she finds the matching one, she goes through the code and does precisely what it tells her to do. In this case, she notes the gameID, let's say it's "YOLO," and asks the big boss (Mongo) to look into game YOLO. Then she tells him he needs to loop through YOLO's player array and prepare that in a format the requester can understand. Once the big boss has a list of all the players in the game YOLO, Anna sends the requester a response. If there's an error along the way, she exits from the task completely and sends the requester an error message.

#### A note on Express routes
Anna has a lot of Express paths she has to keep track of. For that reason, she keeps them separated in tidy piles on her desk. One is for requests related to players, another for games, one for stories, and a final one exclusively for when a player joins a game. The point of having these different piles (called routes when they're used in Express code) is a to keep your code clean and find what you need fast. I have as few as one or as many as four request options within each route. This helps me stay organized and find the route I want quickly, but it isn't strictly necessary. But, imagine Anna looking through all the papers just to find one particular type of route requests — it's easier to have 4 piles.

#### Express route in-depth documentation<a name="ExpressDiscussion"></a>
This part is me trying to show off what I made Express do in this app. If you don't care about the level of technical detail (I can't blame you), skip to the next question.

**Games**
* ``axios.get(`api/games/add`)``: When a player visits the home page, a function in componentDidMount automatically creates a new game in the backend with basic info, including the game code and link.
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

### R
**_(React)_**

**add more description here!**

#### React components
* **Components:** Components are the building blocks of React. I outline my major components below.
* **App.js:** This is the most top-level component. It is the parent to five other components: HomeScreen, Join, Waitscreen, WritingPaper, and StoryRevealed. It contains state that is needed in more than one component and several functions, including functions that reset certain states upon reloads, update player and game info, or start the game.
* **HomeScreen.js:** This is the main page of the app. From it, players can create a game. When the component mounts, it creates a game in the backend. The components has functions to update the player's name, avatar, and number of rounds for the game. This information is stored in the backend and frontend. A help button when clicked displays a modal that explains the purpose of the Story Game. If a player has recently played a game and enters this page, some state is reset.
* **Join.js:** The host uses an invite link to get friends to join the game. When a player uses the invite link, it takes them to the Join.js page. The frontend grabs the 4-letter game code from the URL before asking the backend to see if the corresponding game exists. When the page mounts, it makes a call to the backend to make sure the game in the URL exists and hasn't yet started. If it doesn't exist or has already started, it gives an error message and prompts the user to return to the homepage. If the game exists and hasn't started, the player enters their name, selects an avatar, and joins the waiting room. Their info is stored in the backend.
* **WaitScreen.js:** If a player has recently played a game and enters this page, some state is reset. This page makes a periodic call to the backend to see which players are in the game and displays them. Players who joined, like the host, can send a link to friends to have them join. The host can see who has joined before deciding to start the game. This alters some backend information and sends a message to the other players that it's time to get writing! Some dandy code immediately copies the join URL text when a player hits the Copy URL button. When this component unmounts, it stops querying the backend periodically for the players. When the host clicks start and the other players click Begin Writing, React-Router-Dom takes them to the writing portion of the app.
* **WritingPaper.js:** This component has the most complex functionality. Indented bullets time!
  * Each player has a form box with a 150 character limit to write their story. As a player types, state updates so it knows how much a player has left to write. A blue progress bar fills up as it approaches 135 characters (the minimum required). If a player tries to type more than 150 characters, they'll find they're unable to.
  * After the first round, the player is able to see approximately the last 50 characters of a story submitted by another player (The frontend uses code that finds the first full word 50 characters or more from the end of the story and displays all text after that). This gives the player some clue as to what the previous player was writing about, and lets them keep writing the story seamlessly, but there isn't enough context for the story to make sense (that's how you get a bunch of weird stories at the end).
  * If there are 3 or more players, the host sees an active Remove Players button. This is helpful if a player needs to quit or loses Internet connection or their phone dies. It removes them from the game and modifies the backend so that everyone else is able to keep playing seamlessly. If there are 2 players, the host sees the Remove Players button, but it's disabled. This page also has a Help button that opens a modal that explains how to play the game.
  * When the component mounts, a listener function is activated. This is used later to see if everyone else has submitted their story for that round to the backend. 
  * When a player has submitted their story, the frontend makes a call to the backend to see who's still working. If people are still working, the player receives a list of these people. For example, "Jade and Greg are still working on their story."
When everyone but 1 player has submitted their story for the round, the front-end randomly chooses a snarky but lighthearted line to display. For example, "Just waiting on Brad to finish their story. Bless their heart."
  * Once everyone has submitted their story for the round, the frontend makes a call to the backend for the new story. The backend takes into account the round, player ID, and which players have been deleted before passing on a new story to each player. Players receive their stories, and the frontend updates to reflect the new round.
  * The frontend keeps track of what round it is, so it knows when it's the last round. After submitting for the last round, and receiving news that everyone has finished, the frontend makes a call to the backend for the finalStory. The backend takes into account the player number and which players have been deleted (if any) when deciding how to chain the final stories together.
  * Once a player receives their final story (a joint production with several players' contributions changed together), they see a button that says "Read Final Story." In the traditional, non-digital Story Game, players keep their stories turned over until it's their turn to read them aloud. For that reason, I included this button that players have to press before revealing their story.
* **StoryRevealed.js:** This component is simple. Players see their final story so they can read it aloud to everyone. (If you're playing remotely, Zoom is a good option for this). Players have the option to copy their story to save for later and to start a new game.

#### React tools and packages
* **Create-React-App:** I used this handy package to create a React skeleton. I recommend this to anyone new to React who wants an easy setup.
* **React-Router-Dom:** Users easily navigate from page to page thanks to React-Router-Dom.
* **React Developer tools:** This is an incredible tool - I don't know how I could build any React application without it. It's like Google Developer Tools, except you can inspect elements and see the actual React componenents, how they're nested, and their state and props. Absolute must-have for debugging and visualizing your app's structure.
* **React-Simple-Storage:** This simple and wonderful package maintains the state of the page when refreshing or leaving and returning. For state that I didn't want to maintain on a page refresh or when revisiting, I was able to use a special function in componentDidMount.
* **Emoji-Picker-React:** I used this wonderful package to let users choose an emoji to represent them. It uses React Hooks, which were fun to learn about. I had to research them a bit before being able to use this package effectively.

### N
**_(Node)_**

There's not too much to say about Node. It basically lets you run Javascript on your computer outside of a browser. So ... yeah. I promise I'm not just phoning it in because this is the last letter in MERN and I'm tired of writing. Here's some stuff related to Node I used, though:
* **Npm:** I used npm to install packages and run the server. I've typed "npm run dev" more than I can count.
* **Concurrently:** Concurrently lets you run multiple commands at the same time. When I run "npm run dev," this actually sets off two commands: one to get the frontend going, and another to get the backend going (remember earlier I said it helps to think of the frontend and backend as two separate applications - here's another case where this holds true. It takes different things to get the frontend and backend up on your local server).
* **Nodemon:** Nodemon automatically restarts your application when you make changes. A true lifesaver.

### S
**_(Stuff)_**

Okay, there is no S in MERN, but I'm adding and S in MERN and making it MERNS because so many other technologies (STUFF!) helped me in making this app! I'll forget some, but here are my favorites:

* **Postman:** This gem of a program lets you test your Express routes. This is great because you don't need to have your frontend set up to test them. Just build them in the backend, and see if they work. Then you can tie them in to the frontend and test them there. I love Postman.
* **Google:** I've read many a time that one of the top, if not THE top skill a programmer must learn, is how to Google things. I only half-believed this when I started coding, but it's so true. Almost everything is on Google. Part of me wants to believe I'm a special snowflake and my problem is unique, but that's literally never been the case. When you're stuck, head to Google. The more you Google, the better you'll get at it. 
* **Udemy:** When I got stuck and wanted to bang my head against a wall, I went to Udemy for answers. I'm naturally extremely stingy and don't want to spend $12 on a class, even if it's 10 to 40 hours of content. But saving 10 hours of head-banging frustration in exchange for $12 is so worth it. My favorite class was on how to deploy an app to Heroku. This was truly a lifesaver. I might still be trying to figure out how to deploy without it.

## Can you show me how to make an app?
Yes. Just contact me via email or phone. I love helping people learning to code.
- **Email:** tomrainswrites@gmail.com
- **Phone:** (918) 933-9899

## Can I hire you?
Kindly do. Email or text me.
