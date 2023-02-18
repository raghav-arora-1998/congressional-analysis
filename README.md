# Congressional-Analysis

## Variable Explanation:
The additional variables we selected from the census data set are Employment status for district populations for those over the age of 16 total, total not in labor force, and total in labor force.

It is important to note that the labor force is made up of those who are either employed or unemployed. With actively employed persons being in the employed category and those who are actively seeking work in the unemployed category. Those who fall under neither category are considered not in the labor force.

## Hypothesis
Based on our previous analysis, we believe that republicans will have a lower labor force participation rate than democrats becuase they tend to have a higher median age, rate of disability, rate of retired individuals, rate of stay at home moms, and a lower rate of health insurance.

This is because we expect republican districts to have a higher demographic of older people and others who support their platforms based on so called traditional family values. Amongst this group we expect that older people will be retired as well as be more likely to have a disability. We also expect women who work from home are more likely to support republicans due to their support of the nuclear family. Since the democratic party traditionally supports healthcare for all we expect that districts which are controlled by democrats will have more constituents with health care coverage. Households with healthcare are less likely to be out of the labor force as they will not need to stay home to take care of others.

## Previous Hypothesis
According to the bureau of labor statistics those who fall in the category of not in the labor force are typically either in school, retired, or have family responsibilities such as caring for a sick family member. We hypothesize that these groups are all more likely to support democratic congressional candidates. We believe college students are more likely to support democratic candidates due to social believes and potential student loan payouts, retired persons are more likely to support democratic candidates as they typically support medicare and medicaid programs that will benefit the elderly, and those caring for the sick are more likely to support democratic candidates due to their support of healthcare programs.

## Variable Explanation:
The additional variables we selected from the census data set are Employment status for district populations for those over the age of 16 total, total not in labor force, and total in labor force.

It is important to note that the labor force is made up of those who are either employed or unemployed. With actively employed persons being in the employed category and those who are actively seeking work in the unemployed category. Those who fall under neither category are considered not in the labor force.

## Data collection and Wrangling

### Our variables:
B23025_001E: EMPLOYMENT STATUS FOR THE POPULATION 16 YEARS AND OVER
Estimate!!Total:
B23025_007E: EMPLOYMENT STATUS FOR THE POPULATION 16 YEARS AND OVER
Estimate!!Total:!!Not in labor force
B23025_002E: EMPLOYMENT STATUS FOR THE POPULATION 16 YEARS AND OVER
Estimate!!Total:!!In labor force \

### New variables:

#### import needed packages

```
import pandas as pd
import json
from urllib.request import urlopen
import matplotlib.pyplot as plt
import plotly.express as px
from urllib import request
import json
import numpy as np
import seaborn as sns
```

