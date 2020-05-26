---
title: The Causality-Driven Company
description: Increasingly, enterprises embrace decision-making based on data, thus becoming data-driven companies. However, decisions based purely on data can lead to mistakes and may waste resources. Combining causal models with data promises to fix these issues. In this article I explain why the causality-driven company is the sensible step up from the data-driven company.
hidden: true
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
3. All business units have a data mindset, i.e. are driven by testable hypotheses and have the technological means to do so, and
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

![Smart manufacturing with machine learning](/images/posts/smart_manufacturing_machine_learning.jpg "Figure 1: Production floor indicating production parameters A, B, C, and D alongside resultant quality score QA of items coming off the production line.")

Consider our production line, pictured above in Figure 1, which consists of a number of workstations that the different parts of our product need to pass through before being assembled into our final product.

The various workstations can be described by parameters, e.g. how much pressure or force is applied, how fast a tool is spinning, or categories such as the type of drill used.

Our fictional production line above has four production parameters spread across its workstations: $$A$$, $$B$$, $$C$$, and $$D$$.

At the far end of our production the quality of our assembled product is quantified in the quality assurance gateway ($$\verb!QA!$$).
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

Parameters $$A$$ and $$B$$ of our production line are numerical features while parameters $$C$$ and $$D$$ are categorical features.
Our quality measurement $$\verb!QA!$$ is a numerical target variable which takes values between 0 and 1 with the latter representing the highest possible product quality.

Provided that we collect enough production line data, machine learning offers a rich toolbox for predicting the expected $$\verb!QA!$$ score or whether our $$\verb!QA!$$ score will lie above a certain threshold.
The former application is called regression while the latter is called classification.

Machine learning models at our disposal in our toolbox include:

- Linear and polynomial regression,
- (Deep) neural networks / deep learning,
- Classification and regression trees (CART),
- Ensemble methods such as random forests and gradient tree boosting,
- Logistic regression,
- Support vector machines,
- Gaussian processes, and
- Naive Bayes.

These machine learning models allow us to learn the, potentially, highly complex relationship between production line parameters $$A$$, $$B$$, $$C$$, and $$D$$ and product quality, $$\verb!QA!$$.

These are also the same models used routinely to implement the aforementioned industry applications such as demand forecasting or churn prediction.

So training a machine learning model on our production line data we get something like this:

$$ \verb!QA! = \verb!model!(A, B, C, D) $$.

Where our model predicts the quality of our final product based on the parameters of our production line.

### Using our prediction model

Imagine we manage to train a highly accurate machine learning model on our production line data - what do we do with it?

Sure, if measuring the quality of our product ($$\verb!QA!$$) is prohibitively expensive we could cut cost and just use our model to determine the quality score for items coming off our production line.

We could also try and adapt our model so that it provides accurate quality score predictions early in the production cycle for a given item - thus allowing us to cancel work on substandard items before they consume resources unnecessarily.

But really, what we are most likely interested in is:

How should we tweak individual production parameters to increase product quality?

### Issue with our prediction model

Unfortunately, our machine learning model does not hold the answer to our core question -
out of the box it will not help us optimize individual production parameters to improve our quality score.

Let's see why that is.

We can write our machine learning model as a function:

$$ \verb!QA! = f(A, B, C, D), $$

where $$ f $$ is our model that predicts the quality score for items manufactured with production parameters
$$A$$, $$B$$, $$C$$, and $$D$$.

The issue with this model is that it learns the relationship between quality score and production parameters "in bulk" - or the conditional distribution of $$\verb!QA!$$ jointly on production parameters $$A$$, $$B$$, $$C$$, and $$D$$.

The only sensible operation we can carry out with this model is asking all production line workers for their presently (or planned) production parameter values, inputting them into our model, and informing our colleagues in production of the expected quality score.

Of course, a far more desirable operation would be to take the presently used production parameters, e.g.

$$ (A, B, C, D) = (a, b, c, d),$$

