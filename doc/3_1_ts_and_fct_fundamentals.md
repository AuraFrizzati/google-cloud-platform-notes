# Time Series and Forecasting Fundamentals

## 1. Sequence models

A sequence includes any data where the **order** of the data items matters (e.g. a sentence is an ordered sequence of words, not just any sequence of words). In other words, sequences are data points that can be **meaningfully ordered**, so that **earlier observations provide information about later observations**. 

- Simple definition of **Forecasting** --> You should be able to take a **slice of past observations** and use them to get a **better-than-chance prediction of later observations**.

- **Sequences** can be either the **input** or the **output** of a machine learning **model**. According to this perspective, we can fit sequence models into three types:
    - **One-to-sequence**: **one input** is passed in and the model generates a **sequence as the output**. A typical example is image captioning where one image is given as input to generate its textual description (e.g. a few sentences)
    - **Sequence-to-one**: a **sequence is the input** to the model and **one output is generated**. Sentiment analysis is a typical sequence-to-one problem, based on the comments or few sentences, the machine can generate one rating.
    - **Sequence-to-sequence**: **sequence is both the model's input and output**. Google Translate is an example of a model that delas with sequence-to-sequence problems

### Applications of sequence models

- Solve **forecasting** problems (e.g. sales, demand, weather and traffic forecasting)
- Solve **natural language processing** (**NLP**) problems (e.g. machine translation, speech recognition and sentiment analysis)
- Solve computer vision problems (e.g. image or video generation, captioning)

## 2. Time series patterns

A **time series** is a **series of data points indexed/listed in time order**. This is in contrast with other sequence models, such as NLP (where the order is based on the position of words in sentences) or computer vision (where the order relies on frames), etc...

![alt text](img/time_series_patterns.png)
*From [https://www.statology.org/understanding-time-series-in-python/](https://www.statology.org/understanding-time-series-in-python/)*

Although time series can take different shapes and sizes, they show a common few patterns, including 
- **Trend pattern**: a **gradual increase or decrease pattern over time** (e.g. increasing housing price in the past decade; decreasing birth rate since 1980)
- **Seasonal pattern**: a **recurring pattern over successive period**s. Data is affected by **calendar factors** (e.g. time of the day, day of the week or month of the year) and the **pattern** is **repeated at a known and fixed frequency** (e.g. peak hours at a coffee shop or hiloday seasons of a retail store). A time series may show multiple seasonality.
    - Vertex AI does a godd job in detecting multiple seasonalities in time series 
- **Cyclical pattern**: it exhibits **fluctuations in a recurring pattern**, which however **do not occur at a fixed frequency or the same amplitude**. The time series showing a cyclical pattern is often affected by **economic cycles** (e.g. unemployment rate over the past few decades). 
    - A cyclical pattern is **easily confused with a seasonal pattern**: 
        - Their major difference relies on the **frequency (or regularity) of the pattern**. If the **fluctuations are at a fixed frequency** and are **related to calendar factors**, the pattern is **seasonal**. 
        - Another difference is that the **amplitude of the seasonal pattern** is **usually the same**, whereas the **amplitude of the cyclical pattern** can be **different**
        - A third difference (which might not apply to all cases) is the **length of one cycle**. A **seasonal pattern normally happens within a season** (e.g. year one or two) whereas a **cyclical pattern normally occurs during a long period of time** (e.g. one or more decades)
- Noise pattern

## 3. Time series analysis

## 4. Forecasting notations
