# binance-trading
About trade features
Prices and volumes
Binance, unlike many exchanges, strictly regulates lot sizes and price orders. You can’t buy an arbitrary amount of currency at an arbitrary rate - there are restrictions for each pair that must be followed when creating an order.

There is a price step - for example, for a pair of NEOUSDT the price should be a multiple of 0.00100000. You cannot place an order at a price, 0.1234 - you can either 0.123 or 0.125.

There is a step of sell / buy coins - for example, for a pair of NEOUSDT, a step of volume of 0.001 - so if you can’t sell or buy 123.45678 - you can either 123.456 or 123.457.

Well, and like everywhere else, you cannot create orders less than the specified volume.

These restrictions can be obtained through the exchangeInfo api method (details about the operation of the Binance API here ), the necessary information is in the filters section for each pair. For the price it is tickSize, for the volume stepSize in the corresponding data structures.

The bot takes these restrictions into account, but pay attention to how the bid price changes:

Let's say you are going to trade at 11 USDT.

The bot receives prices from a glass - for example, 5 prices - [118.753, 118.750, 118.730, 118.712, 118.704]. It takes the average -   118.7298. Because the minimum price step is 0.001, then the price is taken 118.729 (down).

After that, the bot calculates the amount of currency that can be bought at this price - it divides 11 USDT by 118.729, gets 0.092648. Because the minimum step on coins is 0.001, then the amount of 0.092 is selected.

As a result, the bot buys 0.092 NEO at the rate of 118.729 - and the total trading amount will be 10.923068. This is less than specified in the settings, but the bot is forced to adapt to the requirements of the exchange.

the bot will sell in such a way as to obtain 10.923068 + the desired percentage of profit.

In general, this is good - because Binance gives a discount. Paying a commission through BNB, you pay 50% less. Theoretically, if the commission is 0.1%, then paying the commission in this way, you pay 0.05% from each transaction. The bot is designed primarily for such a commission, and it is recommended to use it. If suddenly you want to trade with the usual type of commission, then go to your profile and check the box:

USE_BNB_FEES = True

On the

USE_BNB_FEES = False  

True, I do not see why you should do this. In any case, if you have not previously traded on Binance
