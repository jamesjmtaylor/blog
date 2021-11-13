---
title: HackerRank Game Theory
date: '2021-11-13T07:34:13-08:00'
---
<img style="float: left; margin:0 1em 0 0; width: 25%" src="/img/blog/nash.png"/>

I've used HackerRank throughout my career, both when I've been screened as a candidate, as well as to screen my own candidates.  I've also used it to study algorithms and data structures.  It's a great platform for practicing and evaluating the fundamentals of Computer Science.  In the process of skills evaluation the candidate must check a box stating 

> "I will not consult/copy code from any source including a website, book, or friend/colleague to complete these tests, though may reference language documentation or use an IDE that has code completion features."

Having studied John Nash's work I'm naturally curious as to what the  Game Theory analysis of this simple statement is.  So I decided to apply what I had learned while reading [Game Theory 101: The Complete Textbook](http://gametheory101.com/) (an excellent introduction to the field if you're interested) to the problem of competing candidates.  

Before we get into it, a quick primer.  Game theory is "the study of strategic interdependence."  In other words, the objective analysis of the decisions made by two or more parties and how those decisions impact the parties involved.  One of the ultimate goals of Game Theory is to determine the "Nash Equilibrium" of any given problem. The Nash Equilibrium is the set of strategies that maximizes each player's outcome, regardless of the choices of the other players.  When most people think of Game Theory they think of the Prisoner's Dilemma.  Prisoner's Dilemma is the oldest and simplest game theory problem.  Two prisoners must simultaneously decide whether to confess or keep quiet.  The "Nash Equilibrium" is that both players confess, even though this means an objectively worse outcome than if both players kept quiet. 

The technical interview process is more akin to a Game Theory "Game Tree".  This is because there are multiple sequential stages.  Each path in the tree terminates at a discrete outcome.  For the sake of keeping the tree from getting too large or complex we'll define a number of premises:

1. There are two competing candidates.
2. There is only one job opening, so at most one candidate can get the job.  It is also possible neither candidate gets the job.
3. Both candidates are of equal starting skill.
4. There are two interview stages, a HackerRank digital challenge and an in-person whiteboard challenge
5. The time invested studying for the whiteboard interview only imperfectly translates to skill in the digital challenge.
6. The time invested studying for the digital challenge only imperfectly translates to skill in the whiteboard challenge.
7. You cannot pass the digital challenge without either studying or cheating.
8. You have even odds of being hired as a candidate if both candidates study for the digital challenge since the candidates are otherwise indistinguishable to the interviewers.
9. If you study for the whiteboard and the other candidate does not you will be hired over them.
10. Both candidates are sufficiently skilled that their cheating would not be detected and sufficiently amoral that they would experience no guilt for having cheated.

https://workplace.stackexchange.com/questions/150857/not-allowed-to-use-google-during-programming-test





_Image Credit: MIT Museum_
