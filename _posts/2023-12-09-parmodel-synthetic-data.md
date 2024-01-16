## Wind Turbine Synthetic Data Generation with ParModel

### Unlocking the Power of Synthetic Data
In today's data-driven era, where insights fuel innovation, the quest for valuable information often encounters significant roadblocks. Privacy concerns and data limitations are growing concerns, many times blocking the free flow of data necessary for research, development, analysis, and ultimately, economical growth. 

### Synthetic Data - the Answer to Data Challenges?

Synthetic data is here trying to mitigate these problems. It’s a solution designed to revolutionize how we deal with the issue of data availability and data privacy. Synthetic data is computed to mirror the statistical properties of their authentic counterparts. It serves as a versatile tool for researchers, data scientists, and organizations across diverse domains.

### What is Synthetic Data useful for?

- **Navigating Privacy Concerns:**
    - Privacy regulations and ethical concerns limit access to real datasets.
    - It provides privacy-preserving alternatives by replicating statistical intricacies without revealing individual identities.
- **Overcoming Data Limitations:**
    - Scarcity of data is a common bottleneck in niche or emerging fields.
    - Synthetic data acts as a solution to data scarcity, allowing researchers to augment training models, validate algorithms, and perform testing without constraints.
 

### Why synthetic data for wind turbine data? 

Wind-turbine data, particularly Wind Speed, is crucial to optimize renewable energy systems. The efficiency of wind turbines depends on understanding and harnessing wind patterns effectively. Accurate Wind Speed data aids in predicting power output, enabling precise grid management. This forecasting capability ensures a reliable energy infrastructure. Analyzing wind-turbine data identifies optimal locations for new wind farms, contributing to strategic resource allocation and cost-effective project execution.


Navigating the world of wind-turbine data is like trying to catch the wind itself – challenging. Real data is a precious commodity for renewable energy researchers, and the scarcity of diverse, real-world datasets complicates the development of accurate models.

Privacy concerns further tighten the knot. Companies guard their wind farm data as a trade secret, limiting the accessibility for research purposes.

###  ParModel Overview

ParModel is a synthetic data model that mimics the intricacies of the real stuff, providing a workaround for data scarcity and privacy constraints. But how does it work?

The PAR class serves as an implementation of a Probabilistic AutoRegressive model designed for the learning of multi-type, multivariate time-series data. Ok, many fancy words here, I know. 

Simplyfing: It is probabilistic because it uses probabilities to predict what comes next in a sequence of events, making it versatile for capturing various types of patterns. The term "AutoRegressive" refers to the modeling approach used to predict future values in a time series based on its own past values. The idea is that there is a certain degree of correlation or dependence between consecutive observations in a time series. So PARmodel considers the probabilistic nature of predicting future events in a time series while also incorporating information from previous time steps.

Multi-type and multivariate time-series data means that the model is also designed for datasets that involve different types of information recorded at various points in time. For example, in wind-turbine data we might have multiple variables like active power and turbine number. The PAR model can handle this mix of information, providing a comprehensive understanding of how these variables interact.

Subsequently, it enables the generation of new synthetic data that faithfully replicates the format and properties of the initially learned dataset. 🔄


###  Data Preparation

As in any synthetic generation task and most machine learning assignments, the foundation for success lies in the quality of the training dataset.

A diverse and representative dataset acts as the guiding force, helping the model discern and capture intricate patterns and nuances embedded in the original data. This is crucial to ensure that the synthetic data reflects the complexity of real-world scenarios.

Inaccuracies or biases present in the training data can impact the generative process, leaving imprints on the synthetic data. Thus, preprocessing of the training dataset should be the first concern in this type of task.  

👉 Making sure the data is representative, mitigating bias, ensuring proper alignment of timestamps in the data, outlier removal and dealing with NaN were the main pre-processing tasks employed here before generating synthetic data.

Note: This model requires all rows to be non-null (NaN). So make sure you do the necessary data pre-processing so you do not have any NaN values. 

Here is an example of a scatter plot of some wind speed data over time:

<div><img src="/images/WS.png" alt="Plot of WindSpeed over time"></div> 

### Generating Synthetic Data

The implementation of ParModel is very simple and straight-forward. 

Here is a snapshot:

