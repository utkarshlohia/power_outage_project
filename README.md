# Power Outages Research Project
This is a data science project on investigating the relationship between power outage durations and geoeconomic factors. This project is for DSC80 at UCSD.


### An Investigation on Power Outage Durations Based on Geographical and Economic Factors
**Authors:** Utkarsh Lohia and Rishaant Jain

### Introduction
The project focuses on analyzing power outage data, a critical aspect of modern infrastructure that affects communities, businesses, and the overall economy. Power outages, whether due to natural disasters, technical failures, or other causes, significantly impact daily life and can provide insight into the resilience of power systems, the effectiveness of response strategies, and the vulnerabilities of regions to specific types of outages.

The dataset at the heart of this analysis contains detailed records of power outages across various regions, including information on the duration, affected customers, the geographical area of the outage, and the underlying causes. By exploring this data, we aim to identify patterns and trends that could inform improvements in infrastructure planning, emergency response, and preventive measures.

Understanding the dynamics of power outages is crucial for several reasons:

- Emergency Response and Preparedness: Analyzing outage data helps in improving the efficiency and effectiveness of emergency response, minimizing downtime, and ensuring quick restoration of services.
- Infrastructure Improvement: Insights gained from outage data can guide investments in infrastructure upgrades to mitigate the impact of future outages.
- Public Safety and Welfare: Power outages can pose serious risks to public safety, especially during extreme weather conditions. Data analysis can help in prioritizing actions to protect vulnerable populations.
- Economic Impact: Outages can have a substantial economic cost due to lost productivity and damage to goods and equipment. Understanding outage patterns helps in developing strategies to reduce economic vulnerabilities.

This project seeks to leverage the power outage dataset to offer actionable insights, enhance our understanding of outage dynamics, and contribute to building more resilient power systems capable of withstanding and quickly recovering from disruptions. Through this analysis, we aim to highlight areas of improvement and suggest data-driven solutions to stakeholders involved in managing and mitigating the impact of power outages.

The dataset we used contains 1534 rows, each corresponding to a different power outage. The original dataset contained 57 rows, but some of them were redundant and not useful to our analysis, so we decided to drop the columns. The columns that we continued to use were `'YEAR', 'MONTH', 'U.S._STATE','NERC.REGION', 'CLIMATE.REGION', 'CLIMATE.CATEGORY', 'OUTAGE.START.DATE', 'OUTAGE.START.TIME', 'OUTAGE.RESTORATION.DATE','OUTAGE.RESTORATION.TIME', 'CAUSE.CATEGORY', 'CAUSE.CATEGORY.DETAIL', 'OUTAGE.DURATION', 'DEMAND.LOSS.MW', 'CUSTOMERS.AFFECTED', 'TOTAL.PRICE', 'TOTAL.SALES', 'TOTAL.CUSTOMERS'`. 

| Column                     | Description |
| -------------------------- | ------- |
| YEAR                       | Indicates the year when the outage event occurred    |
| MONTH                      | Indicates the month when the outage event occurred     |
| U.S._STATE                 | Represents all the states in the continental U.S.    |
| NERC.REGION                | The North American Electric Reliability Corporation (NERC) regions involved in the outage event |
| CLIMATE.REGION             | U.S. Climate regions as specified by National Centers for Environmental Information (nine climatically consistent regions in continental U.S.A.)    |
| CLIMATE.CATEGORY           | This represents the climate episodes corresponding to the years. The categories—“Warm”, “Cold” or “Normal” episodes of the climate are based on a threshold of ± 0.5 °C for the Oceanic Niño Index (ONI)     |
| OUTAGE.START.DATE          | This variable indicates the day of the year when the outage event started (as reported by the corresponding Utility in the region)    |
| OUTAGE.START.TIME          | This variable indicates the time of the day when the outage event started (as reported by the corresponding Utility in the region)    |
| OUTAGE.RESTORATION.DATE    | This variable indicates the day of the year when power was restored to all the customers (as reported by the corresponding Utility in the region)    |
| OUTAGE.RESTORATION.TIME    | This variable indicates the time of the day when power was restored to all the customers (as reported by the corresponding Utility in the region) |
| CAUSE.CATEGORY             | Categories of all the events causing the major power outages    |
| CAUSE.CATEGORY.DETAIL      | Detailed description of the event categories causing the major power outages    |
| OUTAGE.DURATION            | Duration of outage events (in minutes)    |
| DEMAND.LOSS.MW             | Amount of peak demand lost during an outage event (in Megawatt) |
| CUSTOMERS.AFFECTED         | Number of customers affected by the power outage event    |
| TOTAL.PRICE                | Average monthly electricity price in the U.S. state (cents/kilowatt-hour)     |
| TOTAL.SALES                | Total electricity consumption in the U.S. state (megawatt-hour)   |
|TOTAL.CUSTOMERS             | Annual number of total customers served in the U.S. state        |