```
# With all variables
url="https://api.census.gov/data/2021/acs/acs1?get=NAME,B23025_001E,B23025_007E,B01002_001E,B23001_009E,B23001_016E,B23001_023E,B23001_030E,B23001_037E,B23001_044E,B23001_051E,B23001_058E,B23001_065E,B23001_072E,B23001_077E,B23001_082E,B23001_087E,B23001_095E,B23001_102E,B23001_109E,B23001_116E,B23001_123E,B23001_130E,B23001_137E,B23001_144E,B23001_151E,B23001_158E,B23001_163E,B23001_168E,B23001_173E,B12006_007E,B12006_012E,B12006_018E,B12006_023E,B13012_005E,B13012_008E,B14005_006E,B14005_020E,B16010_009E,B16010_022E,B16010_035E,B16010_048E,B17005_007E,B17005_012E,B23003_016E,B27011_014E,C18120_010E,B23007_009E,B23007_009E&for=congressional%20district:*&in=state:*"
#,B14005_006E,B14005_020E. ,'Male in School','Female in school'
response = urlopen(url)
data_json = json.loads(response.read())
data_json.pop(0)
cols = ['District Name','Total Households','Not in labor force','Median Age','Male 16 to 19','Male 20 and 21','Male 22 to 24','Male 25 to 29','Male 30 to 34','Male 35 to 44','Male 45 to 54','Male 55 to 59','Male 60 and 61','Male 62 to 64','Male 65 to 69','Male 70 to 74','Male 75+','Female 16 to 19','Female 20 and 21','Female 22 to 24','Female 25 to 29','Female 30 to 34','Female 35 to 44','Female 45 to 54','Female 55 to 59','Female 60 and 61','Female 62 to 64','Female 65 to 69','Female 70 to 74','Female 75+','Never Married Male NIL','Never Married Female NIL','Currently Married Male NIL','Currently Married Female NIL','Birth In Past 12 Months Married NIL','Birth In Past 12 Months Not Married NIL','Enrolled in School Male Age 16 to 19 NIL','Enrolled in School Female Age 16 to 19 NIL','Did Not Graduate from Highschool NIL Age 25+ NIL','High School Graduate Age 25+ NIL','Some College or Assosciates Degree Age 25+ NIL','Bachelors Degree or Higher Age 25+ NIL','Below Poverty Level in Past 12 Months Male NIL','Below Poverty Level in Past 12 Months Female NIL','Have Children Under 18 and are Female 20-64 yrs old NIL','With Health Insurance NIL','With Disability NIL','Child-Under18 Husband-IL-UnEmp Wife-NIL','Child-Under18 Husband-IL-Emp Wife-NIL','State code', 'district number']
acs_df = pd.DataFrame(data_json, columns = cols)
acs_df[['District', 'State']] = acs_df['District Name'].str.split(',', expand=True)
acs_df['district number'] = pd.to_numeric(acs_df['district number'])
acs_df['State'] = acs_df['State'].str.strip()
acs_df = acs_df[['Total Households', 'Not in labor force', 'Median Age','Enrolled in School Male Age 16 to 19 NIL','Enrolled in School Female Age 16 to 19 NIL','Male 16 to 19','Male 20 and 21','Male 22 to 24','Male 25 to 29','Male 30 to 34','Male 35 to 44','Male 45 to 54','Male 55 to 59','Male 60 and 61','Male 62 to 64','Female 16 to 19','Female 20 and 21','Female 22 to 24','Female 25 to 29','Female 30 to 34','Female 35 to 44','Female 45 to 54','Female 55 to 59','Female 60 and 61','Female 62 to 64','Never Married Male NIL','Never Married Female NIL','Currently Married Male NIL','Currently Married Female NIL','Birth In Past 12 Months Married NIL','Birth In Past 12 Months Not Married NIL','Did Not Graduate from Highschool NIL Age 25+ NIL','High School Graduate Age 25+ NIL','Some College or Assosciates Degree Age 25+ NIL','Bachelors Degree or Higher Age 25+ NIL','Below Poverty Level in Past 12 Months Male NIL','Below Poverty Level in Past 12 Months Female NIL','Have Children Under 18 and are Female 20-64 yrs old NIL','With Health Insurance NIL','With Disability NIL','Female 65 to 69','Female 70 to 74','Female 75+','Male 65 to 69','Male 70 to 74','Male 75+','Child-Under18 Husband-IL-UnEmp Wife-NIL','Child-Under18 Husband-IL-Emp Wife-NIL','district number', 'State']]
acs_df.head()
```
```
list(acs_df.columns.values)
```


### Merge with Congressional Statistics, 2021

https://www.ssa.gov/policy/docs/factsheets/cong_stats/

