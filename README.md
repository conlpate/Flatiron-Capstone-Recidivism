# Attempting a Better Criminal Justice System: Analyzing Recidivism and Prison Populations in the US

![prisonh](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/prison%20hall.jpg)

### Quick facts:
- The United States spends more than $80 billion annually to keep [roughly 2.3 million people behind bars.](https://www.themarshallproject.org/2019/12/17/the-hidden-cost-of-incarceration)
- Nearly 47% of people released from prison in 2005 were [re-arrested within 3 yeaars.](https://bjs.ojp.gov/content/pub/pdf/18upr9yfup0514.pdf)
- 450,000 are incarcerated "for nonviolent drug offenses on [any given day."](https://www.prisonpolicy.org/reports/pie2020.html)
- Those in prison who suffer from violence, an estimated 21% of men, often suffer from severe [trauma upon released.](https://www.prisonpolicy.org/blog/2017/06/22/mental_health/)

## Project Overview and Objectives

![global](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/incarc%20nato.png)

### Analyze relationships among recidivists and variable importance. 
### Use various classifiers to predict recidivism among Iowa inmates.

As the United States often is reported as having the highest incarceration rate in the world, I felt it prudent to take a look at: prison population growth and recidivism. Taking a cursory look at the statistics and sources above, it became apparent that incarceration is much more about being incarcerated. More often than not, incarceration involves an unknown amount of hidden variables, debilitating for those imprisoned and those funding it. 

With that in mind, I've analyzed prison population growth across four different states (Iowa, Pennsylvania, Louisiana, and Massachusetts), each representing different incarceration rates, and built a classification model to analyze recidivism in Iowa. 

![rank](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/percent_incarcerated.png)

## Procedure 
#### 1. Gather Data
  - [Sentencing Project, State Imprisonment Rate](https://www.sentencingproject.org/the-facts/#rankings)
    - Used for isolating and comparing different state incarceration numbers.
  - [Bureau of Justice Statistics](https://www.bjs.gov/recidivism_2005_arrest/#)
    - Used in conjunction with the Iowa Department of Corrections to analyze recidivism across the US.
  - [Iowa Department of Corrections](https://data.iowa.gov/Correctional-System/3-Year-Recidivism-for-Offenders-Released-from-Pris/mw8r-vqy4)
    - Data for classification modeling.
  - [Prison Policy Initiative](https://www.prisonpolicy.org/profiles/)
    - Supporting data. 
#### 2. Clean and Investigate Data
  - What information is being reported? 
    - Are we missing any data in demographics, conviction types, etc? 
  - How is the information grouped? 
    - Breaking out and identifying features of note. 
#### 3. EDA
  - Identify features of note. 
    - What demographics, if any, are indicative of recidivism?
    - Does release type play a role in recidivism? Conviction type?
  - Identify relationships between features.
    - What demographics, if any, are indicative of recidivism? 
#### 4. Time Series Visualizations
  - Since our primary focus is on classifying recidivism, we simply want some basic plotting of prison populations. 
  - Forecasting and modeling can be saved for larger datasets. 
![comparison](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/rec%20changing%20prison%20pop.png)

    
## Relationships

#### Demographics
![dist](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/rec%20ethnicity.png)
  - Looking at Iowa's prison population without accounting for recidivism can be somewhat misleading. 
![intro](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/rec%20sex%20and%20ethnicity.png)
  - While White Non-Hispanic Iowans make up the majority of inmates between 2010 and 2018, a closer look involving recidivism opens the door to further conversations. 
    - Note, we clearly do not have enough information on every ethnicity to comment, as seen by our error bars. 
    - However, we see a near equal distribution of recidivists across ethnicities and genders. Suggesting, based on this data, we need more information aside from plain demographics to draw relationships. 

#### Release Type
![rtype](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/rec%20with%20release%20type.png)
  - Parole seems to be one of the biggest indicators for both recidivists and non-recidivists. 
    - More information needs to be available for the type of parole, past history of probation, and type of recidivism and corresponding type of release. 
    - Based on the informataion on hand, however, we can recommend a closer look at parole over other types of release. 

#### Offense Type
![offense](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/rec%20sex%20and%20offense.png)
  - As with parole, we see a relationship between demographics and sentencing/release. 
    - Perhaps, surprisingly, violent offenses don't always relate to recidivism. As to whether this is due to parole or type of violent crime, we are unable to state definitively based on the information we have at hand. 
    - We can say different types of convicting offenses suggest higher rates of recidivism than others, especially within genders. 


## Modeling and Results

#### I constructed and tuned the following classifiers: 
  - Logistic regression
  - Decision trees
  - Random forest classifier
  - SVC / LinearSVC

#### Out of the above models, SVC (support vector machine classifier) and LinearSVC outperformed the others. 

### Top Model Scores
#### Note: While a standard SVC can be equipped with a 'linear' attribute, to eliminate performance and time waste, I separated the two for gridsearch and model analysis. In that sense, we could simply say our support vector machine classifier outperformed the other classifiers, 'linear' attribute or no. 
    - The scores are neaarly identical, but I include both to compare tuning times without subsampling the data. 
##### Overview: 
  - "Given a set of training examples, each marked as belonging to one of two categories, an SVM training algorithm builds a model that assigns new examples to one category or the other, making it a non-probabilistic binary linear classifier (although methods such as Platt scaling exist to use SVM in a probabilistic classification setting). SVM maps training examples to points in space so as to maximise the width of the gap between the two categories. New examples are then mapped into that same space and predicted to belong to a category based on which side of [the gap they fall."](https://en.wikipedia.org/wiki/Support-vector_machine)


![Scores](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/scores.png)
![cmlsvc](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/linearsvctuned.png)
#### Analysis: As seen in the notebook, our data needed oversampling to fix its imbalance issues and feature selection to eliminate any overfitting. However, our scores are still not ideal. Our recall scores stay consistent across various SVC iterations, hovering between 62-65%. 
![linearsvcroc](https://github.com/conlpate/Flatiron-Capstone-Recidivism/blob/main/images/linearsvc%20roc.png)


### Next Best Performing
