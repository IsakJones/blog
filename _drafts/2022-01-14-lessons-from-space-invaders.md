---
layout: post
title:  "Lessons from making Space Invaders"
date:   2022-01-27 18:20:49 +0100
categories: programming 
---

During my application process to the Recurse Center, I made my own version of Space Invaders in Python. I originally chose the project because I thought it would be a simple 

## Not everything has to be an object

When I wrote Space Invaders, I thought that rigorous object-oriented design was essential to good programming. Just one or two months before I learned to program in Java, and while I was first frustrated with its baroque object-oriented features, I quickly grew to like them. After mostly coding in Python, I got strangely comfortable with Java's rigid structure, where code lies in packages and each file necessarily defines an object. I got so comfortable with it, in fact, that I started thinking that all code should be organized like that.

So that's how I organized my Space Invaders game. I organized my code in a Python module and had each file describe a class. I of course made a class for the space invaders, and a class for the player-controlled spaceship, and both of them inherited from the abstract class "creature". 

 including the control flow of the game itself, in a class called Game. 

The gratest insight I gathered was on object-orientation. I gave the project a rigorous java-like structure, with each file including a class that interacted with others only through dedicated methods. The results were overall mixed - while it makes a lot of sense to make a class for the player-controlled spaceship and a separate one for the enemy invaders, I think that dividing other aspects of the game logic in objects is less convenient and can be clunky. I wrote a blog post on the topic.

## ORMs can be convenient

This project also gave me the opportunity to interface with a database through an ORM, which was a very different experience than with raw SQL queries, such as in my other project polka. While I think I still prefer the degree of control and performance of directly using SQL, I also have to admit that ORMs definitely make it much easier to write simple code that can easily be changed.

Lastly, the project introduced me to a lot of the challenges involved in game design, and gave me a whole new appeciation for videogames and the effort that lies behind them!
