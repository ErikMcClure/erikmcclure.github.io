+++
blogimport = true
categories = ["blog"]
comments = [43073140725898600]
date = "2018-01-01T11:37:00Z"
title = "How To Avoid Memorizing Times Tables"
updated = "2018-01-01T11:49:02.000+00:00"
[author]
name = "Erik McClure"
uri = "https://plus.google.com/104896885003230920472"

+++
I was recently told that my niece was trying to memorize her times tables. As an applied mathematician whose coding involves plenty of multiplication, I was not happy to hear this. Nobody who does math actually memorizes times tables, and furthermore, forcing a child to memorize *anything* is probably the worst possible thing you can do in modern society. No one should *memorize* their times tables, they should learn how to *calculate them*. Forcing children to memorize useless equations for no reason is a great way to either ensure they hate math, teach them they should blindly memorize and believe anything adults tell them, or both. So for any parents who wish to teach their children how to be critical thinkers and give them an advantage on their next math test, I am going to describe how to derive the entire times tables with only 12 rules.

 1. Anything multiplied by 1 is itself. Note that I said *anything*, that includes fractions, pies, cars, the moon, or anything else you can think of. Multiplying it by 1 just gives you back the same result.
 
 1. Any number multiplied by 10 has a zero added on the end. 1 becomes 10, 2 becomes 20, 72 becomes 720, 9999 becomes 99990, etc.
 
 1. Any single digit multiplied by 11 simply adds itself on the end instead of 0. 1 becomes 11, 2 becomes 22, 5 becomes 55, etc. This is because you never need to multiply something by eleven. Instead, multiply it by 10 (add a zero to it) then add itself. {{<bmath>}} \begin{aligned} 11*11 = 11*(10 + 1) = 11*10 + 11 = 110 + 11 = 121\\ 12*11 = 12*(10 + 1) = 12*10 + 12 = 120 + 12 = 132 \end{aligned} {{</bmath>}}
 1. You can always reverse the numbers being multiplied and the same result comes out. {{<math>}}12*2 = 2*12{{</math>}}, {{<math>}}8*7 = 7*8{{</math>}}, etc. This is a simple rule, but it's very easy to forget, so keep it in mind.
 
 1. Anything multiplied by 2 is doubled, or added to itself, but you only need to do this up to 9. For example, {{<math>}}4*2 = 4 + 4 = 8{{</math>}}. Alternatively, you can count up by 2 that many times: {{<bmath>}}4*2 = 2 + 2 + 2 + 2 = 4 + 2 + 2 = 6 + 2 = 8{{</bmath>}} To multiply any large number by two, double each individual digit and *carry the result*. Because you multiply each digit by 2 separately, the highest result you can get from this is 18, so you will only ever carry a 1, just like in addition. {{<bmath>}} \begin{aligned} \begin{matrix} 3 & 6\\ & 2\\ \hline & \\ & \\ \hline &  \end{matrix}\quad \begin{matrix} 3 & 6\\ & 2\\ \hline 1 & 2\\ & \\ \hline &  \end{matrix}\quad \begin{matrix} 3 & 6\\ & 2\\ \hline 1 & 2\\ 6 & \\ \hline &  \end{matrix}\quad \begin{matrix} 3 & 6\\ & 2\\ \hline 1 & 2\\ 6 & \\ \hline 7 & 2 \end{matrix} \end{aligned} {{</bmath>}} This method is why multiplying anything by 2 is one of the easiest operations in math, and as a result the rest of our times table rules are going to rely heavily on it. Don't worry about memorizing these results - you'll memorize them whether you want to or not simply because of how often you use them.
 1. Any number multiplied by 3 is multiplied by 2 and then added to itself. For example: {{<bmath>}}6*3 = 6*(2 + 1) = 6*2 + 6 = 12 + 6 = 18{{</bmath>}}Alternatively, you can add the number to itself 3 times: {{<math>}}3*3 = 3 + 3 + 3 = 6 + 3 = 9{{</math>}}
 1. Any number multiplied by 4 is simply multiplied by 2 twice. For example: {{<math>}}7*4 = 7*2*2 = 14*2 = 28{{</math>}}
 1. Any number multiplied by 5 is the same number multiplied by 4 and then added to itself. {{<bmath>}}6*5 = 6*(4 + 1) = 6*4 + 6 = 6*2*2 + 6 = 12*2 + 6 = 24 + 6 = 30{{</bmath>}} Note that I used our rule for 4 here to break it up and calculate it using only 2. Once kids learn division, they will notice that it is often easier to calculate 5 by multiplying by 10 and halving the result, but we assume no knowledge of division.
 1. Any number multiplied by 8 is multiplied by 4 and then by 2, which means it's actually just multiplied by 2 three times. For example: {{<math>}}7*8 = 7*4*2 = 7*2*2*2 = 14*2*2 = 28*2 = 56{{</math>}}
 1. Never multiply anything by 12. Instead, multiply it by 10, then add itself multiplied by 2. For example: {{<math>}}12*12 = 12*(10 + 2) = 12*10 + 12*2 = 120 + 24 = 144{{</math>}}
 1. Multiplying any single digit number by 9 results in a number whose digits always add up to nine, and whose digits decrease in the right column while increasing in the left column. {{<bmath>}} \begin{aligned} 9 * 1 = 09\\ 9 * 2 = 18\\ 9 * 3 = 27\\ 9 * 4 = 36\\ 9 * 5 = 45\\ 9 * 6 = 54\\ 9 * 7 = 63\\ 9 * 8 = 72\\ 9 * 9 = 81 \end{aligned} {{</bmath>}}10, 11, and 12 can be calculated using rules for those numbers.
 1. For both 6 and 7, we already have rules for all the other numbers, so you just need to memorize 3 results: {{<bmath>}} \begin{aligned} 6*6 = 36\\ 6*7 = 42\\ 7*7 = 49 \end{aligned}{{</bmath>}}Note that {{<math>}}7*6 = 6*7 = 42{{</math>}}. This is where people often forget about being able to reverse the numbers. Every single other multiplication involving 7 or 6 can be calculated using a rule for another number.
 
And there you have it. Instead of trying to memorize a bunch of numbers, kids can learn *rules* that build on top of each other, each taking advantage of the rules established before it. It's much more engaging then trying to memorize a giant table of meaningless numbers, a task that's so mind-numbingly boring I can't imagine forcing an adult to do it, let alone a small child. More importantly, this task teaches you what math is *really* about. It's not about numbers, or adding things together, or memorizing a bunch of formulas. It's establishing simple rules, and then combining those rules together into more complex rules you can use to solve more complex problems.  

This also establishes a fundamental connection to computer science that is often glossed over. Both math and programming are **repeated abstraction and generalization**. It's about combining simple rules into a more generalized rule, which can then be abstracted into a simpler form and combined to create even more complex rules. Programs start with machine instructions, while math starts with propositions. Programs have functions, and math has theorems. Both build on top of previous results to create more powerful and expressive tools. Both require a spark of creativity to recognize similarities between seemingly unrelated concepts and unite them in a more generalized framework.  

We can demonstrate all of this simply by refusing to memorize our times tables.
