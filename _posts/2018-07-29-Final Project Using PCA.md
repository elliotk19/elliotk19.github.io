---
layout: post
title: Using Principal Component Analysis (PCA) Final(Abstract,Question, Variables and Hypothesis, and Background):The Human Microbiome Dataset
category: Summer Camp
tag: [COSMOS, Final Project, Camp]
---
After using PCA for two practice datasets, one of which the professor provided and one that I found by myself, our group decided to move onto the dataset that we needed to analyze for the final project. As introduced and explained on the previous posts regarding the final project, we needed to analyze the human microbiome dataset using PCA. But before getting into the code, let's first take a look at the informative aspects of the project.
<h2> Abstract </h2>
Microbiomes, microscopic communities of bacteria in a particular environment, exist all over the human body and are responsible for carrying out numerous essential biological functions, both good and bad. In fact, bacteria living on the human body outnumber human cells 10 to 1. Despite their tremendous importance, microbiomes are difficult for researchers to study and understand due to their complexity and minuscule scale. Our paper examines data from The Human Microbiome Project, which contains 2,913 individual samples gathered from 242 human subjects, measuring 43,144 variables. Using principal component analysis (PCA) to reduce our multi-dimensional data, we are able to compare the clustering of microbes on various regions of the body. By examining the bacterial composition of these samples and applying PCA, we classified the samples according to the region of the body from which the sample was gathered. By plotting the data points onto the principal component axes and analyzing the linear clustering of the data, we were able to conclude that different parts of the body contain different types of bacteria.
<h2> Question </h2>
Utilizing Principal Component Analysis, in what ways can we classify the human microbiome?
<h2> Variables and Hypothesis </h2>
The independent variables were the gender and regions from which the samples were taken.
The dependent variables were the 43,144 different OTUs that were present in the various microbiomes. We hypothesize that PCA will produce clearly distinguishable clusters for each of the regions and genders from which the sample was taken. The Principal Components will be useful for explaining differences in the OTUs present in each microbiome.
<h2> Background </h2>
<h5> Human Microbiome Project </h5>
The [Human Microbiome Project](https://www.hmpdacc.org/hmp/overview/) has the mission of generating resources that would enable the comprehensive characterization of the human microbiome and analysis of its role in human health and disease. The data analyzed for this investigation was sourced from the Human Microbiome Project The data set was then checked for mislabeling and sample contamination. We used the variable region 1-3 (V13) OTU table for our analysis.
<h5> Human Microbiome </h5>
Microbiomes are communities of bacteria, viruses, fungi, and other microbial organisms that exist all over the bodies of every eukaryotic organism. The specific microbiomes present on the human body are responsible for vital functions such as food digestion, producing vitamins, and recognizing malicious microbes. Disruption or unbalance of microbiomes can cause many health problems, such as cancer, diabetes, obesity, and autoimmune disease. According to the Human Microbiome Project, current research has shown that microbiome research holds the possibility to understand and even treat numerous human maladies. Our research focuses on classifying samples of different microbiomes present in different regions of the human body.

In analyzing the differences between microbiomes present on different regions of the human body, we look at the microbes present in the microbiome sample. Since the technology for sequencing DNA and analyzing the microbiome is relatively young, a traditional system of classifying microbes into species doesn’t exist. Thus, the different varieties of microbes are classified into Operational Taxonomic Units (OTUs). Our data examines the specific OTUs found in each of the samples drawn from different regions of the human body.
<h5> Principal Component Analysis (PCA) </h5>
Principal Component Analysis (PCA) is a statistical tool used to reduce the dimensionality of multi-variable data. Because we are unable to visualize data in more than 3 dimensions, PCA must be used to reduce the number of variables/dimensions for effective visualization and classification.

To do this, PCA takes a matrix of size n by m, where n represents the number of samples taken and m represents the number of measurements, and reduces the data into i vectors, or “principal components,” which can be used to project the original data onto new axes.
![Image](/public/img/before.png)
As shown in the picture above, PCA takes a dataset and creates two axes named principal components 1 and 2 so that it presents the data on the left with the most variance as shown on the graph on the right.
<iframe src="https://drive.google.com/file/d/1jg4YdPflTm-SYvvhZLsoEIU6ZVoKJmRL/preview" width="640" height="480"></iframe>