tweak a single parameter, e.g. increase parameter $$B$$ by 10%, and compute the newly expected quality score

$$\verb!QA modified! = f(a, 1.1 \times b, c, d).$$

If the newly predicted $$\verb!QA modified!$$ turns out greater than the presently achieved or predicted quality score we would go back to the production floor and ask our colleague manning the workstation to increase their parameter $$B$$ by 10%.

Unfortunately, this operation of tweaking and optimizing individual input variables (production parameters) is not supported by our machine learning model.

In essence, our model learns the bulk relationship between the entire set of production parameters and quality score, and not the individual impact of each production parameter on the quality score.

This problem pertains to most machine learning models independent of the specific problem tackled.

How do we modify our machine learning model so that we can estimate the impact of individual production parameter modifications and help our colleagues on the production floor make optimal, surgical changes to their manufacturing process?

One major piece to closing this gap is causality.

## The missing piece: causality

### Variables and parameters are often interconnected

To disentangle their individual contributions, we first need to account for how our production parameters influence one another and the quality score.

To understand the connections between the parameters of our production line and the quality score we pop down to the production floor and chat with our colleagues manning the individual workstations.
From them we learn that:

- When the stiffness of the raw material (A) increases then downstream workers increase the pressure applied on the item (B),
- Depending on the stiffness of the raw material (A) the type of downstream drill (C) is chosen,
- The worker choosing the type of drill (C) also informs the worker moulding our item under pressure (B) since they need to balance bore friction and metal stiffness,
- The worker applying a specific type of finish (D) to our product is not affected by the choices of their co-workers.

Armed with these insights from our production floor we draw up the following causal model - showing the connections among our production parameters and the quality score of our product:

![Smart manufacturing with machine learning](/images/posts/smart_manufacturing_causal_inference.jpg "Figure 2: Production line with causal linkes between production parameters indicated by arrows. Production parameters are linked due to practical considerations and the experience of production floor workers.")

### Interconnected variables are hard to disentangle

One of our main uses for a machine learning model that predicts the eventual quality score of manufactured items is to support our colleagues on the production floor in optimizing their individual production parameters to maximize our overall output quality.

However, our causal model (Figure 2) makes it apparent that our model should be a lot more complex that previously thought. The connections in our causal model indicate that we should write our model as

$$ \verb!QA! = f(g(A, h(A)), h(A), D), $$

where both $$B$$ and $$C$$ are functions of $$A$$

$$ B = g(A, C), C = h(A) $$.

In short: In addition to teaching our machine learning model the relationship between our four production parameters and the quality score, we also need to teach our model the various relationships between our production parameters before we can use our model to optimize individual workstations.

Now, we may be inclined to just go down to the production floor and start experimenting at random with our production parameters generating enough data to learn both the interconnections between the production parameters ($$B = g(A, C)$$, $$C = h(A)$$) and their individual impact on our quality score $$\verb!QA!$$.

However, each manufactured item with substandard quality due to a randomly selected combination of production parameters means money lost for your company.

This approach, called randomized experiment or randomized controlled trial, is oftentimes undesirable or simply infeasible:

1. In the case of our production line (and smart manufacturing in general), randomly assigning production parameters to measure their individual impact would be both very time consuming and prohibitively expensive - imagine all the low-quality items made and discarded,
2. In e-commerce, for smart pricing, it is in fact prohibited by law to price the same item differently for different customers in the same market,
3. In customer care it would be counterproductive to treat some customers poorly just to see how their purchase behavior and brand loyalty are affected,
4. In predictive maintenance it would be questionable or prohibited not to perform best practice maintenance steps on individual items to see how they deteriorate differently, and
5. In online marketing it is ofentimes prohibitively complex or infeasible, due to data protection legislation, to cleanly understand the effect (uplift) of isolated marketing parameters on individual users.

Hence, we cannot rely on randomized experiments to understand how to optimally help our production floor colleagues.

