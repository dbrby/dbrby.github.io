---
title: 'Scaling Models in Poltical Science'
date: 2021-06-25
permalink: /posts/2021/06/scaling_models/
tags:
  - Scaling Models
  - Dimensional Reduction
  - Political Science
---


**Daniel Braby, Benjamin Guinaudeau & Marius Sältzer**

Models of political competition, questions of representation and voting behaviour alike require an understanding of what policies political actors prefer, offer and promote. To make positive assessments of these processes, political scientists have developed and refined tools to measure the relative positions of political actors on dimensions of politics.

Where do parties or voters stand on the left-right dimension? What policies define it? Is left-right really the most salient dimension in a political system? To answer these questions, so-called scaling models have been developed to estimate the positions of actors on the basis of their observable behaviours. To this end, many data sources have been explored: from voting behaviour in parliament (Poole and Rosenthal 1985), over party manifestos (Budge 2001) and parliamentary speech (Lauderdale and Herzog 2016), to positions in social media networks (Barbera 2015). This contribution gives an intuition on how this can be achieved, an introduction to the math behind them and provides an overview of their applications, both technical and substantive. We close with criticisms of these methods, which speaks to the development of further validation methods.

## Dimensional Reduction and Politics

The making of political decisions is a complex process of selecting issues, formulating positions on them, publicly communicating these, making, adopting and implementing policy. What issues are relevant for often binary voting choices is not a priori given, but is aggregated by political actors, such as parties, legislators or political entrepreneurs to cohesive policy platforms that allow direct comparison. These "dimensions" of politics allow reducing complex multi-dimensional spaces into simple vote choices and offer some solution to the information dilemma of democracy.

To answer these questions, it is necessary to develop measurements for the positions of actors on different dimensions. The "spatial" interpretation is very intuitive to voters and political scientists alike. To voters, spatial understandings of politics serve as powerful metaphors, in which parties serve an aggregative function for informational reduction and policy-making across a multitude of issues. Faced with at times many party platforms voters consider parties positions relative to their own on a limited number of dimensions or substantive issues. This notion of spatial modelling, has its origins in the translation of geospatial to preferential proximity in economics (Hotelling 1929). Such theorising of preferential proximity between political actors has led to powerful concepts such as the Median Voter Theorem (Black 1948) and Downsian Spatial Voting (Downs 1957).

But where do voters or parties stand and what policy issues define these positions? These are crucial questions for studying many questions of keen interest to political scientists.

## Traditional Dimensions

One way of doing this is the a priori definition of what is important for politics, such as analysing the left-right dimension. Traditionally, in most western societies, the left-right dimension reflects positions on state intervention into the economy. However, other dimensions such as cultural values, cosmopolitanism versus nation state, liberal versus conservative or even integration into the European union, matter most for decision making (Kriesi et al, 2008).

All these dimensions can be measured using expert surveys (Polk et al, 2017), where scholars sort parties along defined axes. This is comparable to self-placement in general population surveys. Respondents are asked where they stand on the respective dimension from one to ten. Of course, this measure depends on respondents knowing the substantial meaning of the dimension. We can then think of these as relative positions based on voter estimates of party placements along common dimensions. This is not always clear, since not all political systems are defined by a single dimension, and what indicates these positions varies between contexts.


## Inductive Recovering of Dimension

As an alternative, the question of what constitutes the most important differences in political orientation, can be derived inductively (ex post) (Benoit & Laver 2012). Such models are called unsupervised, as they require no human input as to the substance of the dimension and autonomously discover and scale positions on the underlying dimensions which explain most difference in the data.

May it be from votes, responses, political language, or even social media behaviour, using mathematical algorithms to identify the most important differences is a well-established idea in political science. These approaches attempt to explain the largest amount of difference from the minimal number of "dimensions". Here we already see the analogy to information aggregation in substantive political terms. A statistical analogy is the use of factor analysis to identify correlated items to measure latent, underlying constructs. They allow turning individual decisions across items into scales, which differentiate types of correlated behaviour. This "scaling" applies to any given behaviour and allows clustering individuals along a common dimension. We attempt to identify clusters of behaviour over individuals that allows us to describe them as similar or different, based on these important dimensions.

