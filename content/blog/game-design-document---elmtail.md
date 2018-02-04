+++
title = "Game Design Document - Elmtail"
date = 2012-10-09T18:25:00Z
updated = 2012-10-16T16:46:45Z
draft = true
blogimport = true 
categories = [ "blog" ]
[author]
	name = "Erik McClure"
	uri = "https://plus.google.com/104896885003230920472"
+++

Copyright ©2008 Erik McClure

<div style="color: #EE0000; font-style:oblique;">This is an old game design idea I had from 2008. I have changed very little aside from spelling corrections. I've added comments in **red**; these are either clarifications or apologies for my terrible writing.</div>
##### I. Table of Contents

**I. Table of Contents**
**II. Introduction**
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">A. Overview
B. Primary Concepts
C. Technology Brief</li></ul>**III. Gameplay**
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">A. Exploration Phase
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">1. Exceptions</li></ul>B. Combat Phase
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">1. Fighting Concepts
2. Exceptions</li></ul>C. Character Development
D. Player Interaction
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">1. Interface
2. Controls</li></ul>E. Major/Minor Goals
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">1. A Note On Hooks</li></ul></li></ul>**IV. Storyline**
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">A. Purpose
B. Synopsis
C. Chapter Detail
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">1. Introduction (Mysteries of Memory)
2. Warrior's Purpose
3. Love's Madness
4. In'thley
5. New Home
6. Wanderer
7. The Confrontation
8. Treacherous Returns
9. Broken Heroes
10. Hidden Secrets
11. Hellspawn
12. Mice of Misery
13. Light's Vengeance
14. Terror's Truth
15. Epilogue</li></ul>D. Characters
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">1. Elmtail
2. Dewpine
3. Alden
4. Quickfoot
5. Letheera
6. Gorode
7. Nitha
8. Ranthel
9. Meyah
10. Moonclaw
11. Tidleaf
12. Venera
13. Nithrot
14. Jerla
15. Lothro
16. Twiter</li></ul></li></ul>**V. World**
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">A. History
B. Primary Species
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">1. Rabbits
2. Foxes
3. Squirrels
4. Felines
5. Mice</li></ul>C. Secondary Species
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">1. Rats
2. Stoats
3. Bluejays
4. Canines
5. Otters</li></ul>D. Classes
<ul style="margin:0;"><li style="list-style-type:none;padding:0;margin:0;">1. Savage
2. Swordsman
3. Fighter
4. Archer
5. Mage
6. Warlock</li></ul></li></ul>

**II. Introduction**
Elmtail is a game designed around the story of a sword-wielding rabbit with a troubled past. While keeping to traditional techniques of a 2D RPG game, Elmtail deliberately makes unexpected design decisions and throws away all the RPG rules, where battles can happen anywhere, without warning, in arbitrary circumstances, with arbitrary rules. Gone are the rigidly defined rules of battle followed by games such as Fire Emblem and Final Fantasy. No event is restricted to a certain space, and the player will realize early on that they cannot assume anything.

{{%blockquote%}}**A. Overview**
The gameplay is designed to take full advantage of a 2D graphics engine in every way possible, integrating both a top-down view and a sidescrolling view. Furthermore, the game's mechanics are not as rigidly defined as most other games of this genre - Battles can take place either in the usual sidescrolling battle view, or in the topdown view. Players will control their character in both sidescrolling and top down views, and battles can happen anywhere, at any time. The key is to make the player believe that no matter how much they think they know, there will always be something they didn't expect, a battle somewhere they thought was safe, or a safehouse underneath a troop of gaurds.

The storyline is extremely important, and fully animated movies are used wherever possible in place of in-game cinematics. Character development is RPG in the sense that characters gain experience, but they do not have levels, and instead choose what to use that experience to improve. Characters almost never recieve new weapons, instead learning to use their existing weapon in new ways, if they even use one. Magic plays an important, though secluded role in the world. Woodlanders are aware of its existance, but few have ever seen its use, and even fewer can utilize it. It is said, however, that magic is attracted to lives which are entwined with fate.

**B. Primary Concepts**
The game is all about exploration, both of the physical world and the mental landscape of Elmtail and his companions. The storyline is very linear, and the player does not affect the storyline. There are not very many puzzles, either, the game being more combat oriented then puzzle oriented. However, the game tends to focus on interesting, unexpected and bizzare combat circumstances, as well as catching the player offgaurd. Instead of traditional item-based leveling of RPGs, the characters in Elmtail rarely get new equipment, instead developing new creative ways to use what they already have.

There are no levels, instead only experience which is used to acquire ever increasing skill sets. The storyline is focused on the feelings of revenge, forgiveness, perseverence in even the most horrific circumstances, desperation, the difference between a warrior and a hero, along with a subtext of what happens when crazy people get in charge of things. Multiplayer would be similar to Super Smash Bros, except with magical, sword-swinging woodlanders, where it is always one team against another.

**C. Technology Brief**
Elmtail's technology is all about breaking the normal. Powered by a custom graphics engine, a custom audio engine, and a custom networking engine. Its game engine will be designed to expect the unexpected, empowering the game to adapt the most absurd situations in all sorts of circumstances. The graphics engine uses DirectX 9.0c, the audio engine uses OpenAL, and the networking engine would use native winAPI calls. Because of how the characters move, using an animation rig with static images is not possible, requiring the use of an animation net where each animation node has certain other possible nodes it can go to based on circumstance. A large number of effects will utilize a particle engine, and the combat will have significant amounts of physics, but not to an extreme extent.
<div style="color: #EE0000; font-style:oblique;">The theoretical basis I had constructed for the combat system is actually still relevant. My mistake was assuming that an animation rig was impossible, when in fact an animation rig combined with interpolating frames could be adapted and use a similar nodal graph system to determine appropriate actions. The physics weren't well described here, but they involved primarily box-based collision with significant leeway and a very heavy emphasis on walljumping. With a few minor tweaks this would actually be pretty awesome.</div>{{%/blockquote%}}
**III. Gameplay**
The gameplay in Elmtail is designed to be both storyline-driven and fast-paced, without excessive amounts of battles. Instead, Elmtail utilizes unique circumstances to create innovative gameplay that is designed to surprise the player and force them to assume nothing. The player is in a city? That's not going to stop the assassins. They're on a raft going down a river? Lets dodge falling rocks while hopping between rafts! On a waterfall? Perfect place to have a swordfight. On the forest canopy? Yep. On top of a house? Oh yeah. Underground in a warren? That too. Don't forget the squirrel stampede either.

{{%blockquote%}}**A. Exploration Phase**
Generally this phase is done in topdown view, and is how the player explores their world. Elmtail does not have a world map for traveling on, all traveling is done in a topdown view in the standard game world, meaning that crossing large distances can be tedious if transportation is not available. The player can move in all 8 directions without any sort of constraint, other then hitting things. While some of the graphics are tile-based, the movement is not, and collisions are usually calculated with either hit-rectangles or hit-circles.

While there are often well-defined areas that can be distinguished from the generic plains and forests, a lack of a worldmap means that it functions similar to pokemon or zelda. All of the creatures following the player are on screen at all times, and it is rare that anything or anyone will simply be "assumed" to exist. This can create interesting situations for the player when they must lead their group through a camp or past gaurds. This means that early on in the game they will be granted the ability to give simple commands to their group - Freeze, Relax, Wait here, Come, and Bolt.
<div style="color: #EE0000; font-style:oblique;">This is another particularly intriguing aspect of gameplay that still interests me. The problem is that the attempts to use both top-down and side-scrolling would result in hugely different visual styles, which would be jarring. What I hadn't considered was a more isometric view for exploration, which could then be better integrated with a side-scrolling combat phase. In addition, you would need to be able to zoom out and come up with some method of faster travel or the game would simply become boring.</div>
{{%blockquote%}}**1. Exceptions**
While battling is usually done in the Combat Phase with a sidescrolling view, many storyline related battles or otherwise sideways movement dependent battles take place in top-down view in the exploration phase. The player still has access to all of their combat skills, but with limitations to compensate for the lack of a height dimension. For example, if a rabbit is caught while the rest make it past the guards, the combat will not switch to sidescrolling view, it will stay in exploration mode and the battle will take place there.{{%/blockquote%}}
**B. Combat Phase**
The Combat Phase occurs when the game goes into a sidescrolling representation of the current area. This enables the player to have a very lively real-time battle with their adversaries. The combat phase is where the vast majority of action-oriented abilities are put to full use, and like the exploration phase is completely real-time, which means Mages and any other players requiring charge-up time are vulnerable. Most environments extend a significant distance beyond the normal battling area, allowing battles to be skewed to one side or the other, or sometimes even split into two or three portions.

When entering combat phase, the game draws a best-fit line among the contenders and uses that to keep track of each player's location. It also uses this line to generate the actual arena, putting cliffs where cliffs are on the map and ponds where ponds are on the map, so the player is never magically transported off the map and into some alternate combat dimension, but in a physical world that is physically solid in all circumstances. Intuitive players can use this to their advantage and create environments beneficial for themselves.<div style="color: #EE0000; font-style:oblique;">Again, this would work way better with an isometric view than a strictly top-down view</div>
This allows the player to view the 2 phases as simply different ways of looking at the same thing, instead of 2 separate, self-contained worlds. It achieves this by assigning each individual tile an equivalent sideview picture - the battleground is assembled by taking the tiles that the best fit line intersects and stacking them up against each other, discarding tiles with less then 30% coverage.<div style="color: #EE0000; font-style:oblique;">This wouldn't actually work. A different approach would be required. The idea is similar, but tiles do not have one-to-one correspondence to side views. It is better to construct a separate metaworld to keep track of objects, then build both the sideview and the topdown (or preferably isometric) view using that information.</div>
{{%blockquote%}}**1. Fighting Concepts**
The fighting style of Elmtail is one in which the characters are jumping around quickly, mostly because rabbits can only hop quickly on all fours, which is hard to do with a sword in two paws. Melee fighting involves lots of jumping and quick strikes, with extremely fast encounters. Blocking can either be done by attacking the enemy at the same time as they attack you, if appropriate (Magic attacks rarely block an incoming punch), or a special block command can be issued which will block most anything, but has a cool-down time and a set period of use (Hit the button once, will block for 1.5 seconds, holding it down has no effect). The moves are heavily circumstantial, a good example being that if two swordsman are flying at each other at high speed and attack at the same time, they will cross paths, and one, both, or neither will take damage. Unarmed melee attacks often send victims flying into walls, especially if done while jumping.<div style="color: #EE0000; font-style:oblique;">This is actually pretty awesome.</div>
Swordsman are incredibly proficient with their swords, and also physically strong. They have the ability to leap off walls like rubber, delivering their sword blows at devastating speeds. If they ever get their paws on someone, they can throw them hard enough to crack the rocks on the face of a cliff, and their kicks snap bones like chopsticks. Fighters, however, have perfected the art of unarmed combat, and while only just as strong as Swordsman, know where to hit their adversary in order to get their eyes to water. While swordsman leap off walls like frogs, Fighters are rarely seen anywhere except in midair, where they're powerful throwing attacks can devastate an entire brigade of warriors. If encountered on the ground, a Fighter will usually try to leap into the air or get their opponent airborne as soon as possible. Archers are dextrous and possess some of the fighter's moves, but lack physical strength, instead relying on various projectiles to cut down their enemies. A Savage is a class reserved for any woodlander who knows how to use their teeth and claws, but have little or no fighting experience.

Magic is designed to be excessive, but prohibitively draining. Mages can only afford to use a few powerful attacks, or lots of weak attacks, and there is no middle ground. Beware a mage that has enough time to prepare a doomsday spell. At the same time, they are capable of defending themselves with rapid fire weak magic attacks, and have incredible defensive capabilities - but only if they cast the spell in time. Mages are physically frail, but agile, with extraordinarily powerful attacks, but excessively long charge-up times. Warlocks require no equipment, usually don't wear anything at all, and have magic attacks that often accompany their melee attacks. They have far quicker response times then mages, allowing them to better protect themselves, in addition to being physically stronger. However, they have access to very few powerful magic attacks, instead relying on unrefined magical blasts to defeat foes. When they do unlock powerful attacks, however, their enemies won't last more then 2 seconds.

