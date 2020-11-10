## Analyzing the Impact of Covid-19 on Employment and Industries in the US (Part 1)
                      
The corona virus (COVID -19) has created an economic crisis worldwide, forcing businesses and big firms to shut down in order to stop the fast spread of the virus. This lock down has negatively impacted the global employment levels including the US. For instance, certain industries filed for bankruptcy, which in result had to layoff many of its employees. Nevertheless, many idustries benefited from the pandemic including online retailing such as Amazon, supermarkets, entertainment such as Netflix, etc.

This project focuses on certain aspects to understand which industries, demographic backgrounds, and employees educational attainment are more vulnerable to unprecedented events that cuase economic crisis such as COVID-19.

The Analysis Includes The Following Steps:
1) Analyzing unemployment rates in the US before and after COVID-19 for the age of 16 and over.

2) Analyzing unemployment rates by ethnicity in the US before and after COVID-19 for the age of 16 and over.

3) Analyzing unemployment rates by educational attainment in the US before and after COVID-19.

4) Narrowing the analysis to five states that have the highest Nonveterans population levels, who are 18 years and over in 2019. These states are: California, Texas, Florida, New York, and Pennsylvania.

5) Analyzing unemployment rates in the chosen five states.

6) Analyzing the employment level by industry in the chosen five states before and after COVID-19.

# Data Source:
The data was extracted from the US Bureau of Labor Statistics: open("link.html", "w").write(' Link ')

# Data Timeframe:
The years chosen in this analysis consist of 1 year and 10 months, from Jan 2019 - October 2020. However, the population data for the five states for the year of 2020 is not avaiable; therefore, the population analysis includes only one year which is 2019.
```
# Importing the packages
import requests
import json
import pandas as pd 
import plotly.express as px
```
```
# Defining the variable and the Series ID needed for the Analysis
uneployment_rate = {"Total Uneploymentt": "LNS14000000"}
```
```
# Creating a list to only print out the Series IDs
l = list(uneployment_rate .values())
l
```
```
# Extracting the data from BLS.gov using API

json_l= []

headers = {'Content-type': 'application/json'}
data = json.dumps({"seriesid":l,"startyear":"2019", "endyear":"2020"})
p = requests.post('https://api.bls.gov/publicAPI/v2/timeseries/data/', data=data, headers=headers)
json_d = json.loads(p.text)
json_l.append(json_d)
```

```
# print the data to see how it looks like

print (json_d)

```

