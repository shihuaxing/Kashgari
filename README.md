# Kashgari

[![Pypi](https://img.shields.io/pypi/v/kashgari.svg)](https://pypi.org/project/kashgari/)
[![Python version](https://img.shields.io/pypi/pyversions/Kashgari.svg)](https://www.python.org/downloads/release/python-360/)
[![Travis](https://img.shields.io/travis/BrikerMan/Kashgari.svg)](https://travis-ci.org/BrikerMan/Kashgari)
[![](https://img.shields.io/coveralls/github/BrikerMan/Kashgari.svg)](https://coveralls.io/github/BrikerMan/Kashgari)
[![Issues](https://img.shields.io/github/issues/BrikerMan/Kashgari.svg)](https://github.com/BrikerMan/Kashgari/issues)
![Contributions welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)
[![License: MIT](https://img.shields.io/badge/License-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)
[![](https://img.shields.io/pypi/dw/kashgari.svg)](https://pypi.org/project/kashgari/)

Simple and powerful NLP framework, build your own state-of-art model in 5 minutes.

Kashgare is:

* **Human-friendly framework**. Kashgare's code is very simple, well documented and tested, which makes it very easy to understand and modify.
* **Powerful and simple NLP library**. Kashgare allows you to apply state-of-the-art natural language processing (NLP) models to your text, such as named entity recognition (NER), part-of-speech tagging (PoS) and classification.
* **A Keras NLP framework**. Kashgare builds directly on Keras, making it easy to train your own models and experiment with new approaches using different embeddings and model structure.

 
## Feature List 

* Embedding support
    * Classic word2vec embedding
    * BERT embedding
* Text Classification Models
    * CNN Classification Model
    * CNN LSTM Classification Model
    * Bidirectional LSTM Classification Model
* Text Labeling Models (NER, PoS)
    * Bidirectional LSTM Labeling Model
    * Bidirectional LSTM CRF Labeling Model
    * CNN LSTM Labeling Model
* Model Training
* Model Evaluate
* GPU Support
* Customize Model

## Roadmap
* ELMo Embedding
* Pre-trained models
* More model structure

## Tutorials

* [Tutorial 1: Word Embeddings](docs/Tutorial_1_Embedding.md)
* [Tutorial 2: Classification Model](docs/Tutorial_2_Classification.md)
* [Tutorial 3: Sequence labeling Model](docs/Tutorial_3_Sequence_Labeling.md)

### Chinese

* [15分钟搭建中文文本分类模型](https://eliyar.biz/nlp_chinese_text_classification_in_15mins/)

## Quick start

### Requirements and Installation
The project is based on Keras 2.2.0+ and Python 3.6+, because it is 2019 and type hints is cool.

```bash
pip install kashgari
# CPU
pip install tensorflow
# GPU
pip install tensorflow-gpu 
```

### Example Usage
lets run a text classification with CNN model over [SMP 2017 ECDT Task1](http://ir.hit.edu.cn/smp2017ecdt-data).

```python
>>> from kashgari.corpus import SMP2017ECDTClassificationCorpus
>>> from kashgari.tasks.classification import CNNLSTMModel

>>> x_data, y_data = SMP2017ECDTClassificationCorpus.get_classification_data()
>>> x_data[0]
['你', '知', '道', '我', '几', '岁']
>>> y_data[0]
'chat'

# provided classification models `CNNModel`, `BLSTMModel`, `CNNLSTMModel` 
>>> classifier = CNNLSTMModel()
>>> classifier.fit(x_data, y_data)
_________________________________________________________________
Layer (type)                 Output Shape              Param #   
=================================================================
input_1 (InputLayer)         (None, 10)                0         
_________________________________________________________________
embedding_1 (Embedding)      (None, 10, 100)           87500     
_________________________________________________________________
conv1d_1 (Conv1D)            (None, 10, 32)            9632      
_________________________________________________________________
max_pooling1d_1 (MaxPooling1 (None, 5, 32)             0         
_________________________________________________________________
lstm_1 (LSTM)                (None, 100)               53200     
_________________________________________________________________
dense_1 (Dense)              (None, 32)                3232      
=================================================================
Total params: 153,564
Trainable params: 153,564
Non-trainable params: 0
_________________________________________________________________
Epoch 1/5
 1/35 [..............................] - ETA: 32s - loss: 3.4652 - acc: 0.0469

... 

>>> x_test, y_test = SMP2017ECDTClassificationCorpus.get_classification_data('test')
>>> classifier.evaluate(x_test, y_test)
              precision    recall  f1-score   support
         
        calc       0.75      0.75      0.75         8
        chat       0.83      0.86      0.85       154
    contacts       0.54      0.70      0.61        10
    cookbook       0.97      0.94      0.95        89
    datetime       0.67      0.67      0.67         6
       email       1.00      0.88      0.93         8
         epg       0.61      0.56      0.58        36
      flight       1.00      0.90      0.95        21
...
```

### Run with Bert Embedding

```python
from kashgari.embeddings import BERTEmbedding
from kashgari.tasks.classification import CNNLSTMModel
from kashgari.corpus import SMP2017ECDTClassificationCorpus

bert_embedding = BERTEmbedding('bert-base-chinese', sequence_length=30)                                   
model = CNNLSTMModel(bert_embedding)

train_x, train_y = SMP2017ECDTClassificationCorpus.get_classification_data()
model.fit(train_x, train_y)
```

### Run with Word2vec embedded

```python
from kashgari.embeddings import WordEmbeddings
from kashgari.tasks.classification import CNNLSTMModel
from kashgari.corpus import SMP2017ECDTClassificationCorpus

bert_embedding = WordEmbeddings('sgns.weibo.bigram', sequence_length=30)                                  
model = CNNLSTMModel(bert_embedding)
train_x, train_y = SMP2017ECDTClassificationCorpus.get_classification_data()
model.fit(train_x, train_y)
```

## Reference
This library is inspired and reference following framework and papers.

* [flair - A very simple framework for state-of-the-art Natural Language Processing (NLP)](https://github.com/zalandoresearch/flair)
* [anago - Bidirectional LSTM-CRF and ELMo for Named-Entity Recognition, Part-of-Speech Tagging](https://github.com/Hironsan/anago)
* [Chinese-Word-Vectors](https://github.com/Embedding/Chinese-Word-Vectors)
