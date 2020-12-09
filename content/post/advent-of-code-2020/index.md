---
output: hugodown::md_document
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Advent of Code 2020"
subtitle: ""
summary: "My attempts at [Advent of Code](https://adventofcode.com)."
authors: []
tags: []
categories: []
date: 2020-12-09
lastmod: 2020-12-09
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
rmd_hash: 5cbb1c4009279fdf

---

[Advent of Code](https://adventofcode.com) is a series of small programming challenges, released daily throughout December in the run-up to Christmas. They are designed to be solved in any programming language. I will be using R.

There will no doubt be a wide variety of ways of solving these problems. I'm going to go with the first thing I think of that gets an answer. In most cases, I image there will be more efficient solutions.

Each participant gets different input data, so my numerical solutions may be different from others.

1.  <a href="#day1">Report repair</a>

<p>
<a id='day1'></a>
</p>

Day 1: [Report Repair](https://adventofcode.com/2020/day/1)
-----------------------------------------------------------

The challenge is to find two numbers from a list that sum to 2020, then to report their product.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)

<span class='k'>expenses</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/readLines.html'>readLines</a></span>(<span class='s'>"AoC_day1.txt"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://rdrr.io/r/base/numeric.html'>as.numeric</a></span>()

<span class='nf'><a href='https://rdrr.io/r/base/expand.grid.html'>expand.grid</a></span>(<span class='k'>expenses</span>, <span class='k'>expenses</span>) <span class='o'>%&gt;%</span> 
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(sum = <span class='k'>Var1</span> <span class='o'>+</span> <span class='k'>Var2</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>sum</span> <span class='o'>==</span> <span class='m'>2020</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(prod = <span class='k'>Var1</span> <span class='o'>*</span> <span class='k'>Var2</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/slice.html'>slice</a></span>(<span class='m'>1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>prod</span>)
<span class='c'>#&gt; [1] 926464</span></code></pre>

</div>

The follow-up challenge is the same but with three numbers. I went with essentially the same code but it's notably slower. There are a lot of repeated calculations here: each triplet appears six times in the table.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/expand.grid.html'>expand.grid</a></span>(<span class='k'>expenses</span>, <span class='k'>expenses</span>, <span class='k'>expenses</span>) <span class='o'>%&gt;%</span> 
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(sum = <span class='k'>Var1</span> <span class='o'>+</span> <span class='k'>Var2</span> <span class='o'>+</span> <span class='k'>Var3</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>sum</span> <span class='o'>==</span> <span class='m'>2020</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(prod = <span class='k'>Var1</span> <span class='o'>*</span> <span class='k'>Var2</span> <span class='o'>*</span> <span class='k'>Var3</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/slice.html'>slice</a></span>(<span class='m'>1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>prod</span>)
<span class='c'>#&gt; [1] 65656536</span></code></pre>

</div>

