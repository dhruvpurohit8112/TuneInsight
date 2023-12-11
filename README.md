## TuneInsight

Design and implementation of different simple state-of-art models for a musical recommendation system from scratch, useful to suggest to users of an hypothetic music platform the most relevant songs they *might like* and they *might add to their playlist* of favorite songs based on the songs they previously have listened to on the platform.


## The project
Nowadays all major companies use *recommendation systems* to provide suggestions for items that are most pertinent to a particular user, and in this project, some of the main used recommender models have been implemented *from scratch* to evaluate, at the end, all their performance.

The models that have been implemented in this project are:
- **Unpersonalized recommenders:**
    - *Random Recommender*
    - *Popular Recommender*
- **Personalized recommenders:**
    - *User-based Collaborative FIltering*
    - *Item-based Collaborative FIltering*
    - *SVD*
    - *Content-based*

## Datasets
For this project a subset of the famous [**Million Song Dataset**](https://millionsongdataset.com) has been used. In particular *two* main datasets has been merged together to form the final dataset used in this project: the [**Echo Nest
Taste Profile dataset**](https://millionsongdataset.com/tasteprofile/) which contains (**user_id, song_id, listen_count**) triplets for 1M users and the [**Last.fm dataset**](https://millionsongdataset.com/tasteprofile/) which contain songs’ information in the form (**song_id, artist, title, tags**) where *‘tags’* stands for the genre of each song. The final merged dataset used contains *500k+* users and *8k+* songs with their relative genres


### Data Exploration
Most of the *500k+* users in the dataset have listened only *one* song *one* time as shown in Figure below. This could cause problems to the evaluation of the implemented models (because we didn’t have enough information on a user to suggest him songs and evaluate the correctness of our recommendations). Therefore, some dataset cleaning operations were carried out to remove all those users with a total number of listening less than a fixed treshold.


In the project, we have transformed all the **implicit ratings** to **explicit** ones by computing the *listening frequency* for every song
listened by every user and normalizing it in a range [1-5] as if the user had rated all his listened songs **from 1 to 5 stars**. Then, from a simple analysis on the *distribution of all ratings* in the dataset it is easy to see that the most used rating has been *“1-star”* (due to the fact that most of our users have listened only one song).

![ratings_distribution-removebg-preview](https://user-images.githubusercontent.com/96207365/226417083-07e3e610-4a7e-4f9a-8a9d-45b5e0c4189a.png)


At the end of the Explorative data Analysis phase, also a visual representation of the **sparsity** of our **User-Item matrix** has been showed. The matrix has a sparsity of *98,44%*.

![sparsity-removebg-preview](https://user-images.githubusercontent.com/96207365/226417693-23390eee-0ff7-4f03-a084-dc07a9e0f7ff.png)



## Implemented methods
- **Random Recommender:** model that has also been used as baseline to compare the performance of all the other more complex methods. Thus, the random is the simplest recommendation model which proposes to the user *random songs* without taking into account his tastes.
- **Popularity-based Recommender:** it recommends to every user the *most popular songs*. In our work this model was build in such a way that the system doesn’t recommend the absolute most listened songs (because there are some users that have listened a single song even 200 times, and this outlier behavior could influence our recommendations), so, the model was implemented to suggest songs listened *by the major number of different users* (that is the definition of *‘popularity’*).
  
- **Memory-based Collaborative Filtering**:
    - **User-based CF:** it uses the similarity between users as the basis for the computation of the recommendations. For this method the *Pearson Correlation* has been used as *similarity measure* because, as specified in the scientific literature, this measure gives the best results for the recommenders in terms of performance. 


    - **Item-based CF:** it uses the *cosine-similarity* as similarity metric among the items to compute the recommendations for the users.


- **Model-based Collaborative Filtering:**
    - **Truncated SVD:** it uses mathematical models to produce a *score* for each item to recommend to every user. In particular, the *sparse User-Item matrix* is decomposed into 3 different matrices **U**, **sigma** and **V^T** useful to retrieve hidden common features between the items and useful to make better recommendations.


    - **Content-based:** instead of using only similarity measures between users or items, also some **features** of the listened songs are exploited to compute the recommendations. In particular, in this project the used features regard the **musical genre** of the songs, useful to make the right recommendations. 




In particular, in our project, for the *content-based model*, after the computation of the final user-item matrix with the computed *scores* for all songs to recommend, a further step has been done to improve the recommendations by calculating also the **Euclidean distance** between all users and generate the final recommendations taking into account also this *“user-similarity”* metric.


## Models evaluation
Different metrics have been used to evaluate the goodness of our models in terms of recommendations. 
**Precision@k**, **Recall@k** and **F1-score@k** metrics have been evaluated and their relative graphs are shown after each implementation. Plus, *only* for the *memory-based* models the *precision@k* and *recall@k* metrics have been evaluated *also for different threshold of
predicted ratings* used for the creations of the top recommendations. 

All these metrics have been computed also in relation to the **Top5**, **Top10**, **Top15**, **Top20**, **Top30** and **Top50** recommended songs, and as predicted from different studies in literature, the decrease of the *precision@k* value and the increment of *recall@k* when the number of the *topK* recommendation increase is shown in figure below. (The strange graph for the recommended songs with a rating threshold *>=2* is due to the *unbalanced* rating distribution).

Furthermore, for these *memory-based models*, also the **RMSE** value on the predictions has been
computed.

![prec_reca_userBased-removebg-preview](https://user-images.githubusercontent.com/96207365/226418359-7c5989a1-b419-41a1-a918-87dac8de8fd8.png)



At the end of our project, an **interactive graph** with all the *Precision@k* and *Recall@k* values **for all the implemented methods** is shown.

![all_graph](https://user-images.githubusercontent.com/96207365/226410766-a7d2fa51-499c-4436-9d52-4f99bc61c814.png)
