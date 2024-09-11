
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


