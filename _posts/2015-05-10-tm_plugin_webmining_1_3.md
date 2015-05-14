---
layout: post
title:  "tm.plugin.webmining 1.3 on CRAN"
categories: [Textmining, tm.plugin.webmining]
tags: [R, Release]
---

**tm.plugin.webmining** supports the retrieval of full-text content from 
structured news feeds (e.g. RSS) and HTML pages in a 2-step procedure:

1. Download meta data from the feed (through *WebSource*)
2. Download and extract main content for the feed item (through *WebCorpus*)

The newest version of **tm.plugin.webmining** 1.3 has now been 
released on [CRAN](CRAN-tm.plugin.webmining) including minor changes and bug-fixes:

- Fix [issue](#7) with Yahoo 
  News Source; since Yahoo News seems to have de-activated RSS feeds we now switched to 
  HTML retrieval. Thanks Eliano for reporting!
- Fix retrieval [issue](#6) 
  with NYTimes Source.

[CRAN-tm.plugin.webmining]: http://cran.r-project.org/web/packages/tm.plugin.webmining/index.html
[#6]: https://github.com/mannau/tm.plugin.webmining/issues/6
[#7]: https://github.com/mannau/tm.plugin.webmining/issues/7

<!--more-->

To retrieve current financial news for Tesla (TSLA) from Google Finance we can 
use the following code:


```r
library(tm)
library(tm.plugin.webmining)
```

The resulting corpus of length 20 includes the following meta-data:

```r
gf <- WebCorpus(GoogleFinanceSource("NASDAQ:TSLA"))
meta(gf[[1]])
```

```
##   author       : character(0)
##   datetimestamp: 2015-05-10 15:56:15
##   description  : 
## Tesla Motors, Inc. Buys Its Way Into Michigan Manufacturing
## Motley Fool - May 10, 2015 
## When Tesla Motors (NASDAQ: TSLA ) management said in its fourth-quarter letter to shareholders it intended to spend $1.5 billion this year on capital expenditures, this figure wasn't conservative by any means.
## Tesla Motors, Inc. (TSLA) Bulls Bet On an End-of-Week Breakout - Schaeffers Research (blog)Tesla Motors Inc Owes $600 Million To Its Own Customers - LearnBonds
## 
##   heading      : Tesla Motors, Inc. Buys Its Way Into Michigan Manufacturing
##   id           : tag:finance.google.com,cluster:52778836093904
##   language     : character(0)
##   origin       : http://www.fool.com/investing/general/2015/05/10/tesla-motors-inc-buys-its-way-into-michigan-manufa.aspx
```

We can now show a frequency histogram of datetimestamps as follows:

```r
gf <- WebCorpus(GoogleFinanceSource("NASDAQ:TSLA"))
hist(do.call(c, lapply(gf, meta, "datetimestamp")), breaks = "days", 
freq = TRUE, col = "grey")
```

![plot of chunk web-3](/figure/source/2015-05-10-tm_plugin_webmining_1_3/web-3-1.png) 

The corpus is not big enough for serious statistical analysis, but should be 
sufficient for a simple wordcloud:

```r
library(wordcloud)
```

Remove stopwords and show wordcloud:

```r
gf.proc <- tm_map(gf, content_transformer(tolower))
gf.proc <- tm_map(gf.proc, removeWords, stopwords("english"))
wordcloud(gf.proc, colors=brewer.pal(6,"Dark2"),random.order=FALSE)
```

![plot of chunk web-5](/figure/source/2015-05-10-tm_plugin_webmining_1_3/web-5-1.png) 
