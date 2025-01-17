## Data Analysis (27 questions)

#### 1. (Given a Dataset) Analyze this dataset and tell me what you can learn from it.
  - Typical data cleaning and visualization

#### 2. What is R2? What are some other metrics that could be better than R2 and why?
  - goodness of fit measure. variance explained by the regression / total variance
  - the more predictors you add, the higher R^2 becomes. Potential overfitting issue.
    - hence use adjusted R^2 which adjusts for the degrees of freedom 
    - or train error metrics

#### 3. What is the curse of dimensionality?
  - High dimensionality makes clustering hard, because having lots of dimensions means that everything is "far away" from each other.
  - For example, to cover a fraction of the volume of the data we need to capture a very wide range for each variable as the number of variables increases
  - All samples are close to the edge of the sample. And this is a bad news because prediction is much more difficult near the edges of the training sample.
  - The sampling density decreases exponentially as p increases and hence the data becomes much more sparse without significantly more data. 
  - We should conduct PCA to reduce dimensionality
  - *Distance Concentration: all the pairwise distances between different points n the space converging to the same value as the dimensionality of the data increases. Machine learning models such as clustering or nearest neighbours' methods use distance-based metrics to identify similar of the samples.*
  - *Data sparsity: training samples to not capture all combinations. Training a model with sparse data could lead to high-variance or overfitting condition.*

#### 4. Is more data always better?
  - Statistically
    - It depends on the quality of your data, for example, if your data is biased, just getting more data won’t help.
    - It depends on your model. If your model suffers from high bias, getting more data won’t improve your test results beyond a point. You’d need to add more features, etc.
  - Practically
    - More data usually benefit the models
    - Also there’s a tradeoff between having more data and the additional storage, computational power, memory it requires. Hence, always think about the cost of having more data.

#### 5. What are advantages of plotting your data before performing analysis?
  - Data sets have errors. You won't find them all but you might find some. That 212 year old man. That 9 foot tall woman.  
  - Variables can have skewness, outliers, etc. Then the arithmetic mean might not be useful, which means the standard deviation isn't useful.
  - Variables can be multimodal! If a variable is multimodal then anything based on its mean or median is going to be suspect. 

#### 6. How can you make sure that you don’t analyze something that ends up meaningless?
  - Proper exploratory data analysis.  
  - In every data analysis task, there's the exploratory phase where you're just graphing things, testing things on small sets of the data, summarizing simple statistics, and getting rough ideas of what hypotheses you might want to pursue further.  
  - Then there's the exploratory phase, where you look deeply into a set of hypotheses.   
  - The exploratory phase will generate lots of possible hypotheses, and the exploratory phase will let you really understand a few of them. Balance the two and you'll prevent yourself from wasting time on many things that end up meaningless, although not all.

#### 7. What is the role of trial and error in data analysis? What is the the role of making a hypothesis before diving in?
  - data analysis is a repetition of setting up a new hypothesis and trying to refute the null hypothesis.
  - The scientific method is eminently inductive: we elaborate a hypothesis, test it and refute it or not. As a result, we come up with new hypotheses which are in turn tested and so on. This is an iterative process, as science always is.

#### 8. How can you determine which features are the most important in your model?
  - Linear regression can use p-value
    - *The p-value for each independent variable tests the H0 that the variable has no correlation with the dependent variable. If the p-value for a variable is less than your significance level, your sample data provide enough evidence to reject the H0 for the entire population.*
  - run the features though a Gradient Boosting Machine or Random Forest to generate plots of relative importance and information gain for each feature in the ensembles.
  - Look at the variables added in forward variable selection
    - *forward stepwise regression: begins with an empty model and adds in variables one by one. Once the model no longer improves with adding more variables, the process stops.*

