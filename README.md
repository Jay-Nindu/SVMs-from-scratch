# SVMs-from-scratch
Support vector machines are a staple of ML classification algorithms. This project aims to implement SVMs from scratch, in c++, and to detail the theory behind them.

## The aim
A support vector machine aims to create a hyperplane (defined b the equation $wx + b = 0$) that separates two sets of data into two labels (+1 or -1) with as large a margin as possible between the two data sets. We normalise the hyperplane based on the closest points of the hyperplane, so that each of $|wx+b| = 1$ In summary:
- $y_i = +1 \implies wx + b > 1$
- similarly $y_i = -1 \implies wx + b < -1$.

These equations can be combined as: $y_i(wx_i+b) > 1, \forall i$. Or for ease in future equations: $y_i(wx_i+b) - 1 > 0, \forall i$

We can calculate the distance (euclidean norm) of the closest data point $x_n$ to the hyperplanem by taking any point on the hyperplane $x$ and projecting the vector between these points ($x_n - x$) onto a vector orthogonal with our hyperplane (i.e. the normalised vector of $w$ which defines our hyperplane). Note that for steps 1-3 $b$ (threshold) has been absorbed into $w$
- $\text{Normalised W} = \frac{w}{|w|}$
- $\text{projection} = (\frac{w}{|w|})^T \cdot (x_n-x)$
- $\text{Distance} = |(\frac{w}{|w|})^T \cdot (x_n-x)| = \frac{1}{|w|} \cdot |w^T \cdot x_n - w^T \cdot x|$
- $\text{Distance} = \frac{1}{|w|} \cdot |(w^T \cdot x_n + b) - (w^T \cdot x - b)|$ ($x$ is on the plane: $wx+b = 0$, and $wx_n + b = 1$)
- $\text{Distance} = \frac{1}{|w|} \cdot |1 + 0| = \frac{1}{|w|} $

This derivation is taken from [1]. We must maximise the above margin, which is dependant on minimising $|w| = w^{T}w$.
Thus we must solve the following eqn:
```math
\begin{equation}
\min_{w, b \text{ subject to } y_i(wx_i+b) -1 \geq 0 \text{, } \forall i} \frac{1}{2} w^{T}w
\end{equation}
```
Note: I have introduced $\frac{1}{2}$ for ease later on (as many do), it does not change the objective function.

To deal with the constraints upon this optimisation, we directly include the constraints in our equation with lagrangian multipliers. 
```math
\begin{equation}
\min_{w,b \text{ subject to } \alpha_i \geq 0 \text{, } \forall i} \frac{1}{2} w^{T}w - \sum_{i=1}^N \langle \alpha_i,  y_i(wx_i + b - 1) \rangle
\end{equation}
```
The above equation is derived from the idea that at the minima that satisfies all constraints ($g(x)_i$, there exists lagrangian multipliers $a_i$, so that:
- $f(x) = \langle \alpha_i \text{,    } g(x)_i \rangle$ (the constrain eqn's intersect with f(x))
- $\nabla f(x) = \nabla \langle \alpha_i \text{,    } g(x)_i \rangle$ (the constrain eqn's are tangential to f(x)


Now, to solve this equation, we set its gradient to 0. Setting the derivative wrt w to 0 results in the constraint:
```math
\begin{equation}
w - \sum_{i=1}^N \langle \alpha_i, y_{i}x_i \rangle = 0
\end{equation}
```

Setting the derivative wrt b to 0 results in the constraint:
```math
\begin{equation}
\sum_{i=1}^N \langle \alpha_i, y_{i} \rangle = 0
\end{equation}
```

Note that the inner product $\langle a, b \rangle$ for the linear case is simply the dot product between the vectors $a$ and $b$.

## The dual problem

## Non-linearly separable data
  
## Solving for $\alpha$ with sequential minimal optimisation


## References:
1. Caltech machine learning lectures (CS 156).
2. Article on sequential minimal optimisation: https://www.microsoft.com/en-us/research/publication/sequential-minimal-optimization-a-fast-algorithm-for-training-support-vector-machines/
3. Various resources on math (Lagrangian multipliers, etc.)
   