There are two classes of algorithms that allow reducing dimensions: parametric and non-parametric. Non-parametric approaches usually apply singular value decomposition (SVD) to decompose a matrix representing individuals and nominal data on some behaviour of interest. Factor analysis, correspondence analysis and multidimensional scaling all belong to this group.

The second class are algorithms from Item-Response-Theory, using maximum-likelihood or Bayesian methods to iterate over all combinations of individual and behaviour to fit all coefficients simultaneously. The result is an explicit modelling of estimation errors, but they are computationally intensive and thus take considerably more time to estimate. In reality, these algorithms all describe the similarity of the actors and their behaviour simultaneously. What this behaviour is and therefore represents, depends on the behaviour used for scaling. In other words, any matrix of behavioural units can allow us to position actors in a n-dimensional space, where n is the number of decisions, they all decide on. This very general approach lets us apply these methods to numerous behavioural traces so long as we have comparable count or relative count data.

## What to Scale

Traditionally, roll call votes (Poole and Rosenthal 1985) are used as a way to derive ideology from voting behaviour. In party-centred systems, absence of informative RCVs leads to a stronger focus on alternative sources such as political text produced by political parties or individual legislators (Beauchamp 2012). Social media has become an important data source for these endeavours, as it exists in most political contexts and involves most political actors. Behaviour on social media is also individuated and, in contrast to parliamentary speeches, less likely to be affected by institutional provisions. Two main data sources can be exploited on social media: the social network, i.e., the interconnection between the accounts, and the text entailed in the posts and/or comments. The next section will give a brief overview of how to estimate policy positions using each of these data sources.

### Behaviour

For US legislators, a popular application for measuring policy positions has been through Roll-Call Votes (RCV). The underlying argument is that by voting in costly elections in Congress, legislators reveal their preferences on each proposal. Knowing how each legislator voted on each proposal allows us to derive central dimensions of conflict. The NOMINATE (or Nominal Three-Step Estimation) is a multidimensional scaling application for RCVs developed by Poole and Rosenthal (1985). NOMINATE has gone through many iterations: Dynamic (Poole and Rosenthal 2001), Weighted (Poole et al, 2007), Dynamic-Weighted (Carroll et al, 2009) and more recently alpha-- a fully Bayesian, Markov Chain Monte Carlo-based version of the original implementation (Carroll et al, 2013).

While there are significant technical differences between these versions of the NOMINATE scaling technique, they all rely on the same fundamental assumptions. Political choice data, in this case nominal legislative voting data, can be projected to low-dimensional Euclidean spaces. Positions are revealed on broader dimensions, which allow us to 1) make positive statements on legislator positions and 2) predict voting in subsequent roll calls.

The innovation of NOMINATE shows that ideal points of preference can be recovered from observing political choices. RCV is just one type of behaviour we can look at, there are many more. One variant of NOMINATE which has proven influential is PAC-NOMINATE. For PAC-NOMINATE, interest groups are treated like legislators who make binary choices regarding the candidates and campaigns they do or do not finance (McCarty et al, 2006). However, a far more popular implementation of deriving positions from campaign finance data is Bonica’s (2014) DIME+ (Database on Ideology, Money in Politics and Elections), which recovers ideal points not just for incumbents, but candidates, donors and interest groups alike.

While positioning legislators and candidates is valuable itself, it is often necessary to put voters and politicians on the same scale. One approach exploits the fact that politicians and citizens are increasingly using Social Media as a platform for news consumption and political engagement, most notably via Twitter. Given the structure of Twitter as a network of followers and followings, we can use these as binary choice data to infer positions of political actors. The most prominent application of this was developed by Pablo Barbera (2015), in which a Bayesian Spatial Model is implemented based on similarities in the followings of social media users. Assuming that social networks are homophilic, then ‘birds of the same feather tweet together’. Barbera’s model fits additional controls for the popularity of accounts. Given that party leaders and Government Officials are more likely to be followed by those who do not politically agree with them- this can be factored for using Bayes’ Theorem.

