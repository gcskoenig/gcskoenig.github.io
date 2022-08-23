---
title: "If interpretability is the answer, what is the question?"
date: 2022-04-16T00:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["interpretability", "taxonomy"]
author: "Gunnar König"
# categories: ['test']
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

The goal of Interpretable Machine Learning (IML) is to provide human-intelligible descriptions of potentially complex ML models. However, IML descriptors themselves can be difficult to interpret: To reduce complexity, each method is limited to provide insight into *a specific aspect* of model and data. If the method's focus is not clearly understood, IML descriptors can be misinterpreted.

We illustrate the problem on a simple example: Suppose a practitioner aims to learn about the relevance of *in-store temperature* for *sales* at a petrol station. Therefore she fits a model on a sales dataset and computes the Partial Dependence Plot (PDP) for the temperature (Figure 1). 

![Left: the data generating process, the corresponding causal graph. Right: The model's mechanism and the partial dependence plot for temperature.]()

Looking at the PDP output, she decides to turn up the theromstat in order to increase the sales. Her conclusion is not valid: In reality, increasing in-store temperature has a small negative causal effect on the sales.[^1] The reason is that the chosen method, PDP, is designed to provide insight into the model's mechanism, but in general does not allow insight into the effect of real-world actions.[^2]

In this article, we aid practicitioners to chose suitable IML tools and to avoid misinterpretation of the IML descriptor. More specifically, we guide practitioners to

1. carefully choose the aspect of model and data that they are interested in and 
2. to understand which IML descriptors are suitable for their task. 

Therefore we present a taxonomy of the different aspects of model and data that may be of interest ({{< relref "#taxonomy:-aspects-of-model-and-data" >}}) and classify existing established IML methods within that taxonomy ({{< relref "#classification-of-common-iml-techniques" >}}). Thereby we focus on model-agnostic post-hoc interpretability methods that are designed for supervised learning scenarios with tabular data. [We do not distinguish between global/local etc].

[^1]: The learned model includes both the temperature in-store and the temperature back-room. Both variables are strongly correlated; in the model they cancel each other out, such that they do not contribute to the prediction on real-world data. However, for the PDP the correlation between the variables is broken, such that the variable has an effect and we cannot make conclusions about the correlations in the data. Furthermore, correlation is not causation.

[^2]: Insight into the associations in the data is also not possible in this case. In fact there is only a very small association between in-store temperature and sales and it is negative (the PDP suggests the opposite).

# Background and Notation

We denote the optimal prediction model as $f^*$, the estimated model as $\hat{f}$. We suppose that the model was fitted on the covariates $X$ to predict $Y$. The model's prediction is denoted as $\hat{Y}$. Random variables are denoted with capital letters, the respective realizations as $X=x$ or just $x$. 

Conditional dependence is denoted as $X \not \perp Y|Z$ (and independence as $X \perp Y |Z$). Probability distributions are denoted as $P(X)$, event probabilities as $p(x)$. 

To denote causal interventions we make use of the $do$-operator: to indicate that variable $X_j$ is set to value $\theta_j$, we write $do(X_j=\theta_j)$. Furthermore, we assume that the data generating process can be described with a structural causal model (SCM) where the values of the variables are determined by the mutually independent exogenous variables $U_j$, the structural equations $f_j$ and the observation of the causal parents $X_{pa(j)}$, i.e. that  $X_j := f_j(X_{pa(j)}, U_j)$. The SCM induces a causal graph $\mathcal{G}$, where each node is connected with its direct effects with a directed edge. We assume that $\mathcal{G}$ is a directed acyclic graph (DAG).

# Taxonomy: Aspects of Model and Data

As follows we introduce a taxonomy of model and data aspects. Therefore, we differentiate [..] along two axes:

1. Which object do we aim to understand: the model's prediction, the underlying target or their relationship?
2. On what level do we aim to understand: on the level of association, regarding data-level interventions or regarding model-level interventions?

We visualize the differences on a schematic illustration of model in data, presented in Figure [..]. [Explain the element categories of the figure (nodes, arrows, boxes).] [Announce that we will guide through the figure and explain each element step by step].#

![Image with a simple scenario which allows to distinguish all relevant aspects. Use the one from the IML lecture?](/Users/gcsk/Public/gcskoenig.github.io/static/img/posts/taxonomy/taxonomy-overview.jpg)

## What do we aim to understand?

Depending on the interpretation goal, we may either be interested in understanding 

* the model's prediction $\hat{Y}$, 
* the prediction target $Y$,
* the prediction loss $L$, or more generally the relationship between $\hat{Y}$ and $Y$.

### Differentiation

First of all, $Y$ and $\hat{Y}$ are different objects: In general they take different values within the observational distribution (since in general $Y$ cannot be perfectly predicted from the covariates). Furthermore they take different causal roles and as such behave very differently in interventional environments. For example, changing the test result has a causal effect on the Covid prediction, but does not affect whether someone is infected with covid. Clearly, $Y$ and $\hat{Y}$ are different objects than e.g. their loss $L$. For instance, the loss $L$ takes different values than $Y$ or $\hat{Y}$ and also has a different causal role.

### Suitability

We have established that $Y$, $\hat{Y}$ and $R$ are different objects; the question which of the objects is of interest in which scenario remains. The practitioner has to carefully assess which of those is of interest in any situation; we cannot give general recommendations. However, we can roughly categorize as follows.

In scenarios where we trust the model and want to learn from it, e.g. in scientific inference, we typically aim to understand the underlying target $Y$. In these scenarios, the prediction is only a proxy for understanding $Y$.

In scenarios where we are sceptical of the model and the goal is to debug it, we typically aim to understand the prediction $\hat{Y}$. For instance, if the model relies on a protected attribute or a spuriously correlated variable, we would want to expose this, irrespective of whether that relationship is reflected in the underlying data generating process. 

For the relationship it is more nuanced. Is definitely interesting in scenarios where we are sceptical of the model. I.e. if it does not perform so well we may want to assess how the relationship between prediction and target differs between training and test environments. (Or to see which variables are required to achieve a certain performance with a linear model?)

## On what level do we aim to understand?

Not only may we be interested in different objects, but we can also understand each of the objects different levels. We may either be interested in understanding

* how the object is associated with the covariates (statistical dependence)
* how the object is affected by real-world interventions (interventions on light blue variables)
* how the object is affected by model-level interventions (interventions on purple variables)

### Differentiation

These are distinct levels. For instance, suppose that we are interested in the model's prediction. Data-level causation is different from model-level causation: For instance intervening on trust on the model level does not affect the prediction, but intervening on the data-level affects the prediction via the vaccination status. Furthermore causation is different from association: *quarantine* is associated with the prediction, but neither model-level intervention nor data-level intervention on *quarantine* affect the prediction.

### Suitability

It remains an open question when which of the levels is of interest. Typically either data-level causation or association. Data-level causation tells us how the the object of interest behaves if we act in the real-world. Association is helpful if we are only interested in co-occurance (which is e.g. required for prediction). The use of model-level causation is debated; some argue that this is exactly what we want if we are interested in in interpretability since we want to understand the model's mechanism, others argue that interpretation of the model in unrealsitic regions is not helpful at all and that model-level interventions lead to extrapolation into unrealistic environments. 

### Notation

association: statistical quantities, aspects of conditional distributions

causal data-level: $do$-operator statements, interventions on real-world variables

causal model-level: distinguish real-world variables from model inputs $\underline{X}$. Intervention on model inputs.

# Classification of Common IML Techniques

Overview table with the 9 categories. 

|                          | prediction                   | target              | performance                        |
| ------------------------ | ---------------------------- | ------------------- | ---------------------------------- |
| **association**          | M-Plot<br />conditional SHAP | -                   | conditional SAGE<br />CFI<br />ASV |
| **causal (data-level)**  | CR<br />CSVs                 | MCR<br />Hastie PDP | Invariant prediction importance    |
| **causal (model-level)** | CXplain<br />marginal SHAP   | -                   | PFI<br />marginal SAGE             |



# Relationships between categories

In principle mutually exclusive. 
Demonstrate on examples in one figure (e.g. respective global relevance plot for each type in a 3x3 grid), proof in appendix.

**approach 1:** assumptions and their consequences

* statistical independence of covariates
* causal independence of covariates
* observational identifiability of causal effect?
* bayes optimal predictor
* perfect predictor
  

**approach 2:** for each of the 9 classes, list which other classes imply the class and under which assumption they do so

e.g.: causal model-level prediction given ..., associative causal data-level prediction given 

# Use Cases and Application

## Example 1

Scientific model, trust in model, goal is knowledge generation; psychological dataset

* includes causes, effects, other variables

## Example 2

Decision model, trust in prior knowledge, goal is debugging; medical diagnosis

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



# Old text/Notes

**prediction $\hat{Y}$ :** The model's estimate for $Y$. In our example, xy. Can be seen as detached object with its own causal mechanism. Understanding it in order to understand the target must be differentiated from understanding it for its own sake, i.e. for debugging.

**underlying target $Y$:** The underlying prediction target is the real-world state that the model tries to predict. For instance, in May also be referred to as label. In our example . Real world object/entity. The quantity that the model tries to predict. Understanding it is the main inquiry for scientific applications.

**their relationship (e.g. risk $R(Y, \hat{Y})$):** in order to understand how the model is able to estimate the prediction target. potentially provides both insight into prediction and underlying data. however, always provides a filtered perspective of the data via the model and vice versa. in general, interesting for purposes like variable selection.

**association:** preserves dependence structure in the data. model is only evaluated where it was trained. for bayes error models aspect of the conditional distribution. Formally: effect scanning through the observational distribution

**causal data-level:** preserves causal relationships, breaks some dependencies. understand how the model behaves w.r.t. to acting in the real world.

**causal model-level:** only preserves the model's mechanism, but does not repsect causal or dependence structure in the data. regards model as separate entity detached from the world. easy to simulate the effect of these interventions on the prediction.



### Potential extension: Which aspect do we aim to understand?

E.g. main effect, average effect, global, local, interactions, etc.
Quickly explain them an that typically distinguished along that axis



## Mixed Categories

E.g. dedact is a combination
or Causal shapley values paper (direct and indirect)
