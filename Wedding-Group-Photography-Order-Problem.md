# The Wedding Group Photography Order Problem and how to solve it. 

Your own wedding - one of the most beautiful days in your life, and we all want to keep this moment forever, remember the moments with our guests but also as a couple. Thus it is highly relevant to have a great photographer that is able to capture all these moments.

But besides all the nice and spontaneous shots of laughing and talking guests, in the end everybody expects the perfectly staged group and family photos in each and every combination - with grandparents, without them, with uncles and cousins, without them, ...

... and that takes it's time. 
And after some while we just want to continue with the celebration but still some combinations are missing.

![photo_2024-09-12 10 24 05](https://github.com/user-attachments/assets/c860085f-c869-46c8-b5b0-845687d309fb)

Doesn't that already sound like a perfectly defined combinatorial optimization problem? It is - with a clear objective: minimizing the overall time of taking group photos by choosing an order that allows the smallest amount of guest exchanges.

And very important, doing all that subject to not missing out any relevant photo combination of guests with the bride and groom.

Just to give you a small introduction to the complexity of this problem, think of around 100 guests and assume about 25 different photo combinations. In such a case there about $\frac{25(25-1)}{2} = 300$ possible orders how to take the photos. - thats a lot.

### The Art of orderung Group Photos :camera:
First of all we need the relevant data to take the ordering decision. Let's start with a list of needed Photocombinations and the information about the needed guests for that photo.

Let $P^{k}_{i}$ define the information that guest $k$ of $m$ guests is part of the Photo $i$ of $n$ photos:
```math
P^{k}_{j} = 
\begin{cases}
    1 & \text{if guest } k \text{ is part of the photo } i \\
    0 & \text{otherwise}
\end{cases}
```

Example input data matrix:


| Guests:| 1 | 2 | 3 | 4 | 5 | 6 | ... |
|--------|---|---|---|---|---|---|-----|
| Photo1 | 1 | 1 | 1 | 0 | 0 | 0 |     |
| Photo2 | 0 | 0 | 0 | 1 | 1 | 1 |     |
| Photo3 | 0 | 1 | 1 | 1 | 1 | 0 |     |
| ...    |   |   |   |   |   |   |     |

Based on that input data we can derive a transition graph from one photo to the next photo that defines of many changes are necessary in guests. And here we are... it's a classical graph problem.

It's rather easy to model the Wedding Group Photography Order Problem as a nice shortest path optimization problem where we want to find the shortest path to navigate the graph of possible photo combinations with the number of changes in guests to get from one photo to the next one.

![Photo Order](https://github.com/user-attachments/assets/f6140906-724f-4437-a44f-7ea49026d20a)

As we can see for our example above the order of photos with the least number of changes in guests is defined along the edges with the lowest numbers in the graph.

### :star2: Let mathematical optimization help us with solving this problem for larger instances:

Let $c_{ij}$ define the number of changes in guests to get from the photo number $i$ to the photo number $j$ of $n$ photos defined in the Distance-Matrix $C_{n\times n}$.
The matrix of changes can be derived from the information matrix $P^{k}_{i}$.

The optimization Problem can then be formulated using the Decision Variable $x_{ij}$ that defines the order of photos:
```math
x_{ij} = 
\begin{cases}
    1 & \text{if photo } j \text{ is taken directly after photo } i \\
    0 & \text{otherwise}
\end{cases}
```

The objective of our shortest path problem is to minimize:
```math
\sum^{n}_{i=1}\sum^{n}_{j=1, j\neq i} c_{ij} x_{ij}
```

subject to 
```math
\sum^{n}_{j=1, j\neq i} x_{ij} = 1 \;\;\;\;\;\; \forall\;\; 1 \leq i \leq n
```
```math
\sum^{n}_{i=1, i\neq j} x_{ij} = 1 \;\;\;\;\;\; \forall\;\; 1 \leq j \leq n
```

In english words we just want to ensure that every photo is taken exactly ones.

Further we need a dummy variable $y_{i}$ for each photo $i$ of $n$ photos that defines the order in which photos are taken, counting from photo 1. For all photos $i$ and $j$ it is defined that $u_i < u_j$ which implies that photo $i$ is taken before photo $j$.

Because linear programming favors non-strict inequalities ($\geq$ ) over strict (>), we adapt the constraint formulation to the following:

```math
u_i - u_j + 1 \leq (n-1)(1-x_{x_ij}) \;\;\;\;\;\; \forall\;\; 2 \leq i \neq j \leq n
```
```math
2 \leq u_i \leq n \;\;\;\;\;\; \forall\;\; 2 \leq i \leq n
```

Mathematical Explanation: 
The way that the $u_{i}$ variables then enforce that a single photo order includes all photos is that they increase by at least 1 for each step along an order, with a decrease only allowed where the photo order passes photo number 1. That constraint would be violated by every order which does not include photo 1, so the only way to satisfy it is that the photo order including photo 1 also includes all other photos.

For normal people:
In other words it is only allowed to have one photo order that include all photo combinations because it is not possible to take to seperate photos with bride and groom at the same time. 