```
url = 'https://raw.githubusercontent.com/maxsohl/CongressionalBook/main/Retire_disab.csv'
ret_disab_df = pd.read_csv(url)
```
```
states = {
        'AK': 'Alaska',
        'AL': 'Alabama',
        'AR': 'Arkansas',
        'AS': 'American Samoa',
        'AZ': 'Arizona',
        'CA': 'California',
        'CO': 'Colorado',
        'CT': 'Connecticut',
        'DC': 'District of Columbia',
        'DE': 'Delaware',
        'FL': 'Florida',
        'GA': 'Georgia',
        'GU': 'Guam',
        'HI': 'Hawaii',
        'IA': 'Iowa',
        'ID': 'Idaho',
        'IL': 'Illinois',
        'IN': 'Indiana',
        'KS': 'Kansas',
        'KY': 'Kentucky',
        'LA': 'Louisiana',
        'MA': 'Massachusetts',
        'MD': 'Maryland',
        'ME': 'Maine',
        'MI': 'Michigan',
        'MN': 'Minnesota',
        'MO': 'Missouri',
        'MP': 'Northern Mariana Islands',
        'MS': 'Mississippi',
        'MT': 'Montana',
        'NA': 'National',
        'NC': 'North Carolina',
        'ND': 'North Dakota',
        'NE': 'Nebraska',
        'NH': 'New Hampshire',
        'NJ': 'New Jersey',
        'NM': 'New Mexico',
        'NV': 'Nevada',
        'NY': 'New York',
        'OH': 'Ohio',
        'OK': 'Oklahoma',
        'OR': 'Oregon',
        'PA': 'Pennsylvania',
        'PR': 'Puerto Rico',
        'RI': 'Rhode Island',
        'SC': 'South Carolina',
        'SD': 'South Dakota',
        'TN': 'Tennessee',
        'TX': 'Texas',
        'UT': 'Utah',
        'VA': 'Virginia',
        'VI': 'Virgin Islands',
        'VT': 'Vermont',
        'WA': 'Washington',
        'WI': 'Wisconsin',
        'WV': 'West Virginia',
        'WY': 'Wyoming'
}


### Convert the state abbreviations to state names using the map function
ret_disab_df['State'] = ret_disab_df['State'].map(states)
ret_disab_df

```

```
# Remove commas from numbers 
ret_disab_df['Retired'] = pd.to_numeric(ret_disab_df['Retired'].str.replace(',', ''))
ret_disab_df['disabled'] = pd.to_numeric(ret_disab_df['disabled'].str.replace(',', ''))
```
```
#Check datatypes
print(ret_disab_df.dtypes)
```
```
#Merge here
acs_df = pd.merge(acs_df, ret_disab_df, on=["State", "district number"])
acs_df
```
```
#Convert the number columns into number data types

df_n = acs_df[['Total Households', 'Not in labor force', 'Median Age','Enrolled in School Male Age 16 to 19 NIL','Enrolled in School Female Age 16 to 19 NIL','Male 16 to 19','Male 20 and 21','Male 22 to 24','Male 25 to 29','Male 30 to 34','Male 35 to 44','Male 45 to 54','Male 55 to 59','Male 60 and 61','Male 62 to 64','Female 16 to 19','Female 20 and 21','Female 22 to 24','Female 25 to 29','Female 30 to 34','Female 35 to 44','Female 45 to 54','Female 55 to 59','Female 60 and 61','Female 62 to 64','Never Married Male NIL','Never Married Female NIL','Currently Married Male NIL','Currently Married Female NIL','Birth In Past 12 Months Married NIL','Birth In Past 12 Months Not Married NIL','Did Not Graduate from Highschool NIL Age 25+ NIL','High School Graduate Age 25+ NIL','Some College or Assosciates Degree Age 25+ NIL','Bachelors Degree or Higher Age 25+ NIL','Below Poverty Level in Past 12 Months Male NIL','Below Poverty Level in Past 12 Months Female NIL','Have Children Under 18 and are Female 20-64 yrs old NIL','With Health Insurance NIL','With Disability NIL','Female 65 to 69','Female 70 to 74','Female 75+','Male 65 to 69','Male 70 to 74','Male 75+','Child-Under18 Husband-IL-UnEmp Wife-NIL','Child-Under18 Husband-IL-Emp Wife-NIL','Retired','disabled']]
col = df_n.columns
acs_df[col] = acs_df[col].apply(pd.to_numeric, errors='coerce')
acs_df = acs_df.fillna(0)

acs_df['% of households not in labor force'] = (100*acs_df['Not in labor force'])/acs_df['Total Households']
acs_df['% Age 16-19 in School'] = (100*(acs_df['Enrolled in School Male Age 16 to 19 NIL']+acs_df['Enrolled in School Female Age 16 to 19 NIL']))/(acs_df['Male 16 to 19']+acs_df['Female 16 to 19'])
acs_df['% not-in-LaborF above 65'] = (100*(acs_df['Male 65 to 69']+acs_df['Male 70 to 74']+acs_df['Male 75+']+acs_df['Female 65 to 69']+acs_df['Female 70 to 74']+acs_df['Female 75+']))/acs_df['Not in labor force']
acs_df.head()
```

