---
layout: post
title: PIG Game Strategy NumberOne:Limiting the Number of Rolls
category: Summer Camp
tag: [COSMOS, Simulation, Camp, PIG Game]
---
Since the goal of the PIG Game is to get the most points possible until a 100 before one rolls a 1. So when one makes a strategy regarding the number of rolls, he/she has to be reasonable in that he/she has to approximate the probability of rolling a 1 every time they roll the die. For example, the probability of rolling a 1 in the player's first turn is 1/6 and the probability increases every subsequent rolls therefore the player has to be careful.

```python
import numpy as np
import matplotlib.pyplot as plt
def computer_roll(n):
    turn=0
    for i in range(n):
        computer_dice = np.random.randint(1,7)
        if(computer_dice is not 1):
            turn+=computer_dice
        elif(computer_dice==1):
            turn=0
            return 0
    return turn
def computer1_roll(n):
    turn=0
    for i in range(n):
        computer_dice = np.random.randint(1,7)
        if(computer_dice is not 1):
            turn+=computer_dice
        elif(computer_dice==1):
            turn=0
            return 0    
    return turn
def play(n,c,c1):   
    computer1_win=0
    computer_win=0
    for i in range(n):      
        computer_total=0
        computer1_total=0
        while(computer1_total<=100 and computer_total<=100):       
            computer_total+=computer_roll(c)
            computer1_total+=computer1_roll(c1)
        if(computer_total>=100):
            computer_win+=1
        elif(computer1_total>=100):
            computer1_win+=1
    return computer_win/float(n), computer1_win/float(n)
x=[]
y=[]
for i in range(1,21):
    x=[]
    for j in range(1,21):
        a,b= play(100,i,j)
        x.append(b)
    y.append(x)
t=range(1,21)
plt.figure()
plt.imshow(y)
plt.xticks(np.arange(min(t), max(t)+1, 1.0))
plt.yticks(np.arange(min(t), max(t)+1, 1.0))
plt.colorbar()
plt.show()
```
This is my full Python code that allows two computers to simulate a certain number of trials (in this case we can change the '100' in line 49 to the number of trials one wants to simulate) of the PIG Game. Specifically, this simulates 40,000 trials (20x20x100) in that each computer has limits its roll from 1 roll to 20 rolls and simulates 100 trials for each of the situations. So let's take a closer look at each section of the code.

<header>
  <h3>Importing Libraries </h3>
</header>
```python
import numpy as np
import matplotlib.pyplot as pyplot
```
The first part of code imports the necessary libraries needed to simulate the PIG Game. So the two libraries I imported was numpy and matplotlib in which I used numpy for the basic calculations and matplotlib in order to visualize my results into a nice plot.
<header>
  <h3>Creating Functions for Simulations </h3>
</header>
```python
def computer_roll(n):
    turn=0
    for i in range(n):
        computer_dice = np.random.randint(1,7)        
        if(computer_dice is not 1):
            turn+=computer_dice
        elif(computer_dice==1):
            turn=0
            return 0
    return turn
def computer1_roll(n):
    turn=0
    for i in range(n):
        computer_dice = np.random.randint(1,7)    
        if(computer_dice is not 1):
            turn+=computer_dice
        elif(computer_dice==1):
            turn=0
            return 0    
    return turn
```
The next two parts of the code are two functions I made called computer_roll and computer1_roll which are the same but just different function names. By taking the parameter(n) of the number of rolls the user is going to use roll before ending his/her turn, it runs a for loop that runs 'n' times to simulate the dice rolls. In the for loop, I made it so that each computer inputs a random integer between 1 and 6 (as shown in lines 4 and 14), thus simulating a dice roll. Since the game of PIG says that whenever a user rolls the number 1 he/she scores no points, I also inputted an if and elif statement regarding that case as shown in lines 5-9 and 15-19. So after the for loop runs n amount of times, this function returns the total score of the user.
<header>
  <h3>Creating Functions for Overall Game </h3>
</header>
```python
def play(n,c,c1):   
    computer1_win=0
    computer_win=0
    for i in range(n):      
        computer_total=0
        computer1_total=0
        while(computer1_total<=100 and computer_total<=100):       
            computer_total+=computer_roll(c)
            computer1_total+=computer1_roll(c1)            
        if(computer_total>=100):
            computer_win+=1
        elif(computer1_total>=100):
            computer1_win+=1
    return computer_win/float(n), computer1_win/float(n)
  ```
  The next part of the code is a function I made that simulates the overall game. It inputs three parameters (n: Number of simulations for each situation, c: number of rolls computer will roll before ending turn, c1: number of rolls computer_1 will roll before ending turn) in which are used to generate the win rate for each computer. How the function works is that it runs a big for loop to generate the number of simulations inputted and in it are two variables that reset every simulation which are the total points of each computer. Then I put a while loop that runs the two dice roll functions until one of the computers reach a total score above 100 which will then cause the while loop to end. This will lead to the if and elif statements in which separates the two cases of computer winning or computer_1 winning as it adds the total number of wins each computer has. As the big for loop runs n number of times, the play function will return the win rate of each computer for each number of rolls each computer rolls before ending its turn.
<header>
  <h3>Gathering Data and Plotting </h3>
</header>
```python
x=[]
y=[]
for i in range(1,21):#y-axis, computer
    x=[]
    for j in range(1,21):#x-axis, computer 1
        a,b= play(100,i,j)
        x.append(b)
    y.append(x)
t=range(1,21)
plt.figure()
plt.imshow(y)
plt.xticks(np.arange(min(t), max(t)+1, 1.0))
plt.yticks(np.arange(min(t), max(t)+1, 1.0))
plt.colorbar()
plt.show()
```
The final part of the code is using all the functions created before and using them. By employing a nested for loop to gather all the data, I created two arrays (x and y) in which x appends the win rates of computer 1 for every number of rolls they roll before ending its turn. Then y appends all the values in the list x, which will then go back in the nested for loop as the code will then run again for each roll number for computer. This will thus result in generating 100 win rates for each case (20x20) in which y array appends it.
![Image](/public/img/result.png)
Now the concluding parts of the code is for the visualization of the data in which I used the matplotlib library to depict the win rates of each case. To read this plot, we have to look at what the color bar on the right represents in which shows the win rates ranging from 0-1.(where 0 means 0% winning and 1 meaning 100% win rate) Though the plots will vary every time we run the code since we are using random number generator, in this specific case I can conclude that when computer_1 rolls 5 times and stops has the highest win rate for computer_1. This can be justified by the lightness of the plot when x=5 signifying that the win rates are generally high.