**2. Exceptions**
Combat phase is also used in an exploratory manner in some circumstances, such as caves, waterfalls or anywhere in which a top-down view wouldn't be practical. Not only that, but it is used in many cutscenes, and in fact the player will often be surprised when after a battle is completed, they must either continue exploring while still in combat phase or watch a cinematic begin in combat phase only to have it switch to exploration phase halfway through. This helps blur the distinction between the two phases and emphasis the concept that they are two different ways of looking at the same thing instead of two different worlds altogether.<div style="color: #EE0000; font-style:oblique;">Side-scrolling is actually preferable to top-down in MOST circumstances, even with a better isometric representation.</div>{{%/blockquote%}}
**C. Character Development **
Elmtail has a development system where characters receive experience in traditional ways, but never gain levels. Instead, their level is measured by the highest level of skill that they have acquired. All classes have their own unique skill trees, and each tree has two or three different beginning skills that focus on different aspects of that class. However, all skill branches are intertwined, so one can focus on one branch and slowly gain access to another branch, though many powerful skills are only available if the skill branch is invested in from the very beginning. The player can gain access to a skill the instant they gain enough experience to acquire it, but higher skill levels require exponentially larger amounts of experience. Characters have many mundane items such as potions, gold, gems, herbs, and whatnot, but rarely, if ever, gain new equipment. Armor is almost non-existent, with only Mages wearing any significant amount of clothing, and weaponry is scarce. Thus, the skills that each character learns is what determines their might, not their items. Items are almost completely immaterial in the game, and there is a slight subtext detailing how the abilities of someone far outweigh their material possessions.<div style="color: #EE0000; font-style:oblique;">It's not explained well here but the idea was that there would be a complex system of skills that interacted with each other, so creating a proper build Diablo 2 style is crucially important. Items would still be useful for some classes, but only in combination with skills, which would magnify their power by an order of magnitude.</div>
Stats of a character, including health, strength, magical prowess, stamina, etc. are increased on a linear basis whenever experience is spent on a skill. The rate of each stat increase is stored as a decimal based on each experience point gained, and in addition to the class bias given to these values, the skills a character chooses affect the rates of increase (If a skill were to increase a character's rate of health improvement, the skill first modifies the rate value, then the experience points are tabulated for the health gain). There is no limited to the amount of unused experience points a said character can have.

**D. Player Interaction**
The player's interaction with the game always involves controlling a single character, which is usually Elmtail. Elmtail can talk to other woodlanders, occasionally tell his band of creatures simple commands, fight, climb, run, jump over small obstacles, move small objects, as well as other miscellaneous moves inherited from combat phase. Some conversations have multiple ends, especially those with townspeople or one's with little or no effect on the storyline.<div style="color: #EE0000; font-style:oblique;">Dues Ex: Rabbit Revolution.</div>
{{%blockquote%}}**1. Interface**
Elmtail practices a minimal but functional GUI, spread out along the top and bottom of the screen. It changes based on circumstance, whether the player is in battle, outside battle, or engaging in a special event. It displays information relating to the character's health, a mini-map, situation dependent goals, among other things. Menus are accessed with a menu button, navigated with the arrow keys (up and down change menu selection, left and right expand and contract submenus, respectively). Information is easily accessible and the GUI is responsive, allowing the player to instantly disengage it, go forward or back a step, all by using arrow keys and the menu toggle key.

**2. Controls**
The arrow keys are used to navigate the game world and interact in almost all circumstances. The game is designed to allow the player to adapt a "When in doubt, use the arrow keys" philosophy. The keys on the left side of the keyboard are used for attacking, defending, interacting with other characters, opening menus, and whatnot. The keys are arranged in such a manner that the pinky should rest on the shift key, the ring finger on A, the middle finder on S, first finger on C or D, and the thumb on space. This configuration would be supplemented with one using the default FPS hand position. The F1-F12 keys are used as shortcuts for various actions. Note that this is the default key configuration and can be manipulated by the player.
<div style="color: #EE0000; font-style:oblique;">This confuses me because that position is already almost identical to a default FPS configuration. A shift+z+x+space cave story esque control scheme might work better</div>
In exploration phase, holding down an arrow key will cause Elmtail to hop in that direction. Hitting an arrow key twice will cause him to start bolting, and he will adapt a zigzagging pattern as well. In combat phase, holding down left or right will make Elmtail walk, and hitting them twice results in him jumping in that direction. The up arrow key causes him to look up and to aim his attacks in that direction, while the down arrow when held makes him prone, and pressing the down arrow key twice will prepare a power jump.

Spacebar will be used for jumping, and there is no double jumping, but pressing down twice (and holding) prepares a power jump, 1.5x as high as a normal jump. Further control details cannot be determined without playtesting.
<div style="color: #EE0000; font-style:oblique;">This is kind of stupid. You absolutely need a double jump in this kind of game. Apparently I either didn't realize that at the time or was trying to hard to be different.</div>{{%/blockquote%}}
**E. Major/Minor Goals**
Minor goals of the game include winning a battle, gaining enough experience to get a skill, reaching a destination, and intermediate points between major goals. The major goals are usually tied to the storyline and involve the completion of puzzles, conclusion of quests (which involve anything from beating a boss to finding a friend), and furthering the storyline. Precursors to major goals are sprinkled through the game in order to keep things interesting, as the battle completion minor goal gets tedious quickly, as does the gain enough experience to get a skill goal does.

{{%blockquote%}}**1. A Note on Hooks**
The player needs constant hooks to keep them engaged in gameplay. They can save anytime during exploration phase, but not during combat phase. This results in hooks that should be concentrated directly after a battle is concluded or when the player reaches a destination, as those are the places that they are most likely to save, and thus they will be most likely to load the game up from those areas and need a new hook to re-engage themselves. Hooks range from unimportant village crisis to major storyline plot development.
<div style="color: #EE0000; font-style:oblique;">I had been reading a lot of game design theory when I wrote this...</div>{{%/blockquote%}}{{%/blockquote%}}
** IV. Storyline**
The storyline is key to Elmtail, driving both the gameplay and flow. It is decidedly linear, with very few branches, using cinematics extensively and integrating gameplay into itself in creative ways. The storyline drives itself and drives gameplay, not the other way around - the game is expected to adapt itself to the storyline's needs, instead of the storyline adapting to the limitations of the engine.<div style="color: #EE0000; font-style:oblique;">Later concepts of the game actually considered a more adaptable storyline, since while there are critical plot points, there's quite a bit of room between them and the intervening plots can be intertwined in a number of ways.</div>
{{%blockquote%}}**A. Purpose**
The story of Elmtail is one that follows a young rabbit as he grows first into a warrior, and then into a hero. He learns how his heart is as important as his sword, how true grief is one that leaves a hole in the heart, and how a true leader looks past himself. His journey is long, harsh, and filled with misfortune. Through it all, he continues forward, sometimes with optimism, other times with grim determination. At first, he believes that he is the only one capable of putting an end to the treachery beseiging the countryside. In the end, it is his friends that make that victory possible.
<div style="color: #EE0000; font-style:oblique;">The story of Elmtail is one that has bad grammar, terrible plot direction, reliance on killing off characters as a form of character development, and a total lakc of subtlety. Seriously the plot in this game is just stupid. The underlying message is good, but its execution was terrible. You've been warned.</div>
Elmtail is full of morals, but it is not just about how a young rabbit overcomes his own headstrong nature, it is a lesson on how anyone can be a good person, if you only give them that chance. Even great heroes were once feisty young kittens, and one's ability to recognize that is key in realizing their own potential.

**B. Synopsis**
Elmtail (Swordsman) begins as a small rabbit kit, who is painfully reacquainted with the horrors that resulted in his ending up in the forest. Alden, a lone hermit of the forest, serves as his protector and trainer for almost an entire season, teaching him how to defend himself. Able to defeat Alden with his youthful energy, Elmtail sets off to find his brother and put an end to his underground network of spies and assassins. After travelling for a few days, he finds a rabbit called Quickfoot as he dashes across the path, with two stoats close behind. Quickfoot (fighter) proves to be a rabbit of high martial prowess, and together they fight off the stoats and continue their journey to the Eastern Cliffs.

While hopping through a field, Elmtail is pounced on by a fierce doe, who was driven hysterical after a bear mauled the buck that was to be her mate. Her name is Dewpine (Savage), and she joins their party. Pointing them to the village, they meet a helpful fox storekeeper whose adolescent kit asks to join the party. Elmtail agrees, and they are led to a squirrel who knows the location of Moonclaw's hideout. After giving them a compass to help guide their way, he bids them good luck, and they set off at first light.

A week later, when they stumble on a tranquil valley (and after meeting up with Nitha the squirrel (female)), Dewpine reveals that she is carrying Elmtail's kits, and she begins to build herself a warren. While foraging for supplies, the group stumbles on a pair of unconscious rabbits tied to a stake, which was presumably going to be lit on fire. When they arrive there, however, the only creatures remaining are some mice scavenging for food. The rabbits turn out to be a mated buck (Ranthel) and doe (Meyah), and the doe is expecting kits in a few days. Elmtail suggests that they join up with Dewpine, who was digging herself a warren for their own kits, though she wasn't expecting to give birth for a few more weeks.

Confident that Ranthel would protect both his own mate and Dewpine, Elmtail sets off again to his brother's lair. They join up with a squirrel (Nate, Male, archer) and a mouse (Tipew, female, Warlock). Finally, they find the campsite where his brother resides (mostly of mice, some stoats, one rabbit). They follow one of the patrols as it leaves the city and ambush it, then send Gorode to secure a backdoor passage into the camp. When a search party is sent out, they ambush that as well, then sneak in through a path that Gorode found. They attack the camp from behind, and after plowing their way through the guards, they reach his brother (Moonclaw, swordsman) and his personal bodygaurds. Only after they are all defeated does the confrontation take place. Moonclaw reveals that he was forced to kill his family by the Mice of Misery, who wanted his skills as a fighter. Elmtail would have killed him had Nate not reminded Elmtail of what it meant to be a hero.<div style="color: #EE0000; font-style:oblique;">This is a terrible attempt at a dark moment for our hero but was executed very poorly.</div>
Disgusted, Elmtail takes Moonclaw as a prisoner and they head back to the warren Dewpine had dug. Its a treacherous journey along the way, but they learn a lot more about Moonclaw, the "Mice of Misery," and some of Elmtail's own history. Upon their return, Elmtail is delighted to see that Dewpine had a litter of 8 strong kits. His delight, however, is cut short when a viscous band of mice, stoats, foxes, felines, and rabbits surrounds the warren. Herding the kits into the safety of the warren (and releasing Moonclaw), the party bravely stands up to the onslaught, but Dewpine is mortally wounded and dies in Elmtail's arms. That morning, Moonclaw returns, and Elmtail's hatred is overcome by awe at this unexpected behavior of his brother. Moonclaw wishes to clear his name and right the wrongs that he has done. Elmtail is suspicious, but the rest of the group convinces him that Moonclaw had nowhere else to go, nor any incentive to betray them. Still somewhat shaken by his mate's death, Elmtail leaves his kits in the care of Meyah, who swears on her name to protect them from harm. <div style="color: #EE0000; font-style:oblique;">OH NO I NEED ELMTAIL TO HAVE FEELINGS, GUESS I'D BETTER KILL HIS WIFE.</div>
Elmtail leaves to find the lair of the Mice of Misery and to put an end to their terror once and for all. By now, he is deeply respected as a swordsman of unmatched skill, and creatures from far and wide who have heard of his journey seek him out. Letheera the rabbit mage (female) is the first to join them, and when she describes the stories hailing Elmtail's heroic cause and the many creatures attempting to find him, he begins a tradition of leaving a special mark made of the remains of his camp's fire pointing in the direction they are headed. 3 rabbits (Tidleaf, warlock, male; Venera, archer, female; Nithrot, savage, male), a squirrel (Jerla, fighter, female), a rat (Lothro, mage, male), and a bluejay (Twiter, SPECIAL, male) join up with them as they fight newly emerging hellspawn.<div style="color: #EE0000; font-style:oblique;">This is actually a not-stupid element of gameplay I've incorporated into my more recent game designs where, by the second half of the game, you should be almost universally recognized, and any enemies with a functioning brain and nowhere near enough power to defeat you should simply run. It would be really nice to have random NPCs acknowledge your accomplishments rather than just say random scripted stuff.</div>
The group manages to capture one of the Mice of Misery mages and force him to explain things. The Mice of Misery are apparently attempting to build a sustained bridge between the physical realm and the magical realm. While a link to the magical realm is what allows mages to practice magic, a large sustained bridge would result in hellspawn of unthinkable power appearing in the physical realm. Moonclaw was going to play a mysterious role in all this, but the Mice of Misery's mage doesn't know what it was. Now the group knows why the hellspawn are appearing in such large numbers - the Mice of Misery are getting closer to their goal and have some kind of sustained bridge, somewhere.

A few nights later, Letheera awakes screaming from her sleep. She had a terrible nightmare about a creature that could only have been a hellspawn, but more importantly, she knows where the bridge is located, and that it is called "Hell's Maw." The group is about to begin their journey to the bridge when Venera suggests going to a friend of her's who can teleport them to the location. He lives in a nearby village, and is happy to assist the brave warriors in their quest. It takes almost all of his power, plus some assistance from Letheera, but he manages to transport them within a mile of the bridge's location. Fighting through massive Mice of Misery patrols, the group battles their way into the depths of the underground fortress. The deeper they go, the stronger the magic wielders become, the magical energies from the bridge empowering their abilities. The warlocks, being more directly attuned to raw magical energy, benefit the most.
<div style="color: #EE0000; font-style:oblique;">Hell's maw is such a creative name for the last level! The idea of the characters becomes disproportionately strong as they near the source of magic is probably one of the best ideas outlined in this document. It's a fantastic excuse for absurdly over-the-top battles and epic climactic scenes.</div>
They finally arrive at a giant cavern, where a truly horrifying sight resides. A gigantic portal, a gate to the magical realm, is growing ever larger in size. The Mice of Misery put up a fierce battle to protect it, but even when they are defeated, Letheera realizes in horror that they are too late. The gate is now self sustaining, its size so large that it is able to draw all the magical energy it needs from the magical realm itself. Then, a hellspawn of unprecedented size steps through the gate, and Letheera gets an idea. If they defeat the hellspawn while its halfway into the portal, the energy released should destabilize it for a few precious seconds they could use to shut it off. The final battle begins.
<div style="color: #EE0000; font-style:oblique;">STOP GETTING SCIENCE ALL OVER MY FANTASY!</div>
After lots of fighting, it seems like they are going to win, but then the hellspawn heals itself through the bridge connection, and they realize that their plan cannot work, because the only way to beat the hellspawn is to lure it away from the gate so it cannot heal itself. But if they did that, once the hellspawn was defeated it would only be replaced by one more powerful! Then, Tidleaf realizes the only way for them to win is for him to directly channel the energy of the magic realm and defeat the hellspawn in one huge attack. "You know I have to do this, Letheera!" he shouts, making his way to the portal, and leaving the rest of the group to distract the hellspawn long enough for him to reach it.
<div style="color: #EE0000; font-style:oblique;">More plot-driven sacrifices because I apparently had a thing for brutally murdering all of my characters. I blame Redwall.</div>
Once the player has distracted the hellspawn long enough for Tidleaf to reach the portal (which is no easy task), Tidleaf begins blasting the hellspawn with magical energy. The player can then attack with whatever character they choose, and every single other character will also attack (computer controlled). The hellspawn is 25 times more susceptible to damage, but it is also healing at a fantastic rate. After defeating this insane monstrosity, it detonates, destabalizing the portal for a few precious seconds, and Leethera blasts it. The gate collapses in on itself in a fantastic explosion, and Tipew only barely manages to throw up a shield in time.

**C. Chapter Detail**
The storyline is split up into chapters for ease of reading and the sake of organization. The player is completely unaware of which chapter he is in, and there are no clear boundaries between chapters. Cinematics are written like stories, in order to better capture the essence of the scene. Gameplay is described in impartial third person.

**1. Introduction (Mysteries of Memory)**
-------------------------------------
**Cinematic:** The forest is lively, with birds twittering and squirrels leaping from branch to branch. A young rabbit kit lies unconscious in a nearby clearing, next to some rocks and tuffs of grass. He begins to stir, then rolls over with a start, looking around in confusion.

"Where am I? Who am I? What am I doing here? Um... I'm Elmtail! ...but, where is this?" The rabbit kit hops around in circles several times before sitting down and taking inventory of himself. 

"Well," said Elmtail, looking over himself, "I'm not hurt or anyth-" The rabbit kit paused, his right ear turning towards a sound. Sniffing, he couches to the ground, not knowing what to expect. Suddenly, a fox emerged from the underbrush. Standing upright, it held a knife in one paw, and a smug grin on its face. Terrified, Elmtail nearly bolted when he heard a rabbit's warcry, and a white blur slammed into the fox. Disarmed, the fox gulped as the rabbit held his sword's keen edge to its throat.

"Leave while you're still breathing," growled the swordsman, lowering his sword slightly. The fox slunk off into the forest, its tail between its legs, and with one less knife. Satisfied, the rabbit sheathed his sword and turned to Elmtail, who was quivering in fear. "Well now, that takes care of that. I'm Alden, whats your name?"

"E-E-Elmtail?"

"Right, Elmtail, I don't suppose you know how you got here?" Elmtail shook his head. "Ah, well, we'd better-" Alden froze, ears twitching. He drew his sword and positioned himself between Elmtail and three foxes, who had apparently come back for revenge. "Don't move," he whispered, "I'll handle this."

"You'll handle this?" spat one of the foxes, "Your not handling anything, your getting your butt kicked!"

A smile crept its way across the swordsman's face. "Oh?"

*Break into scripted combat phase*

Alden is an incredibly good swordsman, and the player is treated to his first experience with the side-scrolling real time battle view, watching as Alden makes mincemeat of the idiot foxes. The battle is mostly AI scripted, and could vary slightly in its progression. To prevent time paradoxes, Alden cannot die in the battle.

*Return to cinematic mode*

Alden watches the foxes limp away, nursing their many wounds. "Well, that takes care of that. We'd better go find a safe place to stay the night."

Elmtail was still attempting to comprehend what had just happened when he heard a rustling in the trees above him. Looking up, he squeeks and hides behind Alden at the sight of a large squirrel halfway descended down the tree trunk.

The squirrel grinned. "My, what a frightened little rabbit kit you've got there, Alden!"

Alden smiled. "Roland, where have you been? I haven't seen you in a full moon!"

"Oh, you know, around." The squirrel chuckled, "I traveled to a nearby forest to collect nuts, and met a fine family of squirrels in the process. I was beseeched by them to stay and help defend them against a bunch of nasty stoats, so naturally I obliged. There's a lot fewer stoats around there, nowadays."

Alden laughed. "You old rascal! Your always collecting nuts, with or without stoats!"

Roland looked indignant, "I ain't old, I got another 10 seasons to go before you can go around calling me old!"

"Oh be quiet, you blithering bloke. I was hoping you could take this little rabbit kit here and whisk him off to my camp? I don't think the little guy's in shape for the kind of sprinting necessary to get back there before dark."

Roland nodded. "Of course, if the silly little kit's going to come out from behind your tail, that is!"

Elmtail slowly creeped out from behind Alden, who looked at him encouragingly. "Roland isn't a stoat Elmtail, you'll be perfectly safe with him." Roland came down the tree on to the ground. "Just hop up on his back, and hang on tight!"

Nervous, the rabbit kit hops up on the squirrel, and hangs on for dear life as Roland zips back up the tree and launches into the setting sun, with Alden close behind.

--------------------------------------
*Fade to black, fade back in to next scene*
--------------------------------------

Its dark, the fading light of a fire illuminates the sleeping form of Elmtail. He mutters in his sleep, and appears to be having a nightmare. He is back at his house, on a dark night just like this one, with moonlight filtering in through one of the windows. He can hear his family sleeping in their rooms, and nearly drifts off himself when he notices the absence of his brother. Not a second later, he hears the doorway creak open, and the soft pattering of footsteps. There was the sound of a sword being removed from its sheath, and then his parents breathed no more. The rabbit kit put two and two together, but didn't know what to do. If his parents were dead, surely the only option would be to run?

However, the horrors had only begun, when the breathing of his sister abruptly stopped. Trembling in fear, the rabbit kit wondered who would be opening his door, and if he might be able to hide behind the dresser. Before he could, however, his door creaked open. The grim face of his brother stared back at him, and the little rabbit kit was about to scream when the sword's pommel descended on his head, and the world turned black.

Elmtail woke up wailing piteously, disturbing the sleep of both Alden and Roland.

"Oh no, did you have a nightmare?" asked Alden, scooping up Elmtail and cradling him.

Elmtail sniffed, his crying reduced to a frightened whimper. "I... I think I know how I got here... and that my mommy's dead."

Roland was horrified upon hearing this come out of a month and a half old kit. "Cursed acorns, what happened to this poor thing?!"

Alden frowned. "Maybe he can tell us. Can you, Elmtail?"

Elmtail nodded, "It was back in my home, everyone was sleeping, except my brother wasn't there, and then someone came through the door, and my parents were dead, and then my sister was dead, and then they came in my door and it was my brother and he hit me and everything went away."

Roland scratched his ear, but said nothing. Alden looked at Elmtail for a long time, studying him, digesting his story. "Elmtail, what did you see me do back there in the forest? To the fox?"

Elmtail smiled. "You showed the fox whose boss!"

"Would you like to know how to do that?" asked Alden, "I have to warn you, it won't be easy."

"Really? Cool!"

And so, by night's end, Elmtail had been inducted into the care of Alden, who would teach the young rabbit more ways to use a sword then a cloud had droplets of rain.

*Fade out to gameplay*
--------------------------------------

{{%blockquote%}}**Area Information: **Alden's Home *(Top-down)*
Random encounters: 0%
Effects: *None*
Objects: Firepit, lean-to, pile of wood, logseat, warren entrance hole
Setting: Elmtail is training by a tree when Alden calls for him to come into the warren. Elmtail appears to be almost fully grown now, and his voice is different.{{%/blockquote%}}
{{%blockquote%}}*Dialog*
Alden: Elmtail! I need you in the warren!

Elmtail: Alright, I'll be there in a second!

Alden: Its been more then a second!

Elmtail: Oh, do shut up!{{%/blockquote%}}
The player now has control of Elmtail. He can explore the camp or head directly into the warren. If he tries to leave the camp area, Alden will call him back. Going into the warren triggers the next area.

{{%blockquote%}}**Area Information: **Alden's Warren *(Side-Scrolling)*
Random encounters: 0%
Effects: Entrance tunnel is pitch black, Side-scrolling
Objects: *No objects of interest*
Setting: Elmtail must make his way down into the burrow with his underground senses{{%/blockquote%}}
{{%blockquote%}}*Dialog*
Elmtail: Where'd the lanterns go?! I can't see a thing!

Alden: Nature's grace, Elmtail, your a rabbit! Use your whiskers!{{%/blockquote%}}<div style="color: #EE0000; font-style:oblique;">This is utterly retarded and I have no idea what it's in here.</div>
The walls directly beside the player are illuminated in monochrome, and they must find their way through a somewhat confusing, though not nessacarily difficult, entrance path until they reach the lit interior of the warren. They can look around, but there isn't much to see, and must eventually talk to Alden.

{{%blockquote%}}*Dialog*
Alden: It should be pretty obvious now that I've taught you all I can. You continually defeat me in our sparring and I simply can't keep up with your youth. Therefore, I have decided you can venture out into the world with my blessing.

Elmtail: Whoopee!

Alden: However, I must once again emphasis my discontent at your goals...

Elmtail: Alden, I've told you once and I'll tell you again. I'm going to hunt down my brother, wherever his evil horde might be, and kill him. 

Alden: Killing him won't bring back your sis-

Elmtail: Does it look like I care?! I want him dead! He murdered my entire family! He runs an entire criminal network! He deserves to die a slow, painful death a thousand times over!

Alden: Your violent nature will get the better of you.

Elmtail: Says who? I dare any creature to come and try to harm me or my friends.
<div style="color: #EE0000; font-style:oblique;">HEY LOOK GUYS I CAN WRITE A HERO WITH FLAWS. Really obvious flaws. Ugh.</div>
Alden: Oh why do I even bother. Go on, get out of here, you rascal!

*The two rabbits embrace*

Alden: Magic is everywhere, Elmtail. Try to keep it on your side.
<div style="color: #EE0000; font-style:oblique;">That's actually a really good line.</div>{{%/blockquote%}}
The player may now leave the warren and then leave the camp through the only open path. If they return, Alden will be wandering around and will do the standard "Back so soon?" gig.

{{%blockquote%}}**Area Information: **Path to Alden's Camp *(Top-down)*
Random encounters: 5% - Fox theif
Effects: *None*
Objects: *No objects of interest*
Setting: This is a short pathway through the forest that provides a barrier between the safety of the campsite and the outside wilderness. There are only petty fox theives to fight here.{{%/blockquote%}}
The first battle the player encounters will trigger the following dialog:

{{%blockquote%}}*Dialog* - Before battle
Enemy 1: Oh look, its a cute little rabbit!

Enemy 2: With a cute little sword!

Elmtail: Would you like my cute little sword to slice through your neck, or are you going to shut up and let me pass?

Enemy 1: Oh ho ho, what a cocky little rabbit! I think we need to teach him a lesson!
<div style="color: #EE0000; font-style:oblique;">A better comeback probably would have been "Say one more thing and the cute little rabbit is going to shove his cute little sword right up your ass, dickhead." But that's not very kid-friendly.</div>{{%/blockquote%}}
{{%blockquote%}}*Dialog* - After battle
Enemy 1 (Barely alive): Th-th-thats not cute!

Elmtail: Damn right it isn't.{{%/blockquote%}}
**2. Warrior's Purpose**

{{%blockquote%}}**Area Information: **Western Forest of Feshonia *(Top-down)*
Random encounters: 15% - Fox (Any type), 5% - Stoat, 10% - Non-sentient Feline
Effects: *None*
Objects: *No objects of interest*
Setting: This is a large area, and is the centerpoint for the first portion of the game. The path splits off several times, but most of them are blocked. One is blocked by a magical barrier, another by a magical spell, another by a locked door and a wall, another by a landslide, and the last with a pile of debris. At this time, the pathway to Elmtail's Home Village and the pathway leading out of the forest are open.{{%/blockquote%}}
Leaving the Path to Alden's Camp triggers a cutscene. The cutscene and the subsequent battle are assumed to take place in the Western Forest of Feshonia.
<div style="color: #EE0000; font-style:oblique;">There is something very wrong with the name "Feshonia".</div>
----------------------
**Cinematic:** Elmtail is hopping along the forest trail when he hears something and pauses. Turning his head towards the undergrowth, he sees some rustling bushes, and then a rabbit leaps out, barreling straight into him.

"Oof!" The rabbit scrambled off of Elmtail and looked into the bushes with wide eyes. "Sorry there mate, but I've got foxes on my tail. Come on, lets get out of here before they have us for breakfast!"

Elmtail picked himself up. "I'm not going anywhere," he said, drawing his sword.

The rabbit looked at him, confused. "Are you bleeding mad? You're a rabbit, not an otter!"

Just then, the foxes burst out from the undergrowth and the rabbit bolted behind a tree. Elmtail grinned at them. "Hiya fellas."
<div style="color: #EE0000; font-style:oblique;">This is a direct reference to The Matrix.</div>
The foxes look at each other, and then one of them pounces on Elmtail. There is a move to side scrolling view, but the player doesn't yet have control of Elmtail. First, Elmtail does a super jump, spins around and preforms a coup'de'grace on the fox, slamming it into the ground. At this, the other fox joins in, and the player regains control of Elmtail.
----------------------

{{%blockquote%}}**Minor Boss - The Two Foxes**
These are two abnormally powerful foxes, roughly x2 as powerful as a normal fox. They are not meant to be difficult.{{%/blockquote%}}
{{%blockquote%}}*Dialog*
Quickfoot: Well, tie me up and feed me to mice, that was incredible. I've never seen a rabbit fight like that. Uh, my name's Quickfoot, by the way.

Elmtail: Pleased to meet you, Quickfoot; I'm Elmtail. Is there any particular reason you have that name?

Quickfoot: Er, you could say I'm very fast on my feet.

Elmtail: I figured *that* out.

Quickfoot: I'm also very, ah, skilled in melee combat.

Elmtail: Oh?

**Elmtail looks around**

Elmtail: I certainly hope so, because I can't fight 6 foxes by myself.

Quickfoot: Six foxes?

**Six foxes emerge from the undergrowth around them, growling.**

Quickfoot: Eeep!
<div style="color: #EE0000; font-style:oblique;">SUCH TERRIBLE WRITING AUGH!</div>
Elmtail: Get a hold on yourself and help me out here!{{%/blockquote%}}
{{%blockquote%}}**Minor Boss - The Six Foxes** *(Irregular)*
These foxes are only x1.5 as strong as a normal fox, but they have numbers on their side. The player must cooperate with Quickfoot in order to fight them off. This battle is not, however, a traditional battle, and it takes place in the top-down view, thereby serving to familiarize the player with the controls and flow of a top-down battle. {{%/blockquote%}}
{{%blockquote%}}*Dialog*
Quickfoot: Allow me to say **pant** once again what an incredible **pant** fighter your are.

Elmtail: The same **pant** could be said about yourself. **falls down to catch his breath** Oof!

**The two rabbits pause, both of them trying to recover from the battle**

Quickfoot: So what has you out in the wilderness? Are you going somewhere?

Elmtail: I'm searching for someone. My brother, you see, runs an underground network of assassins, spies and thieves. I intend to put an end to it.

Quickfoot: You don't mean Moonclaw, do you?

Elmtail (surprised): He's that well known?

Quickfoot: I'm afraid so, and I seriously doubt you'll ever get near him.

Elmtail: **picking himself up** Watch me.

Quickfoot: Nah, I'd be bored out of my skull. I'll help you.

Elmtail: Really? We've barely known each other for 10 minutes!

Quickfoot: **crosses his arms** And in those 10 minutes you've saved my life and shown me that rabbits can stand up to their adversaries instead of just running away. I'm coming with you.

Elmtail: **nodding** Thank you.

Quickfoot: No, thank *you*. Now, lead on, brave warrior!{{%/blockquote%}}
Upon reaching a fork in the path, the following dialog occurs:

{{%blockquote%}}*Dialog*
Quickfoot: Hey, a fork in the road. Any idea which one we should take?

Elmtail: Not a clue{{%/blockquote%}}
The player has two choices, they can simply leave the forest by taking the left fork or they can go down the right fork and explore Elmtail's home town. Should they visit Elmtail's hometown (either by choice or by luck), the following information applies.

{{%blockquote%}}**Area Information: **Elmtail's Home *(Top-down)*
Random encounters: *None*
Effects: Fog, Eerie, Silent (+ambience)
Objects: The town has approximately 20 or so objects that can be interacted with. After 3 of them have been interacted with, it triggers a flashback.
Setting: Elmtail is unaware that this is his hometown, until after he examines at least 3 objects, at which point the rising memories trigger a flashback.{{%/blockquote%}}
{{%blockquote%}}*Dialog*
Elmtail: Wait a minute, I've been here before... **brief frames of his memory begin to flash on the screen** No... No! **Elmtail collapses, then the screen fades to black and directly into the memories again, but this time they continue into when he woke up. The house is in flames, the ceiling obscured by smoke. Crying for his mother, Elmtail looks around and spots a hole in the wall and hops out into safety. Not knowing what to do, the young kit continues to sprint into the forest, until the memory fades away.**

Quickfoot (Noticably disturbed): A-Are you alright?

Elmtail: I never thought I'd ever see this place again...

Quickfoot: What place?

Elmtail: This town... is my home.

Quickfoot: Not a very nice looking home, if you ask me.

Elmtail: **Hops over to a pile of ash** ...They burnt it down. They destroyed it all.

Quickfoot: You gonna be ok? Maybe we should camp here for the night?

Elmtail: No, nevermind. Come on, lets try the other path.
<div style="color: #EE0000; font-style:oblique;">This is really dumb ._.</div>{{%/blockquote%}}
Once the player has gone down the left fork, they will pass by the other paths that are blocked. They can attempt to explore these paths and/or break through the blockades but they will not be successful. Upon nearing the edge of the forest, they encounter a small mouse, and the following dialog occurs:

{{%blockquote%}}*Dialog*
Elmtail: Wait, I hear something. Who goes there?

**A mouse comes out from beneath the undergrowth**

Quickfoot: Oh, its only a mouse.

Mouse: *ONLY* a mouse?! What kind of nonsense is this?! **An eerie glow begins to surround the mouse as the screen darkens** Would you like me to demonstrate what a poor little mouse can do?!

Elmtail: Uh, uh, uh, no thanks. We'll just be, uh, going on our way now.

Mouse: Yeah, you certainly will be, and you won't be making *that* mistake again, I hope. You'd best be careful, boys, I happen to be rather forgiving. Not many mice are.

Elmtail: Yes, we'll keep that in mind, thank you.

**The two rabbits scamper down the path**

Quickfoot: Sorry.

Elmtail: Don't do that again. Please.

Quickfoot: Sorry!{{%/blockquote%}}
Leaving the forest triggers a cutscene in the next area.

**3. Love's Madness**

{{%blockquote%}}**Area Information: **The Northern Fields *(Top-down)*
Random encounters: 20% Stoats 15% foxes 10% nonsentient rodents 5% Hostile feline
Effects: *None*
Objects: The Riverside Inn.
Setting: This is a large section, enscapulating all of the area between the forest and the village of In'thley. It contains the field of yellow grass near the forest, and a large green field with a river running through it.{{%/blockquote%}}
----------------------
**Cinematic:** The two rabbits are hopping out of the forest. The pathway cuts through a field of tall, yellow grass swaying gently in the wind, and the path continues forward for several hundred yards before raising up on a hill, where the grass is greener, shorter and presumably well-grazed. The two rabbits had barely gone 10 feet when a female rabbit leaps out from the left and slams into Elmtail. The two rabbits roll over one another until Elmtail whips out his sword and pins the female to the ground.

"What are you *doing?!*" asked Elmtail, his sword still poised near the doe's neck.

"I... don't know?" The confused doe looks around her and then promptly falls unconscious. Elmtail hesitates, then sheathes his sword and looks at Quickfoot with a confused face.

"I think she's a bit crazy, to be honest," said Quickfoot. "But maybe she's just disoriented. Come on, lets make camp here and see if she comes 'round a little more sane.

It was close to sunset, the two rabbits sat around a newly lit campfire, with the confused doe still unconscious. With a groan, she raises her head, blinks, and tries to get her bearings. "Wha- Where am I?"

"Are you ok?" asked Elmtail, trotting over.

"Huh?" The doe rolled over, "Where's Charles?"

Elmtail was confused. "Charles? Whose cha-" Elmtail was cut off by a sudden gasp from the confused rabbit.

"Oh no, oh no... no..." Her mutterings dissolved into broken sobs and she put her paws over her nose.

Quickfoot hopped over and shared a concerned look with Elmtail. "Is something wrong?"

The doe sniffed, raising her head. "Its just... Charles was going to be my mate earlier today, but before I knew it, a giant bear nearly had me for lunch. I wouldn't be here if it wasn't for Charles... He threw himself at the bear and... well..." Her voice trailed off and she curled up into a trembling ball.

Worried, Elmtail nuzzled her, and she uncurled herself.  "What do you want?" she said in a depressed voice.

"Would you like to travel with us?"

"Well, I can't say I have anything better to do. But if i'm going to be travelling with you, I ought to tell you my name, shouldn't I?"

Quickfoot laughed, "Oh my, I guess we forgot all about that!"

"I'm Dewpine," she smiled, "where are you boys headed off to?"

Elmtail scratched his ear. "Well, Dewpine, I'm searching for my brother-"

"Was he taken captive by that awful Moonclaw?" asked Dewpine.

Elmtail was surprised that his brothers name could come up so easiely in conversation. "Er, no, he *is* Moonclaw, I intend to kill him."

"Oh," said Dewpine, taken aback. "Well, if its Moonclaw your after, you'd better be sure your going the right way."

"You know where his hideout is?" asked Quickfoot.

"No, but Charles was from the village, and he told me where that was. Maybe we can find someone there who knows."

At this, Elmtail's ears perked up. "Where's this village?"

"Follow me," said Dewpine, who bolted towards the hill.

"Hey, wait up!" Elmtail only barely managed to follow her through the thick grass. They reached the top of the hill, and Elmtail saw a vast countryside below him.

"The village of In'thley is just towards the west there, you can see its bell tower. If we travel to the river, we can follow it there."

Elmtail nodded and watched as the orange red light of the sun swept across the fields, illuminating the top of the bell tower with white. The two rabbits sat there in silence for a long time, until Elmtail finally spoke up. "I'm sorry for your loss, Dewpine." She said nothing, but touched her nose to his in thanks.
<div style="color: #EE0000; font-style:oblique;">This sounds like a 15 year old's stupid romance novel.</div>
**Fade out*
----------------------

The player gain control of Elmtail after he wakes up in the morning, while the others are still asleep. The player can either explore the fields or wake up the others. He cannot leave the field until he's woken up Dewpine and Quickfoot. It is possible to encounter small enemies on the far side of the field, which Elmtail will fight alone if the others are still asleep. Upon waking the others, the following dialog (or a small variation of it depending on who was woken up first) will occur:

{{%blockquote%}}*Dialog*
Dewpine: Ugggghhh... Is it morning already? I wish the sun would slow down...

Quickfoot: Nature's grace, Elmtail, your an early riser.
<div style="color: #EE0000; font-style:oblique;">UNLIKE ME =:C</div>
Elmtail: Hey, we need to get moving if we're going to make it to that river by sundown, now get your furry tails out of bed!

Dewpine: Thats how you talk to a doe? How rude!
<div style="color: #EE0000; font-style:oblique;">All these "doe" references come from reading Watership Down shortly before writing this. Their necessity is debatable.</div>
Elmtail: ...please?

Dewpine: Better, but you've got some social ettique lessons coming your way, mister!

Quickfoot: Oh leave the poor thing alone, he's got enough to worry about.

Elmtail: No, no, its fine, but really, we must get- oh crap.

Quickfoot: Oh crap? Whats- **He pauses, turning his ear to the right, then looking around** Oh. Crap.

Dewpine: Eeep! A fox!

Quickfoot: Thats not a fox, its a stoat!

Elmtail: Its two stoats, actually.

Dewpine: You have got to be kidding me. Well, they've ruined my morning, so I'm going to ruin theirs.

Elmtail/Quickfoot: Say WHAT?!
<div style="color: #EE0000; font-style:oblique;">This is completely out of character. Elmtail should agree and give hints as to falling in love with her.</div>
Dewpine: **looking innocent** You don't think I can fight? **Elmtail and quickfoot look at each other** Psh, boys. C'mon, let's get'em!{{%/blockquote%}}
The two stoats are dispatched rather easily, but before the battle phase can end, 5 more join in, which makes things considerably more difficult. After 2 of them are killed, 3 more join in, bringing the total to 10, 6 at once.

{{%blockquote%}}*Dialog* (After battle)
Elmtail: Wow, Dewpine really can fight...

Dewpine: *thrusting her nose in the air* And here you thought I was a helpless doe! Shame on you!

Quickfoot: Yes, but I'm still exhausted. Ten stoats? Insanity.

Elmtail: Uh, we may have a problem.

Dewpine: Huh?

Elmtail: Look behind you.

Quickfoot/Dewpine: **They look around and see a patrol of 18 stoats growling behind them**

Dewpine: EEEEEK!
<div style="color: #EE0000; font-style:oblique;">Every single instance of squeaking in this entire story sounds horribly out of place.</div>
Quickfoot: RUN!{{%/blockquote%}}
Rabbits need no second bidding to bolt, and as the player gains control of Elmtail they are immediately prompted to run, really really fast (via a giant flashing arrow on the screen). Rabbits can bolt at extremely high speeds, so the motion blur is going to start taking effect on this scene. The player must first dash to the edge of the hill, which allows them to put a little distance between themselves and the stoats - which they will need. Then, the view switches to side-scrolling (or side-down, in this case) and the player must hop down a hill at high speeds without going too fast, or they'll trip over themselves (they must also avoid obstacles). The other characters are computer controlled and should not have a problem getting down. There is an autosave halfway down the hill, and when the players reach the bottom they hit another autosave and the following dialog in top-down mode.

{{%blockquote%}}*Dialog*
Elmtail: *gasping for breath* Oh jeez, I can barely breath.

Quickfoot: *pant* Your telling me? *pant* I've never bolted that fast *pant* in my life! *pant* Well, maybe a few times.

Dewpine: Uh, I don't think we're in the clear yet.

Elmtail: **looks up at the hill** They're still after us?!

Quickfoot: Wait a second, Elmtail, if we wait here and let them come to us, they're spread so far apart we'll be able to pick them off one after another.

Elmtail: Your right, **Draws his sword** get ready everyone.{{%/blockquote%}}
{{%blockquote%}}**Minor Boss - The Stoat Patrol** (Irregular)
This is another irregular, topdown battle, except this time they aren't surrounded and must use their positioning to their advantage while fighting the stoats (18), who are all x1.5 stronger then normal.{{%/blockquote%}}
{{%blockquote%}}*Dialog* (After battle)
Quickfoot: Well, I think trying to get there by sundown is out of the question now.

Elmtail: Blasted stoats!

Dewpine: Uuugghhhh... I'm whipped, and its close to midday, isn't it? You two do whatever boys do, I'm going to have a nap. **She lays down on the grass and almost instantly falls asleep**

Quickfoot: ... Definitely out of the question.

Elmtail: ...

Quickfoot: You like her, don't you?

Elmtail: Possibly.
<div style="color: #EE0000; font-style:oblique;">HELLOOOOOOOO TEENAGE WRITING</div>
Quickfoot: **laughs** Well you don't have to worry about any competition from me, I'd rather like to keep my head attached to my shoulders.

Elmtail: You don't think I'd actually do that, do you?

Quickfoot: What? Oh, er, of course not!{{%/blockquote%}}
The player is now free to explore the surrounding countryside, but if they stray too far they will get called back by Quickfoot. There are only minor battles to be faught, however the player may stumble on some interesting trinkets, one of which is a rope of some kind. After he finds the rope, Quickfoot will shout at him to get back, and when he arrives, Dewpine is up and ready to travel. With the party reconstructed, the player sets off in direction Dewpine points them in. This begins a rather long section of traveling with occasional random encounters. Battles included, this should probably amount to about a half hour of gameplay. They will reach a tree in the middle of the field, and make camp there. The following dialog occurs before everyone goes to sleep.

{{%blockquote%}}*Dialog*
Dewpine: So, Elmtail, I'm curious... How did you come to be so handy with a sword?

Elmtail: Well, Alden taught me how to use-

Dewpine: Alden?! The great Swordsman of the Western Forest?!

Elmtail: ...Uh?

Dewpine: Wow, that would explain a lot. Taught by Alden himself...

Elmtail: What did Alden do? He raised me from when I was a kit, that's all I know.

Dewpine: Alden raised you?! That's incredible... Alden is the undisputed swordmaster in the Western Forests, where he single-handedly defended a village from an entire stoat strike team.

Elmtail: Which village?

Dewpine: Dyzentry Village, but its burned down now. No one's lived in it for almost a season.

Elmtail: ...I'm only about a season old, myself.

Dewpine: Your not saying...?

Elmtail: Would the village your talking about happen to be the one that was near your field?

Dewpine: Well I wouldn't exactly call it near, but yes, it was down the path.

Elmtail: ...So I was born in Dyzentry village...

Dewpine: ...

Quickfoot: Can we go to sleep now?

Elmtail: Remind me to stab you tomorrow morning.

Quickfoot: *yawns* ok.<div style="color: #EE0000; font-style:oblique;">Once again, a good quote buried in a terribly immature exchange.</div>{{%/blockquote%}}
The screen fades out, and the back in. But it isn't morning, and the player finds themselves unable to move a tied and gagged Elmtail. Quickfoot wakes up below him, and the player gains control of him, fighting off a dark silloutte that looks like a fox with red eyes (side-scrolling). The other two dark figures run off into the night with Elmtail and Dewpine in tow; the camera view switches back to Quickfoot, and the player must chase after his friends - should the player fall too far behind they will have to try again. Eventually the dark figures stop, tie Elmtail and Dewpine to a tree, and turn to face Quickfoot.

{{%blockquote%}}**Minor Boss - Dark Figures**
Its hard to tell, but the player is fighting against abnormally strong foxes (x3 strong as a simple fox thief). This is a traditional side-scrolling, but difficult, battle<div style="color: #EE0000; font-style:oblique;">Fox thieves as the default enemy? NO SURELY THIS HAS NOTHING TO DO WITH REDWALL.</div>{{%/blockquote%}}
{{%blockquote%}}*Dialog* (After battle)
Quickfoot (as he is untieing them): That was a close call

Elmtail: You can be a pretty vicious fighter.

Quickfoot: What can I say? My friends were in danger.

Dewpine: **shivering** Something was... wrong... with those foxes.

Elmtail: **nodding** I know what you mean, their eyes were creepy.

Quickfoot: At least you weren't fighting them.

Elmtail: I wish I could've.

Dewpine: Those eyes...

Quickfoot: Right, lets get back to camp, and this time, we'll post a sentry.

Dewpine: I'll go first.

Elmtail: **Surprised** You want to go first?

Dewpine: Sure, I'm not going to sleep anytime soon, not with fresh memories of our kidnapping. Maybe you can, rabbit *warrior*.

Quickfoot: **snicker**

Elmtail: **laughs** Oh I don't know, they were pretty terrifying. Come on, let's get back to camp already!{{%/blockquote%}}
The player controls Elmtail as they go back to camp - there are no encounters. Quickfoot and Elmtail go to sleep, and the screen fades to black and into a cinematic.

------------------
**Cinematic:** The moonlight pours through the branches of a solitary tree in the field, a brisk wind blowing through the grass. Dewpine is sitting near the fire, now a burnt pile of glowing coals. She stares at Elmtail's sleeping form with longing eyes, lets out a heavy sigh, and looks up at the starry night sky.

"I miss you, Charles..." she whispers, and the wind carries her words away. Her gaze falls to the ground, and she begins to remember the foxes with red eyes, their terrifying gaze burning against her mind's eye. She shivers, then crouches low to the ground, sniffing.

Elmtail stirs, her soft, quiet whimpers disturbing his sleep. Looking up, he notices Dewpine and quietly hops over to her, nuzzling her cheek. Startled and embarrassed, Dewpine quickly gets back up and turns away from Elmtail, her watery eyes downcast. Concerned, Elmtail hops beside her as she raises her gaze to the moon.

"I loved Charles," she whispered, "and now he's gone, and I can't help but wonder where he is."

Elmtail said nothing, but leaned against her shoulder. The two rabbits stare at the moon in silence, their fur wavering in the brisk wind of the night.

"I love you," whispered Elmtail. Dewpine said nothing, but continued to stare at the moon. Elmtail hesitated, then decided he must have said the wrong thing, and began to return to the campsite.

"Wait," said Dewpine, and Elmtail paused, looking over his shoulder. "I love you too, Elmtail. I just... don't know what Charles would want."

"Neither do I, but since he gave his life for you, I don't think he would have wanted you to be crying all night."

Dewpine sniffed. "Your probably right... But he's gone, and he was supposed to have been my mate. I'm so lonely without him."

They sat across from each other for the longest time, and a dandelion seed floated past, on its way to find a place to sprout a new flower.

Dewpine abruptly hopped over to where Elmtail was sitting and leaned against him. "He probably wouldn't have wanted me to be lonely either."
<div style="color: #EE0000; font-style:oblique;">OH MY GOD, THE STUPID, IT BURNS.</div>
-----------------

Screen fades out and back in. They wake up in the morning, and because Dewpine is still sleeping, the player hops around the campsite with Elmtail, talking to quickfoot and in general just poking around until he investigates the tree. Should this take an extended period of time, Quickfoot, when talked to, will suggest for him to look at the tree. There's a hole, and inside it is a beautiful golden pendent with a sapphire in the center. Elmtail gives it to Dewpine, who happens to wake up as he's examining it, and she puts it around her neck. They now proceed to the river, which is only 10 minutes worth of walking/fighting for the player. 

By the riverside is a small cabin. As Elmtail and his party draw near, a voice shouts out.

{{%blockquote%}}*Dialog*
Unseen voice: Halt!

Quickfoot: Uh?

Unseen voice: Because of recent intrusions on village security, no woodlanders are permitted to cross the river!

Elmtail: Well, that won't do.

Dewpine: Wait, do you know Charles?

Unseen voice: Charles?

**A feline emerges from behind the cabit**

Feline: How do you know Charles?

Dewpine: Well... he was going to be my mate...

Feline: Charles taking a woodlander as a mate?! That crazy rabbit!

Elmtail: He's dead now, so let's not go soiling his memory.

Feline: Dead? What happened?

Dewpine: ...

Quickfoot: Bear attack.

Feline: Oh no... I'm so sorry. Listen, I rather liked Charles, but I simply cannot let you pass.

Elmtail: We need to go to the village in order to learn the whereabouts of Moonclaw.

Feline: Moonclaw?! Why on earth would you need to know where that dreadful monster is?

Elmtail: So I can kill him.

Feline: Are you crazy? He'll have you for lunch! No, absolutely not!

Dewpine: Elmtail is the sole apprentice of Alden.

Feline: Say *WHAT?!* Your... Your the apprentice of Alden? I... I had heard rumors, that is all. Forgive me for doubting you.

Elmtail: Uh.... no problem. Now, would you please let us pass the river so we can get to the village?

Feline: No.

Dewpine: What on *earth* are you-

Feline: Instead, I will ferry you down the river directly to the village's port - its much faster.

Dewpine: Oh.

Elmtail: Many thanks, kind sir, but what is your name?

Feline: I am Calvin, 2nd River Guard of In'thley Village. Pleased to meet you.

Elmtail: And you as well. Now, where is this boat you speak of?

Calvin: Follow me!
<div style="color: #EE0000; font-style:oblique;">What the fuck just happened here?</div>{{%/blockquote%}}
The player follows Calvin up and beside the river, until they come to a small dock. Calvin invites them into the boat, and they set off for the village. This is relatively short, with only a single scripted semi-encounter. Maximum of 2 minutes gameplay, probably less.

{{%blockquote%}}**Area Information: **Greshin River *(Top-down)*
Random encounters: 0% (scripted only)
Effects: Water
Objects: *None*
Setting: The river's center is wide open and easily navigable, but if the player gets too close to a side they will sink.  The only encounter is a scripted band of rats on the edge of the river, who shoot arrows at the player until they are out of range.{{%/blockquote%}}
**4. In'thley**
{{%blockquote%}}**Area Information: **In'thley *(Top-down)*
<div style="color: #EE0000; font-style:oblique;">More Watership Down influences!</div>Random encounters: 0%
Effects: Crowd Ambience
Objects: 
Setting: The river's center is wide open and easily navigable, but if the player gets too close to a side they will sink.  The only encounter is a scripted band of rats on the edge of the river, who shoot arrows at the player until they are out of range.
<div style="color: #EE0000; font-style:oblique;">Why was this copy-pasted from above?</div>{{%/blockquote%}}
The player gets to have the fun of maneuvering the boat into the port, and then the following dialog occurs.

{{%blockquote%}}*Dialog*
Calvin: Welcome to In'thley!

Quickfoot: I'm sure I speak for my companions when I thank you for your generosity.

Calvin: Oh, don't worry abo-

???: *CALVIN!*

Calvin: Uh oh.

Dewpine: Who...?

Calvin: Ted - Port manager.

*Ted, a burly looking fox, steps into view*

Ted: You were under direct orders not to shuttle woodlanders into the city! This is a major security breach!

Calvin: With all due respect sir, this rabbit here is the sole apprentice to Alden.

Ted: Rotten fish! Alden's apprentice would never-

*Elmtail whips out his sword, sprints over to the fox and leaps on top of his head with his sword next to his throat*

Ted: -Guh!

Elmtail: Never do what?

???: Bloody acorns, what is this?

*A squirrel dashes over to Elmtail, who is still perched atop Ted*

Elmtail: Eh? *Jumps off and bows to the squirrel*

Calvin: That, Jack, is the sole apprentice of Alden the swordsman demonstrating how good he really is.

Jack: Wait, did you say...?

Dewpine: Yes, Alden, now by Nature's grace, lets get on with it.

Ted: Er, right, what did you need at town?

Quickfoot: We need to find someone who-

Ted: Wait, if its someone you need, you'd best ask the Yoltur's shopkeeper. He knows all the faces around here.

Elmtail: And where is this shopkeeper?

Calvin: Oh he's on the corner of the main street, just look for the sign, you can't miss it.

Dewpine: Right, thank you!

Ted: Good luck!

Jack: ... I'm still confused.
<div style="color: #EE0000; font-style:oblique;">You and me both, buddy.</div>
Calvin: Oh come on Jack, I'll talk to you in the diner.{{%/blockquote%}}
The player is now directed via level construction to a shop that is unmistakably labeled as Yoltur's Shop. The main road is rather narrow and all the sides are filled with people, and discarded things. They can talk to the denizen's of the town, but most of them will either express anxiousness or an absolute lack of caring. Once inside the store, the following dialog occurs.

<div style="color: #EE0000; font-style:oblique;">The rest of the story was (thankfully) never completed. What follows are bits and pieces of various planned scenes scattered throughout the timeline.</div>
{{%blockquote%}}*Dialog*
{{%/blockquote%}}

**5. New Home**



**6. Wanderer**



**7. The Confrontation**

<div style="color: #EE0000; font-style:oblique;">This occurs directly after Elmtail nearly kills his "evil" brother, who is attempting to explain his actions. This is supposed to be some sort of HUGE REVEAL but like everything else in this story just ends up sucking.</div>
"The mice are very persuasive. At the time, I had already attracted a mate, and had fallen in love with her. The mice said that i had to cooperate with them or they would kill everyone in the village, and torture my mate to death in front of my very eyes. Unfortunatly, cooperating with them involved murdering my own family. At first I felt that I would be forced to watch the village destroyed before me, but they were persistent. They tortured her. They sliced open her veins and I was forced to watch her blood stain the grass. She wept and wept and wept - before long i could take it no more, and... well, you know what happened. I left you for dead, and by some miracle of chance, you fell unconscious long enough for them to consider you dead, but woke up before the cleaned the place out and escaped. When i heard of your existance, i had been their leader for some time. I had almost forgotten what it meant to love someone, because after they confirmed that i had indeed killed my own family, they tortured my mate to death in front of me anyway. convinced i had nothing to live for, i bent to their will.

"I couldn't believe that you were alive. It was either a hoax or a miracle, but I knew what the mice wanted. So, I sent out a scouting party, ordered only to observe and to figure out the situation and if it was indeed true. When only one of them returned, battered, i knew that you were more then the brother i had almost killed all those years ago.

**8. Treacherous Returns**
*Alden is no longer at his camp*

<div style="color: #EE0000; font-style:oblique;">This takes place after the attack on the camp is repelled and dewpine is mortally wounded</div>
The warrior's sword fell to the ground with a clatter. Eyes wide with horror, Elmtail dashed to his mate's side on all fours. "No," he kept saying, "no, no, no, no!" He held her paw, and put his other around her head. Ragged breaths eminated from her mutilated body, and she opened her one remaining eye to look at her loved one.

"I love you," she whispered, and her paw went limp.

Eyes already moist, tears began to flow down Elmtail's matted fur. "Dewpine? Dewpine!" Weeping, the distraught rabbit began to scream. "Why?! Why did this happen? Why must I watch this?!" His head held to the clouds, he could no longer see anything but a grey blur, partially because of the rain, but mostly because of his own salty tears. The great swordsman collapsed in a heap, his head buried in the fur of his mate's corpse. The other rabbits could no longer watch without finding their own eyes watering in grief. Little Speck was crying too, and as she prodded the cold form of her once lively playmate, her mother scooped her up. There wasn't a creature in the whole warren that hadn't shed a tear that night. That cold, rainy, dreary, night, when the clouds themselves wept for them.
<div style="color: #EE0000; font-style:oblique;">IS IT POSSIBLE FOR THIS TO BE ANY MORE DEPRESSING?!</div>


**9. Broken Heroes**



**10. Hidden Secrets**

<div style="color: #EE0000; font-style:oblique;">The actual timing of this event is unknown but it was supposed to occur sometime after Dewpine's murder.</div>
"Why?! Why?! What kind of a stupid question is that? You killed her, god damn it, you killed my mate! We had kits! We were going to raise a litter! We were going to be happy, for gods sake, and you killed her! I'll never be happy again as long as I remain breathing! You fucking son of a bitch, you killed Dewpine and I'll try to avenge her until my body is as lifeless as hers!"
<div style="color: #EE0000; font-style:oblique;">SO MUCH WATERSHIP DOWN INFLUENCE D:</div>
The black rabbit spat at him. "Then die, you worthless pile of droppings."

"Eat my tail, you murderous tyrant."

"Come up here, and that can be arranged."

In a rage, Elmtail leapt up to the bell tower and ascended its walls with powerful kicks in mere seconds. He leapt at his adversary, but the black rabbit had already drawn his sword. The sharp blades ground against each other in the echoing stairwell. Elmtail grunted, leapt backwards, jumped off the bell as it gave off a loud DONG, and lunged at the black rabbit. However, the black rabbit leapt up on to the ceiling and came barreling down on Elmtail, who was only just able to deflect the lethal stab. 




**11. Hellspawn**



**12. Mice of Misery**



**13. Light's Vengeance**

<div style="color: #EE0000; font-style:oblique;">I'm not quite sure when this was supposed to happen but it would have been somewhere around here, depending on how the game split up the action.</div>
Elmtail had finally returned to the warren, so he could see his kits one last time before journeying into what might be his last adventure. As he hopped into the clearing, a kit squealed in delight.

"DADDY!" yelled the kit, enthusiastically jumping into Elmtail's arms, who fell over, laughing. Elmtail held the feisty youngster in the air and nuzzled him.

"My, your so strong!" Elmtail put the youngster back on the ground, and he ran around in circles, growling. Elmtail smiled, "Isn't he adorable, Dewpi-" Elmtail froze in the middle of turning to his right. Dewpine. The rabbit he had loved so much, who had mothered his kits, was dead, and would never see the happy scene before him. Suddenly, he felt tears inching their way out of him, and all the happiness of the reunion was gone in a second. Overcome with grief, Elmtail wandered over to a tree to weep in solitude. No one followed, but many watched. Leethera looked down at the ground, and her mate pulled her close. Meyah was close to tears at the sight of Elmtail's misery. Quickfoot was silent, staring at the trembling form of their leader, their hero.

"And yet he fights for us." Quickfoot whispered.



**14. Terror's Truth**

<div style="color: #EE0000; font-style:oblique;">This occurs after the inner fortress has been breached, directly before the final battle with the hellbeast.</div>
Tidleaf would have been impaled on the stone spike had it not been for Leethera, who set up a magical barrier inches away from the deadly point. Having saved her mate, Leethera turned on his assailant - the High Druid.

"Don't. Touch. Him." She growled, her magical presence so incredibly powerful no creature could come within a hundred feet, and the ground began to crumble below her, stones rising up around her in apparent defiance of gravity. As High Druid began to look somewhat unsettled, she let out a roar and launched a force attack unlike anything anyone had ever seen. It was as powerful - no, more powerful then the one Elmtail had only barely managed to defend against. It tore through the air, rending the ground beneath it into dust, slamming into the druid's barrier like a 10 ton boulder hurled from a tornado. The earth split beneath them, the ground trembling in the wake of such insane magical energies. With a scream of rage, Leethera let loose three fire infernos that came down on the druid like hungry snakes and detonated in a blinding explosion that made the druid's own henchmen stare dumbly in awe.

As the debris slammed into the ground, and the clouds of dust dissipate, a form could be seen. The druid, panting, was still alive. Letheera was taken aback, her rage fading into shock.

"You'll have to do more then that to get rid of me, i'm afraid," the druid growled, and spun around, letting loose a fantastically powerful force attack that tore through Leethera's shield like paper, slamming into her chest and sending her flying. She slammed into the ground and rolled several feet before stopping, apparently either unconscious or too weak to move. Venera took one look at Leethera, shoved a spear through a mouse's gut, and ran over to her side with herbs. Tidleaf, now fully recovered, was half crazy with anger, and struck half dumb in awe at Leethera's bravery, temper, and absurd power. Taking the opportunity, Elmtail finished off his two adversaries with some well placed sword blows, and leapt over to Tidleaf.

"We must get rid of that druid, and we both want to see him in a bunch of lifeless pieces, but we cannot lose our heads, do you understand?"

Tidleaf shook himself back to awareness and nodded with a grim look on his face. "How are we to penetrate his defenses?"

**15. Epilogue**

The battle, while it had been won, came at a terrifying cost. Four were dead, three of them had mates, and one of those mates was Letheera. The mage had laid down her staff, holding her mate's mangled body in her arms. Tidleaf's breath was ragged, and the two of them said nothing to each other for the longest time. Elmtail felt his eyes water at the sight of them, as painful memories of his own lost love were brought back to the surface. How easily it could have been Dewpine holding him in her arms, as Letheera did now, instead of himself.

"I love you, Tidleaf," she said, still cradling his head.

Tidleaf smiled. "I know," he said, and breathed his last. Letheera held him tightly as his life slipped away, trembling in grief.

Elmtail trotted to her slowly on all fours, sitting down beside her. She looked up, saw him, and smiled through her tears. "Well, now i know what it was like for you to watch Dewpine pass away."

Elmtail could only stare at the ground in response. "I never wanted another rabbit to be forced to go through what I went through," he whispered, raising his head towards the other grief stricken rabbits paying respects to their dead mates. "And yet here i stand, watching it happen not once, but thrice in a single battle. I wish we could have won the fight without so much death."

As he looked back at Letheera, she was tenderly laying her mate's lifeless body on the ground. "As do we all. Still, i take solace in the fact that Tidleaf gave his life willingly, and his only regret is that he will no longer be there for me."

Elmtail looked at her with genuine respect. "You are truly a strong doe, Letheera."

She bowed her head and sniffed. "Only on the outside, on the inside i might have died. I don't know what i'll do without him."

"Strength comes from within, Letheera." Elmtail smiled and took her paw. Letheera took one last, longing look at her mate's corpse, then touched noses to Elmtail.

"Thank you," she said, and the two rabbits left the dark cave together, welcoming the bright light of a new day in the forest. Birds song joyously in the air, and squirrels chittered in the canopy. The clouds were like large, fluffy rabbit tails, and the sun shone brightly over the forest, now free from evil. A short distance away, the two rabbits climbed the the edge of a cliff, and enjoyed a stunning view of the ocean, where you could just see a group of dolphins swimming off into the distance. Clearly, the world was now free from evil, a place where they could once again enjoy peace, and live out their lives like rabbits should.

Several months later - It was a busy day in the Rabbit village. Squirrels squeaked in merry laughter as acorns bopped another hapless rabbit on the head.

"Blasted squirrels!"

------------



<div style="color: #EE0000; font-style:oblique;">This was apparently supposed to be some kind of event early in the game, but it never materialized and was left unfinished. Elmtail also wouldn't be able to remember his sister very well, which makes the whole exchange rather odd.</div>
It was late, the moon shining through the clouds, Elmtail, as usually, was quiet, the others talking without him. The swordsman rarely said much, preferring to keep his dark secrets to himself. Dewpine looked over at him, sitting alone on a log in front of the fire. Sighing, she lolloped over and sat down beside him. His eyes never moved from the fire, and it looked as though he was lost in his own thoughts.

"Why don't you ever talk to anyone, Elmtail?" She rested her head on her paws, looking at him expectantly.

Elmtail averted his eyes, paused, then unsheathed his sword and examined it in his paws. "I don't want to get close to anyone. I don't want to lose them like I did my sister."

"That must be painful," she replied. Elmtail looked at her, confused. "Elmtail, without anyone to confide in, without anyone to *trust* in, your not a leader."

"Since when was I a leader?"

"Since you were born. Look, a leader has to trust those who trust in him, or it will all be for naught. Your too distant, you are absorbed with yourself, and you don't-"

"SO WHAT?!" Elmtail stood up, shouting in rage. "Why do I have to 

**D. Characters**
Elmtail is filled with many characters, so the characters listed here are the ones encountered most by the player, either because they joined the player's party or because they are present in the storyline extensively. Other minor characters may be important to the storyline, but all of their character traits can be gleaned from the actual chapter detail.
<div style="color: #EE0000; font-style:oblique;">Correction: Elmtail is filled with 2-dimensional cutouts with the emotional expression of a cardboard box.</div>
{{%blockquote%}}**1. Elmtail**
A young warrior who proves himself to be a fearsome swordsman of unmatched skill. Elmtail was the third son in a litter of four (the smallest died at birth) born to Jestha (his mother) and Lorodo (his father). He will never forget the last lulluby that his mother sung to him when he was a kit:

*"When the rabbit sings tonight, I
know that you'll be curled up tight. And
when the morning bluejay comes, you'll
be the first to run with him. 

"The
days grow older, just like you; They
wither away, like a flower in fall; and
With them, you will grow up strong, the
forest trembling before your might.

"Un-
-til that day, though, rest in peace,
for when it comes, you'll wish you could.
I never thought to know the future, but
I would never doubt you, my dream."*
<div style="color: #EE0000; font-style:oblique;">What the fuck is this shit?</div>
His brother, the strongest of the litter, came the night she sang that lulluby and slaughtered their entire family. For reasons unknown, his brother only knocked him unconcious. When he awoke, the house was in flames, and he was only just able to escape them. He ran until he could run no more, and collapsed in exhaustion somewhere deep in the forest.

Raised as a warrior, Elmtail shows unnatural profficiency with a sword and can dispatch creatures far larger then himself with relative ease. Driven by an intense hatred of his brother, he sets off to avenge his family. He is largely introverted, and is not talkative. He does not kill unnessacarily, but unfortunatly he does not hesitate to snuff the life out of his enemies, either. This is a problem that culminates when he nearly kills his brother as he begs for forgiveness. Only the words of Nate stop him.

Slowly, Elmtail learns that heart is as important as strength in being a hero. After he is devastated by the death of his mate, he finds that he is a hero, leading others on a crusade against the Mice of Misery. Knowing that his mate would have wanted him to lead them against the Mice of Misery because of what the mice had done to other creatures, not to them, he tries to rid his mental faculties of revenge. Whenever someone asked him about what they were fighting for, he always replied "A land free of evil."

Once they defeat the magical hellbeast, Elmtail finds himself in an ironic situation, where Letheera is weeping as her own mate dies in her arms, just as Elmtail's had. The two rabbits, lives entwined by many months of adventure, were brought together by the same tragedy. Letheera agrees to be Elmtail's new mate, and they have a litter together, as Elmtail's surviving children find mates of their own. And so Elmtail lives out his life in peace.
<div style="color: #EE0000; font-style:oblique;">That's not IRONIC it's just FUCKED UP!</div>

**2. Dewpine**
Dewpine was born in the very field that Elmtail found her in, and had a violent childhood. Her mother was killed a week before she and her litter were weaned, resulting in half of them dying from malnutrition. She never knew her father, who, presumably, was a rogue buck with which her mother decided to mate with since she had no other promising bucks to choose from. Forced early on to live on her own, she was saved many times by the watchful eyes of her sole remaining brother and her sisters. As they matured, the litter broke apart, wandering to the hills to find mates, or perhaps a settlement of some sort. She stayed behind, preferring a life in the plains of grass to some village.

It wasn't long before she met Charles, a rabbit exploring the countryside after leaving home in the village. The two instantly fell in love, but before they could take each other as mates, Charles was forced to sacrifice himself in order to save the one he loved. Seeing her mate mauled before her eyes, Dewpine was driven into a stupor, attacking Elmtail later that day.


**3. Alden**
An aged rabbit, Alden is a lone swordsman wandering the forests of Igrathia, where he stumbles on Elmtail's unconscious form. Alden never says much about his past, but it is revealed that he is a legendary defender of Elmtail's hometown.
<div style="color: #EE0000; font-style:oblique;">You know, Alden is a pretty stupid name, now that I think about it.</div>
**4. Quickfoot**



**5. Letheera**



**6. Gorode**



**7. Nitha**



**8. Ranthel**



**9. Meyah**



**10. Moonclaw**



**11. Tidleaf**



**12. Venera**



**13. Nithrot**



**14. Jerla**



**15. Lothro**



**16. Twiter**



{{%/blockquote%}}{{%/blockquote%}}
**V. World**
Elmtail takes place in a world very much like our own, with several key differences. Magic is rare, but it exists, and while it is more of a rumor then well-known knowledge, there are those that practice it. The world has 2 major continents, connected by land. the north is known as Feshonia, the south known as Yatolden. The creatures inhabiting this world are identical to our own, except for the total absence of primates as well as a low number of species. Most creatures have evolved a high degree of intelligence, and several (including rabbits) are in the process of becoming bipedal.

Some animals have evolved vocal chords (Rabbits, squirrels, foxes, felines, stoats, and canines, which are rare), and have adapted a common language known as Fortaln. A sign version of this language as well as a written form exists as well, plus there are several dialects of the language spoken by creatures with massively differentiating vocal chords (Rats and mice), and several more arcane forms for some of the creatures that have no vocal chords whatsoever.

The world is the fifth planet in a crowded solar system surrounding a large main sequence star. An asteroid belt lies where the sixth planet might have been, and it is so close that it creates a visible band of darkness in the night sky, known as "The Evil Ring." Four other rocky planets lie closer to the sun, where they are torched by its intense heat, and 3 Gas Giants orbit outside the asteroid belt. The creatures of the woodland are still in the process of combining their ancient names for celestial objects, but the planets in order of distance to the sun are: Lethia, Mezlia, Beetha, Volnu, Aseb, Kaen (home planet), Yoldalia, Cordincia, and Eanmia.
<div style="color: #EE0000; font-style:oblique;">There was actually an impressive amount of world building going on here. Too bad it sucked.</div>
Scientific knowledge remains widely varied through the species, the foxes using the keen eyesight of birds for astronomical observation, the felines unmatched in agricultural skill, the squirrels working as tool makers, the rabbits experts in biology. The mice and rats seem to be more religiously oriented then the other species for reasons only the rabbits can speculate on, but they exhibit an incredibly understanding of psychology in almost all species.

{{%blockquote%}}**A. History**

3000 BR - Estimated beginning of fox society
2800 BR - Estimated beginning of rabbit society
2038 BR (apx.) - Start of the 8-year war between the hares and the rabbits
600 BR - Estimated beginning of squirrel society
0 - Beginning of recorded history, with the invention of a written form of language by the rabbits.
252 AR - The mice society is officially created
2100 AR (apx.) - The felines are introduced to society.

**B. Primary Species**
Be it divine intervention or just chance, the primary species were the first to adapt society. Despite bending to their natural tendencies, these species have, for whatever reason, managed to put their differences aside for the sake of communication and have coached several of the secondary species into proper behavior.

{{%blockquote%}}**1. Rabbits**
{{%blockquote%}}*Base Stats*
Health: 13
Stamina: 30
Speed: 8/50
Strength: 7
Dexterity: 14
Intelligence: 10
Magic Potential: 12{{%/blockquote%}}
Rabbits are well known for their speed, wit, and reproductive ability. In ancient times, rabbits saw the mating of male and female as an event of the most sacred kind, and their culture encouraged the kits to take advantage of becoming mature at a young age. While primitive rabbits had a complex mating system that combined polygamy with monogamy, rabbit society quickly adapted monogamous relationships. In the late 2400s, the rabbit population became such a huge problem the foxes threatened to remove all restrictions on rabbit hunting if the rabbits did not contain themselves. This resulted in the formation of an unwritten rule, that a pair of mated rabbits would produce only a single litter. This was a success, and the rabbit population was once again stabilized.

The rabbits followed the foxes in their practice of building shelters. Soon there were many "above-ground warrens," or rabbit towns, scattered about the landscape. This created competition with hares, who traditionally build their nests aboveground and were confused when they encountered rabbits doing the same. This culminated in an 8 year war between the two species, the rabbit's technological advantage allowing them to wipe the hares out completely. The foxes and rabbits developed an interesting relationship - out in the wild, they were predator and prey, but in society they often relied on each other for technological assistance and protection against natural disasters. Along with foxes, rabbits were the first to move to what can now be called the modern age. 

Rabbits act like rabbits - they are quick to fear, and quicker to run. Even great folklore heroes followed the old proverb, "those who run away will live to fight another day." They are often witty, and adore the telling of a story. Their dialect of Fortaln has a quick accent to it, spoken with few hesitations, though not as quickly as the squirrels. Either by necessity or chance, the rabbits have adapted as being the middle species, their excellent diplomatic skills often being called on to settle disputes between species.

In ancient times, the rabbits had their own religion based around the sun and moon, however in recent times they have come to adapt the universal religion of most woodlanders - the worship of nature as a spirit, where the dead walk the stream of life that flows through the forest. They do not, however, bury their dead, preferring to preserve the ancient tradition of disposing of corpses in secluded, open areas.

**2. Foxes**
{{%blockquote%}}*Base Stats*
Health: 40
Stamina: 80
Speed: 18/28
Strength: 15
Dexterity: 5
Intelligence: 7
Magic Potential: 8{{%/blockquote%}}
In comparison with the other dominant species, foxes aren't that fast, but they are incredibly strong, and can take a serious beating. Foxes have held a more "sane" view of reproduction, and have not deviated from their monogamous partnerships since ancient times. They have often expressed disapproval and sometimes outright disgust at rabbit's seemingly endless mating urge. While they are predator and prey  in the wild, Foxes and rabbits have formed a bizarre relationship for their mutual benefit. Foxes has strict rules on the hunting of rabbits, leaving their convicts and thieves to do most of the hunting. They were the first to build organized villages, and were pioneers of society as a whole, but are almost completely dependent on rabbits and, especially, squirrels, for their technology.

In the mid-2600s, there was a split among the fox species as to how their society should be organized. One side saw the rabbits and squirrels as inferior species, the other saw them as equals. This broke out into open civil war after years of subterfuge. Unfortunately for the inferior-supportive foxes, the rabbits and squirrels both assisted the equality-supporting foxes, and they were doomed from the onset of the conflict.

A decade ago, in approximately 4989, the rabbits, with the help of several squirrel engineers, managed to stone waterways to bring water into many of their villages. Unfortunately the technology is far too recent, and have withheld the intelligence from the foxes. This has been a point of sore political debate, but most foxes don't really care.

Foxes tend to be sly, though modern society has brought out the virtues of honesty. Despite being experts at managing society and being strong leaders, they often fail miserably at solving their own domestic disputes. Foxes once worshiped the great fox of the stars, but like most other species, have turned towards the universal religion of the woodland.

**3. Squirrels**
{{%blockquote%}}*Base Stats*
Health: 13
Stamina: 10
Speed: 5/35
Strength: 4
Dexterity: 20
Intelligence: 15
Magic Potential: 15{{%/blockquote%}}
Squirrels were the third race to create society, though even today they rarely live in any kind of shelter, preferring the open air of the trees to enclosed spaces. It is not uncommon, however, for squirrels to put a wooden floor on the bottom of their nest. They enjoy leaping from branch to branch and are absurdly good climbers. It is said that the first cross-species playground was formed when a young squirrel kit retrieved a wooden ball that had gotten lodged high in a tree.

Squirrels are well known for their intuition, and respected by all for their ingenuity. Over 80% of modern technology was invented by the squirrels, even if they sometimes do not have the labor necessary to make them a reality. The squirrels originally adapted the written language of the rabbits (the only one in existence at the time) and extended it to include mathematical notation. Almost all abstract mathematics have been pioneered by the squirrels, who have sometimes shown a weakness in the formation of society borne of their naturally free spirit.

An interesting form of selective evolution has caused the squirrel species to increase in size by almost 20%, so that they may harbor larger brains while maintaining their incredibly climbing ability. A less obvious side effect has been an explosion in the number of synaptic connections in their brain, which has raised their required amount of nourishment by a sizable amount. The squirrel culture has embraced this, and they always hold a feast to celebrate an event, which has become something of an attraction to wandering travelers. It has also created an intense reliance on the rabbits for their agricultural ability. Because of this dependence, almost all agricultural technology utilized by the rabbits was probably invented by a squirrel at one time or another.

Being level-headed creatures, squirrels are lighthearted and rarely get into disputes, preferring a quick resolution to their conflicts, and sometimes subjecting themselves to bad deals simply to end an argument. They are almost universally frustrated by other creatures total lack of logic when having arguments, and often try to point out the numerous problems. In the past 300 years, after the first multi-species schools were established, squirrels have become practically ubiquitous schoolteachers, and they have recently been pushing for multi-species communities. This has been met with resistance from the foxes, but the rabbits and mice are already participating in an experimental village with the squirrels.

Squirrels practice the traditional form of the universal woodland religion - oddly enough, they do bury their dead.

**4. Felines**
{{%blockquote%}}*Base Stats*
Health: 20
Stamina: 54
Speed: 14/30
Strength: 13
Dexterity: 18
Intelligence: 8
Magic Potential: 2{{%/blockquote%}}
Felines are relatively widespread and exhibit an incredibly amount of diversity, but they are relatively new to modern society, having only come into play in the third millennia (2100, roughly). Early felines often lived in the villages of the foxes, to whom they were more closely related in terms of anatomy. This started a trend, and while there are now several feline settlements, throughout most of history the felines have lived in the villages of other species, and have usually adapted a very respectful attitude towards their benefactors, even to mice.

While the classic predator/prey relationship is between cats and mice, the mice's uncanny ability to practice magic has enraptured the felines ever since they first saw a mice warlock smash his adversary into a tree, with fatal results. Felines have almost no magical ability, but often scale their attitude depending on how magically powerful a mouse is - To them, a mouse without magic is no better then food. This has had the ironic side effect of creating an even more magically oriented species, and as a result the mice are now considered without a doubt the masters of magical ability.
<div style="color: #EE0000; font-style:oblique;">EVOLUTION!</div>
Felines, being travelers and wanderers, are respectful to any who assist them, but can be violent to those who are stupid enough to disrespect them. They are graceful, and are known for their incredible balance, as well as their fearsome abilities as fighters. They adhere strictly to the universal woodland religion.

**5. Mice**
{{%blockquote%}}*Base Stats*
Health: 6
Stamina: 15
Speed: 7/25
Strength: 2
Dexterity: 19
Intelligence: 10
Magic Potential: 24{{%/blockquote%}}
Mice are a curious species, with relatively moderate intelligence, little strength, but extraordinary magical prowess, partially caused by the feline's influence. The fourth species to join society, they were ostracized by the foxes but quickly found friends with the squirrels, who viewed them as equals. The most prominent result of this was the mice's rapid acquisition of technology, and the subsequent infusion of magic in many engineered devices. Indeed, the mice warlocks often assisted the squirrels with their construction back before the foxes were willing to assist in the construction of the squirrel's designs.

The First Ascension (2503-2580) brought a bizarre set of circumstances upon the mice population. They wanted to bring the rats up from a state of savagery, but some of the rats didn't want to be subjected to the rule of a species they saw as weak. Thus, a war broke out, though it was relatively short in terms of the other species, lasting only a few months. In the end, the rats opposed to The Ascension decided to live out their lives in deep south, where no one would bother them.

The mice are a very proud people for a species of their diminutive size, with most of their adolescents seeking to prove themselves to a feline. If anyone wants to learn how to practice magic, it is the mice that they travel to. Mice practice their own form of the universal woodland religion that is more focused on the magical realm, and bury their dead.

{{%/blockquote%}}**C. Secondary Species**
These are other species that aren't quite as widespread as the other species but still play a sizable part in Elmtail's world. All of these except the Otters were helped by one of the primary species to reach a "modern" level of society.

{{%blockquote%}}**1. Rats**
{{%blockquote%}}*Base Stats*
Health: 12
Stamina: 23
Speed: 9/26
Strength: 3
Dexterity: 17
Intelligence: 9
Magic Potential: 22{{%/blockquote%}}
The first species targeted in The First Ascension, the rats were approached by the mice with an offer of mutual hospitality, and the mice sought to raise their brethren's technological level to match their own. This resulted in a conflict of interest between rats who viewed the mice as weaklings, and rats who saw them as equals or superiors. In the end, those not interested in attaining the mice's technological level simply left for greener pastures - in this case, the southern mountains of Tu'le. These rats, while their numbers steadily shrink, are nonetheless still in existence, and are known to violently react to visitors of any kind.

Civilized rats, on the other hand, are well-integrated with the society of mice, and most are well known for bullying the smaller race at a young age. Mutual respect usually takes over in adulthood, however, and their society remains stable. Rats have, however, had some difficulty in being accepted into the experimental multi-species projects, as they generally are viewed unfavorably by the other primary species. They are currently petitioning the elder council of mice for a role in the multi-species projects.

Rats are natural bullies, and despite their outside behavior, are cowards, except in large numbers. Modern civilization has sought to minimize this behavior, either by eliminating it as a learned behavior or breeding it out of the gene pool. Because of these efforts, the actual personalities of rats can vary widely, from boisterous and rude to kind and respectful. The rude ones usually don't get very far.

Rats mimic the mice's religious tendencies and bury their dead.

**2. Stoats**
{{%blockquote%}}*Base Stats*
Health: 43
Stamina: 62
Speed: 11/27
Strength: 14
Dexterity: 9
Intelligence: 6
Magic Potential: 10{{%/blockquote%}}
Stoats were the other race targeted in the First Ascension, and were a complete failure in terms of societal values. Thieves, tricksters, and an overall unpleasant species, the stoats are rarely seen helping one another, much less someone of another species. Their favorite food is, unfortunately, rabbit, which has caused a significant amount of friction between the two species. Usually, headstrong rabbits will attack a stoat on sight, and vice versa. Some call it the Demi-War, since it seems to be an unending conflict that's always just below the surface.

Foxes have good relations with stoats, and while most don't necessarily approve of their theivery, they maintain trade with them, usually in some sort of peace agreement. This trade prevents all out war between the rabbits and stoats, but the agreement hangs in a delicate balance, and the foxes must be vigilant about keeping it stable.

Stoats practice the universal woodland religion but rarely bury their dead, because their dead are usually in enemy paws.

**3. Bluejays**
{{%blockquote%}}*Base Stats*
Health: 26
Stamina: 18
Speed: 2/82 *(walk/fly speed instead of walk/run speed)*
Strength: 6
Dexterity: 7
Intelligence: 11
Magic Potential: 4{{%/blockquote%}}
The Second Ascension (2944-3006) occurred when the Squirrels approached the Bluejays about becoming part of contemporary society. The squirrels had taken an interest in the flying creatures, and the bluejays had proven their intelligence multiple times. In a famous moment, it is said that the squirrel elder of the council explained, "The bluejays, frankly, are the only ones who aren't birdbrains." That didn't go over very well, but in the end the bluejays were successfully added to society and are the only secondary species to have formed their own settlements.

The bluejays are so useful in their ability to fly that it is predicted they will become the sixth primary species. The bluejays are currently investigating several technologies with the squirrels on flying improvements and other methods to take advantage of their wings. Despite their widespread success as part of modern society, bluejays do not practice the universal woodland religion, instead opting for a modified version of their own ancient religion. They are very secretive about their religion, and the mice have so far only deduced that it involves the worship of the clouds.

Bluejays are witty speakers, sometimes at the expense of others, but their good sense of humor usually gets them on the good side of everyone. They have come up with many creative uses for their beaks and claws, as they lack any sort of opposable thumb or other such mechanism. When threatened, a pack of bluejays has been known to peck their victim until they die of blood loss.
<div style="color: #EE0000; font-style:oblique;">Chrome doesn't think opposable is a word :C</div>
**4. Canines**
{{%blockquote%}}*Base Stats*
Health: 57
Stamina: 65
Speed: 17/32
Strength: 17
Dexterity: 10
Intelligence: 7
Magic Potential: 1{{%/blockquote%}}
Canines are almost completely feral creatures, and are extraordinarily rare. Most live in the northern plains, but are rarely seen, preferring to stay secluded for reasons unknown to contemporary anthropologists. When they are encountered, they will usually either hide, or violently attack anyone unfortunate enough to catch sight of them. Known for their incredible strength and absurd hardiness, it takes a trained team of warriors to take down a canine, and they are considered extremely dangerous. However, they are so hard to find, and almost never encountered, so no strong extermination attempts have been made.

Little is known about the canines, and mice psychologists have no idea what religion they practice, nor what their social hierarchy is.

**5. Otters**
{{%blockquote%}}*Base Stats*
Health: 20
Stamina: 35
Speed: 9/20/36 *(walk/run/swim speed)*
Strength: 13
Dexterity: 12
Intelligence: 9
Magic Potential: 8{{%/blockquote%}}
Otters are a secluded species, living in or near the rivers in which they make their dams. They have been relatively resistant to modern society, but practice strict social rules and are never unnecessarily hostile to visitors. Otters have made their intentions of staying away from technology and society in general perfectly clear multiple times, so most species merely ignore them. The rats and mice, however, maintain a strong trade relationship with them.

The otters practice the universal religion of the woodlands and bury their dead. In battle, however, fallen warriors are left in open air in respect to ancient traditions.

{{%/blockquote%}}**D. Classes**
While a creature's species determines their base statistics, personality, and culture, it is a creature's class that determines how those statistics grow and what skills the creature can learn. Usually a species will favor one class over another depending on which one is more beneficial to them. The stat modifiers are the the percentage of skill points allocated to each stat, in decimal form.

{{%blockquote%}}**1. Savage**
{{%blockquote%}}*Stat increase modifiers*
Health: x0.3
Stamina: x0.2
Speed: x0.1
Strength: x0.3
Dexterity: x0.05
Intelligence: x0.05
Magic Potential: x0{{%/blockquote%}}
Savages are so named for their sole reliance on natural weapons, and usually, a complete lack of training. All species possess the innate knowledge to defend themselves, and an advanced savage puts this knowledge to its full potential, dealing vicious amounts of damage at the cost of accuracy and dexterity.

**2. Swordsman**
{{%blockquote%}}*Stat increase modifiers*
Health: x0.25
Stamina: x0.1
Speed: x0.15
Strength: x0.25
Dexterity: x0.15
Intelligence: x0.1
Magic Potential: x0{{%/blockquote%}}
A Swordsman is trained in the use of a sword, and are known to be blindingly fast. A sword in a Swordsman's paw is a weapon of death, dealing out punishment without end. The battle between two master Swordsman is an event that creatures will travel miles to see, and the ultimate fear of any thief is to accidentally jump a Swordsman.

**3. Fighter**
{{%blockquote%}}*Stat increase modifiers*
Health: x0.2
Stamina: x0.15
Speed: x0.1
Strength: x0.25
Dexterity: x0.2
Intelligence: x0.1
Magic Potential: x0{{%/blockquote%}}
Much like savages, Fighters primarily rely on their natural weapons, but focus on punches and kicks instead of claws. They are trained to fully utilize all of the muscles in their limbs, and know where to strike an opponent in order to deal the most damage.

**4. Archer**
{{%blockquote%}}*Stat increase modifiers*
Health: x0.2
Stamina: x0.05
Speed: x0.1
Strength: x0.15
Dexterity: x0.3
Intelligence: x0.2
Magic Potential: x0{{%/blockquote%}}
Archers are trained in the use of a bow and arrow, which was a rather recent invention coming from the squirrels. As a result, most archers are squirrels, but there are exceptions. Archers tend to rely solely on their bow and arrow, but are capable of defending themselves in melee combat.

**5. Mage**
{{%blockquote%}}*Stat increase modifiers*
Health: x0.15
Stamina: x0.05
Speed: x0.2
Strength: x0.1
Dexterity: x0.1
Intelligence: x0.2
Magic Potential: x0.2{{%/blockquote%}}
The mage is a class dedicated to the study of magic and casting it into spells. High level mages create new spells to suit their needs, and most are known to use magical augmentation on many of their stats. Mage's tend to be physically weak, but this is offset by their incredible magical power.

**6. Warlock**
{{%blockquote%}}*Stat increase modifiers (percentage of skill points that go into the stat)*
Health: x0.2
Stamina: x0.15
Speed: x0.15
Strength: x0.1
Dexterity: x0.05
Intelligence: x0.1
Magic Potential: x0.25{{%/blockquote%}}
Warlocks are what one would call the "Savage" version of a mage, tuning into the raw magical energies flowing through the physical realm. Much more physically capable then mages, Warlocks often attack their opponents like a savage, only to surprise them with a blast of magical energy. Warlocks lack the ability to cast any kind of spell, however, and the uses of untamed magical power are limited, even if it is extremely powerful.
<div style="color: #EE0000; font-style:oblique;">This was basically the "blow everything up" class.</div>

{{%/blockquote%}}{{%/blockquote%}}
