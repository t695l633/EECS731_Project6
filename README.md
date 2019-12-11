# EECS731_Project6
Anomaly Detection

# Instructions

The real world does not slow down for bad data
1. Set up a data science project structure in a new git repository in your GitHub account
2. Download the benchmark data set from
https://www.kaggle.com/boltzmannbrain/nab or
https://github.com/numenta/NAB/tree/master/data
3. Load the one of the data set into panda data frames
4. Formulate one or two ideas on how feature engineering would help the data set to establish additional value using exploratory data analysis
5. Build one or more anomaly detection models to determine the anomalies using the other columns as features
6. Document your process and results
7. Commit your notebook, source code, visualizations and other supporting files to the git repository in GitHub

# The data

The data used in the project came from NYC taxi volume throughout 2014 and 2015. There were already 5 known anomalies in the data, Thanksgiving, Christmas, New Years, the NYC Marathon, and a blizzard. Each row contained a timestamp, taken every half hour, and the total number of taxi's called during that half hour. I chose to do analysis on both the raw number of taxi's called, as well as treating the data like a time series to identify patterns in the data.

# The Goal

The goal of this project was to identify anomalies in the data set. I chose the data set with known anomalies so that I could verify my results. I used an isolation forest model to predict anomalies within my data set, using weekday, time, and date as features while taxi volume was the dependent variable I was looking at.

# Feature Engineering

I tried several pieces of future engineering in order to expand my data set to create more value. First, I calculated statistics of my taxi volume values and used them to create a stdFromMean column, which listed the standard deviations each volume was from the mean volume. Next I took a closer look at my data, and saw that there were clear patterns that repeated themselves every week. For this reason I broke down the timestamp and added a day of the week column, and a time column. This helped me detect anomalies within a certain day or time, instead of comparing taxi volume to the average volume of all days/times. This makes sense because some days and times are always busy, such as morning and evening rush hour. It wouldn't make sense to see a spike in the middle of the day and classify it as normal because it was within rushour volumes. Instead this should be an anomaly based on the fact that for the day and time it occurred, it was abnormal.

# Model & Results

I chose to build an isolation forest model to predict anomalies in my data set. I fed the model my volumes, timestamps, day of the week, and time of day. This way the model could compare similar day/times and make more accurate predictions of anomalies. I chose an anomaly fraction of 0.001, so that my model would produce the 11 largest anomalies that it found (out of a data set with ~10,300 entries). 

Overall I was disappointed in the results of my model, as they only detected one of the 5 known anomalies. My model managed to pick out the "North American Blizzard" on January 26th of 2015. Casting anomalies between the hour of 11pm and 12am (midnight). I believe this one was picked because it had such a small timeframe in which the downward spike occurred. While holidays like Christmas and Thanksgiving do see increased volumes, they are not as localized. As mentioned in my notebook, it would make sense that December 24th, 25th, and 26th would all have similar taxi volumes. For this reason my model would not have detected a sharp spike, but rather a gradual increase and therefor not classify the entire day as an anomaly. Perhaps if I had broken down my data on day or even week without taking time into consideration then it would have been able to pick out longer-lasting events such as the Thanksgiving and Christmas holidays.
