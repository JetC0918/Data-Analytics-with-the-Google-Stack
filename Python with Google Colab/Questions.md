1.	read data from folder		
``` 
from google.colab import drive
drive.mount('/content/gdrive')
import pandas as pd
data = pd.read_csv('/content/gdrive/My Drive/calories (1).csv')
``` 

2.	write a command to see what type of columns are present
``` 
data.info() 
``` 
		
3.	show only the first 15 rows of the data		 
``` 
data.head(15)
``` 

4.	convert datenew column to date or datetime type	
``` 
data['Date New'] = data['Date New'].apply(pd.to_datetime)
``` 
	
5.	combine calories and time day table with common link	
``` 	
combinedata = data.merge(timedata, left_on = 'TimeId', right_on ='timeId')
``` 
 
6.	drop the duplicate time id column	
``` 
combinedata.drop('timeId', axis=1, inplace=True)
``` 
	
7.	show only the data for breakfast		
``` 
dataBreakfast = combinedata[combinedata ['time']  == 'Breakfast' ] 
``` 

8.	show only data where it is lunch and calories is more than 250
``` 
dataLunch250 = combineData[(combineData['time'] == 'Lunch') & (combineData['Calories'] > 250)]
``` 
		
9.	group by timeid and see which had the highest calories intake	
``` 
combineData.groupby('time')['Calories'].sum()
``` 
	
10.	sort the data by calories descending order	
``` 
combineData.sort_values(by=['Calories'], ascending = False)
``` 
	
11.	show the unique values for Item column	
``` 
combineData["Item"].unique()
``` 
	
12.	How many unique dates does the data have
``` 	 
combineData["Date New"].nunique() 
``` 
	
13	rename calories column to intake and time column to time_of_day	
``` 	
map = {"Calories": "Intake", "time": "time_of_day"}
combineData = combineData.rename(columns=map)
``` 

14.	from 12 january to 13 january show % increase in total calories	
``` 
# group required data
aggdata = combineData.groupby("Date New")['Intake'].sum().reset_index()
# shift function, similar to LAG on SQL
aggdata['previous_intake'] = aggdata['Intake'].shift(1) 
# calculate
aggdata['percent_change'] = (aggdata['Intake'] - aggdata['previous_intake']) / (aggdata['previous_intake'])
``` 
	
15.	use some function in numpy to create a new column which says small meal if it is snack, otherwise says main meal
``` 
combineData['meals'] = np.where(combineData['time_of_day'] == 'Snack', 'small meal', 'big meal')
``` 
		
16.	rank the food which had the highest overall intake for breakfast,dinner and so on
``` 
# group required data		
rankData = combineData.groupby(['time_of_day', 'Item'])['Intake'].sum().reset_index()
# rank the data
rankData['rank'] = rankData.groupby('time_of_day')['Intake'].rank(method='first',ascending = False)
``` 

17.	plot the time categories and overall calories using some plotting function
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
		
18.	Write a code to find rows which contain the item name as pasta 
``` 
combineData['ifpasta'] = combineData['Item'].str.find('Pasta')
combineData[combineData['ifpasta'] > -1] 
``` 