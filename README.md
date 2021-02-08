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
Sklearn's Linear Regression model is used to predict the values. For the function to work effectively the data had to be reshaped.
```Python
# dependant variable - what we are trying to predict
y = df.Standing
# independant - what we want to predict from
x = df.GD.values.reshape(-1,1)
```
 
### First image
R-Squared = 0.840
Regression equation = y = 10.06 + -0.2323 * x


![](regression_with_names.png)

### Second image
R-Squared = 0.852
Regression equation = y = 10.04 + -0.2348 * x


![](regression_02.png)
