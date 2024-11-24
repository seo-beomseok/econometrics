# Tspoon

<a href=https://pypi.org/project/tspoon/>Tspoon</a> is a Python library for time-series pre-processing, period conversion, normalization, visualization and more.

## Installation

Use the package manager [pip](https://pip.pypa.io/en/stable/) to install tspoon.

```bash
pip install tspoon
```

## Usage

### Date functions
```python
import tspoon as tsp

# returns first and last days of a month 'M' in a year 'Y'
tsp.firstdate(2024,2)
tsp.lastdate(2024,2)

# returns the days between 'startdate' and 'enddate'
tsp.datesbetween('2024-02-01','2024-06-30')

# transform 'YYYYMMDD' to 'YYYY-MM-DD'
tsp.daytrans('20240630')

# returns a date that is 'x' days before and after a given 'basedate'
tsp.dayahead('2024-06-30',25)
tsp.dayafter('2024-06-30',25)

# returns the ISO calendar week number(YYYYWW), month(YYYYMM), and year(YYYY) of a day
tsp.day2week('2024-06-30')
tsp.day2week('20240630')
tsp.day2month('20240630')
tsp.day2year('20240630')

# returns the quarter(YYYYqX) and month(YYYYMM)
tsp.month2quarter('202406')
tsp.quarter2month('2024q2')

# returns the first day(YYYY-MM-DD) of a week(YYYYWW) and month(YYYYMM)
tsp.week2day('202420')
tsp.month2day('202406')

# generates a data frame with converted periods
df_w = tsp.GenDf_d2w(dat, aggregate='mean')
df_m = tsp.GenDf_d2m(dat, aggregate='mean')
df_wd = tsp.GenDf_w2d(df_d=None, df_w=df_w)
df_md = tsp.GenDf_m2d(df_d=None, df_m=df_m)
df_wm = tsp.GenDf_w2m(df_w, aggregate='last')
df_mw = tsp.GenDf_m2w(df_w, df_m, interpolate='linear')
df_mq = tsp.GenDf_m2q(df_m, aggregate='last')
df_qm = tsp.GenDf_q2m(df_mq, interpolate='linear')
df_my = tsp.GenDf_m2y(df_m, aggregate='last')
df_ym = tsp.GenDf_y2m(df_my, interpolate='linear')

# returns backward and central moving average of a time series
tsp.bMA(dat, lag_day=14, todf=True)
tsp.cMA(dat, lag_day=14, lead_day=14, todf=True)

# returns yoy, mom, and un-yoy transformation
tsp.yoy(dat, period=12, smooth=(1,1))
tsp.mom(dat) 		                                    # equivalent to tsp.mom(dat, period=1), tsp.yoy(dat, period=1)
tsp.unyoy(dat, dat_level, period=52, smooth=None) 		# dat_level contains the level information of the beginning day
```

### Dummy generation
```python
# generates dummy data frame 
tsp.GenMonthDummy(['202003','202004','202005'], MW='month')
tsp.GenMonthDummy(['2020'+str(x) for x in range(10,26)], MW='week')
tsp.GenYearDummy(['2020'])

# generates structural break dummy data frame
tsp.GenStructBreakDummy(['202001','202002','202003','202004','202005','202006'], time='202003', MW='month')
tsp.GenStructBreakDummy(['202001','202002','202003','202004','202005','202006'], time='202003', time2='202005', MW='month')
```

### Duplicates check
```python
# returns unique and duplicate observations
tsp.uniq(['202001','202001','202002','202003'])
tsp.dupes(['202001','202001','202002','202003'])
```

### Transformation functions
```python
# returns unique and duplicate observations
tsp.x13as(df, period='M', X12PATH='./x13as/')       # requires x12 ariam from us consensus
tsp.hp(df, period='M')                              # period can be 'D', 'W', 'M', 'Q', 'Y'
tsp.hptrend(df, lamb=1)
tsp.hpcycle(df, lamb=1)
tsp.norm(df)                                        # (x-mean(x))/std(x) normalization
tsp.scale(df)                                       # (x-min(x))/(max(x)-min(x)) rescale
tsp.unnorm(df, df.mean(), df.std())

# returns arima extrapolated time series
tsp.extrapolate(df, maxlags=1)

# returns proportaion, contribution, 
tsp.proportion(df)
tsp.contribution(df, period=12)
tsp.contribution_proportion(df, period=12)
```

### Visualization functions
```python
# Make matplotlib use Hangul
tsp.Hangul(fonthpath='C:/Windows/Fonts/NanumBarunGothic.ttf')

# Make dynamic graph
tsp.Plotly(df, theme='white')

fig = df.plot(width=800, height=600)
plotly_theme(fig, fs=19, left_unit={'text':'(YoY, %)', 'fs':17})
```

## Contact

- Beomseok Seo : https://seo-beomseok.github.io