#### Data Cleaning
Before conducting any analysis on the data, we need to clean it so that it is easier to perform tests and analyses on it.
The data contained a row of unimportant values and descriptions which we first dropped.
Since we had the columns `OUTAGE.START.DATE` and `OUTAGE.START.TIME`, we combine them into a single pd.Timestamp column called `OUTAGE.START`. We do the same for `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME`, and made a new column called `OUTAGE.RESTORATION`. We then drop the four original columns that we used, since they are now redundant.

We converted the datatypes of `'TOTAL.PRICE'`, `'TOTAL.SALES'`, `'OUTAGE.DURATION'` and `'DEMAND.LOSS.MW'` to be `float` values instead of strings.
We converted the `OUTAGE.DURATION` column from minutes to hours, to have easily interpretable results

`print(outage_df.head().to_markdown(index=False))`

|   YEAR |   MONTH | U.S._STATE   | NERC.REGION   | CLIMATE.REGION     | CLIMATE.CATEGORY   | CAUSE.CATEGORY     | CAUSE.CATEGORY.DETAIL   |   OUTAGE.DURATION |   DEMAND.LOSS.MW |   CUSTOMERS.AFFECTED |   TOTAL.PRICE |   TOTAL.SALES |   TOTAL.CUSTOMERS | OUTAGE.START        | OUTAGE.RESTORATION   |
|-------:|--------:|:-------------|:--------------|:-------------------|:-------------------|:-------------------|:------------------------|------------------:|-----------------:|---------------------:|--------------:|--------------:|------------------:|:--------------------|:---------------------|
|   2011 |       7 | Minnesota    | MRO           | East North Central | normal             | severe weather     | nan                     |        51         |              nan |                70000 |          9.28 |   6.56252e+06 |       2.5957e+06  | 2011-07-01 17:00:00 | 2011-07-03 20:00:00  |
|   2014 |       5 | Minnesota    | MRO           | East North Central | normal             | intentional attack | vandalism               |         0.0166667 |              nan |                  nan |          9.28 |   5.28423e+06 |       2.64074e+06 | 2014-05-11 18:38:00 | 2014-05-11 18:39:00  |
|   2010 |      10 | Minnesota    | MRO           | East North Central | cold               | severe weather     | heavy wind              |        50         |              nan |                70000 |          8.15 |   5.22212e+06 |       2.58690e+06 | 2010-10-26 20:00:00 | 2010-10-28 22:00:00  |
|   2012 |       6 | Minnesota    | MRO           | East North Central | normal             | severe weather     | thunderstorm            |        42.5       |              nan |                68200 |          9.19 |   5.78706e+06 |       2.60681e+06 | 2012-06-19 04:30:00 | 2012-06-20 23:00:00  |
|   2015 |       7 | Minnesota    | MRO           | East North Central | warm               | severe weather     | nan                     |        29         |              250 |               250000 |         10.43 |   5.97034e+06 |       2.67353e+06 | 2015-07-18 02:00:00 | 2015-07-19 07:00:00  |


#### Univariate Analysis

In the univariate analysis, we looked at the distribution of frequencies of power outages in warm, normal and cold weathers. 

 <iframe
  src="assets/univar_plot1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The bar chart presents a univariate analysis of power outages classified by climate category. It indicates that outages are most frequently reported in 'normal' climate conditions, followed by 'cold', with 'warm' conditions experiencing the fewest outages. This distribution suggests that climatic conditions have a significant impact on the occurrence of power outages.


In the next univariate analysis, we look at the distribution of frequencies of power outages in different NERC regions.
 <iframe
  src="assets/univar_plot2.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>


The histogram illustrates the frequency of power outages across different NERC (North American Electric Reliability Corporation) regions. The WECC (Western Electricity Coordinating Council) region experiences the highest number of outages, with a frequency of 451, while regions such as HECO (Hawaiian Electric Company), ASCC (Alaska System Coordination Council), FRCC (Florida Reliability Coordinating Council), and PR (Puerto Rico) report significantly fewer outages. This suggests a geographical variance in outage occurrences.


