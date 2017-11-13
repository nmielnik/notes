# Difficult Technical Conversations

Difficult Conversations is an outstanding book which is pretty much required reading for most people I work with as soon as they have an encounter with a teammate or other coworker that doesn't go well.  Though the book teaches all kinds of super valuable perspectives and strategies, it can be difficult to apply these learnings in the workplace, especially as a software engineer running into various conflicts around engineering excellence and practices all while working for a company that is ultimately paying you to build software to allow them to make money delivering value to customers.

I'm not sure if this will end up truly being a "Difficult Conversations" for software engineering, but I want to share some of my observations, thoughts, and suggestions and see where that ends up.

## Guide to Disagreeing

Every single engineer who is passionate about software and has worked on a team has been there: Simon has proposed a code change to the project that is unacceptable and I have to do something about it now before it causes significan problems.  Some examples of why you - a self-respecting engineer who takes pride in you work  - are obligated to take action:

* I just checked in code a few days ago that Simon should be reusing to avoid duplication.  Clearly, despite approving those changes, he just wasn't paying attention to the valuable work I did and once I point this out to him it will be obvious that he should be reusing my code.
* Despite there being clear consensus on using tabs instead of spaces, a space between the function name and the open parentheses, and many other linting rules, Simon is just ignoring them, maybe even in a malicious way!
* There is this awesome FunJS package that I just gave a brown bag about last week that Simon didn't attend.  FunJS contains functionality that Simon should use instead of trying to write this new code himself.  Clearly, an open source library used by 5 people and written by a single author without any unit tests is going to be more well-written and well-supported than anything Simon could have created.  Why reinvent the wheel?  Writing our own implementation instead of using this external framework will bloat our codebase unnecessarily.
* I know someone gave a brown bag on that new utility last week, but that library has way too much code in it and all we need is this one simple function so we should just write it ourselves...in fact, I already have code from another repo I can give Simon to copy and paste and then we can remove the dependency and not start including external frameworks that will bloat our codebase unnecessarily.
* I don't see how this possibly could work. Did Simon even test this?
* I know this entire repo is in Java, but this new class Simon is adding is simply the final straw.  I've seen at least one other file in this repo where if it was in Javascript there's a pretty good chance the file would be at least 3 lines shorter.  Simon, it's time to open your eyes to the wonderful world of Javascript and I'm willing to sacrifice my time and effort to be your guide to this world.  Why fix this one bug when you can rewrite the entire repo and introduce 50 new ones?
* I can't understand these changes in under 30 seconds so I feel safe in assuming that it's too complicated however, Simon is in luck. It just-so-happens that I have a super clever idea for refactoring this class to make it way simpler and easy to understand.  Clearly Simon won't mind if I take an hour of his time explaining why his idea is too complicated and why my refactored approach is objectively better.
* Look at this class hierarchy. Wasn't Simon paying attention when I expounded at length on the benefits of function composition over class inheritance? He really needs to start using functors and applicative monads to truly make his code understandable and elegant. It's like he hasn't even read Lambda the Ultimate.

We as intelligent engineers clearly have some strong opinions.  We love our job, we're proud of what we do, and so we want everything we produce and our teams produce to be the highest quality possible.

### Scoring

In order to properly convey some of the important things to consider, I'm using a scoring system where you get positive and negative points based on certain actions you take.  The scores are pretty arbitrary, but they create a way to compare positive and negative outcomes in a way that's easier to understand than trying to use varying levels of fancy adjectives (ie immensely helpful vs drastically hurtful)

### Point Types

1. Business Points: You're helping the business achieve its goals of making money by delivering value to customers
2. Teammate Points: You're someone your teammates respect and enjoy working with making work an enjoyable experience for everyone

### Global Scoring
* +25 TP: Compliment your teammate
  * At the end of every decision tree, let your teammate know you enjoyed the discussion, appreciate their time, and respect their intelligence and decision making ability...IT'S FREE POINTS, JUST DO IT!!
