---
description: number data math; check save delete; quantify codify model
---

# quant



\
\




[https://github.com/lbwdruid](https://github.com/lbwdruid) api for stock in Ameritrade, IB [https://www.bilibili.com/video/BV1SW411A7Ab?p=13](https://www.bilibili.com/video/BV1SW411A7Ab?p=13) bili

[https://quant.gtja.com/study?f=home\&m=memu](https://quant.gtja.com/study?f=home\&m=memu) gtja quant

[https://github.com/Gary-Hertel/StockQuant](https://github.com/Gary-Hertel/StockQuant) bili [https://www.bilibili.com/video/BV1Hz4y117TQ?p=9](https://www.bilibili.com/video/BV1Hz4y117TQ?p=9) only macd stuff no fundamentals



[https://www.bilibili.com/medialist/play/ml1197568039/BV1zK411u7uG](https://www.bilibili.com/medialist/play/ml1197568039/BV1zK411u7uG) stockquant



'WHR', 'CLW', 'CI', 'HII', 'PHM', 'DHI', 'SJM', 'LEN', 'DHT', 'CAH',

&#x20;      'ALL', 'DGX', 'NAVI', 'HIG', 'UNM', 'UHS', 'BBY', 'IVZ', 'PGR', 'OMC'



[https://pypi.org/project/Backtesting/](https://pypi.org/project/Backtesting/) how to backtesting strategy?



[https://www.quantstart.com/articles/backtesting-systematic-trading-strategies-in-python-considerations-and-open-source-frameworks/](https://www.quantstart.com/articles/backtesting-systematic-trading-strategies-in-python-considerations-and-open-source-frameworks/) list of backtesting websites? [https://github.com/gbeced/pyalgotrade/blob/master/doc/tutorial.rst](https://github.com/gbeced/pyalgotrade/blob/master/doc/tutorial.rst) code



SNP insider should be 0.68! ROE has 11 max, -inf, SNP is 0.1% ROE?? missing data hurts

```
# 9 and 9 factors as of march 2021; each equal to the other
# "priceToBookscore", 
# "trailingPEscore", 
# "priceToSalesTrailing12Monthsscore" - lower is better; need to be asceding!! wrong before
# enterpriseToEbitda - lower is better



# "earningsQuarterlyGrowthscore", 
# "ROEscore", 
# "heldPercentInstitutionsscore", 
# "heldPercentInsidersscore", 
# "fiveYearAvgDividendYieldscore", 



```

Index(\['WHR', 'CLW', 'CI', 'HII', 'PHM', 'DHI', 'SJM', 'CAH', 'DHT', 'LEN',

&#x20;      'ALL', 'NAVI', 'HIG', 'IVZ', 'DGX', 'UNM', 'PGR', 'UHS', 'CE', 'WU'],

\
adding priceToSalesTrailing12Monthsscore\


'KIM', 'NAVI', 'COO', 'CE', 'ARE', 'AGO', 'DJCO', 'EXR', 'BIO', 'FTV',

&#x20;      'STZ', 'WHR', 'IVZ', 'SNA', 'TROW', 'CI', 'MS', 'DHI', 'SNP', 'ATO']

with 5 year dividend yield:

\


Index(\['NAVI', 'KIM', 'CE', 'WHR', 'SNP', 'CLW', 'COO', 'CI', 'SJM', 'IVZ',

&#x20;      'AGO', 'DHI', 'HII', 'DHT', 'CINF', 'PHM', 'LEN', 'DGX', 'BIO', 'SNA'],

insiders and institutional:\


Index(\['CE', 'NAVI', 'CI', 'DHI', 'LEN', 'LH', 'DISH', 'EBAY', 'PHM', 'REGN',

&#x20;      'CLW', 'SJM', 'COO', 'KNX', 'HOLX', 'PKI', 'UHS', 'BIO', 'ZION', 'PGR'],

no peg:\


'CLW', 'NAVI', 'CI', 'COO', 'CE', 'BIO', 'DHI', 'LH', 'FTV', 'KIM',

&#x20;      'LEN', 'REGN', 'WHR', 'DJCO', 'PHM', 'EBAY', 'UHS', 'HII', 'AGO',

&#x20;      'DGX']

insider no institutional:\


Index(\['DHI', 'DISH', 'CI', 'PHM', 'SJM', 'NAVI', 'UHAL', 'CE', 'LEN', 'AFL',

&#x20;      'MS', 'BIO', 'FRO', 'CLW', 'REGN', 'EBAY', 'LH', 'PGR', 'LYB', 'GS'],

&#x20;  &#x20;

peg only:\


Index(\['CI', 'CE', 'DHI', 'PGR', 'SLG', 'DISH', 'NAVI', 'SJM', 'LH', 'PHM',

&#x20;      'LEN', 'GS', 'CLW', 'CAH', 'NEM', 'EBAY', 'HOLX', 'RF', 'UHAL', 'AFL'],

no peg:\


Index(\['CLW', 'CI', 'BIO', 'COO', 'DJCO', 'ALL', 'DHI', 'CE', 'DGX', 'KIM',

&#x20;      'SNP', 'LH', 'HII', 'FTLF', 'PGR', 'PKX', 'WHR', 'SLG', 'DISH', 'PTR'],

no peg with insider:\


Index(\['DJCO', 'CLW', 'BIO', 'CI', 'DHI', 'KIM', 'FTLF', 'MHK', 'FTV', 'UHAL',

&#x20;      'AFL', 'MS', 'HII', 'CINF', 'DISH', 'COO', 'NAVI', 'PHM', 'CE', 'DHT'],

no peg with institutional:\


'COO', 'CE', 'CLW', 'CI', 'NAVI', 'LH', 'WHR', 'DGX', 'LEN', 'KIM',

&#x20;      'BIO', 'HOLX', 'PKI', 'REGN', 'DHI', 'ALL', 'PHM', 'HII', 'EBAY',

&#x20;      'FTV'],

for data outside of yahoo, scrape sec edgar or pay quandl

[https://github.com/robren/quandl\_fund\_xlsx](https://github.com/robren/quandl\_fund\_xlsx) 'quandl\_fund\_xlsx -i lines.csv' command-line tool to download a file list of stocks into one file; SF0 free; SF1 [https://www.quandl.com/databases/SF1/data](https://www.quandl.com/databases/SF1/data) SF1 29$/month 199/year; RB1 50/month; MF1 70/month

[https://help.quandl.com/article/154-what-is-quandls-nomenclature-system-for-data](https://help.quandl.com/article/154-what-is-quandls-nomenclature-system-for-data) explains&#x20;



\
stock dimensions - how many? what are the major ones? what is a business? assets that can bring in money; things that can make money; how can one bring in money? by assets; it's number data math decision choice that brings in money;&#x20;



[https://xueqiu.com/6723912277/26635764](https://xueqiu.com/6723912277/26635764) roa roe roic roec [https://xueqiu.com/5632367028/67528224](https://xueqiu.com/5632367028/67528224) 2 [https://xueqiu.com/5632367028/67528239](https://xueqiu.com/5632367028/67528239) 3

what changes are the data from yahoo? is it accurate and up to date?

diff --suppress-common-lines -iy 2021-03-13-08-11.csv 2021-03-13-08-55.csv

compare lines and show differences in one line; most changes are price related: PB, PE, volume, marketCap keeps changing with price; how many dimensions describe a stock? there are many many dimenstions, too many dimensions; what are the major ones? too many dimensions can distort the reality; the result may not be good with too many factors. 6 factor is enough? what should be in the model?

renaissance has 100B spread out in 5000 stocks; each stock averages 20M dollars; they got too much money, can't concentrate on a small number of stocks, otherwise it'll dominate the stock market. rentech doesn't have any other way except to diversify, they have no choice to concentrate, they are forced to diversity because of too much money. You have <1M dollars, why would you need to diversify? You should not diversify untill one stock holding reaches 1-20M dollars.

rentech smallest holding is 19000 dollars; say 20k.  largest china holding is china mobile 195M, next is snp 136M; top3 nvo bidu pdd;&#x20;

yes ptr earns lots of money; but how much of it goes to shareholders? most earnings goes to central government, not shareholders like you. not shareholder oriented. so those big earnings look nice but they don't belong to you, has nothing to do with you;

money is data money is number money is math; how much rentech is in china? a lot; where is bidu and pdd ranked my way?

number data math \[{(......)}] about all grouping, \[ ( { in any programming language; matrix=table=dateframe

download data always broke in the middle, no reliable source of data.

[http://www.edwardothorp.com/wp-content/uploads/2016/11/thorpwilmottqfinrev2003.pdf](http://www.edwardothorp.com/wp-content/uploads/2016/11/thorpwilmottqfinrev2003.pdf) thorpe stuff too old, not useful any more

\
error processing data:

&#x20; factor\[factorname + "score"]\[i \* single\_groupnum:(i + 1) \* single\_groupnum] = i + 1

/Users/rsdata/git-dir/haiyang-scratch/python/lilu/y2.py:41: SettingWithCopyWarning:&#x20;

A value is trying to be set on a copy of a slice from a DataFrame

error caused by duplicate double entries - two copies of same data in same file with double size



get HK exchange data? [https://github.com/ChERules/HKEX](https://github.com/ChERules/HKEX) only price information [https://eodhistoricaldata.com/](https://eodhistoricaldata.com/) has hk list [https://www.quandl.com/data/HKEX-Hong-Kong-Exchange/documentation](https://www.quandl.com/data/HKEX-Hong-Kong-Exchange/documentation) quandl [https://eodhistoricaldata.com/exchange/HK](https://eodhistoricaldata.com/exchange/HK) list of hk stocks

trade near end of day - so can sell next day at open



SNP and PTR are exactly same - why bother with 2 of them?diversify?

{% embed url="https://www.investopedia.com/simulator/portfolio/" %}

13 +12+..+1 = 14\*13/2=91% so top one would be 13-14%,&#x20;

75 20 5

50 30 10 4 1

geometric sequence?



if it's a losing position being hold and not in the new list, would it still be sold? buy top 15 or 10 and hold, sell when it dropped out of top 20.

[https://superuser.com/questions/451747/mac-osx-10-6-cron-job-not-running](https://superuser.com/questions/451747/mac-osx-10-6-cron-job-not-running) mac cron weirdness





\
\




\
\
\


\
\


SNP      13.0

CI       14.0

PTR      14.0

BRK-B    16.0

ALL      16.0

CIHKY    17.0

AFL      17.0

DHI      18.0

CLW      18.0

PKX      18.0

BIO      18.0

DJCO     19.0

BABA     19.0

COO      19.0

BAYRY    19.0

AEP      19.0

CE       20.0

ITOCY    20.0

C        20.0

MS       20.0

LYG      20.0

GS       20.0

CB       20.0

GE       21.0

ADM      21.0

CAH      21.0

DGX      21.0

PGR      21.0

CINF     22.0

DISH     22.0

dtype: float64

Index(\['SNP', 'CI', 'PTR', 'BRK-B', 'ALL', 'CIHKY', 'AFL', 'DHI', 'CLW', 'PKX',

&#x20;      'BIO', 'DJCO', 'BABA', 'COO', 'BAYRY', 'AEP', 'CE', 'ITOCY', 'C', 'MS'],

&#x20;     dtype='object')\


Index(\['SNP', 'CI', 'PTR', 'BRK-B', 'CIHKY', 'ALL', 'AFL', 'BIO', 'CLW', 'PKX',

&#x20;      'DHI', 'DJCO', 'BAYRY', 'BAYZF', 'COO', 'AEP', 'CE', 'MS', 'C',

&#x20;      'ITOCY'],

&#x20;     dtype='object')

Index(\['CI', 'ALL', 'CIHKY', 'BRK-B', 'DHI', 'AFL', 'GS', 'BIO', 'AEP', 'CE',

&#x20;      'MS', 'COO', 'FTV', 'PGR', 'CB', 'JPM', 'DGX', 'GE', 'DISH', 'CVS'],

&#x20;     dtype='object')

Index(\['CI', 'CIHKY', 'ALL', 'BRK-B', 'AFL', 'DHI', 'AEP', 'BIO', 'GS', 'COO',

&#x20;      'MS', 'CE', 'PGR', 'DGX', 'JPM', 'DISH', 'CB', 'GE', 'FTV', 'C'],

Index(\['CI', 'ALL', 'BRK-B', 'AEP', 'DHI', 'AFL', 'GS', 'BIO', 'MS', 'COO',

&#x20;      'PGR', 'JPM', 'CE', 'DISH', 'CB', 'FTV', 'GE', 'DGX', 'DLTR', 'INTC'],

Index(\['CI', 'BRK-B', 'AEP', 'ALL', 'GS', 'CB', 'AFL', 'C', 'COF', 'DHI', 'MS',

&#x20;      'ADM', 'DISH', 'BAC', 'JPM', 'CVS', 'GE', 'BK', 'BLK', 'CINF'],



[https://www.ricequant.com/doc/rqdata/python/stock-mod.html#get-fundamentals-%E6%9F%A5%E8%AF%A2%E8%B4%A2%E5%8A%A1%E6%95%B0%E6%8D%AE-%E9%80%80%E5%BD%B9%E4%B8%AD](https://www.ricequant.com/doc/rqdata/python/stock-mod.html#get-fundamentals-%E6%9F%A5%E8%AF%A2%E8%B4%A2%E5%8A%A1%E6%95%B0%E6%8D%AE-%E9%80%80%E5%BD%B9%E4%B8%AD) has get\_fundamentals() return type





yahoo data wrong - xueqiu and fidelity are up to date; yahoo auto fetched data all garbage, even though yahoo webiste data show correct. if it's consistently wrong, still ok to sort and score them - as long as data compare correctly.

[https://codingandfun.com/financial-data-from-yahoo-finance/](https://codingandfun.com/financial-data-from-yahoo-finance/) yahoo use; trailingEps is TTM \
\
NVO,trailingEps,2.462 wrong  should be 2.91; bidu 12.83 vs xueqiu 10 fido 9.89???



```
yahoo info https://pypi.org/project/fix-yahoo-finance/0.1.30/

marketCap
trailingPE
priceToBook
revenueQuarterlyGrowth - no data, = trailingEps - last value
earningsQuarterlyGrowth

netIncomeToCommon 
profitMargins
pegRatio

ROE= trailingEps/ bookValue

fundamentals.eod_derivative_indicator.market_cap,
              fundamentals.eod_derivative_indicator.pe_ratio,
              fundamentals.eod_derivative_indicator.pb_ratio,
              fundamentals.financial_indicator.return_on_invested_capital,
              fundamentals.financial_indicator.inc_revenue,
              fundamentals.financial_indicator.inc_profit_before_tax
```

```
dict_keys(['zip', 'sector', 'fullTimeEmployees', 'longBusinessSummary', 'city', 'phone', 'state', 'country', 'companyOfficers', 'website', 'maxAge', 'address1', 'industry', 'previousClose', 'regularMarketOpen', 'twoHundredDayAverage', 'trailingAnnualDividendYield', 'payoutRatio', 'volume24Hr', 'regularMarketDayHigh', 'navPrice', 'averageDailyVolume10Day', 'totalAssets', 'regularMarketPreviousClose', 'fiftyDayAverage', 'trailingAnnualDividendRate', 'open', 'toCurrency', 'averageVolume10days', 'expireDate', 'yield', 'algorithm', 'dividendRate', 'exDividendDate', 'beta', 'circulatingSupply', 'startDate', 'regularMarketDayLow', 'priceHint', 'currency', 'trailingPE', 'regularMarketVolume', 'lastMarket', 'maxSupply', 'openInterest', 'marketCap', 'volumeAllCurrencies', 'strikePrice', 'averageVolume', 'priceToSalesTrailing12Months', 'dayLow', 'ask', 'ytdReturn', 'askSize', 'volume', 'fiftyTwoWeekHigh', 'forwardPE', 'fromCurrency', 'fiveYearAvgDividendYield', 'fiftyTwoWeekLow', 'bid', 'tradeable', 'dividendYield', 'bidSize', 'dayHigh', 'exchange', 'shortName', 'longName', 'exchangeTimezoneName', 'exchangeTimezoneShortName', 'isEsgPopulated', 'gmtOffSetMilliseconds', 'quoteType', 'symbol', 'messageBoardId', 'market', 'annualHoldingsTurnover', 'enterpriseToRevenue', 'beta3Year', 'profitMargins', 'enterpriseToEbitda', '52WeekChange', 'morningStarRiskRating', 'forwardEps', 'revenueQuarterlyGrowth', 'sharesOutstanding', 'fundInceptionDate', 'annualReportExpenseRatio', 'bookValue', 'sharesShort', 'sharesPercentSharesOut', 'fundFamily', 'lastFiscalYearEnd', 'heldPercentInstitutions', 'netIncomeToCommon', 'trailingEps', 'lastDividendValue', 'SandP52WeekChange', 'priceToBook', 'heldPercentInsiders', 'nextFiscalYearEnd', 'mostRecentQuarter', 'shortRatio', 'sharesShortPreviousMonthDate', 'floatShares', 'enterpriseValue', 'threeYearAverageReturn', 'lastSplitDate', 'lastSplitFactor', 'legalType', 'lastDividendDate', 'morningStarOverallRating', 'earningsQuarterlyGrowth', 'dateShortInterest', 'pegRatio', 'lastCapGain', 'shortPercentOfFloat', 'sharesShortPriorMonth', 'impliedSharesOutstanding', 'category', 'fiveYearAverageReturn', 'regularMarketPrice', 'logo_url'])
```

\


dict\_keys(\['zip', 'sector', 'fullTimeEmployees', 'longBusinessSummary', 'city', 'phone', 'state', 'country', 'companyOfficers', 'website', 'maxAge', 'address1', 'industry', 'previousClose', 'regularMarketOpen', 'twoHundredDayAverage', 'trailingAnnualDividendYield', 'payoutRatio', 'volume24Hr', 'regularMarketDayHigh', 'navPrice', 'averageDailyVolume10Day', 'totalAssets', 'regularMarketPreviousClose', 'fiftyDayAverage', 'trailingAnnualDividendRate', 'open', 'toCurrency', 'averageVolume10days', 'expireDate', 'yield', 'algorithm', 'dividendRate', 'exDividendDate', 'beta', 'circulatingSupply', 'startDate', 'regularMarketDayLow', 'priceHint', 'currency', 'trailingPE', 'regularMarketVolume', 'lastMarket', 'maxSupply', 'openInterest', 'marketCap', 'volumeAllCurrencies', 'strikePrice', 'averageVolume', 'priceToSalesTrailing12Months', 'dayLow', 'ask', 'ytdReturn', 'askSize', 'volume', 'fiftyTwoWeekHigh', 'forwardPE', 'fromCurrency', 'fiveYearAvgDividendYield', 'fiftyTwoWeekLow', 'bid', 'tradeable', 'dividendYield', 'bidSize', 'dayHigh', 'exchange', 'shortName', 'longName', 'exchangeTimezoneName', 'exchangeTimezoneShortName', 'isEsgPopulated', 'gmtOffSetMilliseconds', 'quoteType', 'symbol', 'messageBoardId', 'market', 'annualHoldingsTurnover', 'enterpriseToRevenue', 'beta3Year', 'profitMargins', 'enterpriseToEbitda', '52WeekChange', 'morningStarRiskRating', 'forwardEps', 'revenueQuarterlyGrowth', 'sharesOutstanding', 'fundInceptionDate', 'annualReportExpenseRatio', 'bookValue', 'sharesShort', 'sharesPercentSharesOut', 'fundFamily', 'lastFiscalYearEnd', 'heldPercentInstitutions', 'netIncomeToCommon', 'trailingEps', 'lastDividendValue', 'SandP52WeekChange', 'priceToBook', 'heldPercentInsiders', 'nextFiscalYearEnd', 'mostRecentQuarter', 'shortRatio', 'sharesShortPreviousMonthDate', 'floatShares', 'enterpriseValue', 'threeYearAverageReturn', 'lastSplitDate', 'lastSplitFactor', 'legalType', 'lastDividendDate', 'morningStarOverallRating', 'earningsQuarterlyGrowth', 'dateShortInterest', 'pegRatio', 'lastCapGain', 'shortPercentOfFloat', 'sharesShortPriorMonth', 'impliedSharesOutstanding', 'category', 'fiveYearAverageReturn', 'regularMarketPrice', 'logo\_url'])



python

scrape IBM equity - output two numbers

```
#t3.py scrape IBM equity - output two numbers
#
from bs4 import BeautifulSoup
import requests
import sys

# Access page
cik = '0000051143' #ibm
type = '10-K'
dateb = '20160101'

# Obtain HTML for search page
base_url = "https://www.sec.gov/cgi-bin/browse-edgar?action=getcompany&CIK={}&type={}&dateb={}"
edgar_resp = requests.get(base_url.format(cik, type, dateb))
edgar_str = edgar_resp.text

# Find the document link
doc_link = ''
soup = BeautifulSoup(edgar_str, 'html.parser')
table_tag = soup.find('table', class_='tableFile2')
rows = table_tag.find_all('tr')
for row in rows:
    cells = row.find_all('td')
    if len(cells) > 3:
        if '2015' in cells[3].text:
            doc_link = 'https://www.sec.gov' + cells[1].a['href']

# Exit if document link couldn't be found
if doc_link == '':
    print("Couldn't find the document link")
    sys.exit()

# Obtain HTML for document page
doc_resp = requests.get(doc_link)
doc_str = doc_resp.text

# Find the XBRL link
xbrl_link = ''
soup = BeautifulSoup(doc_str, 'html.parser')
table_tag = soup.find('table', class_='tableFile', summary='Data Files')
rows = table_tag.find_all('tr')
for row in rows:
    cells = row.find_all('td')
    if len(cells) > 3:
        if 'INS' in cells[3].text:
            xbrl_link = 'https://www.sec.gov' + cells[2].a['href']

print(xbrl_link)
#https://www.sec.gov/Archives/edgar/data/51143/000104746915001106/ibm-20141231.xml


# Obtain XBRL text from document
xbrl_resp = requests.get(xbrl_link)
xbrl_str = xbrl_resp.text

# Find and print stockholder's equity
soup = BeautifulSoup(xbrl_str, 'lxml')
tag_list = soup.find_all()
for tag in tag_list:
    if tag.name == 'us-gaap:stockholdersequity':
        print("Stockholder's equity: " + tag.text)    
```

```
#t2.py gets a given company's 10Q directly
import bs4 as bs
import requests
import pandas as pd
import re

company = 'Facebook Inc'
filing = '10-Q'
year = 2020
quarter = 'QTR3'
#get name of all filings
download = requests.get(f'https://www.sec.gov/Archives/edgar/full-index/{year}/{quarter}/master.idx').content
download = download.decode("utf-8").split('\n')
print(download)

#build the first part of the url
for item in download:
  #company name and report type
  if (company in item) and (filing in item):
    #print(item)
    company = item
    company = company.strip()
    splitted_company = company.split('|')
    url = splitted_company[-1]

print(url) #edgar/data/1326801/0001326801-20-000076.txt

url2 = url.split('-')
url2 = url2[0] + url2[1] + url2[2]
url2 = url2.split('.txt')[0]
print(url2) #edgar/data/1326801/000132680120000076

to_get_html_site = 'https://www.sec.gov/Archives/' + url
data = requests.get(to_get_html_site).content
data = data.decode("utf-8")
data = data.split('FILENAME>')
#data[1]
data = data[1].split('\n')[0]

url_to_use = 'https://www.sec.gov/Archives/'+ url2 + '/'+data
print(url_to_use) 
#https://www.sec.gov/Archives/edgar/data/1326801/000132680120000076/fb-06302020x10q.htm


resp = requests.get(url_to_use)
soup = bs.BeautifulSoup(resp.text, 'lxml')
```

```
#t1.py
import edgar
#install Python Edgar library
import pandas as pd

edgar.download_index('.', 2018,skip_all_present_except_last=False)

selectedcompany = 'Facebook'
selectedreport = '10-Q'
csv = pd.read_csv('2019-QTR4.tsv', sep='\t',  lineterminator='\n', names=None) 


# second part the code to extract info

csv.columns.values[0] = 'Item'

companyreport = csv[(csv['Item'].str.contains(selectedcompany)) & (csv['Item'].str.contains(selectedreport))]

Filing = companyreport['Item'].str.split('|')
Filing = Filing.to_list()

for item in Filing[0]:
    
    if 'html' in item:
        report = item
        
url = 'https://www.sec.gov/Archives/' + report
#https://www.sec.gov/ix?doc=/Archives/edgar/data/1652044/000165204419000032/goog10-qq32019.htm

df = pd.read_html(url)
document_index = df[0]
document_index = document_index.dropna()

document_name = document_index[document_index['Description'].str.contains(selectedreport)]
document_name = document_name['Document'].str.split(' ')
document_name = document_name[0][0]

report_formatted = report.replace('-','').replace('index.html','')
url = 'https://www.sec.gov/Archives/' + report_formatted + '/' + document_name


df = pd.read_html(url)

for item in df:
    BS = (item[0].str.contains('Retained') | item[0].str.contains('Total Assets'))
    if BS.any():
        Balance_Sheet = item
        

Balance_Sheet = Balance_Sheet.iloc[2:,[0,2,6]]

header = Balance_Sheet.iloc[0]
Balance_Sheet = Balance_Sheet[1:]

Balance_Sheet.columns = header


Balance_Sheet.columns.values[0] = 'Item'
Balance_Sheet = Balance_Sheet[Balance_Sheet['Item'].notna()]

Balance_Sheet[Balance_Sheet.columns[1:]] = Balance_Sheet[Balance_Sheet.columns[1:]].astype(str)
Balance_Sheet[Balance_Sheet.columns[1]] = Balance_Sheet[Balance_Sheet.columns[1]].map(lambda x: x.replace('(','-'))
Balance_Sheet[Balance_Sheet.columns[2]] = Balance_Sheet[Balance_Sheet.columns[2]].map(lambda x: x.replace('(','-'))

Balance_Sheet[Balance_Sheet.columns[1]] = Balance_Sheet[Balance_Sheet.columns[1]].map(lambda x: x.replace(',',''))
Balance_Sheet[Balance_Sheet.columns[2]] = Balance_Sheet[Balance_Sheet.columns[2]].map(lambda x: x.replace(',',''))

Balance_Sheet[Balance_Sheet.columns[1]] = Balance_Sheet[Balance_Sheet.columns[1]].map(lambda x: x.replace('—','0'))
Balance_Sheet[Balance_Sheet.columns[2]] = Balance_Sheet[Balance_Sheet.columns[2]].map(lambda x: x.replace('—','0'))



Balance_Sheet[Balance_Sheet.columns[1:]] = Balance_Sheet[Balance_Sheet.columns[1:]].astype(float)

Balance_Sheet
```

R&#x20;

```
install.packages("usethis")
install.packages("devtools")
library(devtools)
install_github("bergant/finstr")

```

```
install.packages("usethis")
install.packages("devtools")
library(devtools)
install_github("bergant/finstr")
library(XBRL)
xbrl_url2014 <- "https://www.sec.gov/Archives/edgar/data/320193/000119312514383437/aapl-20140927.xml"
xbrl_url2014 <- "https://www.sec.gov/Archives/edgar/data/320193/000119312514383437/aapl-20140927.xml"
old_o <- options(stringsAsFactors = FALSE)
xbrl_data_aapl2014 <- xbrlDoAll(xbrl_url2014)
xbrl_url2013 <-
  "https://www.sec.gov/Archives/edgar/data/320193/000119312513416534/aapl-20130928.xml"
xbrl_data_aapl2013 <- xbrlDoAll(xbrl_url2013)
options(old_o)
library(finstr)
st2013 <- xbrl_get_statements(xbrl_data_aapl2013)
st2014 <- xbrl_get_statements(xbrl_data_aapl2014)
st2014
balance_sheet2013 <- st2013$StatementOfFinancialPositionClassified
balance_sheet2014 <- st2014$StatementOfFinancialPositionClassified
income2013 <- st2013$StatementOfIncome
income2014 <- st2014$StatementOfIncome
balance_sheet2014
check <- check_statement(balance_sheet2014)
check
check <- check_statement(income2014, element_id = "OperatingIncomeLoss")
check
check$expression[1]
check$calculated / 10^6
balance_sheet <- merge( balance_sheet2013, balance_sheet2014 )
st_all <- merge( st2013, st2014 )
balance_sheet <- st_all$StatementOfFinancialPositionClassified
balance_sheet$endDate
write.csv(balance_sheet, "aaplbs.csv")
savehistory(file = ".Rhistory")
```





[htt](https://github.com/ryansmccoy/py-sec-edgar)[ps://github.com/ryansmccoy/py-sec-edgar](https://github.com/ryansmccoy/py-sec-edgar) get sec edgars 10q [https://codingandfun.com/scraping-sec-edgar-python/](https://codingandfun.com/scraping-sec-edgar-python/) annual report [https://codingandfun.com/python-sec-edgar-scraper/](https://codingandfun.com/python-sec-edgar-scraper/) 10q

{% embed url="https://codingandfun.com/python-sec-edgar-scraper/" %}

yahoo finance/google finance/3rd party

[https://github.com/farhadab/sec-edgar-financials](https://github.com/farhadab/sec-edgar-financials) full report?

[https://github.com/galibin24/SEC-EDGAR-python-scraper](https://github.com/galibin24/SEC-EDGAR-python-scraper) not sure

[https://github.com/femtotrader/python-eodhistoricaldata](https://github.com/femtotrader/python-eodhistoricaldata) eod api\_token=6049873ec60863.38641037"
