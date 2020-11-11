---
layout: post
title: Front-End React Game
subtitle: Want to test your geography skills? Kindly try my fun React App
cover-img: /assets/img/stainedglassmap.jpg
thumbnail-img: /assets/img/animalworldmap.jpg
share-img: /assets/img/stainedglassmap.jpg
permalink: /front-end-react-app-experiment
---

My best trivia category is geography, and I love games, so when I was putting my React skills to the test, I decided to make a dynamic geography quiz game. [Test it out it here](https://tomrains.github.io/populations-game/). 

## How does the game work? 
The game makes you guess which country has the larger population. The choices are randomly generated, so you get a new quiz every time. The chance of getting the same quiz twice in a row is approximately 1 in 4.8241784e+22. [Try it out if you like.](https://tomrains.github.io/populations-game/)

## What did you use to make the app?
I used Create-React-App to set up a project template for React. This is the easiest way I've found for beginners to set up a React application. I used React Bootstrap for the buttons. I love how clean React buttons looks, and I feel the simple UI mirrors the straightforward function of the game. I used Atom for my code editor, although now I use Visual Studio Code just because I like the look of it more. I deployed the app to Github Pages following the documentation on Github. I also used the book _React Explained_ by Zac Gordon to help nail down the basic principles of React. The book has a sample project you can build to teach you how React works.

## What challenges did you face making this app?
This project forced me out of my safe vanilla JS in-browser experience and into the wild world of React, which I had read about but didn't have experience tinkering with yet.

### Differentiating state and props
I was confused about the difference between state and props before making this app. Although I had read about the difference and could recite why they were technically different, it took making this web app myself to understand what was really going on. Any component can initiate state within itself, but the moment state is passed down to another component, it becomes props.

### State update issues
I encountered some strange problems along the way, and looking back I'm proud of the ways I fixed them. A big issue was that, when a player progressed to the next question, one of the new answers would briefly remain highlighted green or red from the last round, based on whether the previous answer had been correct or incorrect. State in React can take a while to update (this still strikes me as one of the weakest points of React), so this was causing an issue. I tinkered with Bootstrap and several functions to update state faster, but it just wasn't working. 

To fix this, I decided to have separate components for odd-numbered questions and even-numbered questions. That way, the odd-number component you just used could update in the background while the new even-number component, with refreshed state, would pop up as the next question. This solved the issue of answers being highlighted red or green momentarily. 

### Updating state from a child component
I also had to learn how to update parent state from a child component. This was really hard to wrap my mind around. At the end of the day, you can just pass an update state function down from the parent to the child and call this function when the child needs to update state.

## Do you have a more complex app?
Yes. My [full-stack MERN app](https://tomrains.github.io/full-stack-mern-app/) is the most complex project I have so far.