#### Bivariate Analysis

 <iframe
  src="assets/bivar_plot1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The bar chart displays the average outage duration associated with various cause categories. Severe weather stands out as the leading cause of prolonged power outages, while other categories like islanding, intentional attack, and equipment failure result in relatively shorter outage durations. This data highlights the critical impact of weather-related incidents on power infrastructure stability.

#### Interesting Aggregates

| U.S._STATE           | CAUSE.CATEGORY                |   CUSTOMERS.AFFECTED sum |   OUTAGE.DURATION mean |   OUTAGE.DURATION median |
|:---------------------|:------------------------------|-------------------------:|-----------------------:|-------------------------:|
| Alabama              | intentional attack            |              0           |             1.28333    |               1.28333    |
| Alabama              | severe weather                |         471644           |            23.6958     |              16.9417     |
| Alaska               | equipment failure             |          14273           |           nan          |             nan          |
| Arizona              | equipment failure             |         167000           |             2.30833    |               1.125      |
| Arizona              | intentional attack            |           2713           |            10.66       |               3          |
| Arizona              | severe weather                |         180911           |           428.775      |             446          |
| Arizona              | system operability disruption |         229000           |             6.40833    |               6.40833    |
| Arkansas             | equipment failure             |              0           |             1.75       |               1.75       |
| Arkansas             | intentional attack            |           9200           |             9.13056    |               2.91667    |
| Arkansas             | islanding                     |              0           |             0.05       |               0.05       |
| Arkansas             | public appeal                 |          54094           |            17.7286     |               5          |
| Arkansas             | severe weather                |         556466           |            45.03       |              20.225      |
| California           | equipment failure             |              1.39026e+06 |             8.74683    |               4.48333    |
| California           | fuel supply emergency         |              0           |           102.577      |              14.7083     |
| California           | intentional attack            |         127920           |            15.7743     |               1.95       |
| California           | islanding                     |         131019           |             3.58095    |               2.14167    |
| California           | public appeal                 |              0           |            33.8019     |               7          |
| California           | severe weather                |              2.05794e+07 |            48.8062     |              16.0333     |
| California           | system operability disruption |              3.34489e+06 |             6.06111    |               3.31667    |
| Colorado             | fuel supply emergency         |              0           |           nan          |             nan          |
| Colorado             | intentional attack            |              0           |             1.95       |               1.75       |
| Colorado             | islanding                     |          35230           |             0.0333333  |               0.0333333  |
| Colorado             | severe weather                |         355058           |            45.4542     |              40.4167     |
| Colorado             | system operability disruption |          61379           |             4.6625     |               5.625      |
| Connecticut          | intentional attack            |              0           |             0.81875    |               0.0166667  |
| Connecticut          | severe weather                |         784410           |            37.71       |              34          |
| Delaware             | equipment failure             |          18400           |             0.833333   |               0.833333   |
| Delaware             | intentional attack            |              0           |             0.648649   |               0.0166667  |
| Delaware             | severe weather                |          65000           |            35.8917     |              35.8917     |
| Delaware             | system operability disruption |              0           |           nan          |             nan          |
| District of Columbia | equipment failure             |          52000           |             2.65       |               2.65       |
| District of Columbia | severe weather                |              1.70038e+06 |            79.4019     |              40.4167     |
| Florida              | equipment failure             |         690101           |             9.24167    |               5.14167    |
| Florida              | intentional attack            |              0           |             0.833333   |               0.833333   |
| Florida              | public appeal                 |              0           |            72          |              72          |
| Florida              | severe weather                |              1.15676e+07 |           107.003      |              72.2583     |
| Florida              | system operability disruption |         474561           |             3.42833    |               2.025      |
| Georgia              | intentional attack            |              0           |             1.8        |               1.8        |
| Georgia              | severe weather                |              1.93088e+06 |            23.7125     |              23.6        |
| Hawaii               | severe weather                |         706886           |            16.625      |              15.9167     |
| Hawaii               | system operability disruption |          29300           |             3.95       |               3.95       |
| Idaho                | intentional attack            |              0           |             5.125      |               3          |
| Idaho                | public appeal                 |              0           |            25.8        |              25.8        |
| Idaho                | system operability disruption |          35000           |             2.99444    |               3.66667    |
| Illinois             | equipment failure             |          51500           |             2.48333    |               2.48333    |
| Illinois             | fuel supply emergency         |              0           |            46.0167     |              46.0167     |
| Illinois             | intentional attack            |              0           |            24.1667     |              24.1667     |
| Illinois             | public appeal                 |              0           |             2          |               2          |
| Illinois             | severe weather                |              9.0577e+06  |            27.5117     |              20          |
| Indiana              | equipment failure             |         124000           |             0.0166667  |               0.0166667  |
| Indiana              | fuel supply emergency         |              0           |           204          |             204          |
| Indiana              | intentional attack            |              0           |             7.03125    |               0.825      |
| Indiana              | islanding                     |          29000           |             2.08889    |               1.6        |
| Indiana              | severe weather                |              2.17505e+06 |            75.3882     |              50.3667     |
| Indiana              | system operability disruption |          36700           |            77.86       |               1.08333    |
| Iowa                 | intentional attack            |              0           |            94.2967     |              36.0167     |
| Iowa                 | severe weather                |         376000           |            55.8944     |              17.4        |
| Kansas               | intentional attack            |              0           |             9.35       |               3.11667    |
| Kansas               | public appeal                 |              0           |            15.2167     |              15.2167     |
| Kansas               | severe weather                |         756000           |           155.767      |             227.5        |
| Kentucky             | equipment failure             |              0           |            10.8667     |              10.8667     |
| Kentucky             | fuel supply emergency         |              0           |           209.5        |             209.5        |
| Kentucky             | intentional attack            |            110           |             1.8        |               1.8        |
| Kentucky             | severe weather                |              1.3052e+06  |            74.6685     |              28          |
| Louisiana            | equipment failure             |          32800           |             2.93889    |               3.78333    |
| Louisiana            | fuel supply emergency         |              0           |           469.5        |             469.5        |
| Louisiana            | public appeal                 |              0           |            22.6536     |               8.89167    |
| Louisiana            | severe weather                |              2.86326e+06 |           119.782      |              51.0583     |
| Louisiana            | system operability disruption |         124001           |            19.0778     |               9.6        |
| Maine                | fuel supply emergency         |              0           |            27.9333     |              27.9333     |
| Maine                | intentional attack            |              0           |             1.37778    |               0.00833333 |
| Maine                | islanding                     |              0           |            14.6833     |              14.6833     |
| Maine                | severe weather                |         932269           |            27.8233     |              18.5333     |
| Maryland             | intentional attack            |              2           |             3.75533    |               0.0166667  |
| Maryland             | severe weather                |              5.42407e+06 |            66.7823     |              50.8333     |
| Maryland             | system operability disruption |              0           |             5.06667    |               5.06667    |
| Massachusetts        | fuel supply emergency         |              1           |            48.1833     |              48.1833     |
| Massachusetts        | intentional attack            |          50500           |             6.40417    |               1.075      |
| Massachusetts        | severe weather                |         960000           |            25.9429     |               8          |
| Massachusetts        | system operability disruption |         159250           |             1.11667    |               1.11667    |
| Michigan             | equipment failure             |              0           |           440.589      |              12.6833     |
| Michigan             | intentional attack            |              0           |            60.5875     |              23.4667     |
| Michigan             | islanding                     |              0           |             0.0166667  |               0.0166667  |
| Michigan             | public appeal                 |              0           |            17.9667     |              17.9667     |
| Michigan             | severe weather                |              1.14798e+07 |            80.5275     |              70.9833     |
| Michigan             | system operability disruption |              2.27921e+06 |            43.5        |              44.9        |
| Minnesota            | intentional attack            |           5941           |             6.15833    |               1.3        |
| Minnesota            | severe weather                |              1.73015e+06 |            59.7591     |              50          |
| Mississippi          | intentional attack            |              0           |             0.2        |               0.0833333  |
| Mississippi          | system operability disruption |          10000           |             5          |               5          |
| Missouri             | intentional attack            |              0           |             6.8        |               0.416667   |
| Missouri             | severe weather                |         657944           |            74.7303     |              48          |
| Missouri             | system operability disruption |              0           |             1.08333    |               1.08333    |
| Montana              | intentional attack            |              0           |             1.55       |               1.55       |
| Montana              | islanding                     |              0           |             0.575      |               0.575      |
| Nebraska             | public appeal                 |              0           |             2.65       |               2.65       |
| Nebraska             | severe weather                |         261212           |            53.6889     |               1          |
| Nevada               | intentional attack            |         111100           |             9.22143    |               1.8        |
| New Hampshire        | intentional attack            |              0           |             1          |               0.883333   |
| New Hampshire        | severe weather                |         152568           |            26.625      |              26.625      |
| New Jersey           | intentional attack            |              0           |             1.51875    |               0          |
| New Jersey           | severe weather                |              4.65365e+06 |           106.214      |              85.25       |
| New Jersey           | system operability disruption |         313067           |            12.475      |              12.475      |
| New Mexico           | fuel supply emergency         |              0           |             1.26667    |               1.26667    |
| New Mexico           | intentional attack            |              0           |             2.90833    |               2.10833    |
| New Mexico           | system operability disruption |         500000           |             0          |               0          |
| New York             | equipment failure             |              0           |             4.11667    |               4.11667    |
| New York             | fuel supply emergency         |              0           |           278.121      |             264.25       |
| New York             | intentional attack            |              0           |             5.15139    |               0.291667   |
| New York             | public appeal                 |          55800           |            44.25       |              46          |
| New York             | severe weather                |              5.28162e+06 |           100.576      |              78.5        |
| New York             | system operability disruption |              3.243e+06   |            19.6095     |               3.18333    |
| North Carolina       | intentional attack            |              0           |            17.7292     |               8.06667    |
| North Carolina       | severe weather                |              3.52772e+06 |            28.9822     |              22.275      |
| North Carolina       | system operability disruption |          58772           |             1.37       |               0.75       |
| North Dakota         | fuel supply emergency         |              0           |           nan          |             nan          |
| North Dakota         | public appeal                 |          34500           |            12          |              12          |
| Ohio                 | equipment failure             |              0           |           nan          |             nan          |
| Ohio                 | intentional attack            |           2000           |             5.45476    |               0.683333   |
| Ohio                 | severe weather                |              3.69617e+06 |            72.0378     |              61          |
| Ohio                 | system operability disruption |              1.226e+06   |            29.075      |              29.075      |
| Oklahoma             | intentional attack            |              0           |             1.26111    |               1.83333    |
| Oklahoma             | islanding                     |          14500           |            16.4        |              16.4        |
| Oklahoma             | public appeal                 |              0           |            11.7333     |               7.18333    |
| Oklahoma             | severe weather                |              3.03848e+06 |            70.1078     |              48.25       |
| Oregon               | equipment failure             |              3           |             3.33333    |               3.33333    |
| Oregon               | intentional attack            |           2500           |             6.56842    |               1.21667    |
| Oregon               | severe weather                |         525000           |            38.2633     |              25.4333     |
| Pennsylvania         | equipment failure             |          43903           |             6.26667    |               6.26667    |
| Pennsylvania         | intentional attack            |          35000           |            25.4472     |              15.05       |
| Pennsylvania         | severe weather                |              8.51648e+06 |            71.9        |              50.9833     |
| Pennsylvania         | system operability disruption |              0           |             5.48333    |               5.48333    |
| South Carolina       | severe weather                |              2.01530e+06 |            52.25       |              49.125      |
| South Dakota         | islanding                     |              0           |             2          |               2          |
| Tennessee            | equipment failure             |              0           |             6.73333    |               6.73333    |
| Tennessee            | intentional attack            |            215           |             2.85       |               0.8        |
| Tennessee            | public appeal                 |              0           |            45          |              45          |
| Tennessee            | severe weather                |              1.00818e+06 |            23.1058     |              12.5        |
| Tennessee            | system operability disruption |              0           |             0.333333   |               0.333333   |
| Texas                | equipment failure             |         343530           |             6.76       |               5.3        |
| Texas                | fuel supply emergency         |              0           |           232          |             336          |
| Texas                | intentional attack            |           3314           |             4.97949    |               1.23333    |
| Texas                | public appeal                 |              0           |            19.0069     |               5.5        |
| Texas                | severe weather                |              1.60018e+07 |            64.2482     |              28.75       |
| Texas                | system operability disruption |              4.63514e+06 |            13.5133     |               5.36667    |
| Utah                 | equipment failure             |              0           |             0.25       |               0.25       |
| Utah                 | intentional attack            |           5800           |             2.37143    |               0.25       |
| Utah                 | public appeal                 |              0           |            37.9167     |              37.9167     |
| Utah                 | severe weather                |          60000           |            15.95       |              15.95       |
| Utah                 | system operability disruption |         159210           |             8.95833    |               8.95833    |
| Vermont              | intentional attack            |              0           |             0.590741   |               0.25       |
| Virginia             | equipment failure             |          37000           |           nan          |             nan          |
| Virginia             | intentional attack            |              0           |             0.0333333  |               0.0333333  |
| Virginia             | public appeal                 |              0           |            11.3917     |              11.3917     |
| Virginia             | severe weather                |              4.44359e+06 |            18.8714     |               9.95       |
| Virginia             | system operability disruption |         600000           |             4.01667    |               4.01667    |
| Washington           | equipment failure             |          93300           |            20.0667     |              20.0667     |
| Washington           | fuel supply emergency         |              0           |             0.0166667  |               0.0166667  |
| Washington           | intentional attack            |              0           |             6.19785    |               1.64167    |
| Washington           | islanding                     |              0           |             1.22222    |               0.35       |
| Washington           | public appeal                 |           8000           |             4.13333    |               4.13333    |
| Washington           | severe weather                |              4.38424e+06 |            91.2258     |              64.9333     |
| Washington           | system operability disruption |              0           |             0.416667   |               0.416667   |
| West Virginia        | intentional attack            |              0           |             0.0166667  |               0.0166667  |
| West Virginia        | severe weather                |         539383           |           155.083      |             159.6        |
| Wisconsin            | fuel supply emergency         |              0           |           566.188      |             226.067      |
| Wisconsin            | intentional attack            |              0           |             7.65       |               1.5        |
| Wisconsin            | public appeal                 |           7600           |             6.46667    |               6.46667    |
| Wisconsin            | severe weather                |         451160           |            25.4571     |              16          |
| Wyoming              | equipment failure             |              0           |             1.01667    |               1.01667    |
| Wyoming              | intentional attack            |              0           |             0.00555556 |               0          |
| Wyoming              | islanding                     |              0           |             0.533333   |               0.533333   |
| Wyoming              | severe weather                |          35500           |             1.76667    |               1.76667    |

