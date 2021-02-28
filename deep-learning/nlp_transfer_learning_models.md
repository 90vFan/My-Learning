[TOC]



## Transfer Learning in NLP

### Transfer Learning

- Feature based: word vector
- Fine tuning: add new layer of unfreeze last layers, predict words by context

![1613807038417](images/1613807038417.png)

### Models

#### CBOW

- Context windows
- Feed forward neural network

#### ELMo

- Full sentenceTransfer Learning in NLP
  Transfer Learning
  Feature based: word vector

  Fine tuning: add new layer of unfreeze last layers, predict words by context

  ￼

  Models
  CBOW
  Context windows

  Feed forward neural network

  ELMo
  Full sentence

- Transfer Learning in NLP
  Transfer Learning
  Feature based: word vector

  Fine tuning: add new layer of unfreeze last layers, predict words by context

  ￼

  Models
  CBOW
  Context windows

  Feed forward neural network

  ELMo
  Full sentenceBi-directional context

- LSTM

![1613807182795](images/1613807182795.png)

#### GPT

- Transformer **Decoder**
- **Uni-directional** context

#### BERT

Bi-derectional Encoder Representation from Transformers

- Transformer **Encoder**
- **Bi-derectional** context
- multi-mask, **msked** language modeling, mask 15% words for prediction
- **next sentence** prediction

Two steps:

- pre-training
- fine-tuning: choose 15% tokens at random, mask them 80% of the time

![1613809343252](images/1613809343252.png)

#### T5

- Transfromer Encoder-Decoder
- Bi-directional context
- multi-task
- **masking**

