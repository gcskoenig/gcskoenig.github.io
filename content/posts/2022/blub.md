---
title: "MCR individualized"
date: 2022-04-16T00:30:03+00:00
# weight: 1
# aliases: ["/first"]
tags: ["first"]
author: "Me"
categories: ['test']
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "In this article, we test whether KaTeX Math typesetting works."
# canonicalURL: "https://canonical.url/to/page"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "<image path/url>" # image path/url
    alt: "<alt text>" # alt text
    caption: "<text>" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: true # only hide on current single page
#editPost:
#    URL: "https://github.com/<path_to_repo>/content"
#    Text: "Suggest Changes" # edit text
#    appendFilePath: true # to append file path to Edit link
---

An action $a$ is $\gamma$-individually meaningful if 

$$P(Y^{post}=1|do(a), x^{pre}) \geq \gamma$$ 

The post-recourse predictor is

$$h^{*, ind}(x^{post}, x^{pre}, a) := P(Y^{post}=1|do(a), x^{pre}, x^{post})$$

Then it holds for $\gamma$-meaningful action $a$ that 

$$\mathbb{E}\left[h^{*, ind}(x^{post}, x^{pre}, a)|x^{pre}, a\right] = \gamma(a, x^{pre})$$

Cool!