The pivot table provides the following aggregate statistics:

Total Number of Customers Affected (sum): This column shows the total number of customers affected by outages, grouped by state and cause category. For instance, in Alabama, 'intentional attack' as a cause category affected 70,135 customers, while 'severe weather' affected 471,644 customers.

Average Outage Duration (mean): This column represents the average duration of outages, again grouped by state and cause category. In the example of Alabama, the average outage duration due to 'intentional attack' was 77 minutes, and for 'severe weather', it was approximately 1278 minutes.

Median Outage Duration (median): The median outage duration provides a sense of the 'middle' value of the outage duration data, which is less affected by extremely high or low values. For Alabama, the median duration for 'severe weather' related outages was 803 minutes, which can be different from the average if there are outliers in the data.

Starting with Alabama, we see a stark contrast in impact between 'intentional attack' and 'severe weather.' An intentional attack resulted in no customer impact and had a very short average duration (1.28 hours), while severe weather caused extensive outages affecting over 471,000 customers with a longer mean duration (around 24 hours) and a median duration suggesting that at least half of the weather-related outages were resolved in less than 17 hours.

For Alaska, the data indicates an equipment failure affecting over 14,000 customers, but the mean and median outage durations are not available (NaN), indicating missing or unreported data.

At the bottom of the dataset, Wyoming's data suggests that 'intentional attack' and 'islanding' had minimal impact on customers, with the mean outage duration being around 0.0056 and 0.53 hours, respectively, and 'islanding' not affecting any customers at all. 'Severe weather' in Wyoming, while less impactful than in Alabama, still affected 35,500 customers with an average duration of approximately 1.77 hours.