```
# from the results above the data includes "Results" and "Series". The code below prints it.

json_d["Results"]["series"]
```
```
# Creating a loop for total unemployment, keys which include the variable, and Values that include Series IDs

list_ue = []
counter = 0

s = json_d["Results"]["series"]

for key,value in uneployment_rate.items():
    print(key,value)

    if s [counter] ["seriesID"] == value:
        ue=pd.DataFrame.from_dict(data = s[counter]["data"])
        ue["unemployment"] = key
        list_ue.append(ue)
        counter+=1
       
    
final_ue = pd.concat(list_ue)
final_ue
```
```
# Cleaning the data and choosing the needed columns for the analysis

final_ue = final_ue[["year","periodName","value"]]
final_ue
```
```
# renaming the columns

final_ue = final_ue.rename(columns = {"year": "Year", "periodName": "Month", "value": "Total Unemployment Rates"}) 
final_ue
```
```
# formating the year and the month and joining them together 

final_ue["Date"] = final_ue["Year"] + final_ue["Month"] 

final_ue["Date"] = pd.to_datetime(final_ue["Date"], format = "%Y%B")

final_ue
```
```
# plotting the above data, which is Unemployment Rates in The US 

fig = px.area(final_ue, x = "Date" , y = "Total Unemployment Rates", 
        title = "Unemployment Rates in The US - 16 years and over")

fig.update_xaxes(
    dtick="M1",
    tickformat="%b\n%Y")
fig.show()
```
![image](https://user-images.githubusercontent.com/74209404/98617704-921dd200-22cd-11eb-89a6-4e6ba6ebca6a.png)


```
# Defining the variables and the Series IDs needed for the Analysis

unemployment_race = {"Hispanic" : 'LNS14000009',"African American":'LNS14000006',
                   "White": 'LNS14000003',
                   "Asian": 'LNS14032183'} 

unemployment_race
```

```
# Creating a list to only print out the Series IDs

lst = list(unemployment_race.values())
lst
```
```
# Extracting the data from BLS.gov using API

json_lists = []

headers = {'Content-type': 'application/json'}
data = json.dumps({"seriesid":lst,"startyear":"2019", "endyear":"2020"})
p = requests.post('https://api.bls.gov/publicAPI/v2/timeseries/data/', data=data, headers=headers)
json_data = json.loads(p.text)
json_lists.append(json_data)
```

```
# print the data to see how it looks like

print (json_data)
```
```
# from the results above the data includes "Results" and "Series". The code below prints it.

json_data["Results"]["series"]
```
```
# Creating a loop for unemployment by ethnicity, keys which include the variables, and Values that include Series IDs

list_df = []
counter = 0

s = json_data["Results"]["series"]

for key,value in unemployment_race.items():
    print(key,value)

    if s [counter] ["seriesID"] == value:
        df=pd.DataFrame.from_dict(data = s[counter]["data"])
        df["Ethnicity"] = key
        list_df.append(df)
        counter+=1
     
final_df = pd.concat(list_df)
final_df
```
```
final_df = final_df[["year","periodName","value","Ethnicity"]]
final_df
```
```
final_df = final_df.rename(columns = {"year": "Year", "periodName": "Month", "value": "Unemployment Rates"}) 
final_df
```
```
final_df["Date"] = final_df["Year"] + final_df["Month"] 

final_df["Date"] = pd.to_datetime(final_df["Date"], format = "%Y%B")

final_df
```
```
fig1 = px.bar(final_df, x = "Date" , y = "Unemployment Rates", labels = {"Value": "Ethnicity"}, 
       color= "Ethnicity" , title = "Unemployment Rates by Ethnicity - 16 years and over",  barmode = "group")

fig1.update_xaxes(
    dtick="M1",
    tickformat="%b\n%Y")
fig1.show()
```

![image](https://user-images.githubusercontent.com/74209404/98617916-107a7400-22ce-11eb-9bd6-914f8d881c3b.png)




```
unemployment_educ = {"unemployment rate":{"Less than a High School diploma" : 'LNS14027659', "High School graduates, no college" : 'LNS14027660', "Some college or associate degree": 'LNS14027689', "Bachelor's degree and higher": 'LNS14027662'}} 
```

```
t = []

for k,b in unemployment_educ.items():
    t+=list(b.values())
t 
```

```
json_t= []

headers = {'Content-type': 'application/json'}
data = json.dumps({"seriesid":t,"startyear":"2019", "endyear":"2020"})
p = requests.post('https://api.bls.gov/publicAPI/v2/timeseries/data/', data=data, headers=headers)
json_da = json.loads(p.text)
json_t.append(json_da)
```

```
print (json_da)
```

```
json_da["Results"]["series"]
```
```
list_edu = []
counter = 0

s = json_da["Results"]["series"]

for key,value in unemployment_educ["uemployment rate"].items():
    

    if s[counter]["seriesID"] == value:
        edu=pd.DataFrame.from_dict(data = s[counter]["data"])
        edu["Education"] = key
        list_edu.append(edu)
        counter+=1     
    
final_edu = pd.concat(list_edu)
final_edu
```
```
final_edu = final_edu[["year","periodName","value","Education"]]
final_edu
```
```
final_edu = final_edu.rename(columns = {"year": "Year", "periodName": "Month", "value": "Unemployment Rates", "Education":"Education"}) 
final_edu
```
```
final_edu["Date"] = final_edu["Year"] + final_edu["Month"] 

final_edu["Date"] = pd.to_datetime(final_edu["Date"], format = "%Y%B")

final_edu
```
```
fig2 = px.line(final_edu, x = "Date" , y = "Unemployment Rates", labels = {"Value": "Education"}, color = "Education"
        ,title = "Unemployment Rates by Education")

fig2.update_xaxes(
    dtick="M1",
    tickformat="%b\n%Y")
fig2.show()
```
![image](https://user-images.githubusercontent.com/74209404/98618067-60593b00-22ce-11eb-8864-fc55304cb9f4.png)

## Analyzing the Impact of Covid-19 on Employment in the US (Part 2)

```
# Defining the variables and the Series IDs needed for the Analysis

state_seriesid = {"California":{"unemployment_rate" : 'LASST060000000000003', "Minning" : 'SMS06000001000000001', "Construction": 'SMS06000002000000001', "Manufacturing": 'SMS06000003000000001', "Trade & Transportation" : 'SMS06000004000000001', "Financial Activities": 'SMS06000005500000001', "Education & Health Services": 'SMS06000006500000001', "Leisure & Hospitality": 'SMS06000007000000001', "Government": 'SMS06000009000000001', "Population": 'LNU0004960106'}, 
                 "Texas":{"unemployment_rate" : 'LASST480000000000003', "Minning" : 'SMS48000001000000001', "Construction": 'SMS48000002000000001', "Manufacturing": 'SMS48000003000000001', "Trade & Transportation" : 'SMS48000004000000001', "Financial Activities": 'SMS48000005500000001', "Education & Health Services": 'SMS48000006500000001', "Leisure & Hospitality": 'SMS48000007000000001', "Government": 'SMS48000009000000001', "Population": 'LNU0004960148'},
                 "Florida":{"unemployment_rate" : 'LASST120000000000003', "Minning" : 'SMU12000001000000001', "Construction": 'SMS12000002000000001', "Manufacturing": 'SMS12000003000000001', "Trade & Transportation" : 'SMS12000004000000001', "Financial Activities": 'SMS12000005500000001', "Education & Health Services": 'SMS12000006500000001', "Leisure & Hospitality": 'SMS12000007000000001', "Government": 'SMS12000009000000001', "Population": 'LNU0004960112'},
                  "Newyork":{"unemployment_rate" : 'LASST360000000000003', "Minning" : 'SMS36000001000000001', "Construction": 'SMS36000002000000001', "Manufacturing": 'SMS36000003000000001', "Trade & Transportation" : 'SMS36000004000000001', "Financial Activities": 'SMS36000005500000001', "Education & Health Services": 'SMS36000006500000001', "Leisure & Hospitality": 'SMS36000007000000001', "Government": 'SMS36000009000000001', "Population": 'LNU0004960136'},
                  "Pennsylvania":{ "unemployment_rate" : 'LASST420000000000003', "Minning" : 'SMS42000001000000001', "Construction": 'SMS42000002000000001', "Manufacturing": 'SMS42000003000000001', "Trade & Transportation" : 'SMS42000004000000001', "Financial Activities": 'SMS42000005500000001', "Education & Health Services": 'SMS42000006500000001', "Leisure & Hospitality": 'SMS42000007000000001', "Government": 'SMS42000009000000001', "Population": 'LNU0004960142'}}
  ```
   ``` 
  # Creating a loop to only print out the Series IDs 
lst = []

for k,b in state_seriesid.items():
    lst+=list(b.values())
  ```
   ``` 
  # Printing out the Series IDs
lst
``` 
``` 
# Since the website allows only to print 25 values at once, I had to slice the list

lst1 = lst[:25]
lst2 = lst[25:]
``` 
``` 
# Extracting the data from BLS.gov using API
# Did a loop to include the two lists and choose the years
json_lists = []

for l in [lst1,lst2]: 
    headers = {'Content-type': 'application/json'}
    data = json.dumps({"seriesid":l,"startyear":"2018", "endyear":"2020"})
    p = requests.post('https://api.bls.gov/publicAPI/v2/timeseries/data/', data=data, headers=headers)
    json_data = json.loads(p.text)
    json_lists.append(json_data)
``` 
``` 
# print the data to see how it looks like
# we see that all the population series IDs has no data for year 2020
#print (json_data)
#json_data["Results"]["series"]
``` 
```
# joining the two lists

s = json_lists[0]["Results"]["series"] + json_lists[1]["Results"]["series"]
s
```

```
# Creating a loop for states, keys which include the variables, and Values that include Series IDs

list_df = []
counter = 0

for state,dic in state_seriesid.items():

    for key,value in dic.items():
         
        if s [counter] ["seriesID"] == value:
            df=pd.DataFrame.from_dict(data = s[counter]["data"])
            df["label"] = key
            df["State"] = state
            list_df.append(df)
            counter+=1
       
    
final_df = pd.concat(list_df)
final_df
```
```
# creating state Abbreviation
ab_states = {"California": "CA", "Texas": "TX", "Florida": "FL", "Newyork": "NY", "Pennsylvania": "PA"}
ab_states 
```
```
# Adding the state Abbreviation to the final_df data
final_df.loc[:,"StateAbbreviation"] = final_df.loc[:,"State"].map(ab_states)
```
```
# Cleaning the data and choosing the needed columns for the analysis

final_df = final_df[["year", "periodName","label","State","value", "StateAbbreviation"]]
final_df
```
```
# renaming the columns

final_df = final_df.rename(columns = {"year": "Year", "periodName": "Month", "value": "Value"}) 
final_df
```
```
# Since the population data is annual, it had to be filtered 

population_filter = final_df["label"] == "Population"
population_filter 
```
```
# define the filtered population to graph it by it self

population_dataframe = final_df[population_filter].copy()
population_dataframe 
```
```
# choosing 2019 year for population only
pop_2019 = population_dataframe["Year"] == "2019"
pop_2019
```
```
# printing population data for the year of 2019 only 

population = population_dataframe[pop_2019].copy()
population
```
```
fig = px.choropleth(population, locations="StateAbbreviation", locationmode="USA-states", 
    color= "State" , scope="usa", labels = {"Value": "Value"}, hover_data = ["State", "Value"], title = "Population Level in Thousands - Nonveterans, 18 years and over in 2019")
fig.show()
```
[image](https://user-images.githubusercontent.com/74209404/98619230-d068c080-22d0-11eb-9fa8-5844ff8c7ef1.png)
```
# Drop the population variable from the final data

final_df = final_df[~population_filter]
final_df
```
```
# formating the year and the month and joining them together 

final_df["Date"] = final_df["Year"] + final_df["Month"] 

final_df["Date"] = pd.to_datetime(final_df["Date"], format = "%Y%B")

final_df
```

```
 #choosing the date from Jan 2019 and above
 
time_filter = final_df["Date"] >= "2019-01-01"
final_df = final_df[time_filter]
final_df
```
```
# Filter the data by unemployment rate 

filter_in = final_df["label"]=="unemployment_rate"
filter_in
```
```
# Graphing the unemployment Rates for each state

fig1 = px.bar(final_df[filter_in], x = "Date" , y = "Value", labels = {"Value": "Rates"}, 
       color= "State" , title = "Unemployment Rates",  barmode = "group")

fig1.update_xaxes(
    dtick="M1",
    tickformat="%b\n%Y")
fig1.show()
```


![image](https://user-images.githubusercontent.com/74209404/98620004-681ade80-22d2-11eb-90f6-07bd386728e4.png)


```
# Graphing number of employees for several indutries by each state
# Did a loop to choose the label by industries and drop unemployment rates

for industry in final_df.loc[~filter_in,"label"].unique():
    filter_ind = final_df["label"]== industry
    fig2 = px.bar(final_df[filter_ind], x= "Date" , y = "Value", labels = {"Value": "All Employees, In Thousands"}, color = "State" ,  title = f"{industry} Industry", barmode = "group")
    
    fig2.update_xaxes(
    dtick="M1",
    tickformat="%b\n%Y")
    fig2.show()
  ```
  
![image](https://user-images.githubusercontent.com/74209404/98620100-9f898b00-22d2-11eb-8a8c-d7a6aa223d32.png)

![image](https://user-images.githubusercontent.com/74209404/98620177-c5169480-22d2-11eb-8737-8315c3b91fe9.png)

![image](https://user-images.githubusercontent.com/74209404/98620248-ec6d6180-22d2-11eb-8985-79c464939fca.png)

![image](https://user-images.githubusercontent.com/74209404/98620330-1b83d300-22d3-11eb-9aa5-58c402f73c3f.png)

![image](https://user-images.githubusercontent.com/74209404/98620445-5128bc00-22d3-11eb-8db5-f64a316c0725.png)

![image](https://user-images.githubusercontent.com/74209404/98620524-6f8eb780-22d3-11eb-99d9-6293490fb107.png)

![image](https://user-images.githubusercontent.com/74209404/98620586-8c2aef80-22d3-11eb-96ee-bb9a587a3b65.png)

![image](https://user-images.githubusercontent.com/74209404/98620642-abc21800-22d3-11eb-80c8-4f4382a1f6c8.png)

