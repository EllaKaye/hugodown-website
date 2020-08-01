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
draft: true

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
rmd_hash: f3568b5a9255ec6e

---

Testing using custom fonts with `ragg` and `systemfonts` (although don't seem to need to have either library loaded!)

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='http://ggplot2.tidyverse.org'>ggplot2</a></span>)</code></pre>

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
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Raleway"</span>, size = <span class='m'>20</span>))</code></pre>

</div>

### Using a Google Font saved in Font Book

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Mulish"</span>, size = <span class='m'>20</span>))
</code></pre>
<img src="figs/unnamed-chunk-4-1.png" width="700px" style="display: block; margin: auto;" />

</div>

### Using a Google Font with `showtext`

### Using a .ttf font with `cairo`

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>p</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Mulish"</span>, size = <span class='m'>20</span>))

<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggsave.html'>ggsave</a></span>(<span class='s'>"Mulish-title.pdf"</span>, device = <span class='k'>cairo_pdf</span>)
<span class='c'>#&gt; Saving 7 x 4.33 in image</span></code></pre>

</div>

### Using a .oft font with `cairo`

This also works

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>p</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Museo"</span>, size = <span class='m'>20</span>))

<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggsave.html'>ggsave</a></span>(<span class='s'>"Museo-title.pdf"</span>, device = <span class='k'>cairo_pdf</span>)
<span class='c'>#&gt; Saving 7 x 4.33 in image</span></code></pre>

</div>

### Using a .otf with `ragg`

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/grDevices/quartz.html'>quartz</a></span>()
<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Museo"</span>, size = <span class='m'>20</span>))</code></pre>

</div>

Also works

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='k'>p</span> <span class='o'>&lt;-</span> <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Museo"</span>, size = <span class='m'>20</span>))

<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggsave.html'>ggsave</a></span>(<span class='s'>"Museo-title.png"</span>, type = <span class='s'>"cairo"</span>)
<span class='c'>#&gt; Saving 7 x 4.33 in image</span></code></pre>

</div>

### Using a computer .otf font with `showtext`

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://github.com/yixuan/showtext'>showtext</a></span>)
<span class='c'>#&gt; Loading required package: sysfonts</span>
<span class='c'>#&gt; Loading required package: showtextdb</span></code></pre>

</div>

Does not work:

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/pkg/showtext/man/showtext_auto.html'>showtext_auto</a></span>()

<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Museo"</span>, size = <span class='m'>20</span>))</code></pre>

</div>

Not sure what counts as value for `regular`

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'>font_add</span>(<span class='s'>"Museo"</span>, regular = <span class='s'>"Museo"</span>)
<span class='nf'><a href='https://rdrr.io/pkg/showtext/man/showtext_auto.html'>showtext_auto</a></span>()

<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Museo"</span>, size = <span class='m'>20</span>))</code></pre>

</div>

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='https://dplyr.tidyverse.org'>dplyr</a></span>)
<span class='c'>#&gt; </span>
<span class='c'>#&gt; Attaching package: 'dplyr'</span>
<span class='c'>#&gt; The following objects are masked from 'package:stats':</span>
<span class='c'>#&gt; </span>
<span class='c'>#&gt;     filter, lag</span>
<span class='c'>#&gt; The following objects are masked from 'package:base':</span>
<span class='c'>#&gt; </span>
<span class='c'>#&gt;     intersect, setdiff, setequal, union</span>
<span class='nf'><a href='https://rdrr.io/r/base/library.html'>library</a></span>(<span class='k'><a href='http://stringr.tidyverse.org'>stringr</a></span>)
<span class='k'>systemfonts</span>::<span class='nf'><a href='https://rdrr.io/pkg/systemfonts/man/system_fonts.html'>system_fonts</a></span>() <span class='o'>%&gt;%</span>
  <span class='nf'><a href='https://dplyr.tidyverse.org/reference/filter.html'>filter</a></span>(<span class='nf'><a href='https://stringr.tidyverse.org/reference/str_detect.html'>str_detect</a></span>(<span class='k'>path</span>, <span class='s'>"Museo"</span>))
