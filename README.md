# my-movie-recommender

## Introduction

I like to watch movies but it's becoming increasingly difficult to find new movie choices. I'd rather not rewatch and I haven't had much luck manually scouring the web for random movie ranking lists. So, I started this project to develop a movie recommender system to discover new movies to watch that I will [hopefully] enjoy watching.

I will be using the [MovieLens 20M Dataset](https://grouplens.org/datasets/movielens/20m/) and various recommender techniques. Below is a dataset description from grouplens:

>Stable benchmark dataset. 20 million ratings and 465,000 tag applications applied to 27,000 movies by 138,000 users. Includes tag genome data with 12 million relevance scores across 1,100 tags. Released 4/2015; updated 10/2016 to update links.csv and add tag genome data.

The first technique I'm using is Probabilistic Matrix Factorization (PMF). I've written the [core algorithm from scratch](https://github.com/acetherace/my-movie-recommender/blob/master/src/PMF.py) myself, so this will be a good opportunity to see how well my code performs on a large dataset and generally how well the PMF method performs. My current plan is to perform all computations locally; if this becomes infeasible I will look into cloud computing options.

The general structure of this project is to store classes and other source code in the `src` directory and IPython Notebooks in the `dev` directory. The presentation of the work will be in notebook form.

## Developing My Ratings Profile

It didn't take long to realize that manually rating over 27,000 movies was not exactly feasible. To adjust for this, I decided to filter the list of movies for those I was more likely to have seen. I approached this from several different angles, each filter bringing my task of creating a ratings profile closer to being a reasonable undertaking:

1. **Title length.** I noticed a lot of foreign movies tend to have longer titles. A lot of times they have a translation of the title appended to the original title. So I decided to filter out movies with titles greater than 60 characters. (I chose 60 because 'The Lord of the Rings: The Fellowship of the Ring' is 49 characters and I wouldn't want to miss out on that!). This brought the list of movies down from **27278 to 26184**. Not that great, but a start.
2. **English words.** If the title of a movie isn't in English, there's a good chance I've never seen it. So I downloaded a large list of English words and excluded movies with titles having more than 2 words not in that list. This brought the list down from **26184 to 22501**. Not bad.
3. **Release year.** I was born in the late 80's, and have never been a big fan of older movies. Maybe I'm missing out, but regardless, if the movie was released before 1990, I probably haven't seen it. List dropped from **22501 to 13882**. Now that's what I'm talking about. But wait a second... 13,000 is still way too many. I don't want to spend my weekends punching in numbers from 1-5.
4. **Rating frequency.** The movie rating frequency in the 20M dataset has a mean of **747**, a standard deviation of **3085**, a max of **67,310**, a median of **18** and is heavily right skewed. I chose my cutoff to be 200, which brought my list down from **13882 to 4055**. 4000 sounded doable, so I did it.

Of the 4055 movies I was prompted with, I ended up rating 612 of them. Here is the distribution of my ratings:

5 stars - 260
4 stars - 215
3 stars - 107
2 stars - 24
1 star  - 6

I guess I tend to give movies high ratings. I wonder what the population distribution is on movie ratings and if it follows the same trend. There are probably some interesting sociology/psychology studies on this.

Regardless of how my distribution stacks up with everyone else, I don't expect it to affect the results of the recommender.