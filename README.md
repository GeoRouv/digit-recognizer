# Digit-Recognizer
An image classifier CNN to recognise handwritten digits.

## Data Preparation
Data was loaded from Keras library.

### Reshape
Keras requires an extra dimension in the end which corresponds to channels. Images from
MNIST dataset are gray-scaled, so only one channel is used.

### Labeling
The labels (numbers 0 to 9) were encoded to one hot vectors (e.g : 1 : [1,0,0,0,0,0,0,0,0,0]).

### Normalization
Neural network models often cannot be trained on raw pixel values, such as pixel values
in the range of 0 to 255. The reason is that the network uses a weighted sum of inputs,
and for the network to both be stable and train eectively, weights should be kept small.
Instead, the pixel values must be scaled prior to training.
Normalization is often the default approach as we can assume pixel values are always in
the range 0-255, making the procedure very simple and ecient to implement. Conse-
quently, pixel values are scaled to the range 0-1.

## CNN

### Configuration
• 1 2D-Convolution Layer: (3,3) is the dimensionality space of output, ReLU is the
activation function. HE initializer performs better than normal that's why is selected
• 1 Max-Pooling Layer: Downsampling of data
• 1 Flatten Layer: Data is 
attened so it can be passed to dense layer (keeping 1
dimension)
• 2 Dense Layers: Dense layers are used when association can exist among any feature
to any other feature in data point. Since between two layers of size n1 and n2, there
can n1n2 connections and these are referred to as Dense. The first one contains a
ReLU activation function while the second is the softmax layer
3.2.2 Compilation
• Optimizer: Gradient descent is a good generalized optimizer (Adam can be used as
well)
• Loss function: Since a softmax output layer was used, the Cross-Entropy loss was
the right loss function

## Data Augmentation & Expansion
In order to avoid over-fitting problem, we need to expand artificially our handwritten
digit dataset. Data augmentation is a strategy that enables to significantly increase the
diversity of data available for our training model.
For this part, ImageDataGenerator from Keras Image Data Preprocessing was used to
generate batches of tensor image data. The following arguments were configured to aug-
ment the original data:
• Randomly rotation by 10 degrees
• Randomly Zoom by 10%
• Randomly shift images horizontally by 10% of the width
• Randomly shift images vertically by 10% of the height
• Divide inputs by std of the dataset
• Divide each input by its std
Last but not least, a manual gamma correction function was created and passed along
the ImageDataGenerator to apply gamma processing to each image.

## Model Evaluation
Model was finally fitted to the original dataset, merged with the augmented. Validation
was performed on the validation data.
The steps per epoch was calculated as train-length / batch-size, since this uses all of the
data points, one batch size worth at a time.
With the ReduceLROnPlateau function from Keras.callbacks, we reduced the Learning
Rate by half if the accuracy is not improved after 3 epochs.
