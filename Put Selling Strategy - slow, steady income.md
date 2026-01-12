Can hold balance of account in
	possibilities:
		50% SPY, 50% cash
		70% SPY, 30% cash
		VGSH (1-3 year treasuries - pays about 1% per year)
		other income bond funds?
		position size about 1% of BPU
		calvenolear:  TLT, 
		
Hedging:
	Buy 7DTE, 10% OTM SPY long puts for 2% (annual) NLV every week. So spend 0.038% of NLV every week.
		$700,000 * 0.00038 = $266
	VIX hedge - every month when rolling, buy 10 delta 120 dte for 0.008% of portfolio

Buying Power utilization

	
	margin maint/NLV				57000/185000 = 31%

	VIX		Reg-T Margin		Portfolio Margin	
	40+  	50% max		33.5% max
	30-40, 	40% max		27% max
	20-30	35% max		23% max
	15-20	30% max		21% max
	10-15	25% max		17% max
	

Another person
	40+ 	no more than 50% BPu and I religious trimming positions to stay under 50% BPu.
	20-40	uncomfortable above 40% BPu
	15-20	around 25% BPu but up to 30% before I start trimming

https://www.reddit.com/user/calevonlear/


Plan:
	Keep the overall short positions to about 15% of net liqudity. Increase later once I get this down.  (this is where you look at varying things based on VIX for example).

	Keep individual positions to about 0.5% of net liquidity, so that would be about $3500 per position.  Perhaps 20-30 positions.
	
	(can raise this later to higher % of net liqudity, and adjust position size so that may 
	






Here's the idea:

- On red days, sell puts.  Basically at the money - either one below or one above - whichever has more extrinsic value (for more premium).
		- example - stock at 59, look at 55 and 60, which has delta closer to 50? That's probably the one with most extrinsic value (probably the one at 60)
	always trade the monthly that is > 44 days out  (ie, when 3rd friday in may is 44 DTE, then switch to june)
	
- Immediately set a buy to close order at a 25% profit (ie, if STO price is $1.00, buy it back at $0.75).

- If position  makes 10% on the day I open it I close it out. 
- The day after I’ll close at 15%.

- On last friday of the month (which is 21 days before the monthly date where you sold the option), if the pre-entered order didn't happen, decide what to do.
	- Winners - buy to close
	- Losers that are less than 20% in the red - buy to close
	- Others - roll to same strike in next month

Another way to do it (if you can't stay away from a stock and want to just efficiently milk it)- if still is still below 100 day moving average (or you think it has a lot more upside) can roll the put down a strike as soon as next one down has delta = 50.

	"On the last trading Friday (or Thursday in case of holiday) of the month I will roll all of my positions in the April cycle to May. The last Friday of the trading month is 21 DTE, so use that as your target. I always roll to the same strike, no matter what is going on with the position."
	When the last Friday of the month rolled around, look at all remaining positions. Take down all winners that didn't hit 25% and everything that is 20% or less in the red. Roll the remainder at the same strike to the next month, calculate net premium received from the original put and the roll and set a BTC GTC for that so I can exit at a scratch.

Regarding buying power:
	For cash secured - uses full amount of buying power!
	I personally do 30-40% when VIX > 23.5 and decrease in tiers. 20-30% when VIX is 14 - 23.5 and 20% when VIX is < 14. The reason for this is when volatility is low things are calm and predictable. You will be closing out positions rapidly and don’t need as much ammunition. Save it for when thifngs spike. When things are volatile you are receiving large amounts of premium and banking vega in the subsequent volatility crash. You won’t need as many positions to hit the same premium targets.
	
	
How many stocks?	He does 30-50 positions

	
Finding stocks:
	Whatever comes up on my screen that gets a decent tipsrank (7-10). I don’t have time to do DD because I take the shotgun approach of 30-50 positions. Do I just filter out the dog shit and then let an aggregate scoring system keep me from the hidden dog shit. I don’t care about analysts because I am rarely in a position longer than a week and they always project annually.	
	any company that has options worth at least $20 a share and with 500k average daily trading volume that is bouncing somewhere in between the lower lines of the 50 and 100 linear regression channel on the 3 month, 2 hour chart.
	
	TOS I just set up a scanner that looks for tickers with an open price above the lower 100 and below the lower 50 lines on the 2 hour.





(rolling a put "down" means rolling a put further ITM.)

A single ATM put on /ES represents 200k worth of SPY and requires maybe 10k in buying power reduction and gives 4k in premium. A CC would be about the same but I wouldn’t have to put down 200k worth of cash to buy shares.
It is different though, it’s mark to market so losses and gains are swept to and from your cash every day. In the states it’s taxed 60/40 long term/short term gains. Also it has segmented expirations that you trade in and there is little volume beyond the current cycle until about a week before the next one picks up.
It is however, bar none, the easiest way to add a hedge to your portfolio. A single short /ES will reduce your spy beta weighted delta by 500.



Q.  Something I have never gotten a clear answer on...for ITM puts...is it better to roll on down days or up days? For some reason, I have seen people do it both ways. I would think you roll on down days to get more extrinsic premium in the next month?

A.  Green day for ITM.	IV pop day for deep ITM.









Honestly the main things you are looking for are theta decay which is strongest per contract ATM (although OTM gets more theta/buying power reduction but at the cost of ALOT more buying power expansion during a bear situation) and delta and a little vega. Gamma is practically non-existent if you exit at 21 DTE. During a bull market you will see positions close in 24 hours sometimes or maybe 2 days. During a bear market it could take a while to close out profitably or a little sooner if you set your BTC for net credit of your entry plus your rolls.
The key is buying power management, have rules and follow them.



Sell ATM CSPs until my BP reaches zero. Immediately set a BTC GTC order for 25% profit, wait for an email that something executed, find another trade(s) to take buying power down to zero. When the last Friday of the month rolled around, look at all remaining positions. Take down all winners that didn't hit 25% and everything that is 20% or less in the red. Roll the remainder at the same strike to the next month, calculate net premium received from the original put and the roll and set a BTC GTC for that so I can exit at a scratch. Open more positions with the remaining buying power. I am currently 3-5% monthly YTD with this strategy as of close of market today. Last year was around 4% a month average including March. This works because you are constantly compounding your premium on average every 6 days on winners. You should see a "win rate" of around 90% with this if you are picking things that aren't just stupid high vs. their mean. What most people worry about when selling CSP is losing out on a stupid bull run. However if you are selling ATM you will be getting large amounts of participation and if you are closing at 25% profit (you should be) your delta won't venture too far from 50 because as you get further OTM and you generate more unrealized profit your delta keeps shrinking so your participation in future appreciation shrinks as well. Having very low profit thresholds fixes this fault. ATM also saves money on commissions (I pay 2% of gross profit in premium, or $10 per $2000 in profit. If you are running margin it will not fluctuate too much on buying power. In a deep bear market you will see a 2x expansion vs. 3x or more with OTM. It doesn't take very long to get to a high delta during a down market so you can participate in the recovery. A march 2020 situation would of recovered by May. You make the same profit at 25% that a 30 delta does at 50%, you collect twice as much premium if not more so you need less contracts overall, your delta is higher so if the stock goes up you close out quicker, if it goes down you build delta quicker, because you are closing at 25% your probability should be north of 90% because of the increased theta decay and lower threshold as well. Also while you are waiting for your losing positions to recover you are still collecting theta decay so your break even threshold strike requirement keeps reducing over time.
As for when I sell, whenever the target month I am selling in reaches 30 DTE I switch the the next month. When I say monthlies I always mean the ones expiring on the third friday of the month as well. So right now I am selling only May 21 puts, all of them. Next friday (march 26) I will roll all of my April contracts over to May 21 as well. I will ride everything out then until they close or April 30th and then roll those to June. When April 21st arrives the May monthlies will be 30 DTE so I will start writing my new stuff in June as well.


Sell ATM CSPs until my BP reaches zero. Immediately set a BTC GTC order for 25% profit, wait for an email that something executed, find another trade(s) to take buying power down to zero. When the last Friday of the month rolled around, look at all remaining positions. Take down all winners that didn't hit 25% and everything that is 20% or less in the red. Roll the remainder at the same strike to the next month, calculate net premium received from the original put and the roll and set a BTC GTC for that so I can exit at a scratch. Open more positions with the remaining buying power. I am currently 3-5% monthly YTD with this strategy as of close of market today. Last year was around 4% a month average including March. This works because you are constantly compounding your premium on average every 6 days on winners. You should see a "win rate" of around 90% with this if you are picking things that aren't just stupid high vs. their mean. What most people worry about when selling CSP is losing out on a stupid bull run. However if you are selling ATM you will be getting large amounts of participation and if you are closing at 25% profit (you should be) your delta won't venture too far from 50 because as you get further OTM and you generate more unrealized profit your delta keeps shrinking so your participation in future appreciation shrinks as well. Having very low profit thresholds fixes this fault. ATM also saves money on commissions (I pay 2% of gross profit in premium, or $10 per $2000 in profit. If you are running margin it will not fluctuate too much on buying power. In a deep bear market you will see a 2x expansion vs. 3x or more with OTM. It doesn't take very long to get to a high delta during a down market so you can participate in the recovery. A march 2020 situation would of recovered by May. You make the same profit at 25% that a 30 delta does at 50%, you collect twice as much premium if not more so you need less contracts overall, your delta is higher so if the stock goes up you close out quicker, if it goes down you build delta quicker, because you are closing at 25% your probability should be north of 90% because of the increased theta decay and lower threshold as well. Also while you are waiting for your losing positions to recover you are still collecting theta decay so your break even threshold strike requirement keeps reducing over time.

As for when I sell, whenever the target month I am selling in reaches 30 DTE I switch the the next month. When I say monthlies I always mean the ones expiring on the third friday of the month as well. So right now I am selling only May 21 puts, all of them. Next friday (march 26) I will roll all of my April contracts over to May 21 as well. I will ride everything out then until they close or April 30th and then roll those to June. When April 21st arrives the May monthlies will be 30 DTE so I will start writing my new stuff in June as well.


I use 25% personally for various reasons. But trade management increases your probability of success and decreases the time in trade. At 25% currently YTD my average time in trade is 6.6 days. My average win rate is 92.5%. My average profit vs. hold to exp is 125% normalized. My average turns per 45 day cycle are 6-7. I make more because I can “turn my inventory” more than waiting for expiration. I also do so at a higher rate of success. Singles and doubles. Singles and double.


I use linear regression and momentum personally. I look for stocks with good relative strength vs. the S&P (w strength index) that are trading at between 1-2 standard deviations below their linear regression midline. Works for me. Some might like stocks that are on a crazy momentum train. I have been experimenting with those for a few weeks and the results have been good.


Try this: Standard Deviation Channel
Make one for 1.0 STD and 2.0 STD, both using 90 days as the period. 
.


When there are really big drops in SPX- ie like 1STD move down in a few days:

	Yes. My method for cascading in an event like this uses gamma. 7 DTE, close the strike at a profit equal to Share Size * Number of Contracts * Strike Distance. So for /ES it’s 50 * x * 5. So for each contract you would close out at $250 then open up immediately at the next strike. I was doing 10 contracts at a time so I would just set a BTC for $2500 profit, $5 less than open price. Once I got a notification I would go back and open up more.
	If you were using the monthlies strike distance is $10 so double that.
	If there is a pullback I would open up the previous strike as well. But no more exposure than that if it continues to pull back.
	This is kind of an advanced play that comes around rarely. A 3 day decline that is a + 1STD move down is a pretty good setup to expect some recovery. Then you just can’t overexpose yourself on the way back up.







In chat:
 	So you have 250k options buying power to play with and an account net liq of 694k
So for now, to stay comfortable, I would limit your Stocks and Options (short) to no more than 15% of your net liq, so 104k
	
	
	Once you open them, set a BTC GTC for 75% of premium received and just wait for it to close, if it doesnt close (you should be opening all positions in May 21 right now, switch to June when may contracts get to 44 DTE) by April last friday of the month roll them all to june
Set your BTC GTC after the roll to original credit + roll credit




calevonlear
07:11 PM
10-14 days is still great for 25% atm
You will outperform S&P with just that

terramar9989
07:12 PM
What charting service or stock screener do you use?  Does it show you where a stock is in relation to those 1-2 std deviation levels? Or do you click through a list and look at charts with some lines on them?

calevonlear
07:13 PM
I use Think or Swim for charting.
Also for screening.
I just make a basic screen though
Price $20+, volume 500k+, at or below 1 std lower line linear regression on 2h

terramar9989
07:14 PM
Okay, I'll check that out.

calevonlear
07:14 PM
From there what I do is use my TDA account to look up the symbol and just see what the tipranks rating is

terramar9989
07:14 PM
is " at or below 1 std lower line linear regression on 2h" something you can show on a screener?  Or do you look at charts to see?

calevonlear
07:14 PM
6-10 ill open positions
Both, you will have to set it up on the screener and 2h is just a tick on charts
For charting I use 2h ticks 90 day chart

terramar9989
07:16 PM
ah, okay, I'll check that out tonight and tomorrow.  Today was a travel day (my partner and I sold our respective houses and each of our youngest kids started college, and now we're rather nomadic, just moving from place to place every month or two - we've been living on a boat in San Diego for the last month, and today we went to the mountains...so you can see why I want to move away from strategies that require a lot of babysitting :)

calevonlear
07:17 PM
Yeah, it still requires a check in every day or so to see if stuff closes
And to open new positions if needed
Its the only way I could manage 100+ positions
I let delta, theta and vega do the heavy lifting

terramar9989
07:18 PM
I don't mind that.  I just don't like having to be sure I'm checking constantly like I did with my little day trades.  That was fun for a while, and I did okay, but I'm a bit tired of it
So let me see if I can summarize what you've recommended - and you can tell me if I've mixed anything up, if you would....

calevonlear
07:18 PM
I'm all about lifestyle design, so I am trying to be more work free heh

terramar9989
07:19 PM
So are we - that's why we've been living on a boat.  I figure to sell my current company in the next few years, and then we're off to sail for a couple of years, I think.

calevonlear
07:19 PM
I wish I liked boats that much lol, I am a home body

terramar9989
07:22 PM
Keep the overall short positions to about 15% of net liqudity. Increase later once I get this down.  (this is where you look at varying things based on VISX for example).

Keep individual positions to about 1% of net liquidity, so that would be about $6940 per position
(my summary - let me know if I've got this wrong)

calevonlear
07:23 PM
I would maybe do 0.5% since you are handicapping your buying power
So around 3k-4k

terramar9989
07:23 PM
Ah okay

calevonlear
07:23 PM
That should give you 20-40 positions

terramar9989
07:23 PM
So, looking at that etrade screenshot, is that AAPL put the $2,820.50 amount?

calevonlear
07:23 PM
Yep
So 1 contract for that

terramar9989
07:24 PM
So row "Purchasing power impact of this order", in the "Non-marginable securities/options" column
Got it
And shoot for 20-40 positions
With VIX swinging around so much these days, how often do you adjust for that?  (I know that's something I'm not going to deal with for a bit, since I still have a lot to learn, but I'm curious)

calevonlear
07:25 PM
I just look at it as a "pre-flight" check along with current buying power and net liq and what not
So if I am over ill just wait and not put on more positions and let the others close

terramar9989
07:26 PM
I get it
Well thank you!  I cannot tell you how much I appreciate the info!

calevonlear
07:27 PM
No worries, good luck! Let me know if you get stuck on something
Its pretty simple though
Im all about simple efficiency

terramar9989
07:28 PM
Thanks, I'm sure I'll end up with more questions :)
It's wonderful that you've been so open about sharing.  I've tried to be the same way - have mentored a bunch of younger guys doing startups - I figure I sweated through it all, so if I can help keep someone else from having to do the same, it's a win for them, and it keeps me on my toes

calevonlear
07:30 PM
I’ve always learned best by teaching. By being able to simplify things it shows true mastery.

terramar9989
07:30 PM
Ah here's a question already - are you only opening positions on their red days?
Or just looking for generally bullish setups?

calevonlear
07:30 PM
I usually will sort my list by % change and start there. But really 1 Green Day won’t kill you.
Most of my stuff is opened with a less than 0.5% up though. There are plenty of candidates.

terramar9989
07:32 PM
Setting up a TOS screener right now :)  500K Volume, close over $20.  Looking for how to display linear regression

calevonlear
07:32 PM
It will be a custom study. You will do close price is less than lower line of linear regression channel 50

terramar9989
07:34 PM
I will figure that out!  And hit you up if I get stuck

I think I found the custom study, and I have it set to say 'close is less than LinearRegCh50(), "LowerLOR"'

calevonlear
07:41 PM
Yep that’s it.

terramar9989
07:41 PM
fantastic.  I'm going to go play with this.

calevonlear
07:41 PM
You can limit your search to S&P 500 if you want.

terramar9989
07:41 PM
Oh, interesting thought.

calevonlear
07:42 PM
I usually do all stocks intersect with all optionable at the top.
But you might want to start with name brands.
Or better yet. Make it one of your watchlists of curated tickets.

terramar9989
07:42 PM
Okay, I'll give that a try.
On a given day, how many opportunities do you see?

calevonlear
07:43 PM
10-40, more on a red day.
More than you will need.

terramar9989
07:43 PM
Much more than I would have thought.
=
calevonlear
07:43 PM
You can also look through stuff like IBD screens. Filter strength score 70+ and overall 90+
Whatever you prefer. It’s best to curate a list. I need to get back to doing that.



terramar9989
08:59 PM
Hey - sorry for writing late, and don't feel like you have to answer at any particular time, but I was fooling around with the TOS scanner and charting and I can't get the scanner and chart to agree, so I must have messed something up.
I set up the scanner to show stocks that were under the lower range of LinearRegCh50, and got a list, and then set up charts to show that range too, and they have different numbers.
I put pics at  https://imgur.com/a/T8pT9pj
Maybe you can see what I did wrong?  I can't figure out if the scanner filter is wrong, or if the chart study is wrong

calevonlear
09:00 PM
Add Lin reg 100 to your chart.
So you can see it up to 2 std
On your scanner change the scan length to 2h as well. It is probably set for D

terramar9989
09:03 PM
Yep, it was set to D, and now it looks much more like what I was expecting

calevonlear
09:04 PM
Now add lin reg slope to a lower chart
And look for it to kinda bottom out and start curving upwards. Use 60 period
I also add relative strength vs. SPX as a lower study.
I like whatever I am getting into to have a nice positive momentum vs. spy over 3 month.

terramar9989
09:05 PM
okay, thanks.  I'm adding those and will look through some charts

calevonlear
09:06 PM
I opened DIS today to give you some perspective.

terramar9989
09:08 PM
Okay, thanks.  I'm going to look at some of these now

