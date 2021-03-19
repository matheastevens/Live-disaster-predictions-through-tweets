# Twitter as an Early Detection Tool for Disaster Detection  


*This project was developed by: 
Mathea Stevens, Noah Zuckerman, Maya Morales and Zachary Katsnelson for General Assemly in partnership with [New Light Technologies](https://newlighttechnologies.com/).*

## About the Project  
Twitter is increasingly being leveraged as a tool for early detection for many things - from stock prices to infectious disease outbreaks to election outcomes. This project leverages tweets to detect and flag disasters in real time, as well as aggregate location-related information to identify where a disaster is occuring.

Using popular NLP techniques, each tweets text is processed and passed through a recurrent neural network which was trained on a dataset of more than 90,000 properly labelled tweets aggregated from numrous sources online. The disasters included in this detection systems include:
 - Hurricanes
 - Floods
 - Earthquakes
 - Tornado
 - Bombings 
 - Wildefiles
 - Explosions
 - Noreaster
 - Mudslides


### Modeling:
<img src="/assets/model_.jpeg"/>

Our data was first passed into a pre-trained Word2Vec model in the embedding layer. It was then passed into a Bidirectional LSTM of 128 nodes, which was regularized using L2. To assure that the bias-variance tradeoff would be as low as possible, this was passed into a dropout layer. The data was then passed into a 16 node Dense layer before being output in a softmax layer for multiclass classification. The model was compiled with categorical crossentropy and adam optimization.

### Scraping Twitter

This is a low budget project and therefore only had access to Twitter's free API Tweepy, which has restrictions regarding number of tweets scraped, as well as adjacent information like followers, retweets and likes. To enhance our scraped data, we also used snscrape which scrapes the front-end of twitter on hashtages anda accounts. To ensure we were scraping somewhat relevant tweets, we compiled a list of top 100 accounts that are related to news outbreaks and disasters, such as @DisasterMedics, @RedCross and @FEMA. 

To ensure we were also accessing grassroots-level tweets, we also leveraged our robust, pretrained Word2Vec model for relevant keywords. For each of the disaster labels we are searching for, we selected the top 15 most similar unique words using the w2v.most_similar functionality. These keywords were then used to perform hashtag scraping.

The number of tweets scraped at any time varies depending on how many top accounts the user wishes to include, as well as however many tweets contain our predefined keywords. 

### Predictions, Location Extraction and Aggregation

Once the tweets have been accumulated, they are tokenized, lemmized and passed through our RNN for prediction. The model either predicts a tweet as neurtal (unrrelated to a disaster), or as one of the classes we are searching for. 

Because of the free API, location data is not included in the scrape, and even if it was, typically only around 30% of tweets actually tag geolocation. Therefore, to provide meaningful information relating to location of a predicted disaster, each tweet is reviewed and any location-related words are extracted, such as town, county and state names.

From there, the count of disaster-related tweets, and their locations are aggregated. If the count of for example, hurricane-related tweets reaches a certain threshold determined by the user, it is printed out on the screen along with the count of location mentions. As the scraping and predicting occur in real time, any time the number of mentions of any disaster surpass the threshold, it will be flagged, allowing the user to investigate the location, and whether the disasters effects are migrating based on location changes (for example a wildfire spreading from Fort McMurray to Fort MacKay).

### Mapping

Using the Google Maps API free-trial, the locations of disaster mentions can be plotted on an interactive Google map. In a productionized environment with a paid version of the Google maps API, the map would be updated in real time as well. 

![Screenshot](assets/map.png)



