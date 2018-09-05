# Challenge
## The major steps of a data project by exploring Git:
- Understand project/research objectives
- Get data
- Explore and clean data
- Data analysis
- Data visulization
- Propose implications
- Feedbacks to project/research

# Exercises
## Summary
- What I present is one student data project called "Hong Kong Visitor Analysis: : from the perspectives of country/region of residence, mode of transport and consumption expenditure".
- I found the final project information from the .ipynb file and treated it as the final report.
- Based on this file, I specified three different kinds of content: presentation words and figures, code, and data. 

### Presentation words and figures

> I. Patterns of Hong Kong visitors according to country/region of residence:
> 1) Hong Kong visitors are dominated by people from the mainland China;
> 2) Except the mainland China, visitor from other countries and regions are highly seasonal.
> 3) Visitor arrivals from the maninland China are not seasonal but are influenced by social events and exchange rates:
> For instance, during the "Umbrella Revolution" in September 2014, there is a sharp drop in the number of visitors coming from the mainland;
> In September 2017, visitor arrivals further dropped when typhoon no. 10 was issued in Hong Kong due to the typhoon Hato arrived, when delayed flights and heavy flooding in Hong Kong lasted for more than a month. The number of visitors from the mainland was influenced by the flu season in August in the same year.

![Figure-part1](https://drive.google.com/drive/u/0/folders/1bZNTxKjD8Rt6BGNnBYzKJVNieXjAl-av)

> II. Characteristics of Hong Kong visitors by the mode of transport:
> 1) Most visitors arrive Hong Kong by air, except that most visitors from the mainland China arrive Hong Kong by land; 
> 2) For long haul markets, most visitors arrive Hong Kong by air and traveling by sea was their last option. However, in short haul markets, the number of visitors arrived by sea exceeded the number of visitors by land after 2011 and has continued to rise. Still, coming to Hong Kong by air has always been the first choice.

![Figure-part2](https://drive.google.com/drive/u/0/folders/1M07KgoB_hx8rl4s2aQUm7ArW1QPgg3MY)

> III. The consumption expenditure patterns of Hong Kong visitors:
> 1) The Mainland China made up of the largest part in same-day consumption. And it has a big gap with other countries;
> 2) Apart from the Mainland China, Macao, Taiwan, South and Southeast Asia are the top three regions with the highest consumption;
> 3) In the per capita overnight consumption, Hong Kong visitors from the Mainland still account for the highest but not as dominate as those for same-day consumption;
> 4) Macao is the second highest in per capita same-day consumption while it is the smallest in per capita overnight consumption;
> 5) Apart from the Mainland China, the total consumption of overnight visitors from South and Southeast Asia have been higher than visitors from Europe and America.

