---
layout: post
title: Using Principal Component Analysis (PCA) Final(Methods, Code, Analysis/Conclusion, and Future Directions):The Human Microbiome Dataset
category: Summer Camp
tag: [COSMOS, Final Project, Camp]
---
After doing all the research needed for the project, our group now proceeded on the coding part of the project. Unlike the past two datasets I analyzed using PCA, for this specific dataset, I conducted a PCA within a PCA so that I can classify the specific regions from its general locations. So now let's look into the code in which I classified the code in terms of its purpose.
<h2>Creating the Base PCA Solver </h2>
<h5>Importing Libraries </h5>
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
```
To start the code, I imported the necessary libraries needed to perform the PCA. So I imported 4 main libraries: pandas, numpy, matplotlib, and sklearn. Pandas is mainly used to organize data structures so that one can view certain data in various persepctives. Numpy is mainly used for any type of calculations or math related since it can perform basically any types of mathematical functions. Matplotlib is mainly used to visualize a certain dataset so that one can physically see the results of a certain situation. Finally, I imported the StandardScaler and the PCA from sklearn.decomposition because these are the necessary components of PCA humans cannot physically do by hand yet only computers can handle.
<h5>Loading and Processing the Dataset </h5>
```python
dataF = pd.read_excel('otu_table_psn_v13_annotated_total.xlsx',header=None)
dataF = dataF.T
features = dataF.iloc[1:-2,5:].values
labels = dataF.iloc[1:-2,1].values
regionlabel = dataF.iloc[1:-2,3].values
specificregion = dataF.iloc[1:-2,2].values
```
In this part of the code, I created a pandas dataframe by loading the data as an excel file. In the parameters of the read_excel function, it includes the parameter header in which equalled None.
![Image](/public/img/microexcel.png)
The reason I set the header to None is because on the next line, I processed my dataframe so that it becomes the transpose of itself which basically means that the x and y axis switched. As shown in the picture above, taking the transpose of the dataset would follow our information section in that the independent variable would become the gender and the region the sample was taken and the dependent variable being different OTUs. Next, I split the dataframe to 4 different sub-dataframes called features, labels, regionlabel, and specificregion and the data contained in each of the dataframes are self-explanatory by their names.
<h5>PCA Solver </h5>
```python
numEvec=8
def PCASolver(X, numEvec):
    pca = PCA(n_components= numEvec)
    Y = pca.fit_transform(X)
    return Y, pca.explained_variance_ratio_
Y, all_eigvalues = PCASolver(features, numEvec)
```
So the next part of the code deals with creating the base PCA Solver in which takes in the features dataframe and the number of principal components (numEvec) it will analyze. As this function runs, it runs PCA from the library we imported and also standardizes the matrix into the dataframe Y. Thus this specific function outputs the standardized dataframe Y and a list with all the explained varaince ratio of each principal component. What the explained variance can tell you about a principal component is that it shows what percent of the variance of the data each principal component explains.
<h3>Creating the Variance Plot </h3>
```python
numPCs = numEvec
percent = [i*100 for i in sorted(all_eigvalues, reverse=True)]
cum_var_exp = np.cumsum(percent[:numPCs])
plt.plot(np.arange(1,numPCs+1),cum_var_exp,color='r',marker='s')
plt.bar(np.arange(1,numPCs+1),percent)
plt.legend(['Cumulative Explained Variance','Eigenvalue'])
plt.title('Explained Variance by Different Principle Components')
plt.ylim([0,100])
plt.show()
```
As we ran the PCA solver with the raw dataset, we were able to generate a list called all_eigvalues with the explained variance ratio of each of the eigenvalues. So the next thing I did was to create another list called percent in which multiplied the ratio by 100 so that we can get the percentage as I ordered the percentages in descending order. By organizing the dataset from greatest to least, it would therefore categorize the principal components from the most important to the least important.
![Image](/public/img/explainedvariance.png)
This is the plot we get after running all the commands using the matplotlib library. Basically I created a plot and inside it created bar graphs that signified the explained variance in percent of each of the principal components from the most significant to the least. Also on the plot is a line graph that shows the cumulative explained variance in which shows the sum of the explained variance from pc1~pc(n). Normally, when using PCA and dealing with explained variance, data scientists would use the number of principal components that takes to explain at least 70% of the data. In this case, as shown by the graph the first four principal components represents approximately 70% of the data and we chose to analyze these four principal components.
<h3>PCA Amongst the General Regions </h3>
```python
n = 4
stri=[]
for i in range(n):
    stri.append('PC' + str(i+1))    
