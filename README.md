# Movie Recommendation performed through Singular Value Decomposition ( SVD )

 ***Zarnescu Dragos 321 AC***

As I was wondering what was I going to do in this project on the last days of the holiday, I was watching American Made, staring Tom Cruise as a not so conformist pilot, and it crossed my mind the idea of what if i make a movie recommendations 
alghoritm for my Numerical Methods class project.

_So here it is_

## How i saw this project to be done ?
  -**Firstly**, I was thinking from where can I start on this project, I needed some data, anything that can provide me with some knowledge for the movies I am gonna recommend. I was finishing the movie and by that it meant that I have to log on my _letterboxd_ account where I have all my stored data of any movie I watch.
  ### Scraping The Data
  - As letterboxd has such much informations about movies, i decided to scrap the data so i can have a dataset from where to start
  - For scraping the data I used *_BeautifulSoup_* and *_requestes_*, i got my url address of my account and started to see what information i can extract
  - For the dataset I scraped the data of the first 570 popular users of the platform( almost 4 hours) and I got around 690.000 ratings of almost 20.000 movies and I setted up my users_db and movies_db which I am going to work with

  ### Data Insights  
  - To understand better the data I am going to work with, I 
  - I plot the histogram of Normalized Frequncy/AvgMovieRating of ratings to see a visual representation of how the average ratings are distributed and the cumulative probability associated with different rating values.
  - Then I plot 2 plots of Ratings/Movie and NumberofMovies/NumberofRatings to see how the data performs
  - Also I wanted to see what are the most popular words in movietitle and I plot a WordCloud
  ### User-Item Matrix
  - To have my dataset from where I can perform SVD I create a matrix with the column of movies and the rows of user matching the ratings he has done
  - For this dataset to be make, I imported _pandas_ to get a DataFrame of my matrix
  - Because the dataset was so high, I filter so that only the movies that have more than 100 reviews to be taken in consideration so from 20.000 movies, i selected almost 3000, so that my data have a sparsity of 70%
  ### Recommendation Alghoritm
  - on this part i set up the alghoritm for receiving Movie Recommendation
  - As I was searching how can I implement this type of recommendation, I find out there are 3 type of alghoritms:
    #### Content-Based Filtering:
      - Users profiles are construct by explicitly asking users about their interests. These systems then make recommendations using the user’s profile features and the item’s attribute similarity.
      - it has some drawbacks as the system wouldn't recommend something outside their main genre as it couldn't fill more deep hidden movies that he would like
    #### Cosine similarity based on Collaborative filling
      - a metric based on the cosine distance between two objects whereas as a matrix stand if we were to take to users row and find apply cosine similiarity, it would be : _sim(A,B) = sum(Xai*Ybi)/||A||*||B||_ i from 1 to m
      - and to have the rating received it would be _Rxi = sum(sim(x,y)*Ryi) /sum(sim(x,y))_ where Rxi is the rating we are looking for sim(x,y) the similitary between users i y from 1 to n
      - downside : doesn't work well users don't have movies in common, but their features are similar
    #### Singular Value Decomposition based on Collaborative filling:
      - extract features and correlation from the user-item matrix. This method shrinks the space dimension from N-dimension to K-dimension and reduces the number of features.
      - SVD constructs a matrix with the row of users and columns of items and the elements are given by the users’ ratings.
      - Singular value decomposition decomposes a matrix into three other matrices and extracts the factors from the factorization of a high-level (user-item-rating) matrix:
            - U: ortogonal matrix of users made of left singular vectors
            - S: diagonal matrix of each latent factor
            - Vt: ortogonal matrix of movies made of right singular vectors
      - problems : lack of sparsity of U, Vt -> high computation, hard to interpret as singular vectors are linear combinations
      - but gives a better approximate
        ##### How I implement SVD
          - let's say we have a matrix A and we want to write it as USVt
             - first we find eigenvalues of the AAt and then find eigenvalues and eigenvectors we take the eigenvalues and we put it as rows into our new matrix and now we have U
             - then we compute AtA we do the same, but we transpose the matrix, now we have the matrix Vt
             - Sigma would be the square_root of eigenvalues of U or V base on the size
             - To compute k-svd we would take the only first k eigenvectors and eigenvalues
    ### Alghoritm set up
     - from the user-item matrix that i have, i would first normalise the matrix then i perform svd and by that i get a matrix of users, features( the strenghets which applies) and movies
     - Visualizing the latent factors (U,Vt) to view relationships between users and items
     - visualize how movies contribute to each latent factor
    ### Matrix reconsituition
     - I apply SVD to the normalized filtered user-item matrix created from the data i scraped from letterboxd where columns are movies and rows user, populated with the given ratings
     - Then I reconstruct the matrix , create a dataFrame of the predicted data and return relative error meaning the norm_difference/norm_original
    ### Predictions
     - First, I predict recommend movies unseen by a random user in my dataset.
     - Then I make predictions for any user in my dataset, where i scrape his data, append it to my dataset and perform SVD once again.
     - In this way my dataset gets trained by every user that inputs data


        

