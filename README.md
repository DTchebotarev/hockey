# NHL auction helper 2018

This github repo contains my work for creating a simulator to calculate valuations of individual players for a hockey pool auction.

# Table of Contents
* [Motivation](#motivation)
* [Where it is today](#where-it-is-today)
* [Usage](#usage)

#### Motivation
For fun, I decided to participate in a work hockey auction.
The rules were simple (detailed below), but as very much not-a-hockey-guru I knew I needed some help.
Laptops were not allowed at the auction, but thankfully phones were, so over the weekend leading up to this auction, this project was born.


#### Where it is today
After not touching this project for some time, I have decided to go back to it and make it the best that it can be, because there's always next year!

The auction rules are simple:
1. The pool of players is decided ahead of time, with goalies and injured players removed to simplify things. It's also pruned down to have about 6-7 players per person participating in the auction.
2. Each auction participant starts out with $1000.
3. During the auction, the auctioneer randomly picks a player, and a first-price auction begins.

The structure of the project is as follows:

1. individual_forecasts.ipynb contains the per-player predictions. The output is a pickle which contains the amount of playoff points per game which the model expects each player to score (Expected PPG). At the moment the model is a simple linear model.
1. bracket_simulations.ipynb contains bracket simulations. This uses random draws to simulate different bracket outcomes. This is replicated in the webapp python file linked below.
1. https://github.com/DTchebotarev/webapp/blob/master/hello.py contains a Flask web app which computes dynamic valuations of each player's value.

Value is calculated as follows:

1. Calculate total money left in the pool.
1. Divide by the total amount of points left in the pool in total.
1. Result is the $/point.
1. Multiply currently auctioned player's expected points by $/point to get a valuation in $.

The calculation of expected points for each player is not trivial.
The expected points per game for each player is pre-calculated and fixed.
However, their total contribution to the roster is a function of the likely number of games they get to play, and also where their team is in the bracket, relative to my existing roster.
For example, in 2018 Toronto played Boston in the first round.
So, if I already had 4 players from Toronto, a Boston player would be inherently less valuable to me, than, for example, a Winnipeg player that won't meet any Toronto players until the final.*

More precisely, to calculate the marginal points for a player, assume I have a set of players A that's my current roster.
Imagine a set A' that's the same as A, but with the addition of some player.
I then simulate 1000 NHL playoff brackets, using ELO** scores to get probabilities of wins, and calculate the expected number of points that I earn with rosters A and A'.
Then the difference (A'-A) is my valuation for that player.

\* I didn't consider this carefully then, but this really depends on which quantile of the distribution of points for your roster you're trying to maximize.
If you pick players from teams that face each other early on, you guarantee that at least some part of your team moves on to the next round.
Ideally I should make a conscious decision to maximize some quantile of the distribution of points, and model it explicitly.

\** I use precaculated ELO scores, but in the future I'd ideally either calculate my own ELO, or potentially use a system like GLICKO2, or somehow aggregate up individual players' skill ratings to predict team outcomes.

#### Usage

This was initially designed to be used by two people, one of whom selects the player that's currently auctioned, and the second does the fun part -- the bidding.

The flow is as follows:

1. Person A: Select which player is to be auctioned via the "data entry mode" interface.
2. Person B: See the player valuation via the "bidding mode" screen, make appropriate bid.
3. Person A: Record the sale, get redirected to Step 1.

The roster at any time can be viewed with the roster link. 


#### Future goals

1. Roll in the repo at https://github.com/DTchebotarev/webapp. Really most of the code is there, but this repo had the better name so I'm moving everything here.
1. Change the name of the web app to something other than ``hello.py``
2. Some persistence would be nice. Currently everything is in memory, so if there's a crash, all data is lost.
1. Add some tests, set up CI/CD.
3. Add an interface for the auctioneer, as well as other bidders. Not only does it make data entry easier, but it allows everyone to have a concurrent view of the state of the auction.
1. Make it pretty.
