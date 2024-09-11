
# The Wedding Seating Plan Problem and how to solve it.

Let's consider $n$ wedding guests which shall be seated at $m$ tables. $a$ shall define the maximum number of guests a table $m$ can seat. The minimum number of peaople each guest knowx at their table is defined by $b$. Let $C^{ij}$ be the connection matrix indicating the relation of guest $i$ to guest $j$. 

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

In other words we want to maximize the overall sum of connections $C^{ij}$ where guests $i$ and $j$ sit at the same table $k$.

subject to 
```math
\sum^{m}_{k=1} g^{i}_{k} = 1 \;\;\;\;\;\; \forall\;\; 1 \leq i \leq n
```
```math
\sum^{n}_{i=1} g^{i}_{k} = a \;\;\;\;\;\; \forall\;\; 1 \leq k \leq m
```
