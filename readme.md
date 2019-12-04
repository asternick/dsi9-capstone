# US States Power Generation: 
Renewable Transition and Retail Rates
---
Author: Andrew Sternick

### Table of Contents 

* [Problem Statement](#Problem-Statement)
* [Executive Summary](#Executive-Summary)
* [Datasets](#Datasets)
* [Data Dictionary](#Data-Dictionary) 
* [US Solar Resource Map](#US-Solar-Resource-Map)
* [Conclusion](#Conclusion)
* [Next Steps](#Next-Steps)

### Links
* [Images](./images/)
* [Presentation](./presentation.pdf)

## Problem Statement

In a time of overt hostility to climate goals at the federal level, state regulation is critical to confronting the climate crisis in the US. Electricity generation produces 28% of overall US greenhouse gas emissions and the need to transition to zero-emissions generation is manifest. Fortunately, energy generation is regulated at the state rather than federal level. Numerous states have established mandates for zero-emission energy generation, but there is resistance to such targets in many states, due in part to a perceived high cost of low emissions. In this project I will explore state electricity generation profiles over the period of 1990 - 2018, during which wind and solar power emerged from niche technologies to mainstream, cost-effective forms of electricity generation. I will use unsupervised clustering algorithms to gain insight into how geography, state policy, and the cost of fuels influence the states' journey toward a sustainable electricity grid, and how that journey has impacted retail electricity rates over time.

## Executive Summary

Renewable generation in the US consists of hydroelectric, wind, solar, biomass, and geothermal. The Energy Information Administration provides annual reporting of how electricity consumed at the state level (including Washington, DC) is generated, as well as average electricity rates data. We use this data to visualize each state's electricity generation and pricing profile over the entire 1990 - 2018 reporting period. Unsupervised clustering algorithms are used to gain insight into how state geography and policy have influenced generation profile. 

### Data Gathering

Detailed [state generation profile data](https://www.eia.gov/electricity/data/state/) used in this project is from the Energy Information Administration. [Solar](https://www.nrel.gov/gis/solar.html) and [wind](https://www.nrel.gov/gis/wind.html) resource data is from the National Renewable Energy Laboratory.

### Data Cleaning and Preprocessing

EIA generation data is detailed with total MWh generated and numerous source categories. Rates data includes economic sectors as well as totals. For the purposes of this project, the data was condensed to relative generation by source, with each state's generation expressed as a fraction of 100%. One generation source we remove is pumped hydro, as it is a storage technology rather than pure generation, and energy storage is not considered in our modeling. 

Electricity rates are considered in total by state, which is provided by EIA, whereas residential, industrial, commericial, and transportation rates are dropped. EIA rates data is not indexed for inflation. We use Los Angeles Times Data and Graphics Department's [CPI](https://github.com/datadesk/cpi) python library to adjust rates data to 2005 dollars. 

### Clustering Algorithms

Both K-Means and DBSCAN are used to cluster the states by generation profile each year. This takes maximum advantage of the algorithms, allowing them to identify patterns over time. K-Means was instantiated for each year's data with 3 - 7 centroids, and the cluster count that produced the highest silhouette score was retained. Since each year was evaluated separately, cluster count changes, but nonetheless a pattern is [evident over time](./images/cluster-count-km.png). DBSCAN was also instantiated for each year, and 100 epsilons of 0.01 - 1.00 were evaluated. A minimum [cluster count](./images/cluster-count-dbscan.png) of 3 was required, excluding [outliers](./images/cluster-outliers-db.png). 

For the purposes of providing insight, K-Means outperformed DBSCAN. The property of forcing all data points into a cluster was useful for our data set, with only 51 data points per year. For example, K-Means identified states which used majority petroleum for generation as a cluster, even when that cluster included only one state (Hawaii) in the later years of our analysis. DBSCAN considered these states as outliers for all years. This cluster has explanatory power and is useful for analysis. DBSCAN clusters, no matter what the minimum required count, did not map to generation trends closely, while K-Means clustering did. All analysis and conclusions are based on the K-Means clusters. 



### **[Capstone, Part 4: Report Writeup + Technical Analysis](./part_04/)**

By now, you're ready to apply your modeling skills to make machine learning predictions. Your goal for Part 4 is to develop a technical document (in the form of Jupyter notebook) that can be shared among your peers.

Document your research and analysis including a summary, an explanation of your modeling approach as well as the strengths and weaknesses of any variables in the process. You should provide insight into your analysis, using best practices like cross validation or applicable prediction metrics.

- **Goal**: Detailed report and code with a summary of your statistical analysis, model, and evaluation metrics.
- **Due**: TBD

### **[Capstone, Part 5: Presentation + Recommendations](./part_05/)**

Whether during an interview or as part of a job, you will frequently have to present your findings to business partners and other interested parties - many of whom won't know anything about data science! That's why for Part 5, you'll create a presentation of your previous findings with a non-technical audience in mind.

You should already have the analytical work complete, so now it's time to clean up and clarify your findings. Come up with a detailed slide deck or interactive demo that explains your data, visualizes your model, describes your approach, articulates strengths and weaknesses, and presents specific recommendations. Be prepared to explain and defend your model to an inquisitive audience!

- **Goal**: Detailed presentation deck that relates your data, model, and findings to a non-technical audience.
- **Due**: TBD
