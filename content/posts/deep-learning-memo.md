---
title: "Deep Learningメモ"
date: 2018-08-30T10:21:25+09:00
draft: true
---


Except on very simple problems, `RMSProp` optimizer almost always performs much better than `AdaGrad`, It also generally performs better than Momentum optimization and Nesterov Accelerated Gradients. In fact, it was the preferred optimization algorithm of many researchers until Adam optimization came around.


You can actually use a linear model to fit nonlinear data. A simple  way to do this is to add powers of each feature as new features, then train a linear model on this extended set of features. This technique is called `Polymonial Regression`
