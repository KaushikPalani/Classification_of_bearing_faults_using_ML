# Fault Diagnosis and Classification for rotating machinery components using ML techniques on CWRU vibration dataset

The goal of this project is to detect and classify the different bearing faults based on the raw vibration data.  
The data was obtained from Case Western Research University - Bearing Data Center https://engineering.case.edu/bearingdatacenter

## Acomplishments through this project 
 - Achieved the classification model with 96% accuracy on test data
 - Choosing the optimal time domain and frequency domain features to increase accuracy in SVM 
 - Tuning (Hyper parameter) of sampling stride length (Data augmentation) on the time series data for improved accuracy 
 - Faster training when time-series data is fed as 2D image in 2D CNN 
 - k-fold cross validation to prevent over-fitting 
 - Understanding the principles of PCA and the working of SOFM

## Daset Information
 - Drive End Bearing 
 - Sampling frequency - 48,000 samples/sec 
 - 2HP Motor with 1750rpm 
 - 1670 approx. samples per rotation 
 
**File Name - Fault type - Fault Dia - Class**  

99.mat - Normal baseline data                             - Class 0   
111.mat -  Inner race - 0.007in                             - Class 1  
124.mat -  Ball - 0.007in                                   - Class 4  
137.mat -  Outerrace - centered @6.00 - 0.007in fltdia      - Class 7  
176.mat -  Inner race - 0.014in                             - Class 2    
191.mat -  Ball - 0.014in                                   - Class 5  
203.mat -  Outerrace - centered @6.00 - 0.014in fltdia      - Class 8  
215.mat -  Inner race - 0.021in                             - Class 3  
228.mat -  Ball - 0.021in                                   - Class 6  
240.mat -  Outerrace - centered @6.00 - 0.021in fltdia      - Class 9  

## Data Pre-Processing 
### Sampling and Labelling 
The time series data is split and sampled in lengths of 1681 data per block (nearest perfect square of 1670) which approximates data for one complete rotation. In order to increase the number of samples and capture the features better, the sampling is done in strides of 200, therby enabling overlap of the data between adjacent samples. Also, the classes were labelled and stored as a separate list.

### Feature Extraction (SVM)
For SVM, the following features were extracted from the raw data in MATLAB and used as an input. 
 - Time Domain 
   - max, min, peak-to-peak, mean, variance, standard deviation, root-mean-square, skewness, crest factor, kurtosis
 - Frequency Domain 
   - max frequency value 
   - amplitude of max frequency 
### Dimensionality Reduction - Principal Component Analysis (SVM)
The features extraced were normalized before performing pca. Only first five principal components were chosen after evaluating the latent values and weightage, thereby reducing the dimension of the input data.  


## Classification Models 
The following Machine learning classification models were experimented. 
 - Support Vector Machine 
   - gaussian Radial basis function kernel
   - Polynomial 
   - Sigmoid
 - 1D Convolutional Neural Network  
 - 2D Convolutional Neural Network (Input data as an image of size 41x41)
 - Long Short-Term Memory - Recurrent Neural Network 

k-fold cross validation was used to prevent over-fitting and ensure generalization.

## Results   
### Support Vector Machine - RBF - Decision Boundary represented in 2D
<img src="/Images/SVM.png" width=50% height=50%>
The decision boundary is drawn with 2 principal components in 2D compared to the 5 principal components available. Therefore, in many regions, data seem to overlap which is not the actual case. 

### 1D Convolutional Neural Network 
![](/Images/CNN_1D.png)
### 2D Convolutional Neural Network 
![](/Images/CNN_2D.png)
### Long Short-Term Memory
![](/Images/LSTM.png)
### Comparison 
<img src="/Images/ComparisonPlot.png" width=80% height=70%>

Well, 1D CNN seems to be the winner in here. But, when varying the stride length to higher values more than 200 while sampling, 2D CNN proved to be fast and efficient in capturing the features. Also, though LSTM has decent accuracy, it consumes a lot of time to train, which could be frowned upon.

One of the observation was that the accuracy increases on decreasing the stride interval during sampling. This gives better opportunity to capture the feature and acts as data augmentation as well. After many trials the stride interval length of 200 was chosen to perform comparison between different ML techniques under reasonable computing time and resources.  

Therefore, with the **minimal resources**, 1D CNN and 2D CNN techniques prove to be efficient and reliable for this problem. 

Considering this as a clustering problem, and to find the hidden patterns present in the data, SOFM (Self-organizing feature Map) is also considered and will be implemented in further studies.

# References 
 - https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9078761
 - https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9345676
