## Python

**Read Data from Folder**
```
from google.colab import drive
drive.mount('/content/gdrive')
import pandas as pd
data = pd.read_csv('/content/gdrive/My Drive/calories (1).csv')
```

**Check Column Types**
data.info()

**Check Column Types**
```
data.info()
```

**Show First 15 Rows of the Data**
```
data.head(15)
```

**Convert 'Date New' Column to Date or Datetime Type **
```
data['Date New'] = data['Date New'].apply(pd.to_datetime)
```

** Combine Calories and Time Day Table with Common Link**
```
combinedata = data.merge(timedata, left_on='TimeId', right_on='timeId')
```

** Drop Duplicate 'timeId' Column**
```
combinedata.drop('timeId', axis=1, inplace=True)
```

** Show Only the Data for Breakfast**
```
dataBreakfast = combinedata[combinedata['time'] == 'Breakfast']
```

**Show Only Data Where It Is Lunch and Calories Is More Than 250 **
```
dataLunch250 = combineData[(combineData['time'] == 'Lunch') & (combineData['Calories'] > 250)]
```

** Group by TimeId and See Which Had the Highest Calories Intake**
```
combineData.groupby('time')['Calories'].sum()
```

**Sort the Data by Calories in Descending Order **
```
combineData.sort_values(by=['Calories'], ascending=False)
```

**Show Unique Values for 'Item' Column**
```
combineData["Item"].unique()
```

**Count Unique Dates in the Data**
```
combineData["Date New"].nunique()
```

**Rename Columns**
```
map = {"Calories": "Intake", "time": "time_of_day"}
combineData = combineData.rename(columns=map)
```

**Calculate Percentage Increase in Total Calories from 12th January to 13th January**
```
aggdata = combineData.groupby("Date New")['Intake'].sum().reset_index()
aggdata['previous_intake'] = aggdata['Intake'].shift(1)
aggdata['percent_change'] = (aggdata['Intake'] - aggdata['previous_intake']) / (aggdata['previous_intake'])
```

**Create a New Column Based on Time of Day Using Numpy**
```
combineData['meals'] = np.where(combineData['time_of_day'] == 'Snack', 'small meal', 'big meal')
```

**Rank the Food Items Based on Overall Intake for Each Time of Day**
```
rankData = combineData.groupby(['time_of_day', 'Item'])['Intake'].sum().reset_index()
rankData['rank'] = rankData.groupby('time_of_day')['Intake'].rank(method='first', ascending=False)
```

**Plot Total Calories by Time of Day**
```
import matplotlib.pyplot as plt
plotdata = combineData.groupby("time_of_day")["Intake"].sum()
plt.bar(plotdata.index, plotdata.values)
plt.title('Total Calories by Time')
plt.xlabel('Time')
plt.ylabel('Total Calories')
plt.legend(['Total Calories'])
plt.show()
```

**Find Rows Containing the Item Name "Pasta"**
```
combineData['ifpasta'] = combineData['Item'].str.find('Pasta')
combineData[combineData['ifpasta'] > -1]
```
