---
output: hugodown::md_document
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "n_letter_words and a personal (publicly available) package"
subtitle: ""
summary: "How I created a handy function and a personal package"
authors: []
tags: ["R", "EMK", "rstats"]
categories: ["R"]
date: 2017-06-17
lastmod: 2020-07-22
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
rmd_hash: c8f38a07cabe2473

---

There's a little R function that I wrote and packaged up to generate a vector or data frame of words of a given length. I find it useful in a wide variety of contexts and thought other might too. To kick off my new blog, here's a post about it.

The function, `n_letter_words`, came about because I wanted to be able to generate row and column names for a large matrix - didn't matter what they were, as long as they were unique. Since I was in the habit of using the built-in `LETTERS` vector to do this for small matrices, I naturally thought of using combinations of letters to do this in a larger case. In figuring out how to do this, as is so often the case, it was [stackoverflow](https://stackoverflow.com/questions/11388359/unique-combination-of-all-elements-from-two-or-more-vectors) to the rescue. There, I learnt about `expand.grid` and could then use some tidyverse tools to get the vector I was after:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='http://tidyverse.tidyverse.org'>tidyverse</a></span>)
<span class='k'>out</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/r/base/expand.grid.html'>expand.grid</a></span>(<span class='k'>LETTERS</span>, <span class='k'>LETTERS</span>) <span class='o'>%&gt;%</span>
  <span class='nf'>as_tibble</span>() <span class='o'>%&gt;%</span>
  <span class='nf'>unite</span>(<span class='k'>word</span>, <span class='m'>1</span><span class='o'>:</span><span class='m'>2</span>, sep = <span class='s'>""</span>) <span class='o'>%&gt;%</span>
  <span class='nf'>pull</span>()
<span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='nf'><a href='https://rdrr.io/r/utils/head.html'>head</a></span>(<span class='k'>out</span>), <span class='nf'><a href='https://rdrr.io/r/utils/head.html'>tail</a></span>(<span class='k'>out</span>))
<span class='c'>#&gt;  [1] "AA" "BA" "CA" "DA" "EA" "FA" "UZ" "VZ" "WZ" "XZ" "YZ" "ZZ"</span></code></pre>

</div>

Sorted! At least I thought so, until, a couple of months later, when I wanted to generate names for a 1000\*1000 matrix, and realised both that I'd forgotten the `expand.grid` trick, and once I'd re-found the stackoverflow post, that it didn't give me enough words. That was enough to make it worth writing a function, taking `n` as an argument, that gives all 'words' of length $n$.

Writing functions always makes me think of what other arguments might be useful. What if we want something between the 676 two-letter words and 17,576 three-letter words (or the 456,976 four-letter words, etc)? Hence the argument `num_letters`, which can be set between 1 and 26, and results in a total of $\text{num_letters}^n$ words. By default, the function returns a `tibble`, but setting `as_vector = TRUE` does what you'd expect. And I threw in a `case` argument too.

Now that I had my function, what to do with it? I remembered articles I'd read about the usefulness of [making](https://hilaryparker.com/2014/04/29/writing-an-r-package-from-scratch/) and [sharing]((https://hilaryparker.com/2013/04/03/personal-r-packages/)) a personal package. Now seemed like the time to do that myself.

