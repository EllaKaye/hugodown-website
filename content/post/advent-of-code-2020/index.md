---
output: hugodown::md_document
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Advent of Code 2020"
subtitle: ""
summary: "My attempts at [Advent of Code](https://adventofcode.com)."
authors: []
tags: ["R", "rstats", "code"]
categories: ["R"]
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
rmd_hash: 0af13bed7b60a71b

---

[Advent of Code](https://adventofcode.com) is a series of small programming challenges, released daily throughout December in the run-up to Christmas. Part 1 of the challenge is given first. On its successful completion, Part 2 is revealed. The challenges are designed to be solved in any programming language. I will be using R.

There will no doubt be a wide variety of ways to solve these problems. I'm going to go with the first thing I think of that gets the right answer. In most cases, I expect that there will be more concise and efficient solutions. Most of the time I'm working in R, it's within the [tidyverse](https://www.tidyverse.org), so I imagine that framework will feature heavily below.

Each participant gets different input data, so my numerical solutions may be different from others. If you're not signed up for Advent of Code yourself, but want to follow along with my data, you can download it at from the data links at the beginning of each day's section. The links in the day section headers take you to challenge on the Advent of Code page. The links directly below are a table of contents for this page.

1.  <a href="#day1">Report Repair</a>
2.  <a href="#day2">Password Philosophy</a>
3.  <a href="#day3">Toboggan Trajectory</a>
4.  <a href="#day4">Passport Processing</a>
5.  <a href="#day5">Binary Boarding</a>
6.  <a href="#day6">Custom Customs</a>
7.  <a href="#day7">Handy Haverstocks</a>
8.  <a href="#day8">Handheld Halting</a>
9.  <a href="#day9">Encoding Error</a>
10. <a href="#day10">Adapter Array</a>

<p>
<a id='day1'></a>
</p>

Day 1: [Report Repair](https://adventofcode.com/2020/day/1)
-----------------------------------------------------------

[My day 1 data](https://ellakaye.rbind.io/post/advent-of-code-2020/data/AoC_day1.txt)

#### Part 1: Two numbers

The challenge is to find two numbers from a list that sum to 2020, then to report their product.

[`expand.grid()`](https://rdrr.io/r/base/expand.grid.html) creates a data frame from all combinations of the supplied vectors. Since the vectors are the same, each pair is duplicated. In this case the two numbers in the list that sum to 2020 are 704 and 1316, and we have one row with 704 as Var1 and one with 704 as Var2. [`slice(1)`](https://dplyr.tidyverse.org/reference/slice.html) takes the first occurrence of the pair.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)

<span class='k'>expenses</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/readLines.html'>readLines</a></span>(<span class='s'>"data/AoC_day1.txt"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://rdrr.io/r/base/numeric.html'>as.numeric</a></span>()

<span class='nf'><a href='https://rdrr.io/r/base/expand.grid.html'>expand.grid</a></span>(<span class='k'>expenses</span>, <span class='k'>expenses</span>) <span class='o'>%&gt;%</span> 
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(sum = <span class='k'>Var1</span> <span class='o'>+</span> <span class='k'>Var2</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>sum</span> <span class='o'>==</span> <span class='m'>2020</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(prod = <span class='k'>Var1</span> <span class='o'>*</span> <span class='k'>Var2</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/slice.html'>slice</a></span>(<span class='m'>1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>prod</span>)
<span class='c'>#&gt; [1] 926464</span></code></pre>

</div>

#### Part 2: Three numbers

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

Day 2: [Password Philosophy](https://adventofcode.com/2020/day/2)
-----------------------------------------------------------------

[My day 2 data](https://ellakaye.rbind.io/post/advent-of-code-2020/data/AoC_day2.txt)

#### Part 1: Number of letters

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

First load the libraries we'll need. We then read in the data and use `tidyr` functions to separate out the parts of the policy and the password, making sure to convert the columns to numeric as appropriate:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>passwords</span> <span class='o'>&lt;-</span> <span class='k'>readr</span>::<span class='nf'><a href='https://readr.tidyverse.org/reference/read_delim.html'>read_tsv</a></span>(<span class='s'>"data/AoC_day2.txt"</span>, col_names = <span class='kc'>FALSE</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/separate.html'>separate</a></span>(<span class='k'>X1</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>"policy"</span>, <span class='s'>"password"</span>), sep = <span class='s'>":"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/separate.html'>separate</a></span>(<span class='k'>policy</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>"count"</span>, <span class='s'>"letter"</span>), sep = <span class='s'>" "</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/separate.html'>separate</a></span>(<span class='k'>count</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>"min"</span>, <span class='s'>"max"</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(min = <span class='nf'><a href='https://rdrr.io/r/base/integer.html'>as.integer</a></span>(<span class='k'>min</span>),
         max = <span class='nf'><a href='https://rdrr.io/r/base/integer.html'>as.integer</a></span>(<span class='k'>max</span>))</code></pre>

</div>

Next, we use the `stringr` function [`str_count()`](https://stringr.tidyverse.org/reference/str_count.html) to count how many times the given letter appears in the password, and conditional logic to check whether it is repeated within the specified number of times. Because `TRUE` has a numeric value of 1 and `FALSE` has a numeric value of 0, we can sum the resulting column to get a count of how many passwords are valid according to their policies.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>passwords</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(count = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_count.html'>str_count</a></span>(<span class='k'>password</span>, <span class='k'>letter</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(password_in_policy = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='k'>count</span> <span class='o'>&gt;=</span> <span class='k'>min</span> <span class='o'>&amp;</span> <span class='k'>count</span> <span class='o'>&lt;=</span> <span class='k'>max</span>, <span class='kc'>TRUE</span>, <span class='kc'>FALSE</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(correct = <span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>password_in_policy</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>correct</span>)
<span class='c'>#&gt; [1] 625</span></code></pre>

</div>

#### Part 2: Position of letters

Now the policy is interpreted differently. Each policy actually describes two positions in the password, where 1 means the first character, 2 means the second character, and so on. Exactly one of these positions must contain the given letter. How many are valid now?

There were a couple of *gotchas* here. When I used [`separate()`](https://tidyr.tidyverse.org/reference/separate.html) in the previous part, I had inadvertently left a leading whitespace in front of the password, something that was messing up my indexing with `str_sub`. Using [`str_trim()`](https://stringr.tidyverse.org/reference/str_trim.html) first cleared that up. Also, we need *exactly one* of the positions to match. [`|`](https://rdrr.io/r/base/Logic.html) is an inclusive or. We need [`xor()`](https://rdrr.io/r/base/Logic.html) for exclusive or instead.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>passwords</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(password = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_trim.html'>str_trim</a></span>(<span class='k'>password</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(pos1_letter = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>password</span>, <span class='k'>min</span>, <span class='k'>min</span>),
         pos2_letter = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>password</span>, <span class='k'>max</span>, <span class='k'>max</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(match_one = <span class='nf'><a href='https://rdrr.io/r/base/Logic.html'>xor</a></span>(<span class='k'>pos1_letter</span> <span class='o'>==</span> <span class='k'>letter</span>, <span class='k'>pos2_letter</span> <span class='o'>==</span> <span class='k'>letter</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(correct = <span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>match_one</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>correct</span>) 
<span class='c'>#&gt; [1] 391</span></code></pre>

</div>

<p>
<a id='day3'></a>
</p>

Day 3: [Toboggan Trajectory](https://adventofcode.com/2020/day/3)
-----------------------------------------------------------------

[My day 3 data](https://ellakaye.rbind.io/post/advent-of-code-2020/data/AoC_day3.txt)

#### Part 1: Encountering trees

Starting at the top left corner of the map, how many trees ("\#") do we encounter, going at a trajectory of 3 right and 1 down?

First, read in the data and save it into a matrix. My method here feels really hack-y. I'm sure there must be a better approach.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)

<span class='k'>tree_map</span> <span class='o'>&lt;-</span> <span class='k'>readr</span>::<span class='nf'><a href='https://readr.tidyverse.org/reference/read_delim.html'>read_tsv</a></span>(<span class='s'>"data/AoC_day3.txt"</span>, col_names = <span class='kc'>FALSE</span>)

<span class='k'>num_col</span> <span class='o'>&lt;-</span> <span class='k'>tree_map</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(length = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_length.html'>str_length</a></span>(<span class='k'>X1</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/slice.html'>slice</a></span>(<span class='m'>1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>length</span>)

<span class='k'>tree_vec</span> <span class='o'>&lt;-</span> <span class='k'>tree_map</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(X1 = <span class='nf'><a href='https://rdrr.io/r/base/strsplit.html'>strsplit</a></span>(<span class='k'>X1</span>, split = <span class='nf'><a href='https://rdrr.io/r/base/character.html'>character</a></span>(<span class='m'>0</span>), fixed = <span class='kc'>TRUE</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>X1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://rdrr.io/r/base/unlist.html'>unlist</a></span>()

<span class='k'>tree_mat</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/matrix.html'>matrix</a></span>(<span class='k'>tree_vec</span>, ncol = <span class='k'>num_col</span>, byrow = <span class='kc'>TRUE</span>)</code></pre>

</div>

Now work my way across and down the matrix, using the [`%%`](https://rdrr.io/r/base/Arithmetic.html) modulo operator to loop round where necessary. The `-1` and `+1` in the line `((y + right - 1) %% num_col) + 1` is a hack to get round the fact that, for `num_col` columns, the modulo runs from `0` to `num_col - 1`, but the column indexes for our matrix run from `1` to `num_col`.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>right</span> <span class='o'>&lt;-</span> <span class='m'>3</span>
<span class='k'>down</span> <span class='o'>&lt;-</span> <span class='m'>1</span>

<span class='k'>num_rows</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/nrow.html'>nrow</a></span>(<span class='k'>tree_mat</span>)
<span class='k'>num_col</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/nrow.html'>ncol</a></span>(<span class='k'>tree_mat</span>)

<span class='c'># start counting trees encountered</span>
<span class='k'>trees</span> <span class='o'>&lt;-</span> <span class='m'>0</span>

<span class='c'># start square</span>
<span class='k'>x</span> <span class='o'>&lt;-</span> <span class='m'>1</span>
<span class='k'>y</span> <span class='o'>&lt;-</span> <span class='m'>1</span>
  
<span class='kr'>while</span> (<span class='k'>x</span> <span class='o'>&lt;=</span> <span class='k'>num_rows</span>) {
  
  <span class='c'># cat("row: ", x, "col: ", y, "\n")</span>
  
  <span class='kr'>if</span> (<span class='k'>tree_mat</span>[<span class='k'>x</span>,<span class='k'>y</span>] <span class='o'>==</span> <span class='s'>"#"</span>) <span class='k'>trees</span> <span class='o'>&lt;-</span> <span class='k'>trees</span> <span class='o'>+</span> <span class='m'>1</span>
  
  <span class='k'>x</span> <span class='o'>&lt;-</span> <span class='k'>x</span> <span class='o'>+</span> <span class='k'>down</span>
  <span class='k'>y</span> <span class='o'>&lt;-</span> ((<span class='k'>y</span> <span class='o'>+</span> <span class='k'>right</span> <span class='o'>-</span> <span class='m'>1</span>) <span class='o'>%%</span> <span class='k'>num_col</span>) <span class='o'>+</span> <span class='m'>1</span>
  
}

<span class='k'>trees</span>
<span class='c'>#&gt; [1] 299</span></code></pre>

</div>

#### Part 2: Checking further slopes

We now need to check several other trajectories, and multiply together the number of trees we find, so we wrap the Part 1 code into a function.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>slope_check</span> <span class='o'>&lt;-</span> <span class='nf'>function</span>(<span class='k'>tree_mat</span>, <span class='k'>right</span>, <span class='k'>down</span>) {
  
  <span class='k'>num_rows</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/nrow.html'>nrow</a></span>(<span class='k'>tree_mat</span>)
  <span class='k'>num_col</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/nrow.html'>ncol</a></span>(<span class='k'>tree_mat</span>)

  <span class='c'># start counting trees encountered</span>
  <span class='k'>trees</span> <span class='o'>&lt;-</span> <span class='m'>0</span>

  <span class='c'># start square</span>
  <span class='k'>x</span> <span class='o'>&lt;-</span> <span class='m'>1</span>
  <span class='k'>y</span> <span class='o'>&lt;-</span> <span class='m'>1</span>
  
  <span class='kr'>while</span> (<span class='k'>x</span> <span class='o'>&lt;=</span> <span class='k'>num_rows</span>) {
  
    <span class='kr'>if</span> (<span class='k'>tree_mat</span>[<span class='k'>x</span>,<span class='k'>y</span>] <span class='o'>==</span> <span class='s'>"#"</span>) <span class='k'>trees</span> <span class='o'>&lt;-</span> <span class='k'>trees</span> <span class='o'>+</span> <span class='m'>1</span>
  
    <span class='k'>x</span> <span class='o'>&lt;-</span> <span class='k'>x</span> <span class='o'>+</span> <span class='k'>down</span>
    <span class='k'>y</span> <span class='o'>&lt;-</span> ((<span class='k'>y</span> <span class='o'>+</span> <span class='k'>right</span> <span class='o'>-</span> <span class='m'>1</span>) <span class='o'>%%</span> <span class='k'>num_col</span>) <span class='o'>+</span> <span class='m'>1</span>
  
  }
  <span class='k'>trees</span>
}

<span class='nf'><a href='https://rdrr.io/r/base/prod.html'>prod</a></span>(<span class='nf'>slope_check</span>(<span class='k'>tree_mat</span>, <span class='m'>1</span>, <span class='m'>1</span>),
     <span class='nf'>slope_check</span>(<span class='k'>tree_mat</span>, <span class='m'>3</span>, <span class='m'>1</span>),
     <span class='nf'>slope_check</span>(<span class='k'>tree_mat</span>, <span class='m'>5</span>, <span class='m'>1</span>),
     <span class='nf'>slope_check</span>(<span class='k'>tree_mat</span>, <span class='m'>7</span>, <span class='m'>1</span>),
     <span class='nf'>slope_check</span>(<span class='k'>tree_mat</span>, <span class='m'>1</span>, <span class='m'>2</span>))
<span class='c'>#&gt; [1] 3621285278</span></code></pre>

</div>

<p>
<a id='day4'></a>
</p>

Day 4: [Passport Processing](https://adventofcode.com/2020/day/4)
-----------------------------------------------------------------

[My day 4 data](https://ellakaye.rbind.io/post/advent-of-code-2020/data/AoC_day4.txt)

#### Part 1: Complete passports

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)
<span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://tidyr.tidyverse.org'>tidyr</a></span>)</code></pre>

</div>

Using [`readr::read_tsv()`](https://readr.tidyverse.org/reference/read_delim.html) off the bat removes the blank lines, making it impossible to identify the different passports, but reading in the data via [`readLines()`](https://rdrr.io/r/base/readLines.html) then converting [`as_tibble()`](https://tibble.tidyverse.org/reference/as_tibble.html) preserves them, and then allows us to use `tidyverse` functions for the remaining tidying. [`cumsum()`](https://rdrr.io/r/base/cumsum.html) on a logical vectors takes advantage of `FALSE` having a numeric value of zero and `TRUE` having a numeric value of one.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>passports</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/readLines.html'>readLines</a></span>(<span class='s'>"data/AoC_day4.txt"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tibble.tidyverse.org/reference/as_tibble.html'>as_tibble</a></span>() <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/separate_rows.html'>separate_rows</a></span>(<span class='k'>value</span>, sep = <span class='s'>" "</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(new_passport = <span class='k'>value</span> <span class='o'>==</span> <span class='s'>""</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(ID = <span class='nf'><a href='https://rdrr.io/r/base/cumsum.html'>cumsum</a></span>(<span class='k'>new_passport</span>) <span class='o'>+</span> <span class='m'>1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='o'>!</span><span class='k'>new_passport</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/select.html'>select</a></span>(<span class='o'>-</span><span class='k'>new_passport</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/separate.html'>separate</a></span>(<span class='k'>value</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>"key"</span>, <span class='s'>"value"</span>), sep = <span class='s'>":"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/relocate.html'>relocate</a></span>(<span class='k'>ID</span>)</code></pre>

</div>

Our data is now in three columns, with ID, key and value, so now we need to find the number of passports with all seven fields once `cid` is excluded:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>passports</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>key</span> != <span class='s'>"cid"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/count.html'>count</a></span>(<span class='k'>ID</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>n</span> <span class='o'>==</span> <span class='m'>7</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://rdrr.io/r/base/nrow.html'>nrow</a></span>()
<span class='c'>#&gt; [1] 210</span></code></pre>

</div>

#### Part 2: Valid passports

Now we need to add data validation checks:

-   byr (Birth Year) - four digits; at least 1920 and at most 2002.
-   iyr (Issue Year) - four digits; at least 2010 and at most 2020.
-   eyr (Expiration Year) - four digits; at least 2020 and at most 2030.
-   hgt (Height) - a number followed by either cm or in:
    -   If cm, the number must be at least 150 and at most 193.
    -   If in, the number must be at least 59 and at most 76.
-   hcl (Hair Color) - a \# followed by exactly six characters 0-9 or a-f.
-   ecl (Eye Color) - exactly one of: amb blu brn gry grn hzl oth.
-   pid (Passport ID) - a nine-digit number, including leading zeroes.
-   cid (Country ID) - ignored, missing or not.

Ignoring the `cid` field, we narrow down on passports that at least have the right number of fields, and extract the number from the `hgt` column:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>complete_passports</span> <span class='o'>&lt;-</span> <span class='k'>passports</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>key</span> != <span class='s'>"cid"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/count.html'>add_count</a></span>(<span class='k'>ID</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>n</span> <span class='o'>==</span> <span class='m'>7</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/select.html'>select</a></span>(<span class='o'>-</span><span class='k'>n</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(hgt_value = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/case_when.html'>case_when</a></span>(
    <span class='k'>key</span> <span class='o'>==</span> <span class='s'>"hgt"</span> <span class='o'>~</span> <span class='k'>readr</span>::<span class='nf'><a href='https://readr.tidyverse.org/reference/parse_number.html'>parse_number</a></span>(<span class='k'>value</span>),
    <span class='kc'>TRUE</span> <span class='o'>~</span> <span class='m'>NA_real_</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>ungroup</a></span>()</code></pre>

</div>

Then we create a `check` column, which is `TRUE` when the value for each key meets the required conditions. Those with 7 `TRUE`s are valid. Note that with [`case_when()`](https://dplyr.tidyverse.org/reference/case_when.html) we've left the check column as `NA` when the condition is `FALSE`, requiring `na.rm = TRUE` in the call to [`sum()`](https://rdrr.io/r/base/sum.html). We can get round that by adding a final line to the [`case_when()`](https://dplyr.tidyverse.org/reference/case_when.html) condition stating `TRUE ~ FALSE`. `TRUE` here is a catch-all for all remaining rows not covered by the conditions above, and then we set them to `FALSE`, but I find the line `TRUE ~ FALSE` unintuitive.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>complete_passports</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(check = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/case_when.html'>case_when</a></span>(
    (<span class='k'>key</span> <span class='o'>==</span> <span class='s'>"byr"</span> <span class='o'>&amp;</span> <span class='k'>value</span> <span class='o'>&gt;=</span> <span class='m'>1920</span>) <span class='o'>&amp;</span> (<span class='k'>key</span> <span class='o'>==</span> <span class='s'>"byr"</span> <span class='o'>&amp;</span> <span class='k'>value</span> <span class='o'>&lt;=</span> <span class='m'>2002</span>) <span class='o'>~</span> <span class='kc'>TRUE</span>,
    (<span class='k'>key</span> <span class='o'>==</span> <span class='s'>"iyr"</span> <span class='o'>&amp;</span> <span class='k'>value</span> <span class='o'>&gt;=</span> <span class='m'>2010</span>) <span class='o'>&amp;</span> (<span class='k'>key</span> <span class='o'>==</span> <span class='s'>"iyr"</span> <span class='o'>&amp;</span> <span class='k'>value</span> <span class='o'>&lt;=</span> <span class='m'>2020</span>) <span class='o'>~</span> <span class='kc'>TRUE</span>,
    (<span class='k'>key</span> <span class='o'>==</span> <span class='s'>"eyr"</span> <span class='o'>&amp;</span> <span class='k'>value</span> <span class='o'>&gt;=</span> <span class='m'>2020</span>) <span class='o'>&amp;</span> (<span class='k'>key</span> <span class='o'>==</span> <span class='s'>"eyr"</span> <span class='o'>&amp;</span> <span class='k'>value</span> <span class='o'>&lt;=</span> <span class='m'>2030</span>) <span class='o'>~</span> <span class='kc'>TRUE</span>,
    <span class='k'>key</span> <span class='o'>==</span> <span class='s'>"hgt"</span> <span class='o'>&amp;</span> <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_detect.html'>str_detect</a></span>(<span class='k'>value</span>, <span class='s'>"cm"</span>) <span class='o'>&amp;</span> <span class='k'>hgt_value</span> <span class='o'>&gt;=</span> <span class='m'>150</span> <span class='o'>&amp;</span> <span class='k'>hgt_value</span> <span class='o'>&lt;=</span> <span class='m'>193</span> <span class='o'>~</span> <span class='kc'>TRUE</span>,
    <span class='k'>key</span> <span class='o'>==</span> <span class='s'>"hgt"</span> <span class='o'>&amp;</span> <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_detect.html'>str_detect</a></span>(<span class='k'>value</span>, <span class='s'>"in"</span>) <span class='o'>&amp;</span> <span class='k'>hgt_value</span> <span class='o'>&gt;=</span> <span class='m'>59</span> <span class='o'>&amp;</span> <span class='k'>hgt_value</span> <span class='o'>&lt;=</span> <span class='m'>76</span> <span class='o'>~</span> <span class='kc'>TRUE</span>,  
    <span class='k'>key</span> <span class='o'>==</span> <span class='s'>"hcl"</span> <span class='o'>&amp;</span> <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_detect.html'>str_detect</a></span>(<span class='k'>value</span>, <span class='s'>"^#[a-f0-9]{6}$"</span>) <span class='o'>~</span> <span class='kc'>TRUE</span>,
    <span class='k'>key</span> <span class='o'>==</span> <span class='s'>"ecl"</span> <span class='o'>&amp;</span> <span class='k'>value</span> <span class='o'>%in%</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>"amb"</span>, <span class='s'>"blu"</span>, <span class='s'>"brn"</span>, <span class='s'>"gry"</span>, <span class='s'>"grn"</span>, <span class='s'>"hzl"</span>, <span class='s'>"oth"</span>) <span class='o'>~</span> <span class='kc'>TRUE</span>,
    <span class='k'>key</span> <span class='o'>==</span> <span class='s'>"pid"</span> <span class='o'>&amp;</span> <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_detect.html'>str_detect</a></span>(<span class='k'>value</span>, <span class='s'>"^[0-9]{9}$"</span>) <span class='o'>~</span> <span class='kc'>TRUE</span>
  )) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>group_by</a></span>(<span class='k'>ID</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(check_all = <span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>check</span>, na.rm = <span class='kc'>TRUE</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>check_all</span> <span class='o'>==</span> <span class='m'>7</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://rdrr.io/r/base/nrow.html'>nrow</a></span>()
<span class='c'>#&gt; [1] 131</span></code></pre>

</div>

<p>
<a id='day5'></a>
</p>

Day 5: [Binary Boarding](https://adventofcode.com/2020/day/5)
-------------------------------------------------------------

[My day 5 data](https://ellakaye.rbind.io/post/advent-of-code-2020/data/AoC_day5.txt)

#### Part 1: Finding all seat IDs

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)
<span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='http://stringr.tidyverse.org'>stringr</a></span>)</code></pre>

</div>

The code below sets starts by setting each row number to 127 and each column number to 7, the maximum they can be, then, working along the string, lowering the maximum (or leaving it as is) one letter at a time:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>boarding</span> <span class='o'>&lt;-</span> <span class='k'>readr</span>::<span class='nf'><a href='https://readr.tidyverse.org/reference/read_delim.html'>read_tsv</a></span>(<span class='s'>"data/AoC_day5.txt"</span>, col_names = <span class='kc'>FALSE</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/rename.html'>rename</a></span>(binary = <span class='k'>X1</span>)

<span class='k'>seat_IDs</span> <span class='o'>&lt;-</span> <span class='k'>boarding</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(row = <span class='m'>127</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(col = <span class='m'>7</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(row = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>binary</span>, <span class='m'>1</span>, <span class='m'>1</span>) <span class='o'>==</span> <span class='s'>"F"</span>, <span class='k'>row</span> <span class='o'>-</span> <span class='m'>64</span>, <span class='k'>row</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(row = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>binary</span>, <span class='m'>2</span>, <span class='m'>2</span>) <span class='o'>==</span> <span class='s'>"F"</span>, <span class='k'>row</span> <span class='o'>-</span> <span class='m'>32</span>, <span class='k'>row</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(row = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>binary</span>, <span class='m'>3</span>, <span class='m'>3</span>) <span class='o'>==</span> <span class='s'>"F"</span>, <span class='k'>row</span> <span class='o'>-</span> <span class='m'>16</span>, <span class='k'>row</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(row = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>binary</span>, <span class='m'>4</span>, <span class='m'>4</span>) <span class='o'>==</span> <span class='s'>"F"</span>, <span class='k'>row</span> <span class='o'>-</span> <span class='m'>8</span>, <span class='k'>row</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(row = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>binary</span>, <span class='m'>5</span>, <span class='m'>5</span>) <span class='o'>==</span> <span class='s'>"F"</span>, <span class='k'>row</span> <span class='o'>-</span> <span class='m'>4</span>, <span class='k'>row</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(row = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>binary</span>, <span class='m'>6</span>, <span class='m'>6</span>) <span class='o'>==</span> <span class='s'>"F"</span>, <span class='k'>row</span> <span class='o'>-</span> <span class='m'>2</span>, <span class='k'>row</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(row = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>binary</span>, <span class='m'>7</span>, <span class='m'>7</span>) <span class='o'>==</span> <span class='s'>"F"</span>, <span class='k'>row</span> <span class='o'>-</span> <span class='m'>1</span>, <span class='k'>row</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(col = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>binary</span>, <span class='m'>8</span>, <span class='m'>8</span>) <span class='o'>==</span> <span class='s'>"L"</span>, <span class='k'>col</span> <span class='o'>-</span> <span class='m'>4</span>, <span class='k'>col</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(col = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>binary</span>, <span class='m'>9</span>, <span class='m'>9</span>) <span class='o'>==</span> <span class='s'>"L"</span>, <span class='k'>col</span> <span class='o'>-</span> <span class='m'>2</span>, <span class='k'>col</span>)) <span class='o'>%&gt;%</span>  
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(col = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/if_else.html'>if_else</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_sub.html'>str_sub</a></span>(<span class='k'>binary</span>, <span class='m'>10</span>, <span class='m'>10</span>) <span class='o'>==</span> <span class='s'>"L"</span>, <span class='k'>col</span> <span class='o'>-</span> <span class='m'>1</span>, <span class='k'>col</span>)) <span class='o'>%&gt;%</span>  
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(ID = <span class='k'>row</span> <span class='o'>*</span> <span class='m'>8</span> <span class='o'>+</span> <span class='k'>col</span>) 

<span class='k'>seat_IDs</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(max = <span class='nf'><a href='https://rdrr.io/r/base/Extremes.html'>max</a></span>(<span class='k'>ID</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>max</span>)
<span class='c'>#&gt; [1] 963</span></code></pre>

</div>

OK, I know I said in the introduction to this post that I would go with the first solution I think of that gets the right answer, and the above does work, but I'm *deeply* unhappy with the code. There's too much repetition, I don't like the use of subtraction when diving by 2 feels more appropriate in a binary context, and it doesn't feel like I've taken full advantage of the mathematical structure of the problem. So, on further reflection, I realise that the way that ID is defined is essentially turning a binary number into a decimal, where we get the binary number as a string by replacing "B" and "R" by "1" and L\" and "F" by "0". Then, I just found, there is a base R function [`strtoi()`](https://rdrr.io/r/base/strtoi.html) that takes a string of digits in a given base and converts it to a base 10 integer, just what we need:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>seat_IDs</span> <span class='o'>&lt;-</span> <span class='k'>boarding</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(binary = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_replace.html'>str_replace_all</a></span>(<span class='k'>binary</span>, <span class='s'>"L|F"</span>, <span class='s'>"0"</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(binary = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_replace.html'>str_replace_all</a></span>(<span class='k'>binary</span>, <span class='s'>"B|R"</span>, <span class='s'>"1"</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(ID = <span class='nf'><a href='https://rdrr.io/r/base/strtoi.html'>strtoi</a></span>(<span class='k'>binary</span>, base = <span class='m'>2</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/arrange.html'>arrange</a></span>(<span class='nf'><a href='https://dplyr.tidyverse.org/reference/desc.html'>desc</a></span>(<span class='k'>ID</span>))

<span class='k'>seat_IDs</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/slice.html'>slice</a></span>(<span class='m'>1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>ID</span>)
<span class='c'>#&gt; [1] 963</span></code></pre>

</div>

That's better!

#### Part 2: Finding my seat ID

We need to find the missing number, so we arrange the IDs in ascending order and look at the gap between each ID and the preceding one. In most cases, that should be one. Where we have a gap of 2, we must have skipped the integer below:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>seat_IDs</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/arrange.html'>arrange</a></span>(<span class='k'>ID</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(diff = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/lead-lag.html'>lag</a></span>(<span class='k'>ID</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(gap = <span class='k'>ID</span> <span class='o'>-</span> <span class='k'>diff</span>) <span class='o'>%&gt;%</span> 
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>gap</span> <span class='o'>==</span> <span class='m'>2</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(my_seat = <span class='k'>ID</span> <span class='o'>-</span> <span class='m'>1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>my_seat</span>)
<span class='c'>#&gt; [1] 592</span></code></pre>

</div>

<p>
<a id='day6'></a>
</p>

Day 6: [Custom Customs](https://adventofcode.com/2020/day/6)
------------------------------------------------------------

[My day 6 data](https://ellakaye.rbind.io/post/advent-of-code-2020/data/AoC_day6.txt)

#### Part 1: Anyone answers

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)
<span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://tidyr.tidyverse.org'>tidyr</a></span>)
<span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='http://stringr.tidyverse.org'>stringr</a></span>)</code></pre>

</div>

Within each group, we need to find the number of unique letters within each group. We read in and separate the data using the tricks learnt for <a href="#day4">Day 4</a>, and take advantage of the [`rowwise()`](https://dplyr.tidyverse.org/reference/rowwise.html) feature in `dplyr 1.0.0`.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>customs_groups</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/readLines.html'>readLines</a></span>(<span class='s'>"data/AoC_day6.txt"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tibble.tidyverse.org/reference/as_tibble.html'>as_tibble</a></span>() <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(new_group = <span class='k'>value</span> <span class='o'>==</span> <span class='s'>""</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(group_ID = <span class='nf'><a href='https://rdrr.io/r/base/cumsum.html'>cumsum</a></span>(<span class='k'>new_group</span>) <span class='o'>+</span> <span class='m'>1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='o'>!</span><span class='k'>new_group</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/select.html'>select</a></span>(<span class='o'>-</span><span class='k'>new_group</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>group_by</a></span>(<span class='k'>group_ID</span>) 

<span class='k'>customs_groups</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(qs = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_c.html'>str_c</a></span>(<span class='k'>value</span>, collapse = <span class='s'>""</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>ungroup</a></span>() <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(qss = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_split.html'>str_split</a></span>(<span class='k'>qs</span>, <span class='s'>""</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/rowwise.html'>rowwise</a></span>() <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(qsu = <span class='nf'><a href='https://rdrr.io/r/base/list.html'>list</a></span>(<span class='nf'><a href='https://rdrr.io/r/base/unique.html'>unique</a></span>(<span class='k'>qss</span>))) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(count = <span class='nf'><a href='https://rdrr.io/r/base/length.html'>length</a></span>(<span class='k'>qsu</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>ungroup</a></span>() <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(total = <span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>count</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>total</span>)
<span class='c'>#&gt; [1] 6585</span></code></pre>

</div>

#### Part 2: Everyone answers

Now, instead of unique letters in a group, we need to find the number of letters which appear in all the answers for everyone in the same group. I first note how many people are in each group, then tabulate the number of occurrences of each letter in the group, then count (by summing a logical vector) the number of matches between occurrences of letter and the number in group. Finally, we sum across all groups.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>customs_groups</span> <span class='o'>%&gt;%</span>  
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/count.html'>add_count</a></span>(<span class='k'>group_ID</span>, name = <span class='s'>"num_in_group"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>group_by</a></span>(<span class='k'>group_ID</span>, <span class='k'>num_in_group</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(qs = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_c.html'>str_c</a></span>(<span class='k'>value</span>, collapse = <span class='s'>""</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>ungroup</a></span>() <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(qss = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_split.html'>str_split</a></span>(<span class='k'>qs</span>, <span class='s'>""</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/rowwise.html'>rowwise</a></span>() <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(letter_table = <span class='nf'><a href='https://rdrr.io/r/base/list.html'>list</a></span>(<span class='nf'><a href='https://rdrr.io/r/base/table.html'>table</a></span>(<span class='k'>qss</span>))) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/slice.html'>slice</a></span>(<span class='m'>1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(in_common = <span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>num_in_group</span> <span class='o'>==</span> <span class='k'>letter_table</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/group_by.html'>ungroup</a></span>() <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(total = <span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>in_common</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>total</span>)
<span class='c'>#&gt; [1] 3276</span></code></pre>

</div>

<p>
<a id='day7'></a>
</p>

Day 7: [Handy Haverstocks](https://adventofcode.com/2020/day/7)
---------------------------------------------------------------

[My day 7 data](https://ellakaye.rbind.io/post/advent-of-code-2020/data/AoC_day7.txt)

#### Part 1: Number of colour bags

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='http://tidyverse.tidyverse.org'>tidyverse</a></span>)</code></pre>

</div>

We have colour-coded bags that must contain a specific number of other colour-coded bags.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>bags</span> <span class='o'>&lt;-</span> <span class='nf'>read_tsv</span>(<span class='s'>"data/AoC_day7.txt"</span>, col_names = <span class='kc'>FALSE</span>)

<span class='nf'><a href='https://rdrr.io/r/utils/head.html'>head</a></span>(<span class='k'>bags</span>)
<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 6 x 1</span></span>
<span class='c'>#&gt;   X1                                                                            </span>
<span class='c'>#&gt;   <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>                                                                         </span></span>
<span class='c'>#&gt; <span style='color: #555555;'>1</span><span> wavy bronze bags contain 5 striped gold bags, 5 light tomato bags.            </span></span>
<span class='c'>#&gt; <span style='color: #555555;'>2</span><span> drab indigo bags contain 4 pale bronze bags, 2 mirrored lavender bags.        </span></span>
<span class='c'>#&gt; <span style='color: #555555;'>3</span><span> pale olive bags contain 3 faded bronze bags, 5 wavy orange bags, 3 clear blac…</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>4</span><span> faded white bags contain 5 vibrant violet bags, 4 light teal bags.            </span></span>
<span class='c'>#&gt; <span style='color: #555555;'>5</span><span> mirrored magenta bags contain 2 muted cyan bags, 3 vibrant crimson bags.      </span></span>
<span class='c'>#&gt; <span style='color: #555555;'>6</span><span> dull purple bags contain 1 striped fuchsia bag.</span></span></code></pre>

</div>

Our first task is to parse the natural language and split the rules into one container/contains pair per line:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>rules</span> <span class='o'>&lt;-</span> <span class='k'>bags</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(rule = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/ranking.html'>row_number</a></span>()) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/separate.html'>separate</a></span>(<span class='k'>X1</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>"container"</span>, <span class='s'>"contains"</span>), sep = <span class='s'>" bags contain "</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/separate_rows.html'>separate_rows</a></span>(<span class='k'>contains</span>, sep = <span class='s'>","</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(contains = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_remove.html'>str_remove</a></span>(<span class='k'>contains</span>, <span class='s'>"\\."</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(contains = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_remove.html'>str_remove</a></span>(<span class='k'>contains</span>, <span class='s'>"bags|bag"</span>)) <span class='o'>%&gt;%</span>
  <span class='c'>#mutate(contains = str_replace(contains, "no other", "0 other")) %&gt;%</span>
  <span class='nf'><a href='https://tidyr.tidyverse.org/reference/extract.html'>extract</a></span>(<span class='k'>contains</span>, <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>'number'</span>, <span class='s'>'contains'</span>), <span class='s'>"(\\d+) (.+)"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='o'>!</span><span class='nf'><a href='https://rdrr.io/r/base/NA.html'>is.na</a></span>(<span class='k'>number</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(contains = <span class='nf'><a href='https://stringr.tidyverse.org/reference/str_trim.html'>str_trim</a></span>(<span class='k'>contains</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(number = <span class='nf'><a href='https://rdrr.io/r/base/integer.html'>as.integer</a></span>(<span class='k'>number</span>)) </code></pre>

</div>

To find all bags that con eventually contain our `shiny gold` bag, we first find the bags that can contain it directly. We then find the bags that can contain those bags and take the union of the two levels. We repeat, stopping when going up a level adds no further bags to the vector of bag colours already found. We then subtract 1, because we don't want to count the original shiny gold bag.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='c'># function to find all colours that contain a vector of other colours:</span>
<span class='k'>contains_colours</span> <span class='o'>&lt;-</span> <span class='nf'>function</span>(<span class='k'>colours</span>) {
  <span class='k'>rules</span> <span class='o'>%&gt;%</span>
    <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>contains</span> <span class='o'>%in%</span> <span class='k'>colours</span>) <span class='o'>%&gt;%</span>
    <span class='nf'><a href='https://dplyr.tidyverse.org/reference/distinct.html'>distinct</a></span>(<span class='k'>container</span>) <span class='o'>%&gt;%</span>
    <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>container</span>)
}

<span class='k'>bags</span> <span class='o'>&lt;-</span> <span class='s'>"shiny gold"</span>
<span class='k'>old_length</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/length.html'>length</a></span>(<span class='k'>bags</span>)
<span class='k'>new_length</span> <span class='o'>&lt;-</span> <span class='m'>0</span>

<span class='c'># keeping adding to the vector of bags, until no change</span>
<span class='kr'>while</span>(<span class='k'>old_length</span> != <span class='k'>new_length</span>) {
  <span class='k'>old_length</span> <span class='o'>=</span> <span class='nf'><a href='https://rdrr.io/r/base/length.html'>length</a></span>(<span class='k'>bags</span>)
  <span class='k'>bags</span> <span class='o'>&lt;-</span> <span class='k'>base</span>::<span class='nf'><a href='https://rdrr.io/r/base/sets.html'>union</a></span>(<span class='k'>bags</span>, <span class='nf'>contains_colours</span>(<span class='k'>bags</span>)) <span class='o'>%&gt;%</span> <span class='nf'><a href='https://rdrr.io/r/base/unique.html'>unique</a></span>()
  <span class='k'>new_length</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/length.html'>length</a></span>(<span class='k'>bags</span>)
  <span class='c'>#cat(old_length, ", ", new_length, "\n")</span>
}

<span class='nf'><a href='https://rdrr.io/r/base/length.html'>length</a></span>(<span class='k'>bags</span>) <span class='o'>-</span> <span class='m'>1</span>
<span class='c'>#&gt; [1] 274</span></code></pre>

</div>

#### Part 2: Number of bags

Now we need to discover the number of bags that a shiny gold bag must contain. I figured that lends itself to recursion, but struggled on the details. Hat tip to David Robinson for [this solution](https://twitter.com/drob/status/1336003816395845632). I've learnt a lot for myself by unpicking how it works.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>count_all_contained</span> <span class='o'>&lt;-</span> <span class='nf'>function</span>(<span class='k'>colour</span>) {
  
  <span class='k'>relevant_rules</span> <span class='o'>&lt;-</span> <span class='k'>rules</span> <span class='o'>%&gt;%</span>
    <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>container</span> <span class='o'>%in%</span> <span class='k'>colour</span>)
  
  <span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>relevant_rules</span><span class='o'>$</span><span class='k'>number</span> <span class='o'>*</span> (<span class='m'>1</span> <span class='o'>+</span> <span class='nf'>map_dbl</span>(<span class='k'>relevant_rules</span><span class='o'>$</span><span class='k'>contains</span>, <span class='k'>count_all_contained</span>)))
  
}

<span class='nf'>count_all_contained</span>(<span class='s'>"shiny gold"</span>)
<span class='c'>#&gt; [1] 158730</span></code></pre>

</div>

<p>
<a id='day8'></a>
</p>

Day 8: [Handheld Halting](https://adventofcode.com/2020/day/8)
--------------------------------------------------------------

[My day 8 data](https://ellakaye.rbind.io/post/advent-of-code-2020/data/AoC_day8.txt)

#### Part 1: Infinite Loop

Our programme gets stuck in an infinite loop. As well as keeping track of the accumulator, we need to keep track of where we've visited, and stop when we visit the same instruction twice. We use a [`data.frame()`](https://rdrr.io/r/base/data.frame.html) rather than a [`tibble()`](https://tibble.tidyverse.org/reference/tibble.html) as the former is easier to index into.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>instructions</span> <span class='o'>&lt;-</span> 
  <span class='nf'><a href='https://rdrr.io/r/utils/read.table.html'>read.table</a></span>(<span class='s'>"data/AoC_day8.txt"</span>, col.names = <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='s'>"instruction"</span>, <span class='s'>"value"</span>))</code></pre>

</div>

We start with a pretty straight-forward loop, noting that at most it can run for one more than the number of instructions in the programme until it hits an instruction it's already visited. We update row number to visit next and the accumulator as appropriate.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>instructions</span><span class='o'>$</span><span class='k'>visited</span> <span class='o'>&lt;-</span> <span class='m'>0</span>

<span class='k'>row</span> <span class='o'>&lt;-</span> <span class='m'>1</span>
<span class='k'>accumulator</span> <span class='o'>&lt;-</span> <span class='m'>0</span>

<span class='k'>num_rows</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/nrow.html'>nrow</a></span>(<span class='k'>instructions</span>)

<span class='kr'>for</span> (<span class='k'>i</span> <span class='kr'>in</span> <span class='m'>1</span><span class='o'>:</span>(<span class='k'>num_rows</span><span class='o'>+</span><span class='m'>1</span>)) {

  <span class='kr'>if</span> (<span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"visited"</span>] != <span class='m'>0</span>) <span class='kr'>break</span>
  
  <span class='c'># +1 on number of times the row is visited</span>
  <span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"visited"</span>] <span class='o'>&lt;-</span> <span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"visited"</span>] <span class='o'>+</span> <span class='m'>1</span>

  <span class='c'># case when the instruction is "acc"</span>
  <span class='kr'>if</span> (<span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"instruction"</span>] <span class='o'>==</span> <span class='s'>"acc"</span>) {
    <span class='k'>accumulator</span> <span class='o'>&lt;-</span> <span class='k'>accumulator</span> <span class='o'>+</span> <span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"value"</span>]
    <span class='k'>row</span> <span class='o'>&lt;-</span> <span class='k'>row</span> <span class='o'>+</span> <span class='m'>1</span>
  }
  
  <span class='c'># case when the instruction is "jmp"</span>
  <span class='kr'>else</span> <span class='kr'>if</span> (<span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"instruction"</span>] <span class='o'>==</span> <span class='s'>"jmp"</span>) {
    <span class='k'>row</span> <span class='o'>&lt;-</span> <span class='k'>row</span> <span class='o'>+</span> <span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"value"</span>]
  }

  <span class='c'># case when the instruction is "nop"</span>
  <span class='kr'>else</span> <span class='kr'>if</span> (<span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"instruction"</span>] <span class='o'>==</span> <span class='s'>"nop"</span>) {
    <span class='k'>row</span> <span class='o'>&lt;-</span> <span class='k'>row</span> <span class='o'>+</span> <span class='m'>1</span>
  }
}
  
<span class='k'>accumulator</span>
<span class='c'>#&gt; [1] 1915</span></code></pre>

</div>

#### Part 2: Fixing the programme

To break the loop, one of the `nop` instructions in the programme should be a `jmp` or vice versa. The plan is to swap these out one by one and check if the programme completes. It's not a sophisticated approach, but it works fast enough (about a second).

First we note that the broken instruction must be one that we visited in Part 1. Also, an instruction of `jmp` with a value of 0 will get us stuck in a one-line infinite loop, so we avoid that.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)

<span class='k'>rows_to_check</span> <span class='o'>&lt;-</span> <span class='k'>instructions</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate.html'>mutate</a></span>(row_id = <span class='nf'><a href='https://dplyr.tidyverse.org/reference/ranking.html'>row_number</a></span>()) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>visited</span> != <span class='m'>0</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>instruction</span> != <span class='s'>"acc"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='o'>!</span>(<span class='k'>instruction</span> <span class='o'>==</span> <span class='s'>"nop"</span> <span class='o'>&amp;</span> <span class='k'>value</span> <span class='o'>==</span> <span class='m'>0</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>row_id</span>)</code></pre>

</div>

We have 93 instruction to check. We modify our code from Part 1 slightly, converting it into a function and returning a list with values `completes` and `accumulator`. `completes` is `FALSE` as soon as we visit a row twice and `TRUE` if the number of our next row to visit is greater than the number of rows in the programme.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>programme_completes</span> <span class='o'>&lt;-</span> <span class='nf'>function</span>(<span class='k'>instructions</span>) {
  
  <span class='k'>row</span> <span class='o'>&lt;-</span> <span class='m'>1L</span>
  <span class='k'>accumulator</span> <span class='o'>&lt;-</span> <span class='m'>0</span>
  
  <span class='k'>num_rows</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/nrow.html'>nrow</a></span>(<span class='k'>instructions</span>)
  
  <span class='kr'>for</span> (<span class='k'>i</span> <span class='kr'>in</span> <span class='m'>1</span><span class='o'>:</span>(<span class='k'>num_rows</span><span class='o'>+</span><span class='m'>1</span>)) {
  
    <span class='kr'>if</span> (<span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"visited"</span>] != <span class='m'>0</span>) {
      <span class='nf'><a href='https://rdrr.io/r/base/function.html'>return</a></span>(<span class='nf'><a href='https://rdrr.io/r/base/list.html'>list</a></span>(completes = <span class='kc'>FALSE</span>, accumulator = <span class='k'>accumulator</span>)) 
    }
    
    <span class='c'># +1 on number of times the row is visited</span>
    <span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"visited"</span>] <span class='o'>&lt;-</span> <span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"visited"</span>] <span class='o'>+</span> <span class='m'>1</span>
  
    <span class='c'># case when the instruction is "acc"</span>
    <span class='kr'>if</span> (<span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"instruction"</span>] <span class='o'>==</span> <span class='s'>"acc"</span>) {
      <span class='k'>accumulator</span> <span class='o'>&lt;-</span> <span class='k'>accumulator</span> <span class='o'>+</span> <span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"value"</span>]
      <span class='k'>row</span> <span class='o'>&lt;-</span> <span class='k'>row</span> <span class='o'>+</span> <span class='m'>1</span>
    }
  
    <span class='kr'>else</span> <span class='kr'>if</span> (<span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"instruction"</span>] <span class='o'>==</span> <span class='s'>"jmp"</span>) {
      <span class='k'>row</span> <span class='o'>&lt;-</span> <span class='k'>row</span> <span class='o'>+</span> <span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"value"</span>]
    }
  
    <span class='kr'>else</span> <span class='kr'>if</span> (<span class='k'>instructions</span>[<span class='k'>row</span>, <span class='s'>"instruction"</span>] <span class='o'>==</span> <span class='s'>"nop"</span>) {
      <span class='k'>row</span> <span class='o'>&lt;-</span> <span class='k'>row</span> <span class='o'>+</span> <span class='m'>1</span>
    }
  
    <span class='kr'>if</span> (<span class='k'>row</span> <span class='o'>&gt;</span> <span class='k'>num_rows</span>) {
      <span class='nf'><a href='https://rdrr.io/r/base/function.html'>return</a></span>(<span class='nf'><a href='https://rdrr.io/r/base/list.html'>list</a></span>(completes = <span class='kc'>TRUE</span>, accumulator = <span class='k'>accumulator</span>)) 
    }
  }
}  </code></pre>

</div>

We now loop over the rows we've identified to check, breaking the loop as soon as we find a programme that completes. Finally, we extract the accumulator value from the successful programme.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>instructions</span><span class='o'>$</span><span class='k'>visited</span> <span class='o'>&lt;-</span> <span class='m'>0</span>

<span class='kr'>for</span> (<span class='k'>row</span> <span class='kr'>in</span> <span class='k'>rows_to_check</span>) {
  
  <span class='c'># modify one row of the instructions,</span>
  <span class='c'># copying data frame so we don't have to modify it back</span>
  <span class='k'>modified_instructions</span> <span class='o'>&lt;-</span> <span class='k'>instructions</span>
  
  <span class='nf'><a href='https://rdrr.io/r/base/ifelse.html'>ifelse</a></span>(<span class='k'>instructions</span>[<span class='k'>row</span>, <span class='m'>1</span>] <span class='o'>==</span> <span class='s'>"jmp"</span>, 
         <span class='k'>modified_instructions</span>[<span class='k'>row</span>, <span class='m'>1</span>] <span class='o'>&lt;-</span> <span class='s'>"nop"</span>, 
         <span class='k'>modified_instructions</span>[<span class='k'>row</span>, <span class='m'>1</span>] <span class='o'>&lt;-</span> <span class='s'>"jmp"</span>) 
  
  <span class='c'># check if the modified programme completes</span>
  <span class='k'>check_programme</span> <span class='o'>&lt;-</span> <span class='nf'>programme_completes</span>(<span class='k'>modified_instructions</span>)
  
  <span class='kr'>if</span> (<span class='k'>check_programme</span><span class='o'>$</span><span class='k'>completes</span>) 
    <span class='kr'>break</span>
}

<span class='k'>check_programme</span><span class='o'>$</span><span class='k'>accumulator</span>
<span class='c'>#&gt; [1] 944</span></code></pre>

</div>

<p>
<a id='day9'></a>
</p>

Day 9: [Encoding Error](https://adventofcode.com/2020/day/9)
------------------------------------------------------------

[My day 9 data](https://ellakaye.rbind.io/post/advent-of-code-2020/data/AoC_day9.txt)

#### Part 1: Weak Link

We have to find the first number in the list which is *not* the sum of a pair of different numbers in the preceding 25 numbers.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>input</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/double.html'>as.double</a></span>(<span class='nf'><a href='https://rdrr.io/r/base/readLines.html'>readLines</a></span>(<span class='s'>"data/AoC_day9.txt"</span>)) </code></pre>

</div>

There's a nice trick for finding the pair of numbers in a vector that sum to a target that was doing the rounds on twitter in response to the <a href="#day1">Day 1</a> challenge: [`intersect(input, 2020 - input)`](https://rdrr.io/pkg/generics/man/setops.html). For this challenge, we expand on that idea, writing it as a `check_sum` function. Where there's more than one pair, it won't say which pair together, and if the number that's half the target appears in the addends, it will only appear once in the output. However, for this challenge, we only need to know when there are *no* pairs that sum to the target, which will be the case when the length of the output is 0.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>check_sum</span> <span class='o'>&lt;-</span> <span class='nf'>function</span>(<span class='k'>target</span>, <span class='k'>addends</span>) {
  <span class='nf'><a href='https://rdrr.io/pkg/generics/man/setops.html'>intersect</a></span>(<span class='k'>addends</span>, <span class='k'>target</span><span class='o'>-</span><span class='k'>addends</span>)
}</code></pre>

</div>

PICK UP HERE

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>find_invalid_num</span> <span class='o'>&lt;-</span> <span class='nf'>function</span>(<span class='k'>vec</span>, <span class='k'>win</span> = <span class='m'>25</span>) {
  
  <span class='kr'>for</span> (<span class='k'>i</span> <span class='kr'>in</span> (<span class='k'>win</span><span class='o'>+</span><span class='m'>1</span>)<span class='o'>:</span><span class='nf'><a href='https://rdrr.io/r/base/length.html'>length</a></span>(<span class='k'>vec</span>)) {
    <span class='k'>check</span> <span class='o'>&lt;-</span> <span class='nf'>check_sum</span>(<span class='k'>vec</span>[<span class='k'>i</span>], <span class='k'>vec</span>[(<span class='k'>i</span><span class='o'>-</span><span class='k'>win</span>)<span class='o'>:</span>(<span class='k'>i</span><span class='o'>-</span><span class='m'>1</span>)])
    
    <span class='kr'>if</span> (<span class='nf'><a href='https://rdrr.io/r/base/length.html'>length</a></span>(<span class='k'>check</span>) <span class='o'>==</span> <span class='m'>0</span>) <span class='nf'><a href='https://rdrr.io/r/base/function.html'>return</a></span>(<span class='k'>vec</span>[<span class='k'>i</span>])
  }
  
}

<span class='nf'>find_invalid_num</span>(<span class='k'>input</span>)
<span class='c'>#&gt; [1] 507622668</span></code></pre>

</div>

#### Part 2: Contiguous set

Find a contiguous set in the list that sums to the invalid number from part 1, and add together the largest and smallest number in that range.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>target</span> <span class='o'>&lt;-</span> <span class='nf'>find_invalid_num</span>(<span class='k'>input</span>)

<span class='k'>input_reduced</span> <span class='o'>&lt;-</span> <span class='k'>input</span>[<span class='m'>1</span><span class='o'>:</span>(<span class='nf'><a href='https://rdrr.io/r/base/which.html'>which</a></span>(<span class='k'>input</span> <span class='o'>==</span> <span class='k'>target</span>)<span class='o'>-</span><span class='m'>1</span>)]


<span class='k'>contiguous_sum</span> <span class='o'>&lt;-</span> <span class='nf'>function</span>(<span class='k'>input</span>, <span class='k'>target</span>) {
  
  <span class='k'>len</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/length.html'>length</a></span>(<span class='k'>input</span>)
  
  <span class='kr'>for</span> (<span class='k'>i</span> <span class='kr'>in</span> <span class='m'>1</span><span class='o'>:</span><span class='k'>len</span>) {
    <span class='k'>a</span> <span class='o'>&lt;-</span> <span class='k'>purrr</span>::<span class='nf'><a href='https://purrr.tidyverse.org/reference/accumulate.html'>accumulate</a></span>(<span class='k'>input</span>[<span class='k'>i</span><span class='o'>:</span><span class='k'>len</span>], <span class='k'>sum</span>)
    <span class='k'>b</span> <span class='o'>&lt;-</span> <span class='k'>a</span> <span class='o'>==</span> <span class='k'>target</span>
    
    <span class='kr'>if</span> (<span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>b</span>) <span class='o'>==</span> <span class='m'>1</span>) {
      <span class='k'>output_length</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/which.html'>which</a></span>(<span class='k'>b</span>)
      
      <span class='k'>contiguous_set</span> <span class='o'>&lt;-</span> <span class='k'>input</span>[<span class='k'>i</span><span class='o'>:</span>(<span class='k'>i</span> <span class='o'>+</span> <span class='k'>output_length</span> <span class='o'>-</span> <span class='m'>1</span>)]
      
      <span class='nf'><a href='https://rdrr.io/r/base/function.html'>return</a></span>(<span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='nf'><a href='https://rdrr.io/r/base/range.html'>range</a></span>(<span class='k'>contiguous_set</span>)))
    }
  }
}

<span class='nf'>contiguous_sum</span>(<span class='k'>input_reduced</span>, <span class='k'>target</span>)
<span class='c'>#&gt; [1] 76688505</span></code></pre>

</div>

<p>
<a id='day10'></a>
</p>

Day 10: [Adapter Array](https://adventofcode.com/2020/day/10)
-------------------------------------------------------------

[My day 10 data](https://ellakaye.rbind.io/post/advent-of-code-2020/data/AoC_day10.txt)

#### Part 1: Adapter Distribution

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)</code></pre>

</div>

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>adapters</span> <span class='o'>&lt;-</span> 
  <span class='nf'><a href='https://rdrr.io/r/base/readLines.html'>readLines</a></span>(<span class='s'>"data/AoC_day10.txt"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://rdrr.io/r/base/integer.html'>as.integer</a></span>()

<span class='k'>adapter_diffs</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='k'>adapters</span>, <span class='m'>0</span>, <span class='nf'><a href='https://rdrr.io/r/base/Extremes.html'>max</a></span>(<span class='k'>adapters</span>) <span class='o'>+</span> <span class='m'>3</span>) <span class='o'>%&gt;%</span> 
  <span class='nf'><a href='https://rdrr.io/r/base/sort.html'>sort</a></span>() <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://rdrr.io/r/base/diff.html'>diff</a></span>()

<span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>adapter_diffs</span> <span class='o'>==</span> <span class='m'>1</span>) <span class='o'>*</span> <span class='nf'><a href='https://rdrr.io/r/base/sum.html'>sum</a></span>(<span class='k'>adapter_diffs</span> <span class='o'>==</span> <span class='m'>3</span>)
<span class='c'>#&gt; [1] 3034</span></code></pre>

</div>

#### Part 2: Adapter combinations

Instead of building up sequences of adapters, we see what we can remove from the full list.

First, we check the diffs: are they just 1 and 3 or are there any 2s?

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/table.html'>table</a></span>(<span class='k'>adapter_diffs</span>)
<span class='c'>#&gt; adapter_diffs</span>
<span class='c'>#&gt;  1  3 </span>
<span class='c'>#&gt; 74 41</span></code></pre>

</div>

We can't remove an adapter if its difference with the previous adapter is 3, otherwise the difference between the adapters on either side of it will be too big.

What about diffs of 1? It depends how many ones there are around it. We can check this using the [`rle()`](https://rdrr.io/r/base/rle.html) (run length encoding) function

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>runs</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/rle.html'>rle</a></span>(<span class='k'>adapter_diffs</span>)
<span class='k'>runs</span>
<span class='c'>#&gt; Run Length Encoding</span>
<span class='c'>#&gt;   lengths: int [1:48] 3 2 4 2 4 2 4 1 4 2 ...</span>
<span class='c'>#&gt;   values : num [1:48] 1 3 1 3 1 3 1 3 1 3 ...</span></code></pre>

</div>

What is the distribution of lengths of sequences of 1s?

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>runs_table</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/table.html'>table</a></span>(<span class='k'>runs</span><span class='o'>$</span><span class='k'>lengths</span>) 
<span class='k'>runs_table</span>
<span class='c'>#&gt; </span>
<span class='c'>#&gt;  1  2  3  4 </span>
<span class='c'>#&gt; 13 14 10 11</span></code></pre>

</div>

We have at most four diffs on 1 in a row.

Example sequences really helped me figure out what's going on here:

-   If the diff sequence is ..., 3, 1, 3,... (e.g. adapters 1, 4, 5, 8)
    -   we can keep the sequence as is (1 option)
    -   We cannot remove any adapters
    -   **1 option in total**
-   If the diff sequence is ..., 3, 1, 1, 3,... (e.g. adapters 1, 4, 5, 6, 9)
    -   we can keep as is (1 option)
    -   If removing one adapter, we can only remove the one with a difference of one on either side (e.g. the 5) (1 option)
    -   we cannot remove two adapters
    -   **2 options total**
-   If the diff sequence is ..., 3, 1, 1, 1, 3,...(e.g. adapters 1, 4, 5, 6, 7, 10)
    -   We can keep as is (1 option)
    -   If removing one adapter, we cannot remove the adapter leading to the first diff of 1, but we could remove either of the next two (2 options) (e.g. the 5 or 6)
    -   If removing two adapters, we can only remove the two leading to the second two diffs of 1 (e.g. the 5 and 6) (1 option)
    -   We cannot remove three adapters
    -   **4 options total**
-   If the diff sequence is ..., 3, 1, 1, 1, 1, 3,... (e.g. adapters 1, 4, 5, 6, 7, 8, 11)
    -   we can keep as is (1 option)
    -   If removing one adapter, it can be any except the one leading to the first diff of 1 (e.g. 5, 6, 7) (3 options)
    -   Similarly, there are three options for removing two adapters (e.g. any two of 5, 6, 7) (3 options)
    -   We cannot remove three adapters
    -   **7 options total**

Now, we multiply each run of difference of 1s with the number of options we have for removing adapters, then take the product of those products.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>runs_df</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://tibble.tidyverse.org/reference/tibble.html'>tibble</a></span>(lengths = <span class='k'>runs</span><span class='o'>$</span><span class='k'>lengths</span>, values = <span class='k'>runs</span><span class='o'>$</span><span class='k'>values</span>)

<span class='k'>options</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://tibble.tidyverse.org/reference/tibble.html'>tibble</a></span>(lengths = <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='m'>1</span>,<span class='m'>2</span>,<span class='m'>3</span>,<span class='m'>4</span>), options = <span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='m'>1</span>,<span class='m'>2</span>,<span class='m'>4</span>,<span class='m'>7</span>))

<span class='k'>runs_df</span> <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='k'>values</span> <span class='o'>==</span> <span class='m'>1</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/mutate-joins.html'>left_join</a></span>(<span class='k'>options</span>, by = <span class='s'>"lengths"</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/summarise.html'>summarise</a></span>(prod_options = <span class='nf'><a href='https://rdrr.io/r/base/prod.html'>prod</a></span>(<span class='k'>options</span>)) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/pull.html'>pull</a></span>(<span class='k'>prod_options</span>) <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://rdrr.io/r/base/format.html'>format</a></span>(scientific = <span class='kc'>FALSE</span>) 
<span class='c'>#&gt; [1] "259172170858496"</span></code></pre>

</div>

Next
----

This post will be updated as I work my way through the remainder of the challenges.

