## Analyzing the Impact of Covid-19 on Employment and Idustries in the US (Part 1)
                      
The corona virus (COVID -19) has created an economic crisis worldwide, forcing businesses and big firms to shut down in order to stop the fast spread of the virus. This lock down has negatively impacted the global employment levels including the US. For instance, certain industries filed for bankruptcy, which in result had to layoff many of its employees. Nevertheless, many idustries benefited from the pandemic including online retailing such as Amazon, supermarkets, entertainment such as Netflix, etc.

This project focuses on certain aspects to understand which industries, demographic backgrounds, and employees educational attainment are more vulnerable to unprecedented events that cuase economic crisis such as COVID-19.

The Analysis Includes The Following Steps:
1) Analyzing unemployment rates in the US before and after COVID-19 for the age of 16 and over.

2) Analyzing unemployment rates by ethnicity in the US before and after COVID-19 for the age of 16 and over.

3) Analyzing unemployment rates by educational attainment in the US before and after COVID-19.

4) Narrowing the analysis to the five hights populated states in the US. These states are: California, Texas, Florida, New York, and Pennsylvania.

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
### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/fofo123-r/Wafa-DATS6103/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