<span class='c'>#&gt; <span style='color: #555555;'># A tibble: 3 x 9</span></span>
<span class='c'>#&gt;   path                 index name    family  style weight width italic monospace</span>
<span class='c'>#&gt;   <span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>                </span><span style='color: #555555;font-style: italic;'>&lt;int&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>   </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span>   </span><span style='color: #555555;font-style: italic;'>&lt;chr&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;ord&gt;</span><span>  </span><span style='color: #555555;font-style: italic;'>&lt;ord&gt;</span><span> </span><span style='color: #555555;font-style: italic;'>&lt;lgl&gt;</span><span>  </span><span style='color: #555555;font-style: italic;'>&lt;lgl&gt;</span><span>    </span></span>
<span class='c'>#&gt; <span style='color: #555555;'>1</span><span> /Users/ellakaye/Lib…     0 Museo-… Museo   500   normal norm… FALSE  FALSE    </span></span>
<span class='c'>#&gt; <span style='color: #555555;'>2</span><span> /Users/ellakaye/Lib…     0 MuseoS… Museo … 500   normal norm… FALSE  FALSE    </span></span>
<span class='c'>#&gt; <span style='color: #555555;'>3</span><span> /Users/ellakaye/Lib…     0 Museo-… Museo   300   light  norm… FALSE  FALSE</span></span></code></pre>

</div>

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'>font_add</span>(<span class='s'>"Museo"</span>, regular = <span class='s'>"/Users/ellakaye/Library/Fonts/exljbris - Museo-500"</span>)
<span class='nf'><a href='https://rdrr.io/pkg/showtext/man/showtext_auto.html'>showtext_auto</a></span>()

<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Museo"</span>, size = <span class='m'>20</span>))</code></pre>

</div>

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'>font_add_google</span>(<span class='s'>"Cinzel"</span>, <span class='s'>"Cinzel"</span>)
<span class='nf'><a href='https://rdrr.io/pkg/showtext/man/showtext_auto.html'>showtext_auto</a></span>()

<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>diamonds</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_bar.html'>geom_bar</a></span>(<span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(<span class='k'>color</span>, fill = <span class='k'>color</span>)) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>ggtitle</a></span>(<span class='s'>"A fancy font"</span>) <span class='o'>+</span> 
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(text = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Cinzel"</span>, size = <span class='m'>20</span>))</code></pre>

</div>

### From Andrew's blog

Need to have font family installed on computer

<div class="highlight">

<pre class='chroma'><code class='language-r' data-lang='r'><span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggplot.html'>ggplot</a></span>(<span class='k'>mpg</span>, <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/aes.html'>aes</a></span>(x = <span class='k'>cyl</span>, y = <span class='k'>hwy</span>)) <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_point.html'>geom_point</a></span>() <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/geom_smooth.html'>geom_smooth</a></span>(method = <span class='s'>"lm"</span>) <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/annotate.html'>annotate</a></span>(<span class='s'>"text"</span>, x = <span class='m'>5</span>, y = <span class='m'>35</span>, label = <span class='s'>"There aren't a lot of\n5 cylinder cars"</span>,
           family = <span class='s'>"Source Sans Pro Semibold"</span>, color = <span class='s'>"#DC5B44"</span>, size = <span class='m'>4</span>) <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/labs.html'>labs</a></span>(title = <span class='s'>"Highway miles per gallon and cylinders"</span>,
       subtitle = <span class='s'>"This is an interesting relationship, I guess"</span>,
       caption = <span class='s'>"Source: ggplot's built-in data"</span>,
       x = <span class='s'>"Cylinders"</span>, y = <span class='s'>"Highway miles per gallon"</span>) <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/ggtheme.html'>theme_light</a></span>(base_family = <span class='s'>"Source Sans Pro"</span>) <span class='o'>+</span>
  <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/theme.html'>theme</a></span>(plot.caption = <span class='nf'><a href='https://ggplot2.tidyverse.org/reference/element.html'>element_text</a></span>(family = <span class='s'>"Source Sans Pro ExtraLight"</span>))</code></pre>

</div>

{{% alert note %}} Using theme fonts does NOT work. {{% /alert %}} {{% alert note %}} Also, using a .otf font in Font Book doesn't work. {{% /alert %}} {{% alert note %}} Also, using adding `face = "italic"` to `element_text` doesn't work if italic option not specified in font set. {{% /alert %}} {{% alert warning %}} After adding a font to Font Book, might need to restart computer before it works in plots {{% /alert %}}

