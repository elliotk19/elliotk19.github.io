---
layout: post
title: Using Principal Component Analysis (PCA) Practice Number One:Iris Dataset
category: Summer Camp
tag: [COSMOS, Final Project, Camp]
---
Before directly stepping to the dataset of the human microbiomes, our professor recommended implementing PCA on various example datasets one or two times. The first dataset he provided us data of the three different species of the plant <i>Iris</i> : <i>Iris Setosa, Iris Versicolour , </i>and <i>Iris Virginica.</i> More information about the dataset can be found [here](https://archive.ics.uci.edu/ml/datasets/Iris) To basically sum it up, the three classes in the Iris dataset are Iris-setosa (n=50) Iris-versicolor (n=50) Iris-virginica (n=50) and the four features are sepal length(cm), sepal width(cm), petal length(cm), and petal width(cm).
```python
import pandas as pd    
import numpy as np
import matplotlib.pyplot as plt
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
def analysis(n,df,x):
    x = StandardScaler().fit_transform(x)
    pca = PCA(n_components=n)
    stri=[]
    for i in range(n):
        stri.append('principal component ' + str(i+1))
    principalComponents = pca.fit_transform(x)
    principalDf = pd.DataFrame(data = principalComponents
             , columns = stri)
    finalDf = pd.concat([principalDf, df[['class']]], axis = 1)    
    for i in range(n):
        for j in range(i+1,n):
            fig = plt.figure(figsize = (8,8))
            ax = fig.add_subplot(1,1,1)
            ax.set_xlabel(stri[i], fontsize = 15)
            ax.set_ylabel(stri[j], fontsize = 15)
            ax.set_title('2 component PCA', fontsize = 20)
            targets = ['Iris-setosa', 'Iris-versicolor', 'Iris-virginica']
            colors = ['r', 'g', 'b']
            for target, color in zip(targets,colors):
                indicesToKeep = finalDf['class'] == target
                ax.scatter(finalDf.loc[indicesToKeep, stri[i]]
                           , finalDf.loc[indicesToKeep, stri[j]]
                           , c = color
                           , s = 50)
            ax.legend(targets)
            ax.grid()
            plt.show()
df = pd.read_csv(
filepath_or_buffer='https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data',
header=None,
sep=',')
df.columns=['sepal_len', 'sepal_wid', 'petal_len', 'petal_wid', 'class']
df.dropna(how="all", inplace=True)
x=df.iloc[:,0:4].values
y=df.iloc[:,4].values            
analysis(4,df,x)                             
```
This is the basic code I coded in order to perform an effective PCA comparing each of the first n principal components with one another so that I can observe any clustering of each of the Iris petals. So now let's take a closer look at each part of the code.
<h3> Importing Libraries </h3>
```python
import pandas as pd    
import numpy as np
import matplotlib.pyplot as plt
from sklearn.preprocessing import StandardScaler
from sklearn.decomposition import PCA
```
To start the code, I imported the necessary libraries needed to perform the PCA. So I imported 4 main libraries: pandas, numpy, matplotlib, and sklearn. Pandas is mainly used to organize data structures so that one can view certain data in various persepctives. Numpy is mainly used for any type of calculations or math related since it can perform basically any types of mathematical functions. Matplotlib is mainly used to visualize a certain dataset so that one can physically see the results of a certain situation. Finally, I imported the StandardScaler and the PCA from sklearn.decomposition because these are the necessary components of PCA humans cannot physically do by hand yet only computers can handle.
<h3> Loading the Dataset </h3>
```python
df = pd.read_csv(
filepath_or_buffer='https://archive.ics.uci.edu/ml/machine-learning-databases/iris/iris.data',
header=None, sep=',')
```
The next part of the code is actually loading the dataset into a pandas dataframe so that I could easily organize, drop, or add certain rows/columns. First I created a pandas dataframe that read the csv file from the given filepath. One can see two more parameters that are part of the pd.read_csv function which are 'header' and 'sep'. I specifically set the header=None because if you didn't put anything in there, it is set default to header=0 meaning that the first row would automatically set as the header of the dataset. However, one can see from the image below that the specific dataset doesn't really contain a specific row for header so we have to set the header to nothing and later make adjustments to it. The next parameter, sep or separate, as one can also see in the image below that all the data is separated by a comma so by putting sep=',' the computer knows that a comma signifies the next data.
![Image](/public/img/comma.png)
```python
df.columns=['sepal_len', 'sepal_wid', 'petal_len', 'petal_wid', 'class']
df.dropna(how="all", inplace=True)
x=df.iloc[:,0:4].values
y=df.iloc[:,4].values        
```
After loading the data in to a pandas dataframe, since I set the header as None, I have to identify the names of the columns in which I did in line 1. The next line of the code is very important in data science which is dealing with rows/columns that contain an extraneous/erroneous value which has a possibility to mess up the whole process. So by executing the second line, I dropped all the rows that contained no information at all so that all rows will have all parts of the data.
![Image](/public/img/iris.png)
The next two lines allow the array x to have stored the data in a form of a 150×4 matrix where the columns are the different features, and every row represents a separate flower sample. Each sample row x can be pictured as a 4-dimensional vector as shown by the picture above. Since the last column with the class only has string values, it will later be used for plotting purposes.
<h3> Creating the PCA Solver Function </h3>
```python
def analysis(n,df,x):
    x = StandardScaler().fit_transform(x)
    pca = PCA(n_components=n)
    stri=[]
    for i in range(n):
        stri.append('principal component ' + str(i+1))
    principalComponents = pca.fit_transform(x)
    principalDf = pd.DataFrame(data = principalComponents
             , columns = stri)
    finalDf = pd.concat([principalDf, df[['class']]], axis = 1)    
```
The next part of the program involves creating the PCA solver that inputs 3 parameters: n- the number of principal components it will compare, df- the raw dataset, and x-the dataset with only the measurements. The first line of the function utilizes the standardizing function in which dataframe x gets standardized. Whether to standardize the data prior to a PCA on the covariance matrix depends on the measurement scales of the original features. Since PCA yields a feature subspace that maximizes the variance along the axes, it makes sense to standardize the data, especially, if it was measured on different scales. Although, all features in the Iris dataset were measured in centimeters, let us continue with the transformation of the data onto unit scale (mean=0 and variance=1), which is a requirement for the optimal performance of many machine learning algorithms. After standardizing, I used to PCA function by giving the correct number of principal components I was trying to compare. There's also a for loop that runs in the range n which is used for the titling each of the axes when graphing the principal components against one another. So after running through all the PCA functions, I created a finalDf that combined the principalDf that contained the principal components with the list df['class'] or the y dataframe we created at the start so that we know which type of flower each data represents.
<h3> Creating the PCA Solver Function (Graphing part) </h3>
```python
for i in range(n):
    for j in range(i+1,n):
        fig = plt.figure(figsize = (8,8))
        ax = fig.add_subplot(1,1,1)
        ax.set_xlabel(stri[i], fontsize = 15)
        ax.set_ylabel(stri[j], fontsize = 15)
        ax.set_title('2 component PCA', fontsize = 20)
        targets = ['Iris-setosa', 'Iris-versicolor', 'Iris-virginica']
        colors = ['r', 'g', 'b']
        for target, color in zip(targets,colors):
            indicesToKeep = finalDf['class'] == target
            ax.scatter(finalDf.loc[indicesToKeep, stri[i]]
                       , finalDf.loc[indicesToKeep, stri[j]]
                       , c = color
                       , s = 50)
        ax.legend(targets)
        ax.grid()
        plt.show()
```
This set of code is still part of the PCA Solver function but only includes the graphing. Talking about the basic structure, I created a nested for loop so that I would compare each of the principal components with one another so we can see the correlation amongst each of the princpal components. In the for loops, I set the figsize as shown, labelled each of the axes using the stri array that contained the axes titles. I also created two more lists: a targets array that included the names of each flower, and a colors array which included the colors of each of the points on the graph. Making both arrays, I created scatter plot in which plotted each of the principal components with each flower being labelled by a different color.
<h3> Analyzing the Results </h3>
![Image](/public/img/iris.png)
This is the graph when setting n as two as we compare the first two principal components of the Iris dataset. As shown by the clustering of each of the colors(or the flowers) PCA allowed us to conclude that there are relationships amongst the variables as they follow strong general trends. So when an unknown datapoint is given regarding the different variables, with the PCA and the graph it generated we can identify what kind of flower the unkown sample is. 
