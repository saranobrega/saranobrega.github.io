## An Exploration of Model-State Data in Anomaly Detection


Is there an alternative approach to predicting anomalies in image data compared to the usual methods? Can the information about the state of the neurons be enough for a model to detect anomalies in image data?
<br><br>
Let's find out!

This was a project I performed during my the internship in the Netherlands, in the beginning of 2022. It was my first internship, right after graduation, and it allowed me to gain confidence on my skills in machine learning as well as to develop them. 

This is one of my favorite projects I have performed I genuinely had fun and I was extremely curious to discover the outcomes of this new method that was still just an idea before the implementation. 

Although by far it is not the most complete project I have performed in terms of end-to-end development (I only did the part of experimentation), I find the project conceptually very interesting yet not immediately straightforward to grasp the underlying idea.

In a nutshell, my task was to test a new method to detect anomalies in image data. This new method employs information about the state of the neurons of a previously trained ANN. 

### Motivation and Method

The goal is to test and compare two different methods of detecting anomalies in image data. The methods only differ in the input data that is given to the models! In this project, two different models were tested: a simple ANN (supervised task) and the Isolation Forest model (unsupervised task). 

### The data

As mentioned, the two methods differ in their input data:

- The usual/conventional method: using image (pixel) data as input data. More specifically, I used MNIST image data. I will refer to this data here as “normal data”.
- New method: using the model-state of the MNIST 10-digit classifier model as input data. What does this mean? To start, I first trained a ANN with MNIST pixel data both with normal and noisy data (here, representing the anomalies). Then, I access the state of the neurons of this ANN on the first and second layer of the model. Then, I use this data,  the state of the neurons of both layers, as the input data of an Isolation Forest model and as an input of another ANN. We named this data “model-state data”.

The main goal is to identify which kind of anomalies each method can detect and to determine if one method can detect certain types of anomalies that the other method cannot. Besides, regarding the new method, the goal was also to study which model-state layer data is the best to predict anomalies.

As mentioned, the first step was to train a simple ANN with MNIST pixel data (with normal and anomalous data):
<br><br>
<div><img src="/images/1.png" alt="Fig1: Training a simple ANN with MNIST pixel data (with normal and noisy data)."></div> 
<br><br>

In the image below, we can see the input data. On the left there is MNIST pixel data (normal data) and, on the right sid,e model-state data of the neurons after training an ANN with MNIST pixel data (model-state data).
<br><br>
<div><img src="/images/2.png" alt="Fig2: Input data. On the left: MNIST pixel data (normal data). On the right: State of the neurons after training an ANN with MNIST pixel data (model-state data)."></div> 
<br><br>

### The models

As mentioned, the goal was to compare the anomaly detection task using two different input data with two different models. The models are a simple ANN and the Isolation Forest.

<div><img src="/images/3.png" alt="Fig3: The models: ANN and Isolation Forest."></div> 
<br><br>

### Types of Anomalies

To test this new method and compare it to the conventional one, I created various types of anomalous data. The objective is to evaluate which types of anomalies both methods can or cannot detect. As illustrated in the image below, the noise rate increases from the first to the fourth type of noise.

<div><img src="/images/4.png" alt="Fig4: Different types of anomalous data."></div> 
<br><br>

### Results

**1) ANN**

<div><img src="/images/5.png" alt="Fig 5: Predicting anomalies with the ANN  model with model-state method. TP= True Positives. The y axis represents the anomaly score threshold, in which data points with a threshold>0.5 are considered anomalies."></div> 
<br><br>

Here, data is fed into an ANN. The left plot shows the results for the normal pixel data, while the right plot displays the results for the model-state data of the first layer of neurons. TP= True Positives. The y axis represents the anomaly score threshold, in which data points with a threshold>0.5 are considered anomalies.

The different anomaly types are depicted in the plots with different colors. We can check that the method with the normal pixel data detects less true positives regarding type 2 and 3 anomalies than the method with the model-state-data. However, compared to the normal method, the new method does not predict well the anomalies of type 1 and 4! Interestingly, anomalies of types 1 and 4 represent the least and most noisy data, respectively.

