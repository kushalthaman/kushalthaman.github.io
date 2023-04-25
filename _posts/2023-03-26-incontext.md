---
layout: distill
title: Evaluating In-Context Learning for Eliciting Preferences  
description: How well do image embedding models learn human preferences in-context?
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

One solution often presented to the alignment problem is a Cooperative Inverse Reinforcement Learning (CIRL) game. A CIRL game is a cooperative game where a machine agent and a human seek to accomplish some tasks, and are rewarded in accordance with the human's reward function. <d-cite>1</d-cite> The machine agent initially does not know this reward function, and seeks to learn it throughout the game. Thus, for the agent, the objective of maximizing self-reward is intrinsically linked to maximizing reward for humans. 

Compared to standard Inverse Reinforcement Learning, it offers two key advantages. First, it encourages optimizing for the human as opposed to  as the human. Equivalently, it prevents the robot from adopting a reward function as its own. The second advantage is that it allows the robot to fulfill a human reward function better than a human can. Equivalently, it does not assume human behavior is the best teacher for accomplishing a particular task. 

One example of a CIRL game is the decision thresholding problem. In a classification task, a model is often queried for a decision among options. For a binary classification problem, this threshold determines the minimum confidence required in the positive class for a positive decision output. The decision threshold is often directly relevant to the real-world application of classification results, and requires a careful balancing of type-1 and type-2 classification errors. The tradeoff between precision and recall encoded in decision threshold learning can be framed as a CIRL game. 

The meta-learning that occurs through OpenAI's image-embedding model 'CLIP' is to learn human preferences. As an example, inferring what ratio of type-1 v/s type-2 error a human prefers over training protocol is an example of a simple CLIP meta-learning scheme. When the model generates softmax probabilities over the two classes, the question still remains of what the final decision should be. Learning preferences in error distributions by the human trainer in turn influence the decision threshold for the final output.

Previous CIRL games have framed the problem as explicit Bayesian inference. In that case, the agent explicitly models probability distributions over actions that would maximize the human's reward and updates priors based on the reward received. The implicit framing arises as a result of in-context learning. The model learns latent concepts through its training data, and if a particular distribution of latent concepts is repeatedly reinforced in the data then it is better understood by the model. Insofar as in-context learning uses these latent concepts for task-specific performance, it can be framed as an implicit Bayesian inference scheme.

In-context learning is a phenomenon recently observed among large language models based on transformer architecture. Implicit inference occurs in these models when trained on input and output distributions without any specific task. However, when a text classification task is given for example, the model is an example to perform highly-accurate inference and classification without having actually learned the task itself. Our current mechanistic understanding of the phenomenon suggests that the model actually learns concepts for how to do this task somewhere in latent space, and these latent variables are grouped together to solve specific in-context tasks.

We seek to explore how transformer models can perform in-context learning in order to achieve good task performance on a binary classification problem.  

***

## Hard-coded Transformers

Just wrap the text you would like to show up in a footnote in a `<d-footnote>` tag.
The number of the footnote will be automatically generated.<d-footnote>This will become a hoverable footnote.</d-footnote>

***

## Code Blocks

Syntax highlighting is provided within `<d-code>` tags.
An example of inline code snippets: `<d-code language="html">let x = 10;</d-code>`.
For larger blocks of code, add a `block` attribute:

<d-code block language="javascript">
  var x = 25;
  function(x) {
    return x * x;
  }
</d-code>

**Note:** `<d-code>` blocks do not look good in the dark mode.
You can always use the default code-highlight using the `highlight` liquid tag:

{% highlight javascript %}
var x = 25;
function(x) {
  return x * x;
}
{% endhighlight %}

***

## Interactive Plots

You can add interative plots using plotly + iframes :framed_picture:

<div class="l-page">
  <iframe src="{{ '/assets/plotly/demo.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>

The plot must be generated separately and saved into an HTML file.
To generate the plot that you see above, you can use the following code snippet:

{% highlight python %}
import pandas as pd
import plotly.express as px
df = pd.read_csv(
  'https://raw.githubusercontent.com/plotly/datasets/master/earthquakes-23k.csv'
)
fig = px.density_mapbox(
  df,
  lat='Latitude',
  lon='Longitude',
  z='Magnitude',
  radius=10,
  center=dict(lat=0, lon=180),
  zoom=0,
  mapbox_style="stamen-terrain",
)
fig.show()
fig.write_html('assets/plotly/demo.html')
{% endhighlight %}

***

## Layouts

The main text column is referred to as the body.
It is the assumed layout of any direct descendants of the `d-article` element.

<div class="fake-img l-body">
  <p>.l-body</p>
</div>

For images you want to display a little larger, try `.l-page`:

<div class="fake-img l-page">
  <p>.l-page</p>
</div>

All of these have an outset variant if you want to poke out from the body text a little bit.
For instance:

<div class="fake-img l-body-outset">
  <p>.l-body-outset</p>
</div>

<div class="fake-img l-page-outset">
  <p>.l-page-outset</p>
</div>

Occasionally you’ll want to use the full browser width.
For this, use `.l-screen`.
You can also inset the element a little from the edge of the browser by using the inset variant.

<div class="fake-img l-screen">
  <p>.l-screen</p>
</div>
<div class="fake-img l-screen-inset">
  <p>.l-screen-inset</p>
</div>

The final layout is for marginalia, asides, and footnotes.
It does not interrupt the normal flow of `.l-body` sized text except on mobile screen sizes.

<div class="fake-img l-gutter">
  <p>.l-gutter</p>
</div>

***

## Other Typography?

Emphasis, aka italics, with *asterisks* (`*asterisks*`) or _underscores_ (`_underscores_`).

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~

1. First ordered list item
2. Another item
⋅⋅* Unordered sub-list.
1. Actual numbers don't matter, just that it's a number
⋅⋅1. Ordered sub-list
4. And another item.

⋅⋅⋅You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

⋅⋅⋅To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅
⋅⋅⋅Note that this line is separate, but within the same paragraph.⋅⋅
⋅⋅⋅(This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)

* Unordered list can use asterisks
- Or minuses
+ Or pluses

[I'm an inline-style link](https://www.google.com)

[I'm an inline-style link with title](https://www.google.com "Google's Homepage")

[I'm a reference-style link][Arbitrary case-insensitive reference text]

[I'm a relative reference to a repository file](../blob/master/LICENSE)

[You can use numbers for reference-style link definitions][1]

Or leave it empty and use the [link text itself].

URLs and URLs in angle brackets will automatically get turned into links.
http://www.example.com or <http://www.example.com> and sometimes
example.com (but not on Github, for example).

Some text to show that the reference links can follow later.

[arbitrary case-insensitive reference text]: https://www.mozilla.org
[1]: http://slashdot.org
[link text itself]: http://www.reddit.com

Here's our logo (hover to see the title text):

Inline-style:
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

Reference-style:
![alt text][logo]

[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 2"

Inline `code` has `back-ticks around` it.

```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

```python
s = "Python syntax highlighting"
print s
```

```
No language indicated, so no syntax highlighting.
But let's throw in a <b>tag</b>.
```

Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the
raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.


Here's a line for us to start with.

This line is separated from the one above by two newlines, so it will be a *separate paragraph*.

This line is also a separate paragraph, but...
This line is only separated by a single newline, so it's a separate line in the *same paragraph*.