for i in range(n):
    for j in range(i+1,n):        
        fig = plt.figure(figsize = (8,8))
        ax = fig.add_subplot(1,1,1)
        ax.set_xlabel(stri[i], fontsize = 15)
        ax.set_ylabel(stri[j], fontsize = 15)
        ax.set_title('2 component PCA', fontsize = 20)        
        targets = ['Urogenital_tract', 'Airways', 'Skin', 'Oral', 'Gastrointestinal_tract']
        colors = ['r', 'g', 'b','c','m']        
        for target, color in zip(targets,colors):
            indicesToKeep = regionlabel == target
            ax.scatter(Y[indicesToKeep,i]
                       , Y[indicesToKeep,j]
                       , c = color
                       , s = 50)
        ax.legend(targets)
        ax.grid()
        plt.show()
```
The next part of the code deals with the PCA for the general regions of where the samples were taken which includes the Urogenital tract, Airways, skin, the oral region, and the Gastrointestinal tract. Similar to the PCA Solver created for the past two datasets, the function takes the first n principal components it will try to compare with one another. First I created a separate list for the axes names called stri in which the values changed based on which principal component it was linked with as I was able to plot the graphs.
![Image](/public/img/pc1.png)
![Image](/public/img/pc3.png)
As shown by these two graphs of comparing the first four principal components, different regions contain points that are located on a certain line in the PCA graph. Therefore, points that lie on one of the “spokes” of the graph can be classified with a high degree of certainty.
<h3>PCA Amongst the Specific Regions </h3>
```python
for target in targets:
    Y, all_eigvalues = PCASolver(features, numEvec)
    indicesToKeep = regionlabel == target
    Y = Y[indicesToKeep][:]
    shortenedspecific = specificregion[indicesToKeep][:]    
    unique = []
    for x in specificregion[indicesToKeep]:
        if x not in unique:
            unique.append(x)            
    fig = plt.figure(figsize = (8,8))
    plt.xlabel("PC1", fontsize = 15)
    plt.ylabel("PC2", fontsize = 15)
    plt.title(target+ " PCA", fontsize = 20)    
    for specific, color in zip(unique, colors):        
        indices = shortenedspecific == specific
        plt.scatter(Y[indices,0], Y[indices,1], c = color, s = 50)
    plt.legend(unique)
    plt.grid()
    plt.show()
```
Now in order to conduct a better PCA, our group decided to conduct PCA within each of the general regions since the dataset also provided the specific regions the samples were taken from. So by taking the regionlabel and specificregion list we created earlier, we were able to conduct PCA amongst each of the general locations. By creating two more lists, shortenedspecific and unique, I was able to categorize the dataset into which general location it came from so that the graphs would generate the principal components of the specific locations.
![Image](/public/img/specific.png)
As shown by the 5 graphs of each general location, there is very little horizontal variation along PC1 for samples taken from the Urogenital Tract region. Conversely, skin samples experience only slight vertical dispersal and vary almost exclusively in terms of PC1. Samples from the gastrointestinal region are clustered very tightly in the convergence of Oral, Urogenital Tract, and Skin samples. Airways samples nearly mirror those from Skin, but with less density/variation.
<h3> Future Directions </h3>
Future research might include applying our PCA algorithm to other sample sets from microbiomes found in the soil and ocean. Also, a more comprehensive dataset on the microbiome samples would allow researchers to recognize more patterns and clusters in the PCA graphs based on unexamined variables, such as age, location, and health. By further examining microbiomes, it could be possible to identify the different properties of the microbes and help cure/prevent certain illnesses.
<h3> Conclusion </h3>
As we predicted correctly in the hypothesis, communities of bacteria on different organs are very distinct. PCA showed us that samples from different regions varied directly to a their corresponding principal components; each region showed correlation to their specific OTUs. Our PCA solver ran smoothly, and we performed it on several reduced versions before plugging in the total dataset. We were initially confused about our graphs because unlike most PCA examples, our data did not group together in clusters. However, it makes sense because there is a linear variation of OTUs in specific areas. Some future research would include using our PCA solver on other sample sets from the Human Microbiome Project or from the soil and ocean microbiomes.
<h3> Works Cited </h3>
About the Human Microbiome. (2018). Retrieved from https://hmpdacc.org/hmp/overview

Klotter, J. (2010, July). NIH launches Human Microbiome Project. Townsend Letter, (324), 18. Retrieved from http://link.galegroup.com/apps/doc/A231544962/SUIC?u=pleasant_valhs&sid=SUIC&xid=c4109b64

Morgan XC, Huttenhower C (2012) Chapter 12: Human Microbiome Analysis. PLoS Comput Biol 8(12): e1002808. https://doi.org/10.1371/journal.pcbi.1002808

Savage DC. Microbial ecology of the gastrointestinal tract. Annu. Rev. Microbiol. 1977;31:107–133.