Indeed, actionable behaviours such as RCV and campaign finance are informative of preference, however these have limited transferability to other contexts where we might lack appropriate data, or in the case of many parliamentary systems, RCV is not informative of individual position. Instead, linking how political rhetoric differs by ideology has been a recent focus of political scientists.

### Text as Data

Where roll-call votes and campaign finance are not feasible, automatic text scaling is a promising measurement strategy for ideological positions. Using textual sources has many advantages: text is abundant and provides extremely rich detail which can be leveraged in the measure. In addition, textual documents, such as social-media posts or parliamentary speeches, are supposedly individual behaviours and thus well-designed data sources to measure individual ideological position. It is very intuitive to understand that to get a sense of the ideological position of Boris Johnson or Angela Merkel, their speeches will provide a much more fine-grained picture than their voting behaviour.

Still, scaling automatically and systematically based on text has proven to be challenging in many ways. For instance, most words are very rarely used and cannot be automatically linked with a given position. On the other hand, the most frequent words (stopwords) are mere noise and do not inform about a specific position. In addition, many words are polysemic -endorse a different meaning depending on the context- and will even signal different positions depending on the context. For instance, words such as liberty, taxes or immigration can all be used to signal both left-leaning and right-leaning position. Finally, word choices are shaped by many factors that are independent from the policy position. Scaling models are word choice models: they model word frequencies as a function of ideological positions. Unsupervised models deployed for these contexts often assume that the principal dimension structuring word choices is the left-right dimension. But, when writing a text, authors are influenced by many more factors: the topic of the debate, their mood, their audience. These factors create noise that needs to be sorted out of the estimation to ensure its accuracy. Here again, models are associated with different data sources.


### Manifestos

The assumption that party competition was driven by issue emphasis, and therefore allowing to position parties based on these issues, led to the emergence of the Comparative Manifestos Project (CMP) (Budget et al. 2001). In a gigantic effort, the issue emphasis of each manifesto sentence was coded for numerous political systems across time. Coding the content by issue, the relative emphasis of issues was used to measure the positions of political parties. To estimate positions, traditionally these codings were scaled, comparable to roll call votes. However, coding is an extensive and expensive task, and not all manifestos can be coded (Bräuninger et al 2012).

Algorithms can help to overcome this issue and estimate the position of manifestos at low-cost. Benoit and Laver (2003) propose Wordscores which identifies the left-right dimension using anchor texts (for application see Müller 2012, Bräuninger and Giger 2018, Kosmidis et al 2018, Gross and Debus 2018, Bräuninger et al. 2020, Gross and Jankowski 2020). Slapin and Proksch (2008) develop a fully unsupervised method, Wordfish, that does not require any anchor text and use it to accurately scale German party manifestos. To do so, Wordfish jointly estimates the position as well as their relationship with the words counts and assumes that the principal dimension structuring word choices is the left-right dimension (for application see Louwerse 2011, Catalinac 2015).

### Parliamentary Speech

Parliamentary speeches are one of the primary data sources when estimating the position of individual politicians. They have been analysed using both Wordscores (Herzog and Benoit 2015, Bäck and Debus, 2016, Slapin et al. 2018) and Wordfish (Proksch and Slapin 2010, Ceron 2012, Ceron 2013, Louwerse 2012, Klüver 2013, Bauman et al. 2015, Arzheimer 2015). When using unsupervised models, speeches should be carefully considered, because the recovered dimension can be misleading. Lauderdale and Herzog (2016) demonstrate that debates in the Irish Dail are primarily structured around the government-opposition divide rather than along the left-right dimension. To mitigate this problem, they propose Wordshoals that takes the debate structure into account and estimates a Wordfish position for each debate and then represents these positions in a low-dimensional space through Bayesian estimation. 

At least two further structural aspects can bias the scaling of parliamentary speeches: agenda setting (Slapin and Proksch 2009) and selection of speakers (Bäck, Debus & Müller, 2014). Finally, a very promising method targets the specific issue of polysemic words. Rheault and Cochrane (2020) take advantage of word embedding techniques to incorporate the context in their estimations. Using neural networks, they model the choice of a specific word as a function of both its context (defined as the 6 words appearing around the modeled word) and the author. In doing so, they provide some control for the context and can distinguish between the left-leaning and the right-leaning meaning of terms. Similarly, Watanabe (2020) introduces Latent Semantic Scaling, which provides a semi-supervised text scaling approach. Based on a given context character (and its synonyms), a fitted word window and two sets of seed words (positive/negative) word embeddings can be derived from an associative model and document positions recovered based on reciprocal averaging.

