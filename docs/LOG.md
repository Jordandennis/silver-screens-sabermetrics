# Development Logs

## June 12, 2026

License and repo scaffolding created. Before work begins, I'm going to brainstorm with Claude over some of the main questions, and then start looking for sources for research in order to hammer down the statistical aspects of our scoring system. 

With respect to WAR, the main question for this project immediately becomes *how can you define a replacement level actor?* On top of that, how about other roles associated with the film? Well, in order to define this, I think it will be important to first consider exactly how much any individual role plays into the outcome of a film, on average. Nobody expects the 6th billed actor to contribute as much as the director, but exactly how much weight can be put on any role.

This specific question will be very tricky to answer. Colinearity is going to be a serious problem with this data, certainly; Yorgos Lanthamos works with Jesse Plemmons and Emma Stone frequently, and Martin Scorcese and Leonardo Dicaprio or Robert Deniro stick together like white on rice. Teasing apart the individuals from those they frequently work with, let alone the production companies involved and the whole mess of other frequent groups found in the industry is going to be tough

**Perhaps there could be a "frequent group" score for those who commonly work well together!**

In the mean time, I think one of the first courses of action has to be to start teasing out some role scores. This alone should result in some interesting findings, at least to me! My initial idea was a neural network, but Claude recommended Gradient Boosted Trees for the readability of findings. I have some familiarity with the theory behind this model, but I should familiarize myself with it better before beginning work on creating it, both for ease of creation and to ensure it's a good fit.

**Action Item: Consult *"Hands-On Machine Learning with Scikit-Learn, Keras & TensorFlow - Third Edition" by Aurelien Geron 2023* (And learn how to add e's with hats) for information on gradient boosted trees (XGBoost/LightGBM per Claude).** 

**Action Item: Verify sources in prior work flagged by claude. These should be available through the UVic library**