### Party affiliation data

```
# read from the congressional data and put into a pandas dataframe
party_df = pd.read_csv("http://goodcsv.com/wp-content/uploads/2020/08/us-house-of-representatives-2020.csv", encoding = "ISO-8859-1")

# extract the district number from the data (it was in the format of 5th and we want that to just be 5) using a regular expression.
party_df['district number'] = party_df['District/Position'].str.extract('(\d+)')
party_df['district number'] = party_df['district number'].fillna(0)
party_df['district number'] = pd.to_numeric(party_df['district number'])
party_df['State'] = party_df['State/Territory']
party_df['State'] = party_df['State'].str.strip()
party_df['Party'] = party_df['Party'].str.strip() # remove extraneous whitespace

# Let's just keep the columns we need
party_df = party_df[['State', 'Party', "district number"]]

party_df.head(500)
```


### Merge Party affiliation and labor Force data

#### Merge on State and District number

```
party_df.head(100)
merged_df = pd.merge(acs_df, party_df, on=["State", "district number"])
merged_df['Party'].value_counts()
```

### Merge Data With Region Data

```
#Load Region Data from github repository
region_df = pd.read_csv('https://raw.githubusercontent.com/cphalpert/census-regions/master/us%20census%20bureau%20regions%20and%20divisions.csv')
region_df = region_df[['State', 'Region']]
```
```
#Merge
merged_df = pd.merge(merged_df, region_df, on=["State"])
merged_df
```

## Analysis and Data Visualization

### State Level Analysis

#### Map View Analysis By Party

```
#abreviations for state names
us_state_to_abbrev = {
    "Alabama": "AL",
    "Alaska": "AK",
    "Arizona": "AZ",
    "Arkansas": "AR",
    "California": "CA",
    "Colorado": "CO",
    "Connecticut": "CT",
    "Delaware": "DE",
    "Florida": "FL",
    "Georgia": "GA",
    "Hawaii": "HI",
    "Idaho": "ID",
    "Illinois": "IL",
    "Indiana": "IN",
    "Iowa": "IA",
    "Kansas": "KS",
    "Kentucky": "KY",
    "Louisiana": "LA",
    "Maine": "ME",
    "Maryland": "MD",
    "Massachusetts": "MA",
    "Michigan": "MI",
    "Minnesota": "MN",
    "Mississippi": "MS",
    "Missouri": "MO",
    "Montana": "MT",
    "Nebraska": "NE",
    "Nevada": "NV",
    "New Hampshire": "NH",
    "New Jersey": "NJ",
    "New Mexico": "NM",
    "New York": "NY",
    "North Carolina": "NC",
    "North Dakota": "ND",
    "Ohio": "OH",
    "Oklahoma": "OK",
    "Oregon": "OR",
    "Pennsylvania": "PA",
    "Rhode Island": "RI",
    "South Carolina": "SC",
    "South Dakota": "SD",
    "Tennessee": "TN",
    "Texas": "TX",
    "Utah": "UT",
    "Vermont": "VT",
    "Virginia": "VA",
    "Washington": "WA",
    "West Virginia": "WV",
    "Wisconsin": "WI",
    "Wyoming": "WY",
    "District of Columbia": "DC",
    "American Samoa": "AS",
    "Guam": "GU",
    "Northern Mariana Islands": "MP",
    "Puerto Rico": "PR",
    "United States Minor Outlying Islands": "UM",
    "U.S. Virgin Islands": "VI",
}
```
```
# State Level data frame

df_state_1 = pd.pivot_table(merged_df, index="State", values=(['Total Households', 'Not in labor force']),aggfunc=sum)
df_state_2 = pd.pivot_table(merged_df, index="State", columns = (['Party']),aggfunc='count')
df_state_3 = pd.pivot_table(merged_df, index="State", values= (['Median Age']),aggfunc='mean')
df_state_2 = df_state_2[['Total Households', 'Not in labor force']]
df_state_2.columns = df_state_2.columns.map('_'.join)
col = df_state_2.columns
df_state_2[col] = df_state_2[col].apply(pd.to_numeric, errors='coerce')
df_state_2 = df_state_2.fillna(0)
df_state_2['More Democratic                                    Balanced                                    More Republican"'] = df_state_2['Total Households_R']/(df_state_2['Total Households_D']+df_state_2['Total Households_L']+df_state_2['Total Households_R'])
df_state_2 = df_state_2[['More Democratic                                    Balanced                                    More Republican"']]
df_state = pd.merge(df_state_1, df_state_2, on=["State"])
df_state = pd.merge(df_state, df_state_3, on=["State"])
df_state['Not in Labor Force Rate']= (100*df_state['Not in labor force']/df_state['Total Households'])
df_state.reset_index(inplace=True)

#  State Level Geo Map

df_state['State_Code'] = df_state.State.map(us_state_to_abbrev)
fig_e1 = px.choropleth(df_state, locations='State_Code', locationmode="USA-states", scope="usa", color='Not in Labor Force Rate',color_continuous_scale="viridis")
fig_e1.show()
```
```
# State Level Political sub Geo Map
df_state['State_Code'] = df_state.State.map(us_state_to_abbrev)
df_state['Party'] = np.where(df_state['More Democratic                                    Balanced                                    More Republican"']>= 0.5, 'Republican Majority', 'Democratic Majorty')
fig_e3 = px.choropleth(df_state, locations='State_Code', locationmode="USA-states", scope="usa", color='Not in Labor Force Rate',color_continuous_scale="viridis", facet_col="Party" )
fig_e3.show()
```

