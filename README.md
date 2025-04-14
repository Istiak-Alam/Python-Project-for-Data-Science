# Python-Project-for-Data-Science
Analyzing Historical Stock/Revenue Data and Building a Dashboard

\[38\]: **import yfinance as yf**

**import pandas as pd**

**import requests**

**from bs4 import **BeautifulSoup **import plotly.graph\_objects as go** **from plotly.subplots import **make\_subplots

\[39\]: **def **make\_graph\(stock\_data, revenue\_data, stock\): fig = make\_subplots\(rows=2, cols=1, shared\_xaxes=**True**,␣

↪subplot\_titles=\("Historical Share Price", "Historical Revenue"\),␣

↪vertical\_spacing = .3\)

stock\_data\_specific = stock\_data\[stock\_data.Date <= '2021--06-14'\]

revenue\_data\_specific = revenue\_data\[revenue\_data.Date <= '2021-04-30'\]

fig.add\_trace\(go.Scatter\(x=pd.to\_datetime\(stock\_data\_specific.Date,␣

↪infer\_datetime\_format=**True**\), y=stock\_data\_specific.Close.astype\("float"\),␣

↪name="Share Price"\), row=1, col=1\) fig.add\_trace\(go.Scatter\(x=pd.to\_datetime\(revenue\_data\_specific.Date,␣

↪infer\_datetime\_format=**True**\), y=revenue\_data\_specific.Revenue. 

↪astype\("float"\), name="Revenue"\), row=2, col=1\) fig.update\_xaxes\(title\_text="Date", row=1, col=1\) fig.update\_xaxes\(title\_text="Date", row=2, col=1\) fig.update\_yaxes\(title\_text="Price \($US\)", row=1, col=1\) fig.update\_yaxes\(title\_text="Revenue \($US Millions\)", row=2, col=1\) fig.update\_layout\(showlegend=**False**, height=900, 

title=stock, 

xaxis\_rangeslider\_visible=**True**\) fig.show\(\)

**0.1**

**Question 1: Use yfinance to Extract Stock Data** **Using the Ticker function enter the ticker symbol of the stock we want to extract data** **on to create a ticker object. The stock is Tesla and its ticker symbol is TSLA. **

\[53\]: Tesla = yf.Ticker\('TSLA'\)

**Using the ticker object and the function history extract stock information and save** **it in a dataframe named tesla\_data. Set the period parameter to max so we get** **information for the maximum amount of time. **

1

\[54\]: tesla\_data = Tesla.history\(period = "max"\)

\[55\]: tesla\_data.reset\_index\(inplace = **True**\) tesla\_data.head\(\)

\[55\]:

Date

Open

High

Low

Close

\\

0 2010-06-29 00:00:00-04:00

1.266667

1.666667

1.169333

1.592667

1 2010-06-30 00:00:00-04:00

1.719333

2.028000

1.553333

1.588667

2 2010-07-01 00:00:00-04:00

1.666667

1.728000

1.351333

1.464000

3 2010-07-02 00:00:00-04:00

1.533333

1.540000

1.247333

1.280000

4 2010-07-06 00:00:00-04:00

1.333333

1.333333

1.055333

1.074000

Volume

Dividends

Stock Splits

0

281494500

0.0

0.0

1

257806500

0.0

0.0

2

123282000

0.0

0.0

3

77097000

0.0

0.0

4

103003500

0.0

0.0

**0.2**

**Question 2: Use Webscraping to Extract Tesla Revenue Data**

\[64\]: url = "https://www.macrotrends.net/stocks/charts/TSLA/tesla/revenue" 

headers = \{"User-Agent": "Mozilla/5.0 \(Windows NT 10.0; Win64; x64\)"\}

response = requests.get\(url, headers=headers\)

\[66\]: soup = BeautifulSoup\(response.text, "html.parser"\) print\(soup.find\_all\('title'\)\)

\[<title>Tesla Revenue 2010-2024 | TSLA | MacroTrends</title>\]

\[78\]: tesla\_revenue = \[\]

**for **table **in **soup.find\_all\('table'\): header = table.find\('th'\)

**if **header **and **'Tesla Quarterly Revenue' **in **header.text: rows = table.find\_all\('tr'\)

**for **row **in **rows:

cols = row.find\_all\('td'\)

**if **len\(cols\) >= 2:

date = cols\[0\].text.strip\(\)

revenue = cols\[1\].text.strip\(\).replace\('$', ''\).replace\(',', ''\) **if **revenue \!= '':

tesla\_revenue.append\(\{"Date": date, "Revenue": revenue\}\) tesla\_revenue\_df = pd.DataFrame\(tesla\_revenue\)