### Social Media

Inherently political language like manifestos and speeches are recently complemented by a new form of expression for politicians: social media. In comparison it is noisy, unstructured and not necessarily political; but it is also free, unmediated and free of selection biases often encountered in parliament. Boireau (2014) and Ceron (2016) first apply traditional scaling models (Wordfish) to this new text form and show meaningful differences on political axes. Temporao et al (2018) show the validity of these positions, by comparing them to vote advice applications and Barbera’s ideal point method. Sältzer (2020) shows that they represent a meaningful political space, even inside political parties, making social media a promising data source for positioning individuals. More recently, Ebanks et al. (2021) have shown that networks of retweets among US Senators and factor analysis of joint sentiment-topics discussed on social media derive similar positions in policy space.


## Criticism and Validation

While scaling positions has become a staple method in political science, its application is not without valid criticism.

### Meaning

Both ex ante and ex post interpretations have been criticized for not accurately relating to political conflict. Ex ante dimensions can impose a bias to the data, as scholars base their observation in other sources, which are potentially subjective and outdated.
For instance, one common critique of expert surveys such as Chapel Hill (CHES) is the relative invariance of the survey over time. On the other hand, automated scaling measures often recover dimensions that only loosely represent what we would expect as scholars to be the main dimension of conflict. Depending on the data source, the data generating process can introduce non-observed bias, such as agenda setting or an exogenous shock, which leads to confusing non-ideological differences with ideological ones. The most accepted applications in both approaches seem to be those that produce both "face validity" and “convergent validity”. Converging indicators is not evidence for validity if all indicators suffer from the same biases. In addition, as ideology is a moving target, specific measures could grow inaccurate and miss important shifts. For this reason, the robust and objective validation of scaling models is crucial.

### Validity

Face validity is an implicit form of criterion validity- Is the measure behaving in a way we would expect? This of course is a trade-off with the ability to measure unexpected changes. Often, long term established measures such as NOMINATE are used to validate newer methods. However, it is often impossible to make an argument of an improved scaling technique, if credibility is weighted against external validation of past metrics. For scaling models, there exist approaches for statistical validity, such as perplexity measures or the explained variation. However, those are often uninformative about what the dimensions really mean. For text models, the concept of semantic validity has recently been introduced to see whether the terms that define positions on dimensions actually relate to what we would expect to be the substantive policy content of underlying dimensions (Chan and Sältzer 2020, Chang et al. 2009).

### Continuity

