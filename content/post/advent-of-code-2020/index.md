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
rmd_hash: a199b9adb7c36375

---

[Advent of Code](https://adventofcode.com) is a series of small programming challenges, released daily throughout December in the run-up to Christmas. Part 1 of the challenge is given first. On its successful completion, Part 2 is revealed. The challenges are designed to be solved in any programming language. I will be using R.

There will no doubt be a wide variety of ways to solve these problems. I'm going to go with the first thing I think of that gets an answer. In most cases, I expect that there will be more concise and efficient solutions.

Each participant gets different input data, so my numerical solutions may be different from others. If you're not signed up for Advent of Code yourself, but want to follow along with my data, you can download it at from the data links at the beginning of each day's section. The links in the day section headers take you to challenge on the Advent of Code page. The links directly below are a table of contents for this page.

1.  <a href="#day1">Report Repair</a>
2.  <a href="#day2">Password Philosophy</a>
3.  <a href="#day3">Toboggan Trajectory</a>

<p>
<a id='day1'></a>
</p>

Day 1: [Report Repair](https://adventofcode.com/2020/day/1)
-----------------------------------------------------------

[My day 1 data](https://ellakaye.rbind.io/post/advent-of-code-2020/AoC_day1.txt)

#### Part 1: Two numbers

The challenge is to find two numbers from a list that sum to 2020, then to report their product.

[`expand.grid()`](https://rdrr.io/r/base/expand.grid.html) creates a data frame from all combinations of the supplied vectors. Since the vectors are the same, each pair is duplicated. In this case the two numbers in the list that sum to 2020 are 704 and 1316, and we have one row with 704 as Var1 and one with 704 as Var2. [`slice(1)`](https://dplyr.tidyverse.org/reference/slice.html) takes the first occurrence of the pair.

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

[My day 2 data](https://ellakaye.rbind.io/post/advent-of-code-2020/AoC_day2.txt)

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

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>passwords</span> <span class='o'>&lt;-</span> <span class='k'>readr</span>::<span class='nf'><a href='https://readr.tidyverse.org/reference/read_delim.html'>read_tsv</a></span>(<span class='s'>"AoC_day2.txt"</span>, col_names = <span class='kc'>FALSE</span>) <span class='o'>%&gt;%</span>
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

[My day 3 data](https://ellakaye.rbind.io/post/advent-of-code-2020/AoC_day3.txt)

#### Part 1: Encountering trees

Starting at the top left corner of the map, how many trees ("\#") do we encounter, going at a trajectory of 3 right and 1 down?

First, read in the data and save it into a matrix. My method here feels really hack-y. I'm sure there must be a better approach.

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)

<span class='k'>tree_map</span> <span class='o'>&lt;-</span> <span class='k'>readr</span>::<span class='nf'><a href='https://readr.tidyverse.org/reference/read_delim.html'>read_tsv</a></span>(<span class='s'>"AoC_day3.txt"</span>, col_names = <span class='kc'>FALSE</span>)

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

