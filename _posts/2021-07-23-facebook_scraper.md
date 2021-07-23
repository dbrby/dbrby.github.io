---
title: 'Scraping Legislators’ Facebook Posts Using facebook-scraper'
date: 2021-06-25
permalink: /posts/2021/07/twint/
tags:
  - Social Media
  - Legislative Studies
  - Political Science
---

This blog post is the cousin of the last one. Instead of looking at
legislators on Twitter, this time we are looking at Facebook. Much of
the data I outline collecting here can be accessed through their
official API. But Facebook’s rules and rate limits are strict. Tools
like CrowdTangle are powerful for many forms of social inquiry, though
they aren’t the most accessible (see here in the [New York
Times](https://www.nytimes.com/2021/07/14/technology/facebook-data.html)
and in
[Gizmodo](https://gizmodo.com/facebook-knifes-its-own-analytics-tool-to-hide-its-ben-1847291288).
However, for our use case, scraping metadata and all posts from
legislators, this can be readily achieved through targeted, non-rate
limited tools. And much like the last blog, this is a tool that breaches
all terms of service of Facebook (I myself have never used this, I don’t even
really know what The Facebook is).

Lets begin from the same foundation as the [Twitter
blog](https://dbrby.github.io/posts/2021/07/twint/). There are many
repos and databases that provide coverage of Legislators’ social media
presence. My personal favourite, that I am certain will scale up in
years (hopefully months) to come and become the one-stop-shop for
legislative studies research is Sascha Gobel and Simon Munzert’s
‘Comparative Legislators Database’. Which we can call from the
convenient R interface `{legislatoR}`.

… yes, this is almost the exact same code as last time with two terms
changed. I saw someone post recently about Facebook posts for
US-Senators, so I’ll do that instead of MSPs.

``` r
require(legislatoR) # Interface to CLD
require(tidyverse) # Handy Joins and wrangling tools

us <- "usa_senate" # Having to do quotation marks gets annoying store "usa_senate" as an object

sen116 <- left_join(get_political(us) %>% filter(session == 116), get_core(us), by = "pageid") %>%
  select(pageid, session, party, constituency, wikidataid) %>%
  left_join(., get_social(us) %>%
              select(wikidataid, facebook), by = "wikidataid") %>% drop_na(facebook)

# Filter to Senators by Session == 116 
# Join political (Senators by terms) to core (Ind. Senators)
# Join again to incorporate social media data
# Remove variables except wikidataid for join and facebook
# Drop NA facebook values

write.csv(sen116, "116-senators-facebook.csv")
```

Again, same disclaimer, for research purposes I would manually validate
each of these accounts. From the 116th Senate, we have handles for 92
Senators’ Facebook pages

# facebook-scraper for scraping pages

[facebook-scraper](https://pypi.org/project/facebook-scraper/) is
package written in Python for scraping public Facebook pages without the
need for an API key. on is simple enough:

`$ pip install facebook-scraper`

Currently the package doesn’t support scraping multiple pages, however I
hear they are currently or planning on providing support for this. For
now there are ways to get around this (probably in ways far easier than
I set out here).

Here’s an example of using `facebook_scraper` to get posts by Bernie
Sanders

-   Almost forgot, if you use this hell-app by choice use a VPN, I’m
    currently in Ukraine

``` python
from facebook_scraper import get_posts
listposts = []
for post in get_posts("senatorsanders", pages = 50):
  print(post['text'][:50])
  listposts.append(post)
  ])
```

We could do a nested for loop over accounts and write this to csv.
However, I opt to use the CLI and generate the command for pulling posts
for each Senator.

We can do this in R

``` r
# The CLI Usage example from the repo: $ facebook-scraper --filename nintendo_page_posts.csv --pages 10 nintendo
# Let's recreate this /senator
page_count <- 5 # Set to whatever you like, this goes backwards chronologically

commands <- paste0("facebook-scraper --filename ", sen116$facebook, ".csv", " --pages ", page_count, " ", sen116$facebook)

commands[1]
```

`[1] "facebook-scraper --filename dougjonessenate.csv --pages 5 dougjonessenate"`

These commands can then be passed through the command line, storing
posts from x number of pages into the csv file
\[senators\_facebook\].csv.

We can then load these into R by generating the paths, binding the posts
and merging in metadata from out dataframe of Senators.

``` r
paths <- paste0("/Users/danbraby", sen116$facebook, ",csv")

posts <- lapply(paths, read_csv)

posts <- posts %>% bind_rows()

posts <- left_join(posts, sen116.csv, by = c("Username", "facebook"))
```

On a final note, sometimes you’ll be required to provide a path to a
cookies.txt file. This can be achieved by logging into Facebook (maybe
use a burner) and using an extension like EditThisCookie (Chrome) or
Cookie Quick Manager (Firefox) export this file and point to it in the
command:

`facebook-scraper --filename senatorsanders.csv --cookies "/Users/danbraby/cookies.txt" --pages 5 senatorsanders`
