---
layout: distill
title: Evaluating In-Context Learning for Eliciting Preferences  
description: How well can image embedding models learn human preferences in-context?
giscus_comments: true
tags: ai
date: 2023-03-27

authors:
  - name: Kushal Thaman
    url: "https://kushalthaman.github.io/"
    affiliations:
      name: Stanford University
  - name: Dhruv Pai
    url: "https://mesaoptimized.substack.com/"
    affiliations:
      name: Stanford University
  - name: Nishan Srishankar
    url: "https://nsrishankar.github.io//"
    affiliations:
      name: Stanford University
  - name: Zachary Robertson
    url: "https://zrobertson466920.github.io/about/"
    affiliations:
      name: Stanford University

bibliography: 2023-03-27-incontext.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Introduction 
  - name: Hard-coded Transformers
  - name: Learned Transformers
  - name: Conclusion

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }

---

## Introduction
### Cooperative Inverse Reinforcement Learning

One solution often presented to the alignment problem is a Cooperative Inverse Reinforcement Learning (CIRL) game. A CIRL game is a cooperative game where a machine agent and a human seek to accomplish some tasks, and are rewarded in accordance with the human's reward function. <d-cite>https://arxiv.org/pdf/1606.03137.pdf</d-cite> The machine agent initially does not know this reward function, and seeks to learn it throughout the game. Thus, for the agent, the objective of maximizing self-reward is intrinsically linked to maximizing reward for humans. In such a game, the model is incentivized to preserve the option value between two decisions if presented with an uncertainty, and truly attempt to understand the human's reward function before giving up the option value.  

Compared to standard Inverse Reinforcement Learning, CIRL offers two key advantages. First, it encourages optimizing for the human's reward function as opposed to inferring the reward from the observed behavior of the agent. Equivalently, it prevents the machine agent from adopting a reward function as its own. The second advantage is that it allows the robot to fulfill a human reward function better than a human can. 

One example of a CIRL game is the problem of determining the human's decision threshold. In a classification task, a model is often queried for a decision among multiple options. For a binary classification problem, the threshold determines the minimum confidence required in the 'positive' class for a positive decision output. The decision threshold is often directly relevant to the real-world application of classification results, and requires a careful balancing of Type-I (false positive) and Type-II (false negative) classification errors. The tradeoff between the precision and recall encoded in learning the human's decision threshold can be framed as a CIRL game. 

### In-Context Learning

In this post, we consider meta-learning that occurs through OpenAI's image-embedding model CLIP <d-cite>https://openai.com/research/clip</d-cite> learn human preferences. As an example, inferring what ratio of Type-I or Type-II error a human prefers over the training protocol is an example of a simple CLIP meta-learning scheme. When the model generates softmax probabilities over the two classes, the question still remains of what the final decision should be. Learning preferences in error distributions in turn influences the decision threshold for the final output.

One could frame a CIRL game as a problem in explicit or implicit Bayesian inference. In the former case, the agent explicitly models a probability distribution over actions that would maximize the human's reward and updates its priors based on the reward received. One might consider a CIRL game as a problem in implicit Bayesian inference <d-cite>https://arxiv.org/abs/2111.02080</d-cite>. The model learns latent concepts through its training data, and if a particular distribution of latent concepts is repeatedly reinforced in the data then it is better understood by the model. Insofar as in-context learning uses these latent concepts for task-specific performance, it can be framed as an implicit Bayesian inference scheme.

In-context learning is a phenomenon recently observed among large language models based on transformer architecture. Implicit inference occurs in these models when trained on input and output distributions without any specific task. However, when a text classification task is given for example, the model is an example to perform highly-accurate inference and classification without having actually learned the task itself. Our current mechanistic understanding of the phenomenon suggests that the model actually learns concepts for how to do this task somewhere in latent space, and these latent variables are grouped together to solve specific in-context tasks.

We seek to explore how transformer models can perform in-context learning in order to achieve good task performance on a binary classification problem.  

***

## Hard-coded Transformers

Just wrap the text you would like to show up in a footnote in a `<d-footnote>` tag.
The number of the footnote will be automatically generated.<d-footnote>This will become a hoverable footnote.</d-footnote>

***