The mean and median duration values are interesting to compare: when they are close, it suggests a symmetric distribution of outage durations; when they are far apart, it suggests a skewed distribution with outliers influencing the mean. The scientific notation (e.g., 1.28e+00) is just a different way to express decimal numbers, often used to represent large or very precise numbers; for instance, 1.28e+00 is equivalent to 1.28.

### Missingness Analysis

#### NMAR Analysis
The missingness of the OUTAGE.RESTORATION column is probably due to the fact that the outage in that observation has not been restored, leading to NaN values. Thus, we can say that the values in OUTAGE.RESTORATION are Not Missing at Random (NMAR), and are dependent on the values themselves. If we want to prove this column to be Missing at Random(MAR), we could collect boolean data that indicates whether the power was restored or not.

#### MAR Analysis

We looked at the missingness of the `DEMAND.LOSS.MW` column to check if it was dependent on `NERC.REGION`. 
**Null Hypothesis:** The missingness of `DEMAND.LOSS.MW` is not dependent on `NERC.REGION`
**Alternate Hypothesis:** The missingness of `DEMAND.LOSS.MW` is does depend on `NERC.REGION`

 <iframe
  src="assets/missingness_plot1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
The bar chart explores the relationship between missing data in the DEMAND.LOSS.MW variable and NERC.REGION categories. Each NERC region shows two bars representing the proportion of missing (True) and non-missing (False) DEMAND.LOSS.MW data. The relatively uniform distribution of missing and non-missing data across different regions suggests that missingness in demand loss is likely missing completely at random (MCAR) rather than missing at random (MAR), as there's no clear pattern indicating that missingness depends on the NERC region.