* -50 TP | -25 BP: Let your teammate know you disagree
  * The moment the other person realizes that you disagree, regardless of whether you make a minor suggestion or flat out tell them you disagree, it's a negative experience.  It is now your responsibility to do something to turn this back into a positive experience.  If you don't do that, you will have had a negative impact on the company and a negative impact on your relationship with your coworker.  Whether or not you are "right" doesn't matter in this case because enough negative can happen where arriving at a different solution which might be better for the project ultimately doesn't outweigh the damage done, which can have a long lasting ripple effect and hurt the project in many unintentional ways.
  * Multiply this by the number of people who must now know that you disagree, it impacts all of them
* -1 TP | -2 BP
  * Every 5 minutes spent discussing a decision after your teammate(s) realize that you disagree
  * Multiply this by 2 to the n power, where n is the number of people you are discussing this with
    * ie: If you are discussing the issues / alternative solution with 3 people, that's 2 ^ 3 = 8 as the multiplier.  That means every 5 minutes spent discussing this with 3 people is -8 TP and -16 BP.  A 30 minute discussion / meeting with those 3 people = -240 TP + -480 BP

1) You see a technical decision, do you agree?
  * Yes: +50 BP (You double checked the solution to ensure we're doing the smartest thing we can as a business
    * BONUS +25 TP: Compliment your teammate!
  * No -> Goto #2

2) The most likely cause for disagreement is that someone doesn't have all the information. Keeping mind that the moment you communicate your disagreement, it's -75 points and the penalty clock starts ticking, so put this off as long as possible.
  * Go and find the person and talk to them (in person or video chat is better than hipchat.  Don't do this in github).  Ask them about what their goal with the change was, what kind of issues they ran into, what they know about the project, the codebase, or other factors.
    * First 30 minutes without showing disagreement: +25 BP | +25 TP
      * Giving someone a chance to explain what they did and why they did it can be a good thing if they think you're just curious, people like explaining the work they've done and are proud they got it working, so talking more about it to others is a good feeling...you can quickly make it a bad feeling if you tell them they did the wrong thing though so keep that in mind.
    * A: The person who made the change realizes that they misunderstood something, forgot something, or there is some other issue which leads them to ultimately make the change you wanted without you having to disagree: +1000 BP | +250 TP (Best case scenario!)
      * You helped spot an issue but didn't cause a disagreement.  The business benefits a ton, and your relationship with the person has gotten that much better.  The other person might reach out to you specifically in the future for discussions about this stuff.
      * Bonus points for complimenting their thought process and their work so far!
    * B: You realize there was an important piece of info you didn't know about, or they actually tried your approach originally and there was an unforseen problem that you didn't know.  You realize you are now fine with what they've done, no need to disagree. +25 BP | + 25 TP
      * You've double checked that they made the right decision, which is good.  You've also learned something which will help you in the future.
      * Bonus points for compliment. Extra bonus for telling them you originally were going to suggest something but realized they actually made an even better decision and you think they're smart. (+150 TP)
    * C: If it's 30 minutes later and it's clear you have all the same info and they still chose a different solution than you. Thank them for their time and walk away. 

3) Walk away from the conversation, and on your own look through the code again or think about the problem some more.  Ask for someone else's opinion on the matter, make sure what you think should change is actually meaningful.  Talking to someone else outside of the problem can be really telling of how important the topic is and how your feedback could be perceived.
      * A: After thinking about it or talking with someone else, you realize something else you didn't think about, or you've convinced yourself it's not worth the effort in changing: +5 BP
        * It's still somewhat valuable to make sure we do things well, but don't spend your whole day worrying whether a change that someone else made that is actually working.
      * B: You still feel like it's critical to the business that this be done differently. You know it's not about you or the other person but it's about the value we're giving to our customers and/or our ability to deliver value in the future.  If this is all true, then come back to the person and let them know you have a thought you'd like to get their feedback on (again, it's critical to do this in person or over videochat, hipchat isn't great for this and github comments are far worse)  [Take on the 'disagreement' penalty: -50 TP + -50 BP]
