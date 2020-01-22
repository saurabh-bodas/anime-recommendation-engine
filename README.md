# Building An Anime Recommendation Engine

Recommendation engines have a wide range of applications for a better customer experience - from on-demand content platforms to online retail shopping; from recommending the the most popular items within a category, to a personalized list specifically based on one's online activities.

For my marketing analytics class, I built an anime recommendation engine using collabroative filtering and content-based algorithms.

# The Data 

I used an anime dataset available from Kaggle (https://www.kaggle.com/CooperUnion/anime-recommendations-database) containing two files: 

**anime** - containing information about ratings, number of viewers, genre, number of episodes

**ratings** - the rating (between 1-10) that a user gives to each show ('-1' if a show was watched but not rated)

# Content-based algorithm

In this algorithm, similarity between the 2 animes is calculated based on features of the anime (genre, popularity, number of episodes, etc.) and the most similar animes are then recommended accordingly.

I have used the jaccard similarity and cosine similarity metrics to determine which animes are the most similar. 

## Jaccard similarity

Jaccard similarity is calculated as the intersection between two sets by the union of those sets. 

To calculate the jaccard similarity, I created a bag of words for each anime that represents all the features of the anime to input into the algorithm. The jaccard score would be calculating by dividing the number of common words between the two anime by the number of words present in both animes' bags of words. A matrix of anime ids for both columns and indices was then created to store jaccard similarity scores - higher the score, the more similar are the two animes. 

The algorithm outputs the most similar animes to the input anime based on these similarity scores. 

## Cosine similarity

Cosine similarity works by calculating the cosine of the angle between two vectors. I used text vectorization to convert the bag of words computed earlier into the two vectors, and then finding the cosine similarity scores. 

A cosine score of 0 represents no similarity while 1 suggests that the two vectors are (almost) same. The most similar anime are then determined basis these scores. 

**Note:** The primary difference between jaccard scores and cosine scores is how they treat repeated words. Jaccard similarity scores are not affected by duplicate words, since a unique count of words is considered, but cosine similarity scores increase with repitition. Since there wasn't any repitition of words in the word bag, the results from the two metrics are not very different.

**Note II:** The content-based algorithms would be improved if more information is provided: the plot description, actors, directors, location, language, etc.

# Collaborative filtering 

Our anime dataset contains two types of reactions from users: 

*implicit*: the user watched the anime but did not rate it (denoted by -1 in our dataset)

*explicit*: the user rated the anime between 1 and 10

Unlike the content-based recommendation algorithm, where similarities between animes are calculated, collaborative filtering involved calculating similarities based on the online behaviour of the users (the animes they watch and/or rate).

For the collaborative filtering algorithm, I followed a *two-step process*: 

1. Similarities are calculated between users based on the ratings they provide to different animes, and

2. From the animes watched by these users, those with the highest average ratings are suggested to the input user. (note that animes already rated or watched by the input user were filtered out).

*Note:* Those animes who had fewer than 4 viewers were filtered out as well.

For this algorithm, I created a pivot table of animes vs. users and the ratings provided to each anime. Because not every user will have watched every anime, there will be a lot of NA values - thus making the dataframe a sparse matrix. 

I centered all the ratings by subtracting each rating by the mean rating of each particular user. This allows us to fill in 0's instead of NA values, thus allowing the use of the cosine similarity metric.  

There are numerous ways to deploy recommendation engines based on the dataset and the business context. The python Jupyter notebook files show my code for this project. 