![Figure-part3](https://drive.google.com/drive/u/0/folders/1kaInZHshOZHyOJ7Jt6V0yEUrad6OzrfV)


### Code

```
import requests
import bs4
import csv
import pandas as pd

from datetime import datetime
from dateutil import parser
import numpy
import seaborn as sns
from matplotlib import pyplot as plt
from matplotlib.ticker import MaxNLocator
from collections import namedtuple

df = pd.read_csv('visitors.csv')
df

def parse_datetime(x):
    try:
        return parser.parse(x)
    except:
        return numpy.nan
df['datetime'] = df['Date'].apply(parse_datetime)

df_all = df.set_index('datetime').resample('1m').aggregate('sum')
df_all.plot(linewidth=3, figsize=(10,5))
plt.title('Visitor Arrivals By Country/Region of Residence')
plt.xlabel('Year')
plt.ylabel('Visitors')

selected_columns = list(set(df_all.columns))
df_all[['ThemainlandofChina']].plot(linewidth=2, figsize=(10,5),color='G').plot()
plt.title('Visitor Arrivals By Country/Region of residence')
plt.xlabel('Year')
plt.ylabel('Visitors')
```
```
import seaborn as sns
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
% matplotlib inline
year = ['2011', '2012', '2013', '2014', '2015', '2016', '2017', '2018']
month = ['01 January', '02 February', '03 March', '04 April', '05 May', '06 June', '07 July', '08 August', '09 September',
        '10 October', '11 November', '12 December'] 
values = df['ThemainlandofChina']
np.random.seed(100)
arr_year = np.random.choice(year, size=(10000,))
list_year = list(arr_year)

arr_month= np.random.choice(month, size=(10000,))
list_month = list(arr_month)

values = np.random.randint(50, 1000, 10000)
list_values = list(values)

df2 = pd.DataFrame({'year':list_year,
                  'month': list_month,
                  'values':list_values})
df2.head()

pt = df2.pivot_table(index='month', columns='year', values='values', aggfunc=np.sum)
pt.head()

f, ax = plt.subplots(figsize = (10, 4))
cmap = sns.cubehelix_palette(start = 1, rot = 3, gamma=0.8, as_cmap = True)
sns.heatmap(pt, cmap = 'Blues',  linewidths = 0.05, ax = ax)
ax.set_title('Visitor Arrivals from the Mainland China')
ax.set_xlabel('Year')
ax.set_ylabel('Month')

myax = plt.subplot(7, 1, 1)
selected_columns = list(set(df_all.columns))
df_all[['South&SoutheastAsia']].plot(linewidth=2,color='Orange', ax=myax)
plt.title('Visitor Arrivals By Country/Region of Residents')
plt.xlabel('Year')
plt.ylabel('Number of Visitors')

myax = plt.subplot(7, 1, 3)
selected_columns = list(set(df_all.columns))
df_all[['Europe']].plot(linewidth=2, figsize=(20,5),color='Brown', ax=myax)
plt.title('Visitor Arrivals By Country/Region of Residence')
plt.xlabel('Year')
plt.ylabel('Visitors')

myax = plt.subplot(7, 1, 5)
selected_columns = list(set(df_all.columns))
df_all[['Macao']].plot(linewidth=2, figsize=(20,5),color='blue', ax=myax)
plt.title('Visitor Arrivals By Country/Region of Residence')
plt.xlabel('Year')
plt.ylabel('Visitors')

myax = plt.subplot(7, 1, 7)
selected_columns = list(set(df_all.columns))
df_all[['Australia,NewZealand&SouthPacific']].plot(linewidth=2, figsize=(20,5),color='Pink', ax=myax)
plt.title('Visitor Arrivals By Country/Region of Residence')
plt.xlabel('Year')
plt.ylabel('Visitors')
```
```
import matplotlib.pyplot as plt
import numpy as np
df3 = pd.read_csv('transport_air.csv')
df3

# data to plot
n_groups = 7
data_2004 = df3['2004']
data_2005 = df3['2005']
data_2006 = df3['2006']
data_2009 = df3['2009']
data_2010 = df3['2010']
data_2011 = df3['2011']
data_2012 = df3['2012']
data_2013 = df3['2013']
data_2014 = df3['2014']
data_2015 = df3['2015']
data_2016 = df3['2016']

# create plot
fig, ax = plt.subplots(figsize=(15, 5))
index = np.arange(n_groups)
bar_width = 0.06
opacity = 0.8
   
rects1 = plt.bar(index, data_2004, bar_width,
                 alpha=opacity,
                 color='lightseagreen',
                 label='2004')
 
rects2 = plt.bar(index + bar_width, data_2005, bar_width,
                 alpha=opacity,
                 color='darkcyan',
                 label='2005')

rects3 = plt.bar(index + bar_width + bar_width, data_2006, bar_width,
                 alpha=opacity,
                 color='deepskyblue',
                 label='2006')

rects3 = plt.bar(index + bar_width + bar_width + bar_width, data_2009, bar_width,
                 alpha=opacity,
                 color='slategray',
                 label='2009')

rects4 = plt.bar(index + bar_width + bar_width + bar_width + bar_width, data_2010, bar_width,
                 alpha=opacity,
                 color='royalblue',
                 label='2010')

rects5 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width, data_2011, bar_width,
                 alpha=opacity,
                 color='navy',
                 label='2011')

rects6 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2012, bar_width,
                 alpha=opacity,
                 color='mediumpurple',
                 label='2012')

rects7 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2013, bar_width,
                 alpha=opacity,
                 color='darkorchid',
                 label='2013')

rects8 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2014, bar_width,
                 alpha=opacity,
                 color='plum',
                 label='2014')

rects9 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2015, bar_width,
                 alpha=opacity,
                 color='m',
                 label='2015')

rects10 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2016, bar_width,
                 alpha=opacity,
                 color='mediumvioletred',
                 label='2016')

plt.xlabel('Regions/Countries')
plt.ylabel('No. of visitors (in thousands)')
plt.title('No. of Hong Kong Inbound Visitors By Air between 2004 and 2016')
plt.xticks(np.arange(7),('America', 'Europe, Africa and the Middle East', 'Australia, New Zealand and South Pacific', 'North Asia', 'South and Southeast Asia', 'Mainland China', 'Taiwan'), rotation=20)
plt.legend()
plt.minorticks_on()
plt.autoscale(enable=True, axis='both', tight=False)
plt.show()
```
```
df4 = pd.read_csv('transport_land.csv')
df4
# data to plot
n_groups = 8
data_2004 = df4['2004']
data_2005 = df4['2005']
data_2006 = df4['2006']
data_2009 = df4['2009']
data_2010 = df4['2010']
data_2011 = df4['2011']
data_2012 = df4['2012']
data_2013 = df4['2013']
data_2014 = df4['2014']
data_2015 = df4['2015']
data_2016 = df4['2016']

# create plot
fig, ax = plt.subplots(figsize=(15, 5))
index = np.arange(n_groups)
bar_width = 0.06
opacity = 0.8
   
rects1 = plt.bar(index, data_2004, bar_width,
                 alpha=opacity,
                 color='lightseagreen',
                 label='2004')
 
rects2 = plt.bar(index + bar_width, data_2005, bar_width,
                 alpha=opacity,
                 color='darkcyan',
                 label='2005')

rects3 = plt.bar(index + bar_width + bar_width, data_2006, bar_width,
                 alpha=opacity,
                 color='deepskyblue',
                 label='2006')

rects3 = plt.bar(index + bar_width + bar_width + bar_width, data_2009, bar_width,
                 alpha=opacity,
                 color='slategray',
                 label='2009')

rects4 = plt.bar(index + bar_width + bar_width + bar_width + bar_width, data_2010, bar_width,
                 alpha=opacity,
                 color='royalblue',
                 label='2010')

rects5 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width, data_2011, bar_width,
                 alpha=opacity,
                 color='navy',
                 label='2011')

rects6 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2012, bar_width,
                 alpha=opacity,
                 color='mediumpurple',
                 label='2012')

rects7 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2013, bar_width,
                 alpha=opacity,
                 color='darkorchid',
                 label='2013')

rects8 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2014, bar_width,
                 alpha=opacity,
                 color='plum',
                 label='2014')

rects9 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2015, bar_width,
                 alpha=opacity,
                 color='m',
                 label='2015')

rects10 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2016, bar_width,
                 alpha=opacity,
                 color='mediumvioletred',
                 label='2016')

plt.xlabel('Region/Countries')
plt.ylabel('No. of visitors (in Thousands)')
plt.title('No. of Hong Kong Inbound Visitors By Land between 2004 and 2016')
plt.xticks(np.arange(8), ('America', 'Europe, Africa and the Middle East', 'Australia, New Zealand and South Pacific', 'North Asia', 'South and Southeast Asia', 'Mainland China', 'Taiwan', 'Macao'), rotation=20)
plt.legend()
plt.minorticks_on()
plt.autoscale(enable=True, axis='both', tight=None)
plt.show()
```
```
df5 = pd.read_csv('transport_sea.csv')
df5

# data to plot
n_groups = 8
data_2004 = df5['2004']
data_2005 = df5['2005']
data_2006 = df5['2006']
data_2009 = df5['2009']
data_2010 = df5['2010']
data_2011 = df5['2011']
data_2012 = df5['2012']
data_2013 = df5['2013']
data_2014 = df5['2014']
data_2015 = df5['2015']
data_2016 = df5['2016']


# create plot
fig, ax = plt.subplots(figsize=(15, 5))
index = np.arange(n_groups)
bar_width = 0.06
opacity = 0.8
   
rects1 = plt.bar(index, data_2004, bar_width,
                 alpha=opacity,
                 color='lightseagreen',
                 label='2004')
 
rects2 = plt.bar(index + bar_width, data_2005, bar_width,
                 alpha=opacity,
                 color='darkcyan',
                 label='2005')

rects3 = plt.bar(index + bar_width + bar_width, data_2006, bar_width,
                 alpha=opacity,
                 color='deepskyblue',
                 label='2006')

rects3 = plt.bar(index + bar_width + bar_width + bar_width, data_2009, bar_width,
                 alpha=opacity,
                 color='slategray',
                 label='2009')

rects4 = plt.bar(index + bar_width + bar_width + bar_width + bar_width, data_2010, bar_width,
                 alpha=opacity,
                 color='royalblue',
                 label='2010')

rects5 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width, data_2011, bar_width,
                 alpha=opacity,
                 color='navy',
                 label='2011')

rects6 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2012, bar_width,
                 alpha=opacity,
                 color='mediumpurple',
                 label='2012')

rects7 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2013, bar_width,
                 alpha=opacity,
                 color='darkorchid',
                 label='2013')

rects8 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2014, bar_width,
                 alpha=opacity,
                 color='plum',
                 label='2014')

rects9 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2015, bar_width,
                 alpha=opacity,
                 color='m',
                 label='2015')

rects10 = plt.bar(index + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width + bar_width, data_2016, bar_width,
                 alpha=opacity,
                 color='mediumvioletred',
                 label='2016')

plt.xlabel('Region')
plt.ylabel('No. of visitors (in thousands)')
plt.title('No. of Hong Kong Inbound Visitors By Sea between 2004 and 2016')
plt.xticks(np.arange(8), ('America', 'Europe, Africa & the Middle East', 'Australia, New Zealand & South Pacific', 'North Asia', 'South & Southeast Asia', 'Mainland China', 'Taiwan', 'Macau'), rotation=20)
plt.legend()
plt.minorticks_on()
plt.autoscale(enable=True, axis='both', tight=False)
plt.show()
```
```
df_long = pd.read_csv('long_haul_market.csv')
df_long

df_long = df_long.set_index('Year')
df_long.plot(kind='area', stacked=False, figsize=(10,5))
plt.ylabel('No. of visitors in thousands')
plt.xlabel('Year')
plt.xticks(np.arange(2004,2018, step=1))
plt.title('Mode of Transport of Hong Kong Inbound Visitors (Long Haul Markets)')
```
```
df_short = pd.read_csv('short_haul_market.csv')
df_short

df_short = df_short.set_index('Year')
df_short.plot(kind='area', stacked=False, figsize=(10,5))
plt.ylabel('No. of visitors in thousands')
plt.xlabel('Year')
plt.xticks(np.arange(2004,2018, step=1))
plt.title('Mode of Transport of Hong Kong Inbound Visitors (Short Haul Markets excluding Mainland China)')
```
```
df_mainland = pd.read_csv('mainland.csv')
df_mainland = df_mainland.set_index('Year')
df_mainland.plot(kind='area', stacked=False, figsize=(10,5))
plt.ylabel('No. of visitors in thousands')
plt.xlabel('Year')
plt.xticks(np.arange(2004,2018, step=1))
plt.title('Mode of Transport of Hong Kong Inbound Visitors (Mainland China)')
```
```
import numpy
df=pd.read_csv('consumption 1.csv')
df

def parse_datetime(x):
    try:
        return parser.parse(x)
    except:
        return numpy.nan
df['datetime'] = df['Year'].apply(parse_datetime)

df_all=df.set_index('datetime').resample('1y').aggregate('sum')
df_all.plot(linewidth=1, figsize=(20,10)).plot()
plt.title('Destination consumption expenditure of same-day in-town visitors ($)')
plt.legend(loc=2,prop={'size':13})

selected_columns = list(set(df_all.columns) - set(['Total', 'The mainland of China']))
df_all[selected_columns].plot(linewidth=2, figsize=(20,10)).plot()
plt.legend(loc=1,prop={'size':13})
plt.title('Destination consumption expenditure of same-day in-town visitors except for Total and mainland China ($)')

```
```
df=pd.read_csv('consumption 2.csv')

def parse_datetime(x):
    try:
        return parser.parse(x)
    except:
        return numpy.nan
df['datetime'] = df['Year'].apply(parse_datetime)

df_all=df.set_index('datetime').resample('1y').aggregate('sum')
df_all.plot(linewidth=2, figsize=(20,10)).plot()
plt.title('Destination consumption expenditure of overnight visitors ($)')
plt.legend(loc=2,prop={'size':10})

selected_columns = list(set(df_all.columns) - set(['Total', 'The mainland of China']))
df_all[selected_columns].plot(linewidth=2, figsize=(20,10)).plot()
plt.title('Destination consumption expenditure of overnight visitors except for Total and mainland China($)')
plt.legend(loc=2,prop={'size':12})
```
```
df=pd.read_csv('per capita 1.csv')

def parse_datetime(x):
    try:
        return parser.parse(x)
    except:
        return numpy.nan
df['datetime'] = df['Year'].apply(parse_datetime)

df_all=df.set_index('datetime').resample('1y').aggregate('sum')
df_all.plot(linewidth=2, figsize=(20,10)).plot()
plt.title('Per capita spending of same-day in-town visitors ($)')
plt.legend(loc=4,prop={'size':11})

selected_columns = list(set(df_all.columns) - set(['Overall', 'The mainland of China']))
df_all[selected_columns].plot(linewidth=2, figsize=(20,10)).plot()
plt.title('Per capita spending of same-day in-town visitors except for Total and mainland China($)')
plt.legend(prop={'size':12})
```
```
df=pd.read_csv('per captia 2.csv')

def parse_datetime(x):
    try:
        return parser.parse(x)
    except:
        return numpy.nan
df['datetime'] = df['Year'].apply(parse_datetime)

df_all=df.set_index('datetime').resample('1y').aggregate('sum')
df_all.plot(linewidth=2, figsize=(20,10)).plot()
plt.title('Per capita spending of overnight visitors ($)')
plt.legend(loc=1,prop={'size':11})

selected_columns = list(set(df_all.columns) - set(['Overall', 'The mainland of China']))
df_all[selected_columns].plot(linewidth=2, figsize=(20,10)).plot()
plt.title('Per capita spending of overnight visitors except for Total and mainland China($)')
plt.legend(loc=1,prop={'size':11})
```

### Data

[visitors.csv](https://github.com/data-projects-archive/201804-HK-Tourism/blob/master/Final%20Group%20Project/visitors.csv)
[consumption1.csv](https://github.com/data-projects-archive/201804-HK-Tourism/blob/master/Final%20Group%20Project/consumption%201.csv)
[consumption2.csv](https://github.com/data-projects-archive/201804-HK-Tourism/blob/master/Final%20Group%20Project/consumption%202.csv)
[mainland.csv](https://github.com/data-projects-archive/201804-HK-Tourism/blob/master/Final%20Group%20Project/mainland.csv)
[percapita1.csv](https://github.com/data-projects-archive/201804-HK-Tourism/blob/master/Final%20Group%20Project/per%20capita%201.csv)
[percapita2.csv](https://github.com/data-projects-archive/201804-HK-Tourism/blob/master/Final%20Group%20Project/per%20captia%202.csv)
[transport_land.csv](https://github.com/data-projects-archive/201804-HK-Tourism/blob/master/Final%20Group%20Project/transport_land.csv)
[transport_sea.csv](https://github.com/data-projects-archive/201804-HK-Tourism/blob/master/Final%20Group%20Project/transport_sea.csv)
[Long_haul_market.csv](https://github.com/data-projects-archive/201804-HK-Tourism/blob/master/Final%20Group%20Project/Long_haul_market.csv)
[Short_haul_market.csv](https://github.com/data-projects-archive/201804-HK-Tourism/blob/master/Final%20Group%20Project/Short_haul_market.csv)

### Commit history

- The total number of commit: 49 times.
- Commit time point:
1. Commits on Mar 2, 2018
2. Commits on Mar 4, 2018
3. Commits on Mar 9, 2018
4. Commits on Mar 15, 2018
5. Commits on Apr 5, 2018
6. Commits on Apr 9, 2018
7. Commits on Apr 14, 2018
8. Commits on Apr 29, 2018
9. Commits on Apr 30, 2018
10. Commits on May 1, 2018
11. Commits on May 2, 2018