# Partial transfusion: on the expressive influence of trainable batch norm parameters for transfer learning


> TL;DR: Fine-tuning only the batch norm affine parameters leads to similar performance as to fine-tuning all of the model parameters, based on results obtained on seven publicly available 2D medical imaging datasets.



Jupyter notebook code for reproducing the results of the [paper](https://arxiv.org/abs/2102.05543).



## Abstract

Transfer learning from ImageNet is the go-to approach when applying deep learning to medical images. The approach is either to fine-tune a pre-trained model or use it as a feature extractor. Most modern architecture contain batch normalisation layers, and fine-tuning a model with such layers requires taking a few precautions as they consist of trainable and non-trainable weights and have two operating modes: training and inference. Attention is primarily given to the non-trainable weights used during inference, as they are the primary source of unexpected behaviour or degradation in performance during transfer learning. It is typically recommended to fine-tune the model with the batch normalisation layers kept in inference mode during both training and inference. In this paper, we pay closer attention instead to the trainable weights of the batch normalisation layers, and we explore their expressive influence in the context of transfer learning. We find that only fine-tuning the trainable weights (scale and centre) of the batch normalisation layers leads to similar performance as to fine-tuning all of the weights, with the added benefit of faster convergence. We demonstrate this on a variety of seven publicly available medical imaging datasets, using four different model architectures.


## Tensorflow example

```python

from tensorflow import keras

base_model = keras.applications.DenseNet121(include_top=False, pooling='avg')

input_layer = keras.Input((None, None, 3))
x = base_model(input_layer,
               training=False)  # training=False to freeze the moving average updates of the batch norm layers
x = keras.layers.Dropout(0.25)(x)
x = keras.layers.Dense(1, activation='sigmoid')(x)
model = keras.Model(input_layer, x)

# set only the batch norm layers as trainable in the base model
for layer in base_model.layers:
    layer.trainable = True if isinstance(layer, keras.layers.BatchNormalization) else False

```



```
@misc{kanavati2021partial,
      title={Partial transfusion: on the expressive influence of trainable batch norm parameters for transfer learning}, 
      author={Fahdi Kanavati and Masayuki Tsuneki},
      year={2021},
      eprint={2102.05543},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```