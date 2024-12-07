# Predicting Power Outage Duration: An Exploratory Analysis
Portfolio Homework for EECS 398: Practical Data Science

## Step 1: Introduction 
The dataset we chose to use is the power outages dataset describing major power outage events in the continental U.S., which we retrived from [this ScienceDirect article](https://www.sciencedirect.com/science/article/pii/S2352340918307182) and [this Purdue link to the dataset](https://engineering.purdue.edu/LASCI/research-data/outages). 

### Reasoning behind selection:
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

We 
Describe, in detail, the data cleaning steps you took and how they affected your analyses. The steps should be explained in reference to the data generating process. Show the head of your cleaned DataFrame (see Part 2: Report for instructions).

| abbr   | state     |   duration | cause          |   level |   customers |
|:-------|:----------|-----------:|:---------------|--------:|------------:|
| MN     | Minnesota |       3060 | severe weather |    -0.3 |       70000 |
| MN     | Minnesota |       3000 | severe weather |    -1.5 |       70000 |
| MN     | Minnesota |       2550 | severe weather |    -0.1 |       68200 |
| MN     | Minnesota |       1740 | severe weather |     1.2 |      250000 |
| MN     | Minnesota |       1860 | severe weather |    -1.4 |       60000 |

- ### Univariate Analysis: 
Embed at least one plotly plot you created in your notebook that displays the distribution of a single column (see Part 2: Report for instructions). Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present, and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one univariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)
<iframe
  src="assets/pie-chart.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

- ### Bivariate Analysis: 
Embed at least one plotly plot that displays the relationship between two columns. Include a 1-2 sentence explanation about your plot, making sure to describe and interpret any trends present and how they answer your initial question. (Your notebook will likely have more visualizations than your website, and that’s fine. Feel free to embed more than one bivariate visualization in your website if you’d like, but make sure that each embedded plot is accompanied by a description.)

<iframe
  src="assets/choropleth.html"
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
If you imputed any missing values, visualize the distributions of the imputed columns before and after imputation. Describe which imputation technique you chose to use and why. If you didn’t fill in any missing values, discuss why not.

## Step 3: Framing a Prediction Problem 
- ### Problem Identification:
Clearly state your prediction problem and type (classification or regression). If you are building a classifier, make sure to state whether you are performing binary classification or multiclass classification. Report the response variable (i.e. the variable you are predicting) and why you chose it, the metric you are using to evaluate your model and why you chose it over other suitable metrics (e.g. accuracy vs. F1-score).
Note: Make sure to justify what information you would know at the “time of prediction” and to only train your model using those features. For instance, if we wanted to predict your Final Exam grade, we couldn’t use your Portfolio Homework grade, because we (probably) won’t have the Portfolio Homework graded before the Final Exam! Feel free to ask questions if you’re not sure.

## Step 4: Baseline Model 
- ### Baseline Model: 
Describe your model and state the features in your model, including how many are quantitative, ordinal, and nominal, and how you performed any necessary encodings. Report the performance of your model and whether or not you believe your current model is “good” and why.

Tip: Make sure to hit all of the points above: many Portfolio Homeworks in the past have lost points for not doing so.

## Step 5: Final Model 
- ### Final Model: 
State the features you added and why they are good for the data and prediction task. Note that you can’t simply state “these features improved my accuracy”, since you’d need to choose these features and fit a model before noticing that – instead, talk about why you believe these features improved your model’s performance from the perspective of the data generating process.

Describe the modeling algorithm you chose, the hyperparameters that ended up performing the best, and the method you used to select hyperparameters and your overall model. Describe how your Final Model’s performance is an improvement over your Baseline Model’s performance.

Optional: Include a visualization that describes your model’s performance, e.g. a confusion matrix, if applicable.