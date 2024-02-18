## An Exploration of Model-State Data in Anomaly Detection


This was a project my supervisor during my the internship in the Netherlands proposed to me, in the beginning of 2022. It was my first internship, right after graduation, and it allowed me to gain confidence on my skills as well as to develop them. 

This is one of my favorite projects I have performed, I genuinely had fun and I was extremely curious to know the results. 
Although by far it is not the most complete project I have performed in terms of end-to-end development (I only did the part of experimentation), I think that conceptually, the project is very interesting and somehow not so straightforward to understand.
Basically, my task was to test a new method to detect anomalies in image data.

### Motivation and Method

The goal is to test and compare 2 different methods of detecting anomalies. The methods only differ in the input data that is given to the models! In this project, two different models were tested: a simple ANN (supervised) and the Isolation Forest model (unsupervised). 

### The data

As mentioned, the two methods differ in the input data:

- The usual method: using image (pixel) data (MNIST images) as input data. I will refer to this data here as “normal data”.
- New method: using the model state of the MNIST 10-digit classifier model as data. What does this mean ? I first trained a ANN with MNIST pixel data both with normal and noisy data (the anomalies). Then, I access the state of the neurons of this ANN on the first and second layer of the model. I use this data,  the state of the neurons of both layers, as the input of Isolation Forest and another ANN. We named this data “model-state data”.

The goal here is to see which kind of anomalies each method can detect, if one method can detect certain kind of anomalies that the other is not able to detect. Besides, regarding the second method, the goal is also to study which layer data is the best to predict anomalies.

<div><img src="/images/1.png" alt="Fig1: First, I trained a simple ANN with MNIST pixel data (with normal and noisy data)."></div> 
<br><br>

<div><img src="/images/2.png" alt="Fig2: Input data. On the left: MNIST pixel data (normal data). On the right: State of the neurons after training an ANN with MNIST pixel data (model-state data)."></div> 
<br><br>

## The models

As mentioned, the goal was to compare the anomaly detection task using two different input data with two different models. The models are a simple ANN and the Isolation Forest.


<div><img src="/images/3.png" alt="Fig3: The models: ANN and Isolation Forest."></div> 
<br><br>

## Types of Anomalies

To test this new method, my supervisor suggested to generate different kinds of anomalous data. The goal is to assess which types of anomalies both methods would/would not detect. We can see in the image below that from the first to the fourth kind of noise, the noise rate increases.


<div><img src="/images/4.png" alt="Fig4: Different types of anomalous data."></div> 
<br><br>

### Results

**1) ANN**

<div><img src="/images/5.png" alt="Fig 5: Predicting anomalies with the ANN  model with model-state method. TP= True Positives. The y axis represents the anomaly score threshold, in which data points with a threshold>0.5 are considered anomalies."></div> 
<br><br>
The different anomaly types are depicted in the plots with different colors. We can check that the method with the normal input data detects less true positives regarding type 2 and 3 than the method with the model-state-data (in this case, using data from the first layer of neurons). However, compared to the normal method it does not predict well the anomalies of type 1 and 4!

Also interesting to note is the statistical distribution of the samples in the plots. In the normal data method, the predictions are more scattered, and in the model-state one it is almost the opposite scenario.
<br><br>
<div><img src="/images/6.png" alt="Fig 6: Predicting normal data with the ANN  model with model-state method."></div> 
<br><br>
In anomaly detection tasks, achieving a balance between precision and recall is crucial. Recall, in the context of anomaly detection, represents the ability of the model to correctly identify all actual anomalies in the dataset. However, it's often important to prioritize precision over recall in these tasks. This is because false positives, instances where normal data is incorrectly classified as anomalous, can have significant consequences, such as triggering unnecessary alarms or interventions. In situations where the cost or impact of false positives is high, maintaining a low recall helps minimize the number of false alarms, ensuring that the anomalies identified by the model are more likely to be genuine.

It is clear  from the plots that the model-state method performs much better on detecting true negatives.
<br><br>
<div><img src="/images/7.png" alt="Fig 7: Results concerning the ANN model."></div> 
<br><br>

**2) Isolation Forest**

I was super curious about the isolation forest’s results. Mostly for being an unsupervised model but also beacause I had never worked with the model before.  Could an unsupervised model detect anomalies using as input data the state of the neurons? Let's see:

**Pixel data**

<div><img src="/images/8.png" alt="Fig 8: Predicting normal samples with isolation forest with normal data."></div> 
<br><br>

The percentage of false positives is 50%. As good as flipping a coin!

**Predicting anomalies**

<div><img src="/images/9.png" alt="Fig 9: Predicting anomalies with isolation forest with normal data.."></div> 
<br><br>

The model was only able to detect a bit more than half of the anomalies of Type 1. 

### Model-state data


**Predicting normal samples**

<div><img src="/images/10.png" alt="Fig 10: Predicting anomalies with isolation forest with normal data."></div> 
<br><br>

The first layer predicted 100% of false positives whereas the second layer only 1% of false positives.

**Predicting anomalous data**

The results from the first layer and second layer are shown in separated images:

<div><img src="/images/11.png" alt="Fig 11: Predicting anomalous data with isolation forest with model-state data of the first layer."></div> 
<br><br>

<div><img src="/images/12.png" alt="Fig 12: Predicting anomalous data with isolation forest with model-state data of the second layer."></div> 
<br><br>

• The first layer predicts well all the noisy samples. But it also yielded 100% of false positives. At the end, it is not a good approach.

• The second layer yields interesting results. With 100% of true positives for the second, third and fourth type of noisy types, it sounds promising on detecting certain kinds of anomalous data. The only downside is predicting 81% of false negatives for the 1st type of noisy samples. But again, these are the hardest anomalies to detect, given that the noise ratio is the lowest.
<br><br>

<div><img src="/images/13.png" alt="Fig 13: Results regarding the isolation forest model."></div> 
<br><br>

### **Conclusion**

**ANN**

In conclusion, the results obtained from comparing the anomaly detection performance of an Artificial Neural Network (ANN) and an Isolation Forest model using two different input data types provide valuable insights. The ANN, when trained with model-state data representing the state of neurons, demonstrated improved performance in detecting anomalies compared to the traditional method using pixel data. Notably, the model-state data from the second layer of neurons showed promising results in identifying certain types of anomalies with a low false positive rate.

A potential possibility for future exploration could be combining both methods within the ANN framework—utilizing normal data to capture certain anomalies and model-state data to detect others. Further experiments with diverse datasets and anomaly types would be needed to validate the effectiveness of this hybrid approach. 

This project was a preliminary experiment, but I believe it underscores the importance of trying new methods in detecting anomalies. 

**Isolation Forest** 

The Isolation Forest, being an unsupervised model, faced some challenges in effectively detecting anomalies when fed with model-state data, especially using mode-state data of the first layer. This layer  showed high accuracy in identifying noisy samples and it also produced a high rate of false positives, making it less suitable for this task. However, the second layer of neurons in the Isolation forest model showcased a better performance, particularly in detecting specific types of anomalous data with minimal false positives, despite facing challenges in identifying the hardest outliers.

The findings highlight the  importance of considering different input data types and layers of abstraction in anomaly detection tasks. Both models have their strengths and weaknesses, and probably a combination of both methods could be an optimized anomaly detection approach compared to the original method.

This project not only contributed to the understanding of anomaly detection methodologies but also underscored the importance of exploring innovative approaches, such as utilizing the state of neurons, to enhance the performance of anomaly detection models!




