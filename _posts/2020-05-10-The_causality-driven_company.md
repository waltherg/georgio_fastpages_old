---
title: The Causality-Driven Company
description: Increasingly, enterprises embrace decision-making based on data, thus becoming a data-driven company. However, decisions based purely on data can lead to mistakes and may waste resources. Combining causal models with data promises to fix these issues. In this article I explain why the causality-driven company is the sensible step up from the data-driven company.
permalink: /the_causality-driven_company
toc: true
comments: true
layout: post
author: Georg Walther
categories: [causal inference, causal discovery, data-driven company, causality-driven company]
image: images/posts/evolution_causality_driven_company.png
---

## The data-driven company

Much has been written on data being the new gold and enterprises needing to embrace big data and machine learning, enabling enterprises in three important ways:

1. Utilizing existing resources more efficiently thus reducing cost,
2. Increasing success rates thus increasing revenue, and
3. Developing entirely new business models thus diversifying their business.

Enterprises are said to be data-driven once their entire organization is data-first, meaning:

1. Both their core processes (e.g. production lines) and supporting processes (e.g. recruiting) are quantifiable,
2. Business and unit performance is quantifiable with transparent metrics,
3. All business units have a data mindset, i.e. are driven by testable hypothesis and have the technological means to do so, and
4. The entire organization, top to bottom, embraces measurability and hypothesis-driven progress.

## Machine learning applications for the data-driven company

There a numerous applications of big data and machine learning in industry and the enterprise. Common applications include:

- Predictive maintenance,
- Demand forecasting,
- Optimal pricing,
- Intelligent routing,
- Data-driven customer support (e.g. chatbots), amd
- Churn prediction.

Another key application in industry is smart manufacturing which promises to decrease cost and increase both production line output and quality.

## Smart manufacturing

### Product quality prediction

![Smart manufacturing with machine learning](/images/posts/smart_manufacturing_machine_learning.jpg)

Consider our production line, pictured above, which consists of a number of workstations that the different parts of our product need to pass through before being assembled into our final product.

The various workstations can be described by parameters, e.g. how much pressure or force is applied, how fast a tool is spinning, or categories such as the type of drill used.

Our fictional production line above has four production parameters spread across its workstations: A, B, C, and D.

At the far end of our production the quality of our assembled product is quantified in the quality assurance gateway (QA).
Here, either an employee or a machine visually inspects or physically tests our product to gauge its quality.

Since our company is data-driven we collect data on every item coming off our production line. Such a data set may look as follows:

| Product ID | Timestamp | A | B | C | D | QA|
| ---------- |:---------:|:-:|:-:|:-:|:-:|:-:|
| ... | ... | ... | ... | ... | ... | ... |
| 123 | 2020-05-10 14:35:00 | 1.2 | 102.1 | ZZ | YY | 0.98 |
| 456 | 2020-05-10 16:20:00 | 1.1 | 104.3 | ZZ | XX | 0.96 |
| 789 | 2020-05-11 06:04:00 | 1.3 | 100.6 | ZX | YY | 0.83 |
| 012 | 2020-05-11 10:45:00 | 1.1 | 101.3 | ZX | YZ | 0.92 |
| ... | ... | ... | ... | ... | ... | ... |

Parameters A and B of our production line are numerical features while parameters C and D are categorical features.
Our quality measurement QA is a numerical target variable which takes values between 0 and 1 with the latter representing the highest possible product quality.

Provided that we collect enough production line data, machine learning offers a rich toolbox for predicting either the QA score or whether it our QA score will lie above a certain threshold.
The former case is called regression while the latter is called classification.

Machine learning models at our disposal include:

- Linear and polynomial regression,
- (Deep) neural networks / deep learning,
- Classification and regression trees (CART),
- Ensemble methods such as random forests and gradient tree boosting,
- Logistic regression,
- Support vector machines,
- Gaussian processes, and
- Naive Bayes.

These machine learning models allow us to learn the, potentially, highly complex relationship between production line parameters A, B, C, and D and product quality, QA.

So training a machine learning on our production line data we get something like this:

QA = model(A, B, C, D)

Where our model predicts the quality of our final product based on the parameters of our production line.

### Using our prediction model

Imagine we manage to train a highly accurate machine learning model on our production line that - what do we do with it?

Sure, if measuring the quality of our product (QA) is prohibitively expensive we could cut cost and just use our model to determine the quality score for items coming off our production line.

We could also try and adapt our model so that it provides accurate quality score predictions early in the production cycle for a given item - thus allowing us to cancel substandard items before they consume resources unnecessarily.

But really, what we are most likely interested in is: ***How should I tweak individual production parameters to increase product quality?***

### Issue with our prediction model

Unfortunately, our machine learning model does not allow us to answer our core question.
Our machine learning model does not know how changing one individual production parameter affects the quality of our product.

Even though we write our model as

QA = model(A, B, C, D)

what this really means is

QA = model(complete set of production line parameters).

So imagine the parameters of our production line are set to the following values:

A = a, B = b, C = c, and D = d or parameter set (a, b, c, d).

Now if we wanted to predicted the quality score of our product with parameter A increased by 10%, i.e.

(1.1 x a, b, c, d)

then we would need to hope that we already produced an item with parameters very similar to all four target parameter values, i.e. (1.1 x a, b, c, d), otherwise our model would provide us with, possibly, widely inaccurate predictions of our quality score.

In essence: ***Our machine learning model learns the relationship between the entire set of production parameters and quality score - hence our model does not learn the individual contribution of each production parameter on the quality score.***

This issue pertains to all machine learning models: They learn the bulk - not individual - contribution of all observable input variables (or features) to the target value.

## Missing piece: Causality

### Issues arising from causal dependence

![Smart manufacturing with machine learning](/images/posts/smart_manufacturing_causal_inference.jpg)

A major problem with the "bulk contributions" that machine learning models learn is the fact that oftentimes the variables we measure to predict target variables influence one another.

Consider our fictional production line again, however we now draw arrows between our four production parameters that indicate how these parameters influence one another.

For instance, parameter A influences parameters B and C: Imagine parameter A is the temperature some metal component is moulded at thus influencing the amount of pressure that needs to be applied on it in workstation B and the type of drill required to bore a hole into it in workstation C.

The causal model of our fictional production line showcases a number of ways that our expectation of machine learning models fail:

- Parameter A does not have any direct impact on our target variable QA thus bearing the question whether modifying A in isolation is worthwhile our resources,
- Parameter A influences QA via three indirect routes (A -> C -> QA, A -> C -> B -> QA, and A -> B -> QA) thus making it highly likely that changing A in isolation has highly complex and opaque impact on QA making it hard for us to understand how to adjust A for optimal QA,
- Parameter C influences our target QA both directly and indirectly (via B) thus begging the question whether the direct or the indirect route has greater influence.

### Causal inference toolset

Causal inference offers a toolset that helps us disentangle causal structures and understand the impact of individual input variables: Do-Calculus.

Do-calculus encompasses a set of rules that allow us to ask questions such as: "How will the quality score QA of my product change if I increase production parameter A by 10%".

QA = model(do(A = 1.1 x a), b, c, d)

### Nomenclature of causality

Causal model, causal inference, causal discovery

### Common pitfalls uncovered by causality considerations

## Becoming a causality-driven company

[![Evolution of the causality-driven company](/images/posts/evolution_causality_driven_company.png)](/images/posts/evolution_causality_driven_company.png)