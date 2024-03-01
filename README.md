---
title: "Causality test - Livestream case study"
author: "Do Vu"
date: "2024-02-29"
output:
  html_document:
    toc: true
    toc_depth: '3'
    df_print: paged
toc-title: "Table of Contents"
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE,fig.width = 10,fig.height = 10,fig.align='center')
```

# General information
Git-hub repository at:
https://github.com/XuanDoVu1996/Causality-test-1.git

- Rmarkdown: **Causality test.Rmd**
- data set: **datasets\framing.csv**

The data are created based on the paper “Rejections Are More Contagious than Choices: How Another’s Decisions Shape Our Own” by Nan, L. X., Park, S. K., & Yang, Y. (2023) published by Journal of Consumer Research. 

- case study: https://doi.org/10.1093/jcr/ucad007

<img src="images/livestream.jpg" alt="Livestream" width="300"/>

<P style="page-break-before: always">

# Introduction

Our objective is to examine the impact of message framing on customers' inclination to adhere to someone else's decision.

In this investigation, the researchers collaborated with a social media influencer boasting millions of followers to conduct a real-world experiment. Specifically, within an ongoing live streaming event, the influencer showcased two insulated water bottles for children to the audience. Participants were randomly divided into two groups. In one group, viewers were informed that an acquaintance of the influencer had selected one bottle (referred to as the choice frame condition). In the other group, individuals were informed that an acquaintance of the influencer had rejected the alternative bottle (referred to as the rejection frame). The authors hypothesized that the rejection frame would heighten the likelihood of consumers adhering to the decision made by the influencer's friend, in contrast to the choice frame.

<P style="page-break-before: always">

# Data variable description

| Name       | Description                                                |
|------------|------------------------------------------------------------|
| ID         | Unique ID of the viewer                                    |
| condition  | 1 if viewer was in the choice frame condition, 2 if in the rejection frame condition |
| conformity | 1 if the viewer did not conform to the friend’s decision, 2 otherwise |
| gender     | Gender of the viewer                                       |
| age        | Age of the viewer                                          |



<P style="page-break-before: always">

# Required packages
```{r}
library(DescTools)  # Tools for Descriptive Statistics
library(ggpubr)     # to draw scatter plots
library(dplyr)      # to wrangle data
library(ggplot2)    # to draw different type of plots
library(tinytex)    # to knit Rmarkdown
library(tidyverse)  # packages for data analysis
library(jtools)     # Analysis and Presentation of Social Scientific Data
```

# 1) Data preprocessing and exploration 
## a) Data structure and type

```{r}
# Load data
framing <- read.csv("./datasets/framing.csv")
# observe the data
head(framing,2)
```

Our data has 5 variables ("ID","condition","conformity","gender","age") and 170 observations, which is considered a small dataset. All variables are in "integer" format except "gender".

```{r}
cat("Number of observations: ",nrow(framing),"\n") # number of observations
cat("Number of variables: ",ncol(framing),"\n")    # number of variables
```

## b) Missing values

Generally, our data is clean and does not have many missing values, except for the "gender" variable with 8 "NA" values. Missing data might cause sampling bias and give us unrepresentative sample (Pritha, 2023), which affects the quality of statistical inference (Dong & Peng, 2013). There is no universal rule for the acceptable proportion of missing data, but a missing rate of 5% or less is inconsequential (Schafer, 1999) and more than 10% will give bias in statistical analysis (Bennett, 2001). As our missing rate is only 4.7%, we apply list-wise deletion to remove those 8 observations without affecting further analysis too much.   

```{r}
cat("\n Number of missing values per columns: \n")
colSums(is.na(framing))             # count number of NAs per each column
framing <- framing %>% na.omit()    # list-wise deletion
```

## c) Descriptive statistics

As age must be positive, we drop the observation having negative value of "age" (only one observation having age -1).

```{r}
cat("\n Descriptive statistics before handling incorrect information:\n")
summary(framing)                       # before summary
framing <- framing[framing$age >0, ]   # remove negative age
cat("\n Descriptive statistics after handling incorrect information:\n")
summary(framing)                       # after summary
```

## d) Data transformation
To observe the variables in different categories, we convert the binary categorical variables ("conformity", "gender" and "condition") and continuous variable "age" to factors, in which "age" is categorized into 6 levels.       

Our final data frame has 161 observations and 5 variables. We have two conditions (Rejection frame and Choice frame), and two outcomes conformity or non-conformity, three genders, and six levels of age.

```{r}
cat("\n Data type before: \n")
t(lapply(X=framing, FUN = class))                 # check data type
framing[,2:4] <- lapply(framing[,2:4], as.factor) # convert to factors
framing$age <- cut(framing$age,                   # convert age to range of age
                   breaks = c(15, 25, 35, 45, 55, 60, Inf), 
                   labels = c("15-25", "26-35", "36-45", "46-55", "56-60","60+"))