#### 9. How do you deal with some of your predictors being missing?
  - Remove rows with missing values - This works well if
    - the values are missing randomly (see [Vinay Prabhu's answer](https://www.quora.com/How-can-I-deal-with-missing-values-in-a-predictive-model/answer/Vinay-Prabhu-7) for more details on this)
    - if you don't lose too much of the dataset after doing so.
  - Build another predictive model to predict the missing values
    - This could be a whole project in itself, so simple techniques are usually used here.
  - Use a model that can incorporate missing data 
    - Like a random forest, or any tree-based method.

#### 10. You have several variables that are positively correlated with your response, and you think combining all of the variables could give you a good prediction of your response. However, you see that in the multiple linear regression, one of the weights on the predictors is negative. What could be the issue?
  - [link](https://www.researchgate.net/post/What-does-it-indicating-If-there-is-positive-correlation-but-negative-regression-coefficient)
  - e.g., imagine height correlates positively with reading ability in children. Consider a more complex model in which age is added. Now age predicts reading and the impact of height is negative. Now add gender and the height effect disappears (because the smaller girls are better readers). In these models it is silly to think of the simple correlation as somehow the effect with the 'correct' sign.
  - **Multicollinearity** refers to a situation in which two or more explanatory variables in a multiple regression model are highly linearly related. 
  - Leave the model as is, despite multicollinearity. The presence of multicollinearity doesn't affect the efficiency of extrapolating the fitted model to new data provided that the predictor variables follow the same pattern of multicollinearity in the new data as in the data on which the regression model is based.
  - principal component regression, *partial least squares regression*
  - *lasso and ridge regression can handle multicollinearity*

#### 11. Let’s say you’re given an unfeasible amount of predictors in a predictive modeling task. What are some ways to make the prediction more feasible?
  - [link](https://towardsdatascience.com/11-dimensionality-reduction-techniques-you-should-know-in-2021-dcb9500d388b)
  - Why a lot of predictors bad:
    - overfitting
    - takes care of multicollinearity
    - removes noise -> increase model accuracy
  - Principle component analysis (PCA), Factor analysis, linear descriminant analysis (all linear approaches)
    - FA is not just reduce the dimensionality of the data. It is a useful approach to find latent variables which are not directly measured in a single variable.
    - LDA: finds a linear combination of input features that optimizes class separability (supervised)
    - PCA: finds a set of uncorrelated components of maximum variance in a dataset. (nonsupervised)

#### 12. Now you have a feasible amount of predictors, but you’re fairly sure that you don’t need all of them. How would you perform feature selection on the dataset?
  - ridge / lasso / elastic net regression
  - *Regularization(L1) lass can lead to zero coefficients -> help us in feature selection*
  - Univariate Feature Selection where a statistical test is applied to each feature individually. You retain only the best features according to the test outcome scores
  - Recursive Feature Elimination:  
    - First, train a model with all the feature and evaluate its performance on held out data.
    - Then drop let say the 10% weakest features (e.g. the feature with least absolute coefficients in a linear model) and retrain on the remaining features.
    - Iterate until you observe a sharp drop in the predictive accuracy of the model.

#### 13. Your linear regression didn’t run and communicates that there are an infinite number of best estimates for the regression coefficients. What could be wrong?
  - p > n.
  - If some of the explanatory variables are perfectly correlated (positively or negatively) then the coefficients would not be unique. 

#### 14. You run your regression on different subsets of your data, and find that in each subset, the beta value for a certain variable varies wildly. What could be the issue here?
  - The dataset might be heterogeneous. In which case, it is recommended to cluster datasets into different subsets wisely, and then draw different models for different subsets. Or, use models like non parametric models (trees) which can deal with heterogeneity quite nicely.

#### 15. What is the main idea behind ensemble learning? If I had many different models that predicted the same response variable, what might I want to do to incorporate all of the models? Would you expect this to perform better than an individual model or worse?
  - *Ensemble learning: multiple learning algorithms to obtain better predictive performance than could be obtained from any of thee constituent learning algorithms alone.*
  - The assumption is that a group of weak learners can be combined to form a strong learner.
  - Hence the combined model is expected to perform better than an individual model.
  - Assumptions:
    - average out biases
    - reduce variance
  - Bagging works because some underlying learning algorithms are unstable: slightly different inputs leads to very different outputs. If you can take advantage of this instability by running multiple instances, it can be shown that the reduced instability leads to lower error. If you want to understand why, the original bagging paper( [http://www.springerlink.com/](http://www.springerlink.com/content/l4780124w2874025/)) has a section called "why bagging works"
    - [link](https://analyticsindiamag.com/primer-ensemble-learning-bagging-boosting/#:~:text=Bagging%20is%20a%20way%20to,based%20on%20the%20last%20classification.)
    - *Bagging reduces the variance of a decision tree classifier. Create several subsets of data from training sample chosen randomly with replacement. Each collection of subset data is used to train their decision trees.*
  - Boosting works because of the focus on better defining the "decision edge". By re-weighting examples near the margin (the positive and negative examples) you get a reduced error (see http://citeseerx.ist.psu.edu/vie...)
    - *Boosting is used to create a collection of predictors. Consecutive trees (random sample) are fit and at every step, the goal is to improve the accuracy from the prior tree. When an input is misclassified by a hyppthesis, its weight is increased so that the next hypothesis is more likely to classify it correctly.*
  - Use the outputs of your models as inputs to a meta-model.   

For example, if you're doing binary classification, you can use all the probability outputs of your individual models as inputs to a final logistic regression (or any model, really) that can combine the probability estimates.  

One very important point is to make sure that the output of your models are out-of-sample predictions. This means that the predicted value for any row in your data-frame should NOT depend on the actual value for that row.

### More project-oriented questions

#### 16. Given that you have wifi data in your office, how would you determine which rooms and areas are underutilized and over-utilized?
  - If the data is more used in one room, then that one is over utilized!
  - Maybe account for the room capacity and normalize the data.

#### 17. How could you use GPS data from a car to determine the quality of a driver?
  - Speed
  - Driving paths

#### 18. Given accelerometer, altitude, and fuel usage data from a car, how would you determine the optimum acceleration pattern to drive over hills?
  - Historical data?

#### 19. Given position data of NBA players in a season’s games, how would you evaluate a basketball player’s defensive ability?
  - Evaluate his positions in the court.

#### 20. How would you quantify the influence of a Twitter user?
  - like page rank with each user corresponding to the webpages and linking to the page equivalent to following.

#### 21. Given location data of golf balls in games, how would construct a model that can advise golfers where to aim?
  - winning probability for different positions

#### 22. You have 100 mathletes and 100 math problems. Each mathlete gets to choose 10 problems to solve. Given data on who got what problem correct, how would you rank the problems in terms of difficulty?
  - One way you could do this is by storing a "skill level" for each user and a "difficulty level" for each problem.  We assume that the probability that a user solves a problem only depends on the skill of the user and the difficulty of the problem.*  Then we maximize the likelihood of the data to find the hidden skill and difficulty levels.
  - The Rasch model for dichotomous data takes the form:  
{\displaystyle \Pr\\{X_{ni}=1\\}={\frac {\exp({\beta_{n}}-{\delta_{i}})}{1+\exp({\beta_{n}}-{\delta_{i}})}},}  
where  is the ability of person  and  is the difficulty of item}.

#### 23. You have 5000 people that rank 10 sushis in terms of saltiness. How would you aggregate this data to estimate the true saltiness rank in each sushi?
  - Some people would take the mean rank of each sushi.  If I wanted something simple, I would use the median, since ranks are (strictly speaking) ordinal and not interval, so adding them is a bit risque (but people do it all the time and you probably won't be far wrong).

#### 24. Given data on congressional bills and which congressional representatives co-sponsored the bills, how would you determine which other representatives are most similar to yours in voting behavior? How would you evaluate who is the most liberal? Most republican? Most bipartisan?
  - collaborative filtering. you have your votes and we can calculate the similarity for each representatives and select the most similar representative
  - for liberal and republican parties, find the mean vector and find the representative closest to the center point

#### 25. How would you come up with an algorithm to detect plagiarism in online content?
  - reduce the text to a more compact form (e.g. fingerprinting, bag of words) then compare those with other texts by calculating the similarity
  - other people's project [link](https://towardsdatascience.com/build-a-plagiarism-checker-using-machine-learning-6538110ce162)

#### 26. You have data on all purchases of customers at a grocery store. Describe to me how you would program an algorithm that would cluster the customers into groups. How would you determine the appropriate number of clusters to include?
  - Demographic, geographical, pyschographics (social class, lifestyle, personality), behavioral data (spending and consumption habits)
  - K-means
    - choose a small value of k that still has a low SSE (elbow method), within cluster sum of squares (WCSS)
    - [Elbow method](https://bl.ocks.org/rpgove/0060ff3b656618e9136b)

#### 27. Let’s say you’re building the recommended music engine at Spotify to recommend people music based on past listening history. How would you approach this problem?
  - content-based filtering [A good article](https://towardsdatascience.com/how-to-build-an-amazing-music-recommendation-system-4cce2719a572)
    - Similar genres will sound similar and will come from similar time periods
    - We can use this idea to build a recommendation system by taking the data points of the songs a user has listened to and recommending songs corresponding to nearby data points.
      1. compute the average vector of the audio and metadata features for each song that user has listened to.
      2. Find th n-closest data points in the dataset (excluding the points from the songs in the user's listening history) to this average vector.
      3. Take these n points and recommend the songs corresponding to them
  - collaborative filtering
    - user-based approach
      - It finds users with similar interests and behavior, and considering waht those similar users listened to, it makes a recommendation.
    - item-based approach
      - it can take into account what songs the user has considered in the past and recommend new similar songs that the user can enjoy
