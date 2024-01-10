# How To Become A Successful Weightlifter?
Na Nguyen, Jordi Marin Urbina

# Introduction

You may have probably seen people lifting different weights in the gym. But why do people even lift? There are numerous reasons for lifting but one of the most common reasons is to become stronger, while others do lifting for a competition such as powerlifting. Powerlifting is a strength sport that has been part of the Olympics Games since 1896 and consists of three attempts to achieve the maximum weight on each of the three lifts: squat, bench press, and deadlift. During these competitions, some participants lift heavier weights than others. So we wonder, what accounts for this difference in a competitor’s performance? Could it be merely because of biological factors? Or are there any other factors that might impact a person's ability to lift a certain weight such as their age or the equipment that they use? 

## Research Questions

This research will explore the aspects that enable people to lift heavier weight. In particular, we aim to answer the following research question: What aspects of a lifter makes them the most successful in lifting weights? Does the equipment used affect the weight lifted by lifters? Our initial prediction is that a person's physiology is one of the determining factors in lifting weights. Hence, we will look at how body weight and sex assigned at birth affect the total weight lifted by an individual. Furthermore, we are keen to find out the role with which different equipment plays in assisting a weightlifter performance.

# Data

## Context

  The original Powerlifting dataset contains 1,423,354 cases, each indicating a single powerlifter. This dataset is a snapshot of the OpenPowerlifting database as of April 2019. Moreover, this dataset pertains to the results of powerlifting competition in three barbell lifts: Squat, Bench, and Deadlift. Additionally, the dataset incorporates relevant variables corresponding to the competitor’s personal information such as their name, age, body weight in kilograms, the equipment they used during the competition, as well as their final placement in competition. 
  
  Our research aims at exploring which variables might have a higher effect on the total weight lifted by a participant. Thus, we will use total kilograms lifted by a participant, which is the sum of the best three attempts for each participant in the three barbell lifts mentioned above. Furthermore, we will use one quantitative variable as our explanatory variable, the body weight in kilograms. Additionally, we will also explore two categorical variables, the participants’ sex assigned a birth (Male and Female), and we will also consider the equipment they used, which is a categorical variable that tells us what they used during competitions which include wraps, raw, single-ply, and multi-ply. To gain a better understanding of the equipment, it is worth explaining what each category level entails. According to the website Barbend, raw powerlifting means lifting with little to no equipment. On the other hand, single-ply means the suit has one layer of fabric whereas multi-ply means two layers or more. Also, wraps aim to support a lifter’s joints (i.e knees).
  
  It should be emphasized that this global dataset contains the recorded competition from September 4, 1964 to April 19, 2019. As mentioned before, this is a compilation of data performed by OpenPowerlifting, whose main objective is to create a public domain archive of powerlifting history. Lastly, the data can be accessed on the Kaggle website under the name of [“Powerlifting Database”](https://www.kaggle.com/datasets/open-powerlifting/powerlifting-database).


## Cleaning

  Since we are analyzing the total weights lifted for all three competitions - squats, bench presses, and deadlifts - we removed those lifters who were unable to fully finish all their tasks by filtering out those with missing data, since they would have skewed the results. At the same time, the original dataset contained 5 different categories for the ‘Equipment’ variable: straps, raw, single-ply, multi-ply, and wrapped. However, the ‘straps’ category was largely insufficient, and we removed it since we did not feel as if there was a large enough sample size to make accurate conclusions. We also decided to only include those competitors who were confirmed to have been tested for drugs, so as to ensure the most accurate results. 
  
  Lastly, the original dataset contained many problems regarding age that were concerning. There were many outliers for `Age` and `TotalKg`, that had described many young children lifting unusual weights, but also winning awards. At the same time, there were many data observations that had contrary values for `Age` and `AgeClass`, that were mostly found in the younger ages. We believe, then, that this was simply a mistake, where typos and similar errors created inconsistencies. We also found similar errors in the older ages of the data set. As such, in order to make the data more accurate and remove outliers, we decided to filter out all data observations that were outside of the age ranges between 16 and 50 years old. After filtering out all the data that we were originally working with, only roughly 300,000 of the original roughly 1.5 million data observations were left to be analyzed. While this was worrisome because we thought we removed too many, we later came to the conclusion that this sample size makes more sense, as it removes extreme age differences that created outliers, the errors that were occurring.
  
# Linear Regression

## Exploratory Data Analysis 

![image](https://github.com/gnguyen87/weightlift_success/assets/134335069/beee2d59-7d54-4171-84b8-a6add5b5cbb5)


For our initial visualization, we decided to look at how the weight of the participants is correlated to the total weight that they were able to successfully lift considering the equipment that they used in order to complete the lift. In this visualization, we can see that there is indeed a trend for all equipment types that the more a person weighs, the more they can seemingly lift. This trend is most obvious to those lifters who used multi-ply equipment when looking solely at the scatterplot. However, while the other equipment types seem to have weaker relations, this trend is still clearly present and seems to have very similar slopes for all equipment types, but different intercepts. At the same time, we faceted on the basis of biological sex. We found that there is also a linear relationship between the weights of the lifters and weights that they were able to successfully lift. In the facets for ‘Sex’, we found seemingly contrary results than we did for equipment types; males and females appear to have different slopes for the linear models, but have more similar y-intercepts. 

# Model Creation 

  We are interested in discovering the character and strength of the association between different factors and the total weight they can lift for their best attempts at Squat, Dead-lift, and Bench combined. Therefore, we decided to use a linear regression model. For our big model, we want to observe the correlation between biological factors, specifically a person's body weight as well as their biological sex, and the total amount of the total amount of kilograms lifted in the three categories. 
  

$$[TotalKg | Bodyweight,SexM] =  \beta_0 + \beta_1 * Bodyweight +\beta_2 *SexM$$

 For our larger model, we want to answer the other side of our research: Is it only biological factors that have an influence on a weight lifter's performance? By introducing a non-biological variable that we predict may affect the amount of weight lifted in 3 categories: Equipment, we are attempting to see if there is a statistical significance that Equipment plays in our response variable: TotalKg. Hence, one of our larger models is as follows:  

$$[TotalKg | Bodyweight,Sex,Equipment] =  \beta_0 + \beta_1 * Bodyweight +\beta_2 *SexM + \\ \beta_3*Equipment_{raw}+\beta_4*Equipment_{single-ply}+\beta_5*Equipment_{wraps} $$

 Furthermore, in our Exploratory Data Analysis, we observe a difference between the slope for female and male weightlifter across all equipment. More specifically, the slope for female is considerably steeper than that for male within each equipment category. This suggests that the value of the change in total weight lifted for each kilogram of body weight when it comes to female weightlifters is lower than when it comes to male weightlifters for each type of equipment we are considering. Therefore, we suspect that there is an effect modification in which $Sex$ can modify the effect of $Equipment$ on the outcome variable $TotalKg$, or the total weight in kilograms lifted in 3 categories for a weightlifter. Therefore, the second larger model we consider is as follows:  

$$[TotalKg | Bodyweight,Sex,Equipment] =  \beta_0 + \beta_1 * Bodyweight +\beta_2 *SexM + \\ \beta_3*Equipment_{raw}+\beta_4*Equipment_{single-ply}+\beta_5*Equipment_{wraps} 
\\ \beta_6*Equipment_{raw}*SexM+\beta_7*Equipment_{single-ply}*SexM+
\\\beta_8*Equipment_{wraps}*SexM $$

# Fitted Model 
<img width="904" alt="Screenshot 2024-01-10 at 3 39 42 PM" src="https://github.com/gnguyen87/weightlift_success/assets/134335069/16b3d7fb-fbb1-496d-8f10-5ce4c29c6659">

<img width="895" alt="Screenshot 2024-01-10 at 3 40 09 PM" src="https://github.com/gnguyen87/weightlift_success/assets/134335069/b9cf390f-1cd4-47ef-bf8b-80496416098a">

# Model Interpretation

## Estimates 

  As previously stated, our research intends to explore not only physiological factors of a competitor, but also other external factors that go into the competition. Based on our exploratory data analysis, we suspect that the model in which there is an interaction term between `Sex` and `Equipment`. Therefore, we will focus mainly on the fitted model 3. Through an examination of the slope coefficients for the variable `SexMale`, we can observe that the estimated average total kilograms lifted by a competitor whose sex assigned at birth is male is 193.61 kg higher than a lifter whose sex assigned at birth is female, holding body weight and equipment constant. Additionally, while interpreting the coefficient of the variable `BodyweightKg` we can observe that the estimated increase in the average total kilograms lifted by a competitor is 3.44 kg for a 1 unit increase in a lifter’s body weight, holding equipment and sex constant. 
  
  By examining the slope coefficients for the variable `Equipment`, we can see that the estimate suggests that lifters who do not wear any equipment, considered as “raw” under this category, lift, on average, 77.12 kg less than lifters that use multi-ply equipment (our reference category), holding all other variables constant. Likewise, the slope coefficient of equipment single-py suggests that lifters who use single-ply equipment can lift, on average, 2.71 kg more than lifters who use multi-ply equipment, holding other variables constant. Lastly, the slope coefficient of equipment wraps indicates that lifters who use wraps lift, on average, 84.25 kg less than  lifters who use multi-ply equipment.
  
  Furthermore, when we take a closer look at the values of slope coefficients for the interaction terms, we could see the degree to which `Sex` modifies the effect of `Equipment` has on `TotalKg`. For example, 25.15 is the difference in the estimated “effect” between a weightlifter using multi-ply equipment and a weightlifter using no equipment (`EquipmentRaw`), in which the effect is the 1 unit  change in the `Sex` variable—from female to male. In other words, while a female weightlifter with no equipment is expected to lift, on average, 77.12kg less than when they use multi-ply equipment, a male weightlifter with no equipment is expected to lift, on average, (77.12+25.15) or 102.27kg less than a female using multi-ply equipment. 
	
  In summary, all fitted models suggest that there seems to be a positive relationship between our predictor variables, body weight and sex assigned at birth and our outcome variable, total kilograms lifted. These results are consistent with our initial predictions when considering which variables to use, especially sex assigned at birth and body weight. Furthermore, from the model we can see the average differences in total kilograms lifted while comparing each equipment category to the multi-ply equipment as our reference category. Certainly, lifters who used raw and wraps as equipment seem to have a higher average difference in the total kilograms lifted as compared to those  who used multi-ply as equipment. Hence, we can glimpse from the model that there seems to be some sort of effect in our outcome variable across different equipment used. Likewise, with regards to fitted model 3, we observe that sex assigned at birth, to a certain degree, affects how the use of equipment influences the total amount lifted (in kilograms).
  
## Confidence Intervals

  As part of the interpretation process, we decided to look at our 95 % confidence intervals to better assess the statistical significance of our predictor variables and our outcome variable. For each additional 1 kg increase in body weight, we would expect an average increase of 3.42 to 3.46 kilograms in weight lifted. We are unsure whether this interval contains the true population average change in kilograms lifted per 1 kg increase in body weight, but we trust that 95% of samples will have intervals that contain the true change. 
  
  For a lifter who does not wear any equipment, we expect an average decrease of 65.69 to 88.56 kilograms in weight lifted. We expect that for competitors that use single-ply equipment, there would be an average decrease of 0 to 8.74.kilograms in weight lifted and an average increase of weight lifted from 0 to 14.15. Given that 0 is part of our confidence interval, it suggests that there might not be a relationship between lifters that use single-ply equipment and the total kilograms lifted. Lastly for lifters that use wraps, we expect an average decrease of 72.46 to 96.05 kilograms in weight lifted. For male lifters that do not use any equipment, we expect on average that the effect modification of sex on equipment is between -37.54 to -12.76. This means that we would expect male lifters that do not use any equipment to lift 12.76 to 77.54 kg lower than females that do not use any equipment. We expect competitors that are males and use single-ply to lift on average 9.06 to 33.85 kilograms lower than females that use single-ply. Lastly, we expect male participants that use wraps to lift on average 2.58 to 28.27 kilograms lower than females lifters that use wraps. 
  
  For all of these observations, we are confident in the interval construction process because we expect 95% of samples to lead to intervals that contain the true population parameter value. However, we are not sure if our particular interval from our sample contains that true population parameter value or not.

## P-value and Test Statistic(s)

  We also looked at the test statistic of this model to know how far the observed data is from the null hypothesis. The test statistic is a discrepancy measure where large values indicate higher discrepancy with the null hypothesis. The test statistic is the distance from the null hypothesis value of 0 (the null value) in terms of standard errors. Our model has a test statistic of 368.98, 30.72, -13.22, 0.46, -14.00, -3.97, -3.39, -2.35 for respectively `Bodyweight`, `SexMale`, `EquipmentRaw`, `EquipmentSingleply`, `EquipmentWraps`, `SexM:EquipmentRaw`, `SexM:EquipmentSingle-Ply`, and `SexM:EquipmentWraps` coefficients. Besides the coefficient `EquipmentSingleply`, all the other coefficients have a large test statistic which suggests that our data (and our estimate) is discrepant with what the null hypothesis proposes because our estimate is quite far away from the null value in standard error units. However, for our `EquipmentSingle-Ply` coefficient, we have a test statistic of -0.46 which suggests that there might not be any relationship between `EquipmentSingle-Ply` and the average difference in total kilograms lifted by a competitor that uses multi-ply equipment. 
  
  Moreover, we used the p-value to delve deeper into the statistically significant relationship between each of our explanatory variables and our outcome variable. The p-value for our coefficients measures the probability of observing a test statistic as or more extreme than the one we saw, if our null hypothesis holds. To illustrate, we could perform a hypothesis test for each slope coefficient associated with each predictor variable, in relation to the outcome variable, yet we decided to run only two hypothesis tests to make this process less repetitive.  
  
  Firstly, we wanted to explore the first part of our research questions, which relates to the physiological variables that might have an effect on the ability to lift higher weight. One of the variables in our model is `Bodyweight`, which corresponds to a lifter’s body weight in kilograms. Therefore, we want to test whether body weight has a statistically relationship with the total weight lifted by a competitor. We started by stating our hypotheses as follow:
$$H_0: \beta_1 = 0 \text{ vs } H_A: \beta_1 \neq 0,$$

> The null hypothesis for the `Bodyweight` variable is that its slope coefficient is equal to zero, suggesting that there is no relationship between a lifter’s body weight and the total weight lifted (in kilograms), after accounting for the other variables in our model.

> The alternative hypothesis for the `Bodyweight` variable is that its slope coefficient is not equal to zero, suggesting that there is a relationship between a lifter’s body weight and the total weight lifted (in kilograms), after accounting for the other variables in our model. 

  By setting a significance threshold of 0.05, we can see that the p-value associated with the variable `Bodyweight` is equal to zero, which is less than our threshold value of 0.05. Therefore, we can reject our null hypothesis in favor of our alternative hypothesis given that we have enough evidence to suggest that there is a statistically significant relationship between the variable `Bodyweight` and the outcome variable, after controlling for the other variables.  
  
  Moreover, we wanted to investigate the second part of our research questions which pertains to external factors that might have an effect on the total weight lifted, rather than focusing merely on physiological factors. In particular, one of the variables is the Single-Ply Equipment. Therefore, we can test whether there is statistically significant relationship between the predictor variable corresponding to the Single-Ply Equipment and the outcome variable total weight lifted. The hypothesis test is as follow:
  
$$H_0: \beta_4 = 0 \text{ vs } H_A: \beta_4 \neq 0,$$
 
> The null hypothesis for the `EquipmentSingle-ply` variable is that its slope coefficient is equal to zero, suggesting that there is no statistically difference in total weight lifted between lifters who use single-ply equipment with those who use multi-ply equipment, after accounting for the other variables in our model.

> The alternative hypothesis for the `EquipmentSingle-ply` variable is that its slope coefficient is not equal to zero, suggesting that there is a statistically difference in the total weight lifted between lifters who use single-ply equipment with those who use multi-ply equipment, after accounting for the other variables in our model.

  By setting a significance threshold of 0.05, we can see that the p-value associated with the variable `EquipmentSingle-ply` is equal to 0.64, which is greater than our threshold value of 0.05. Therefore, we fail to reject the null hypothesis since we do not have enough evidence to conclude that there is a statistically significant difference in total weight lifted between lifters who use single-ply equipment with those who use multi-ply equipment, after controlling for the other variables. 
  
  Lastly, by performing a hypothesis test for the remaining slope coefficients in our linear regression model, and setting the null hypothesis to be that each slope coefficient is equal to zero or that there is no relationship between the individual predictor variable associated with that coefficient and the total weight lifted, holding all other variables constant. Additionally, the alternative hypothesis is that each slope coefficient is not equal to zero, or that there is a relationship between the individual predictor variable and the total weight lifted, after accounting for the other variables. By setting a threshold of 0.05, we can finalize our conclusions. Interestingly, the rest of the p-values are zero or close to zero Therefore, we can reject the null hypothesis for each slope coefficient in favor of the alternative hypothesis since we have enough evidence that suggests that each predictor variable associated with that coefficient has a statistically significant relationship with the outcome variable, when accounting for the other variables in our model. 


# Model Evaluation 

In order to finalize on a model that best answers our research question as well as reflects our population, we will use these following linear regression tools as the basis of performance criterion to select our final model: exploratory visualization (elaborated above), $R^2$, residual standard error $s_e$, and nested test. 

## R-squared 

<img width="537" alt="Screenshot 2024-01-10 at 3 40 47 PM" src="https://github.com/gnguyen87/weightlift_success/assets/134335069/33ba6a67-ee09-44ce-b602-8ffae722dd3c">

 For our model 1, we found that for those observations without equipment, there was a R-squared of 0.597. This means that the regression model explains approximately 59.7% of the variability in the data. In other words, the model 1 is able to account for 59.7% of the variation in the data, while the remaining 40.3% of the variation is unexplained by the model. While this R-squared is not ideal, it does show that there is a large percent of data that is explained. However, for the model 2 with equipment, there was an R-squared of 0.654. As such, the model can explain 65.4% of the variance, leaving 34.6% of the variance unaccounted for. Still, this is not as high of the R-squared that we were expecting. However, there is still a large amount of variability that is explained by the model 2. This becomes more interesting, especially, when looking at model 3 in which we included the interaction variable as well. For this model, the R-squared stayed almost exactly the same, but it increased by 0.0001. As such, for both models 2 and 3, we are able to explain around 67% of the variation for both. When looking at the R-squared for all three models, it is clear that the latter two models that account for the equipment used by the lifters performs better. 
  
  We also looked into the residual standard error of all three models. For our model 2, the residual standard error is 100.91, which represents the estimate of the standard deviation of the error term in the regression model. This is simply an estimate of the average distance that the data points fall from the regression line. This residual standard error value shows that on average, the actual values of the response variable `TotalKg` are expected to deviate from the predicted values by 100-200 kg. For our model 3 that accounts for the interaction term, the residual standard error is also 100.91 compared to that of model 1 (which is 108.92). As such, we can conclude the same as for the R-squared tests, where model 2 and 3, that include the equipment type, have more accuracy--this time is for when it comes to the size of our prediction errors.
  

## Nested F-Test

By using a hypothesis nested F test, we directly compare our 3 models:

$$[TotalKg | Bodyweight,SexM] =  \beta_0 + \beta_1 * Bodyweight +\beta_2 *SexM$$

$$[TotalKg | Bodyweight,SexM,Equipment] =  \beta_0 + \beta_1 * Bodyweight +\beta_2 *SexM + \\ \beta_3*Equipment_{raw}+\beta_4*Equipment_{single-ply}+ \beta_5*Equipment_{wraps} $$

$$[TotalKg | Bodyweight,Sex,Equipment] =  \beta_0 + \beta_1 * Bodyweight +\beta_2 *SexM + \\ \beta_3*Equipment_{raw}+\beta_4*Equipment_{single-ply}+\beta_5*Equipment_{wraps} 
\\ \beta_6*Equipment_{raw}*SexM+\beta_4*Equipment_{single-ply}*SexM+
\\\beta_5*Equipment_{wraps}*SexM $$

For our first F test between model 1 and model 2:

> Null Hypothesis $H_0 : \beta_{Equipment} = 0$: Our smaller nested model with just $Bodyweight$ and $Sex$ is correct; $Equipment$ does not have an impact on $TotalKg$ after accounting for $Bodyweight$ and $Sex$.

> Alternative Hypothesis $H_A: \beta_{Equipment} \neq 0$: Our smaller model is not correct and our removed variable ($Equipment$) has a non-zero slope in the population and, therefore, has an impact on a weightlifter's performance. 
<br>


For our second F test between model 2 and model 3:

> Null Hypothesis $H_0 : \beta_{Equipment} = 0$: Our model without an interaction term is correct; $Sex$ does not have an impact on how $Equipment$ affects $TotalKg$.

> Alternative Hypothesis $H_A: \beta_{Equipment} \neq 0$: Our model without an interaction term is not correct and our removed interaction term between $Sex$ and $Equipment$ has a non-zero slope in the population and, therefore, has an impact on a weightlifter's performance. 

Below are the results from the 2 F-tests we carried out:

```{r,eval=FALSE,message=FALSE,warning=FALSE, echo = FALSE}
anova(mod1,mod2)
anova(mod2,mod3)
```

> Test statistics: 17.46

For an F Test in a linear regression model, the test statistic is a ratio comparing the sum of squared residuals. This means that $H_0$ is true if $F$ ~ 1. Our test statistic when we are comparing between model 1 and model 2 is 17064. When we are comparing between model 2 and model 3, the test statistics is 17.457. Both are rather far from 1 and suggest that model 3 is the most preferable out of the 3 models, but there are no rules for “how far is far”. 

> P-value: 2.5*10^-11 =  0.000000000025

For a threshold of $\alpha=0.05$, our p-value for the both F-tests is very much below the threshold (almost close to zero), suggesting that it is very unlikely to have observed a big difference between the two models if the smaller nested model were true. Therefore, we reject both the $H_0$'s that the model with only `Sex` and `Bodyweight` and the model without an interaction term between `Sex` and `Bodyweight` is correct. We are, on the other hand, in favor of model 3 as it fits our data statistically better. 

## Residuals versus Fitted Values Plot

![image](https://github.com/gnguyen87/weightlift_success/assets/134335069/b11c89ee-d177-485b-9cda-7cbca0f37870)

 By examining Figure 2, we can observe the Residuals vs Fitted values of our final model. From the plot we can assess how good our final model can fit our data. The horizontal line at 0 corresponds to the line of perfect fit, in which the predicted values and actual values from the data are equal. Overall, this plot suggests that our final model is a good fit for the data, as the smooth line is, in its majority, close to the line of perfect fit. This indicates that the model is accurately predicting most of the data points. Yet, it is important to note that our model overpredicts the large fitted values (shown by the negative residuals), yet we can observe that these are few data points compared to our large dataset.
  
# Conclusion

## General Takeaways

  From the results of our model interpretation and evaluation, we can conclude that model 3 performs more preferably than all of our models. Through model 3, it is revealed that although biological factors play an important role in a lifter's performance, there are other factors such as the equipment that they use that impact their performance. There is a positive relationship between the body weight of a lifter and the total kilograms that they can lift. Furthermore, the sex assigned at birth plays a critical role on how much weight a competitor can lift. Our model found that on average, the estimated increase in the total kg associated with a 1 unit increase in a lifter’s body weight is 3.44 kg for a lifter, holding all other variables constant. Holding body weight and equipment constant, the estimated average total kilograms lifted by a lifter of sex male is 193.61 kg higher than a lifter whose sex assigned at birth is female. 
  
  In addition, equipment also plays a critical role in a lifter's ability to lift heavier weights. According to our model, female lifters that use no equipment are expected to lift on average 77.12 kg less than female lifters that use multi-ply equipment, holding all other variables constant. Likewise, the slope coefficient of equipment wraps indicates that female lifters who use wraps lift, on average, 84.25 kg less than female lifters who use multi-ply equipment. Lastly, female lifters who use single-ply equipment can lift, on average, 2.71 kg more than female lifters who use multi-ply equipment, holding other variables constant. 
  
  We also observed from our model that sex has a modification effect on the equipment used. Male Individuals who do not use any equipment competitors are expected to lift on average 25.15 kg less than female lifters that do not use any equipment compared to competitors (109.4 kg less) that use multi-ply equipment. From these observations, we think that biological factors such as the sex assigned at birth of a person, their body weight and other factors such as the equipment that a lifter uses determines a lifter’s performance. 

## Limitations

  As we explained in the data cleaning process, we filtered a significant amount of our cases as there was missing data, or some inconsistencies, which might have arisen during the data compilation process. Although we were left with a significant amount of data, we are uncertain about if all these values accurately represent the actual values or information for each participant. Furthermore, this dataset did not contain other variables that could have been relevant to our research questions, such as the time each participant has been lifting (i.e. years of experience, or the number of competitions), which might have provided further insight into some of the relationships with our outcome variable. We also understand that there were many inconsistencies in each data observation. This comes from the fact that there was not one standardized method of collecting data, as it was up to each competition to record. We have tried to mitigate these effects as much as possible by cleaning the data, but recognize that the data is not perfect. 

  In addition, this dataset is useful in different domains as it helps to analyze the patterns and trends of powerlifting competitions, and inform the general public. Nevertheless, there are some ethical concerns, in relation to privacy, as it provides the name of each competitor, as well their personal information that might put them in the loop of public scrutiny. Moreover, we are hesitant if the disclosure of this information was consensual. On another note, we acknowledge that our study might reinforce some biases and stereotypes if these results are not well explained and contextualized to the public since our study focuses merely on a lifter’s performance based on the total kilograms they can lift. We also recognize that the R-squared of our models could be considerably higher. While they were somewhat sufficient in explaining variation, there is a relatively large amount of variation that is left unexplained. Future research could build upon this, trying to raise this R-squared test value, and also considering the removal of extreme outliers. To conclude, it would be interesting for further study to explore the statistical relationship of other predictor variables such a lifter’s nutrition, mental factors, and injury history with the total outcome of weight lifted in kilograms, yet these variables are not included in our dataset.


# Appendix

## Works Cited 

Heggeseth, Brianna, Myint, Leslie, and Grinde, Kelsey. STAT 155 Notes. January 15, 2021.
https://bcheggeseth.github.io/Stat155Notes/

Siem, Brooke. “Raw vs Equipped Powerlifting.” BarBend, July 10, 2017. https://barbend.com/raw-vs-equipped-powerlifting/


