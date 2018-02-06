+++
blogimport = true
categories = ["blog"]
comments = [8152926649784086811, 211157025285730602, 2821259584975881087]
date = "2015-05-30T10:36:00Z"
title = "Using Data To Balance Your Game: Pony Clicker Analysis"
updated = "2015-10-08T23:21:38.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
{{%blockquote%}}*The only thing more addicting than heroine are numbers that keep getting larger.*{{%/blockquote%}}

Incrementer and idle games are seemingly simplistic games where you wait or click to increase a counter, then use that counter to buy things to make the counter go up faster. Because of the compounding effects involved, these types of games inevitably turn into a study of growth rates and how different functions interact. [Cookie Clicker](http://orteil.dashnet.org/cookieclicker/) is perhaps the most well-known, which employs an exponential growth curve for the costs of buildings that looks like this:

{{<bmath>}} Cost_n = Cost_0\cdot 1.15^n {{</bmath>}}

Where {{<math>}}Cost_0{{</math>}} is the initial cost of the building. Each building, however, has a fixed income, and so the entire game is literally the player trying to purchase upgrades and buildings to fight against an interminable exponential growth curve of the cost function. Almost every single feature added to Cookie Clicker is yet another way to battle the growth rate of the exponential function, delaying the plateauing of the CPS as long as possible. This includes the reset functionality, which grants heavenly chips that yield large CPS bonuses. However, no feature can compensate for the fact that the buildings do not have a sufficient growth rate to keep up with the exponential cost function, so you inevitably wind up in a dead end where it becomes almost impossible to buy anything in a reasonable amount of time regardless of player action.

Pony Clicker is based off Cookie Clicker, but takes a different approach. Instead of having set rates for each building, each building generates a number of smiles based on the number of ponies and friendships that you have, along with other buildings that "synergize" with that building. The more expensive buildings generate more smiles because they have a higher growth rate than the buildings below them. This makes the game extremely difficult to balance, because you only have equations and the cost curves to work with, instead of simply being able to set the per-building SPS. Furthermore, the SPS of a building continues to grow and change over the course of the game, further complicating the balance equation. Unfortunately, in the first version of the game, the growth rate of the end building exceeded the growth rate of the cost function, which resulted in immense end-game instability and all around unhappiness. To address balance problems in pony clicker, rather than simply throwing ideas at the wall and trying to play test them infinitely, I wrote a program that played the game for me. It uses a nearly optimal strategy of buying whatever the most efficient building is in terms of cost per +1 SPS increase. This is not a perfectly optimal strategy, which has to take into account how long the next building will need to take, but it was pretty close to how players tended to play.

Using this, I could analyze a game of pony clicker in terms of what the SPS looked like over time. My first graph was not very promising:

{{<img src="https://cloud.githubusercontent.com/assets/3387013/7786493/21b10990-0188-11e5-86de-7eaa7d5e07d5.png" width="700px" >}}

The SPS completely exploded and it was obviously terrible. To help me figure out what was going on, I included a graph of the optimal store purchases and the time until the next optimal purchase. My goal in terms of game experience was that no building would be left behind, and that there shouldn't be enormous gaps between purchases. I also wanted to make sure that the late game or the early game didn't take too long to get through.

{{<img src="https://cloud.githubusercontent.com/assets/3387013/7786492/1ccd4ac4-0188-11e5-840b-d3a47f4a898a.png" width="700px" >}}

In addition to this, I created a graph of the estimate SPS generation of each individual building, on a per-friendship basis. This helped compensate for the fact that the SPS changed as the game state itself changed, allowing me to ensure the SPS generation of any one building wasn't drastically out of whack with the others, and that it increased on a roughly linear scale.

{{<img src="https://cloud.githubusercontent.com/assets/3387013/7787681/f5c01296-01cc-11e5-829e-a0e9e1f89334.png" width="700px" >}}

This information was used to balance the game into a much more sane curve:

{{<img src="https://cloud.githubusercontent.com/assets/3387013/7899795/3bb8210c-06e7-11e5-9514-1b66669631dc.png" width="700px">}}

I then added upgrades to the main graph, and quickly learned that I was giving the player certain upgrades way too fast:

{{<img src="https://cloud.githubusercontent.com/assets/3387013/7806412/491fa6cc-0335-11e5-8e2e-6e6903d94240.png" width="700px">}}

This was used to balance the upgrades and ensure they only gave a significant SPS increase when it was really needed (between expensive buildings, usually). The analysis page itself is [available here](https://blackhole12.github.io/PonyClicker/utilities/analysis.html), so you can look at the current state of pony clicker's growth curve.

These graphs might not be perfect, but they are incredibly helpful when you are trying to eliminate exponential explosions. If you got a value that spirals out of control, a graph will tell you *immediately*. It makes it very easy to quickly balance purchase prices, because you can adjust the prices and see how this affects the optimal gameplay curve. Pony Clicker had so many interacting equations it was basically the *only* way i could come up with a game that was even remotely balanced (although it still needs some work). It's a great example of how important **rapid feedback** is when designing something. If you can get immediate feedback on what changing something does, it makes the process of refining something much faster, which lets you do a better job of refining it. It also lets you experiment with things that would otherwise be way too complex to balance by hand.
