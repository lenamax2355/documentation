---
layout: post
title:  "Which algorithms are in healthcare.ai?"
date:   2016-12-21 20:17:11 -0700
tags: algorithms
author: Levi Thatcher
categories: blog
---

Machine learning has been around for decades and has been used to solve lots of problems. Some of these include [spam filtering](http://ats.cs.ut.ee/u/kt/hw/spam/spam.pdf) for email, [suggestions on Netflix](http://techblog.netflix.com/2012/04/netflix-recommendations-beyond-5-stars.html), [optimized playlists on Spotify](http://qz.com/571007/the-magic-that-makes-spotifys-discover-weekly-playlists-so-damn-good/), [custom recommendations on Amazon](https://www.cs.umd.edu/~samir/498/Amazon-Recommendations.pdf), [facial recognition on Facebook](https://medium.com/@ageitgey/machine-learning-is-fun-part-4-modern-face-recognition-with-deep-learning-c3cffc121d78#.7b9c9jmg7), [voice recognition](https://research.googleblog.com/2015/09/google-voice-search-faster-and-more.html) on your phone, [language translation](http://www.nytimes.com/2016/12/14/magazine/the-great-ai-awakening.html) on demand, [image search](http://fusion.net/story/142326/the-new-google-photos-app-is-disturbingly-good-at-data-mining-your-photos/) in your photo app, and [many more](https://www.kaggle.com/wiki/DataScienceUseCases).

While reading that long and varied list, you may be wondering where healthcare stands by comparison. Even though machine learning can solve many problems in healthcare, the field has not yet seen significant adoption. As [we've mentioned](http://healthcare.ai/blog/2016/12/01/welcome-to-healthcareai/), the goal of healthcare.ai is to change that.

We plan to bring the benefits of machine learning into healthcare by starting with the low-hanging fruit. Health Catalyst is a very practical company and that is reflected in [healthcare.ai](http://healthcare.ai/). While many of the machine learning projects mentioned above are using advanced algorithms like [deep learning](https://en.wikipedia.org/wiki/Deep_learning), healthcare.ai is instead starting with the workhorses of the algorithm world. Note that this a different approach to healthcare machine learning compared to that of [Google](https://research.google.com/teams/brain/healthcare/) and [Microsoft](http://searchhealthit.techtarget.com/opinion/Microsoft-Project-Adam-may-reach-healthcare-specialties), which are focusing on the sexier (but less practical) deep learning applications in healthcare.

Before starting the discussion on healthcare.ai algorithm choices, I should note that we'll focus on our R package and on [classification](https://en.wikipedia.org/wiki/Statistical_classification) (rather than regression), since *most* problems in healthcare revolve around predicting [Yes or No](http://healthcare.ai/blog/2016/12/12/what-models-has-health-catalyst-created/) rather than a continuous variable. To make the terminology clear, it should also be stated that a machine learning algorithm, when paired with data, leads to a model. The algorithms exist off the shelf. The great value-add comes from pairing the proper algorithm with the data of interest. Healthcare.ai has open-sourced tools that allow you to easily match your data with suitable algorithms, create models, and help you answer your most important business questions. The models that you and Health Catalyst create are proprietary, but the tools used to make those models are free. 

Arguably the most simple and common algorithm when trying to classify things via machine learning is [logistic regression](https://en.wikipedia.org/wiki/Logistic_regression). (Note that despite the name, this algorithm is used for classification problems.) We love it because it's easy to use, quick to finish, and easy to interpret. We've been using this algorithm, but with a twist to it. 

We built healthcare.ai with the goal of providing users with guidance as to which [features](https://en.wikipedia.org/wiki/Feature_(machine_learning)) (i.e., variables) were predictive and worth using when building a model. This drove us to use [lasso](https://en.wikipedia.org/wiki/Lasso_(statistics)), which is a linear model much like logistic regression, but it provides feedback on which features should or shouldn't be included in the model. You might say, "Well, couldn't logistic regression just ignore features that weren't predictive?" Yes, but when the user has brought 20-40 variables into a focused dataset—what Health Catalyst calls a source area mart—to see if they help predict Sepsis, they'll often only want to keep those variables that are predictive, as ETL processes often have hard resource constraints. With knowledge of how important each feature is, one can often remove many non-predictive variables from a model without any significant loss in accuracy.

Next to linear models like lasso and logistic regression, the most common algorithm in machine learning is the [random forest](https://en.wikipedia.org/wiki/Random_forest). It's different in that it can model non-linear relationships accurately. The random forest algorithm is an ensemble method, which aggregates the result of 100+ randomized decision trees to produce a prediction. While it's a little harder to interpret than linear algorithms (like lasso), it typically doesn't need much [hyperparameter tuning](https://en.wikipedia.org/wiki/Hyperparameter_optimization), can run quite quickly, and from our experience often provides more accurate models (compared to linear algorithms) for common healthcare questions. These reasons drove us to include a random forest option in healthcare.ai.

To back up a bit, in typical machine learning problems row order doesn't matter. If you think of the simple example of housing data in Salt Lake City to determine the relationship between square footage and house price, it doesn't matter if the row for house 10045 was listed before house 10057 in the dataset. Those two rows are treated independently. In longitudinal datasets however, the relationship between the rows often *does* matter. In certain clinical datasets, we often find multiple entries for the same person, showing the person's progression over time. When this progression is important to the business question that's being addressed with machine learning, basic machine learning might not be suitable.

This frequent longitudinal aspect of healthcare data is why we're excited to offer linear [mixed models](https://en.wikipedia.org/wiki/Mixed_model) in our the R version of healthcare.ai. Mixed models offer the ability to combine a personal trend with a population trend. It's called a mixed model because it combines fixed effects (i.e., those that relate to the population as a whole) with random effects (i.e., those that are innate to that individual). [This paper](http://www.bodowinter.com/tutorial/bw_LME_tutorial.pdf) provides a good introduction to the topic. From our experience, this algorithm is slow compared to random forest and lasso, but can create a better model if the prediction at hand significantly depends on a person's history (i.e., think diabetic amputation risk rather than [CLABSI](https://en.wikipedia.org/wiki/Central_venous_catheter#Bloodstream_infections)). As always, healthcare.ai makes it easy to see how this algorithm performs on your dataset, and determine if it does a better job than the more common lasso and random forest algorithms.

Again, the goal of healthcare.ai is to help the medical community use machine learning to improve healthcare outcomes. On the first pass, we have implemented what we feel are the simplest and most effective algorithms specific to healthcare data. We’ll certainly expand in the future, but for now there are a lot of efficiency gains to be <achieved></achieved> with basic algorithms, standardized performance metrics, smart implementation, and centralized documentation. 

If you want more detail, check out [our code](https://github.com/HealthCatalystSLC/healthcareai-r). If you have questions or feedback, [contact us](http://healthcare.ai/contact)!