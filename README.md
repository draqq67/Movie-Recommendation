# Movie Recommendation performed through Singular Value Decomposition (SVD)

***Zarnescu Dragos 321 AC***

As I was wondering what I was going to do in this project on the last days of the holiday, I was watching "American Made," starring Tom Cruise as a not so conformist pilot. It crossed my mind to create a movie recommendation algorithm for my Numerical Methods class project.

## How I saw this project being done?
  - **Firstly**, I thought about where to start on this project. I needed some data, anything that could provide me with some knowledge for the movies I am going to recommend. I finished the movie, which meant I had to log into my _Letterboxd_ account where I have all my stored data of any movie I watch.

  ### Scraping The Data
  - As Letterboxd has much information about movies, I decided to scrape the data so I could have a dataset to start with.
  - For scraping the data, I used *BeautifulSoup* and *requests*. I got the URL address of my account and started to see what information I could extract.
  - For the dataset, I scraped the data of the first 570 popular users of the platform (almost 4 hours), and I got around 690,000 ratings of almost 20,000 movies. I set up my `users_db` and `movies_db` which I am going to work with.

  ### Data Insights  
  - To better understand the data I am going to work with, I plotted the histogram of Normalized Frequency/AvgMovieRating of ratings to see a visual representation of how the average ratings are distributed and the cumulative probability associated with different rating values.
  - Then I plotted 2 plots of Ratings/Movie and NumberofMovies/NumberofRatings to see how the data performs.
  - Also, I wanted to see what the most popular words in movie titles are, so I plotted a WordCloud.

  ### User-Item Matrix
  - To have my dataset from where I can perform SVD, I created a matrix with the column of movies and the rows of users matching the ratings they have done.
  - For this dataset to be made, I imported _pandas_ to get a DataFrame of my matrix.
  - Because the dataset was so high, I filtered so that only the movies that have more than 100 reviews are taken into consideration. From 20,000 movies, I selected almost 3000, so that my data has a sparsity of 70%.

  ### Recommendation Algorithm
   - On this part, I set up the algorithm for receiving Movie Recommendations.
   - As I was searching how I can implement this type of recommendation, I found out there are 3 types of algorithms:
    #### Content-Based Filtering:
      - Users' profiles are constructed by explicitly asking users about their interests. These systems then make recommendations using the user’s profile features and the item’s attribute similarity.
      - It has some drawbacks as the system wouldn't recommend something outside their main genre as it couldn't find more deeply hidden movies that the user would like.
    #### Cosine Similarity based on Collaborative Filtering:
      - A metric based on the cosine distance between two objects, whereas as a matrix stand if we were to take two users' rows and find apply cosine similarity, it would be: _sim(A,B) = sum(Xai*Ybi)/||A||*||B||_ i from 1 to m.
      - To have the rating received, it would be _Rxi = sum(sim(x,y)*Ryi) /sum(sim(x,y))_ where Rxi is the rating we are looking for, sim(x,y) the similarity between users i and y from 1 to n.
      - Downside: It doesn't work well if users don't have movies in common, but their features are similar.
    #### Singular Value Decomposition based on Collaborative Filtering:
      - Extract features and correlation from the user-item matrix. This method shrinks the space dimension from N-dimension to K-dimension and reduces the number of features.
      - SVD constructs a matrix with the rows of users and columns of items, and the elements are given by the users’ ratings.
      - Singular Value Decomposition decomposes a matrix into three other matrices and extracts the factors from the factorization of a high-level (user-item-rating) matrix:
          - U: orthogonal matrix of users made of left singular vectors.
          - S: diagonal matrix of each latent factor.
          - Vt: orthogonal matrix of movies made of right singular vectors.
      - Problems: Lack of sparsity of U, Vt -> high computation, hard to interpret as singular vectors are linear combinations.
      - But gives a better approximation.
        ##### How I implement SVD
          - Let's say we have a matrix A and we want to write it as USVt.
             - First, we find eigenvalues of the AAt and then find eigenvalues and eigenvectors. We take the eigenvalues and put them as rows into our new matrix, and now we have U.
             - Then we compute AtA, we do the same, but we transpose the matrix. Now we have the matrix Vt.
             - Sigma would be the square root of eigenvalues of U or V based on the size.
             - To compute k-SVD, we would take only the first k eigenvectors and eigenvalues.
  ### Algorithm Set Up
   - From the user-item matrix that I have, I would first normalize the matrix, then I perform SVD, and by that, I get a matrix of users, features (the strengths which apply), and movies.
   - Visualizing the latent factors (U, Vt) to view relationships between users and items.
   - Visualize how movies contribute to each latent factor.

  ### Matrix Reconstitution
   - I apply SVD to the normalized filtered user-item matrix created from the data I scraped from Letterboxd where columns are movies and rows are users, populated with the given ratings.
   - Then I reconstruct the matrix, create a DataFrame of the predicted data, and return the relative error, meaning the norm_difference/norm_original.

  ### Predictions
   - First, I predict recommended movies unseen by a random user in my dataset.
   - Then I make predictions for any user in my dataset, where I scrape his data, append it to my dataset, and perform SVD once again.
   - In this way, my dataset gets trained by every user that inputs data.

