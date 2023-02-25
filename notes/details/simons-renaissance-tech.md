---
description: >-
  matlab > python!! data has money; computerized automated trading 3000-5000
  stocks(US ); only 13.3% annual return according to US stock 13F-HR reports
---

# quant simons renaissance tech

[https://github.com/ricequant/rqalpha/blob/master/docs/source/intro/tutorial.rst](https://github.com/ricequant/rqalpha/blob/master/docs/source/intro/tutorial.rst) worked; bundles are hdf5 format, under \~/.rqalpha by default; 查询 get\_all\_factor\_names() 得到所有有效因子字段

```
[In]
get_factor(['000001.XSHE', '600000.XSHG'],'WorldQuant_alpha010', '20190601', '20190610')

[Out]
	600000.XSHG	000001.XSHE
2019-06-03	0.162771	0.093489
2019-06-04	0.255633	0.281502
2019-06-05	0.789430	0.222253
2019-06-06	0.437743	0.415231
2019-06-10	0.935448	0.134391

CLOSE = Factor('close')
RETURNS = (CLOSE - REF(CLOSE, 1)) / REF(CLOSE, 1)
CAP = Factor('market_cap')
VWAP = Factor('total_turnover') / Factor('volume)

alpha001 = (RANK(TS_ARGMAX(SIGNEDPOWER(IF((RETURNS < 0), STDDEV(RETURNS, 20), CLOSE), 2.), 5)) - 0.5)

alpha002 = (-1 * CORRELATION(RANK(DELTA(LOG(VOLUME), 2)), RANK(((CLOSE - OPEN) / OPEN)), 6))

alpha003 = (-1 * CORRELATION(RANK(OPEN), RANK(VOLUME), 10))

alpha004 = (-1 * TS_RANK(RANK(LOW), 9))

alpha005 = (RANK((OPEN - (SUM(VWAP, 10) / 10))) * (-1 * ABS(RANK((CLOSE - VWAP)))))

alpha006 = (-1 * CORRELATION(OPEN, VOLUME, 10))

alpha007 = IF((ADV(20) < VOLUME), ((-1 * TS_RANK(ABS(DELTA(CLOSE, 7)), 60)) * SIGN(DELTA(CLOSE, 7))), (-1 * 1))

alpha008 = (-1 * RANK(((SUM(OPEN, 5) * SUM(RETURNS, 5)) - DELAY((SUM(OPEN, 5) * SUM(RETURNS, 5)), 10))))

alpha009 = IF((0 < TS_MIN(DELTA(CLOSE, 1), 5)), DELTA(CLOSE, 1), (IF((TS_MAX(DELTA(CLOSE, 1), 5) < 0), DELTA(CLOSE, 1), (-1 * DELTA(CLOSE, 1)))))

alpha010 = RANK(IF((0 < TS_MIN(DELTA(CLOSE, 1), 4)), DELTA(CLOSE, 1), IF((TS_MAX(DELTA(CLOSE, 1), 4) < 0), DELTA(CLOSE, 1), (-1 * DELTA(CLOSE, 1)))))

alpha011 = ((RANK(TS_MAX((VWAP - CLOSE), 3)) + RANK(TS_MIN((VWAP - CLOSE), 3))) * RANK(DELTA(VOLUME, 3)))

alpha012 = (SIGN(DELTA(VOLUME, 1)) * (-1 * DELTA(CLOSE, 1)))
```

rentech holds SPDR SER TR not SPY? try all SMA/EMA combinations and backtest to see which short and long number of days work best, for both sharpe ratio and alpha. ch7 on jupyter notebook; ch9 done on pdf abu book -  UMpire judge; got original pdf of abu book; was able to install abu and run the ipynb

ricequant - [https://www.bilibili.com/medialist/play/ml1197302639/BV1Ft4y1B7XR](https://www.bilibili.com/medialist/play/ml1197302639/BV1Ft4y1B7XR) [4天学会python量化交易投资课程](https://www.bilibili.com/video/BV1Ft4y1B7XR); multi-factor - not just SMA crossing; day 1 3 4 got; day 2 large file bidu download slow; their web portal need pay; Arbitrage pricing theory (**APT**) FF=french fama; multi-factor including PE ROE etc=market neutral; median absolute deviation=mad; day2-05 good stuff about market neutral! rqalpha worked with "python setup.py install" zipline didn't work; need "rqalpha download-bundle" before running "python test.py"; need "pip install ta-lib"

tsing-hua yingcheng talks about abu, highest 120k lines of code; on p104 US tech stock analysis; quite easy basic stuff; already got

[http://theautomatic.net/2020/05/05/how-to-download-fundamentals-data-with-python/](http://theautomatic.net/2020/05/05/how-to-download-fundamentals-data-with-python/) yahoo fundamental; python get stock fundamental data;  works now in f.py

matlab金融算法分析实战 吴婷,余胜威编著 got code and ppt, but not original pdf

[https://space.bilibili.com/635258539/favlist?fid=1197302639\&ftype=create](https://space.bilibili.com/635258539/favlist?fid=1197302639\&ftype=create) bili space personal

```
# 可以自己import我们平台支持的第三方python模块，比如pandas、numpy等。
# 1、获取市值和市净率因子数据，
# 因子：极值、标准化、中性化处理
# 2、选定股票池（根据方向权重）
# 市净率小的某些股票
from sklearn.linear_model import LinearRegression
import numpy as np


# 在这个方法中编写任何的初始化逻辑。context对象将会在你的算法策略的任何方法之间做传递。
def init(context):
    # 在context中保存全局变量
    # context.s1 = "000001.XSHE"
    # # 实时打印日志
    # logger.info("RunInfo: {}".format(context.run_info))
    scheduler.run_monthly(get_data, tradingday=1)


def get_data(context, bar_dict):
    # 查询两个因子的数据结果
    fund = get_fundamentals(
        query(
            fundamentals.eod_derivative_indicator.pb_ratio,
            fundamentals.eod_derivative_indicator.market_cap
        ))

    # 查看fund格式
    # logger.info(fund)
    context.fund = fund.T

    # 进行因子数据的处理，去极值、标准化、市值中心化
    treat_data(context)

    # 利用市净率进行选股（市净率小的股票表现好）
    context.stock_list = context.fund['pb_ratio'][
        context.fund['pb_ratio'] <= context.fund['pb_ratio'].quantile(0.2)].index


def treat_data(context):
    """
    市净率因子数据的处理逻辑
    """
    # 对市净率去极值标准化
    context.fund['pb_ratio'] = mad(context.fund['pb_ratio'])
    context.fund['pb_ratio'] = stand(context.fund['pb_ratio'])

    # 选股的处理，对市净率进行市值中性化
    # 特征值：市值
    # 目标值：市净率因子
    x = context.fund['market_cap'].reshape(-1, 1)
    y = context.fund['pb_ratio']

    # 建立线性回归，中性化处理
    lr = LinearRegression()
    lr.fit(x, y)

    y_predict = lr.predict(x)

    context.fund['pb_ratio'] = y - y_predict


# before_trading此函数会在每天策略交易开始前被调用，当天只会被调用一次
def before_trading(context):
    pass


# 你选择的证券的数据更新将会触发此段逻辑，例如日或分钟历史数据切片或者是实时数据切片更新
def handle_bar(context, bar_dict):
    # 开始编写你的主要的算法逻辑

    # bar_dict[order_book_id] 可以拿到某个证券的bar信息
    # context.portfolio 可以拿到现在的投资组合信息

    # 使用order_shares(id_or_ins, amount)方法进行落单

    # TODO: 开始编写你的算法吧！
    # order_shares(context.s1, 1000)
    # 卖出
    for stock in context.portfolio.positions.keys():

        if stock not in context.stock_list:
            order_target_percent(stock, 0)

    weight = 1.0 / len(context.stock_list)

    # 买入
    for stock in context.stock_list:
        order_target_percent(stock, weight)


def mad(factor):
    """
    实现3倍中位数绝对偏差去极值
    """
    # 1、找出因子的中位数 median
    me = np.median(factor)

    # 2、得到每个因子值与中位数的绝对偏差值 |x – median|
    # 3、得到绝对偏差值的中位数， MAD，median(|x – median|)
    # np.median(abs(factor - me))就是MAD
    mad = np.median(abs(factor - me))

    # 4、计算MAD_e = 1.4826*MAD，然后确定参数 n，做出调整
    # 求出3倍中位数上下限制
    up = me + (3 * 1.4826 * mad)
    down = me - (3 * 1.4826 * mad)

    # 利用3倍中位数的值去极值
    factor = np.where(factor > up, up, factor)
    factor = np.where(factor < down, down, factor)
    return factor


def stand(factor):
    """自实现标准化
    """
    mean = factor.mean()
    std = factor.std()

    return (factor - mean) / std


# after_trading函数会在每天交易结束后被调用，当天只会被调用一次
def after_trading(context):
    pass
```

markov = memory-less M2 money [https://fred.stlouisfed.org/series/WM2NS](https://fred.stlouisfed.org/series/WM2NS) 19432.4 B M2; $21.49 trillion gdp from [bea.gov](https://www.bea.gov) at of Q4 2020; [https://fred.stlouisfed.org/series/WILL5000PRFC](https://fred.stlouisfed.org/series/WILL5000PRFC) Wilshire 5000 Full Cap Price Index or Dow Jones U.S. Total Stock Market Index INDEXDJX: DWCF 41000; must download csv manually

tasks - old kaufman trading systems and methods book on digital iucat; matrix fortran - matlab; trig regression - fortran; python better? too old contents [https://github.com/yungshun317/trading-systems-methods/blob/master/datacamp\_algorithmic\_trading.ipynb](https://github.com/yungshun317/trading-systems-methods/blob/master/datacamp\_algorithmic\_trading.ipynb) trading volatility; Mac Numbers is better at openning text file; rentech holds bilibili futu etc; tencent music IPO first May 2020, how could they analyze past history data on it? holds domino, dht but not nordic

[https://github.com/vnpy/vnpy](https://github.com/vnpy/vnpy) github 1 [https://github.com/quantopian/zipline](https://github.com/quantopian/zipline) 2 [https://github.com/bbfamily/abu](https://github.com/bbfamily/abu) 3 [https://github.com/wilsonfreitas/awesome-quant](https://github.com/wilsonfreitas/awesome-quant) list of stuff  [https://github.com/QUANTAXIS/QUANTAXIS](https://github.com/QUANTAXIS/QUANTAXIS) 4 [https://github.com/microsoft/qlib](https://github.com/microsoft/qlib) 6 [https://github.com/ricequant/rqalpha](https://github.com/ricequant/rqalpha) 7 [https://github.com/jindaxiang/akshare](https://github.com/jindaxiang/akshare) 8

[https://github.com/ricequant/rqalpha.git](https://github.com/ricequant/rqalpha.git) ricequant [https://github.com/onshek/Ricequant](https://github.com/onshek/Ricequant) notes [https://github.com/topics/ricequant](https://github.com/topics/ricequant) links [https://www.ilovematlab.cn/thread-527143-1-1.html](https://www.ilovematlab.cn/thread-527143-1-1.html) matlab book [https://www.bilibili.com/medialist/play/ml1197302639/BV1Ft4y1B7XR](https://www.bilibili.com/medialist/play/ml1197302639/BV1Ft4y1B7XR) python ricequant videos



vnpy is mostly CTA - commodity trade advisor; it moved to gitee now;&#x20;

zipline :  brew install freetype pkg-config gcc openssl hdf5; python -m ensurepip --default-pip; pip install zipline won't work on python 3.9 two computers

olmar [http://icml.cc/2012/papers/168.pdf](http://icml.cc/2012/papers/168.pdf); examples ema20>ema40; sma100>sma300; momentum pipeline [https://github.com/quantopian/zipline/tree/master/zipline/examples](https://github.com/quantopian/zipline/tree/master/zipline/examples) list of examples

[量化交易之路（阿布）](https://www.yangchunmian888.com/%E9%87%8F%E5%8C%96%E4%BA%A4%E6%98%93%E4%B9%8B%E8%B7%AF-%EF%BC%88%E9%98%BF%E5%B8%83%EF%BC%89%E3%80%90%E7%AE%80%E4%BB%8B\_%E4%B9%A6%E8%AF%84\_%E5%9C%A8%E7%BA%BF%E9%98%85%E8%AF%BB-pdf-mobi-epub%E3%80%91/10660/)mentioned 2 opensource codes: zipline pyalgotrade&#x20;



[http://numericalmethod.com/papers/course1/](http://numericalmethod.com/papers/course1/) acar acar 1993 framework quantitative research github - can't find; [http://numericalmethod.com/papers/course1/](http://numericalmethod.com/papers/course1/) has code and reports [http://www.numericalmethod.com/javadoc/suanshu/com/numericalmethod/suanshu/stats/test/timeseries/adf/package-summary.html](http://www.numericalmethod.com/javadoc/suanshu/com/numericalmethod/suanshu/stats/test/timeseries/adf/package-summary.html) suanshu java stats package



[https://github.com/quantopian/zipline](https://github.com/quantopian/zipline) quantopian [https://github.com/quantopian](https://github.com/quantopian) git quantconnect.com

[https://www.youtube.com/watch?v=nCKzRY\_JV4c](https://www.youtube.com/watch?v=nCKzRY\_JV4c) quantopian shutdown [https://www.quantconnect.com/forum/discussions/2/interesting](https://www.quantconnect.com/forum/discussions/2/interesting) left one [https://www.reddit.com/r/CryptoCurrency/comments/b78lwb/nanex\_exchange\_shutting\_down\_in\_april\_get\_your/](https://www.reddit.com/r/CryptoCurrency/comments/b78lwb/nanex\_exchange\_shutting\_down\_in\_april\_get\_your/) nanex shutdown

[https://www.aclweb.org/anthology/J93-2003.pdf](https://www.aclweb.org/anthology/J93-2003.pdf) mercer paper [https://quantlabs.net/blog/2019/11/did-this-research-paper-kick-off-renaissance-technologies/](https://quantlabs.net/blog/2019/11/did-this-research-paper-kick-off-renaissance-technologies/) quantlabs blog [http://www.cs.jhu.edu/\~post/bitext/](http://www.cs.jhu.edu/\~post/bitext/) mercer audio talk; rentech only hire c++ jobs

[https://youtu.be/ggpfTX3Mm70](https://youtu.be/ggpfTX3Mm70) quantlabs guy;&#x20;

rentech front-running other big orders; Mac Numbers can import text files with spaced tables correctly without any special handing, just delete the extra lines (118 lines from top; 5 bottom for 13F-HR full-submission.txt in year 2008; sec edgar keeps changing file format, even for text file) - Excel can''t and has no intelligence. a human can't handle 3000 stocks - only computer can.

[https://github.com/silpol/mrsync](https://github.com/silpol/mrsync) HP Wei at rentech!

[http://www.statmt.org/book/](http://www.statmt.org/book/) stat mt book [https://quant.stackexchange.com/questions/30509/what-is-advent-softwares-geneva/30510](https://quant.stackexchange.com/questions/30509/what-is-advent-softwares-geneva/30510) avent geneva

[https://towardsdatascience.com/python-for-finance-stock-portfolio-analyses-6da4c3e61054](https://towardsdatascience.com/python-for-finance-stock-portfolio-analyses-6da4c3e61054) stock python



covariance part- pstvy has negative covariance with spy [https://numpy.org/doc/stable/reference/generated/numpy.corrcoef.html#numpy.corrcoef](https://numpy.org/doc/stable/reference/generated/numpy.corrcoef.html#numpy.corrcoef) corrcoef not cov() in numpy! [https://numpy.org/doc/stable/reference/generated/numpy.cov.html](https://numpy.org/doc/stable/reference/generated/numpy.cov.html) covariance calc [https://codingandfun.com/portfolio-risk-and-returns-python/](https://codingandfun.com/portfolio-risk-and-returns-python/) starting point but not right [https://codingandfun.com/portfolio-optimization-with-python/](https://codingandfun.com/portfolio-optimization-with-python/) optimization step&#x20;

[https://github.com/yungshun317/trading-systems-methods/blob/master/datacamp\_algorithmic\_trading.ipynb](https://github.com/yungshun317/trading-systems-methods/blob/master/datacamp\_algorithmic\_trading.ipynb) volatility part- spy is less than individual stocks; individual stocks have higer volatility/risk than spy

use ascii text as db, easy to debug when errors; use pickle to save data; separate data getting and computing in 2 steps

minimize risk first - lower correlation; stock return's standard deviation is the volatility of stock itself;

covariance measures correlation between different stocks - diversify away risk

[https://www.aclweb.org/anthology/J93-2003.pdf](https://www.aclweb.org/anthology/J93-2003.pdf) same mercer paper

wilmott.com and quantnet.com&#x20;

Then it’s just a question of estimating parameters from a lot of data. If you look at our blackboards, they look exactly like your blackboards. Full of similar kinds of equations. The big difference in finance is that the level of noise is much greater. It’s all noise in finance and there’s more structure in natural language models.&#x20;

There’s no arguing about the evaluation parts …

But otherwise it’s exactly the same. You’re just sitting there building models with data and writing programs to optimize functions based on the models and that’s that. So the skills are exactly the same except there’s more statistics because you have to worry about noise a lot more.

When there’s more noise then the model is not as complex.

Let me raise one of the differences with finance. In natural language, you can always get more data, by typing in Lassie novels or whatever kind of thing like that. In financial data, there just is what there is and that’s it. You can’t create more data on price movements, and so that’s frustrating. You can’t take the Google approach.



One of the datasets we purchased is cloud cover data. It turns out that when it’s cloudy in Paris, the French market is less likely to go up than when it’s sunny in Paris. That’s true in Milan, it’s true in Tokyo, it’s true in Sao Paulo, it’s true in New York. It’s just true.

Now, you can’t make a lot of money from that data because it’s only slightly more likely to go up. But it is statistically significant. The point is that, if there were signals that made a lot of sense that were very strong, they would have long ago been traded out. So either there are signals that you just can’t understand, but they’re there, and they can be relatively strong. There’s not many of those. Or, much more likely, it’s a very weak signal, such as this cloud cover data signal. What we do is look for lots and lots, and we have, I don’t know, like 90 PhDs in math and physics, who just sit there looking for these signals all day long. We have 10,000 processors in there that are constantly grinding away looking for signals.

The idea is that any one of these or any handful of these wouldn’t be enough to overcome transaction costs, but if you combined lots and lots and lots of them together, then you can overcome transaction costs and you have something. So it’s a lot of weak signals like this cloud cover data, but that’s the only one I’m going to disclose no matter how hard you push me.

We don’t know which exactly signal they’re finding, but what we can see is that a system from 10 years ago or something like that, gradually degrades with time, and that must be because other people are catching on. So we just have to keep extending the building and hiring more people and it’s annoying but that’s the way it is.\


**cloud cover data on cities**

**last part of the talk covers finance only a little**

[https://faculty.fuqua.duke.edu/\~charvey/Teaching/BA453\_2005/II\_On\_Jim\_.pdf](https://faculty.fuqua.duke.edu/\~charvey/Teaching/BA453\_2005/II\_On\_Jim\_.pdf) simmons paper



they have a reservoir of stocks to trade from and which they know how to rank (StatArb) in mean-reversion mode, and where they find the volatility to produce the kind of risks and returns of 70%/year consistently.

When you scale this $50 Billion back to a retail investor with $50,000 or less to trade, how many trades should he need per year to produce these same relative returns: only 50! Keep in mind, these are scaling estimates, not rigorous math.

So with the right reservoir of stocks and with proper ranking and weighting systems, a retail investor should be able to quarterly sell his old and buy his new portfolios of 6 stocks (12 trades/quarter or 48 trades/year) and make 70%/year.

The reservoir that will do that is a collection of some 1300 Wall Street stocks with daily-dollar volumes in excess of $1 Million. The ranking system that is able to rank each quarter the 6 top ranks from this reservoir of 1300 is called Ergodic ranking. You don’t need any breaking news, TA, or FA. You just need the historical eod data for this ranking system. CSI is our dataprovider of choice, and we use Finance.Yahoo as a reference. The portfolio returns are considered as a weighted sum of the individual asset returns. The weighting system is Kelly’s system where portfolio weightings are computed each quarter so as to maximize annual returns. You do this for 10 or more years and you assume, with Jim Simons, that past performance is your best predictor of success.\


[https://www.quora.com/What-are-the-investment-strategies-of-James-Simons-Renaissance-Technologies-I-understand-he-employs-complex-mathematical-models-along-with-statistical-analyses-to-predict-non-equilibrium-changes](https://www.quora.com/What-are-the-investment-strategies-of-James-Simons-Renaissance-Technologies-I-understand-he-employs-complex-mathematical-models-along-with-statistical-analyses-to-predict-non-equilibrium-changes) youtube link [https://www.youtube.com/watch?v=ggpfTX3Mm70](https://www.youtube.com/watch?v=ggpfTX3Mm70) explains

simmons - past history is all there to lead to success

[https://www.quantconnect.com/tutorials/introduction-to-financial-python/pandas-resampling-and-dataframe](https://www.quantconnect.com/tutorials/introduction-to-financial-python/pandas-resampling-and-dataframe) resample pandas dataframe

```
import quandl
quandl.ApiConfig.api_key = 'dRQxJ15_2nrLznxr1Nn4'
aapl_table = quandl.get('WIKI/AAPL')
aapl = aapl_table['Adj. Close']['2017']
print(aapl) ## all print x have to be print(x) below!
print aapl['2017-3']

print aapl['2017-2':'2017-4']
print aapl.head()
print aapl.tail(10)
# resampling into months, and get mean value
by_month = aapl.resample('M').mean()
print by_month
# resampling into weeks, and get weekly mean value
by_week = aapl.resample('W').mean()
print by_week.head()

three_day = aapl.resample('3D').mean() # 3 days
two_week  = aapl.resample('2W').mean() # 2 weeks
two_month = aapl.resample('2M').mean() # 2 months

std = aapl.resample('W').std()    # standard deviation
max = aapl.resample('W').max()    # maximum value
min = aapl.resample('W').min()    # minimum value

#calculate monthly returns of a stock, based on prices on the last day of each month. 
#To fetch those prices, we use the series.resample.agg() method:
last_day = aapl.resample('M').agg(lambda x: x[-1])
print last_day

#diff() calculates the difference between consecutive elements
#pct_change() calculates the percentage change
print last_day.diff()
print last_day.pct_change()

#When dealing with NaN values, we usually either removing the data point or 
#fill it with a specific value. Here we fill it with 0:
daily_return = last_day.pct_change()
print daily_return.fillna(0)
#Alternatively, we can fill a NaN with the next fitted value. 
#This is called 'backward fill', or 'bfill' in short:
daily_return = last_day.pct_change()
print daily_return.fillna(method = 'bfill')
#a 'forward fill' method, or 'ffill' also exists. 
#However we can't use it here because the NaN is the first value here.
#We can also simply remove NaN values by .dropna()
daily_return = last_day.pct_change().dropna()
print daily_return

#calculate the monthly rates of return using the data for the first day and the last day:
monthly_return = aapl.resample('M').agg(lambda x: x[-1]/x[1] - 1)
print monthly_return

print monthly_return.mean()
print monthly_return.std()
print monthly_return.max()
```

The **DataFrame** is the most commonly used data structure in Pandas. It is essentially a **table**, just like an Excel spreadsheet.

More precisely, a DataFrame is a collection of Series objects, each of which may contain different data types. A DataFrame can be created from various data types: dictionary, 2-D numpy.ndarray, a Series or another DataFrame.

```
dict = {'AAPL': [143.5,  144.09, 142.73, 144.18, 143.77],
        'GOOG': [898.7,  911.71, 906.69, 918.59, 926.99],
        'IBM':  [155.58, 153.67, 152.36, 152.94, 153.49]}
dates = pd.date_range('2017-07-03', periods = 5, freq = 'D')
df = pd.DataFrame(dict, index = dates)
print df

#fetch a column by square brackets: df['column_name']
#If a column name contains no spaces, then we can also use df.column_name to 
#fetch a column:

df = aapl_table
print df.Close.tail(5)
print df['Adj. Volume'].tail(5)

aapl_2016 = df['2016']
aapl_month = aapl_2016.resample('M').agg(lambda x: x[-1])
print aapl_month
```

[https://www.bilibili.com/medialist/play/ml1194776039/BV1rJ411A74G](https://www.bilibili.com/medialist/play/ml1194776039/BV1rJ411A74G) python for finance

[量化交易之路（阿布）](https://www.yangchunmian888.com/%E9%87%8F%E5%8C%96%E4%BA%A4%E6%98%93%E4%B9%8B%E8%B7%AF-%EF%BC%88%E9%98%BF%E5%B8%83%EF%BC%89%E3%80%90%E7%AE%80%E4%BB%8B\_%E4%B9%A6%E8%AF%84\_%E5%9C%A8%E7%BA%BF%E9%98%85%E8%AF%BB-pdf-mobi-epub%E3%80%91/10660/)zipline pyalgotrade opensource code

holding 2 days on average

statistical artitrage - historic patterns of the market, relationships of those patterns, factors

人少活在梦境里，多活在现实中，现实虽然残忍，但却可以刺激人向上，来一段我看过的话：生活只欺负穷苦人，佛门只度有钱人。麻绳专挑细处断，厄运专挑苦命人。万恶穷为首，百善钱为先。 苦难，从来都不应该回避，也不是堕落放弃的理由，而是刺激你变大更强的动力。



洛克菲勒40句经典语录：没有体验过不幸的人，反而不幸！

1.沉默带给你的好处很多，摆低姿态，变的谦虚，换言之就是，隐藏你的聪明。越聪明的人，越懂得沉默，就像成熟的稻子，垂下稻穗。

2.缺乏行动的人都有一个坏习惯：喜欢维持现状，拒绝改变。

3.我做事总有一个习惯，在做决定之前，我总会冷静地思考、判断，但我一旦做出决定，就将义无反顾地执行到底。

4.世界上什么事都可以发生，就是不会发生不劳而获的事，那些随波逐流、墨守成规的人，我不屑一顾。

5.我鄙视那些善找借口的人，因为那是懦弱者的行为，我也同情那些善找借口的人，因为借口是制造失败的病源。

6.这是公然的挑衅，我却装作充耳不闻，我知道自己尊重自己比什么都重要，但是。我在心里已经同他开战：，我一遍一遍地叮嘱自己：超越他，你的强大是对他最好的羞辱，是打在他脸上最响的耳光。

7.没有体验过不幸的人，反而不幸。

8.享有特权而没有力量的人是废物，受过教育而无影响力的人是一堆一文不值的垃圾。

9.一头猪好好被夸奖一番，它就能爬上树去。

10.如果你们想成为重要人物，就必须首先使自己承认“我确实很重要”，而且要真正的这么觉得，别人才会跟着这么想。

11.你应该清楚，知识原本是空的，除非把知识付出行动，否则什么事都不会发生。

12.我们要勇于在别无选择中，毅然杀出一条生路。

13.在机会的世界里，没有太多的机会可以争取，如果你真的想成功，你一定要掌握并保护自己的机会，更要设法抢夺别人的机会。

14.每个人都有失去自信，怀疑自己能力的时候，尤其是在逆境中的时候。

15.一个人不可能靠着运气而成功，而是要付出努力的代价。

16.人生就是不断抵押的过程，为前途我们抵押青春，为幸福我们抵押生命。因为如果你不敢逼近底线，你就输了。为成功我们抵押冒险不值得吗？

17.没有谁可以阻挡我们回家的路，除非我们不想回家。

18.尊严不是天赐的，也不是别人给予的，是你自己缔造的。

19.在业务的基础上建立的友谊，胜地过在友谊的基础上建立的业务。

20.如果你要成功，你应该朝新的道路前进，不要跟随被踩烂了的成功之路。

21.在苦难中向上攀爬的人，知道如何千方百计地去寻找方法、手段，让自己得救。

22.要知道，每个人对自己受到轻视都非常敏感，被看矮一截都会丧失干劲。

23.每一次说不懂的机会，都会成为我们人生的转折点。

24.坦率地说，自我感觉到人世间因贫穷而疾苦的时候，我就萌发了一个信念：我应该是富翁，我没有权利当穷人。随着时间的推移，这个信念变得犹如钢铁般坚硬。

25.我从来没想过会输，但是即使输了，唯一该做的就是光明磊落地去输。

26.没有行动就没有结果，世界上没有哪一件东西不是由一个个想法付诸实施得来的。

27.我们追求完美，但是人类的事情没有一件绝对完美，只有接近完美。

28.要知道，地狱里住满了好人，失败的痛苦是商战的一部分，我们彼此都在扼杀对手，没有竞争奋斗到底的决心，就只有做失败者的资格。

29.每一件事情都要做得尽善尽美。你付不起贪小失大所累积的种种额外负担。

30.想想看，我们这个世界就如同一座高山，当我们的父母生活在山顶时，注定你不会生活在山脚下；当我们的父母生活在山脚下时，注定你不会生活在山顶上。

31.在多数情况下，父母的位置决定了孩子的人生起点。但这并不意味着，每个人的起点不同，其人生结果也不一样。在这个世界上，永远没有穷、富世袭之说，也永远没有成、败世袭之说，有的只是我奋斗我成功的真理。我坚信，我们的命运由我们的行动决定，而绝非完全由我们的出身决定。

32.一个人必须找到自己的家，才不至于去流浪或者沦为乞丐。

33.精神的力量来自远大的目标和坚强的信念是超越自我的内在动力。

34.慈善是有害的，除非它能帮助接受慈善的人不再依赖慈善。

35.无论何时何地，在最终胜负显现之前，决不能押上所有的筹码。

36.人生中最令人感到挫折的，莫过于想做的事太多，结果不但没有足够的时间去做，反而想到每件事的步骤繁多，而被做不到的情绪所震慑，以致一事无成。

37.我将每一个阻碍，视为让自己蜕变的一个机会，而非斤斤计较他人对我做了什么。

38.我从不相信失败是成功之母，我只相信信心是成功之父！

39.只知道努力工作的人，失去的是赚钱的时间。

40.我行走于人世己近八十年，我见过不会吃牛排的人，却没有见过一个不贪心的人。
