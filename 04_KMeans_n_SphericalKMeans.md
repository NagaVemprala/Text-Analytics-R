04_Topic_Models
================
Naga Vemprala
2023-05-29

#### Text Clustering and Topic Models

**Machine Learning - ** - Machine learning focuses on creating
algorithms and models that let computers learn and make predictions or
judgments without having to be explicitly programmed. It is predicated
on the notion that computers are capable of learning from and adapting
to data, seeing patterns and making wise judgments or predictions.

-   In conventional programming, programmers specifically specify the
    instructions in the code they produce to tell a computer how to
    carry out particular tasks. Machine learning, on the other hand,
    involves training a computer to recognize patterns and relationships
    in data by utilizing data as its training set. The machine learning
    algorithms may evaluate data and derive insightful patterns or
    insights using statistical methods, and then they can apply what
    they’ve learned to anticipate the future or take action when faced
    with new or unforeseen data.

Machine learning algorithms come in a variety of forms. Mainly
supervised and unsupervised.

-   Supervised learning: each input example has a corresponding output
    or target label, and the system is trained using labeled data. The
    algorithm picks up knowledge from these labeled samples to forecast
    or categorize brand-new data.
-   Unsupervised learning: The algorithm is given unlabeled data and is
    required to identify patterns or connections by itself. It gains the
    ability to identify hidden structures in the data or to group
    together comparable data points.

An unsupervised machine learning method used in information retrieval is
document clustering. The goal of document clustering is to automatically
organize a large collection of documents into meaningful clusters or
categories without any prior knowledge or explicit labeling of the
documents.

Steps involved in document clustering include:  
1. Text Pre-processing. Cleaning the data by removing unnecessary words
such as stopwords, removing punctuation, removing numbers etc  
2. Feature extraction: employing word frequencies, term
frequencies-inverse document frequencies (TF-IDF), or word embeddings
produced by algorithms like Word2Vec or GloVe to transform the text data
into comprehensible numerical representations  
3. Similarity Measurement: The similarity between documents based on
their feature representations is assessed using a similarity metric,
such as cosine similarity or Euclidean distance. The similarity metric
calculates how similar two documents are on a quantitative level.  
4. Running clustering algorithm: Based on their similarity scores,
related papers are grouped together using a clustering method. K-means,
hierarchical clustering, DBSCAN etc.  
5. Evaluate and interpret: The resulting clusters are evaluated to
assess their quality and coherence. Precision, Recall, and F-scores are
used for benchmarking.

``` r
library(readxl)
library(stringr)
library(tidytext)
library(tidyverse)
library(tm)
library(qdap)
library(factoextra) # visualization - ggplot2-based visualizations 
library(fpc) # Plot clusters using a DTM matrix. Base plot style plotting 
library(cluster) # Required package to plot silhoutte plot 
library(skmeans) # For running Spherical K-Means clustering 
library(clue) # For running cluster prototypes. Cluster of words 
library(wordcloud)
library("textcat") # To detect language of text 
```

-   Read the dataset consisting of Blutooth speaker reviews from Amazon

``` r
text <- read_xlsx(paste0(getwd(), "/Input/BlutoothSpeaker_B09JB8KPNW.xlsx"))
```

``` r
text_pre_process <- function(corpus) {
  corpus <- tm_map(corpus, tolower)
  corpus <- tm_map(corpus, removeNumbers)
  corpus <- tm_map(corpus, stripWhitespace)
  corpus <- tm_map(corpus, removePunctuation)
  corpus <- tm_map(corpus, removeWords, stopwords(kind = "en"))
  return(corpus)
}
```

``` r
text$review_text <- str_replace_all(text$review_text, "\\r|\\n", " ")
text$language <- textcat(text$review_text)
text <- text %>%
  filter(language == "english") 
# Include sentences that contains at least 3 words
text <- text %>% 
  mutate(review_text <- str_replace_all(review_text, "[[:punct:]]", "")) %>% 
  filter(length(
    unlist(str_split(str_squish(review_text), " "))) >= 3) %>%
  select(review_text)
corpus <- Corpus(VectorSource(text$review_text))
corpus <- text_pre_process(corpus)
text_tm_processed <- corpus$content
text_tm_processed <- str_squish(text_tm_processed)
text_tm_processed <- as.data.frame(text_tm_processed)
names(text_tm_processed) <- c("text")
```

