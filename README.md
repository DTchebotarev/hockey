# NHL auction helper 2018

This github repo contains my work for creating a simulator to calculate values of individual players for a hockey pool auction.

#### Motivation
As not-a-hockey-fan participating in a hockey auction, I knew that I needed every available crutch, so this project was born.

#### Where it is today

After not touching this project for some time, I have decided to go back to it and make it the best that it can be, because there's always next year!

The structure is as follows:

1. individual_forecasts.ipynb contains the per-player predictions. The output is a pickle which contains the amount of playoff points per game which the model expects each player to score (Expected PPG). At the moment the model is a simple linear model.
1. bracket_simulations.ipynb contains bracket simulations.
1. https://github.com/DTchebotarev/webapp/blob/master/hello.py contains a Flask web app which computes dynamic valuations of each player's value.

Value is calculated as follows:

1. Calculate total money left in the pool.
1. Divide by the total amount of points left in the pool in total (sum of all players' expected PPG).
1. Result is the $/PPG.
1. Multiply currently auctioned player's PPG by $/PPG to get a valuation in $.

#### Goals

1. Roll in the repo at https://github.com/DTchebotarev/webapp. Really most of the code is there, but this repo had the better name so I'm moving everything here.
2. Some persistence would be nice. Currently everything is in memory, so if there's a crash, all data is lost.
1. Add some tests, set up CI/CD.
3. Add an interface for the auctioneer, as well as other users. Not only does it make data entry easier, but it allows everyone to have a concurrent view.
1. Make it pretty.
