# Machine Learning Nano-Degree 
## Presented by Ali Ahmed Nada 


### Abstract

we have 7 basic emotions: happy, sad, surprise, fear, anger, disgust, and neutral. 
Our facial emotions are expressed through face muscules and role to recognize it and to be able to detect the emotions accordingly
the Main Objective is to be able to recognize the Emotions from image sent from anyone ..


### Problem Statement
To detect the facial emotions and detect whether it is sad , happy , anger , etc 


### Datasets and Inputs
https://www.kaggle.com/c/challenges-in-representation-learning-facial-expression-recognition-challenge/data


### Solution Statement
we will use Facial Emotion Recognition CNN Architecture
#### Input Layer
The input layer has pre-determined, fixed dimensions, so the image must be pre-processed before it can be fed into the layer. I used OpenCV, a computer vision library, for face detection in the image. The haar-cascade_frontalface_default.xml in OpenCV contains pre-trained filters and uses Adaboost to quickly find and crop the face.
The cropped face is then converted into grayscale using cv2.cvtColor and resized to 48-by-48 pixels with cv2.resize. This step greatly reduces the dimensions compared to the original RGB format with three color dimensions (3, 48, 48). The pipeline ensures every image can be fed into the input layer as a (1, 48, 48) numpy array.
#### Convolutional Layers

The numpy array gets passed into the Convolution2D layer where I specify the number of filters as one of the hyperparameters. The set of filters(aka. kernel) are unique with randomly generated weights. Each filter, (3, 3) receptive field, slides across the original image with shared weights to create a feature map.
Convolution generates feature maps that represent how pixel values are enhanced, for example, edge and pattern detection. In Figure 5, a feature map is created by applying filter 1 across the entire image. Other filters are applied one after another creating a set of feature maps.


Figure 5. Convolution and 1st max-pooling used in the network
Pooling is a dimension reduction technique usually applied after one or several convolutional layers. It is an important step when building CNNs as adding more convolutional layers can greatly affect computational time. I used a popular pooling method called MaxPooling2D that uses (2, 2) windows across the feature map only keeping the maximum pixel value. The pooled pixels form an image with dimentions reduced by 4.
#### Dense Layers

The dense layer (aka fully connected layers), is inspired by the way neurons transmit signals through the brain. It takes a large number of input features and transform features through layers connected with trainable weights.


Figure 6. Neural network during training: Forward propagation (left) to Backward propagation (right).
These weights are trained by forward propagation of training data then backward propagation of its errors. Back propagation starts from evaluating the difference between prediction and true value, and back calculates the weight adjustment needed to every layer before. We can control the training speed and the complexity of the architecture by tuning the hyper-parameters, such as learning rate and network density. As we feed in more data, the network is able to gradually make adjustments until errors are minimized.
Essentially, the more layers/nodes we add to the network the better it can pick up signals. As good as it may sound, the model also becomes increasingly prone to overfitting the training data. One method to prevent overfitting and generalize on unseen data is to apply dropout. Dropout randomly selects a portion (usually less than 50%) of nodes to set their weights to zero during training. This method can effectively control the model's sensitivity to noise during training while maintaining the necessary complexity of the architecture.
#### Output Layer

Instead of using sigmoid activation function, I used softmax at the output layer. This output presents itself as a probability for each emotion class.
Therefore, the model is able to show the detail probability composition of the emotions in the face. As later on, you will see that it is not efficient to classify human facial expression as only a single emotion. Our expressions are usually much complex and contain a mix of emotions that could be used to accurately describe a particular expression.
It is important to note that there is no specific formula to building a neural network that would guarantee to work well. Different problems would require different network architecture and a lot of trail and errors to produce desirable validation accuracy. This is the reason why neural nets are often perceived as "black box algorithms." But don't be discouraged. Time is not wasted when experimenting to find the best model and you will gain valuable experience.

#### Deep Learning I built a simple CNN with an input, three convolution layers, one dense layer, and an output layer to start with. As it turned out, the simple model preformed poorly. The low accuracy of 0.1500 showed that it was merely random guessing one of the six emotions. The simple net architecture failed to pick up the subtle details in facial expressions. This could only mean one thing...



This is where deep learning comes in. Given the pattern complexity of facial expressions, it is necessary to build with a deeper architecture in order to identify subtle signals. So I fiddled combinations of three components to increase model complexity:

number and configuraton of convolutional layers
number and configuration of dense layers
dropout percentage in dense layers
Models with various combinations were trained and evaluated using GPU computing g2.2xlarge on Amazon Web Services (AWS). This greatly reduced training time and increased efficiency in tuning the model (Pro tip: use automation script and tmux detach to train on AWS EC2 instance over night). In the end, my final net architecture was 9 layers deep in convolution with one max-pooling after every three convolution layers as seen in Figure 7.

### Benchmark Model
validation accuracy of 58%

### Evaluation Metrics
to detect new image emotion instantly 

### Project Design
To prepare the Training set , validation , testing , after that we should enter new images inorder to check accuracy.