We fail to reject the null hypothesis. For the columns DEMAND.LOSS.MW and NERC.REGION, we calculate a p-value of 1.0, which is greater than 0.05. Thus we can say that `DEMAND.LOSS.MW` is not dependent on the `NERC.REGION` column.

### Hypothesis Testing

**Null Hypothesis:**
There is no difference in the average outage duration between warmer and colder climates. Any observed difference is due to chance.

**Alternative Hypothesis:**
Warmer climates have a different outage duration than colder climates

**Test Statistic:** Absolute difference in means of outage durations in warm and cold climates

**Significance Level:** 0.05

**Resulting p-value:** 1.0

A p-value of 1.0 indicates that 100% of the permutation test's iterations produced a mean difference as extreme as, or more extreme than, the observed difference under the null hypothesis of no difference between the groups.

Given the higher p-value observed, the implication is this: there's insufficient statistical evidence to conclude a significant difference in the average outage durations between warm and cold climates based on the dataset. This result reinforces the interpretation that any observed differences could likely be due to chance rather than a systematic difference between the climate categories.

Hence we fail to reject the null hypothesis in favor of the alternate hypothesis.

The choices made here are suitable for a few reasons. The test statistic is a direct measure of the effect size we are interested in. The significance level is standard, allowing for a familiar interpretation of the p-value, and the high p-value suggests that the observed sample means' difference is consistent with random sampling variability under the null hypothesis. Additionally, given that the p-value is so high, we have strong evidence that there is no difference in outage durations between the climate groups, rather than a borderline result that might necessitate a closer examination or a more complex model.


