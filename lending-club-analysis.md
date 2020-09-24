Final Paper
================
STOR 320.(02) Group 6
September 24, 2020

# INTRODUCTION

Following the initial meeting and introduction of the project, Group 6
members perused the vast libraries of datasets online and happened
across a particularly large dataset via data.world containing a complete
profile of loans issued by the company LendingClub (LC). LendingClub is
the first largest peer-to-peer platform for facilitating lending and
borrowing of loans from $1000 to 35000. LC hit the highest IPO in the
tech sector in 2014. Our group found the prospect of evaluating credit
for investments and loan issuing particularly interesting and useful for
future applications in risk analysis and creating realistic
recommendations for potential investors.

The initial questions that Group 6 posed were, unfortunately, not
thorough enough to create substantial predictive models. One of the
questions Group 6 initially posed but didn’t expand on during the EDA
phase of the project was “Can we predict whether an investor is able to
pay a loan in full or if the account becomes delinquent?” A loan is
considered delinquent the first day after a payment is missed, and the
account remains delinquent until the past due amount is paid in full.
This is one of the largest factors in credit risk: the investors ability
to repay principal loans or interests. It is, of course, in the
investor’s best interest to be able to repay a loan on time to avoid
an on-record delinquency and higher interest rates. The lending company
will also seek clients with a good credit history, and issue loans with
lower interest rates to maintain an investor for the duration of the
loan term. Thus, I went rogue and decided to tackle this question in my
independent final analysis.

By recommendation of Dr. Mario, the second question I wanted to explore
was “Can we predict the loan amount an investor will be issued?” This
question aims to explore the factors that most significantly influence
an individual’s line of credit and ability to be issued a loan.
Moreover, since peer to peer lending is highly risky in terms of
unsecured loans and high probability of investors defaulting, it would
be interesting to determine if the amount a borrower is able to obtain
from an unsecured loan could even be determined by factors including a
person’s past spending habits, income, and past credit managing
behavior.

Through the exploration and analysis of these two questions, my aim to
create a preliminary risk assessment of borrowers in the peer-to-peer
lending industry and make recommendations to LC based on volatility of
investor habits and potential loss the company could face by issuing
loans to people unable to pay back full amount plus interest. This is
likely pretty dry information to most millennials and young adults, but
building good credit starts now, especially for those with some student
loan debt. If you are ever considering opening a credit card account or
taking out a loan on a mortgage, these results might interest you.

# DATA

The dataset we chose was found on data.world, uploaded by user
lpetrocelli, but the source originated from the LendingClub official
website, available as an downloadable file of loan data issued in the
first quarter of 2017. The dataset contains over 96000 observations from
customers who were issued loans from January to March of 2017, and 120
different variables related to each investor. Our group considered this
a valuable dataset thorough enough create a predictive model for the
questions proposed.

One important variable is the record of delinquencies within the last
two years (delinq\_2yrs), which indicates the number of 30+ day past-due
incidents borrowers have had in the past 2 years. Once an overdue
payment is recorded, it remains on an individual’s credit history for up
to 7 years, and is generally a good indicator of a borrower’s ability to
make payments on a loan on time. For most lending companies, generally
the less delinquencies on a customer’s credit history the more likely
they are to get a better loan. The purpose and status of home ownership
(home\_ownership) variables appeared to be valuable as well, as with a
quick bar plot gives us a good indicator of the general demographic of
individuals being issued loans from LC.

