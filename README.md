# Serie A standings prediction using linear regression

## A model is created to predict the final league rank of a based on goal difference.  
The data was acquired from [WorldFootball](https://www.worldfootball.net/schedule/ita-serie-a-2019-2020-spieltag/38/) using the pandas Python library. The dataframe was cleaned from the null values and the needed information (Team name, standings, GD) was subtracted. 
```Python
import pandas as pd
df = pd.read_html("https://www.worldfootball.net/schedule/ita-serie-a-1990-1991-spieltag/34/")[3]
df["Year"] = 1990
for year in range(1991, 2004):
    my_url = "https://www.worldfootball.net/schedule/ita-serie-a-{}-{}-spieltag/34/".format(year, year+1)
    df_temp = pd.read_html(my_url)[3]
    df_temp["Year"] = year
    df = pd.concat([df, df_temp])
for year in range(1994, 2020):
    my_url = "https://www.worldfootball.net/schedule/ita-serie-a-{}-{}-spieltag/38/".format(year, year+1)
    df_temp = pd.read_html(my_url)[3]
    df_temp["Year"] = year
    df = pd.concat([df, df_temp])
```
```Python
df = df[df['#'].notna()]
df = df[['#', 'Dif.', 'Team.1', 'Year']]
df.columns = ['Standing', 'GD', 'Team', 'Year']
```

Sklearn's Linear Regression model is used to predict the values. For the function to work effectively the data had to be reshaped.
```Python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.linear_model import LinearRegression

# dependant variable - what we are trying to predict
y = df.Standing
# independant - what we want to predict from
x = df.GD.values.reshape(-1,1)
```
The model needs to be fit usings Sklearn's function, from which we can evaluate our model and create the line of best fit. R-Squared = 0.8403
```Python
model = LinearRegression().fit(x,y)
r_sq = model.score(x,y)
intercept = model.intercept_
slope = model.coef_
```
The equation of a straight line is : **y = mx + b**  
Where **b** is the intercept and **m** is the slope.
Each GD change influences your standing by around 0.23 position.
```Python
y_pred = intercept + slope*x
```

To create our first plot the matplotlib and seaborn libraries were used.
```Python
fig, ax = plt.subplots(figsize= (12,12), facecolor='#fffbdc')
ax.set_facecolor('#fffbdc')
plt.scatter(x,y, alpha=0.8, c="#0066cc")
plt.plot(x, y_pred, c="red")


ax.spines['left'].set_linewidth(1.5)
ax.spines['bottom'].set_linewidth(1.5)

plt.yticks(np.arange(0,21,1))
plt.xticks(np.arange(-70,71,10))

plt.ylim(0.5, 20.5)
plt.xlim(-65, 70)

plt.grid(axis = 'x', linewidth = 0.2)
plt.grid(axis = 'y', linewidth = 0.2)

plt.gca().invert_yaxis()

plt.xlabel("Goal difference", fontsize=20)
plt.ylabel("Standings ", fontsize=20)
sns.despine()
plt.title("Relationship between goal diff. and standings \n Serie A 1990-2020\n",fontdict= { 'fontsize': 24, 'fontweight':'bold'});
```
 
### 1990 - 2020
R-Squared = 0.840
Regression equation = y = 10.06 + -0.2323 * x

![](regression_with_names.png)

The line of best fit cleary suggests that a bigged GD usually means a better standings on the table. The most outstanding values and performances are highlighted from the last 30 years.

Intresingly, there are some protruding values connected to dominant teams in the year 2005/06. The simple explanation is the [Calciopoli](https://en.wikipedia.org/wiki/Calciopoli). Calciopoli was a scandal of football match fixing in Italy's top professional leagues, Serie A, and Serie B. The scandal was uncovered in May 2006, when a number of telephone interceptions showed relations between team managers and referee organizations during the 2004–05 and 2005–06 seasons, being accused of selecting favourable referees. This implicated league champions Juventus and several other teams including Milan, Fiorentina, Lazio, and Reggina. In July 2006, Juventus were stripped of the 2004–05 title, and was downgraded to last place in the 2005–06 championship (title given to Internazionale) and relegated to Serie B. Several other teams than Juventus, such as AC Milan, Fiorentina  or Lazio had some or their points deducted.  



### 1990 - 2020 (excl. 2005/06)
R-Squared = 0.852
Regression equation = y = 10.04 + -0.2348 * x


![](regression_02.png)
