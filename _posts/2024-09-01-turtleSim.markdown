---
layout: post
title:  "A Python project to help balance a boardgame"
date:   2024-09-01 09:40:00 +0200
categories: python TDD
---
A while ago a few friends were doing a playtest of a boardgame called Greenwashing. They've yet to make a website but when they do I'll add a link to it. 

It's a bluffing game played with cards. During playtesting a bunch of us felt the most dominating strategy with the rules as written then was to play defensively and turtle up. 

The basic idea of the game is that you play cards of various values that either increase your greenwashing score (these are open information to everyone) or play cards facedown to increase your industry score (these are hidden). Players can call you out that your industry is higher than your greenwashing and you will lose some points and they will gain. If they call you out but you didn't have a too high industry, they lose and you gain. 

After the playtest was over, I felt that this simplest strategy could easily by simulated so they could see what kind of score a non interactive strategy produces.

I started writing the project with the aim of getting a histogram of scores for this strategy but also to get some experience doing test driven development. I chose python because one of the friends developing the game knows python. The project was going to be a library and an example script on how to use it.

The entire project repository including commits is uploaded on [github][github-greenwashturtle]

I first wrote the test for an opening play of the game. The game would be a solo player drawing and playing cards. I had to decide on the interface to access the library functions before writing any of the actual code. I felt the easiest way was to have two objects to track the game state and the players hand.

{% highlight python %}
TestGame = GameState()
PlayerHand = TestGame.Hand
{% endhighlight %}

As the player's hand would hold cards that are more than just a numeric value I added cards as another object type `PlayerHand.Draw(Card(0))`. I wrote enough code for the test to compile, that is I added classes for `GameState`, `Hand` and `Card`. After getting the test to compile and fail, I wrote methods to pass them. At the end of this, the test would draw some predetermined cards and play the correct cards for the strategy I was aiming to emulate.

The next step was adding a script where a deck of cards is defined and a game is played. The input for the deck is a dictionary where each card value is assigned how many of that card is in the deck.

{% highlight python %}
twoPlayerDeck = {
    #GreenwashValue:Quantity of cards
    0:2,
    1:2,
    2:4,
    3:6,
    4:4,
    5:2
}
{% endhighlight %}

I wrote some additional functions in the library. At this point I skipped the tests as I wanted to get some results ASAP and I thought figuring out how to do non deterministic tests would have taken more time.

After doing that, all I had to do was to run a bunch of these games, noting the result of each and putting them all in a histogram and plotting the results.

![Histogram of the game results]({{ site.baseurl }}/assets/images/turtleResult1.png)

This tool ended up being useful during the development of the game as they could now iterate on reducing the end game score for this strategy without actually playing a bunch of games. In the end they only needed to change the rule concerning the values of your greenwash and industry score. Instead of greenwash >= industry it was changed to greenwash > industry. Checking the expected value for turtling under this new rule, the histogram is visible below. 

![Histogram of the game results after rule change]({{ site.baseurl }}/assets/images/turtleResult2.png)


[github-greenwashturtle]: https://github.com/jutranjo/Greenwash_Turtle_Sim