# Predicting Power Outage Duration: An Exploratory Analysis

## Step 1: Introduction 
The dataset we chose to use is the power outages dataset describing major power outage events in the continental U.S., which we retrieved  from [this ScienceDirect article](https://www.sciencedirect.com/science/article/pii/S2352340918307182) and [this Purdue link to the dataset](https://engineering.purdue.edu/LASCI/research-data/outages). 

### Reasoning Behind selection:
We are not familiar with League of Legends, and power outages were more interesting to us than recipes. The example questions for recipes seem easier to intuitively predict ourselves because food and recipes are familiar, whereas we are genuinely curious about what we might find in the power outages data because we are less familiar with it. This dataset also has a high impact, since power outages affect many people

### Research Question:
How do cause of outage, state, number of customers affected, and anomaly level affect the duration of an outage? 

### Data Information:
- Size: 1534 rows x 57 columns
- Columns of Interest: 

| name                 | description                                                  |
|:---------------------|:-------------------------------------------------------------|
| 'CAUSE.CATEGORY'     | Type of cause of event                                       |
| 'U.S._STATE'         | State event occurred in                                      |
| 'POSTAL.CODE'	       | Abbreviation of state                                        |
| 'CUSTOMERS.AFFECTED' | Number of customers affected by outage event                 |
| 'ANOMALY.LEVEL'	     | Oceanic El Niño/La Niña (ONI) index -- 3 month  running mean | 
| 'OUTAGE.DURATION'    | Duration of outage in minutes                                | 

## Step 2: Data Cleaning and Exploratory Analysis
### Data Cleaning: 
1. We first checked all of our columns that we intended on using during the analysis for NaN values. We found that OUTAGE.DURATION and CUSTOMERS.AFFECTED both had columns that contained NaN values.
2. We dropped any rows containing NaN values for the two columns identified as containing those values. 
3. We remaned columns to easier to use/read names. 

  Below is the head of the cleaned data: 

  | abbr   | state     |   duration | cause          |   level |   customers |
  |:-------|:----------|-----------:|:---------------|--------:|------------:|
  | MN     | Minnesota |       3060 | severe weather |    -0.3 |       70000 |
  | MN     | Minnesota |       3000 | severe weather |    -1.5 |       70000 |
  | MN     | Minnesota |       2550 | severe weather |    -0.1 |       68200 |
  | MN     | Minnesota |       1740 | severe weather |     1.2 |      250000 |
  | MN     | Minnesota |       1860 | severe weather |    -1.4 |       60000 |

### Univariate Analysis: 
  **Pie Chart -- Distribution of Outage Causes in Dataset**
  - This plot shows the distribution  of outage causes within our dataset. It shows that there is a large percentage of outages caused by severe weather, almost 2/3, followed by intentional attack and system operability disruptions, making up almost 1/4 combined.
  - This helps us answer our initial question since it addresses the proportion of causes behind outages, which may affect duration. 
  <iframe
    src="assets/pie-chart.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
  **Histogram -- Distribution of Duration**
  - This plot shows the distribution  of power outage durations within our dataset. It shows a trend in our data, with most power outage events being under 1000 minutes, with longer outage durations being rarer. 
  - This helps us answer our initial question since it addresses how long outages usually last, and how those durations are spread. 
  <iframe
    src="assets/hist.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>

### Bivariate Analysis:
  **Choropleth -- Mean Outage Duration and Most Common Cause of Outages by State**
  - This plot shows the mean outage duration, as well as the most common cause for outages, grouped by state. This highlights which states usually experience higher duration outages, as well as the most common cause of those outages, which may impact duration. 
  - This illustrates two of the factors that will go into our prediction of duration, and how they might be linked. 
  <iframe
    src="assets/choropleth.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>
  **Box Plot -- Distribution of Duration by Cause**
  - This plot shows the distribution of outage duration grouped by cause. This highlights how different causes of outages affect the duration of the outage. For example, severe weather had many outliers, while fuel supply emergencies have a wide range of durations and a high average duration, while islanding had shorter durations. 
  - This illustrates cause of outage affects duration of outage, which is pivotal to answering our primary question of how that factor affects outage duration
  <iframe
    src="assets/box-plot.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>

### Interesting Aggregates: 
  This grouped table allows us to look at the most common cause of outage for each state as well as the mean duration in minutes for each state. 

  | state      | abbr   | cause             |   duration |
  |:-----------|:-------|:------------------|-----------:|
  | Alabama    | AL     | severe weather    |   1421.75  |
  | Arizona    | AZ     | equipment failure |    726.875 |
  | Arkansas   | AR     | severe weather    |   2210.85  |
  | California | CA     | severe weather    |   2289.69  |
  | Colorado   | CO     | severe weather    |   1178.6   |

### Imputation: 
We decided not to impute duration because we wanted to use start time and end time to fill in duration values. However, we realized that if there was NaN for duration, then at least one of the start time or end time was also NaN. We did not feel comfortable filling in with any other values since there is a lot of variety in all of our columns, so something like the mean would not accurately reflect the data.

## Step 3: Framing a Prediction Problem 
### Problem Identification:
  - Our prediction problem is: How do the cause of outage, state, anomaly level, and number of customers affected associate with the duration of an outage? 

  - This is a regression problem because we are trying to predict the value, duration. We chose the response value, duration, because given the cause, anomaly level, US state, and number of customers affected, people are likely curious about how long the outage will last. 
  
  - We will know all of the features (anomaly level, state, cause, and customers affected) at the time of prediction because these values are determined before the outage is resolved. 
  
  - The metric that we will use to evaluate our model is mean squared error because we are trying to minimize how far our predictions are from the actual duration that the outage lasted.

## Step 4: Baseline Model
- Our model uses four features to predict the duration of a power outage. The features are: 
  - state which is nominal
  - cause which we are treating as nominal (we have not ranked causes in any way)
  - anomaly level which is quantitative
  - customers affected which is quantitative. 

- We performed one hot encoding using OneHotEncoder for the categorical features. We also used PolynomialFeatures for the quantitative features.

- To measure performance, we took mean squared error for the training data and mean squared data for the validation data. For one of our train-test splits, we found the averages of these values to be:
  - Training MSE: 20755913.197023116
  - Validation MSE: 21225479.300911967

- We do not believe this model is good because when considering that
duration is measured in minutes, these are very large discrepancies.

## Step 5: Final Model 
- The feature that we added was a StandardScaler for the numerical columns. The two numerical columns were the number of customers affected and the anomaly level which have different units. Since these features have different units, standardizing improved the accuracy of predictions.

- Another feature that we added was a search for the best polynomial feature number by using GridSearchCV. Since we somewhat randomly chose 8 for the original model, it made sense for us to do a search for the best value. 

- We used Linear Regression and the most optimal hyperparemter for polynomial features was 2. We used GridSearchCV to select the hyperparameters.

- Our final model performed better for both training and validation data but more significantly for validation data. The ability to predict unseen data is important and our final model was much better at this.

- Final train-test split MSE: 
  - Training MSE: 14690109.197452102
  - Validation MSE: 11606353.005600391


