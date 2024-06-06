---
description: >-
  Briefly explains about retraining a translation model to accommodate a
  domain-specific usecase.
---

# Model Retraining

The production environment of Anuvaad runs on top of translation models trained in the general domain, which will cover a good amount of scenarios. However, in case we need a separate instance of Anuvaad to translate domain-specific data, like financial, biomedical, etc: the existing model must be finetuned with more relevant data in a particular domain to improve the accuracy of translation. This page briefly summarises how it could be done

## Data Collection

Bi-lingual, or parallel dataset is required for training the model. It is nothing but the same sentence pair in both source and target language. Example:

> Source(en): India is my country
>
> Target(hi): भारत मेरा देश है

The more the amount of available data, the more accurately the model could be trained. In short, data collection could be done in one of the following three approaches

#### Manual Annotation by linguistic experts

This is exhaustive but the best approach. At least a small sample size of the dataset must be manually curated and used for validation purposes

#### Creation of Corpus by web and document crawling

Certain websites will have the same data in multiple languages. The idea is to somehow find matching pairs of sentences from them. Scraping could be done using frameworks such as [selenium](https://www.selenium.dev/) and sentence matching could be done by using techniques such as [LaBSE](https://huggingface.co/sentence-transformers/LaBSE). This method if used properly can produce huge amounts of data without much manual effort. however random manual verification is recommended to ensure data accuracy

A lot of sample crawlers for reference are available in this [repo](https://github.com/project-anuvaad/anuvaad-corpus-tools/tree/master)

To do sentence matching of scraped sentences, Anuvaad aligner also could be used, which is implemented using LaBSE. The specs for [Aligner](https://anuvaad.sunbird.org/use/service-contracts#aligner) is available [here](https://github.com/project-anuvaad/anuvaad/blob/master/anuvaad-etl/anuvaad-extractor/aligner/docs/etl-aligner-api-contract.yml)&#x20;

#### Purchasing or using an open-sourced dataset

Very often, datasets will be made available by certain research institutes or private vendors. This data also could be included to increase the quantity of training data

## Data cleaning & formatting

The raw data that is purchased or web-scraped might have too much noise which could affect the training accuracy and thereby translation. Noise includes unwanted characters, blank spaces, bullets and numbering,html tags etc.

The basic script for sentence alignment, cleaning and formatting is available [here](https://github.com/project-anuvaad/anuvaad-corpus-tools/tree/master/dataset-cleaner)&#x20;

However, more rules for cleaning could be applied based on context and manual verification of raw data as per the scenario.

## Model retraining

The present default model of Anuvaad is Indictrans. The instructions to retrain and benchmark an  Indicrans model is explained [here](https://github.com/AI4Bharat/indicTrans)

The training repo of legacy openNMT-py models is available [here](https://github.com/project-anuvaad/nmt-training)&#x20;

{% hint style="info" %}
Once a model is retrained, if there are plans to open-source it, hosting it in [Dhruva](https://dhruva.ai4bharat.org/) will facilitate seamless integration with Anuvaad.
{% endhint %}