### Prediction Problem

**Question:** Predicting the duration of power outages based on geographical factors (such as climate, location, and state) and economic factors (such as total sales and customers).

**Type**: Regression

**Response Variable**: Outage Duration (continuous variable measured in hours or minutes).

**Reason for Choosing Response Variable**: The duration of a power outage is a crucial factor in assessing the impact of the outage on affected areas and planning for restoration efforts.

**Chosen Metric**: Root Mean Squared Error (RMSE)

**Reason for Choosing Metric**: 
Since this is a regression problem, RMSE is a suitable metric as it measures the average error in the predictions. RMSE is chosen for its sensitivity to larger errors, which might be important if longer outages have a disproportionately higher impact.

### Baseline Model


My model is a RandomForestRegressor designed to predict the duration of power outages. It considers three features: `TOTAL.CUSTOMERS`,`CLIMATE.CATEGORY`, and `NERC.REGION`.

**Feature Breakdown:**

`TOTAL.CUSTOMERS`: Quantitative feature representing the number of customers. No encoding needed, but missing values are imputed with the mean.
`CLIMATE.CATEGORY`: Nominal categorical feature. It was one-hot encoded to transform into a format suitable for the model.
`NERC.REGION`: Nominal categorical feature. Also one-hot encoded.

**Encodings:**

One-hot encoding was performed on nominal categorical variables ('CLIMATE.CATEGORY' and 'NERC.REGION') to convert them into a binary matrix necessary for the model.
No ordinal features were specified or encoded in the model.
Simple imputation was applied to fill missing values for numerical features with the mean and for categorical features with the most frequent category.
**Performance:**
The model’s performance was evaluated using the Root Mean Squared Error (RMSE), and it achieved an RMSE of 92.6008. The RMSE is a measure of how accurately the model predicts the response, with a lower RMSE indicating a better fit to the data. This is "good" since the range of the `OUTAGE.DURATION` is very large and an RMSE of 92.6008 is acceptable.

**Conclusion:**

Considering the model's RMSE in the context of the outage duration scale is necessary to fully assess its goodness. Moreover, without a benchmark or a simpler model to compare against, it's challenging to declare the absolute goodness of the model. Generally, further steps would include comparing this RMSE to other models, considering the complexity of the RandomForest, and possibly looking at more nuanced performance metrics or different aspects of the prediction errors. It would also be beneficial to consider other relevant features that might improve the model if they are available in the data, and to use domain knowledge to determine if the RMSE value is low enough for practical purposes.

### Final Model

