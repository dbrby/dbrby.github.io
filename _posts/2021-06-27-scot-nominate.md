---
title: 'Scaling Models and scotpol: Legislative Voting Records'
date: 2021-06-27
permalink: /posts/2021/06/scot-nominate/
tags:
  - Scottish politics
  - Dimensional Reduction
  - Political Science
---


**Daniel Braby, Benjamin Guinaudeau & Marius Sältzer**

**Daniel Braby**


Earlier this year me and my friend and colleague Fraser Stewart
@fraserjfstewart released parlScot v1: a dataset of 1.8 million spoken
contributions in the Scottish Parliament.

The dataset provides an exhaustive record of transcripts from plenary
debates and committees in Holyrood as a rectangular dataframe, with rich
metadata for MSPs and convenient crosswalks to other databases.

So you might be wondering what actually went into making a dataset like
that and why do it? Well I saw a couple of problems/opportunities,
learned the programming I needed to and tinkered away. The fundamental
problem with the Scottish Parliament’s data is that for all 20+ years of
data collection there has never been an indexing system which generates
the name of whose talking. Thus, records of a days’ session have
inconsistent titles for speakers.

Thus, this data is still useful for qualitative analysis and archival
purposes, but for modern big corpus, text analytics this doesn’t cut it.
Ideally, for such text-as-data analysis we want a consistent indicator
of whose talking, as well as some covariates for our analysis. That is
all parlScot is, but it is very useful and with the right tools very
powerful for describing, predicting and inferring about Scottish
Politics.

As a further note, Sascha Gobel and Simon Munzert’s (@simonsaysnothing)
Comparative Legislator Database (CLD) and the accompanying R package
`{legislatoR}` is a powerful tool and framework for starting up
something like this. As of right now it covers 10 countries, 338
legislative sessions and 45,540 legislators.

The CLD and the [accompanying
paper](https://www.cambridge.org/core/journals/british-journal-of-political-science/article/abs/comparative-legislators-database/D28BB58A8B2C08C8593DB741F42C18B2)
is well worth a read. It solves a number of problems with existent
resources on legislative elites and does so in a minimal and
reproducible way.
