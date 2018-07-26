---
layout: post
title: PIG Game Strategy Number Two:Stop at Certain Value per Turn
category: Summer Camp
tag: [COSMOS, Simulation, Camp, PIG Game]
---
Now let's move on to the second strategy my class discussed regarding the PIG Game. The first strategy I discussed earlier was the user ending his/her turn at a certain number of rolls. Now I would like to show how I coded the second strategy which was for the user ending his/her turn on a certain point value. For example if the strategy is stop until scoring 10 points in the turn, the user keeps rolling until the cumulative score for the turn is 10 or more then stops.
```python
import numpy as np
import matplotlib.pyplot as plt
def computer_roll(n):
    turn=0
    while(turn<=n):
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
for i in range(1,31): #y-axis, computer
    x=[]
    for j in range(1,31): #x-axis, computer 1
        a,b= play(100,i,j)
        x.append(b)
    y.append(x)
t=range(1,31)
plt.figure()
plt.imshow(y)
plt.xticks(np.arange(min(t), max(t)+1, 1.0))
plt.yticks(np.arange(min(t), max(t)+1, 1.0))
plt.colorbar()
plt.show()
```
This is my full Python code that does same functions but just coded with different strategies. For this specific code, it also runs 90,000 trials(30x30x100) in that each computer limits the number of points from 1 to 30 and runs 100 simulations for each situation. Just like the previous strategy, let's take a closer look at each section of the code.
<header>
  <h3>Importing Libraries </h3>
</header>
```python
import numpy as np
import matplotlib.pyplot as pyplot
```
The first part of code is the same as the previous one in that it also imports numpy and matplotlib. Just like the first strategy, I used numpy for calculations purposes and matplotlib in order to visualize my data better.
<header>
  <h3>Creating Functions for Simulations </h3>
</header>
```python
def computer_roll(n):
    turn=0
    while(turn<=n):
        computer_dice = np.random.randint(1,7)
        if(computer_dice is not 1):
            turn+=computer_dice
        elif(computer_dice==1):
            turn=0
            return 0    
    return turn
def computer1_roll(n):
    turn=0
    while(turn<=n):
        computer_dice = np.random.randint(1,7)
        if(computer_dice is not 1):
            turn+=computer_dice
        elif(computer_dice==1):
            turn=0
            return 0    
    return turn
```
The next two parts of the code are two functions that simulates each turn of the two computers. I clearly defined each user's turn by creating two functions called computer_roll and computer1_roll which have the same roll but just for each user. Inside each function, I inputted a parameter 'n' in which for the number of points scoring in each turn before ending the turn. After getting the value, the function runs a while loop with the condition of the turn's cumulative score being less than the parameter 'n' being the specified score. In the loop, it inputs a random integer between 1 and 6 like a normal die using numpy.random.randint. Then it goes through an if/else statement regarding the situation when the user rolls a 1 as that results in the user's cumulative turn score becoming 0 and being forced to end the user's turn. After going through all the statements, the function returns the turn's cumulative target score.
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
The next part of the code simulates the overall PIG Game. It inputs three parameters (n: Number of simulations for each situation, c: number of points computer will try to score before ending turn, c1: number of points computer_1 will try to score before ending turn) in order to generate the win rate of each computer. The play function for this strategy is the exact same as the play function for the first strategy because both functions call the necessary functions and keeps track of the total points for each user. Therefore, there wouldn't be much difference in each of the codes and the function returning the two win rates of each user.
<header>
  <h3>Gathering Data and Plotting </h3>
</header>
```python
x=[]
y=[]
for i in range(1,31): #y-axis, computer
    x=[]
    for j in range(1,31): #x-axis, computer 1
        a,b= play(100,i,j)
        x.append(b)
    y.append(x)
t=range(1,31)
plt.figure()
plt.imshow(y)
plt.xticks(np.arange(min(t), max(t)+1, 1.0))
plt.yticks(np.arange(min(t), max(t)+1, 1.0))
plt.colorbar()
plt.show()
```
The final parts of the code iterates the play function so that the x array appends the win rates of computer 1 for every value they try to reach from 1-30. Then the y array appends all the values in the list x, which will then go back in the nested for loop as the code will then run again for each value the users try to score before ending his/her turn.
![Image](/public/img/imshow.png)
Now the concluding parts of the code is for the visualization of the data in which I used the matplotlib library to depict the win rates of each case. To read this plot, we have to look at what the color bar on the right represents in which shows the win rates ranging from 0-1.(where 0 means 0% winning and 1 meaning 100% win rate) Though the plots will vary every time we run the code since we are using random number generator, in this specific case I can conclude that when computer 1 has a highest win rate when he/she ends the turn after scoring 23 points because the column with computer 1 stopping at 23 points is overall the lightest.
