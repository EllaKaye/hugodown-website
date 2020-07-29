---
output: hugodown::md_document
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Testing custom fonts in plots"
subtitle: ""
summary: "Playing around with `ragg`"
authors: []
tags: []
categories: []
date: 2020-07-29
lastmod: 2020-07-29
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
rmd_hash: b96227e5e42ef857

---

Testing using custom fonts with `ragg`
--------------------------------------

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='http://ggplot2.tidyverse.org'>ggplot2</a></span>)
<span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://ragg.r-lib.org'>ragg</a></span>)</code></pre>

</div>

### Using a font saved on my computer as .ttf

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Neutraface Slab Display TT Bold"</span>, size = <span class='m'>20</span>))
</code></pre>
<img src="figs/unnamed-chunk-2-1.png" width="700px" style="display: block; margin: auto;" />

</div>

### Using a Google Font declared in `emk_font_set2`

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Fira Code"</span>, size = <span class='m'>20</span>))
</code></pre>
<img src="figs/unnamed-chunk-3-1.png" width="700px" style="display: block; margin: auto;" />

</div>

### Using a variation of a Google Font saved in Font Book

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"FiraCode-Light"</span>, size = <span class='m'>10</span>))
</code></pre>
<img src="figs/unnamed-chunk-4-1.png" width="700px" style="display: block; margin: auto;" />

</div>

<div class="alert-note">

-   Using a Google Font NOT declared in `emk_font_set2` does not work.
-   Also, using a .otf font in Font Book doesn't work.
-   Also, using adding `face = "italic"` to `element_text` doesn't work if italic option not specified in font set.

</div>