#### Regional Analysis
```
grouped_regions = merged_df.groupby('Region').mean()
grouped_regions
grouped_regions['% of households not in labor force'].plot(kind='bar')
plt.xlabel('Region')
plt.ylabel('% of People Not in Labor Force')
plt.title('% of People Not in Labor Force by Region')
plt.show()
```

#### Party Dominance Ratio

To better assess how the "Not in Labor Force Rate" would impact/reflect the political affiliation we categorize the US states political affiliation based on more griadient political affiliation variable with One (Solid Red) represending states with 100% of the congressional districts dominated by Republicans and and Zero being the states with 100% democrares domination.

The plot provide didn't provide support for our original hypothesis where the Red/Republican dominant states are having relatively higher values of "Not in Labor Force Rate", compared to the Blue/Democratic demonant states

```
#  Democratic districts ratio vs Not in Labor Force Rate
fig_e2, ax = plt.subplots(figsize=(15, 9))
pos = df_state.plot.scatter(x='More Democratic                                    Balanced                                    More Republican"', y='Not in Labor Force Rate', c='More Democratic                                    Balanced                                    More Republican"',s=65, cmap='bwr', ax=ax)
plt.ylabel("Not in Labor Force Rate %", fontsize=14)
plt.xlabel("More Democratic                                    Balanced                                    More Republican", fontsize=14)
plt.legend([])
ax.set_xticklabels([])
```

## District Level Analysis

### Variance Analysis

In This part we tried to understand the main factor behind the wide range of "Not in Labor Force Rate" among different states/districts, spaning from 25% up to 57% on district level
While there are many reasons for not being in Labor Force, we found that that it is highly correlated with the age group distribution, clearly the higher the "Population Median Age" on a district level the higher the "Not in Labor Force Rate"
Additionally, The Republican districts are generally having higher "Median Age" which is providing good explanation for higher "Not in Labor Force Rate(s)" among the Rebuplican states/districts as observed from the previous map view.