A third major criticism lies in the question of metrics. Scales are per definition continuous. Traditional evaluations of correct classification, like the ability to correctly predict the class of a text, are categorical in nature. This makes computing criterion validity difficult, as scales live from the comparisons. Pairwise comparison is a promising way to tackle this issue. Instead of assigning a numeric score to individual cases, documents or authors are iteratively compared one to another. Coders must state for each pair of cases which one holds the most left-leaning or right-leaning position. The pairwise comparisons can then be modelled using a Bradley-Terry model, which allows estimating an ideological score. Comparing pairs of documents takes longer because each document has to be compared many times to be scaled precisely, but the task is considerably easier for human coders, and thus more reliable, than direct scaling on a numeric scale. Breunig, Guinaudeau and Roth (2021) asked young political leaders to compare pairs of German MPs. This new measurement strategy comes close to what could be a gold standard to an ideological measure within a specific context. It is certainly not the last measure to be developed, but this non-behavioral measure can now be used to validate behavioral measures and understand when speeches, roll-call votes or social-media can be used to estimate ideological position. The scope of pairwise comparisons only depends on the compared documents. If MPs from different countries are compared, they could then be scaled in one single continuum.



























	•	Arzheimer, K. (2015). The AfD: finally a successful right-wing populist Eurosceptic party for Germany. West European Politics. https://doi.org/10.1080/01402382.2015.1004230
	•	Barberá, P. (2015). Birds of the same feather tweet together: Bayesian ideal point estimation using Twitter data. Political analysis. https://doi.org/10.1093/pan/mpu011
	•	Beauchamp, N. (2011). Using text to scale legislatures with uninformative voting. New York University Mimeo.
	•	Benoit, K., & Laver, M. (2012). The dimensionality of political space: Epistemological and methodological considerations. European Union Politics, 13(2), 194-218.
	•	Breunig Christian, Guinaudeau Benjamin and Roth Simon. 2021.Measuring Legislators' Ideological Position using Pairwise-Comparisons. Working Paper
	•	Black, D. (1948). On the rationale of group decision-making. Journal of political economy. https://doi.org/10.1086/256633
	•	Bonica, A. (2014). Mapping the ideological marketplace. American Journal of Political Science.https://doi.org/10.1111/ajps.12062
	•	BUDGE, I. (2001). Validating Party Policy Placements. British Journal of Political Science, 31(01). https://doi.org/10.1017/S0007123401230087
	•	Carroll, Royce, Jeffrey B. Lewis, James Lo, Keith T. Poole, and Howard Rosenthal. "The Structure of Utility in Spatial Models of Voting." American Journal of Political Science 57, no. 4 (2013): 1008-028.
	•	Catalinac, A. (2016). From pork to policy: The rise of programmatic campaigning in Japanese elections. The Journal of Politics. https://doi.org/10.1086/683073
	•	Ceron, A. (2012). Bounded oligarchy: How and when factions constrain leaders in party position-taking. Electoral Studies, 31(4), 689-701. https://doi.org/10.1016/j.electstud.2012.07.004
	•	Ceron, A. (2014). Gamson rule not for all: Patterns of portfolio allocation among Italian party factions. European Journal of Political Research, 53(1), 180-199. https://doi.org/10.1111/1475-6765.12020
	•	Ceron, A. (2017). Intra-party politics in 140 characters. Party Politics, 23(1), 7-17. https://doi.org/10.1177/1354068816654325
	•	Chan et al., (2020). oolong: An R package for validating automated content analysis tools. Journal of Open Source Software, 5(55), 2461, https://doi.org/10.21105/joss.02461
	•	Chang, J., Gerrish, S., Wang, C., Boyd-Graber, J. L., & Blei, D. M. (2009). Reading tea leaves: How humans interpret topic models. Advances in Neural Information Processing Systems, 288–296. https://papers.nips.cc/paper/3700-reading-tea-leaves-how-humans-interpret-topic-model
	•	Downs, A. (1957). An Economic Theory of Political Action in a Democracy. Journal of Political Economy, 65(2), 135-150. https://doi.org/10.1086/257897
	•	Gross, M., & Debus, M. (2018). Does EU regional policy increase parties’ support for European integration. West European Politics. https://doi.org/10.1080/01402382.2017.1395249. 
	•	Ebanks, D., Yan, H., Alvarez, R.M., Das. S. & Sinclair, B. (2021). Legislative Communication and Power: Measuring Leadership from Social Media Data. Working Paper.
	•	Gross, M., & Jankowski, M. (2020). Dimensions of political conflict and party positions in multi-level democracies: evidence from the Local Manifesto Project. West European Politics. https://doi.org/10.1080/01402382.2019.1602816
	•	Herzog, A., & Benoit, K. (2015). The Most Unkindest Cuts: Speaker Selection and Expressed Government Dissent during Economic Crisis. The Journal of Politics, 77(4), 1157-1175. https://doi.org/10.1086/682670
	•	Hotelling, H. (1990). Stability in Competition. In The Collected Economics Articles of Harold Hotelling (pp. 50-63). Springer New York. https://doi.org/10.1007/978-1-4613-8905-7_4
	•	https://doi.org/10.1177/1465116511434618
	•	Klüver, H. (2013). Lobbying in the European Union: interest groups, lobbying coalitions, and policy change. https://books.google.com/books?hl=en&lr=&id=t_3RBCzPJv8C&oi=fnd&pg=PP1&dq=Lobbying+in+the+European+Union:+Interest+Groups,+Lobbying+Coalitions,+and+Policy+Change&ots=xXb1G9b48f&sig=xKdbv6ZtHO6r2ukGc5MtKsbl4_A
	•	Kosmidis, S., Hobolt, S. B., & Molloy…, E. (2019). Party competition and emotive rhetoric. Comparative Political …. https://doi.org/10.1177/0010414018797942
	•	Kriesi, H., Grande, E., Lachat, R., Dolezal, M., Bornschier, S., & Frey, T. (2008). West European Politics in the Age of Globalization. Cambridge University Press. https://doi.org/10.1017/cbo9780511790720
	•	Lauderdale, B. E., & Herzog, A. (2016). Measuring Political Positions from Legislative Speech. Political Analysis, 24(3), 374-394. https://doi.org/10.1093/pan/mpw017
	•	Louwerse, T. (2011). The Spatial Approach to the Party Mandate. Parliamentary Affairs, 64(3), 425-447. https://doi.org/10.1093/pa/gsr012
	•	Louwerse, T. (2012). Mechanisms of Issue Congruence: The Democratic Party Mandate. West European Politics, 35(6), 1249-1271. https://doi.org/10.1080/01402382.2012.713745
	•	McCarty, N., Poole, K. T., & Rosenthal, H. (2006). Polarized America: The dance of political ideology and unequal riches. MIT Press.
	•	Polk, J., Rovny, J., Bakker, R., Edwards, E., & Hooghe…, L. Research & Politics.
	•	Poole, K. T., & Rosenthal, H. (1985). A Spatial Model for Legislative Roll Call Analysis. American Journal of Political Science, 29(2), 357. https://doi.org/10.2307/2111172
	•	Poole, K. T., & Rosenthal, H. (2001). D-Nominate after 10 Years: A Comparative Update to Congress: A Political-Economic History of Roll-Call Voting. Legislative Studies Quarterly, 26(1), 5. https://doi.org/10.2307/440401
	•	Poole, K. T., & Rosenthal, H. (2007). on party polarization in Congress. Daedalus, 136(3), 104-107. https://doi.org/10.1162/daed.2007.136.3.104
	•	Proksch, S.-O., & Slapin, J. B. (2009). How to Avoid Pitfalls in Statistical Analysis of Political Texts: The Case of Germany. German Politics, 18(3), 323-344. https://doi.org/10.1080/09644000903055799
	•	Proksch, S.-O., & Slapin, J. B. (2010). Position Taking in European Parliament Speeches. British Journal of Political Science, 40(3), 587-611. https://doi.org/10.1017/S0007123409990299
	•	Rheault, L., & Cochrane, C. (2020). Word embeddings for the analysis of ideological placement in parliamentary corpora. Political Analysis. https://doi.org/10.1017/pan.2019.26
	•	Sältzer, M. (2020). Finding the bird’s wings: Dimensions of factional conflict on Twitter. Party Politics, 135406882095796. https://doi.org/10.1177/1354068820957960
	•	Slapin, J. B., Kirkland, J. H., & Lazzaro…, J. A. (2018). Ideology, grandstanding, and strategic party disloyalty in the British Parliament. American Political …. https://www.cambridge.org/core/journals/american-political-science-review/article/ideology-grandstanding-and-strategic-party-disloyalty-in-the-british-parliament/08E3BD520B35339C941D2B95E7CA6A4A
	•	Temporão, M., Kerckhove, C. V., & Linden…, C. V. D. (2018). Ideological scaling of social media users: A dynamic lexicon approach. Political …. https://www.cambridge.org/core/journals/political-analysis/article/ideological-scaling-of-social-media-users-a-dynamic-lexicon-approach/0173A3145A67CB89ACFC8DE09B30C482
	•	Temporão, M., Vande Kerckhove, C., van der Linden, C., Dufresne, Y., & Hendrickx, J. M. (2018). Ideological Scaling of Social Media Users: A Dynamic Lexicon Approach. Political Analysis, 26(4), 457-473. https://doi.org/10.1017/pan.2018.30
