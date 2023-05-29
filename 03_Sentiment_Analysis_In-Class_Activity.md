Sentiment Analysis
================
Vemprala
2023-05-25

#### Opinion Mining or Sentiment Analysis & TF-IDF (Term Frequency - Inverse Document Frequency) representation

-   Sentiment Analysis is the process of extracting an author’s
    emotional intent from the text.
-   When more than one topic is discussed in a text and each topic has a
    distinct set of sentiments attached by the author, opinion mining
    becomes quite difficult. Therefore, extracting the topic from the
    text becomes crucial. However, topic extraction necessitates a
    different text representation known as TF-IDF. In favor of
    straightforward term frequency, the TF-IDF value increases with the
    occurrence of the term but is offset by the word’s overall frequency
    in the corpus. The offsetting effect helps in the removal of
    frequently occurring terms that may not provide useful information.
    For instance, if all reviews are about “Batman,” the “Batman”
    keyword will appear in all reviews. The inclusion of “Batman” in the
    analysis would yield no new insights.
-   Sentiment Analysis is based on a framework that classifies a set of
    keywords specific to a sentiment and assigns a score to the level of
    sentiment (emotion) captured by the keywords. There are eight
    psychologically evolved emotions, that are widely accepted:

1.  anger
2.  fear
3.  sadness
4.  disgust
5.  surprise
6.  anticipation
7.  trust
8.  joy

![](Images/Plutchik-wheel-of-emotions.png) - Beyond sentiment analysis
for emotional states, an easier approach is to merely state whether a
document is positive or negative. This is called polarity of text.

-   Approaches to sentiment analysis:

1.  tidytext lexicon approach
2.  “sentiment” package for polarity (Uses a Bayesian Approach)
3.  qdap implementation of polarity (Accounts for valence shifts)
4.  Machine Learning approach

Valence shifting is the change in emotions/sentiments due to negation in
sentences

Why does the subjectivity lexicons work?

-   Explanation is due to the Zip’s law and the priciple of least
    effort. The average person probably has tens of thousands of words
    in his or her personal vocabulary, so a limited subjectivity lexicon
    may seem inappropriate. Numerous additional factors, including
    ethnicity, gender, and demographics, influence the outcome.
    Therefore, it may appear that a lexicon with approximately 7,000
    entries would not be sufficient.

-   The average person probably has tens of thousands of words in his or
    her personal vocabulary, so a limited subjectivity lexicon may seem
    inappropriate. Numerous additional factors, including ethnicity,
    gender, and demographics, influence the outcome. Therefore, it may
    appear that a lexicon with approximately 7,000 entries would not be
    sufficient.

-   However, according to Zipf’s law, the frequency of a word in a
    document is inversely proportional to its rank of appearance. For
    instance, the most frequent word on a list will appear approximately
    twice as often as the second most frequent word, three times as
    often as the third, and so on. As a result, humans utilize a large
    number of repetitive words and exert minimal effort when
    communicating. All of the words are not required to analyze the
    overarching sentiment.

**Usage of Sentiment Analysis in real life scenarios**

-   Extract sentiment polarity of a product over time and make updates
    to the product
-   Understand the effectiveness of a marketing campaign
-   Social media data analysis to understand the public response to a
    new policy

``` r
library(readxl)
library(tidytext)
library(tidyverse)
library(tm)
library(qdap)
```

``` r
airbnb_reviews <- read_excel(paste0(getwd(), "/Input/Airbnb_reviews.xlsx"))
```

#### Approach 1. Using sentiment lexicons from tidytext.

-   There are four popular lexicons available under tidytext package.

1.  nrc
2.  afinn
3.  bing
4.  loughran

-   Extract the sentiment for each review using “bing” lexicon.
-   If we combine all the reviews into a series of sentences and then
    convert these sentences into words, the dimension per review is
    lost. Therefore, we must identify each review individually prior to
    conducting word-level analysis. The purpose of the word-level
    analysis is to determine whether the review words convey positive,
    negative, or other possible emotions. The word level sentiments are
    used to compute the overall review sentiment. (Reviewer_id is
    already present in the data. Make sure that it is unique for each
    review)
