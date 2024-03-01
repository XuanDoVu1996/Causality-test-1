# Project: Causality test - Livestream case study

# General information
Git-hub repository at:
https://github.com/XuanDoVu1996/Causality-test-1.git

- Markdown: **Causality test.Qmd**
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
