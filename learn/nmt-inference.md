# NMT Inference

## NMT Inference

This module provides the NMT based translation service for various Indic language pairs. Currently the NMT models are trained using OpenNMT-py framework version 1 and the model binaries are generated using ctranslate2 module provided for OpenNMT-py and the same is used to generate model predictions.

### Data preparation

NMT requires parallel corpus between languages. Typically the size of language corpus is in the millions. The language corpus must have enough examples to cover various situations. This is one of the most important portions of the system and a very time consuming work where quality of data has to be checked to ensure the accuracy of translation. At Anuvaad, we have collected data for 11 languages as parallel corpora. The corpus is available under MIT license.

### Training and Retraining

Training and retraining is a continuous process and training is dependent on the quality of input dataset. We have to constantly monitor the quality of translation. The translation mistakes should be used to generate training examples and retraining exercises have to periodically be taken up. The training cycle is a costly affair as they need GPU infrastructure and long training hours.

### Model Evaluation

The model output is evaluated on the per-selected sentences and BLEU score is calculated. BLEU score provides a score that helps as the guidance to provide feedback on the model quality. Translation output has to be evaluated by human translators as well before it can be used in a production environment.

### Architecture

Anuvaad uses the current state-of-the-art Transformer model to achieve target sentence prediction or translation Supporting code and paper is in open source domain, [LINK](https://arxiv.org/abs/1706.03762)



<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

We are leveraging an open-source project called “openNMT” and also exploring “FairSeq”(IndicTrans) from the perspective of enhancement and usage. The deeplearning platform used is pytorch

### MODEL TRAINING

* Vocabulary or dictionary generation
  1. Tokenizer (Detokenizer) or breaking of given sentence in word or sub-word. (Language specific) Moses or IndicNLP(for indian languages)
  2. Sentence Piece or subword-nmt Supporting code and paper is in open source domain, [LINK](https://github.com/google/sentencepiece.git)

### Approaches used:

* BPE (Byte Pair Encoding)
* Unigram
* Tune model parameters and hyper parameters to improve accuracy.

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

### Modules

* [anuvaad-nmt-inference](https://github.com/project-anuvaad/anuvaad/tree/develop/anuvaad-nmt-inference)&#x20;
* Opennmt-py based

### Training Repo

[https://github.com/project-anuvaad/nmt-training](https://github.com/project-anuvaad/nmt-training)

### API Details

* [Api contract](https://github.com/project-anuvaad/anuvaad/tree/develop/anuvaad-nmt-inference/docs/contracts/apis)&#x20;

### aai4b-nmt-inference (indicTrans)

* [https://github.com/project-anuvaad/aaib4-inference](https://github.com/project-anuvaad/aaib4-inference)
* Fairseq based

### Training Repo

[https://github.com/AI4Bharat/indicTrans](https://github.com/AI4Bharat/indicTrans)

### API Details

[Api contract](https://github.com/project-anuvaad/aaib4-inference/tree/main/docs/contracts/apis)

### Prerequisites

* python 3.6
* ubuntu 16.04

Install various python libraries as mentioned in requirements.txt file

```bash
pip install -r requirements.txt
```

### APIs and Documentation

Run app.py to start the service with all the packages installed

```bash
python src/app.py
```

For more information about api documentation, please check @ [https://github.com/project-anuvaad/aaib4-inference/tree/main/docs/contracts](https://github.com/project-anuvaad/aaib4-inference/tree/main/docs/contracts)

### License

[MIT](https://choosealicense.com/licenses/mit/)
