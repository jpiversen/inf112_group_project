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
To enhance replayability, we developed a system to dynamically generate maps in our game. One of the requierments of the
project was to be able to create maps from strings, so a logical step to this was to first dynamically create the strings
then create the map based on said strings. We settled on a three method aproach, where the TileMap-class was responsible
for converting the StringMap-objects into tiles that could be used by the gamehandler. The StringMap-class was responsible
for putting together pre-made sections of maps into a full mapString. The pre-made sections were divided into either named
-rooms or numbered-rooms. The funtion of named-rooms was to ensure we always had a spawn location for the player, and a way
to get to the next map. It also allows for later adding specialised rooms, such as bosses, but we ended up only expanding this
function to include test-maps. The numbered-rooms were sections intended for random selection, and a StringMap would always
consist of a startingRoom, an endRoom and a number of randomly selected numbered-rooms. In order for the StringMap-class to
know where to place the rooms however, it needed a GridMap-object. The GridMap-class created a Grid, always starting with the
spawn-room of the player, and then randomly picked a direction (up, down, left, right) and placed a random room. Then it continued
this behaviour, randomly picking a location adjacent to a previously placed room untill it had placed all the random- rooms before
placing the end-room in the same manner. This grid was then the basis of the StringMap, which was the basis of the TileMap.
In addition to the actual tiles, the map also included spawn locations for items, enemies and the player character.