For the final model, additional features and preprocessing steps were chosen based on domain knowledge and a hypothesis about their potential predictive power regarding power outage durations.

**Added Features:**

`U.S._STATE`: Given that different states may have different infrastructure quality, weather patterns, and response capabilities, including the state as a feature can capture these regional differences.
`CLIMATE.REGION`: Similar to U.S. state, climate regions may also impact outage durations as certain regions could be more prone to severe weather events that can cause longer outages.

These features are good for the data and prediction task because they capture geographically relevant variability in outage durations that could be due to infrastructure, policy, and environmental factors that vary by state and climate region.

**Modeling Algorithm:**
The chosen algorithm was the RandomForestRegressor, which is a powerful ensemble learning method known for its high performance and robustness to outliers and non-linear relationships.

**Hyperparameters and Selection Method:**

**n_estimators**: The number of trees in the forest. A larger number typically increases performance but also computational time.
**max_depth**: The maximum depth of each tree. Deeper trees can capture more complex patterns but also risk overfitting.
**min_samples_split**: The minimum number of samples required to split a node. Higher values prevent creating splits that only work for a small number of instances.
A GridSearchCV was used to exhaustively search through a subset of the hyperparameter space and identify the best combination based on cross-validated negative mean squared error.

**Performance Improvement:**
The final model achieved a lower RMSE of 88.6289 compared to the baseline model's RMSE, indicating a better fit to the data. The improvement could stem from the additional state and climate region features, which allow the model to account for regional effects on outage durations that the baseline model did not consider. The fine-tuning of hyperparameters also likely contributed to the performance improvement by optimizing the model's complexity to balance bias and variance better. This combination of a more sophisticated feature set and hyperparameter optimization makes the final model more adept at capturing the underlying patterns in the data, leading to more accurate predictions.

### Fairness Analysis

##### Null Hypothesis: 
Our model is fair. Its RMSE for the first half of the year and the second half of the year are roughly the same, and any differences are due to random chance.
##### Alternative Hypothesis: 
Our model is unfair. Its RMSE for the first half of the year is lower than its RMSE for the second half of the year.

**Group X and Group Y:**

Group X (True in 'Second_half_year'): Outages occurring in the second half of the year (months 7-12).
Group Y (False in 'Second_half_year'): Outages occurring in the first half of the year (months 1-6).

**Evaluation Metric:**

Root Mean Squared Error (RMSE) is used to measure the accuracy of outage duration predictions, with a lower RMSE indicating higher prediction accuracy.

**Test Statistic:**

The test statistic is the difference in RMSE between Group X and Group Y.

**Significance Level:**

The significance level (α) is conventionally set at 0.05, meaning there is a 5% chance of rejecting the null hypothesis when it is true (Type I error).

**Resulting p-value:** 0.782

 <iframe
  src="assets/fairness_plot1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

The p-value of 0.808 suggests there is no significant difference in the RMSE — our measure of predictive accuracy for outage durations — between Group X (second half of the year) and Group Y (first half of the year). This high p-value indicates that any observed discrepancy in RMSE between the two groups can be attributed to random variation rather than a systematic disparity due to the time of year.

In terms of RMSE parity, this result implies that the model performs consistently across both time periods. Therefore, from the perspective of prediction parity — a fairness metric that examines whether a model's predictive errors are consistent across groups — the model appears fair with respect to the time of year the outages occurred. There's no evidence of bias that systematically leads to better or worse performance in predicting the duration of outages for either half of the year.

The trimodal appearance of the graph—showing three peaks or modes—suggests that there are three different subsets within the data where the differences in RMSE concentrate more frequently than chance would predict for a normal distribution. Here's why this might happen:

**Data Subgroups:** If the data contains distinct subgroups with their own variance in outage durations, this could result in multiple modes. For example, if outages due to certain causes occur more frequently in one half of the year and these causes are associated with consistently different outage durations, this could create separate peaks when we look at the overall distribution.

**Sampling Artifacts:** Sometimes, especially with smaller or more complex datasets, the process of random permutation can lead to a non-uniform distribution of test statistics by chance. This could happen if certain combinations of data points are more likely given the way data is permuted.

**Statistical Fluctuations:** In any stochastic process, there can be fluctuations that result in a non-smooth distribution. If the number of permutations is not large enough, these fluctuations can result in what appears to be multiple modes.

**Underlying Patterns:** The data generation process itself might have inherent characteristics that cause certain values of the test statistic to be more likely. These could relate to seasonal patterns not fully captured by splitting the year into two halves, or to other cyclical factors influencing outage durations.

