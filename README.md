# Evaluating German T5 Models on GermEval 2014 (NER)

This repository presents an **on-going** evaluation of [German T5 models](https://huggingface.co/GermanT5)
on the [GermEval 2014](https://sites.google.com/site/germeval2014ner/data) NER downstream task.

# Changelog

* 03.02.2023: Initial version.

# Fine-Tuning T5

A few approaches exist for fine-tuning T5 models for token classification tasks:

* ["Structured Prediction as Translation between Augmented Natural Languages"](https://arxiv.org/abs/2101.05779)
* ["Autoregressive Structured Prediction with Language Models"](https://arxiv.org/abs/2210.14698)

These approaches tackle the token classification task as a sequence-to-sequence task.

However, it is also possible to use obly the encoder of a T5 model for downstream tasks as presented in:

* ["EncT5: A Framework for Fine-tuning T5 as Non-autoregressive Models"](https://arxiv.org/abs/2110.08426)

The proposed "EncT5" architecture was not evaluated on token classification tasks.

This repository uses the [Flair library](https://github.com/flairNLP/flair) and encoder-only fine-tuning is performed
for the GermEval 2014 NER dataset. The recently released [T5 models for German](https://huggingface.co/GermanT5) are
used as LM backbones.

# Results

We perform a basic hyper-parameter search over and report micro F1-Score, averaged over 5 runs (with different seeds).
Score in brackets indicates result on development split.

| Model Size                                                                      | Configuration        | Run 1           | Run 2           | Run 3           | Run 4           | Run 5           | Avg.
| ------------------------------------------------------------------------------- | -------------------- | --------------- | --------------- | --------------- | --------------- | --------------- | ---------------
| [Small](https://huggingface.co/GermanT5/t5-efficient-gc4-all-german-small-el32) | `bs16-e10-lr0.00011` | (87.24) / 85.53 | (86.40) / 85.63 | (86.50) / 85.47 | (86.32) / 85.57 | (86.77) / 85.38 | (86.65) / 85.52
| [Large](https://huggingface.co/GermanT5/t5-efficient-gc4-all-german-large-nl36) | `bs16-e10-lr0.00011` | (87.16) / 86.46 | (87.07) / 85.76 | (87.46) / 85.57 | (87.05) / 86.91 | (87.15) / 86.11 | (87.18) / 86.16

For hyper-parameter search, the script `flair-fine-tuner.py` is used in combination with a configuration file (passed as argument).
All configuration files are located under `./configs` that were used for the experiments here.

## Baselines:

* [Fine-tuned DistilBERT](https://huggingface.co/dbmdz/flair-distilbert-ner-germeval14) models reports (86.84) / 85.62.
* [GELECTRA Base](https://aclanthology.org/2020.coling-main.598/) reports 86.02 on test set.
* Current SOTA is [GELECTRA Large](https://aclanthology.org/2020.coling-main.598/) with 88.95 on test set.

# Hardware/Requirements

Latest Flair version (commit [6da65a4](https://github.com/flairNLP/flair/tree/6da65a4f665bb333f639067bc5f868046880e63b)) is used for experiments.

All models are fine-tuned on A10 (24GB) instances from [Lambda Cloud](https://lambdalabs.com/service/gpu-cloud).