So, [here](https://github.com/EllaKaye/EMK) is my personal package, `EMK`. If you think that `n_letter_words` might be of use to you, then feel free to install!

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>devtools</span>::<span class='nf'><a href='https://devtools.r-lib.org//reference/remote-reexports.html'>install_github</a></span>(<span class='s'>"EllaKaye/EMK"</span>)
<span class='c'>#&gt; </span>
<span class='c'>#&gt;      checking for file ‘/private/var/folders/n2/y0dp6crn77x9_4fdnfzw52cm0000gn/T/RtmpOeORYa/remotesb49a1e64d0a2/EllaKaye-EMK-1f9be81/DESCRIPTION’ ...  <span style='color: #00BB00;'>✔</span><span>  </span><span style='color: #555555;'>checking for file ‘/private/var/folders/n2/y0dp6crn77x9_4fdnfzw52cm0000gn/T/RtmpOeORYa/remotesb49a1e64d0a2/EllaKaye-EMK-1f9be81/DESCRIPTION’</span><span style='color: #00BBBB;'> (436ms)</span></span>39m
<span class='c'>#&gt;   <span style='color: #555555;'>─  preparing ‘EMK’:</span></span>9m
<span class='c'>#&gt;      checking DESCRIPTION meta-information ...  <span style='color: #00BB00;'>✔</span><span>  </span><span style='color: #555555;'>checking DESCRIPTION meta-information</span></span>[39m
<span class='c'>#&gt;   <span style='color: #555555;'>─  checking for LF line-endings in source and make files and shell scripts</span></span>9m
<span class='c'>#&gt;   <span style='color: #555555;'>─  checking for empty or unneeded directories</span></span>9m
<span class='c'>#&gt;   <span style='color: #555555;'>─  building ‘EMK_0.1.0.tar.gz’</span></span>9m
<span class='c'>#&gt;      </span>  
<span class='c'>#&gt; </span>
<span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://github.com/EllaKaye/EMK'>EMK</a></span>)</code></pre>

</div>

Some examples of `n_letter_words`:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/pkg/EMK/man/n_letter_words.html'>n_letter_words</a></span>(<span class='m'>2</span>)
<span class='c'>#&gt; Warning: `as_data_frame()` is deprecated as of tibble 2.0.0.</span>
<span class='c'>#&gt; Please use `as_tibble()` instead.</span>
<span class='c'>#&gt; The signature and semantics have changed, see `?as_tibble`.</span>
<span class='c'>#&gt; <span style='color: #555555;'>This warning is displayed once every 8 hours.</span></span>
<span class='c'>#&gt; <span style='color: #555555;'>Call `lifecycle::last_warnings()` to see where this warning was generated.</span></span>
<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 676 x 1</span></span>
<span class='c'>#&gt;    word </span>
<span class='c'>#&gt;    <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 1</span><span> AA   </span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 2</span><span> BA   </span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 3</span><span> CA   </span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 4</span><span> DA   </span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 5</span><span> EA   </span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 6</span><span> FA   </span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 7</span><span> GA   </span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 8</span><span> HA   </span></span>
<span class='c'>#&gt; <span style='color: #555555;'> 9</span><span> IA   </span></span>
<span class='c'>#&gt; <span style='color: #555555;'>10</span><span> JA   </span></span>
<span class='c'>#&gt; <span style='color: #555555;'># … with 666 more rows</span></span>

<span class='k'>some_three_letter_words</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://rdrr.io/pkg/EMK/man/n_letter_words.html'>n_letter_words</a></span>(<span class='m'>3</span>, num_letters = <span class='m'>10</span>, case = <span class='s'>"lower"</span>, as_vector = <span class='kc'>TRUE</span>)
<span class='nf'><a href='https://rdrr.io/r/base/c.html'>c</a></span>(<span class='nf'><a href='https://rdrr.io/r/utils/head.html'>head</a></span>(<span class='k'>some_three_letter_words</span>), <span class='nf'><a href='https://rdrr.io/r/utils/head.html'>tail</a></span>(<span class='k'>some_three_letter_words</span>))
<span class='c'>#&gt;  [1] "aaa" "baa" "caa" "daa" "eaa" "faa" "ejj" "fjj" "gjj" "hjj" "ijj" "jjj"</span>
<span class='nf'><a href='https://rdrr.io/r/base/length.html'>length</a></span>(<span class='k'>some_three_letter_words</span>)
<span class='c'>#&gt; [1] 1000</span></code></pre>

</div>

For now, my personal package has only this one function, but watch this space! No doubt I'll be adding more that I find useful. Perhaps, you'll find them useful too.

Incidentally, none of the above would have happened if I'd just thought, for my test matrix `A`, to set `dimnames(A) <- list(1:nrow(A), 1:ncol(A))`!

