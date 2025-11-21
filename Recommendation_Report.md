Author: Aman Mujawar
1. Introduction

Recommendation systems play a central role in helping users discover relevant content across digital platforms. This project implements three different recommendation strategies and compares their performance using the MovieLens 100K dataset. The three methods are:

User-based collaborative filtering

Item-based collaborative filtering

A Pixie-inspired random walk algorithm for graph-based recommendation

Each method highlights a different way of modeling user preferences and item relationships. The goal is to explore how these approaches behave on sparse ratings data and to understand their strengths and limitations.

2. Dataset Description

The MovieLens 100K dataset contains:

943 users

1,682 movies

100,000 ratings on a scale from 1 to 5

Three files are used:

u.data: ratings with timestamps

u.item: movie metadata

u.user: user demographics

Preprocessing Steps

All data files were loaded with appropriate separators and encoding.

Unix timestamps were converted to datetime objects.

The dataset was checked for missing values. Only one movie had a missing release date.

Ratings were normalized per user for the graph-based algorithm to reduce bias from users who consistently rate higher or lower than others.

Cleaned datasets were exported as CSV files.

3. Visualizations and Their Purpose

Visualizations were added to support understanding of the underlying data and to justify the design choices of each algorithm. Because recommendation datasets are typically sparse, visual inspection helps reveal how users and movies are distributed and where the model may struggle. For example:

Rating frequency plots highlight how often each rating value occurs.

Sparsity visualizations show that the user–movie matrix is more than 90 percent empty.

Similarity heatmaps reveal clusters of similar users or movies.

These patterns help explain why certain algorithms work better in specific scenarios. For example, the Pixie-inspired random walk performs well even in sparse environments because it can explore indirect relationships through multi-hop paths.

4. Methodology
User-Based Collaborative Filtering

User-based collaborative filtering identifies users who rate movies similarly. Cosine similarity is applied to rows of the user–movie matrix. For each unrated movie, a predicted rating is computed from similar users’ ratings. The system then recommends movies with the highest predicted values.

This method works well when many users share rating behavior patterns but can be computationally expensive when the number of users is large.

Item-Based Collaborative Filtering

Item-based filtering compares movies based on the users who rated them. It transposes the user–movie matrix and computes cosine similarity between movies. This method is stable over time because movie characteristics do not change as frequently as user behavior.

It is particularly effective when the number of items is smaller or more consistent than the number of users.

Pixie-Inspired Random Walk Algorithm

This method represents the dataset as a bipartite graph where users and movies are nodes connected by edges weighted by normalized ratings. A weighted random walk is performed:

Starting from a user or movie

Edges with higher weights are more likely to be chosen

Movies visited more often during the walk are ranked higher

This approach captures multi-hop relationships, enabling recommendations that may not be found through similarity alone. Normalized weights also help center user preferences so that high and low rating habits do not dominate the walk.

5. Implementation Summary
Data Loading and Matrix Construction

All data files were cleaned and converted into DataFrames.

A pivoted user–movie matrix was created and used for similarity calculations.

A bipartite graph and weighted adjacency structure were created for the Pixie algorithm.

Similarity Computation

Cosine similarity was used for both user and item matrices.

The similarity matrices were stored and indexed for fast lookups during recommendation generation.

Graph Construction

Each user and movie was represented as a node.

Edges connected users to movies they rated, weighted by normalized ratings.

The graph included more than 2,600 nodes and nearly 95,000 edges.

Random Walk Implementation

The walk alternates between users and movies.

Transitions are based on weighted probabilities.

Movie visit counts determine recommendation ranking.

6. Results and Evaluation
User-Based Recommendations

User-based filtering produced reasonable recommendations by identifying users with similar rating histories. Performance depends heavily on the number of co-rated movies.

Item-Based Recommendations

Item-based filtering returned stable and intuitive matches because it focuses on movie relationships rather than user behavior.

Graph-Based Random Walk

The Pixie-inspired method often produced diverse recommendations because it explored multiple layers of the graph. It frequently surfaced movies that were not direct neighbors of the user's rated items.

Comparison

User-based filtering excels at identifying users with aligned preferences.

Item-based filtering excels at finding content similar to a specific movie.

The Pixie-inspired graph approach excels at identifying indirect but meaningful recommendations.

7. Limitations

The dataset is highly sparse, which limits similarity-based methods.

Cold start problems remain for users or movies with very few ratings.

Random walk methods require tuning of walk length and restart strategies.

Only rating data was used; genre or content features were not included.

8. Conclusion

This project successfully implemented three different recommendation approaches and investigated their behavior on a benchmark dataset. Each method offers unique advantages:

User-based methods leverage rating patterns.

Item-based methods rely on movie similarity.

Graph-based random walks capture deeper relational structure.

The visualizations, similarity analysis, and graph modeling together provide a comprehensive understanding of the dataset and the strengths of each recommendation approach.