<div><img src="/images/impl.png" alt=""></div> 

The entity columns are like labels that tell us which rows belong to the same group or entity. In this case, each row with the same “WT” would represent data for a specific wind turbine.

The **`data_types`** parameters is employed to specify the types of data for the different columns in the data. It helps the model to understand the nature of the data in each column.

In my case, data types are WindSpeed and ActivePower (I also generated synthetic data using WindSpeed only).

The **`sequence_index`** parameter in the code is used to specify the column  that represents the sequence index or timestamp.

I generated WindSpeed synthetic data both considering only WindSpeed column and WindSpeed+ActivePower column. My goal was to check if adding the ActivePower column would improve the results!

### Evaluation Metrics

For this project I used Synthetic Data Metrics (SDMetrics),  an [open source](https://github.com/sdv-dev/SDMetrics) Python library for evaluating tabular synthetic data. With this library anyone is was able to compare synthetic data against real data using a variety metrics, generate visual reports and share them. 

There are two main metrics in these SDMetrics:

- **Column Pair Trends:** This metric focuses on evaluating the matching trends between pairs of real and synthetic data columns. For each pair, the correlation is calculated, and the final score represents the average of these correlation measures across all column pairs. The metric utilizes two similarity metrics: CorrelationSimilarity for continuous columns and ContingencySimilarity for discrete columns.

- **Column Shapes:** This metric assesses the shape similarity between real and synthetic data columns. It computes a metric score column-wise, and the final score is the average over all columns. The KSComplement metric is used for numerical and datetime columns, measuring the Kolmogorov-Smirnov distance. For categorical and boolean columns, the TVComplement metric is employed, assessing the total variation distance. The metric provides insights into how well the synthetic data replicates the shapes of the original data columns across various types.

- **LSTM score:**  LSTMDetection calculates how difficult it is to tell apart the real, sequential data from the synthetic data and it is done using a neural network. The scores obtained from these metrics are normalized within the range [0, 1]. The better the model can distinguish between real and synthetic sequences, or the higher its accuracy when fitted on synthetic data compared to real data, the higher the resulting metric score. Therefore, a **higher score** implies a higher level of similarity and quality in the synthetic time series data.


### Comparative Analysis

#### Metrics
<br><br>
LSTM metrics:
<br><br>

| Turbine | WS   | AP+WS |
| ------- | ---- | ----- |
| 1       | 0.32 |0.32 |
| 2      | 0.35 | **0.36**  |
| 3      | **0.34** | 0.30  |
| 4      | 0.34 | 0.34  |
| 5      | 0.34 | **0.36**  |
| 6      | 0.35 | **0.39**  |
| 7      | **0.37** | 0.35  |
| 8      | **0.35** | 0.35  |
| 9       | 0.35 | **0.38** |

<br><br>
SDMetrics (Overall score, considering Column Shapes and Column Pair Trends metrics):
<br><br>

| Turbine | WS   | AP+WS |
| ------- | ---- | ----- |
| 1       | **81%** | 78%  |
| 2      | 80% | **83%**  |
| 3      | **85%** | 82%  |
| 4      | **85%** | 78%  |
| 5      | 80% | **85%**  |
| 6      | 78% | **81%**  |
| 7      | 80% | **85%**  |
| 8      | **84%** | 84%  |
| 9       | 82%| **83%**  |



#### Plots

I generated synthetic data by providing the model datasets comprising 500, 1000, 2000, and 10000 samples. 
<br><br>
👉Some results concerning 500 samples:
<br><br>
<div><img src="/images/500_1.png" alt=""></div> 
<br><br>
👉Some results concerning 1000 samples:
<br><br>
<div><img src="/images/1000_1.png" alt=""></div> 
<br><br>
👉2000 samples:
<br><br>
<div><img src="/images/2000.png" alt=""></div> 
<br><br>
👉 And finally, 10k samples:
<br><br>
Here, I showcase both synthetic datasets, one employing only WindSpeed as a data type and the other including ActivePower: 
<br><br>
<div><img src="/images/ws_only.png" alt=""></div> 
<br><br>
<div><img src="/images/ws_ap.png" alt=""></div> 
<br><br>
And a plot with both synthetic dataframes:
<br><br>
<div><img src="/images/both.png" alt=""></div> 
