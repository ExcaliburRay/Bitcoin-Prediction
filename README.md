Bitcoin Price Prediction
Bitcoin Price Prediction	1
Data Preprocessing	2
1.Remove non-English items & time processing	2
Process Tweet Post	2
Process Reddit Post	2
2.Bitcoin prices percentage	2
3.Sentiment	3
Vader	3
SvelteMoji API	3
Group By Time	3
Combine Sentiment	4
4.Influencer	4
Find Influencer	4
Group By Time	4
Combine Sentiment	5
Econometrics Model	5
Split Bucket-6hr	5
Find optimal lag	6
Drop records without sentiment information	6
Models	6
Testing price + sentiment model with split buckets	6
Testing price-only model with split buckets and simple trading strategy	7
LSTM Model	8
Models	8
Split Bucket-24hr	8




Data Preprocessing
1.Remove non-English items & time processing

Process Tweet Post
Source Code: Preprocessing_tweets.ipynb
Input FIle: raw_tweets.csv 
Output File: Tweet_eng.csv*
* This is the file after removing nonEnglish rows
Process Reddit Post
Source Code: Preprocessing_reddit_post.ipynb
Input FIle: raw_reddit_post.csv
Output File: processed_reddit_post.csv*

* This is the file after removing nonEnglish rows, running Vadder sentiment, parsing the Integer timestamp to datetime, and round the time to hours.
2.Bitcoin prices percentage
InputFileCodeFileOutputFileraw_bitcoin.csvbitcoin_percentage.csvbitcoin_percentage.csv** This file has a new column of the bitcoin prices six hours later, a column of the percentage change between the price now and price six hours later.
InputFile:  
?
OutputFile:  
3.Sentiment
We got sentiment values from Vader Sentiment and SvelteMoji API. Further works are based on the sentiment result from SvelteMoji API.
Vader
In the first step, Preprocessing_reddit_post.ipynb and Preprocessing_tweets.ipynb already includes the code part for preprocessing. Therefore, processed_tweets.csv and processed_reddit_post.csv already have the vader sentiment tags and scores available.
SvelteMoji API
InputFileCodeFileOutputFileTweet_eng.csv
Svelte_API.ipynbPreprocessing/Post_api_output.zip
Preprocessing/Tweet_api_output.zipGroup By Time
InputFileCodeFileOutputFileFolder: post_api_output
processed_reddit_post.csvpost_hourly.ipynbpost_average_maximum.csv
InputFileCodeFileOutputFileFolder: tweet_api_output
tweet_eng.csvtweet_hourly.ipynbtweet_average_maximum.csvCombine Sentiment
InputFileCodeFileOutputFilePost_average_maximum.csv
tweet_average_maximum.csvpost_tweet_hourly.ipynbcombined_average_maximum.csv
InputFileCodeFileOutputFilecombined_average_maximum.csv
Merge price & influencer.ipynbcombined_average_maximum_null.csv
4.Influencer
Find Influencer
InputFileCodeFileOutputFileFolder: Tweet_api_output
tweet_eng.csvinfluencer_tweet.ipynbinfluential_tweet_user.csv
InputFileCodeFileOutputFileFolder:
post_api_output
processed_reddit_post.csvinfluencer_reddit_post.ipynbReddit_influencer_and_weight_new.csv + reddit_influencer_with_weighted_sentiment_new.csv
? influencer_input_bitcoin.csv
Group By Time
InputFileCodeFileOutputFileFolder: post_api_output
influcner_input_bitcoin.csvpost_hourlypost_average_maximum_influence.csv
InputFileCodeFileOutputFileFolder: tweet_api_output
Tweet_eng.csv
influential_tweet_user.csvtweet_hourly_influencer.ipynbtweet_average_maximum_influence.csvCombine Sentiment
InputFileCodeFileOutputFilePost_average_maximum_influence.csv
tweet_average_maximum_influence.csvpost_tweet_hourly.ipynbcombined_average_maximum_influence.csv
InputFileCodeFileOutputFilecombined_average_maximum_influence.csvMerge price & influencer.ipynbcombined_average_maximum_influence_null.csv

Econometrics Model
Split Bucket-6hr
The source code is to label the data into four buckets and generate the processed data as input for econometrics model.

Read File: combined_average_maximum_influence_null.csv
Output File: 6hour_price_emotion_4label.csv (The output for the second econometrics model)

Find optimal lag
The source code is to find the optimal lag with ADF Test for subsequent econometrics model. The output lag is indicated by Lags Used as shown below. The notebook file name is 1.AR Model - find optimal lag.ipynb



Read File: gemini_BTCUSD_1hr.csv
Drop records without sentiment information
The source code is to remove inconsistency of price and sentiment for further model training. Records that have no sentiment score (0) are removed. The notebook file name is 2.price_sentiment_zero_label_6h.ipynb

Read File: price_emotion_4label_6hour.csv
Output File: price_sentiment_zero_label.csv 

Models
The source code is to construct autoregression model with price input as well as price and sentiment input and use them to make predictions, plotting graphs for result. If you want to zoom in or out the graph, change the parameter of figure function as the following:

 The notebook file name is 3.Graph & dataframe - 6h.ipynb
 Read File: (1)  price_emotion_4label_6hour.csv (for price only model)	
                 (2)  price_sentiment_zero_label.csv (for price + sentiment model)


Testing price + sentiment model with split buckets
The source code is to test training model (price + sentiment) with given bucket labels to generate the accuracy rate in predicting the range of change of price. 

 The notebook file name is 4.Price sentiment - bucket.ipynb
 Read File:	(1) price_sentiment_zero_label.csv (for price + sentiment model)


Testing price-only model with split buckets and simple trading strategy
The source code is to test training model (price only) with given bucket labels to generate the accuracy rate in predicting the range of change of price. The source code also includes the result of simple trading based on the prediction result. 

 The notebook file name is 5. Price_Only Test_Trading.ipynb
 Read File:	(1) price_emotion_4label_6hour.csv (for price only)










LSTM Model
Models
Regression Model read file: price_emotion_labeled.csv 
Classification Model read file: 24hour_price_emotion_4label.csv

We use TensorBoard to track and visualize model performance metrics such as loss and accuracy. We've commented out codes for connecting with TensorBoard because they only work in the Google Colab environment. If you are also using Google Colab, you can add the following lines of code to visualize the model structure and training process. If you are using Jupyter, here is the link for setting up TensorBoard: https://www.tensorflow.org/tensorboard/r2/tensorboard_in_notebooks


Then, you will get a link like this:

Please temporarily ignore this link, and insert one line in the fit model:


After training the model, you can go up and click the previous link, which will lead you to the TensorBoard interface.
Split Bucket-24hr
The source code is to label the data into four buckets and generate the processed data as input for LSTM classification model.

Read File: combined_average_maximum_influence_null.csv
Output File: 24hour_price_emotion_4label.csv (This is the output for LSTM classification model)

