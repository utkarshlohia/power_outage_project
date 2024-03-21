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





