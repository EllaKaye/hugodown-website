---
output: hugodown::md_document
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "alerts"
subtitle: ""
summary: ""
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
rmd_hash: b6561e2eb2a0b895

---

{{% alert note %}} A note {{% /alert %}}

{{% alert warning %}} A warning {{% /alert %}}

<div class="alert-note">

Another note

</div>

<div class="alert-warning">

Another warning

</div>

Need to use the standard syntax with curly brackets, not the ::: one.

{{% alert note %}} It doesn't seem possible to use bullet list within an alert. {{% /alert %}}

