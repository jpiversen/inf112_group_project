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