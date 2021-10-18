---
categories:
- ""
- ""
date: "2017-10-31T21:28:43-05:00"
description: "Omega Group plc- Pay Discrimination"
draft: false
image: Data1.PNG
keywords: ""
slug: Data
title: Data project
---

The objective of this analysis is to find out whether there is a significant difference between the salaries of men and women, and whether the difference is due to discrimination or whether it is based on another, possibly valid, determining factor.

We performed different types of analyses to check whether they all lead to the same conclusion:
* Confidence intervals
* Hypothesis testing
* Correlation analysis
* Regression

For example, we ran a hypothesis testing, assuming as a null hypothesis that the mean difference in salaries is zero, or that, on average, men and women make the same amount of money. We run our hypothesis testing using t.test() and with the simulation method from the infer package. In addition, we calculate the correlation coefficient between gender and salary and perform a linear regression with gender as indepedent and salary as dependent variable.

**Example of code 1**
> **#hypothesis testing using t.test()**  
t.test(salary ~ gender, data=omega)  

>**#hypothesis testing using infer package**  
test_diff <- omega %>%  
specify(salary ~ gender) %>%  
hypothesize(null = "independence") %>%  
generate(reps =1000, type ="permute") %>%  
  calculate(stat = "diff in means",  
            order = c("male", "female"))

>**#calculate difference in means to calculate mean value**  
salary_diff <- omega %>%  
  specify(salary ~ gender) %>%  
  calculate(stat = "diff in means", order = c("male", "female"))  
test_diff %>%   
  get_pvalue(obs_stat = salary_diff,  
             direction = "both")

In the end, we used GGally:ggpairs() to create a beautiful scatterplot and correlation matrix as shown above. Essentially, we changed the order our variables will appear in and had the dependent variable (Y), salary, as last in our list. We then piped the dataframe to ggpairs() with aes arguments to colour by gender and make the plots somewhat transparent (alpha  = 0.3).

**Example of code 2**
>omega %>%   
  dplyr::select(gender, experience, salary) %>% #order variables they will appear in ggpairs()  
  ggpairs(aes(colour=gender, alpha = 0.3))+  
  theme_bw()


**Conclusion**  
The salary vs experience scatterplot infers that there is a positive relationship between the two, meaning that the higher the salary, the higher you can expect the experience to be and vice versa. The scatterplot also shows us the dots colored according to the gender and we can clearly see that the blue dots representing male employees are more to the upper-right whereas the red dots representing female employees are mainly concentrated in the bottom left corner. This shows that male employees have a higher experience than female employees and are also generally paid more. However, looking at the red dot that is the furthest to the right (around 30 years of experience) and comparing it to a similar blue dot, the salary of this employee seems to be rather low compared to that of male employees with a similar level of experience. The same holds true for those employees with experiences between 10 and 20 years where the highest salaries are all paid to men. Therefore, there might still be a certain level of discrimination based on gender. However, more data would be needed in order to verify this statement.
          