Also interesting to note is the statistical distribution of the samples in the plots. In the normal data method, the predictions are more scattered, and in the model-state one it is almost the opposite scenario.
<br><br>
<div><img src="/images/6.png" alt="Fig 6: Predicting normal data with the ANN  model with model-state method."></div> 
<br><br>
In anomaly detection tasks, achieving a balance between precision and recall is crucial. Recall, in the context of anomaly detection, represents the ability of the model to correctly identify all actual anomalies in the dataset. However, it's often important to prioritize precision over recall in these tasks. This is because false positives (instances where normal data is wrongly classified as anomalous) can have significant consequences, such as triggering unnecessary alarms or interventions. In situations where it is risky to have a high rate of false positives, maintaining a low recall helps minimize the number of false alarms.

It is clear from the plots that the model-state method performs much better on detecting true negatives (and false positives). In this case, the first layer outperforms the second one.
<br><br>
<div><img src="/images/7.png" alt="Fig 7: Results concerning the ANN model."></div> 
<br><br>

The model-state data of the first layer method outperforms the second layer and pixel data methods in detecting types 2 and 3 anomalies and in identifying false positives. However, normal data still exhibits better performance in detecting types 1 and 4 anomalies.

<br><br>
**2) Isolation Forest**

I was super curious about the isolation forest’s results. Mostly for being an unsupervised model but also because I had never worked with this model before. Could an unsupervised model detect anomalies using as input data the state of the neurons? Let's see:

<br><br>
**Pixel data - Predicting non-anonalous data**

<div><img src="/images/8.png" alt="Fig 8: Predicting normal samples with isolation forest with normal data."></div> 
<br><br>

The percentage of false positives is 50%. As good as flipping a coin!

<br><br>
**Pixel data - Predicting anomalies**

<div><img src="/images/9.png" alt="Fig 9: Predicting anomalies with isolation forest with normal data.."></div> 
<br><br>

The model was only able to detect a bit more than half of the anomalies of Type 1. 

<br><br>
**Model-state data - Predicting non-anonalous data**

<div><img src="/images/10.png" alt="Fig 10: Predicting anomalies with isolation forest with normal data."></div> 
<br><br>

The first layer predicted 100% of false positives whereas the second layer only 1% of false positives.

<br><br>
**Model-state data - Predicting anomalies**

The results from the first and second layer are shown in separated images:

<div><img src="/images/11.png" alt="Fig 11: Predicting anomalous data with isolation forest with model-state data of the first layer."></div> 
<br><br>

<div><img src="/images/12.png" alt="Fig 12: Predicting anomalous data with isolation forest with model-state data of the second layer."></div> 
<br><br>

• The first layer predicts well all the anomalous samples. But it also yielded 100% of false positives. At the end, it is not a good approach.

• The second layer yields interesting results. With 100% of true positives for the second, third and fourth type of noisy types, it sounds promising on detecting certain kinds of anomalous data. The only downside is predicting 81% of false negatives for the 1st type of noisy samples. But again, these are the hardest anomalies to detect, given that the noise ratio is the lowest.
<br><br>

<div><img src="/images/13.png" alt="Fig 13: Results regarding the isolation forest model."></div> 
<br><br>

### **Conclusion**

**ANN**

In conclusion, the results obtained from comparing the anomaly detection performance of an Artificial Neural Network (ANN) and an Isolation Forest model using two different input data types provide valuable insights. The ANN, when trained with model-state data representing the state of neurons, showed improved performance in detecting anomalies compared to the traditional method using pixel data. Besides, the model-state data from the second layer of neurons showed promising results in identifying certain types of anomalies with a low false positive rate.

A potential possibility for future exploration could be combining both methods within the ANN framework — utilizing normal data to capture certain anomalies and model-state data to detect others. Further experiments with diverse datasets and anomaly types would be needed to validate this hybrid approach. 

This project was a preliminary experiment, but I believe it underscores the importance of trying new methods in detecting anomalies. 

**Isolation Forest** 

The Isolation Forest, being an unsupervised model, faced some challenges in effectively detecting anomalies when fed with model-state data, specially using model-state data of the first layer. This layer showed high accuracy in identifying anomalous samples and it also produced a high rate of false positives, making it less suitable for this task. However, the second layer of neurons in the Isolation forest model showcased a better performance, particularly in detecting specific types of anomalous data with minimal false positives, despite facing challenges in identifying the hardest outliers.

The findings highlight the importance of considering different input data types and layers of abstraction in anomaly detection tasks. Both models have their strengths and weaknesses, and probably a combination of both methods could be an optimized anomaly detection approach compared to the original method.

This project not only contributed to the understanding of anomaly detection methodologies but also underscored the importance of exploring innovative approaches, such as utilizing the state of neurons, to enhance the performance of anomaly detection models!




