# Data Science Project: Wolt Applied Science Intern pre-assignment

## Requirements
This project uses Python version 3.13.0 with Jupyter Notebooks, coded in VSCode on a Windows 11 machine.
- to replicate this environment, install the required packages listed in `requirements.txt`.

```bash
# Create a python virtual environment
python -m venv venv
```

```bash
# Activate environment (Windows)
venv\Scripts\activate
```

```bash
# On macOS/Linux:
source venv/bin/activate
```

```bash
# Install requirements in the python virtual environment
pip install -r requirements.txt
```

- make sure Omnes font is included to replicate plots
    - **Font Location**: `/fonts/omnes/Omnes SemiBold.ttf`.

---
## Table of Contents
1. [Introduction](#introduction)
2. [Dataset](#dataset)
3. [Data Cleaning](#data-cleaning)
4. [Data Exploration](#data-exploration)
5. [Modeling](#modeling)
6. [Evaluation](#evaluation)
7. [Conclusion](#conclusion)
8. [Requirements](#requirements)

## Introduction
This project was represents an assignment for the Wolt Applied Science Internship 2025.

The goal is to analyze and model the food delivery orders dataset, to gain insightful knowledge about the data and to make a few predictions. The process involves data cleaning, exploration, modeling, and evaluation.


## Dataset
The dataset used in this project is the ["orders_autumn_2020"](https://github.com/woltapp/applied-science-internship-2025) which contains information about customer orders. The dataset includes features such as time of order, delivery times, amount of items, user location, venue location, weather information.


## Data Cleaning
Dataset cleaning is performed in [`orders_cleaning.ipynb`](./orders_cleaning.ipynb)

Data cleaning involves handling missing values, and correcting data types. The steps include:
- Checking for missing values: missing weather information on 10 September 2020.
- Interpolated missing records.
- Converting data types to appropriate formats: TIMESTAMP to datetime.


## Data Exploration
Data exploration is performed in [`orders_exploration.ipynb`](./orders_exploration.ipynb)

Data exploration involves visualizing and summarizing the dataset to understand its structure and relationships. Key steps include:
- Descriptive statistics (mean, median, mode, etc.)

![Image](https://github.com/user-attachments/assets/509cce44-972a-4886-b831-e03bf081a0bf)
- Correlation relationship between all features (no unexpected strong correlations found)
- Exploration on Wolt KPI's : customer satisfaction, average order value (or quantity here), delivery times (impact by weather, day of the week, rush hour)

![Image](https://github.com/user-attachments/assets/10825026-f615-4d64-a180-972e169a9b46)
- Customer satisfaction:
    - no ratings in the data, but can compute a satisfaction rating based on the time "saved" on each delivery
    - found 14534 deliveries in an acceptable time range (5 minutes late - 10 minutes early)
    - CSAT score of 77.7% (in-line with industry benchmarks)

![Image](https://github.com/user-attachments/assets/388677de-e874-443d-b267-a22f6c3cb795)
![Image](https://github.com/user-attachments/assets/26a2ea93-12be-4ffd-a29f-e34feee308c8)
![Image](https://github.com/user-attachments/assets/fb6cf538-b06f-4797-92da-58714b71066d)
![Image](https://github.com/user-attachments/assets/139ffcbe-2a8c-4389-a0da-df0ecfd06813)
![Image](https://github.com/user-attachments/assets/29b2ec77-101e-4c42-9739-0e483bf88e6e)
- Average order quantity (AOQ), with a focus on daily AOQ:
    - no strong correlations with weather
    - good correlation with day of the week (weekends see bigger orders)
    - autocorrelation function for daily AOQ shows a 7-day seasonality
    - avg daily AOQ: 2.7 items/order

![Image](https://github.com/user-attachments/assets/9ecfc2d5-0c1a-45e6-8559-9de0693baf0e)
![Image](https://github.com/user-attachments/assets/7172c736-2557-4d69-b6a0-995cc3347a9e)
![Image](https://github.com/user-attachments/assets/573a93c9-d90a-41ab-89e8-8455eb2813c8)
![Image](https://github.com/user-attachments/assets/e5044caf-98be-4e81-a890-39491089d5d7)
![Image](https://github.com/user-attachments/assets/d6d11e2d-5290-4b5e-a0fa-d0cda2cab5e1)
- Delivery time impact:
    - some time lost when precipitation is high
    - no time lost when comparing weekday and weekend
    - no time lost during rush hours
    - the ESTIMATED_DELIVERY_MINUTES seems to properly increase when there are inconveniences (weekday, rush hour traffic, bad weather)
    - the users do not get low initial time estimates, impacting the ON_TIME_DELIVERY performance


## Modeling
Data modeling tasks are performed in [`orders_modeling.ipynb`](./orders_modeling.ipynb)

Modeling involves selecting and training machine learning models to make predictions based on the dataset.

![Image](https://github.com/user-attachments/assets/eb65f3db-b72d-40b7-bc08-a12d23bf0606)

![Image](https://github.com/user-attachments/assets/1873d50c-99d0-4c79-8c53-63937d20fcd7)
![Image](https://github.com/user-attachments/assets/ed6416c7-ad43-40f8-adb5-d60332ed3d64)
### Clustering
A clustering based on K-means is implemented to separate the users into distinct groups based on features
- multiple clustering features and correlations were tested, the only significant impact was the user geographical location
- the dataset was split by user location into 5 clusters, or zones
- the venues were then added based on these zones
- the orders were classified then into two major categories: same-zone orders, and cross-boundary orders
- thus hinting efficient courier distribution


### Classification
Classification based on Random Forest
- trying to predict wether an order will remain in the same-zone, or if the courier needs to cross the zone boundaries to get the order
- splitting the data into training and testing sets.
- training the models on the training data.
- tuning hyperparameters for optimal performance.
Two predictions were selected:


![Image](https://github.com/user-attachments/assets/3ecb5fee-b2d0-4bb4-b289-0873d7e4dab4)
![Image](https://github.com/user-attachments/assets/f6a14e44-bfa3-4b36-b69e-cbb56d8b6fc0)
#### Same-zone orders
Predicting if the order stays in the same zone

![Image](https://github.com/user-attachments/assets/5cd7aecf-4e61-43fd-9f70-4fd2739c2fc7)
![Image](https://github.com/user-attachments/assets/b3738b15-40ed-4693-8ff1-c558dc52d121)
#### Venue cluster
Predicting if we can determine a venue's cluster, based on the user's information (user clusters go to preferred clusters, or they order all over the place)



## Evaluation
Evaluation involves assessing the performance of the trained models using metrics such as accuracy, precision, recall, and F1-score. Steps include:
- making predictions on the test data.

![Image](https://github.com/user-attachments/assets/212a734b-3487-4c85-b01f-906a33cba1f0)
- calculating evaluation metrics.
    - same-zone orders predicted with 83% accuracy
    - venue clusters predicted with 64% accuracy

![Image](https://github.com/user-attachments/assets/2abc4231-a745-47d1-9869-087a01660dfe)
- comparing model performance
    - KNN was used to better predict the venue clusters


---
## Conclusions
- EDA showed interesting metrics on customer satisfaction and delivery times
- no extreme correlations with weather, day of the week, hour
- used clustering methods to categorize the dataset into optimal "user zones"
- try to predict if orders remain inside the zones, or cross the zone boundary
- try to predict venue clusters, based on order information
- evaluate performance of modeling

Further developments:
- hyper-parameter search to optimize prediction
- test other classifiers
- time series analysis on AOQ, or daily/weekly orders
    - use SARIMA (because data has seasonality)
    - forecast number of orders on a particular day (focus on weekday vs weekend)
- try other clustering methods (e.g. DBSCAN)
