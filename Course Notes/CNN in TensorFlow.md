## TensorFlow version
TensorFlow 2 has been released, bringing with it major upgrades and changes!

Colab has two versions of TensorFlow pre-installed: 1.x and 2.x. Colab currently uses TensorFlow 1.x by default. To change it to 2.x, do the following:
```python
%tensorflow_version 2.x

import tensorflow
print(tensorflow.__version__)
```

### Avoid using `pip install` with GPUs and TPUs

Colab builds TensorFlow from source to ensure compatibility with our fleet of accelerators. Versions of TensorFlow fetched from **PyPI** by `pip` may suffer from performance problems or may not work at all.

## Visualizing Intermediate Representations

![](intermediate_representations.png)
As you can see we go from the raw pixels of the images to increasingly abstract and compact representations. The representations downstream start highlighting what the network pays attention to, and they show fewer and fewer features being "activated"; most are set to zero. This is called `sparsity`. Representation sparsity is a key feature of deep learning.

These repreesntations carry increasingly less information about the original pixels of the image, but increasingly refined information about the class of the image. You can think of a **convnet** (or a deep network in general) as an `information distillation pipeline`.

![](evaluate_training.png)
As you can see, we are **overfitting** like it's getting out of fashion. Our training accuracy (in blue) gets close to 100% (!) while our validation accuracy stalls as 70%. Our validation loss reaches its minimum after only five epochs.

Since we have a relatively small number of training examples (2000), overfitting should be our number one concern. Overfitting happens when a model exposed to too few examples learns patterns that do not generalize to new data, i.e. when the model starts using irrelevant features for making predictions.