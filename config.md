<!--
Add here global page variables to use throughout your website.
-->
+++
author = "Julian P Samaroo"
mintoclevel = 2
motto = "Fast, Smart Parallelism"

# Add here files or directories that should be ignored by Franklin, otherwise
# these files might be copied and, if markdown, processed by Franklin which
# you might not want. Indicate directories by ending the name with a `/`.
# Base files such as LICENSE.md and README.md are ignored by default.
ignore = ["node_modules/"]

# RSS (the website_{title, descr, url} must be defined to get RSS)
generate_rss = true
website_title = "Dagger.jl Home"
website_descr = "Dagger.jl - $motto"
website_url   = "https://daggerjl.com/"
+++

<!--
Add here global latex commands to use throughout your pages.
-->
\newcommand{\R}{\mathbb R}
\newcommand{\scal}[1]{\langle #1 \rangle}
