# Serendipitous-Clustering-for-Collaborative-Filtering

This repository contains the implementation of SC-CF (Serendipitous Clustering for Collaborative Filtering) as described in the paper [Scalable Clustering Algorithm for Collaborative Filtering](https://google.com) **Paper is under camera ready revision and link to the paper will be updated here soon.**

SC-CF is a clustering based algorithm that introduces the notion of "serendipity" in recommender systems. It employs a Scalable Spherical KMeans Plus Plus algorithm for carrying out clustering in the Collaborative Filtering part of the algorithm. The implementation assumes the dataset under consideration as [Serendipity 2018](https://grouplens.org/datasets/serendipity-2018/) which includes user-rating information for Movies provided by [MovieLens](https://movielens.org/). The codes for clustering can be used for any other dataset that is in the matrix format of data points.

Aftering downloading the Serendipity 2018 dataset, the entire directory needs to be placed inside the `Serendipitous-Clustering-for-Collaborative-Filtering` folder. 

### Requirements
* Python3 (the code runs only on python3)
* numpy
* collections
* sklearn

The repository contains 3 sub-folders. 
#### data processing
Inside `data processing`, you can find all codes required for generating user_profiles, movie_features, etc inside the `data processing` directory. Since the Serendipity data files have not been uploaded, we use a dummy datafile `data processing/trial.csv` just to test these codes. 

If you want to use the Serendipity 2018 dataset, comment out line no. `32` in `getUserRatings.py` and uncomment line no. `31`. If not, just follow the subsequent instructions.

To run the codes, execute them in the following order 

First to get the user-rating information in a matrix format

`python3 getUserRatings.py`

Then to extract genre and cast-directory features of the movies

`python3 getMovieFeatures.py`

Finally to generate user-profiles for each user 

`python3 getUserprofiles.py`  

In order to validate the recommendations made by our algorithm at a later point, we need to find the set of users common to both our test set and answers.csv provided in the dataset. For this, run `python3 getcommonusers.py` which will produce `validation_users.pickle` file inside `data` folder. 

The `distance_func.py` file is just a helper file required by the above three files.


### data
This folder contains all the pickle files generated by the above codes. These files include the user_ratings (both normalised and non-normalised), movies_map and user_map. These files will be required for further parts of the algorithm like clustering and predicting. 

### clustering
This folder contains all the python codes associated with the SPKM || clustering. 

To execute the clustering code, run `python3 simulate.py`.

This code will produce pickle files inside the `data` folder which include the centroids and the labels of each data point (which cluster it belongs to). The naming convention for these files are `centroids_{l}_{k}` where {l} can be `half` or `double` and {k} is the value of k. These file names will have to be mnaually changedi n the code when you use your own k values.

`simulate.py` will inturn invoke the `scalablekmeanspp_func.py` code which makes use of `distance_func.py` for calculating cosine distances. In order to change the metric of distance/similarity, only the `distance` function in this file needs to be changed. `wkpp_func.py` (used internally by `scalablekmeanspp_func.py`) has the code for the weighted k-means ++ part of the SPKM || algorithm for choosing the complete k-initial centers. The output of `simulate.py` is the conventional clusters of the data.

In order to obtain the serendepitous clusters, run `python3 get_ser_clusters.py` next. This will produce the `serendipity_cluster.pickle` file inside the `data` folder. Now the pre-processing and clustering parts are done. Prediction/recommendation and evaluation are remaining. 

In order to make presentations, run `python3 predict_rating.py`. It makes upto 1000 recommendations and prints the RMSE and MAE values of the predictions by comparing them with the ground truth (obtained from Serendipty dataset).

