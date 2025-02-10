# COMPUTER SCIENCE 464: Project Report
## Introduction
This project is an implementation of a Movie Recommendation System. The
system uses multiple methods of recommending movies based on the title of
the movie, then the user receives a list of movies that match the input. These
lists are based off of ratings, genres, movie descriptions, or the cast and crew
of a movie, depending on which method is being used.

This project uses [The Movies Dataset](www.kaggle.com/datasets/rounakbanik/the-movies-dataset/data).

## Related Work
The dataset being used is “The Movies Dataset” from Rounak Banik on
Kaggle. The dataset includes metadata from over 45,000 different movies
from the Full MovieLens dataset. It includes movies released on or before
July 2017 and it includes cast, crew, keywords for the plots, titles, vote
averages, vote counts, among a bunch of other useful data. There is another
file with ratings from over 270,000 users for the movies in the dataset, all of
which are sourced from the GroupLens website.

Related works that use this type of system include IMDb’s recommendation
system. Movies are recommended to users based off of their ratings of
movies, similar to the implementation in this project. Netflix and other
streaming services have systems that will recommend new movies or shows
to the user, so they stay on the platform longer.

Recommendation systems are not only for movies. They can be used for
recommending new products at an online retailer for users. Using
“Collaborative Filtering”, users and their ratings are compared with others
directly, providing a score on how much that user may like the product being
recommended to them.

## Proposed Method & Experiments
The first movie recommendation method is an extremely simple, bare bones,
and basic recommender. It simply orders the highest rated movies out of the
dataset of 45,000+ movies using a formula by IMDb, weighted rating:

![](images/image0)

where ‘v’ is the number of votes for a movie, ‘m’ is the minimum required
votes to be listed in a chart, ‘R’ is the average rating for a movie, and ‘C’ is
the average of the votes across the whole report. The way this can be
calculated in code is as follows:

![](images/image1)

This function allows us to get a list of movies that are popular and rated
highly based on the average ratings and amount of them. The list can bye
thought of a “popular movies” list or a trending list of movies.

![](images/image2)

This weighted rating function can be reused to then search for best rated
movies based on an input genre, instead of a title. Below is a search for
“Romance” movies:

![](images/image3)

The next method approaches the problem of recommending movies based on
their descriptions. This is accomplished by using “Term Frequency-Inverse
Document Frequency” or TF-IDF. This method calculates the amount of
times a term is used in a singular instance or description compared to the
amount of times that term appears in the dataset as a whole. This is then
multiplied by the log of the number of descriptions divided by the amount of
descriptions with that term in it. This then gives a matrix where each column
is a word or term in the description vocabulary, where each row is a movie.
The next step is to calculate a similarity score. There isn’t one right way to
calculate this, but the implementation in the project is using cosine similarity:

![](images/image4)

An example of a function that uses the cosine similarity is a method that is
used to recommend movies based on the title of the movie, using the terms in
the movie’s description to recommend movies with similar terms:

![](images/image5)

Going further with this method, we can alter the function that is used to
recommend movies to use the cast, crew, and genres to give us another set of
recommendations. To accomplish this, all of the metadata is compiled into a
single column, a “soup” of a sort. The director of the movie is alienated and
then tripled when put into the column to be more prominent when comparing
it to other movies, as the director is typically a crew member that people
search for in their movies. Instead of using TF-IDF for this method a “Count
Vectorizer” is used instead as to not weigh down the director of the movies. A
new cosine similarity is used and then the above function is reused to make a
recommendation:

![](images/image6)
![](images/image7)

This method can still be improved further. The function can be changed to
include the weighted rating from the Simple Recommender. This allows the
function to use the scores from the weighted rating and then the
recommender that uses metadata:

![](images/image8)

The third and fourth methods for recommending movies are a bit more
personal to the user, rather than being more general. With “Collaborative
Filtering”, a score is predicted based on the dataset of movies. There are two
types of Collaborative Filtering, user-based and item-based filtering. This
project takes advantage of the item-based Collaborative Filtering. Item-based
Collaborative Filtering recommends items based on their similarity with the
items that the target user rated. It uses Cosine Similarity to accomplish this.
This can be accomplished using a few algorithms, including “Singular Value
Decomposition” (SVD) or a K-Nearest Neighbors algorithm. The method
used in this project is the SVD method.

Singular Value Decomposition is a latent factor model that helps leverage
scalability and sparsity issues that may arise in a project like this. It turns a
recommendations problem into an optimization problem instead. It uses
“Root Mean Square Error” as a metric for accuracy. Essentially, SVD maps
users and items into a latent space, making users and items directly
comparable to each other. The implementation used was about 80% accurate
from cross validation. There is some tuning that can be done to get that
percentage higher, but it didn’t happen for this project.

The “Hybrid” Recommender is a continuation of the Collaborative Filtering
method, but with some added flavor. This recommender will give a list of
movies based on the title of the movie for a specific user ID. This method is
personal to the user ID that is input into the algorithm. Every
recommendation for every user ID will be different. This method displays the
recommendations in order from their scores on how accurate the model is
predicting the user to be interested in the recommended movies.

![](images/image9)

For user ID 1, with the movie ‘Avatar’ being the movie we are searching for
recommendations for, we see that ‘T2 3-D: Battle Across Time’ is the highest
predicted movie to recommend to our user with subsequent recommendations
following it in order of score.

![](images/image10)

For user ID 500, with the movie ‘Toy Story 2’ being the movie we are
searching for recommendations for, we see that ‘Toy Story 3’ is the highest
predicted movie to recommend to our user with more recommendations
following.

If we were to input the same movie title but different user IDs, we would
receive different recommendations because this method is taking in that
user’s scores for the movies they rated. Everyone rates movies differently and
has different tastes in movies.

## Results and Discussion
The results of the project are satisfactory for me, although I acknowledge
there may need to be more wok done on this project and model to make them
more accurate. 80% accuracy isn’t the most accurate for recommending new
things to users, but then again, movies are very subjective and even if the
algorithm gives a perfectly scored recommendation, the user may not like the
movie anyway.

Improvements could include using another model to grant recommendations.
I mentioned that a K-Nearest Neighbors model could have been used, I just
hadn’t had the time to properly test or implement it to compare it to the
Singular Value Decomposition Model.

## Conclusions
In conclusion, the project was a fine learning experience, and could be
improved if that were so desired. Movie Recommendation is a fairly simple
concept, mostly using math and machine learning to come up with a solution.

## References
The following links are the sources that I followed and the dataset that I used
for the project.

Ahmed, Ibtesam. “Getting Started with a Movie Recommendation System.”
Kaggle, Kaggle, 2 Feb. 2024, www.kaggle.com/code/ibtesama/getting-started-with-a-movie-recommendation-system.

Banik, Rounak. “Movie Recommender Systems.” Kaggle, Kaggle, 6 Nov.
2017, www.kaggle.com/code/rounakbanik/movie-recommender-systems.

Banik, Rounak. “The Movies Dataset.” Kaggle, 10 Nov. 2017,
www.kaggle.com/datasets/rounakbanik/the-movies-dataset/data.
