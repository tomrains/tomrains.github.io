---
layout: post
title: Front-End React Game
subtitle: Want to test your geography skills? Kindly try my fun React App
cover-img: /assets/img/stainedglassmap.jpg
thumbnail-img: /assets/img/animalworldmap.jpg
share-img: /assets/img/path.jpg
tags: [books, test]
---

My best trivia category is geography, and I love games, so when I was putting my React skills to the test, I decided to make a dynamic geography quiz game. [Play with it here](https://tomrains.github.io/populations-game/). 

## How does the game work? 
The game makes you guess which country has the larger population. The choices are randomly generated, so you get a new quiz every time. (The chances of getting the same quiz is (insert math here).

## What did you use to make the app?
create-react-app
look at package.json to see what i got
atom for code editor, though now i use visual code studio

## What challenges did you face making this app?
This app forced me out of my safe vanilla JS in-browser experience and step into the wild world of React, which I had read about but didn't have experience tinkering with yet.

Making this app helped me wrap my head around React state and props. I encountered some strange problems along the way, and looking back I'm proud of the novel (if unconventional) ways I fixed them. A big one was that, when a player progressed to the next question, one of the answers would briefly remain highlighted green or red, based on whether the previous answer had been correct or incorrect.

I was confused about the difference between state and props before making this app. Although I had read about the difference and could recite why they were technically different, it took making this web app myself to understand what was really going on. Any component can initiate state within itself, but the moment state is passed down to another component, it becomes props.

I also had to learn how to update parent state from a child component. This was really hard to wrap my mind around. At the end of the day, you can just pass an update state function down from the parent to the child and call this function when the child needs to update state.

## Do you have any more complex apps?
Yes. My [full-stack MERN app](https://tomrains.github.io/2020-10-31-full-stack-mern-app/) is the most complex project I have so far.