#### Perform K-means clustering

``` r
# Step 1: Extract the corpus first (This is required, however, it is redundant because the text is processed for cleaning using the corpus.)
corpus <- Corpus(VectorSource(text_tm_processed$text))
# Step 2: Create a DocumentTermMatrix (DTM) using the corpus. 
# Term Frequency - Inverse Document Frequency is used for creating the matrix. 
# Hint: Look at the help documentation: ?DocumentTermMatrix
dtm <- DocumentTermMatrix(corpus, 
                   control = list(bounds = list(global = c(50, 100) 
                                                ), 
                                  weighting = function(x) 
                                    weightTfIdf(x, normalize = FALSE)))

# Step 3: run the kmeans function. You need to pass the number of clusters you want to create. 
# pass 5 as input 
kmeans_model <- kmeans(dtm, 5)

# step 4 (Plotting output): created model "kmeans_model" has size element.It provides count of elements mapped to each cluster. Using this size, create a frequency bar plot. 
barplot(kmeans_model$size)
```

![](04_KMeans_n_SphericalKMeans_files/figure-gfm/5.%20Perform%20kmeans%20clustering%20using%20the%20base%20stats%20package-1.png)<!-- -->

-   Most of the documents are mapped to a single cluster which does not
    provide any interesting insights. The clusters can be visualized
    next to see how are these centroids of the clusters calculated and
    plotted on a reduced 2-dimension space.  
-   There are two types of visualizations displayed here. The first used
    a ggplot2 version using fviz_cluster() function from “factoextra”
    library. This can take the kmeans model directly with a dtm passed
    to the function as another argument. However, this approach will not
    work on the the spherical K-Means model which will be implemented
    next.

``` r
fviz_cluster(kmeans_model, data = dtm)
```

![](04_KMeans_n_SphericalKMeans_files/figure-gfm/6.%20Plot%20the%20clusters.%20It%20might%20not%20be%20of%20much%20help%20if%20all%20the%20documents%20are%20mapped%20to%20one%20cluster-1.png)<!-- --> -
Approach 2 for plotting clusters

``` r
plotcluster(cmdscale(dist(dtm)), 
            kmeans_model$cluster)
```

![](04_KMeans_n_SphericalKMeans_files/figure-gfm/6.%20Plot%20the%20clusters%20through%20distance%20measures.-1.png)<!-- -->

-   It is evident from the above two plots that the clusters are
    distinct, however, many datapoints are mapped to one cluster.
    Therefore, it would be better to go for spherical K-Means
    clustering.

#### Silhouette analysis:

-   Silhouette analysis can be used to study the separation distance
    between the resulting clusters. The silhouette plot displays a
    measure of how close each point in one cluster is to points in the
    neighboring clusters and thus provides a way to assess parameters
    like number of clusters visually. This measure has a range of \[-1,
    1\].

-   Silhouette coefficients (as these values are referred to as) near +1
    indicate that the sample is far away from the neighboring clusters.
    A value of 0 indicates that the sample is on or very close to the
    decision boundary between two neighboring clusters and negative
    values indicate that those samples might have been assigned to the
    wrong cluster.

``` r
euclidean_distance_matrix <- dist(dtm, 
                       method = "euclidean")
plot(silhouette(kmeans_model$cluster, 
                euclidean_distance_matrix))
```

![](04_KMeans_n_SphericalKMeans_files/figure-gfm/7.%20Silhouette-plot-1.png)<!-- -->
\#### Most of the documents are mapped to a single cluster. It is time
to extend the clustering approaches. - \*\*Sperical K-Means Clustering
(A cosine similarity based clustering method)

``` r
features <- data.frame(measures = c("compassion", "fierce", "trust"), 
                       King = c(2, 5, 2),
                       Queen = c(4, 1, 5))
feature_matrix <- as.matrix(features[, c(2,3)])

# Similarity between King and Queen 
cosine_similarity <- t(feature_matrix) %*% feature_matrix
cosine_similarity
```

    ##       King Queen
    ## King    33    23
    ## Queen   23    42