```
import warnings
warnings.filterwarnings('ignore')

#Filtering on district with high % of not in labor force
#merged_df = merged_df.loc[(merged_df['% of households not in labor force'] >= 43)]

merged_df['high_NIL_Percent'] = np.where(merged_df['% of households not in labor force'] >= 43, 1, 0)

merged_df['low_NIL_Percent'] = np.where(merged_df['% of households not in labor force'] < 31, 1, 0)
merged_df
df_age = merged_df

# District level variables

df_age['% Age 16-24 not in Labor Force']=(100*(df_age['Male 16 to 19']+df_age['Female 16 to 19']+df_age['Male 20 and 21']+df_age['Female 20 and 21']+df_age['Male 22 to 24']+df_age['Female 22 to 24'])/df_age['Not in labor force'])
df_age['% Age 25-34 not in Labor Force']=(100*(df_age['Male 25 to 29']+df_age['Male 30 to 34']+df_age['Female 25 to 29']+df_age['Female 30 to 34'])/df_age['Not in labor force'])
df_age['% Age 35-54 not in Labor Force']=(100*(df_age['Male 35 to 44']+df_age['Male 45 to 54']+df_age['Female 35 to 44']+df_age['Female 45 to 54'])/df_age['Not in labor force'])
df_age['% Age 55-64 not in Labor Force']=(100*(df_age['Male 55 to 59']+df_age['Male 60 and 61']+df_age['Male 62 to 64']+df_age['Female 55 to 59']+df_age['Female 60 and 61']+df_age['Female 62 to 64'])/df_age['Not in labor force'])
df_age['% Age 65+ not in Labor Force']=(100*(df_age['Male 65 to 69']+df_age['Male 70 to 74']+df_age['Male 75+']+df_age['Female 65 to 69']+df_age['Female 70 to 74']+df_age['Female 75+'])/df_age['Not in labor force'])
df_age['% Age 16-19 in School NIL'] = (100*(df_age['Enrolled in School Male Age 16 to 19 NIL']+df_age['Enrolled in School Female Age 16 to 19 NIL']))/df_age['Not in labor force']
df_age['% Female under65 not-in-school NIL'] = (100*(df_age['Female 16 to 19']-df_age['Enrolled in School Female Age 16 to 19 NIL']+df_age['Female 20 and 21']+df_age['Female 22 to 24']+df_age['Female 25 to 29']+df_age['Female 30 to 34']+df_age['Female 35 to 44']+df_age['Female 45 to 54']+df_age['Female 55 to 59']+df_age['Female 60 and 61']+df_age['Female 62 to 64']))/df_age['Not in labor force']
df_age['% Women with birth last-12month NIL'] = (100*(df_age['Birth In Past 12 Months Married NIL']+df_age['Birth In Past 12 Months Not Married NIL']))/df_age['Not in labor force']
df_age['% Housewife: Child-Under18 Husband-IL'] = (100*(df_age['Child-Under18 Husband-IL-UnEmp Wife-NIL']+df_age['Child-Under18 Husband-IL-Emp Wife-NIL']))/df_age['Not in labor force']
df_age['% Male under65 not-in-school NIL'] = (100*(df_age['Male 16 to 19']-df_age['Enrolled in School Male Age 16 to 19 NIL']+df_age['Male 20 and 21']+df_age['Male 22 to 24']+df_age['Male 25 to 29']+df_age['Male 30 to 34']+df_age['Male 35 to 44']+df_age['Male 45 to 54']+df_age['Male 55 to 59']+df_age['Male 60 and 61']+df_age['Male 62 to 64']))/df_age['Not in labor force']
merged_df['Housewife: Child-Under18 Husband-IL'] = merged_df['Child-Under18 Husband-IL-UnEmp Wife-NIL']+merged_df['Child-Under18 Husband-IL-Emp Wife-NIL']
df_age


R = df_age[df_age['Party'].str.contains('R')]
D = df_age[df_age['Party'].str.contains('D')]

fig, (ax1, ax2) = plt.subplots(nrows=1, ncols=2, figsize=(17, 9),gridspec_kw={'width_ratios': [3.5, 1]})
R.plot.scatter(x='% of households not in labor force', y=['Median Age'], c='red', alpha=0.6, s=25,label='Republican', ax=ax1)
D.plot.scatter(x='% of households not in labor force', y=['Median Age'],c='blue', alpha=0.6,s=25,label='Democrat', ax=ax1)
#calculate equation for trendline
z = np.polyfit(df_age['% of households not in labor force'], df_age['Median Age'], 1)
p = np.poly1d(z)
#add trendline to plot
ax1.plot(df_age['% of households not in labor force'], p(df_age['% of households not in labor force']), color="grey", linewidth=2, linestyle="--")
ax1.set_xlabel('% Not in Labor Force', color = 'black', fontsize='12', horizontalalignment='center')
ax1.set_ylabel('Median Age', color = 'black', fontsize='13', horizontalalignment='center')
ax1.set_title('Median Age against Not in Labor Force Rate',size=15)

data = [R['Median Age'], D['Median Age']]
bp =ax2.boxplot(data,patch_artist=True, labels=['Republican', 'Democrat'],widths=0.5)
bp['boxes'][0].set_facecolor('red')
bp['boxes'][0].set_color('red')
bp['boxes'][0].set_alpha(0.7)
bp['boxes'][1].set_facecolor('blue')
bp['boxes'][1].set_color('blue')
bp['boxes'][1].set_alpha(0.7)
ax2.set_ylabel('Median Age', size=13)
ax2.set_title('Median Age Per Party',size=15)
plt.legend()
plt.show()
```

