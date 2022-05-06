---
title: "If interpretability is the answer, what is the question?"
date: 2022-04-16T00:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["first"]
author: "Me"
categories: ['test']
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: true
hidemeta: false
comments: false
description: "I propose a taxonomy of the types of questions that IML methods can answer."
# canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
#cover:
#    image: "<image path/url>" # image path/url
#    alt: "<alt text>" # alt text
#    caption: "<text>" # display caption under cover
#    relative: false # when using page bundles set this to true
#    hidden: true # only hide on current single page
#editPost:
#    URL: "https://github.com/<path_to_repo>/content"
#    Text: "Suggest Changes" # edit text
#    appendFilePath: true # to append file path to Edit link
---

# Introduction

Focus

* model-agnostic post-hoc interpretability
* supervised learning scenarios
* tabular data

# Background and Notation

* target variable $Y$, covariates $Y$, model $\hat{f}$, prediction $\hat{Y}$
* conditional (in)dependence $X \not \perp Y | Z$, probability distribution $P(X)$, event probability $p(x)$
* do operator, graphical models
* structural causal models

# Exemplary Use Cases

## Example 1

Scientific model, trust in model, goal is knowledge generation

## Example 2

Decision model, trust in prior knowledge, goal is debugging

# Dimensions of Model-Agnostic Post-Hoc Interpretability

Figure with all elements, i.e. data-level variables, model-level variables, target, prediction, performance. causal relationship $\hat{Y} \rightarrow$ data level?

## What do we aim to understand?

**underlying target:** May also be referred to as label. In our example, xy. Real world object/entity. The quantity that the model tries to predict. Understanding it is the main inquiry for scientific applications.

**prediction:** The model's estimate of $Y$. In our example, xy. Can be seen as losgelöstes object with its own causal mechanism. Understanding it in order to understand the target must be differentiated from understanding it for its own sake, i.e. for debugging.

**their relationship (model performance):** in order to understand how the model is able to estimate the prediction target. potentially provides both insight into prediction and underlying data. however, always provides a filtered perspective of the data via the model and vice versa. in general, interesting for purposes like variable selection.

## On what level do we aim to understand?

**association:** preserves dependence structure in the data. model is only evaluated where it was trained. for bayes error models aspect of the conditional distribution. Formally: effect scanning through the observational distribution

**causal data-level:** preserves causal relationships, breaks some dependencies. understand how the model behaves w.r.t. to acting in the real world.

**causal model-level:** only preserves the model's mechanism, but does not repsect causal or dependence structure in the data. regards model as separate entity detached from the world. easy to simulate the effect of these interventions on the prediction.

## Potential extension: Which aspect do we aim to understand?

E.g. main effect, average effect, global, local, interactions, etc.
Quickly explain them an that typically distinguished along that axis

## Classification of Common IML Techniques

Overview table with the 9 categories. 

|                          | prediction                   | target              | performance                        |
| ------------------------ | ---------------------------- | ------------------- | ---------------------------------- |
| **association**          | M-Plot<br />conditional SHAP | -                   | conditional SAGE<br />CFI<br />ASV |
| **causal (data-level)**  | CR<br />CSVs                 | MCR<br />Hastie PDP | Invariant prediction importance    |
| **causal (model-level)** | CXplain<br />marginal SHAP   | -                   | PFI<br />marginal SAGE             |

## Mixed Categories

E.g. dedact is a combination
or Causal shapley values paper (direct and indirect)

# Relationships between categories

In principle mutually exclusive. 
Demonstrate on examples in one figure (e.g. respective global relevance plot for each type in a 3x3 grid), proof in appendix.

**approach 1:** assumptions and their consequences

* statistical independence of covariates
* causal independence of covariates
* observational identifiability of causal effect?
* bayes optimal predictor
* perfect predictor
  

**approach 2:** for each of the 9 classes, list which other classes implay the class and under which assumption they do so

e.g.: causal model-level prediction given ..., associative causal data-level prediction given 

# Use Cases and Application

## Use Case 1

## Use Case 2

# Discussion


consequences for development of iml: not suitable for many questions?
- why interpret the data through a model?
- when is refitting more important

consequences for interpretation: many methods are applied incorrectly?

# Limitations

The taxonomy is not comprehensive. I.e. distinguish local/global, also does not include methods not fitting the focus (non tabular, model specific, surrogate models)

# Related Work

interpreting the model vs interpreting the data paper
janzing feature relevance quantification

# Conclusion

bli bla blub