\[82\]: tesla\_revenue\_df = pd.DataFrame\(tesla\_revenue\) print\(tesla\_revenue\_df.tail\(\)\)

2

Date Revenue

57

2010-09-30

31

58

2010-06-30

28

59

2010-03-31

21

60

2009-09-30

46

61

2009-06-30

27

**0.3**

**Question 3: Use yfinance to Extract Stock Data**

\[83\]: GameStop = yf.Ticker\("GME"\)

\[84\]: gme\_data = GameStop.history\(period = 'max'\)

\[85\]: gme\_data.reset\_index\(inplace = **True**\) gme\_data.head\(\)

\[85\]:

Date

Open

High

Low

Close

Volume

\\

0 2002-02-13 00:00:00-05:00

1.620128

1.693350

1.603296

1.691667

76216000

1 2002-02-14 00:00:00-05:00

1.712707

1.716074

1.670626

1.683250

11021600

2 2002-02-15 00:00:00-05:00

1.683251

1.687459

1.658002

1.674835

8389600

3 2002-02-19 00:00:00-05:00

1.666418

1.666418

1.578047

1.607504

7410400

4 2002-02-20 00:00:00-05:00

1.615920

1.662210

1.603296

1.662210

6892800

Dividends

Stock Splits

0

0.0

0.0

1

0.0

0.0

2

0.0

0.0

3

0.0

0.0

4

0.0

0.0

**0.4**

**Question 4: Use Webscraping to Extract GME Revenue Data**

\[125\]: url = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/

↪IBMDeveloperSkillsNetwork-PY0220EN-SkillsNetwork/labs/project/stock.html" 

html\_data = requests.get\(url\).text

\[126\]: soup = BeautifulSoup\(html\_data,"html5lib"\)

\[127\]: tables = pd.read\_html\(url, match="GameStop Quarterly Revenue", flavor='bs4'\) gme\_revenue = tables\[0\]

\[128\]: gme\_revenue = gme\_revenue.rename\(columns=\{'GameStop Quarterly Revenue \(Millions␣

↪of US $\)': 'Date','GameStop Quarterly Revenue \(Millions of US $\).1':␣

↪'Revenue'\}\)

\[129\]: gme\_revenue\["Revenue"\] = gme\_revenue\["Revenue"\].str.replace\(",", ""\).str. 

↪replace\("$", ""\) 3

gme\_revenue.dropna\(inplace=**True**\)

\[130\]: print\(gme\_revenue.tail\(\)\)

Date Revenue

57

2006-01-31

1667

58

2005-10-31

534

59

2005-07-31

416

60

2005-04-30

475

61

2005-01-31

709

**0.5**

**Question 5 - Tesla Stock and Revenue Dashboard**

\[134\]: print\(type\(tesla\_revenue\)\)

<class 'list'> 

\[138\]: tesla\_revenue = pd.DataFrame\(tesla\_revenue, columns=\['Date', 'Revenue'\]\)

\[139\]: *\# Clean up revenue column* tesla\_revenue\["Revenue"\] = tesla\_revenue\["Revenue"\].str.replace\(",", ""\).str. 

↪replace\("$", ""\) tesla\_revenue.dropna\(inplace=**True**\)

\[140\]: make\_graph\(tesla\_data, tesla\_revenue, 'Tesla'\) C:\\Users\\Istiak\\AppData\\Local\\Temp\\ipykernel\_12156\\2068038883.py:5: UserWarning: The argument 'infer\_datetime\_format' is deprecated and will be removed in a future version. A strict version of it is now the default, see https://pandas.pydata.org/pdeps/0004-consistent-to-datetime-parsing.html. You can safely remove this argument. 

C:\\Users\\Istiak\\AppData\\Local\\Temp\\ipykernel\_12156\\2068038883.py:6: UserWarning: The argument 'infer\_datetime\_format' is deprecated and will be removed in a future version. A strict version of it is now the default, see https://pandas.pydata.org/pdeps/0004-consistent-to-datetime-parsing.html. You can safely remove this argument. 

4





**0.6**

**Question 6 - GameStop Stock and Revenue Dashboard**

\[144\]: make\_graph\(gme\_data, gme\_revenue, 'GameStop'\)

\[ \]:

5


# Document Outline

+ Question 1: Use yfinance to Extract Stock Data 
+ Question 2: Use Webscraping to Extract Tesla Revenue Data 
+ Question 3: Use yfinance to Extract Stock Data 
+ Question 4: Use Webscraping to Extract GME Revenue Data 
+ Question 5 - Tesla Stock and Revenue Dashboard 
+ Question 6 - GameStop Stock and Revenue Dashboard



