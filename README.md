**Finished #103 Global**

****Wood1:****

```
Since all the 4 minigames were hurdles the entire game state could fit in dp[30][30][30][30][3][3][3][3]
As the number of positions in hurdle is 0-29 and stun duration is 0-3 and each state has exactly 1 best move
I never make DOWN movement as it does the same thing as jumping.
As the number of states was too high, I reduced the number of states by starting my initial position in every turn as 0 ignoring the positions I already crossed
I only care about the games at a depth of 5.
In other words I always start from position 0 in every mini game and each of my mini game have a maximum length of 5.
At a depth of 5 the no. of states required just looks like dp[15][15][15][15][3][3][3][3].
As there are multiple paths to reach a particular state we also need to know how many turns we have taken before to reach a particular state.
As I'm looking at a depth of 5 my dp state looks like dp[15][15][15][15][3][3][3][3][5].
```

****Bronze and Silver:****

```
After a lot of rewrite, I somehow managed to fit my DP for 3 games ignoring skating (as skating only requires 1 depth simulation)..
My dp state looked like dp[12][3][41][41][11][5][4][4].
[12][3] is for hurdles at depth 4
[41][41] is for the archery
[11][5][4] is for the diving.. At depth 4 we can have a maximum points of 1+2+3+4 = 10 starting at 0..
Maximum combo starting at 0 would be 4.
The last 4 to know which turn I first chose to skip diving.. I need this to add the initial combo till this point.
I used these 3 dimentions for DIVING instead of 2 dimensions to save storage space.. 
The last dimension is for the depth of 4
```

****Gold:****

```
My dp was super easy to work with and debug. So I sticked with depth 4 dp for a while longer.
Once I was sure all my simulations are perfect and I had a good scoring function I switched to all possibilities at depth 8.
This quickly took me to top 50.. But I dropped behind to rank 200 quickly in the next 2 days as everyone were super competitive.
Then I fought back with a better opponent prediction and evaluation which looks something like this.

I gave super powers to both the opponents. The super power being my opponents able to play different moves in each mini game.
So I choose the best move for each opponent in each game using a DP to solve Hurdles and another DP to solve Archery.
Perfect play for diving is easy to calculate with a formula.
This way regardless of which minigames opponent chooses to play his best I would be able to win a decent number of medals with the remaining available options.```
```
****Evaluation****

```
I computed gamePoints[p][g] = goldWins[p][g] x 3 + silverWins[p][g] where p is player and g is game..
I added a score of 1000000000 * product of gamePoints[player_idx][g] + 2..
This automatically prioritizes the game I have good chance of winning gold first as well as increase my overall score by choosing a game that I have less medals already.

Tie breaker 1: (when there are multiple games to choose from).. I added a score of 100000000/pow(number_of_turns_remaning[g],2) to focus on games that ends sooner.
For hurdles distance/3 should give the number of turns remaining.

Tie breaker 2: I add a bonus score in every minigame where I beat my enemy.
I choose my enemy based on my position say if I'm at rank 1 I focus on rank 2 guy and if I'm rank 2 I focus on rank 1 guy.. when I'm rank 3 I focus on rank 2 guy.
I don't consider both the opponents as my enemy because the enemy of my enemy is my ally.

Tie breaker 3: Just the sum of position/score in each mini game.

On the last day of the contest I added skating simulation for all 8 turns assuming I always move 2 steps in future regardless of the move I choose.
I choose the above same behavior for my opponent in skating.
This is except the first and last turns where it's super ideal to take a lot of risk.
Taking a lot of risk on first turn actually helps in getting ourselves stunned and focus on other mini games more.
Additionally getting stunned also this blocks a location to be unusable by others.
Jokes ahead. I only take a lot of risk on the first turn only if I don't get stunned (i.e. my currrent risk is < 3).
```

****What went wrong:****

```
1) Giving too much super power to opponents make my bot to play less aggressive (passive).
I take a super safe approach not challenging opponents when I'm lagging in a mini game.
I tried giving 1 less depth on all games for my opponents to play a little more aggressive.
That helped in keeping opponents in check and capitalize on games they make mistakes..
This helped in climbing the ladder faster but ranks stopped climbing at the higher stage of the ladder.

2) THE SKATING: I didn't simulate the addition of risk factor for two players in same position in skating.
This is because the opponents are super unpredictable in skating.
I could have solved this if I had simulated all combination of moves for opponent at least on the first turn as it's impossible to predict the future collisions.

3) Lack of depth => 8 depth is very low considering mini games are 15 depth long.
I believe an accurate simulation of at least depth 15 is needed to reach Legend and I couldn't find an effective way to prune my paths.

4) I could have created a Score class with all my tie breakers and sorted the scores..
I could have scaled with more tie breakers making it easy to make decisions between two different game states.
Instead I combined all my scores with c1 x factor1 + c2 x factor2 + c3 x factor3 where c1 > c2 > c3 which may not have been perfect.
```

****Feedbacks:****

```
* The olympics theme was super fun
* More minigames like Running, Javelin throw, Triple jump etc. would have been fun especially because it was super easy to simulate the games..
* In the minigame input if we had MINI_GAME_TYPE before each mini game data would have been even more fun with random games spawning at random locations..
* Captchas and IDE testing limit was super annoying provided that there is no officially supported tool for local testing
* It was super cool just having the need to beat the boss in wood leagues
* Contest duration of 14 days was perfect.. I was able to implement all my ideas in the contest till I ran out off ideas.
* Fiverr gigs looks amazing!!
* Can we have Tshirts for top 103 ranks instead of top 10 please.. 
```
