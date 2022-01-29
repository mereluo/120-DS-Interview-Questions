## Statistical Inference (15 questions)

#### 1. In an A/B test, how can you check if assignment to the various buckets was truly random?
  - Plot the distributions of multiple features for both A and B and make sure that they have the same shape. More rigorously, we can conduct a permutation test to see if the distributions are the same.
  - MANOVA to compare different means

#### 2. What might be the benefits of running an A/A test, where you have two buckets who are exposed to the exact same product?
  - Verify the sampling algorithm is random. *The experiment population and control population should be comparable.*
    - *Each condition is randomly assigned to each group with a probability of 0.5. Use this to compute SD of binomial with probability 0.5 of success. Calculate the confidence interval. The observved fraction of control group is greater than the upper bound of CI => there is something wrong with the setup.*
  - *Use A/A test to estimate the empirical variance of the metrics. -- Compare the difference so that the difference is driven by the underlying variability, such as system, user populations, etc. If you see a lot of variability in a metric in an A/A test, the metric might be too sensitive to use in the experiment.*

#### 3. What would be the hazards of letting users sneak a peek at the other bucket in an A/B test?
  - The user might not act the same suppose had they not seen the other bucket. You are essentially adding additional variables of whether the user peeked the other bucket, which are not random across groups.
  - *Spillover effect: If user A is in the treatment group and is connected with user B in the control group, user A's change in behavior might impact user B's behavior.*
    - *Spillover effect might not only lead to an increaased engagement in the treatment group, but also in the control group. This dilutes the positive effect of the treatment, which can ultimately lead to wrong conclustions when using the standard AB testing approach in especially social network analysis or a collaborative application.* [AB-Testing challenges in social networks](https://towardsdatascience.com/ab-testing-challenges-in-social-networks-e67611c92916#:~:text=Cluster%20sampling%2C%20also%20known%20as,to%20deal%20with%20spillover%20effects.&text=When%20an%20AB%2DTest%20is,to%20the%20aforementioned%20spillover%20effects.)

#### 4. What would be some issues if blogs decide to cover one of your experimental groups?
  - Same as the previous question. The above problem can happen in larger scale.

#### 5. How would you conduct an A/B test on an opt-in feature? 
  - *It is a somewhat a ambiguous question* [reference](https://www.kdnuggets.com/2017/03/17-data-science-interview-questions-answers-part-3.html/2)
    1. *How would you conduct an A/B test on an opt-in version of a feature to a non-opt-in version?*
    2. *How would you conduct an A/B test on the adoption (or use) of an opt-in feature (i.e., test the actual opting-in)?*
    3. *How would you conduct an A/B test on different version of an opt-in feature (i.e., for those having already opted in)?*

#### 6. How would you run an A/B test for many variants, say 20 or more?
  - one control, 20 treatment, if the sample size for each group is big enough.
  - Ways to attempt to correct for this include changing your confidence level (e.g. Bonferroni Correction) or doing family-wide tests before you dive in to the individual metrics (e.g. Fisher's Protected LSD).
  - *Multiple Comparison Problem: the chance of getting a false positive increases when the number of variants increases*
    - *False positive formula*: <img src="https://render.githubusercontent.com/render/math?math=1-(1-\alpha)^m"> *with m being the total number of variations tested*
  - *Bonferroni correction*:
    - *formula:*  <img src="https://render.githubusercontent.com/render/math?math=\alpha/m">
    - *For example, if you are testing m = 8 hypotheses with a desired \alpha = 0.05, then the Bonferroni correction would test each individual hypothesis at* \alpha = 0.05/8 = 0.00625
  - *It might be tempted to declare that the variation with the highest life the winner. Even though one variation may be prforming the best in comparison to the control, you may not have significance between it and the other variations.* [reference](https://www.quora.com/How-would-you-run-an-A-B-test-for-many-variants-say-20-or-more)

#### 7. How would you run an A/B test if the observations are extremely right-skewed?
  - lower the variability by modifying the KPI
    - Original metrics: revenue from clicks -> contains a lot of zero (people don't purchase or click on ads), and a long right tail
    - Instead of revenue, look at (1) an indicator for conversion (yes/no), and the conditional (revenue if purchased; null otherwise).
    - [Example at 3.2.1.](https://exp-platform.com/journal-survey/)
  - cap values
    - Capping time on site to 30 minutes is a common strategy; cappinng at a certain dollar values and items purchased lowered the variance.
  - percentile metrics: instead of mean, look at some percentile, such as 90th. (e.g., time-to-onload event for page-load, time-to-click)
  - log transform
  - <https://www.quora.com/How-would-you-run-an-A-B-test-if-the-observations-are-extremely-right-skewed>

#### 8. I have two different experiments that both change the sign-up button to my website. I want to test them at the same time. What kinds of things should I keep in mind?
  - exclusive -> ok
  - *Run the tests in parallel, but on isolated audiences (the audience experiencing test 1 will not participate in test 2, and vice versa)*
  - *Unit of diversion: cookies, to avoid if one sees inconsistent sign-up button on their site*

#### 9. What is a p-value? What is the difference between type-1 and type-2 error?
  - [en.wikipedia.org/wiki/P-value](https://en.wikipedia.org/wiki/P-value)
    - *The probability of obtaining test results at least as extreme as the observed results, under the assumption that the null hypothsis is correct. A very small p-value means that such an extreme observed outcome would be very unlikely under the null hypothesis.*
  - type-1 error (false positives): rejecting Ho when Ho is true
  - type-2 error (false negatives): not rejecting Ho when Ha is true

#### 10. You are AirBnB and you want to test the hypothesis that a greater number of photographs increases the chances that a buyer selects the listing. How would you test this hypothesis?
  - For randomly selected listings with more than 1 pictures, hide 1 random picture for group A, and show all for group B. Compare the booking rate for the two groups.
  - *"How many more photos do we need to have to see the increase"*
  - *Potential metrics: # clicks / # listings per user; # bookings / # users in each group;*

#### 11. How would you design an experiment to determine the impact of latency on user engagement?
  - The best way I know to quantify the impact of performance is to isolate just that factor using a slowdown experiment, i.e., add a delay in an A/B test.
  - *Page load speed: to see if users are more likely to perform clicks on the result page that is served with lower latency, # clicks / # page views; revenue / # page views*
  - *Chat response: high latency leads to channel waiting*
  - *Video streaming, cloud gaming* [reference](https://blog.pusher.com/how-latency-affects-user-engagement/)

#### 12. What is maximum likelihood estimation? Could there be any case where it doesn’t exist?
  - A method for parameter optimization (fitting a model). We choose parameters so as to maximize the likelihood function (how likely the outcome would happen given the current data and our model).
  - maximum likelihood estimation (MLE) is a method of [estimating](https://en.wikipedia.org/wiki/Estimator "Estimator") the [parameters](https://en.wikipedia.org/wiki/Statistical_parameter "Statistical parameter") of a [statistical model](https://en.wikipedia.org/wiki/Statistical_model "Statistical model") given observations, by finding the parameter values that maximize the [likelihood](https://en.wikipedia.org/wiki/Likelihood "Likelihood") of making the observations given the parameters. MLE can be seen as a special case of the [maximum a posteriori estimation](https://en.wikipedia.org/wiki/Maximum_a_posteriori_estimation "Maximum a posteriori estimation") (MAP) that assumes a [uniform](https://en.wikipedia.org/wiki/Uniform_distribution_\(continuous\) "Uniform distribution \(continuous\)") [prior distribution](https://en.wikipedia.org/wiki/Prior_probability "Prior probability") of the parameters, or as a variant of the MAP that ignores the prior and which therefore is [unregularized](https://en.wikipedia.org/wiki/Regularization_\(mathematics\) "Regularization \(mathematics\)").
  - for gaussian mixtures, non parametric models, it doesn’t exist
  - *For normal distribution: sigma = (x - mu)^2, if x = mu, then sigma = 0, which is not allowed. So the MLE does not exist in this case.*
  - *For uniform distribution, if the interval included its boundary, then clearly the MLE would be theta = max[X_i]. But since this interval does not include its boundary, then MLE cannot be the maximum, and therefore an MLE does not exist*

#### 13. What’s the difference between a MAP, MOM, MLE estimator? In which cases would you want to use each?
  - MAP (Maximum A Posteriori) estimates the posterior distribution given the prior distribution and data which maximizes the likelihood function. MLE is a special case of MAP where the prior is uninformative uniform distribution. [reference](https://agustinus.kristia.de/techblog/2017/01/01/mle-vs-map/)
  - MOM (Method of Moments) sets moment values and solves for the parameters. MOM is not used much anymore because maximum likelihood estimators have higher probability of being close to the quantities to be estimated and are more often unbiased.

#### 14. What is a confidence interval and how do you interpret it?
  - For example, 95% confidence interval is an interval that when constructed for a set of samples each sampled in the same way, the constructed intervals include the true mean 95% of the time.
  - *If we get a set of samples each sampled in the same way, and we constructed a interval for each of the sample, the constructed intervals include the true population mean 95% of the time. If I have 100 intervals, then 95 of them include the true mean.*
  - Want to estimate population mean (mu), using x_bar:
    - <img src="https://render.githubusercontent.com/render/math?math=x_bar \pm t(\frac {s} {sqrt{n}})">
  - Want to estimate population porportion (pi), using p:
    - <img src="https://render.githubusercontent.com/render/math?math=p \pm z(\frac {p(1-p} {n})">
  - if confidence intervals are constructed using a given confidence level in an infinite number of independent experiments, the proportion of those intervals that contain the true value of the parameter will match the confidence level.
  - *P-value's relationship with confidence interval*

#### 15. What is unbiasedness as a property of an estimator? Is this always a desirable property when performing inference? What about in data analysis or predictive modeling?
  - Unbiasedness means that the expectation of the estimator is equal to the population value we are estimating. 
  - *E(estimator) = Population parameter*
  - This is desirable in inference because the goal is to explain the dataset as accurately as possible. However, this is not always desirable for data analysis or predictive modeling as there is the bias variance tradeoff. We sometimes want to prioritize the generalizability and avoid overfitting by reducing variance and thus increasing bias.

#### 16. What is Selection Bias?
  - Selection bias is a kind of error that occurs when the researcher decides who is going to be studied. It is usually associated with research where the selection of participants isn’t random. It is sometimes referred to as the selection effect. It is the distortion of statistical analysis, resulting from the method of collecting samples. If the selection bias is not taken into account, then some conclusions of the study may not be accurate.
  - The types of selection bias include:
    - Sampling bias: It is a systematic error due to a non-random sample of a population causing some members of the population to be less likely to be included than others resulting in a biased sample.
    - Time Interval bias: A trial may be terminated early at an extreme value (often for ethical reasons), but the extreme value is likely to be reached by the variable with the largest variance, even if all variables have a similar mean.
    - Data: When specific subsets of data are chosen to support a conclusion or rejection of bad data on arbitrary grounds, instead of according to previously stated or generally agreed criteria.
    - Attrition: Attrition bias is a kind of selection bias caused by attrition (loss of participants) discounting trial subjects/tests that did not run to completion.

### Related Statistical Concepts

#### Permutation Tests
  - Why Use Permutation Approach? [(Video)](https://www.youtube.com/watch?v=rJ3AZCQuiLw)
    - Small Sample Size
    - Assumptions (for parametric approach) not met
    - Test something other than classic approaches comparing Means and Medians
    - Difficult to estimate SE for test-statistic
  - If we have 9 observations for both groups in total (e.g., A: 5, B: 4), then the number of permutation tests we can perform is: 9!
  - Covert test-statistics -> p-value:
    - P-value = # perm.t.s > obs.t.s / # permutation tests

#### Maximum Likelihood Estimator (MLE)
  - [statquest video](https://www.youtube.com/watch?v=XepXtl9YKwc): "This location of the mean maximizs the likelihood of observing the weights we measured. Thus, it is the maximum likelihood estimaate for the mean."
    - When someone says that they have the mle for the mean or the standard deviation, or for something else, you know that they found the value fo the mean or std that maximizes the likelihood that you observed the things you observed.
    - "likelihood" specifically refers to where you are trying to find the optimal value for the mean or std for a distribution given a bunch of observed measurements.
  - [Exponential Distribution Example](https://www.youtube.com/watch?v=p3T-_LMrvBc)
    1. Exponential Distribution: <img src="https://render.githubusercontent.com/render/math?math=y=\lambda e^{-\lambda x}">
    2. What is the likelihood of lambda given first measurement, x1 -> <img src="https://render.githubusercontent.com/render/math?math=L(\lambda|x_1)=\lambda e^{-\lambda x_1}">
    3.  <img src="https://render.githubusercontent.com/render/math?math=L(\lambda|x_1,x_2,...,x_n) = L(\lambda|x_1)L(\lambda|x_2)...L(\lambda|x_n)"> 
    4.  <img src="https://render.githubusercontent.com/render/math?math=\lambda^n[e^{-\lambda(x_1+x_2+...+x_n}]">
    5.  Take the derivative of this, solve for lambda when the derivative is set to be equal to 0
    6.  The mle for lambda is: <img src="https://render.githubusercontent.com/render/math?math=\lambda = \frac {n} {x_1 + x_2 +...+x_n}">