cat("\n Data type after: \n")
t(lapply(X=framing, FUN = class))
head(framing, 2)
```

## e) Visulization and data analysis
From Figure 1.1, we observed that male viewers dominated our observations (63.5%) compared to female viewers (33.5%) and others (only 3%). From Figure 1.2, we see that the proportion of viewers confirming the friend's decision is quite low (30.4%). Most of our viewers are relatively young (82% less than 45 years old). The two frame groups have relatively similar numbers of viewers and the distribution of gender. However, the choice frame group has lower probability of conform (23% compared to 38% of rejection frame) and slightly different age distribution. 

```{r}
par(mfrow = c(3, 1))   # divide the frame into grid for multiple plots

# calculate the conformity probability per each frame condition
conformity <- prop.table(table(framing$condition,
                               framing$conformity),
                         margin = 1)
rownames(conformity) <- c("Choice Frame", "Rejection Frame")
colnames(conformity) <- c("Not conform","Conform")
conformity

# Plot 1.1
barplot(table(framing$condition, framing$gender),
        names.arg = c("Female","Male","Others"),          
        main = "Fig 1.1: Number of viewers per Gender and Framing Condition",
        xlab = "Type of gender",                 
        ylab = "Number of the viewers") 
# Add legend based on "condition"
legend("topright",legend = c("Reject Frame Condition","Choice Frame Condition"),
       fill = c("grey", "black"))

# Plot 1.2
barplot(table(framing$condition, framing$conformity),
        names.arg = c("Not conformed","Conformed"),          
        main = "Fig 1.2: Number of viewers per Conformity and Framing Condition",         xlab = "Type of conformity",                 
        ylab = "Number of the viewers")
# Add legend based on "condition"
legend("topright",legend = c("Reject Frame Condition","Choice Frame Condition"),
       fill = c("grey", "black"))

# Plot 1.3
barplot(table(framing$condition, framing$age),
        names.arg = c("15-25","26-35","36-45",
                      "46-55","56-60","60+"),          
        main = "Fig 1.3: Number of viewers per Age and Framing Condition",
        xlab = "Type of Age",                 
        ylab = "Number of the viewers")
# Add legend based on "condition"
legend("topright",legend = c("Reject Frame Condition","Choice Frame Condition"),
       fill = c("grey", "black"))
```


# 2) Comparing the conformity probabilities - Hypothesis testing 

As mentioned in part 1, the choice frame has noticeably lower probability of conformity than that of the rejection group. This difference in probability gives us a signal that the exposure to different framing conditions affected customers' tendency to comply with another person's decision. In our case, the first insulated bottle is chosen more by viewers if the friend rejects another bottle than if the friend chooses the first one. Our observation is like the experiment result in the study, which is not surprising because we only remove few observations having missing values. However, the significance levels of the result might be different. Thus, we will do statistical hypothesis testing to verify this presumption.

We determine the hypothesis as follows:

**"H1: Consumers are more likely to conform to another’s decision if they perceive the decision as a rejection than if they perceive it as a choice."** (Nan et al., 2023)

Nan et al. (2023) noted that there are uncontrollable factors in this type of study (no control over who participates or when viewers participate). The first factor might distort our result if the two groups of viewers are systematically different (not comparable). Indeed, as mentioned in part 1, two groups of viewers have slightly different age distribution, which makes age being a potential confounder. In addition, if the viewer joins the livestream after the streamer talks about the framing conditions, our framing conditions might not make any effect. Thus, in our study, we assume that the participants were randomly assigned to the choice and rejection frames for this test. We also assume that the observations are independent of each other within and between the groups. This means that for a particular individual, their choice is unaffected by the choice of others in their group, and neither should their choice be affected by the choice of others in an external group.  

Next, we formulate statistical hypotheses:

**•	(H0): There is no association between framing condition and conformity.**  
**•	(HA): There is a significant association between framing conditions and conformity.**

We have two categorical variables ("condition" and "conformity"). Thus, we use the chi-squared test for the independence of the two variables (Shaun, 2022), which will allow us to test whether the condition and conformity variables are related or not. This testing would allow us to confidently claim whether our result is merely by chance or caused by the exposure to different frame conditions. Accordingly, we can conclude whether the probabilities of conformity might be due to the different framing conditions or not. 

```{r}
#Performing the chi squared test to see if the level of 
#conformity depends upon the condition to which they were exposed.