So what we are left with is continue doing what we do on the production floor, and use the production data generated under normal operating conditions (observational data) to estimate the impact of each production parameter individually on our quality score.

To achieve this we need to rely on both our causal model (Figure 2) and a toolset from the field of causal inference called do-calculus.

### Causal inference toolset: Do-calculus

The components we have gathered so far are:

- A causal model of our production floor (Figure 2), and
- Lots of observational data of production parameter settings and measured resultant quality score $$\verb!QA!$$.

Let us go through a thought experiment illustrated in the image below (Figure 3): How would the expected quality score $$\verb!QA!$$ change if our colleague in workstation $$B$$ chose their production parameter independently of their colleagues in workstations $$A$$ and $$C$$?

![Smart manufacturing with machine learning](/images/posts/smart_manufacturing_intervention.jpg "Figure 3: Theoretical randomized experiment where we advise our worker in workstation B to apply specific amounts of pressure 'b' instead of aligning their work with workstations A and C. When assigning parameter B independently from parameters A and C the causal links between these parameters is cut as is indicated by the missing arrows.")

This is what the notation in Figure 3 relates to: $$do(B = b)$$ describes a theoretical scenario where production parameter $$B$$ is assigned to hold value $$b$$ - instead of a scenario where parameter $$B$$ is observed to equal value $$b$$.

Note the seemingly subtle but important difference: If we go back to our observational data generated under normal operating conditions then observing $$B = b$$ would imply something about production parameters $$A$$ and $$C$$ since, according to our causal model(Figure 2), these are causally connected!
However, forcing (or intervening on) parameter $$B$$ by setting it to value $$b$$ breaks that causal link between the workstations - and this is what the notation $$\verb!do!(B=b)$$ signifies.

In essence $$\verb!do!(B=b)$$ would mean asking our colleague in workstation $$B$$ to stop listening to their colleagues in workstations $$A$$ and $$C$$.

Since doing so in reality on the production floor would incur high cost due to lost production we simulate the intervention $$\verb!do!(B = b)$$!

To simulate $$\verb!do!(B = b)$$ we apply a set of algebraic rules, called do-calculus, to our causal model from Figure 2 and draw on our regular observational data from the production floor.

In technical terms, do-calculus allows us to turn theoretical interventional distributions into regular conditional distributions that we can compute using our observational data.

With this toolset we can finally estimate the individual effect of tweaking production parameter $$B$$ on quality score $$\verb!QA!$$ thus allowing us to optimize surgically our manufacturing output.

## Becoming a causality-driven company

I hope I managed to convince you of the important difference between recording lots of data and being able to optimize individual steps of your business operations based on those data.

The former implies being able to predict, and optimize, business outcomes if the web of decisions or variables leading to those outcomes are operated as usual - this is the essence of the data-driven company.

The latter implies being able to predict the impact of changes to the status quo of your business decisions and operations.
This enables you to analyze what would happen if you changed "the way you have always done things" - thus elevating you to a causality-driven company.

How can you start using causal inference and do-calculus to optimize your business - and how do you eventually become a causality-driven company?

I would start with becoming a data-driven company first:

- Set up data collection across all key operations of your business,
- Start working on machine learning prototypes for predicting key business metrics based on your collected data, and
- Start asking yourself: Once I can predict my business metrics with machine learning, what specific changes should I make to my business to optimize these metrics?

Once you get to the point of wanting to make surgical decisions based on your machine learning model, causality and causal inference will start playing key roles.

<a href="/images/posts/evolution_causality_driven_company.png" target="_blank">
![Evolution of the causality-driven company](/images/posts/evolution_causality_driven_company.png "Figure 4: The evolution of the causality-driven company. Companies that can quantify their key operations are driven by data - companies that are able to surgically optimize their operations based both on data and a causal understanding of their operations are causality-driven. (Illustration based on Christoper Penn's illustration of the evolution of the data-driven company).")
</a>