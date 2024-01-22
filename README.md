## 94% Test Accuracy on the CIFAR-10 dataset using a custom 20-layer Residual Network

The diverse range of object classes and the complexity of scenes in the CIFAR-10 dataset, coupled with the computational cost required for hypertuning make it challenging to achieve high test accuracies. In this project, I initially built a 11-layer Neural Network that achieved a **92.81%** test accuracy. I further built a 20-layer custom Residual Network that achieved a **94.47%** test accuracy. In both the cases, the models were trained on the TPU from Google Colab. 

**Note**:
While the original ResNet paper did include studies on the CIFAR-10 dataset, my approach is entirely distinct, incorporating a novel model architecture, unique image augmentation strategies, and a set of hyperparameters that deviates entirely from the ResNet paper's discussions.

### Neural Network:
- Rigorous hypertuning was performed to come up with the optimal neural network architecture and other hyperparameters. `he_normal` weight initialization helped prevent vanishing gradients and *batch normalization* helped stabilize the performance.
- *Exponential Decay* of learning rate was implemented and hypertuning was done to determine the appropriate initial learning rate and decay rate, which resulted in a peak validation accuracy of 89.84%. However, surpassing the 90% threshold proved challenging due to overfitting.
- A set of simple image augmentations were introduced, which helped mitigate overfitting to some extent and pushed the peak validation accuracy to around 92%. Further increase in the depth of the neural network either led to vanishing gradients or degradation of performance.
- At this point, I moved on to building my Residual Network on which additional experiments on image augmentations were conducted (see below). The set of augmentations that gave the best results with the Residual Network, positively impacted even the neural network's performance and it finally achieved a peak validation accuracy of **93.44%** and a test accuracy of **92.81%**
- Notably, my 11-layer neural network's performance was already on par with the ResNet-44 from the ResNet paper, which had a 7.17 error %.

### Image augmentations and normalization:
- Different combinations of image augmentation techniques had a varied effect on the model performance. Through meticulous experimentation, I was able to indentify two sets of augmentations that consistently yielded the best performance:

| Augmentations | Peak Validation Accuracy (%) | Test Accuracy (%) |
|---------------|:----------------------------:|:-----------------:|
| Random left-right flip, contrast, brightness, random crop and resize | 94.72 | 94.47 |
| Random left-right flip, random crop and resize, gaussian noise (stddev=2.0)| 94.88 | 94.31|

(The above results are based on the performance of my 20-layer Residual Network.)

- Random horizontal flip and random cropping followed by resizing turned out to be powerful image augmentations for the CIFAR-10 dataset. Using the **Lanczos-3** method while resizing, particularly helped the model, pushing the validation accuracy above 94%.
- Image normalization to the [0, 1] range outperformed alternative options including normalization to the [-1, 1] range or performing channel-wise Z-Score normalization of images.

### Residual Network:
- In Residual Blocks where the channel depth increased, *zero-padding shortcuts* performed considerably better than shortcut connections with *1x1 convolutions*. Also, reducing the height and width of the shortcut connection using *Average Pooling* generally led to superior performance than using *Max Pooling*.
- A combination of l2 regularization and Dropout was found to give the best results.
- Ultimately, the residual network was able to achieve a peak validation accuracy of **94.72%** and a test accuracy of **94.47%**. In some iterations my model even achieved peak validation accuracies as high as 95.42% but those results were not as consistent.

The studies conducted on the CIFAR-10 dataset in the original ResNet paper show that their ResNet-110 performed the best with a test accuracy of 93.57% (6.43 error %). While the focus of that paper was not on pushing for the best results, it can be observed that my 20-layer Residual Network managed to outperform their ResNet-110.
