
# The Wedding Seating Plan Problem and how to solve it.
For most of us, marrying the love of our lives is the most beautiful moment in our lives and brings with it a lot of emotions in addition to all the joy and happiness. Deciding on the wedding date and location is often one of the first achievements and takes a lot of pressure off during the wedding planning process. Nevertheless, when talking about the location, this could still cause a lot of trouble when thinking about how to seat you lovelly guests at the wedding later.

Thankfully a lot of great researchers and mathematitions married before me and had the same intuition that math could help us with this crazy problem of how to seat your guests. Let me introduce you to the common and absolutely life-relevant "Wedding Seating Plan Problem".

This little post shall give you an idea on how to use mathematical optimization to support your personal journey to a great wedding seating plan - our how it is called by [Borja Menéndez]([https://www.genome.gov/](https://feasible.substack.com/p/8-i-just-got-married-and-i-had-to) the "the Delicate Art of Seating Guests".

Let's consider $n$ wedding guests which shall be seated at $m$ tables. $a$ shall define the maximum number of guests a table $m$ can seat. The minimum number of peaople each guest knows at their table is defined by $b$. Let $C^{ij}$ be the connection matrix indicating the relation of guest $i$ to guest $j$ from number 0 (not knowing each other) to 50 (shall be seated together). 

Decision Variables:
```math
g^{i}_{k} = 
\begin{cases}
    1 & \text{if guest } i \text{ is seated at table } k \\
    0 & \text{otherwise}
\end{cases}
```


The objective is to maximize:
```math
\sum^{m}_{k=1}\sum^{n-1}_{i=1}\sum^{n}_{j=i+1} C^{ij} g^{i}_{k} g^{j}_{k}
```

In other words we want to maximize the overall sum of connections $C^{ij}$ where guests $i$ and $j$ sit at the same table $k$ so that all guests know as many other guests at their own table.

subject to 
```math
\sum^{m}_{k=1} g^{i}_{k} = 1 \;\;\;\;\;\; \forall\;\; 1 \leq i \leq n
```
```math
\sum^{n}_{i=1} g^{i}_{k} = a \;\;\;\;\;\; \forall\;\; 1 \leq k \leq m
```
```math
\sum^{n}_{i=1} g^{i}_{k} g^{j}_{k} \geq (b+1)g^{j}_{k} \;\;\;\;\;\; \forall\;\;  1 \leq j \leq n, \;\; 1 \leq k \leq m, \;\; C^{ij} > 0
```


Helping Constraints to linearize the model:
```math
\sum^{n}_{i=1} g^{i}_{k} g^{j}_{k} \leq a g^{j}_{k}, \;\;\;\;\;\; \forall\;\;  1 \leq j \leq n, \;\; 1 \leq k \leq m
```
```math
\sum^{n}_{j=1} g^{i}_{k} g^{j}_{k} \leq a g^{i}_{k}, \;\;\;\;\;\; \forall\;\;  1 \leq i \leq n, \;\; 1 \leq k \leq m
```

Value Range Constraint:
```math
g^{i}_{k}, g^{j}_{k} \in \{0, 1\}
```

We want to maximize these connections at all tables under consideration that every guest can only sit at one table and each table can only have at max $a$ guests. Each guest must know at minimum $b$ people at the table. 
