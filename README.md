# Predicting Power Outage Duration: An Exploratory Analysis
Portfolio Homework for EECS 398: Practical Data Science

## Step 1: Introduction 
The dataset we chose to use is the power outages dataset describing major power outage events in the continental U.S., which we retrived from [this ScienceDirect article](https://www.sciencedirect.com/science/article/pii/S2352340918307182) and [this Purdue link to the dataset](https://engineering.purdue.edu/LASCI/research-data/outages). 

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
| 'U.S._STATE'         | State event occured in                                       |
| 'POSTAL.CODE'	       | Abbreviation of state                                        |
| 'CUSTOMERS.AFFECTED' | Number of customers affected by outage event                 |
| 'ANOMALY.LEVEL'	     | Oceanic El Niño/La Niña (ONI) index -- 3 month  running mean | 
| 'OUTAGE.DURATION'    | Duration of outage in minutes                                | 

## Step 2: Data Cleaning and Exploratory Analysis
- ### Data Cleaning: 
1. We first checked all of our columns that we intended on using during the analysis for NaN values. We found that OUTAGE.DURATION annd CUSTOMERS.AFFECTED both had columns that contained NaN values.
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

- ### Univariate Analysis: 
  1. **Pie Chart -- Distribution of Outage Causes in Dataset**
  - This plot shows the distribition of outage causes within our dataset. It shows that there is a large percentage of outages caused by severe weather, almost 2/3, followed by intentional attack and system operability disruptions, making up almost 1/4 combined.
  - This helps us answer our initial question since it addresses the proportion of causes behind outages, which may affect duration. 
  
  <iframe
    src="assets/pie-chart.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>

  2. **Histogram -- Distribution of Duration**
  - This plot shows the distribition of power outage durations within our dataset. It shows a trend in our data, with most power outage events being under 1000 minutes, with longer outage durations being rarer. 
  - This helps us answer our initial question since it addresses the distribution of durations of outages and how they are spread. 
  <iframe
    src="assets/hist.html"
    width="800"
    height="600"
    frameborder="0"
  ></iframe>

- ### Bivariate Analysis: 
<iframe
  src="assets/choropleth.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/box-plot.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

- ### Interesting Aggregates: 
Embed at least one grouped table or pivot table in your website and explain its significance.

| state      | abbr   | cause             |   duration |
|:-----------|:-------|:------------------|-----------:|
| Alabama    | AL     | severe weather    |   1421.75  |
| Arizona    | AZ     | equipment failure |    726.875 |
| Arkansas   | AR     | severe weather    |   2210.85  |
| California | CA     | severe weather    |   2289.69  |
| Colorado   | CO     | severe weather    |   1178.6   |

Ths grouped table allows us to look at the most common cause of outage for each state as well as the mean duration in minutes for each state. 
- ### Imputation: 
We decided not to impute duration because we wanted to use start time and end time to fill in duration values. However, we realized that if there was NaN for duration, then at least one of the start time or end time was also NaN. We did not feel comfortable filling in with any other values since there is a lot of variety in all of our columns, so something like the mean would not accurately reflect the data.

## Step 3: Framing a Prediction Problem 
- ### Problem Identification:
Our prediction problem is: How do cause of outage, state, and number of customers affected associated with the duration of an outage? This is a regression problem because we are trying to predict the value, duration. We chose the response value, duration, because given the cause, US state, and number of customers affected, people are likely curious about how long the outage will last. We will know all of the features (state, cause, and customers affected) at the time of prediction because these values are determined before the outage is resolved. The metric that we will use to evaluate our model is mean squared error because we are trying to minimize how far our predictions are from the actual duration that the outage lasted.

## Step 4: Baseline Model 
- Our model uses four features to predict the duration of a power outage. The features are state which is nominal, cause which we are treating as nominal (we have not ranked causes in any way), anomaly level which is quantitative, and customers affected which is quantitative. We performed one hot encoding using OneHotEncoder for the categorical features. We also used PolynomialFeatures for the quantitative features.
- To measure performance, we took mean squared error for the training data and mean squared data for the validation data. For one of our train test splits, we found the averages of these values to be: 
  Training MSE: 14407167.553053152, Validation MSE: 32018249.814064648
  - We do not believe this model is good because considering that
duration is measured in minutes, these are very large discrepancies.

## Step 5: Final Model 
- The feature that we added was a StandardScaler for the numerical columns. The two numerical columns were number of customers affected and the anomaly level which have different units. Since these features have different units, standardizing improved the accuracy of predictions.
- Another feature that we added was a search for the best polynomial feature number by using GridSearchCV. Since we somewhat randomly chose 8 for the original model, it made sense for us to do a search for the best value. 

- We used Linear Regression and the most optimal hyperparemter for polynomial feature was one. This indicated to us that we should not have used it in our original model. We used GridSearchCV to select the hyperparameters.
- Our Final Model performed better for both training and validation data but more significantly for validation data. The ability to predict unseen data is important and our Final Model was much better at this. 