![](lending-club-analysis_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

“Debt to Income Ratio” (dti) is a ratio that calculates the total
monthly debt payments divided of a lender by his or her gross monthly
income. It is another useful indication of an individual’s ability to
manage monthly payments and repay debts, and is typically used by
mortgage lenders. Generally, 43% is the highest DTI ratio a borrower can
have and still be qualified for a mortgage. However, there are values
within the dataset well above 1000 and in the negatives, but we will
still consider the dataset valuable to account for possible events such
as winning the lottery or unreported annual income that are realistic to
daily life.

Other important variables, some of which were used to determine the loan
amount an investor will be issued, were “Interest Rate” (int\_rate),
“Annual Income” (annual\_inc), “Number of Inquiries in the last 6
months” (inq\_last\_6mths) and “Number of Open Credit Lines in a
Borrower’s Credit File” (open\_acc). If we can somehow create a
relationship between these parameters as an indicator of credit score or
liability to make payments on a loan, we will be able to better
understand what qualifies a borrower for a loan and the risks assessed
by lending companies whenever they issue loans.

The following is a representation of the most important variables
provided in our data.

|      | loan\_amnt | term      | int\_rate | purpose             | home\_ownership | annual\_inc |   dti | delinq\_2yrs | open\_acc | inq\_last\_6mths |  dti2 | employ    | status     |
| :--- | ---------: | :-------- | --------: | :------------------ | :-------------- | ----------: | ----: | -----------: | --------: | ---------------: | ----: | :-------- | :--------- |
| 125  |       5000 | 36 months |     14.99 | home\_improvement   | OWN             |       60000 | 13.32 |            0 |         5 |                0 | 13.32 | 2-4 years | Fully Paid |
| 713  |      25000 | 60 months |     18.99 | medical             | MORTGAGE        |       80000 |  8.07 |            0 |        11 |                1 |  8.07 | 2-4 years | Fully Paid |
| 822  |      29225 | 36 months |     18.99 | car                 | RENT            |       85000 |  0.35 |            0 |         3 |                0 |  0.35 | 10+ years | Fully Paid |
| 1184 |      18000 | 36 months |      7.49 | debt\_consolidation | MORTGAGE        |      107000 | 12.09 |            0 |        14 |                0 | 12.09 | 2-4 years | Fully Paid |
| 1395 |       7200 | 36 months |     12.74 | other               | MORTGAGE        |       73000 | 12.05 |            0 |         7 |                0 | 12.05 | 10+ years | Fully Paid |
| 1430 |       4825 | 36 months |     11.44 | car                 | MORTGAGE        |       42000 |  7.80 |            0 |         6 |                0 |  7.80 | 5-9 years | Fully Paid |

# RESULTS

To answer the first question predicting the ratio of delinquent accounts
in a given quarter, I had to select and clean the most valuable
parameters to my investigation. For the sake of simplifying the data, I
decided to select only the variables that seemed to influence credit
score most (indicated by the graphic given by vantagescore.com
<https://www.vantagescore.com/pdf/VantageScore%20Infographic%2005.pdf>).
In addition, I cleaned the loan status parameter to only contain loans
that were either “Fully Paid” or “Delinquent”. I omitted loans that were
current or loans that were late but within the 15-day grace period, as
these loans have not matured and thus would add another level of
unpredictability to the model. I decided the leave dti2 within its
current range of 0.13 up to 84.03, to account for the possibility of
unaccounted debt payments at the time of the request. This brought the
dataset down to a size of 2300 observations from the initial 96000.

Then, the data was split into “train” and “test” data sets. 80% of the
cleaned data was assigned the training set, and the remaining 20%
assigned the test set. Then, I implemented a random forest model to
predict occurrences of delinquencies. The parameters annual income, home
ownership status, interest rate, employment status, debt to income
ratio, term of the loan, and delinquencies were used as predictors.
MeanDecreaseAccuracy is a measure of how much a variable improves the
accuracy of the forest in predicting the categorical outcome. The higher
the variable, the more it improves the prediction. From the plot, annual
income is clearly the most important variable, although the value itself
is quite low. From the MeanDecreaseGini, another measure of importance
of factors to the accuracy of the model, debt to income ratio, annual
income, and interest rate indicate that these are the most important
predictors.

![](lending-club-analysis_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

    ##    
    ##       0   1   2   3   4   5   6   7
    ##   0 351  64  13   6   2   1   1   1
    ##   1   1   0   0   0   0   0   0   0

    ## [1] "Correctly Predicted: 84.38%"

The success of the random forest model is determined by the percentage
of correct predictions over the category with the highest frequency, in
this case being the indicator “Fully Paid”. According to our initial
cleaning of the data, 2128 observations of the 2340 are “Fully Paid”, or
90.94% of loans were recorded as fully paid off. For the model to be a
success, the percentages of cases predicted correctly should be higher
than 90.94%. According the prediction model, 84.38% of observations were
correctly predicted as “Fully Paid”, which is about 6.56% less than the
actual ratio.

To further investigate this question, I implemented the k-Nearest
Neighbors (k-NN) Technique to cross-validate the prediction on the two
possible outcomes of loans. For this model, I wanted to see if
delinquency and debt to income ratio were as invaluable as I initially
expected, then used k-NN to predict a loan to be fully paid off or
become delinquent. Again, splitting the data into test and train
subsets, with 80% of the dataset to train the model. Then, I evaluated
the model by cross tabulating predictions against the actual class of
loan status in the test data set. The model correctly classified 421
loans as “Fully Paid” out of 470, giving us a percentage of 89.6%. This
is about 1.3% less than the actual ratio from the dataset, which
although still less than the actual percentage is still better than the
random forest model. Our total accuracy is around 89.8%.

    ## 
    ## Fully Paid Delinquent 
    ##       2128        212

    ## 
    ## Fully Paid Delinquent 
    ## 0.91069519 0.08930481

    ## [1] Fully Paid Fully Paid Fully Paid Fully Paid Fully Paid Fully Paid
    ## Levels: Fully Paid Delinquent

    ##    Cell Contents 
    ## |-------------------------|
    ## |                   Count | 
    ## |           Total Percent | 
    ## |-------------------------|
    ## 
    ## ==============================================
    ##                pred
    ## test$status    Fully Paid   Delinquent   Total
    ## ----------------------------------------------
    ## Fully Paid           421            4     425 
    ##                     89.6%         0.9%        
    ## ----------------------------------------------
    ## Delinquent            44            1      45 
    ##                      9.4%         0.2%        
    ## ----------------------------------------------
    ## Total                465            5     470 
    ## ==============================================

These results were rather interesting, as I originally thought record of
past delinquencies and debt to income ratio significant predictors of
future delinquent loans. This shows that LC has a pretty good ratio of
fully paid loans to delinquencies, and if otherwise would mean that LC
has highly risky investors and a high probability of experiencing loss
within each quarter.

For the second question, I sought to create a model that could best
predict the loan amount an individual could be issued based on variables
given within the dataset. I used a multiple linear regression model to
visualize this relationship. The model initially constructed predicted
loan amount as a function of annual income, interest rate, debt to
income ratio, the number of open accounts, and delinquencies. The
R-squared value was around 0.223. When replicating the model, I removed
delinquencies and debt to income ratio as predictors and the R-squared
value increased to 0.228. I decided that getting an R-squared value
above 0.50 would be difficult considering the already large variation in
loans issued from the dataset, and continued with the second
multi-linear regression model.

    ## 
    ## Call:
    ## lm(formula = loan_amnt ~ annual_inc + int_rate + open_acc + factor(term) + 
    ##     delinq_2yrs + inq_last_6mths, data = loan2)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -44109  -5832  -1509   4571  28349 
    ## 
    ## Coefficients:
    ##                          Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)             1.781e+03  6.238e+02   2.855  0.00435 ** 
    ## annual_inc              6.419e-02  3.116e-03  20.600  < 2e-16 ***
    ## int_rate                2.603e+02  3.245e+01   8.021 1.65e-15 ***
    ## open_acc                1.566e+02  2.884e+01   5.430 6.21e-08 ***
    ## factor(term) 60 months  6.536e+03  4.391e+02  14.884  < 2e-16 ***
    ## delinq_2yrs            -3.990e+02  2.200e+02  -1.814  0.06979 .  
    ## inq_last_6mths         -5.613e+02  1.953e+02  -2.874  0.00409 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 8132 on 2333 degrees of freedom
    ## Multiple R-squared:  0.3039, Adjusted R-squared:  0.3021 
    ## F-statistic: 169.8 on 6 and 2333 DF,  p-value: < 2.2e-16

![](lending-club-analysis_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->![](lending-club-analysis_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->![](lending-club-analysis_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->

From this predictive model, I performed a diagnosis of the linear model
by plotting diagnostic plots with the built-in R-function plot(). I
looked the Residuals vs Fitted plot, which is an indicator of any
patterns the residuals may have. The plot looks good util it passes the
fitted values at 40,000, where there exists an extreme outlier number
77888. If we look at all the residuals before that point, however, there
doesn’t appear to be any distinctive pattern or relationship in the
model, which is a good indication that we have a linear relationship.
Next, I looked at the Normal Q-Q plot, which depicts the distributions
of the residuals. Again, safe for the extreme outliers, the residuals
look relatively well aligned on the dashed line.

    ## [1] 8119.342

And finally, the root mean squared error (RMSE) of the predictive model
was 8119.342. Considering the loan amounts

For the predictive plot itself, the linear model we created appears to
be more conservative with respect to the loan amounts it predicted, with
expected values well below actual values as the loan amounts increased.
From the appearance of the models, it is clear there are some predictive
issues with the model, especially with the low coefficient of
determination from our linear model and lack of normalcy in the
distribution of loan amounts from the data. I will talk about this in
more detail in the conclusions.

# CONCLUSION

In our first question, we wanted to predict the number of fully paid and
delinquent accounts using debt and income ratio and the borrower’s
record of past delinquencies. From the random forest model and k-NN
Technique, our model was slightly off from the actual ratio of fully
paid to delinquent accounts, but had pretty high accuracy’s of around 84
and 90 percent. I was expecting the model to accurately predict the
status of matured loans given the significance of the predictors DTI and
delinquent accounts, so I was surprised to see that debt to income ratio
did nothing to improve the accuracy of the random forest model. However,
I was still pleased to see that the relative accuracy and predicted
status of issued loans were quite accurate. Especially since lending
companies deal with such a volatile industry with many factors
influencing the qualifications of borrowers for loans, the fact that the
model was able to predict the number of fully paid loans would be highly
beneficial to lending companies seeking to minimize losses in a given
quarter and maximize returns from their investors.

For the second question, we looked to predict the amount of loan that a
qualified investor could be issued and give a general distribution for a
given quarter. I found the most significant model was a multiple linear
regression predicting loan\_amnt as a function of annual\_inc +
int\_rate + open\_acc + factor(term) + delinq\_2yrs + inq\_last\_6mths
with an R-squared of 0.3021 and an RMSE of 8119.342. While these results
are not impressive, with the exception of extreme outliers the linear
regression model appeared to be a good fit for the data, justified by
the diagnostic plots. Even with cleaning the data and removing some
extreme outliers, there appeared to be too much variation in the
loan\_amnt to be able to create a completely accurate and comprehensive
model without any misleading parameters. If this model were to be
explored further in the future with a more extensive and meaningful set
of variables, it could be useful to consider the influence of the
categorical variables purpose and employment length. Majority of loans
taken out were for debt consolidation, and of those loans about half of
borrowers had a mortgage on a house. We could separate the data
according to purpose of loans and from there rerun the predictors of the
linear regression and see if the R-squared improves.

In conclusion, the lending industry and investments will never be risk
free. Hopefully this analysis has revealed the importance of building
good credit and practicing good credit habits, so that you, the
investor, can determine if your loan will help you profit or result in
crippling loss.
