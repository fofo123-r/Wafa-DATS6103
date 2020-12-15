  # Analyzing U.S. Universities
                         
Many students plan to invest in their future. They search for the best university that offer the best programs. In addition, students want to find the best affordable program that can be rewarding once they graduate. In this analysis students can have a great idea on which university to choose based on costs such as tuition fees, school supplies, etc and the salary they can earn after graduation.

# Data Source:
The data was extracted from the U.S. DEPARTMENT OF EDUCATION , who collect their data from the Integrated Postsecondary Education Data System (IPEDS). 

```
 open("link.html", "w").write(' Link ')
 open("link.html", "w").write(' Link ')

```
# Data Timeframe:

```
This data was last updated December 2, 2020.
```

## SECTION 1: UPLOADING AND SUMMARIZING THE DATA

```
# Importing the packages
import pandas
import pandas as pd
import numpy as np
import plotly.express as px
import seaborn as sns
```
# Uploading the file from

df = pd.read_csv('C:\\Users\\wafa\\Desktop\\Introduction to Data Mining\\universities analysis.csv')
```
```
# Printing the dataframe

df.head()
```
```
# The code below illustrates the number of rows and columns

df.shape
```
## SECTION 2: DATA FILTIRING & CLEANING
```
# Filtering the rows to choose the ivy-league for the analysis

ivy_list= ['Harvard University', 'Brown University','Columbia University in the City of New York', 'Cornell University', 'Princeton University','Yale University','Dartmouth College','Stanford University','Massachusetts Institute of Technology','Duke University','University of Pennsylvania']
filt = universities['University Name'].isin(ivy_list)

# Adding a column called Ivy league to distinguish the Ivy leagues
universities['Ivy league'] = filt
universities.head()
```

```
# Creating a vriable for ivy-league

ivy_league = universities[universities['Ivy league']]
ivy_league
```
```
# selecting admissions rate columns and converting it to a percentage

admission_rates = ivy_league[['University Name','Admission Rate']]
admission_rates['Admission Rate']= admission_rates['Admission Rate']*100
admission_rates
```
```
# selecting 25th and 75th percentile SAT scores

sat = ivy_league[['University Name','SAT Scores 25th (Critical Reading)','SAT Scores 25th (math)', 'SAT Scores 25th(writing)', 'SAT Scores 75th (critical reading)','SAT Scores75th(math)', 'SAT Scores 75th (writing)']]
sat
```
```
#  Using panda melt to change the dataFrame from wide format to long format. The function is useful to massage a dataFrame into a format where one or more columns are identifier variables (id_vars), while all other columns, considered measured variables (value_vars)

pd.melt(sat, id_vars=['University Name'], value_vars=['SAT Scores 25th (Critical Reading)','SAT Scores 25th (math)', 'SAT Scores 25th(writing)', 'SAT Scores 75th (critical reading)','SAT Scores75th(math)', 'SAT Scores 75th (writing)'])

```
```
# creating a vraiable for pd.melt, dropping the NaN values, and 

long_sat = pd.melt(sat, id_vars=['University Name'], value_vars=['SAT Scores 25th (Critical Reading)','SAT Scores 25th (math)', 'SAT Scores 25th(writing)', 'SAT Scores 75th (critical reading)','SAT Scores75th(math)', 'SAT Scores 75th (writing)'])
long_sat = long_sat.dropna()

# creating a 0 value for variable column in long_sat dataframe

sat_temp = long_sat['variable'].str.split("(",expand=True)
sat_temp
```
```
# adding two columns to long_sat dataframe and rena,ing them to  sat section and sate percentile

long_sat['sat section'] = sat_temp[1]
long_sat['sat percentile'] = sat_temp[0]
long_sat
```

```
# removing the variable column 

long_sat = long_sat.drop(columns = 'variable')
long_sat
```

```
# removing the parenthesis in the sat section variable 