### Congressional District Grouping

While we have many democratic and also republican districts with relatively high but also low "Not In Labor Force rates" we do believe that we should have significant demographic and behavioral differences between both parties.
To better identify those differences we will creat four distict district groups as follow
Democratic districts with high "Not in Labor Force Rate" (4th Quartile)
Republican districts with high "Not in Labor Force Rate" (4th Quartile)
Democratic districts with Low "Not in Labor Force Rate" (1st Quartile)
Republican districts with Low "Not in Labor Force Rate" (1st Quartile)

### Demographic and behavioral Analysis
After splitting our data into high and low Not in Labor Force groups we looked into the below four variables we believed would be the most differentiating ones
Disability
Retirement
Stay at home mothers
Health Insurance

```
high_NIL_Percent = merged_df[merged_df['% of households not in labor force'] >= 43]

low_NIL_Percent = merged_df[merged_df['% of households not in labor force'] < 43]

democrats_high = high_NIL_Percent[high_NIL_Percent['Party'] == 'D']
republicans_high = high_NIL_Percent[high_NIL_Percent['Party'] == 'R']
democrats_low = low_NIL_Percent[low_NIL_Percent['Party'] == 'D']
republicans_low = low_NIL_Percent[low_NIL_Percent['Party'] == 'R']
```

To try and understand what groups influence who falls into the not in labor force category we looked at the % of disabled in each group, both high NIL groups and low NIL groups.

