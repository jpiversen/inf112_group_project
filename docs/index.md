# Rodent Reboot development blog

We have made a game called Rodent Reboot where the player’s goal is to progress as far as possible into new randomly
generated maps filled with enemies which attack you.

## System Design?
We had some initial requirements we had to build around and that was adhering to a MVC structure. MVC means
model-view-controller and it’s a classic way to separate components of games. For simple projects adhering to this form
the design is very straight forward,but for a larger project like this we need to think more carefully about how to go about it.
Especially the connection to LibGdx which was a graphical library which we used turned out to be somewhat challenging.
For this reason, we took a quick departure from the classic form TDD (test driven development) early on where it’s
normal to start without a comprehensive design for how the elements fit together. Rather we sat down and spent time on
designing how the components fit together before starting the development of the components themselves. When that
process was done our workflow looked more similar to TDD for the components themselves.


## Dangerous Enemies?
We chose to have a rather simple enemy design where we have enemies which don’t have complex path finding algorithms,
instead they continually look for the player and seen they will remember the position they saw the player in and move
towards it. Initially we then had a problem where enemies could easily get stuck into walls using this approach,
but it improved a lot when we allowed the enemies to slide along walls if moving diagonally into one. This would make
sure that if you move behind a wall and continued in a line that the enemies wouldn’t just get stuck chasing you.
When we had finished optimizing everything this simple enemy movement system felt relatively good where you could even
trick enemies by having them see you in one position and you moving up behind them to shoot.

## Dynamic map creation?
To enhance replayability, we developed a system to dynamically generate maps in our game. One of the requirements of the
project was to be able to create maps from strings, so a logical step to this was to first dynamically create the strings
then create the map based on said strings. We settled on a three method approach, where the TileMap-class was responsible
for converting the StringMap-objects into tiles that could be used by the gamehandler. The StringMap-class was responsible
for putting together pre-made sections of maps into a full mapString. The pre-made sections were divided into either named
-rooms or numbered-rooms. The function of named-rooms was to ensure we always had a spawn location for the player, and a way
to get to the next map. It also allows for later adding specialised rooms, such as bosses, but we ended up only expanding this
function to include test-maps. The numbered-rooms were sections intended for random selection, and a StringMap would always
consist of a startingRoom, an endRoom and a number of randomly selected numbered-rooms. In order for the StringMap-class to
know where to place the rooms however, it needed a GridMap-object. The GridMap-class created a Grid, always starting with the
spawn-room of the player, and then randomly picked a direction (up, down, left, right) and placed a random room. Then it continued
this behaviour, randomly picking a location adjacent to a previously placed room until it had placed all the random-rooms before
placing the end-room in the same manner. This grid was then the basis of the StringMap, which was the basis of the TileMap.
In addition to the actual tiles, the map also included spawn locations for items, enemies and the player character.

## Item system?
In the designing phase, we decided that 5 items was 
the magic number to have as less would be challenging and more would be too overpowered.
However, since the number of items spawned would depend on the randomized map system, we had to introduce some 
flexibility. To achieve this, an algorithm was created in which the GameItems class retrieves the spawn locations from 
the TileMap class and starts by shuffling the items locations. If there are less than 5 spawn locations (the 
aforementioned magic number), all the spawn locations would receive a power up item. If there were more spawn locations 
than 5, the first 5 would be selected for an item and the rest would be given a 50% chance of spawning an item. Now that
the locations that will spawn an item has been selected, a random power-up was chosen per location. 

The power-ups offered in Rodent Reboot are the following:

ArmorPU - Reduces damage taken by the player for the next 10 hits

BulletSpeedPU - Temporarily makes the player bullets faster

FullHealthPU - Refills player health

HealthPU - Gives the player 10HP of health

SpeedPU (1-3) - Speeds up player moment for a period of time. 1 is shortest and 3 is the longest. 

The selected locations and item types per map are then passed onto the Model class which used a Strategy Design Pattern
to carry out the power up effect per item type that the player interacted with. 
We wanted the power-ups offered in game to be varied and exciting, we think we achieved that and hope that the player
finds the randomization exciting and fun to play with!

## Custom art work  
![In-game screenshot](assets/screenshot.png)

We were very lucky to get help with the creation of sprites on our project which gave the project a coherent
visual theme which made it so much better. Relying purely on publicly available sprites would make it difficult
to have one distinct theme.

![menu file](assets/menu.png)

Made by Evangelia Koutsoukou aka Apocalypse, specifically for this project:

[Her Facebook Page](https://www.facebook.com/EveOfTheApocalypse)