long_sat['sat section'] = long_sat['sat section'].str.slice(0,-1)
long_sat
```
```
# converting the first letter of the tests names in the sat section column into lowercse 

long_sat['sat section'] = long_sat['sat section'].str.lower()
long_sat
```

```
# Selecting Average faculty salary per month

salary = ivy_league[['University Name', ' (Average Faculty Salary per month)']]
salary 
```
```
# check ivy_league columns

ivy_league.columns
```
```
# Selecting the columns needed from the ivy_league datarame

cost = ivy_league[['University Name','Average cost of attendance', 'In State Tuition', 'Out of  State Tuition']]

# Renaming the columns

cost = cost.rename(columns = {'Average cost of attendance': 'Average academic cost'})
cost
```
```
# creating a list for the required columns, a shortcut for choosing the columns

columns = list(cost.columns)
columns
```
```
#  Using panda melt to change the dataFrame from wide format to long format. The function is useful to massage a dataFrame into a format where one or more columns are identifier variables (id_vars), while all other columns, considered measured variables (value_vars)
# to chose from index 1 use this code value_vars=columns[1:] 

cost = pd.melt(cost, id_vars=['University Name'], value_vars=columns[1:], var_name = 'Fees', value_name = 'Value')
cost

```
```
# selcet Share of enrollment by ethnicity

# multiply the columns chosen by 100, since it is a percentage

enrollment = ivy_league[[ 'Undergraduate total enrollmant share- white', 'Undergraduate total enrollmant share- Black', 'Undergraduate total enrollmant share- Hispanic', 'Undergraduate total enrollmant share- Asian',
                         'Undergraduate total enrollmant share- American Indian/Alaska Native']]*100

# Add the university name to the dataframe for graphical representation
enrollment.insert(loc=0,value=ivy_league['University Name'],column = 'University Name')
enrollment
```
```
enroll = pd.melt(enrollment, id_vars=['University Name'], value_vars=['Undergraduate total enrollmant share- white', 'Undergraduate total enrollmant share- Black', 'Undergraduate total enrollmant share- Hispanic', 'Undergraduate total enrollmant share- Asian','Undergraduate total enrollmant share- American Indian/Alaska Native'], var_name = 'Ethnicity')
enroll
```

```
#Selceting Students' average earnigns

average_earnings = ivy_league[['University Name','Mean earnings of students working and not enrolled 10 years after entry in the highest income tercile $75,001']]
average_earnings
```

```
# filtiring the columns needed for the analysis

roi = ivy_league[['University Name',
                 'Mean earnings of students working and not enrolled 10 years after entry in the highest income tercile $75,001',
                
                'Average cost of attendance', 'In State Tuition','Out of  State Tuition']]
roi
```
## SECTION 3: GRAPHICAL REPRESENTATION
### (A) Ivy League Universities Acceptance Rates:
```
# Admissions rate

fig = px.bar(admission_rates, x = "University Name" , y = "Admission Rate", 
       color= "University Name" , title = 'Ivy League Universities Acceptance Rates')
fig.show()

```

![image](https://user-images.githubusercontent.com/74209404/102160143-dcf1b300-3e52-11eb-8096-8ad954c2520e.png)


### (B) Ivy League SAT Scores:
```
fig1 = px.scatter(long_sat, x = "University Name" , y = "value",  hover_data = {'sat percentile'},
       color= "University Name" , title = 'Ivy League SAT Scores',  facet_col = 'sat section', height = 800)
fig1.show()
```

![image](https://user-images.githubusercontent.com/74209404/102160211-fe529f00-3e52-11eb-820d-5f5cffc366c0.png)


### (C) Ivy League Faculty Average Monthly Salary:
```
fig2 = px.bar(salary, x = "University Name" , y = ' (Average Faculty Salary per month)', 
       color= "University Name" , title = 'Ivy League Average Monthly Salary')
fig2.show()

