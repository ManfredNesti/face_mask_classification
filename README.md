# A CNN-based classifier to recognize people with and without face mask
Project of course **[Artificial Neural Networks and Deep Learning](http://chrome.ws.dei.polimi.it/index.php?title=Artificial_Neural_Networks_and_Deep_Learning)** @ *PoliMi - Second Edition, A.A. 2021/2022, First Semester*

- Manfred Nesti (manfred.nesti@mail.polimi.it)
- Paulina Moskwa (paulina.moskwa@mail.polimi.it)

## Summary
To get started, we imported the images and split them automatically through `ImageDataGenerator` in training set (80%) and validation set (20%). The automatic division did not require any changes or improvements because both training and validation classes were balanced.
In order not to have size problems when reading images as input, we decided to arrange the images at 150x150 pixels. Only later we discovered that the size 256x256 brings much more accuracy in classification.

The first models tested were simple neural networks built by hand. They were combinations of `Conv2D`, `Flatten` and `Dense` layers. We started with a few layers and then we increased the number. However, the results were not satisfying, so we moved on to a **transfer learning** approach.

Using keras.applications we tested almost all the proposed transfer learning models. Initially we attached the CNN, without training it, to a `Flatten` layer followed by a (classifier) `Dense` layer. The results were better than the hand-crafted neural networks' ones, but not yet satisfying. We tried to add some Convolutional layers before the Flatten layer and a Dense layer before the final one. The convolutional layers didn't bring much change, the additional Dense layer did. A big improvement came with the addition of the pooling layers just after the pre-trained CNN. We tried both `MaxPooling` and `AveragePooling` (with some variations in the filters sizes) and we didn't get a general rule: the two variations fit from model to model.

At this point we tried to see what happens if we leave the CNN trainable. With a trial and error method we performed **fine-tuning** freezing of all, none, 25%, 50% and 75% of the CNN layers. We have seen, in more models, that by leaving all the layers trainable the results get better. So we concluded that pre-trained models are mostly used to determine the optimal number of layers and the optimal number of neurons for each layer.

Initially we did not give importance to what kind of **optimizer** or learning rate to use. We also did nothing to prevent overfitting. Later on, we experimented several variations. As optimizers we tested Adam, RMSpropr and Adadelta, noticing that Adadelta greatly improves the results. Moreover, we implemented early stopping, an **adaptive learning rate** and a **drop out** layer to avoid overfitting. Further improvements were brought by **image augmentation** and **L2 regularization** in the dense layers.

For the final evaluation we used an **ensemble method** selecting the 7 models that perform best on the validation set. The best transfer learning models used in the ensamble are: **Xception**, **EfficientNet**, **InceptionV3**, **DenseNet201**, **ResNet152V2**, **InceptionResNetV2**, **ResNet50**. The predictions of each model can be combined as: mode, **average**, **maximum**, **weighted average** (based on the performance on the validation set) and **weighted Borda** (based on Borda voting method). The weighted average is the best, allowing us to score in **top 20%** of the [leaderboard](https://www.kaggle.com/competitions/artificial-neural-networks-and-deep-learning-2020/leaderboard).
