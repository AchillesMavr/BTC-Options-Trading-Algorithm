# BTC-Options-Trading-Algorithm
This repository contains an algorithmic trading system for BTC/USD  options. It includes data formatting (for quotes) and the main trading algorithm.

## File Structure

- **`DataFormat`** (replace with actual filename)  
  Handles data preprocessing and formatting to ensure the main algorithm receives clean, structured data.
  Read files taking from Tardis (Deribit allows us to get the option quotes for the first day of each month for free -- we make use of that).
  We format the files so only ATM options are considered (this means we mainly care about 1D volatility curve w.r.t. time expiration --- no 2D curve including Strikes needed for our trading algorithm).
  We format the files so that only options that expire within the same month are considered (we have less quotes and we avoid the same quotes being traded for consecutive months --- If allowed the main code below would need to change).



- **`Option_Trade_Algo_BTC`**  
  All the the algorithmic trading is implemented here. 
  You may need to import certain libraries (e.g. cctx, tardis) and other standard imports. For the two mentioned libraries the second "cell" in  `Data_Format_Utility.ipynb` includes how to import them.
  We develop a trading algorithm to trade BTC/USD options, backtesting for 3 years against the market.
  We mainly used two large ideas for calculating our fair volality (crucial if we want to trade with profit).
    I) Historical volatility: using information for the past 30 days.
    II) Implied volatility (with or without dynamic Delta hedging): Here basically we found the general implied volatility of the market using a cumulative average (or exponential weighted average), updating the volatility as new quotes come in.
                                                                    Repeating, our volatility guess adjusts and reacts to the market. Then, using B-S model we form a bid-ask spread and trade when appropriate.
                                                                    We also perform a daily dynamic Delta hedging --- so that we remain Delta neutral.  
  





## How to use

- Once the datasets are downloaded (either using the DataFormat.ipynb file or downloading the dataset), one can simply run the `Option_Trade_Algo_BTC.ipynb` notebook step by step (cell by cell). Some intruction and theory behind the trading algorith are also included.
- The main code is the  ___main()___  function. Arguments include:

    - yearList: a list for which years we want to trade for.
    - monthList: a list for which months we want to trade for
    - TradeType: Historical or Implied (for the implied look at ImpliedVolTrading() function, where one can change using cumulative av. or exponential weighted average by commenting or uncommenting properly) 
    - spread: the spread we make arounf my market to trading against other traders.
    - longPositionMax: Amount of long position we allow.
    - shortPositionMax: Amount of sort position we allow (more dangerous).
    - rfr: the risk free rate (list).
    - deltaTrading: True for daily dynamic hedging. 


## Results

See towards the end, the section Results in the main code `Option_Trade_Algo_BTC.ipynb`. 

We see that our code proves to be **profitable**. Specifically the implied volatility code with dail dynamic hedging seem to work better, since the profits/losses seem to be less volatile, which is good for a live market.
