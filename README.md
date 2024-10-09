# What Influences Attraction Queue Times?

Theme parks want to attract business from guests all around the world. Large and well-known parks like Disneyland, Universal Studios, and Six Flags can see millions of visitors annually. According to [Forbes](https://www.forbes.com/sites/suzannerowankelleher/2024/08/16/how-disney-dominated-the-theme-park-industry-in-2023/), in 2023 the top 25 parks collectively drew in 244.6 million guests.

A major draw to these parks are the attractions guests can ride and experience. As a guest, a low queue time is desired so one can experience the ride as soon as possible, and experience more throughout the limited time spent at the park. 

Theme park owners would therefore benefit from understanding what drives attraction queue times up, and guests would be better able to plan their stays if they can anticipate certain days or attractions that will have long queue times.

#### The objective:

**Explore theme park data to determine which features are most significant in the prediction of attraction queue times. The data may need to be cleaned and transformed in order to be used in linear models for analysis. We will then evaluate the models performance and assess whether feature engineering improvements should be made. For the stakeholders, we may consider what recommendations are available for the upcoming quarters.**

The full analysis can be found [here](what_influences_attraction_queue_times.ipynb)


## 1. Explore the data and verify quality

The data was obtained from [kaggle](https://www.kaggle.com/datasets/ayushtankha/hackathon) and is uploaded here as a **compressed zip file**. I used the individual files to examine four CSVs: 


```
attendance = pd.read_csv('data/attendance.csv')
wait = pd.read_csv('data/waiting_times.csv')
weather = pd.read_csv('data/weather_data.csv')
link = pd.read_csv('data/link_attraction_park.csv')
```


![wait_csv](/assets/wait_csv.png)

![weather_csv](/assets/weather_csv.png)

There are several thousand entries that I cleaned and transformed to merge across the `date` and `park` columns. I continued by dropping unnecessary columns and filtering for a subset of the 2019 year's data. After this initial cleaning I visualized some of the field counts and examined the unique values in the data.

![weather_counts](/assets/weather_counts.png)

## 2. Make any necessary transformations

Most of the data was of a number data type. OrdinalEncoder was used to transform the `weather_main` data and use in the models later generated. Before we start building models, we review what numerical features are most correlated to the target wait times

![corr](/assets/corr.png)

## 3. Build various models

I built three different models (SFS, Ridge, and LASSO) with the features `['date', 'guest_carried', 'capacity', 'attendance', 'temp', 'humidity', 'wind_speed', 'weather_main']`.
All of the models utilized a 70/30 training/test split of the data. Shown below are the coefficient values from one of the LASSO models.

![lasso_coef](/assets/lasso_coef.png)

## 4. Assess model performance and analyze results

Reviewing the LASSO coefficients, the models show strong relationships between `attendance` and `capacity`. `weather_main` did not have as strong of an impact on wait times as I initially expected. 
Nevertheless we continue to plot weather against wait to see how the different weather values affect the queue times. 

![attendance_wait](/assets/attendance_wait_scatter.png)

![weather_boxplot](/assets/weather_boxplot.png)

The models had very similar MSE between both the train and test sets:

![mse_table](/assets/mse_table.png)

As we test other attractions and a longer date range we may see different results. For our purposes, focusing on one attraction at a particular park can help identify how to manage queue times for a specific attraction that may be quite popular. Although it
may be expected that attendance and capacity are the largest influences on overall queue times, park owners can take this into account to work on either maintaining or improving the ride capacity. In cases where this is not feasible or economical, parks may benefit from ensuring other attractions or shows are more widely available
in order for guests to have more options and potentially lower this particular attraction's queue.
