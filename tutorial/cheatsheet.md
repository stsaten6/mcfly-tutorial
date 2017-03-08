# mcfly Cheatsheet

Detailed documentation can be found in the mcfly [wiki](https://github.com/NLeSC/mcfly/wiki/Home---mcfly).

Notebook tutorials can be found in the mcfly-tutorial [repository](https://github.com/NLeSC/mcfly-tutorial)

### Jargon terms
* [**accuracy**](https://en.wikipedia.org/wiki/Evaluation_of_binary_classifiers): proportion of correctly classified items on all items
* [**convolutional layer**](http://ufldl.stanford.edu/tutorial/supervised/FeatureExtractionUsingConvolution/): type of layer where a convolutional filter is slided over the input data 
* **epoch**: One full pass through the dataset in the training algorithm
* **CNN**: Convolutional Neural Network, a deep learning network that consists mostly of convolutional layers.
* **DeepConvLSTM**: A deep learning network that consists of convolutional layers and LSTM layers
* [**gradient descent**](http://cs231n.github.io/optimization-1/): the algorithm that is used to learn the weights from training data. In each step of the gradient descent algorithm, the weights are adjusted with a step in the direction of  the gradient ('slope') .
* **hyperparameters**: In mcfly, the hyperparameters are the architectural choices of the model (number of layers, lstm or convolutional layers, etc) and the learning rate and regulization rate. 
* **layer**: A deep learning network consists of multiple layers. The more layers, the deeper your network.
* **learning rate**: the step size to take in the gradient descent algorithm
* [**LSTM layer**](http://colah.github.io/posts/2015-08-Understanding-LSTMs/): Long Term Short Memory layer. This is a special type of Recurrent layer, that takes a sequence as input and outputs a sequence.
* **regularization rate**: how strongly the [L2 regularization](http://cs231n.github.io/neural-networks-2/#reg) is applied to avoid overfitting on train data.
* **[validation set](https://en.wikipedia.org/wiki/Test_set#Validation_set)**: Part of the data that is kept apart to evaluate the performance of your model and choose hyper parameters




### Input data:
*X_train* => Nr samples **x** Nr timesteps **x**  Nr channels

*y_train_binary* => Nr samples **x** Nr classes

### Generate models:
Generate one or multiple untrained Keras models with random hyperparameters.

```
num_classes = y_train_binary.shape[1]
models = modelgen.generate_models(X_train.shape, number_of_classes=num_classes, number_of_models = 2)
```

### Train multiple models:
Tries out a number of models on a subsample of the data, and outputs the best found architecture and hyperparameters.
```
histories, val_accuracies, val_losses = find_architecture.train_models_on_samples(
  X_train, y_train_binary, X_val, y_val_binary,
  models,nr_epochs=5,subset_size=300,
  verbose=True, outputfile=outputfile)
```
### Select best model
```
best_model_index = np.argmax(val_accuracies)
best_model, best_params, best_model_types = models[best_model_index]
```

### Train one specific model (this is done with Keras function fit):
```
best_model.fit(X_train, y_train_binary,
  nb_epoch=25, validation_data=(X_val, y_val_binary))
```