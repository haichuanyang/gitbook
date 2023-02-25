---
description: >-
  multi-factor framework selector; daily data weekly trade; biggest problems is
  no quality data;
---

# ricequant

[https://github.com/dmquant/chinesecloud](https://github.com/dmquant/chinesecloud) 金融文本挖掘系统实现开源——光大证券

day4 scoring system for selecting factors vs regression method

scoring method: sharpe ratio 1.1 quite good; 39% worst down; sharpe 5.9 in simulation 模拟交易 DAY4-10

```
#version 1
# 打分法选股
# 回测：2010-01-01~2018-01-01
# 调仓：按月
# 选股因子：市值、市盈率、市净率、ROIC、inc_revenue营业总收入
# 和inc_profit_before_tax利润增长率
# 选股的指数、模块：全A股
import pandas as pd


# 在这个方法中编写任何的初始化逻辑。context对象将会在你的算法策略的任何方法之间做传递。
def init(context):
    context.group_number = 10
    context.stocknum = 20

    # 定义调仓频率函数
    scheduler.run_monthly(score_select, tradingday=1)


def score_select(context, bar_dict):
    """调仓函数
    """

    # 选股逻辑，获取数据、数据处理、选股方法

    # 1、获取选股的因子数据
    q = query(fundamentals.eod_derivative_indicator.market_cap,
              fundamentals.eod_derivative_indicator.pe_ratio,
              fundamentals.eod_derivative_indicator.pb_ratio,
              fundamentals.financial_indicator.return_on_invested_capital,
              fundamentals.financial_indicator.inc_revenue,
              fundamentals.financial_indicator.inc_profit_before_tax
              )

    fund = get_fundamentals(q)

    # 通过转置将股票索引变成行索引、指标变成列索引
    factors_data = fund.T

    # 数据处理
    factors_data = factors_data.dropna()

    # 对每个因子进行打分估计，得出综合评分
    get_score(context, factors_data)

    # # 进行调仓逻辑，买卖
    rebalance(context)


def get_score(context, factors_data):
    """
    对因子选股数据打分
    因子升序：市值、市盈率、市净率
    因子降序：ROIC、inc_revenue营业总收入和inc_profit_before_tax利润增长率
    """
    # logger.info(factors_data)
    for factorname in factors_data.columns:

        if factorname in ['pe_ratio', 'pb_ratio', 'market_cap']:
            # 单独取出每一个因子去进行处理分组打分
            factor = factors_data.sort_values(by=factorname)[factorname]
        else:

            factor = factors_data.sort_values(by=factorname, ascending=False)[factorname]
            # logger.info(factor)

        # 对于每个因子去进行分组打分
        # 求出所有的股票个数
        single_groupnum = len(factor) // 10

        # 对于factor转换成dataframe，为了新增列分数
        factor = pd.DataFrame(factor)

        factor[factorname + "score"] = 0

        # logger.info(factor)

        for i in range(10):

            if i == 9:
                factor[factorname + "score"][i * single_groupnum:] = i + 1

            factor[factorname + "score"][i * single_groupnum:(i + 1) * single_groupnum] = i + 1

        # 打印分数
        # logger.info(factor)
        # 拼接每个因子的分数到原始的factors_data数据当中
        factors_data = pd.concat([factors_data, factor[factorname + "score"]], axis=1)

    # logger.info(factors_data)

    # 求出总分
    # 先取出这几列分数
    # 求和分数
    sum_score = factors_data[
        ["market_capscore", "pe_ratioscore", "pb_ratioscore", "return_on_invested_capitalscore", "inc_revenuescore",
         "inc_profit_before_taxscore"]].sum(1).sort_values()

    # 拼接到factors_data
    # logger.info(sum_score)
    context.stocklist = sum_score[:context.stocknum].index


def rebalance(context):
    # 卖出
    for stock in context.portfolio.positions.keys():

        if context.portfolio.positions[stock].quantity > 0:

            if stock not in context.stocklist:
                order_target_percent(stock, 0)

    # 买入
    for stock in context.stocklist:
        order_target_percent(stock, 1.0 / 20)


# before_trading此函数会在每天策略交易开始前被调用，当天只会被调用一次
def before_trading(context):
    pass


# 你选择的证券的数据更新将会触发此段逻辑，例如日或分钟历史数据切片或者是实时数据切片更新
def handle_bar(context, bar_dict):
    # 开始编写你的主要的算法逻辑
    pass


# after_trading函数会在每天交易结束后被调用，当天只会被调用一次
def after_trading(context):
    pass
```

```
# version 2
# - 1、回测区间：
#   - 2010-01-01  ~  2018-01-01
# - 2、选股：
#   - 选股因子：6个已知方向的因子
#     - 市值-market_cap、市盈率-pe_ratio、市净率-pb_ratio
#     - ROIC-return_on_invested_capital、inc_revenue-营业总收入 和inc_profit_before_tax-利润增长率
#   - 数据处理：处理缺失值
#   - 选股权重：
#     - 因子升序从小到大分10组，第几组为所在组得分
#     - 因子降序从大到小分10组，第几组为所在组得分
#   - 选股范围：
#       - 选股的指数、模块：全A股
# - 3、调仓周期：
#   - 调仓：每月进行一次调仓选出20个排名靠前的股票
#   - 交易规则：卖出已持有的股票
#   - 买入新的股票池当中的股票
import pandas as pd


# 在这个方法中编写任何的初始化逻辑。context对象将会在你的算法策略的任何方法之间做传递。
def init(context):
    # # 在context中保存全局变量
    # context.s1 = "000001.XSHE"
    # # 实时打印日志
    # logger.info("RunInfo: {}".format(context.run_info))
    # 限定股票池的股票数量
    context.stocknum = 20

    context.up = ['market_cap', 'pe_ratio', 'pb_ratio']

    # 运行按月定时函数
    scheduler.run_monthly(score_select, tradingday=1)


def score_select(context, bar_dict):
    """打分法选股函数
    """
    # 1、选出因子数据、进行缺失值处理
    q = query(
        fundamentals.eod_derivative_indicator.market_cap,
        fundamentals.eod_derivative_indicator.pe_ratio,
        fundamentals.eod_derivative_indicator.pb_ratio,
        fundamentals.financial_indicator.return_on_invested_capital,
        fundamentals.financial_indicator.inc_revenue,
        fundamentals.financial_indicator.inc_profit_before_tax
    )

    fund = get_fundamentals(q)

    factors_data = fund.T

    factors_data = factors_data.dropna()

    # 2、定义打分函数、确定股票池
    select_stocklist(context, factors_data)

    # 3、定义调仓函数
    rebalance(context)


def select_stocklist(context, factors_data):
    """打分的具体步骤、返回股票池
    因子升序从小到大分10组，第几组为所在组得分
        市值-market_cap、市盈率-pe_ratio、市净率-pb_ratio
    因子降序从大到小分10组，第几组为所在组得分
        ROIC-return_on_invested_capital、inc_revenue-营业总收入 和inc_profit_before_tax-利润增长率
    """
    # 循环每个因子去处理
    for name in factors_data.columns:

        # 因子升序的，进行升序排序
        if name in context.up:

            factor = factors_data.sort_values(by=name)[name]

        else:
            # 因子降序的，进行降序排序

            factor = factors_data.sort_values(by=name, ascending=False)[name]

        # 对单个因子进行打分处理
        # 新建一个因子分数列
        factor = pd.DataFrame(factor)

        factor[name + 'score'] = 0

        # 进行打分
        # 先求出每组数量，然后根据数量一次给出分数
        stock_groupnum = len(factors_data) // 10

        for i in range(10):

            if i == 9:
                factor[name + 'score'][(i + 1) * stock_groupnum:] = i + 1

            factor[name + 'score'][i * stock_groupnum: (i + 1) * stock_groupnum] = i + 1

        # 把每个因子的得分进行合并到原来因子数据当中
        factors_data = pd.concat([factors_data, factor[name + 'score']], axis=1)

    # logger.info(factors_data)
    # 对6个因子的分数列进行求和
    all_score = factors_data[
        ['market_capscore', 'pe_ratioscore', 'pb_ratioscore', 'return_on_invested_capitalscore', 'inc_revenuescore',
         'inc_profit_before_taxscore']].sum(1).sort_values()

    # 定义股票池
    context.stock_list = all_score.index[:context.stocknum]

    logger.info(context.stock_list)


def rebalance(context):
    """
    调仓函数
    卖出、买入
    """
    # 卖出
    for stock in context.portfolio.positions.keys():

        if stock not in context.stock_list:
            order_target_percent(stock, 0)

    # 买入
    for stock in context.stock_list:
        order_target_percent(stock, 1.0 / len(context.stock_list))


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
    pass


# after_trading函数会在每天交易结束后被调用，当天只会被调用一次
def after_trading(context):
    pass
```

day4\_06 started regression method: 0.75 sharpe ration 19% worst down;&#x20;

```
    context.weights = np.array(
        [-0.01864979, -0.04537212, -0.18487143, -0.06092573, 0.18599453, -0.02088234, 0.03341527, 0.91743347,
         -0.8066782])
```

```

# 1、回测区间：回归(2014-01-01~2016-01-01)
#           回测2016-01-01  ~  2018-01-01
# 2、选股：
#   选股区间：沪深300
#   选股因子：经过因子分析之后的若干因子，可以不知方向
#   选股权重：回归训练的权重
#   数据处理：缺失值、去极值、标准化、市值中心化处理（防止选股集中）
# 3、调仓周期：
#   调仓：每月进行一次调仓
#   交易规则：卖出已持有的股票
#          买入新的股票池当中的股票

# 步骤
# 准备因子数据
# 处理数据函数
# dealwith_data(context)
# 选股函数(每月调整股票池)
# select_stocklist(context)
# 定期调仓函数
# rebalance(context)
import pandas as pd
import numpy as np
from sklearn.linear_model import LinearRegression


# 在这个方法中编写任何的初始化逻辑。context对象将会在你的算法策略的任何方法之间做传递。
def init(context):
    # 定义沪深300的股票列表
    context.hs300 = index_components("000300.XSHG")

    # 初始化股票因子权重
    context.weights = np.array(
        [-0.01864979, -0.04537212, -0.18487143, -0.06092573, 0.18599453, -0.02088234, 0.03341527, 0.91743347,
         -0.8066782])

    # 定义股票池数量
    context.stocknum = 20

    # 定义定时每月运行的函数
    scheduler.run_monthly(regression_select, tradingday=1)


def regression_select(context, bar_dict):
    """回归法预测选股逻辑
    """
    # 1、查询因子数据
    # 查询因子顺序跟建立回归系数顺序一样
    q = query(fundamentals.eod_derivative_indicator.pe_ratio,
              fundamentals.eod_derivative_indicator.pb_ratio,
              fundamentals.eod_derivative_indicator.market_cap,
              fundamentals.financial_indicator.ev,
              fundamentals.financial_indicator.return_on_asset_net_profit,
              fundamentals.financial_indicator.du_return_on_equity,
              fundamentals.financial_indicator.earnings_per_share,
              fundamentals.income_statement.revenue,
              fundamentals.income_statement.total_expense).filter(fundamentals.stockcode.in_(context.hs300))

    fund = get_fundamentals(q)

    # 行列进行转置
    context.factors_data = fund.T

    # 2、因子（特征值）数据进行处理
    dealwith_data(context)

    # 3、根据每月预测下月的收益率大小替换股票池
    select_stocklist(context)

    # 4、根据股票池的股票列表，进行调仓
    rebalance(context)


def dealwith_data(context):
    """
    contex: 包含因子数据
    需要做的处理：删除空值、去极值、标准化、因子的市值中性化
    """
    # 删除空值
    context.factors_data = context.factors_data.dropna()

    # 市值因子，去做特征值给其它因子中性化处理
    # 市值因子因子不进行去极值、标准化处理
    market_cap_factor = context.factors_data['market_cap']

    # 去极值标准化，循环对每个因子进行处理
    for name in context.factors_data.columns:

        # 对每个因子进行去极值、标准化处理
        context.factors_data[name] = mad(context.factors_data[name])
        context.factors_data[name] = stand(context.factors_data[name])

        # 对因子（除了martket_cap本身不需要中性化）中性化处理
        # 特征值：market_cap_factor
        # 目标： name每个因子
        if name == "market_cap":
            continue

        x = market_cap_factor
        y = context.factors_data[name]

        # 建立回归方程、市值中性化
        lr = LinearRegression()

        # x:要求二维，y：要求1维
        lr.fit(x.reshape(-1, 1), y)

        y_predict = lr.predict(x.reshape(-1, 1))

        # 得出误差进行替换原有因子值
        context.factors_data[name] = y - y_predict


def select_stocklist(context):
    """
    回归计算预测得出收益率结果，筛选收益率高的股票
    """

    # 特征值是：context.factors_data （300， 9）
    # 系数：因子权重
    # 进行矩阵运算，预测收益率
    # （m行，n列）* （n行，l列） = （m行， l列）
    # (300, 9) * (9, 1) = (300, 1)
    # logger.info(context.factors_data.shape)

    # 预测收益率，如果收益高，那么接下来的下一个月都持有收益高的
    # 这写股票
    stock_return = np.dot(context.factors_data.values, context.weights)

    # 赋值给因子数据,注意都是默认对应的股票代码和收益率
    context.factors_data['stock_return'] = stock_return

    # 进行收益率的排序
    # 修改：按照从大到小的收益率排序，选择前
    context.stock_list = context.factors_data.sort_values(by='stock_return', ascending=False).index[:context.stocknum]

    # logger.info(context.stock_list)


def rebalance(context):
    """进行调仓位的函数
    """
    # 卖出
    for stock in context.portfolio.positions.keys():

        if context.portfolio.positions[stock].quantity > 0:

            if stock not in context.stock_list:
                order_target_percent(stock, 0)

    # 买入
    for stock in context.stock_list:
        order_target_percent(stock, 1.0 / context.stocknum)


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
    pass


# after_trading函数会在每天交易结束后被调用，当天只会被调用一次
def after_trading(context):
    pass


def mad(factor):
    """3倍中位数去极值
    """
    # 求出因子值的中位数
    med = np.median(factor)

    # 求出因子值与中位数的差值，进行绝对值
    mad = np.median(np.abs(factor - med))

    # 定义几倍的中位数上下限
    high = med + (3 * 1.4826 * mad)
    low = med - (3 * 1.4826 * mad)

    # 替换上下限以外的值
    factor = np.where(factor > high, high, factor)
    factor = np.where(factor < low, low, factor)
    return factor


def stand(factor):
    """标准化
    """
    mean = np.mean(factor)
    std = np.std(factor)
    return (factor - mean) / std# 可以自己import我们平台支持的第三方python模块，比如pandas、numpy等。

# 1、回测区间：回归(2014-01-01~2016-01-01)
#           回测2016-01-01  ~  2018-01-01
# 2、选股：
#   选股区间：沪深300
#   选股因子：经过因子分析之后的若干因子，可以不知方向
#   选股权重：回归训练的权重
#   数据处理：缺失值、去极值、标准化、市值中心化处理（防止选股集中）
# 3、调仓周期：
#   调仓：每月进行一次调仓
#   交易规则：卖出已持有的股票
#          买入新的股票池当中的股票

# 步骤
# 准备因子数据
# 处理数据函数
# dealwith_data(context)
# 选股函数(每月调整股票池)
# select_stocklist(context)
# 定期调仓函数
# rebalance(context)
import pandas as pd
import numpy as np
from sklearn.linear_model import LinearRegression

# 在这个方法中编写任何的初始化逻辑。context对象将会在你的算法策略的任何方法之间做传递。
def init(context):
    # 定义沪深300的股票列表
    context.hs300 = index_components("000300.XSHG")

    # 初始化股票因子权重
    context.weights = np.array([-0.01864979, -0.04537212, -0.18487143, -0.06092573,  0.18599453,-0.02088234,  0.03341527,  0.91743347, -0.8066782])

    # 定义股票池数量
    context.stocknum = 20

    # 定义定时每月运行的函数
    scheduler.run_monthly(regression_select, tradingday=1)


def regression_select(context, bar_dict):
    """回归法预测选股逻辑
    """
    # 1、查询因子数据
    # 查询因子顺序跟建立回归系数顺序一样
    q = query(fundamentals.eod_derivative_indicator.pe_ratio,
            fundamentals.eod_derivative_indicator.pb_ratio,
            fundamentals.eod_derivative_indicator.market_cap,
            fundamentals.financial_indicator.ev,
            fundamentals.financial_indicator.return_on_asset_net_profit,
            fundamentals.financial_indicator.du_return_on_equity,
            fundamentals.financial_indicator.earnings_per_share,
            fundamentals.income_statement.revenue,
            fundamentals.income_statement.total_expense).filter(fundamentals.stockcode.in_(context.hs300))

    fund = get_fundamentals(q)

    # 行列进行转置
    context.factors_data = fund.T

    # 2、因子（特征值）数据进行处理
    dealwith_data(context)

    # 3、根据每月预测下月的收益率大小替换股票池
    select_stocklist(context)

    # 4、根据股票池的股票列表，进行调仓
    rebalance(context)


def dealwith_data(context):
    """
    contex: 包含因子数据
    需要做的处理：删除空值、去极值、标准化、因子的市值中性化
    """
    # 删除空值
    context.factors_data = context.factors_data.dropna()

    # 市值因子，去做特征值给其它因子中性化处理
    # 市值因子因子不进行去极值、标准化处理
    market_cap_factor = context.factors_data['market_cap']

    # 去极值标准化，循环对每个因子进行处理
    for name in context.factors_data.columns:

        # 对每个因子进行去极值、标准化处理
        context.factors_data[name] = mad(context.factors_data[name])
        context.factors_data[name] = stand(context.factors_data[name])

        # 对因子（除了martket_cap本身不需要中性化）中性化处理
        # 特征值：market_cap_factor
        # 目标： name每个因子
        if name == "market_cap":
            continue

        x = market_cap_factor
        y = context.factors_data[name]

        # 建立回归方程、市值中性化
        lr = LinearRegression()

        # x:要求二维，y：要求1维
        lr.fit(x.reshape(-1, 1), y)

        y_predict = lr.predict(x.reshape(-1, 1))

        # 得出误差进行替换原有因子值
        context.factors_data[name] = y - y_predict


def select_stocklist(context):
    """
    回归计算预测得出收益率结果，筛选收益率高的股票
    """

    # 特征值是：context.factors_data （300， 9）
    # 系数：因子权重
    # 进行矩阵运算，预测收益率
    # （m行，n列）* （n行，l列） = （m行， l列）
    # (300, 9) * (9, 1) = (300, 1)
    # logger.info(context.factors_data.shape)

    # 预测收益率，如果收益高，那么接下来的下一个月都持有收益高的
    # 这写股票
    stock_return = np.dot(context.factors_data.values, context.weights)

    # 赋值给因子数据,注意都是默认对应的股票代码和收益率
    context.factors_data['stock_return'] = stock_return

    # 进行收益率的排序
    # 修改：按照从大到小的收益率排序，选择前
    context.stock_list = context.factors_data.sort_values(by='stock_return', ascending=False).index[:context.stocknum]

    # logger.info(context.stock_list)

def rebalance(context):
    """进行调仓位的函数
    """
    # 卖出
    for stock in context.portfolio.positions.keys():

        if context.portfolio.positions[stock].quantity > 0:

            if stock not in context.stock_list:

                order_target_percent(stock, 0)

    # 买入
    for stock in context.stock_list:

        order_target_percent(stock, 1.0 / context.stocknum)


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
    pass

# after_trading函数会在每天交易结束后被调用，当天只会被调用一次
def after_trading(context):
    pass


def mad(factor):
    """3倍中位数去极值
    """
    # 求出因子值的中位数
    med = np.median(factor)

    # 求出因子值与中位数的差值，进行绝对值
    mad = np.median(np.abs(factor - med))

    # 定义几倍的中位数上下限
    high = med + (3 * 1.4826 * mad)
    low = med - (3 * 1.4826 * mad)

    # 替换上下限以外的值
    factor = np.where(factor > high, high, factor)
    factor = np.where(factor < low, low, factor)
    return factor

def stand(factor):
    """标准化
    """
    mean = np.mean(factor)
    std = np.std(factor)
    return (factor - mean)/std
```

PCA

```


# # 利用alphalens进行因子收益率 分析

# In[97]:


tears.create_returns_tear_sheet(factor_return)


# In[99]:


performance.factor_returns(factor_return).iloc[:, 0].mean()


# In[ ]:


# IC_basic_earnings_per_share
# IC_return_on_equity


# In[137]:


st.spearmanr(IC_basic_earnings_per_share.iloc[:, 0], IC_return_on_equity.iloc[:, 0])


# # 进行因子合成

# In[139]:


# 对于因子的暴露度值进行合成
earn_return = pd.DataFrame()


# In[141]:


for i in range(len(date)):
  
  # 取出两个因子数据
  q = query(fundamentals.income_statement.basic_earnings_per_share,
           fundamentals.financial_indicator.return_on_equity)
  
  fund = get_fundamentals(q, entry_date=date[i])[:, 0, :]
  
  earn_return = pd.concat([earn_return, fund])


# In[144]:


earn_return['basic_earnings_per_share'] = earn_return['basic_earnings_per_share'].fillna(earn_return['basic_earnings_per_share'].mean())
earn_return['return_on_equity'] = earn_return['return_on_equity'].fillna(earn_return['return_on_equity'].mean())


# In[ ]:





# In[142]:


from sklearn.decomposition import PCA


# In[146]:


pca = PCA(n_components=1)


# In[148]:


pca.fit_transform(earn_return[['basic_earnings_per_share', 'return_on_equity']])


# In[ ]:





```

alphalens tutorial in day3 - pandas has 3 major data structures: series, dataframe, panel/multindex

```
# backtesting single factor analysis
# 可以自己import我们平台支持的第三方python模块，比如pandas、numpy等。
# 对每个因子，在股票位置上（因子的方向）上的一个回测结果统计
import pandas as pd
import numpy as np


# 在这个方法中编写任何的初始化逻辑。context对象将会在你的算法策略的任何方法之间做传递。
def init(context):
    context.quantile = 1
    # # 在context中保存全局变量
    # context.s1 = "000001.XSHE"
    # # 实时打印日志
    # logger.info("RunInfo: {}".format(context.run_info))
    scheduler.run_monthly(single_test, tradingday=1)


def single_test(context, bar_dict):
    # 查询因子值
    q = query(fundamentals.income_statement.basic_earnings_per_share)

    fund = get_fundamentals(q)

    fund = fund.T

    # 因子的数据去极值和标准化
    fund['basic_earnings_per_share'] = mad(fund['basic_earnings_per_share'])
    fund['basic_earnings_per_share'] = stand(fund['basic_earnings_per_share'])

    data = fund.iloc[:, 0]

    # 按照分位数进行股票分组回测
    # 0.2, 0.4，0.6，0.8
    if context.quantile == 1:
        data = data[data <= data.quantile(0.2)]
    elif context.quantile == 2:
        data = data[(data > data.quantile(0.2)) & (data <= data.quantile(0.4))]
    elif context.quantile == 3:
        data = data[(data > data.quantile(0.4)) & (data <= data.quantile(0.6))]
    elif context.quantile == 4:
        data = data[(data > data.quantile(0.6)) & (data <= data.quantile(0.8))]
    elif context.quantile == 5:
        data = data[data > data.quantile(0.8)]

    # 确定每次回测的股票池
    context.stock_list = data.index


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


# after_trading函数会在每天交易结束后被调用，当天只会被调用一次
def after_trading(context):
    pass


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


```

class material chrome opens image; safari no image;  [http://quantopian.github.io/alphalens/index.html](http://quantopian.github.io/alphalens/index.html) alphalens was from zipline

biggest problem is no quality data - valueline, fidelity not allowing download data; sp netadvantage? yahoo is only thing?&#x20;

```
# from sectionx.html files, must open each one individually to see
1、 多因子策略流程

多因子策略流程

重在因子的探索和处理

我们可以得出以下步骤

因子挖掘
因子数据的处理
去极值
标准化
中性化
单因子的有效性检测
因子IC分析
因子收益率分析
因子的方向
多因子相关性和和组合分析
因子相关性
因子合成
回测
多因子选股的权重
调仓周期
2、 多因子策略确定的事情

1、选择哪些因子和因子的方向确定
2、因子的权重(打分法和回归法)与调仓周期
其中1步骤属于因子的探索和处理部分，2和3步骤属于选好因子回测部分

基本面数据因子(特征)如此之多，那么如何去找到对应的对股票收益率比较好的。并且能在未来一段时间给我们的选股收益率提供帮助。

2、挖掘因子的过程

我们可以大概从这几个方面去做

1、先从上百个因子当中分析出对股票收益率有效的部分因子（这个数量可以根据筛选的严格程度去做）
在每个大类因子当中去做筛选，每个大类因子中筛选出有效的N个因子
质量、估值、成长等因子
严格：例如20个有效因子
不严格：例如50个有效因子
2、在筛选的单个因子当中做相关性分析，合并相关性强的因子
最终得出有效的，相关性弱的因子，数量不多，一般在10个左右
海选—>N个因子—>精选—>n个因子

N > n
```

去极值 not to remove data only change it to normal range [\
scipy.stats.mstats.winsorize ](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.mstats.winsorize.html)

```
fund = get_fundamentals(query(fundamentals.eod_derivative_indicator.pe_ratio), entry_date="20180102")[:, 0, :]
fund['pe_ration_winsorize'] = winsorize(fund['pe_ratio'], limits=0.025)
# graphize
fund['pe_ratio'][:500].plot()
fund['pe_ration_winsorize'][:500].plot()
#self code:
def quantile(factor,up,down):
  """自实现分位数去极值"""
  up_scale = np.percentile(factor, up)
  down_scale = np.percentile(factor, down)
  factor = np.where(factor > up_scale, up_scale, factor)
  factor = np.where(factor < down_scale, down_scale, factor)
  return factor

quantile(fund['pe_ratio'], 97.5, 2.5)
```

gaussian=3sigma not used much; use MAD; [https://stackoverflow.com/questions/40758562/can-anyone-explain-me-standardscaler](https://stackoverflow.com/questions/40758562/can-anyone-explain-me-standardscaler) to make mean at 0 and 1 for std;

market cap neutralize as 3rd step; &#x20;

```
原因:防⽌止回测时候选股集中
```

```
市值中性化处理理 因⼦子选股回测的时候，防⽌止选到的股票集中在固定的某些股票当中 去除其它的因⼦子存在的市值的影响
回归法去进⾏行行去除
建⽴立某因⼦子跟市值之间的⼀一个回归⽅方程，得出系数 最终预测的结果与因⼦子之间的差值就是不不受影响的部分
中性化处理理:
     原因:防⽌止回测时候选股集中
     原理理:建⽴立回归关系
市值中⼼心化选股对⽐比
  市值中性化处理理:定期分散骚不不同股票⾥里里⾯面
  没有市值中性化处理理:⽐比较集中在某些股票

steps - example p/b vs market cap
# 获取两个因子数据
# 对目标值因子-市净率进行去极值、标准化处理
# 建立市值与市净率回归方程
# 通过回归系数，预测新的因子结果y_predict
# 求出市净率与y_predict的偏差即为新的因子值

# 1、 获取数据
q = query(fundamentals.eod_derivative_indicator.pb_ratio,
         fundamentals.eod_derivative_indicator.market_cap)

fund = get_fundamentals(q, entry_date="2018-01-03")[:, 0, :]
# 2、对因子数据进行处理,3倍中位数、stand
fund['pb_ratio'] = mad(fund['pb_ratio'])
fund['pb_ratio'] = stand(fund['pb_ratio'])
# 对于市值因子可以选择不处理
# 3、确定建立回归方程特征值和目标值
# 传入训练的特征值是二维的形状
x = fund['market_cap'].reshape(-1, 1)
y = fund['pb_ratio']
# 4、利用线性回归进行预测
lr = LinearRegression()
lr.coef_ # can get lr params
lr.intercept_
lr.fit(x, y)
# 5、得出每个预测值，让因子的真实值-预测值得出的误差，就为我们中性化处理之后的结果
y_predict = lr.predict(x)
res = y - y_predict
fund['pb_ratio'] = res //use the market-cap neutralized result for p/b ratio
print(fund)

```

```
# with market cap neutralizing -
#
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

[https://www.bilibili.com/medialist/play/ml1197302639/BV1Ft4y1B7XR](https://www.bilibili.com/medialist/play/ml1197302639/BV1Ft4y1B7XR) wind data is best, provides python and matlab interface; has daily limit of GB; 3 months to get 2007 data; only get updates; filter factors IC(information coefficient) usually get topmost 10-60 factors; backtesting system; bundle is using HDF5 for china stocks only; 统计套利 statistical arbitrage

**market neutral - alpha strategy - factor seiving** - factor collection and tracking performance - testing factors back\_testing, see which ones perform best; constantly tracking and updating factors monthly; factors have cycles; 50 factors collection, use only those 15 factors which back test best, the other 35 as backups. factor base important to maintain. factor scoring system;

multifactor trading not so frequent, can manually trade. futures options as hedging - for protection when factors not working. or just stop trading for a while. zipline Q, jointquant

SMA or EMA not as good as multi-factor analysis;  [https://www.bilibili.com/video/BV1as411c7dM?p=8](https://www.bilibili.com/video/BV1as411c7dM?p=8) stats arbitrage p8 #5 matlab  在自动交易或算法交易方面，主要有五种不同的交易策略。它们分别是动量、均值复归、做市、统计套利、情绪交易。[https://www.bilibili.com/video/BV1qt4y1X7HL?from=search\&seid=4290473018830660140](https://www.bilibili.com/video/BV1qt4y1X7HL?from=search\&seid=4290473018830660140) arb

[https://www.ricequant.com/doc/rqdata/python/factors-dictionary.html#alpha101-%E5%9B%A0%E5%AD%90](https://www.ricequant.com/doc/rqdata/python/factors-dictionary.html#alpha101-%E5%9B%A0%E5%AD%90) list of alpha factors  [https://www.ricequant.com/doc/rqdata/python/fundamentals-dictionary.html#%E7%BB%8F%E8%90%A5%E8%A1%8D%E7%94%9F%E6%8C%87%E6%A0%87%E8%A1%A8](https://www.ricequant.com/doc/rqdata/python/fundamentals-dictionary.html#%E7%BB%8F%E8%90%A5%E8%A1%8D%E7%94%9F%E6%8C%87%E6%A0%87%E8%A1%A8) list of fundamentals;&#x20;

day1 is introduction; day2-02 has intro to multifactor theory and histories; pandas panel datatype is replaced by multindex

[https://github.com/ricequant/rqalpha/blob/master/docs/source/api/base\_api.rst](https://github.com/ricequant/rqalpha/blob/master/docs/source/api/base\_api.rst) base api, has get\__fundamental() depreciated use get\_factor()_

```
import yahoo_fin.stock_info as si
import html5lib
import pandas as pd

quote = si.get_quote_table("aapl")

# get list of Dow tickers
#dow_list = si.tickers_dow()
#dow_list = si.tickers_sp500()
dow_list = ['NVO', 'BIDU', 'DPZ', 'AAPL', 'ABBV']

>>> dow_list #dpz is in sp500
['NVO', 'BIDU', 'A', 'AAL', 'AAP', 'AAPL', 'ABBV', 'ABC', 'ABMD', 'ABT', 'ACN', 'ADBE', 'ADI', 'ADM', 'ADP', 'ADSK', 'AEE', 'AEP', 'AES', 'AFL', 'AIG', 'AIZ', 'AJG', 'AKAM', 'ALB', 'ALGN', 'ALK', 'ALL', 'ALLE', 'ALXN', 'AMAT', 'AMCR', 'AMD', 'AME', 'AMGN', 'AMP', 'AMT', 'AMZN', 'ANET', 'ANSS', 'ANTM', 'AON', 'AOS', 'APA', 'APD', 'APH', 'APTV', 'ARE', 'ATO', 'ATVI', 'AVB', 'AVGO', 'AVY', 'AWK', 'AXP', 'AZO', 'BA', 'BAC', 'BAX', 'BBY', 'BDX', 'BEN', 'BF-B', 'BIIB', 'BIO', 'BK', 'BKNG', 'BKR', 'BLK', 'BLL', 'BMY', 'BR', 'BRK-B', 'BSX', 'BWA', 'BXP', 'C', 'CAG', 'CAH', 'CARR', 'CAT', 'CB', 'CBOE', 'CBRE', 'CCI', 'CCL', 'CDNS', 'CDW', 'CE', 'CERN', 'CF', 'CFG', 'CHD', 'CHRW', 'CHTR', 'CI', 'CINF', 'CL', 'CLX', 'CMA', 'CMCSA', 'CME', 'CMG', 'CMI', 'CMS', 'CNC', 'CNP', 'COF', 'COG', 'COO', 'COP', 'COST', 'CPB', 'CPRT', 'CRM', 'CSCO', 'CSX', 'CTAS', 'CTLT', 'CTSH', 'CTVA', 'CTXS', 'CVS', 'CVX', 'D', 'DAL', 'DD', 'DE', 'DFS', 'DG', 'DGX', 'DHI', 'DHR', 'DIS', 'DISCA', 'DISCK', 'DISH', 'DLR', 'DLTR', 'DOV', 'DOW', 'DPZ', 'DRE', 'DRI', 'DTE', 'DUK', 'DVA', 'DVN', 'DXC', 'DXCM', 'EA', 'EBAY', 'ECL', 'ED', 'EFX', 'EIX', 'EL', 'EMN', 'EMR', 'ENPH', 'EOG', 'EQIX', 'EQR', 'ES', 'ESS', 'ETN', 'ETR', 'ETSY', 'EVRG', 'EW', 'EXC', 'EXPD', 'EXPE', 'EXR', 'F', 'FANG', 'FAST', 'FB', 'FBHS', 'FCX', 'FDX', 'FE', 'FFIV', 'FIS', 'FISV', 'FITB', 'FLIR', 'FLS', 'FLT', 'FMC', 'FOX', 'FOXA', 'FRC', 'FRT', 'FTNT', 'FTV', 'GD', 'GE', 'GILD', 'GIS', 'GL', 'GLW', 'GM', 'GOOG', 'GOOGL', 'GPC', 'GPN', 'GPS', 'GRMN', 'GS', 'GWW', 'HAL', 'HAS', 'HBAN', 'HBI', 'HCA', 'HD', 'HES', 'HFC', 'HIG', 'HII', 'HLT', 'HOLX', 'HON', 'HPE', 'HPQ', 'HRL', 'HSIC', 'HST', 'HSY', 'HUM', 'HWM', 'IBM', 'ICE', 'IDXX', 'IEX', 'IFF', 'ILMN', 'INCY', 'INFO', 'INTC', 'INTU', 'IP', 'IPG', 'IPGP', 'IQV', 'IR', 'IRM', 'ISRG', 'IT', 'ITW', 'IVZ', 'J', 'JBHT', 'JCI', 'JKHY', 'JNJ', 'JNPR', 'JPM', 'K', 'KEY', 'KEYS', 'KHC', 'KIM', 'KLAC', 'KMB', 'KMI', 'KMX', 'KO', 'KR', 'KSU', 'L', 'LB', 'LDOS', 'LEG', 'LEN', 'LH', 'LHX', 'LIN', 'LKQ', 'LLY', 'LMT', 'LNC', 'LNT', 'LOW', 'LRCX', 'LUMN', 'LUV', 'LVS', 'LW', 'LYB', 'LYV', 'MA', 'MAA', 'MAR', 'MAS', 'MCD', 'MCHP', 'MCK', 'MCO', 'MDLZ', 'MDT', 'MET', 'MGM', 'MHK', 'MKC', 'MKTX', 'MLM', 'MMC', 'MMM', 'MNST', 'MO', 'MOS', 'MPC', 'MPWR', 'MRK', 'MRO', 'MS', 'MSCI', 'MSFT', 'MSI', 'MTB', 'MTD', 'MU', 'MXIM', 'NCLH', 'NDAQ', 'NEE', 'NEM', 'NFLX', 'NI', 'NKE', 'NLOK', 'NLSN', 'NOC', 'NOV', 'NOW', 'NRG', 'NSC', 'NTAP', 'NTRS', 'NUE', 'NVDA', 'NVR', 'NWL', 'NWS', 'NWSA', 'O', 'ODFL', 'OKE', 'OMC', 'ORCL', 'ORLY', 'OTIS', 'OXY', 'PAYC', 'PAYX', 'PBCT', 'PCAR', 'PEAK', 'PEG', 'PEP', 'PFE', 'PFG', 'PG', 'PGR', 'PH', 'PHM', 'PKG', 'PKI', 'PLD', 'PM', 'PNC', 'PNR', 'PNW', 'POOL', 'PPG', 'PPL', 'PRGO', 'PRU', 'PSA', 'PSX', 'PVH', 'PWR', 'PXD', 'PYPL', 'QCOM', 'QRVO', 'RCL', 'RE', 'REG', 'REGN', 'RF', 'RHI', 'RJF', 'RL', 'RMD', 'ROK', 'ROL', 'ROP', 'ROST', 'RSG', 'RTX', 'SBAC', 'SBUX', 'SCHW', 'SEE', 'SHW', 'SIVB', 'SJM', 'SLB', 'SLG', 'SNA', 'SNPS', 'SO', 'SPG', 'SPGI', 'SRE', 'STE', 'STT', 'STX', 'STZ', 'SWK', 'SWKS', 'SYF', 'SYK', 'SYY', 'T', 'TAP', 'TDG', 'TDY', 'TEL', 'TER', 'TFC', 'TFX', 'TGT', 'TJX', 'TMO', 'TMUS', 'TPR', 'TRMB', 'TROW', 'TRV', 'TSCO', 'TSLA', 'TSN', 'TT', 'TTWO', 'TWTR', 'TXN', 'TXT', 'TYL', 'UA', 'UAA', 'UAL', 'UDR', 'UHS', 'ULTA', 'UNH', 'UNM', 'UNP', 'UPS', 'URI', 'USB', 'V', 'VAR', 'VFC', 'VIAC', 'VLO', 'VMC', 'VNO', 'VNT', 'VRSK', 'VRSN', 'VRTX', 'VTR', 'VTRS', 'VZ', 'WAB', 'WAT', 'WBA', 'WDC', 'WEC', 'WELL', 'WFC', 'WHR', 'WLTW', 'WM', 'WMB', 'WMT', 'WRB', 'WRK', 'WST', 'WU', 'WY', 'WYNN', 'XEL', 'XLNX', 'XOM', 'XRAY', 'XRX', 'XYL', 'YUM', 'ZBH', 'ZBRA', 'ZION', 'ZTS']



        #scheduler调用的函数需要包括context, bar_dict两个参数
        def query_fundamental(context, bar_dict):
                # 查询revenue前十名的公司的股票并且他们的pe_ratio在25和30之间。打fundamentals的时候会有auto-complete方便写查询代码。
            fundamental_df = get_fundamentals(
                query(
                    fundamentals.income_statement.revenue, fundamentals.eod_derivative_indicator.pe_ratio
                ).filter(
                    fundamentals.eod_derivative_indicator.pe_ratio > 25
                ).filter(
                    fundamentals.eod_derivative_indicator.pe_ratio < 30
                ).order_by(
                    fundamentals.income_statement.revenue.desc()
                ).limit(
                    10
                )
            )

            # 将查询结果dataframe的fundamental_df存放在context里面以备后面只需：
            context.fundamental_df = fundamental_df

            # 实时打印日志看下查询结果，会有我们精心处理的数据表格显示：
            logger.info(context.fundamental_df)
            update_universe(context.fundamental_df.columns.values)

         # 在这个方法中编写任何的初始化逻辑。context对象将会在你的算法策略的任何方法之间做传递。
        def init(context):
            # 每月的第一个交易日查询以下财务数据，以确保可以拿到最新更新的财务数据信息用来调整仓位
            scheduler.run_monthly(query_fundamental, tradingday=1)
```
