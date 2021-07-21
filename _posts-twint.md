Scraping legislators’ tweets using twint
================
Daniel Braby

If you are willing to breach Twitter terms of service and assume they
won’t care, it is actually surprisingly easy to harvest tweets from many
accounts with significantly low waiting time.

This blogpost outlines a blended approach of using R and Python to
access data on legislators and their social media presence scrape those
and wrangle them together for some common text analysis approaches. I of
course don’t use these scraping tools because I respect Twitter terms of
service… however, I am convinced this is a better route than the
alternative of using the API, even the Academic Twitter Track. And keep
in mind you can just say you used the Academic Twitter route, whose to
say you didn’t.

# Finding legislators on Twitter

There are a few repos and databases online which provide coverage of
legislators’ social media presence. In an upcoming guest blog, me and a
team working on collecting Twitter data at scale, note that much of this
data is however flawed. Largely, this results from storing accounts by
their handles, which are variable, unlike user\_ids which are unique to
each account and constant despite changes in handles.

I personally side on, if you are going to take the time to download
tweets from an account, you may as well go through and verify each. Even
if this is 500 accounts, its better to get it right the first time and
make your data collection one unified command.

For this example I use the Comparative Legislators Database by Sacha
Gobel and Simon Munzert, and make calls to this through the convenient
interface package in R `{legislatoR}`. Knowing me, I’ll use their data
of Scottish MSPs for this example.

Below is the annotated code for downloading and stitching together
metadata and social media accounts for the 2016-2021 cohort of MSPs

``` r
require(legislatoR) # Interface to CLD
```

    ## Loading required package: legislatoR

``` r
require(tidyverse) # Handy Joins and wrangling tools
```

    ## Loading required package: tidyverse

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.2     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.0     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
sco <- "sco" # Having to do  quotation marks gets annoying store "sco" as an object

msps_1621 <- left_join(get_political(sco) %>% filter(session_start == "2016-05-05"), get_core(sco), by = "pageid") %>%
  left_join(., get_social(sco) %>%
              select(wikidataid, twitter), by = "wikidataid") %>% drop_na(twitter)

# Filter to MSPs by Session, 2016-05-05 being the 2016-2021 term
# Join political (MSPS by terms) to core (Ind. MSPs)
# Join again to incorporate social media data
# Remove variables except wikidataid for join and twitter
# Drop NA twitter values
```

For research purposes, I would check through these accounts, however for
this blog I’ll cut that corner. Assuming there has been zero false
attribution (account given != given legislator) or MSP changes handle
and someone then takes that handle, we can use a user\_lookup request to
ensure we have valid accounts

``` r
require(rtweet) # Wrapper to the Twitter API
```

    ## Loading required package: rtweet

    ## 
    ## Attaching package: 'rtweet'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     flatten

``` r
msps_1621$twitter <- msps_1621$twitter %>% tolower() %>% trimws()
#Twitter's API is not case sensitive, however it is useful to make handles lowercase and trimws just to be sure and for matching with the user_lookup data

u_lookup <- lookup_users(msps_1621$twitter)

# We have 96 valid accounts returned through the Twitter API

msps_1621 <- filter(msps_1621, twitter %in% tolower(u_lookup$screen_name))

# Filter dataset of MSPS, only keep Twitter handles in user_lookup returns

write.csv(msps_1621, "msps_on_twitter.csv") #Write to csv
```

# Twint for scraping Twitter ILLEGALLLY!!!!

Twint is an advanced Twitter scraping tool written in Python that allows
scraping Tweets from Twitter profiles without using the Twitter API.
This is written in BeautifulSoup4 and has several dependencies for use:

-   Python 3.6
-   aiohttp
-   aiodns
-   beautifulsoup4
-   cchardet
-   dataclasses
-   elasticsearch
-   pysocks
-   pandas (&gt;=0.23.0)
-   aiohttp\_socks
-   schedule
-   geopy
-   fake-useragent
-   py-googletransx

## Why twint?

Twint has several big advantages over using the API: it can fetch almost
all tweets from acccounts (the Twitter API, as far as I know, is limited
to the user’s last 3200 Tweets), it is easier to setup (you don’t have
to fill out responses to Twitter Qs, generate tokens, store them, create
an instance…) and most importantly it is not rate limited.

`$ pip3 install twint`

## Scrape legislators

Now in python we will read in our data, set up a for loop to pull tweets
for each of the 96 accounts.

``` python
import pandas as pd 
import twint

df = pd.read_csv("/Users/danbraby/Dropbox/dbrby.github.io/msps_on_twitter.csv") #Read in CSV

for i in df['twitter']: # Loop over accounts
  t = twint.Config()    # Initialise
  t.Username = i        # Username
  t.Store_csv = True    # Store as CSV
  t.Output = i          # Name of output
  t.Since = "2016-05-05"# Starting Data
  t.Until = "2021-05-05"# End Date
  twint.run.Search(t)   # Run
```

There is probably an easier way to scrap this data into one file, rather
than one per account. If you know a better way please lemme know!

Next onto reading these files into R.