```
fig, (ax1, ax2) = plt.subplots(nrows=1, ncols=2, figsize=(17, 9))


var = 'disabled'

a1 = democrats_high[var]/democrats_high['Not in labor force']
a2 = republicans_high[var]/republicans_high['Not in labor force']
a3= democrats_low[var]/democrats_low['Not in labor force']
a4= republicans_low[var]/republicans_low['Not in labor force']
data = [a1,a2,a3,a4]
bp1 = ax1.boxplot(data,patch_artist=True, labels=['D High-NIL','R High-NIL','D Low-NIL','R Low-NIL'],showfliers=False,widths=0.5)
bp1['boxes'][0].set_facecolor('blue')
bp1['boxes'][0].set_color('blue')
bp1['boxes'][0].set_alpha(0.7)
bp1['boxes'][1].set_facecolor('red')
bp1['boxes'][1].set_color('red')
bp1['boxes'][1].set_alpha(0.7)
bp1['boxes'][2].set_facecolor('blue')
bp1['boxes'][2].set_color('blue')
bp1['boxes'][2].set_alpha(0.7)
bp1['boxes'][3].set_facecolor('red')
bp1['boxes'][3].set_color('red')
bp1['boxes'][3].set_alpha(0.7)

# Change labels for each var
ax1.set_xlabel("Political Affiliation Given High or Low  % NIL")
ax1.set_ylabel("% Disabled relative to NIL")

var = 'Retired'

a1 = democrats_high[var]/democrats_high['Not in labor force']
a2 = republicans_high[var]/republicans_high['Not in labor force']
a3= democrats_low[var]/democrats_low['Not in labor force']
a4= republicans_low[var]/republicans_low['Not in labor force']
data = [a1,a2,a3,a4]
bp = ax2.boxplot(data,patch_artist=True, labels=['D High-NIL','R High-NIL','D Low-NIL','R Low-NIL'],showfliers=False,widths=0.5)
bp['boxes'][0].set_facecolor('blue')
bp['boxes'][0].set_color('blue')
bp['boxes'][0].set_alpha(0.7)
bp['boxes'][1].set_facecolor('red')
bp['boxes'][1].set_color('red')
bp['boxes'][1].set_alpha(0.7)
bp['boxes'][2].set_facecolor('blue')
bp['boxes'][2].set_color('blue')
bp['boxes'][2].set_alpha(0.7)
bp['boxes'][3].set_facecolor('red')
bp['boxes'][3].set_color('red')
bp['boxes'][3].set_alpha(0.7)

# Change labels for each var
ax2.set_xlabel("Political Affiliation Given High or Low  % NIL")
ax2.set_ylabel("% Retired relative to NIL")
plt.show()

fig, (ax3, ax4) = plt.subplots(nrows=1, ncols=2, figsize=(17, 9))

var = 'Housewife: Child-Under18 Husband-IL'

a1 = democrats_high[var]/democrats_high['Not in labor force']
a2 = republicans_high[var]/republicans_high['Not in labor force']
a3= democrats_low[var]/democrats_low['Not in labor force']
a4= republicans_low[var]/republicans_low['Not in labor force']
data = [a1,a2,a3,a4]
bp2 = ax3.boxplot(data,patch_artist=True, labels=['D High-NIL','R High-NIL','D Low-NIL','R Low-NIL'],showfliers=False,widths=0.5)
bp2['boxes'][0].set_facecolor('blue')
bp2['boxes'][0].set_color('blue')
bp2['boxes'][0].set_alpha(0.7)
bp2['boxes'][1].set_facecolor('red')
bp2['boxes'][1].set_color('red')
bp2['boxes'][1].set_alpha(0.7)
bp2['boxes'][2].set_facecolor('blue')
bp2['boxes'][2].set_color('blue')
bp2['boxes'][2].set_alpha(0.7)
bp2['boxes'][3].set_facecolor('red')
bp2['boxes'][3].set_color('red')
bp2['boxes'][3].set_alpha(0.7)

# Change labels for each var
ax3.set_xlabel("Political Affiliation Given High or Low  % NIL")
ax3.set_ylabel("% of Stay at home moms relative to NIL")

var = 'With Health Insurance NIL'

a1 = democrats_high[var]/democrats_high['Not in labor force']
a2 = republicans_high[var]/republicans_high['Not in labor force']
a3= democrats_low[var]/democrats_low['Not in labor force']
a4= republicans_low[var]/republicans_low['Not in labor force']
data = [a1,a2,a3,a4]
bp3 = ax4.boxplot(data,patch_artist=True, labels=['D High-NIL','R High-NIL','D Low-NIL','R Low-NIL'],showfliers=False,widths=0.5)
bp3['boxes'][0].set_facecolor('blue')
bp3['boxes'][0].set_color('blue')
bp3['boxes'][0].set_alpha(0.7)
bp3['boxes'][1].set_facecolor('red')
bp3['boxes'][1].set_color('red')
bp3['boxes'][1].set_alpha(0.7)
bp3['boxes'][2].set_facecolor('blue')
bp3['boxes'][2].set_color('blue')
bp3['boxes'][2].set_alpha(0.7)
bp3['boxes'][3].set_facecolor('red')
bp3['boxes'][3].set_color('red')
bp3['boxes'][3].set_alpha(0.7)

# Change labels for each var
ax4.set_xlabel("Political Affiliation Given High or Low  % NIL")
ax4.set_ylabel("% With Health Insurance relative to NIL")
# Show the chart
plt.show()
```

## Conclusion:

As hypothesized republicans have a higher median age, as well as a higher percent of retired people, disabled people and lower percent of health care coverage. Due to this, they tend to have a higher not in labor force participation rate than democrats, since these are the major areas that make up the not in the labor force segment. The one variable that could be analyzed further is the rate of stay at home moms, which provides inconclusive results.