``` r
# Similarity between "compassion", "fierce", "trust"
cosine_similarity <- feature_matrix %*% t(feature_matrix)
cosine_similarity
```

    ##      [,1] [,2] [,3]
    ## [1,]   20   14   24
    ## [2,]   14   26   15
    ## [3,]   24   15   29

-   Euclidian distance represents the length of a line segment between
    two points. A few high-frequency points influence the overall
    length. Also, sentences with more common keywords influences the
    distance.
-   Cosine similarity - Cosine Similarity measures the cosine of the
    angle between two vectors in the space. It’s also a metric that is
    not affected by the frequency of the words being appeared in a
    document, and it is efficient for comparing different sizes of
    documents.

``` r
# Step 1: Extract the corpus first (This is required, however, it is redundant because the text is processed for cleaning using the corpus.)
corpus <- Corpus(VectorSource(text_tm_processed$text))
# Step 2: Create a DocumentTermMatrix (DTM) using the corpus. 
# Term Frequency - Inverse Document Frequency is used for creating the matrix. 
# Hint: Look at the help documentation: ?DocumentTermMatrix
dtm <- DocumentTermMatrix(corpus, 
                   control = list(
                     #bounds = list(global = c(100, 900)),
                     weighting = function(x)
                      weightTfIdf(x, normalize = T)))

# Step 3: run the Spherical K-Means function. The parameters required for running S K-Means are:
# 1. Number of clusters. It is set to 5.
# 2. Fuzziness (m) between cluster borders. It can be thought of as a border between clusters. How broad is the boarder that you want? Ranges from 1 and upwards. The higher the expected fuzziness the higher should be the value. Ideal value is 1.2 
# 3. nrums. Numbers of times the parameters are to be estimated. 
# 4. verbose to print or not to print the progress 
# pass 5 as input 
skmeans_model <- skmeans(dtm, 3, m = 1.2, 
                         control = list(nruns = 10, verbose = F)) # Setting verbose to FALSE to avoid lot of print output 
barplot(table(skmeans_model$cluster))
```

![](04_KMeans_n_SphericalKMeans_files/figure-gfm/9.%20Running%20Spherical%20K-Means%20clustering-1.png)<!-- -->

``` r
plotcluster(cmdscale(dist(dtm)), 
            skmeans_model$cluster)
```

![](04_KMeans_n_SphericalKMeans_files/figure-gfm/10.%20Plot%20the%20clusters%20using%20plotcluster-1.png)<!-- -->

``` r
plot(silhouette(skmeans_model))
```

![](04_KMeans_n_SphericalKMeans_files/figure-gfm/11.%20Create%20a%20Silhoutte%20plot%20using%20skmeans_model-1.png)<!-- -->

``` r
# Clusters of words are called cluster prototypes 
cluster_prototypes <- cl_prototypes(skmeans_model)
cluster_prototypes <- t(cluster_prototypes)
comparison.cloud(cluster_prototypes, max.words = 100)
```

![](04_KMeans_n_SphericalKMeans_files/figure-gfm/12.%20Create%20clusters%20of%20words%20and%20analyze%20the%20words%20based%20on%20the%20frequencies-1.png)<!-- -->

``` r
top_words <- sort(cluster_prototypes[,1], decreasing = T)

top_words <- as.data.frame(top_words)
top_words <- data.frame(row.names(top_words))
for (i in 2:3) {
  top_words <- cbind(top_words, 
                   row.names(as.data.frame(
                   sort(cluster_prototypes[,i], decreasing = T))
                   ))
}

colnames(top_words) <- c("cluster1", "cluster2", "cluster3")
top_words <- top_words[!( 
  (top_words$cluster1 == top_words$cluster2) &
    (top_words$cluster1 == top_words$cluster3)),]
top_words[1:5, ]
```

    ##     cluster1 cluster2  cluster3
    ## 97   connect  connect   airpods
    ## 98   airpods  airpods   connect
    ## 148 features features       son
    ## 149      son      son  features
    ## 181      say      say earphones
