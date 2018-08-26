---
layout: post
title: Using Principal Component Analysis (PCA) Practice Number Two:Cars Dataset
category: Summer Camp
tag: [COSMOS, Final Project, Camp]
---
After implementing PCA to the famous Iris dataset, I decided that I utilize PCA on one more dataset and then move onto the final project's dataset. So this time, I decided to find my own dataset in which I would analyze using PCA so that I don't have any external sources to refer to. After looking through various websites, I managed to find a very interesting dataset which was relating then different types of cars and trucks with many other variables. I accessed [here](http://ww2.amstat.org/publications/jse/datasets/04cars.dat.txt) to get the actual dataset as I downloaded this text file as an excel and also went [here](http://ww2.amstat.org/publications/jse/datasets/04cars.txt) to get the information of what each column stood for.
![Image](/public/img/variable.png)
![Image](/public/img/cars.png)
As one can see on these two pictures above, the first one shows the description of each the columns' variables and the second picture actually shows a glimpse of the dataset I was going to analyze. So the goal of my PCA was to determine how we can classify the different types of vehicles (sports car, sports utility vehicle, wagon, etc.).
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
    finalDf = pd.concat([principalDf, df[['Type']]], axis = 1)
    finalDf_1 = pd.concat([principalDf, df[['Drive']]], axis = 1)
    for i in range(n):
        for j in range(i+1,n):
            fig = plt.figure(figsize = (8,8))
            ax = fig.add_subplot(1,1,1)
            ax.set_xlabel(stri[i], fontsize = 15)
            ax.set_ylabel(stri[j], fontsize = 15)
            ax.set_title('2 component PCA', fontsize = 20)
            targets = ["Sports Car","Sport Utility Vehicle","Wagon","Minivan","Pickup","Unclassified"]
            colors = ['r', 'g', 'b','c','m','y']
            for target, color in zip(targets,colors):
                indicesToKeep = finalDf['Type'] == target
                ax.scatter(finalDf.loc[indicesToKeep, stri[i]]
                           , finalDf.loc[indicesToKeep, stri[j]]
                           , c = color
                           , s = 50)
            ax.legend(targets)
            ax.grid()
            plt.show()
    for i in range(n):
        for j in range(i+1,n):
            fig = plt.figure(figsize = (8,8))
            ax = fig.add_subplot(1,1,1)
            ax.set_xlabel(stri[i], fontsize = 15)
            ax.set_ylabel(stri[j], fontsize = 15)
            ax.set_title('2 component PCA', fontsize = 20)
            targets = ["All-Wheel Drive","Rear-Wheel Drive","Unclassified"]
            colors = ['r', 'g', 'b']
            for target, color in zip(targets,colors):
                indicesToKeep = finalDf_1['Drive'] == target
                ax.scatter(finalDf_1.loc[indicesToKeep, stri[i]]
                           , finalDf_1.loc[indicesToKeep, stri[j]]
                           , c = color
                           , s = 50)
            ax.legend(targets)
            ax.grid()
            plt.show()
df = pd.read_excel("cars.xlsx",header=None)
df.columns=["Sports Car","Sport Utility Vehicle","Wagon","Minivan","Pickup","All-Wheel Drive","Rear-Wheel Drive","Suggested Retail Price","Dealer Cost","Engine Size","Number of Cylinders","Horsepower","City Miles Per Gallon","Highway Miles Per Gallon","Weight","Wheel Base","Length","Width"]
names = ["Sports Car","Sport Utility Vehicle","Wagon","Minivan","Pickup"]
drive = ["All-Wheel Drive","Rear-Wheel Drive"]
df = df[(df!= "*").all(axis=1)].reset_index(drop=True)
df['Type']="Unclassified"
df['Drive']="Unclassified"
for i in range(len(names)):
    for j in range(387):
        if(df[names[i]][j]==1):
            df['Type'][j]=names[i]

for i in range(len(drive)):
    for j in range(387):
        if(df[drive[i]][j]==1):
            df['Drive'][j]=drive[i]
x=df.iloc[:,0:17].values

analysis(4,df,x)
```
This is the basic code I coded in order to perform an effective PCA comparing each of the first n principal components with one another so that I can observe any clustering of each of the types of vehicles. So now let's take a closer look at each part of the code.
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
df = pd.read_excel("cars.xlsx",header=None)
df.columns=["Sports Car","Sport Utility Vehicle","Wagon","Minivan","Pickup","All-Wheel Drive","Rear-Wheel Drive","Suggested Retail Price","Dealer Cost","Engine Size","Number of Cylinders","Horsepower","City Miles Per Gallon","Highway Miles Per Gallon","Weight","Wheel Base","Length","Width"]
names = ["Sports Car","Sport Utility Vehicle","Wagon","Minivan","Pickup"]
drive = ["All-Wheel Drive","Rear-Wheel Drive"]
```

The next part of the code involves loading and processing the dataset so that I input the appropriate dataframe to the PCA Solver I will later be explaining. To start off, from [here](http://ww2.amstat.org/publications/jse/datasets/04cars.dat.txt), I downloaded the website as a text file in which I imported the text file into an excel sheet. In my case, I named the sheet cars which is why the first line contains cars.xlsx. Also, during this process, I deleted the first column of the original dataset which contained the actual names of each vehicle which I concluded was insignificant to the overall PCA. I also set the header=None because as shown on the pictures before, the dataset's column names aren't really contained in the actual dataset in which I manually inputted the names of columns in the next line.
<p>
The next part of the code contains me creating two lists (names and drive) in which the list 'names' contained the column names of columns 0-4 and the list 'drive' contained the column names of columns 5 and 6. I did this because this way, I could do PCA in two different ways in which the first way was to categorize it based on what type of vehicle it was and the second way being categorizing based on the vehicles' drivetrain.</p>
<h3>Processing the Dataset</h3>
```python
df = df[(df!= "*").all(axis=1)].reset_index(drop=True)
df['Type']="Unclassified"
df['Drive']="Unclassified"
for i in range(len(names)):
    for j in range(387):
        if(df[names[i]][j]==1):
            df['Type'][j]=names[i]

for i in range(len(drive)):
    for j in range(387):
        if(df[drive[i]][j]==1):
            df['Drive'][j]=drive[i]
x=df.iloc[:,0:17].values
```
Moving on, the next part of the code deals with processing the cars dataset.
![Image](/public/img/example.png)
As one goes through the cars dataset, there are various data points in which certain components are marked with the "```*```" as shown in the picture above as this signifies that the data for that certain car wasn't available. Concluding that I would rather get rid of the entire data point, the first line of the code does this function. In that for all rows that contain "```*```", I dropped the entire row and also reset the index. I also created two new columns for the two new lists I created beforehand and assigned a default value of 'Unclassified'. I did this because there were cars that couldn't be classified in either of the ways I specified in which I would create another category called 'Unclassified'.
<p>The next part of the code involves using nested for loops in which I utilized them to assign the values in columns Type and Drive. I did this by first running through all types of cars, and then all the rows. Since the type of car was marked in a form of 0's and 1's in which 1 signaled that it was that specific car/drivetrain. So by using an if statement for such situation, I was able to assigned the values for both of the new lists I created. After running through all the nested for loops, I created the dataframe I would put in my PCA solver in which contained the values of all the changes I made to the original dataset.</p>
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
    finalDf = pd.concat([principalDf, df[['Type']]], axis = 1)
    finalDf_1 = pd.concat([principalDf, df[['Drive']]], axis = 1)
    for i in range(n):
        for j in range(i+1,n):
            fig = plt.figure(figsize = (8,8))
            ax = fig.add_subplot(1,1,1)
            ax.set_xlabel(stri[i], fontsize = 15)
            ax.set_ylabel(stri[j], fontsize = 15)
            ax.set_title('2 component PCA', fontsize = 20)
            targets = ["Sports Car","Sport Utility Vehicle","Wagon","Minivan","Pickup","Unclassified"]
            colors = ['r', 'g', 'b','c','m','y']
            for target, color in zip(targets,colors):
                indicesToKeep = finalDf['Type'] == target
                ax.scatter(finalDf.loc[indicesToKeep, stri[i]]
                           , finalDf.loc[indicesToKeep, stri[j]]
                           , c = color
                           , s = 50)
            ax.legend(targets)
            ax.grid()
            plt.show()
    for i in range(n):
        for j in range(i+1,n):
            fig = plt.figure(figsize = (8,8))
            ax = fig.add_subplot(1,1,1)
            ax.set_xlabel(stri[i], fontsize = 15)
            ax.set_ylabel(stri[j], fontsize = 15)
            ax.set_title('2 component PCA', fontsize = 20)
            targets = ["All-Wheel Drive","Rear-Wheel Drive","Unclassified"]
            colors = ['r', 'g', 'b']
            for target, color in zip(targets,colors):
                indicesToKeep = finalDf_1['Drive'] == target
                ax.scatter(finalDf_1.loc[indicesToKeep, stri[i]]
                           , finalDf_1.loc[indicesToKeep, stri[j]]
                           , c = color
                           , s = 50)
            ax.legend(targets)
            ax.grid()
            plt.show()
```
The PCA Solver Function I created here is similar to that of the one I made for the Iris Dataset. You can refer [here](https://elliotk19.github.io/summer%20camp/2018/07/22/Practice-Using-PCA/) on the explanation of the basic components of the PCA Solver. In terms of the standardizing part, the cars dataset's variables did not really have similar units of measurements which makes standardizing a necessary part of the algorithm. In this specific function, I created two finaldfs in which one was used for categorizing the different vehicles and the other for the different drive trains. By concatenating the necessary columns with each dataframe, I was able to generate the necessary plots that allowed us to effectively categorize each of the vehicles and their specific drivetrains.
<h3> Analyzing the Results </h3>
![Image](/public/img/vehicle.png)
![Image](/public/img/drivetrain.png)
These graphs shown above occurs when we generate the first two principal components of the cars dataset. The top one is for the vehicle type while the second graph is for the specific drivetrain. As shown by the clustering of each of the colors for the first graph, PCA allowed us to conclude that there are relationships amongst the variables as they follow strong general trends. So when an unknown datapoint is given regarding the different variables, with the PCA and the graph it generated we can identify what kind of flower the unknown sample is. However when talking about the PCA for the drivetrain, I did't really notice distinct clustering between the colors. Therefore I concluded that there's not much of a direct relationship between the variables regarding the drivetrains.