```

![image](https://user-images.githubusercontent.com/74209404/102160640-d1eb5280-3e53-11eb-881a-840391e92ccc.png)






### (D)  Percentage of Enrollment by Ethnicity in Ivy League Universities:
```
fig3 = px.bar(enroll, x = 'Ethnicity' , y = 'value', 
       color= 'University Name' , title = 'Percentage of Enrollment by Ethnicity', height = 1000, barmode = 'group')
fig3.show()
```


![image](https://user-images.githubusercontent.com/74209404/102160296-2a6e2000-3e53-11eb-8221-5f8ec8424eb6.png)

### (E) Ivy League Tuition Fees:
```
fig4 = px.bar(cost, x = 'Fees' , y = 'Value', 
       color= 'University Name' , title = 'Tuition fees', barmode = 'group')
fig4.show()

```

![image](https://user-images.githubusercontent.com/74209404/102160341-42de3a80-3e53-11eb-9ea6-63e5485b0d6e.png)



### (F)  Analysis 1: Tuition Cost Vs. Income in Ivy League Universities Only:
```
px.scatter(roi, x = 'Average cost of attendance' , y = 'Mean earnings of students working and not enrolled 10 years after entry in the highest income tercile $75,001', 
       color= 'University Name' , title = 'Analysis')
```


![image](https://user-images.githubusercontent.com/74209404/102160417-66a18080-3e53-11eb-933c-db285ddad572.png)


### (H)  Analysis 2: Comparison of Tuition Cost Vs. Income in Ivy League and Non Ivy League:
```
px.scatter(universities, x = 'Average cost of attendance' , y = 'Mean earnings of students working and not enrolled 10 years after entry in the highest income tercile $75,001', 
       color= 'Ivy league' , title = 'Analysis', hover_data ={'University Name'})
       

```

![image](https://user-images.githubusercontent.com/74209404/102160475-86d13f80-3e53-11eb-93f8-bd58a6a8ee42.png)


### (I)  Analysis 3: Comparison of Admission rate Vs. SAT Scores 25th (math) in Ivy League and Non Ivy League:
```
px.scatter(universities, x = "Admission Rate" , y = 'SAT Scores 25th (math)', 
       color= 'Ivy league' , title = 'Analysis', hover_data ={'University Name'})

```


![image](https://user-images.githubusercontent.com/74209404/102160558-a8322b80-3e53-11eb-85a7-5ff03cb3ebab.png)

## SECTION 4: Conclusion

### Acceptance Rates

When comparing the Ivy league universities, we see that harvard has the lowest acceptance rate while Cornell has the Highest acceptance rate

###  Ivy League SAT Scores

We see that Harvard has the highest SAT scores in the 75th percentile for crtitcal reading. As for the 75th percentile,  Harvard, Yale, Princton, and Columbia have equal math and writing SAT scores. 

Cornell University has the lowest 75th and 25th percentiles SAT Scores in reading and math. Dartmouth university has the lowest 25th percentile SAT scores in writing

### Ivy League Faculty Average Monthly Salary

fig2 shows that Harvard, Princton and Stanford faculty have the highest average monthly salary. While Dartmouth university Faculty has the lowest average monthly salary.

### percentage of Enrollment By Ethnicity

fig3 shows that the white ethnicity has the highest enrollmet in ivy league universities compared to other ethnicities and the second highest is the Asian ethnicitiy. Nevertheless, American Indians have the lowest percentage enrollment.


### Ivy League Tuition Fees

fig4 shows that Columbia university had the highest tuition fees, while Princeton university has the lowest tuition fees.



### Analysis

Students who are interested in attending the lowest cost university in which the univesity aulumuni earn the higest earning in the 75,000 should join Massachusetts Institute of Technology(MIT). The average cost of attending (MIT) is 67K and the average earning is 172K.

Students who cannot afford to join Ivy League universities and want to join a university in which it's aulumni can earn as high as an ivey league aulumni should apply for the United States Merchant Marine Academy in which its alumni earn an average of 103K which is higher than Brown aulumni who earn an average of 91.7K.
