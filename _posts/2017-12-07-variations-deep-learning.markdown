---
title: "Variations of Deep Learning for Cancer Diagnosis Using Gene Expression Data"
layout: post
date: 2017-12-07 20:03
tag:
- deep learning
- machine learning
<!-- image: https://koppl.in/indigo/assets/images/jekyll-logo-light-solid.png
headerImage: true -->
projects: true
hidden: true # don't count this post in blog pagination
description: "My graduation project, in which I used Deep Learning applied to gene expression data for cancer diagnosis"
category: project
author: henriquesilveira
externalLink: false
pipelinepicture: assets/images/architecture.png
---

Before graduating, undergraduate students in Brazil usually have to work on a final project. We usually call it *Trabalho de Conclusão de Curso* (or TCC, for short). And as a Computer Engineering undergraduate student at University of São Paulo, I completed and presented my final project a couple of days ago, a milestone that triggered my graduation ceremony countdown.

It commonly works like this: students can choose a subject they like and then look for a professor to be their tutor throughout the project. In my case, I decided to work in the Machine Learning field and, more specifically, Deep Learning algorithms. This is mainly due to two reasons: I lacked a strong background in Machine Learning, a very important skill for IT professionals nowadays; and, of course, because I was fascinated by the subject, so I wanted to learn more about it. I was lucky to be a student at University of São Paulo, where Professor [André Carvalho](http://bv.fapesp.br/pt/pesquisador/34735/andre-carlos-ponce-de-leon-ferreira-de-carvalho/) is a renowned authority in Artificial Intelligence, Data Mining and Computational Biology. Under the guidance of Professor Carvalho, I developed my final project, entitled **Variations of Deep Learning for Cancer Diagnosis Using Gene Expression Data**.

The project aimed to investigate the potential of different Deep Learning algorithms in the analysis of gene expression data for cancer diagnosis. To do that, I used public domain gene expression datasets in multiple experiments that consisted of an automated pipeline, combining different Machine Learning techniques and comprehending three phases: balancing data, data dimensionality reduction and data classification.

![Pipeline]({{ site.url }}/{{ page.pipelinepicture }})

The experiments involved three different Deep Learning Architectures: Stacked Denoising Autoencoders, for data dimensionality reduction; and Multilayer Perceptron and Convolutional Neural Networks, for data classification. The results demonstrated good performance of the Deep Learning techniques: the Stacked Denoising Autoencoder presented good performance compared to other data dimensionality reduction techniques, indicating that it can potentially aid in the analysis of highly dimensional data such as gene expression data. Furthermore, the Deep Learning models presented the best results among the classification techniques, and the performance of the Convolutional Neural Networks may indicate suitability to use the convolution operation in the analysis of gene expression data.

The project, including the code, the results and the dissertation, can be accessed [here](https://github.com/henrisilver/DeepLearningCancer).
