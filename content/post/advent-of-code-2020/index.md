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
rmd_hash: 9a91b5660ad6c692

---

[Advent of Code](https://adventofcode.com) is a series of small programming challenges, released daily throughout December in the run-up to Christmas. They are designed to be solved in any programming language. I will be using R.

There will no doubt be a wide variety of ways of solving these problems. I'm going to go with the first thing I think of that gets an answer. In most cases, I image there will be more efficient solutions.

Each participant gets different input data, so my numerical solutions may be different from others.

1.  <a href="#day1">Report repair</a>
2.  <a href="#day2">Password philosophy</a>

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

`expand.grid` creates a data frame from all combinations of the supplied vectors. Since the vectors are the same, each pair is duplicated. In this case the two numbers in the list that sum to 2020 are 704 and 1316, and we have one row with 704 as Var1 and one with 704 as Var2. [`slice(1)`](https://dplyr.tidyverse.org/reference/slice.html) takes the first occurence of the pair.

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

<p>
<a id='day2'></a>
</p>

Day 2: [Password philosophy](https://adventofcode.com/2020/day/2)
-----------------------------------------------------------------

We need to find how many passwords are valid according to their policy. The policies and passwords are given as follows:

    1-3 a: abcde
    1-3 b: cdefg
    2-9 c: ccccccccc

Each line gives the password policy and then the password. The password policy indicates the lowest and highest number of times a given letter must appear for the password to be valid. For example, `1-3 a` means that the password must contain `a` at least 1 time and at most 3 times.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)
<span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://tidyr.tidyverse.org'>tidyr</a></span>)
<span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='http://stringr.tidyverse.org'>stringr</a></span>)</code></pre>

</div>

First load the libraries we'll need. We then read in the data and use `tidyr` functions to separate out the parts of the policy and the password:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>passwords</span> <span class='o'>&lt;-</span> <span class='k'>readr</span>::<span class='nf'><a href='https://readr.tidyverse.org/reference/read_delim.html'>read_tsv</a></span>(<span class='s'>"AoC_day2.txt"</span>, col_names = <span class='kc'>FALSE</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/separate.html'>separate</a></span>(<span class='k'>X1</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>"policy"</span>, <span class='s'>"password"</span>), sep = <span class='s'>":"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/separate.html'>separate</a></span>(<span class='k'>policy</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>"count"</span>, <span class='s'>"letter"</span>), sep = <span class='s'>" "</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/separate.html'>separate</a></span>(<span class='k'>count</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>"min"</span>, <span class='s'>"max"</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(min = <span class='nf'><a href='https://rdrr.io/r/base/integer.html'>as.integer</a></span>(<span class='k'>min</span>),
         max = <span class='nf'><a href='https://rdrr.io/r/base/integer.html'>as.integer</a></span>(<span class='k'>max</span>))</code></pre>

</div>

Next, we use the `stringr` function `str_count` to count how many times the given letter appears in the password, and conditional logic to check whether it is repeated within the specified number of times. Because `TRUE` has a numeric value of 1 and `FALSE` has a numeric value of 0, we can sum the resulting column to get a count of how many passwords are valid according to their policies.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>passwords</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(count = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_count.html'>str_count</a></span>(<span class='k'>password</span>, <span class='k'>letter</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(password_in_policy = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='k'>count</span> <span class='o'>&gt;=</span> <span class='k'>min</span> <span class='o'>&amp;</span> <span class='k'>count</span> <span class='o'>&lt;=</span> <span class='k'>max</span>, <span class='kc'>TRUE</span>, <span class='kc'>FALSE</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(correct = <span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>password_in_policy</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>correct</span>)
<span class='c'>#&gt; [1] 625</span></code></pre>

</div>

Part 2 to follow soon...

