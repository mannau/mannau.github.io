---
layout: post
title:  "boilerpipeR 1.3 on CRAN"
categories: [Textmining, boilerpipeR]
tags: [R, Release]
---

The newest version of **boilerpipeR** 1.3 has now been released on 
[CRAN](http://cran.r-project.org/web/packages/boilerpipeR/index.html) including 
minor doc changes and bug-fixes. The package is essentially an R-wrapper for 
Christian Kohlschutters boilerpipe Java library to extract the main content from 
HTML files (see [library](https://code.google.com/p/boilerpipe) and 
[paper](http://www.l3s.de/~kohlschuetter/boilerplate)).

<!--more-->

The functionality of boilerpipeR is shown in one simple example to extract
the main content from a Reuters news site. Firstly, we need to download the
content of the HTML file using e.g. **RCurl**


```r
library(boilerpipeR)
library(RCurl)
```


```r
url <- "http://www.reuters.com/article/2015/05/12/us-russia-usa-idUSKBN0NX0LG20150512"
content <- getURL(url)
substr(content, 1, 80)
```

```
## [1] "<!--[if !IE]> This has been served from cache <![endif]-->\n<!--[if !IE]> Request"
```

The requested content still includes HTML boilerplate like banners, sidebars, etc.
We can get rid of the content using one of boilerpipe's content extractors, e.g.
using the *ArticleExtractor* we can extract the content using


```r
url <- "http://www.reuters.com/article/2015/05/12/us-russia-usa-idUSKBN0NX0LG20150512"
content.extract <- ArticleExtractor(content)
substr(content.extract, 1, 80)
```

```
## [1] "SOCHI, Russia | By Arshad Mohammed and Denis Dyomkin\nSOCHI, Russia Top U.S. and "
```