table_result <- table(framing$condition,framing$conformity)
chi_squared_test <- chisq.test(table_result)
print(chi_squared_test)
```

Our chosen significance level is 95%, and the p-value for our test was 0.048 (less than 0.05). Thus, we can reject the null hypothesis. In other words, we are confident that there is a significant association between framing conditions and conformity levels. In conclusion, our significant level is lower than the p-value of the study at 7.1%. However, it does not change the fact that there is a signal of the effect of framing conditions on the conformity probability. 


# 3) Challenges when measuring the average causal effect

Causal effect of a binary treatment is defined as the difference between two potential outcomes (with and without the treatment) (Imai, 2018). In addition, an average causal effect (ATE) is defined as the average difference between potential outcomes under different treatment (Schafer & Kang, 2008). As we can’t observe the counterfactuals, the gold standard of classical experiment (randomized controlled trials) is usually preferred with the difference-in-difference estimators to estimate the average causal effect. The randomization guarantees the average outcome between two groups are solely attributed to the treatment, while DiD address the confounding bias due to time trends (Imai, 2018).  

In our case, each viewer group watches only a livestream session having one of two framing conditions. From our understanding, in the experiment of Nan et al. (2023), the choice frame is implemented before one lucky draw in a livestream session, while the rejection frame is implemented before the lucky draw of the other livestream session. The two livestream sessions happen in sequential order.  Given the above experiment design, the following problems would arise: 

1) In our case, a confounding variable may come into play. Simply measuring group differences simultaneously for two groups without measuring treatment effects over time can fail to address the systematic differences in two groups caused by the time varying confounders. For example, the credibility or attractiveness of the brand of bottles used and the influencer might vary significantly between two livestream sessions. Another type of confounders is the different characteristics of two viewer groups (the age distribution) might affect our result. 

2) Another type of confounders is the different characteristics of two viewer groups (the age distribution) might affect our result. As the study tries to randomly assign viewers into two groups, the selection bias (viewers self-select themselves into different groups) might be mitigated. Indeed, the two groups have similar gender distribution and slightly different age distribution. However, since the groups are experimented on at different times in the study, there might be differences in the latent (unobservable) characteristics of these two groups such as different types of viewers having different time using social media. Thus, there could be a selection bias that we fail to account for.

3) Temporal ordering technically does not exist for us here. We don’t test a group before and after treatment, but we measure the difference between two groups exposed to different conditions. This can put a question mark on the cause-and-effect relationship.

4) This study also mentioned that the researchers could not control the time the participants joined. So, if certain participants joined after the framing announcement and still voted, the effect size of the conformity level could be underestimated. 

5) Also, through a social media live-streaming event, it isn't easy to mimic real-world scenarios. It is also challenging to ensure independence within groups between observations. Since respondents can view the choices made by others, there is a chance that they are affected by choices made by others. Thus, it threatens the external validity, which make it difficult to generalize to other scenarios.

<P style="page-break-before: always">

# Reference

Bennett DA. How can I deal with missing data in my study? Aust N Z J Public Health. 2001;25(5):464–469.

Dong, Y., Peng, CY.J. Principled missing data methods for researchers. SpringerPlus 2, 222 (2013). https://doi.org/10.1186/2193-1801-2-222

Nan, L. X., Park, S. K., Yang, Y. (2023). Rejections
Are More Contagious than Choices: How Another’s Decisions Shape Our Own. Journal of Consumer Research, 50(2), 363–381. https://doi.org/10.1093/jcr/ucad007

Imai, K., (2018). Quantitative Social Science: An Introduction. https://press.princeton.edu/books/paperback/9780691175461/quantitative-social-science

Pritha, B. (2023). Missing Data | Types, Explanation, & Imputation. Retrieved from: https://www.scribbr.com/statistics/missing-data/

Schafer J., L. (1999). Multiple imputation: a primer. Stat Methods in Med,
8(1), 3–15. 10.1191/096228099671525676.

Schafer, J. L., & Kang, J. (2008). Average causal effects from nonrandomized studies: A practical guide and simulated example. Psychological Methods, 13(4), 279–313. https://doi.org/10.1037/a0014268

Shaun, T. (2022). Chi-Square (Χ²) Tests | Types, Formula & Examples. Retrieved from: https://www.scribbr.com/statistics/chi-square-tests/
