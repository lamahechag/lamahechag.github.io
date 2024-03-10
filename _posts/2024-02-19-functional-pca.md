---
layout: post
title: Principal Component Analysis for functional data
date: 2024-01-19
datatable: true
published: true
---

We usually use PCA for dimensionality reduction or to find the most important
component of variation within a dataset. We use to do it with data-points, but real data are not just points, are curves(time series), are surfaces(images). Here is where functional analysis brings a general approach to find the 
principal components and reduce the data.


PCA solves a maximization problem, whose result is a set of orthonormal components or basis. Orthonormality and basis can be defined more general than 
our dimension intuition of directions and vectors. For example, modes of oscillation can represent basis in the case of string vibrations. This general definition of PCA is stated by the Karhunen-Loéve expansion\[[^fn2]\]  \[[^fn3]\]:

$$ x_{i} = \bar{x} + \sum_{j=1}^{\infty}f_{ij}\xi_{j}.$$

Here $x_{i}$ can be a point or function; because our dataset can be compose of points, functions or both. $\bar{x}$ is the mean of the dataset. $\xi_{j}$ are the components or basis, and $f_{ij}$ are the scores for the given data expansion. Lets do it for the classical example: a cloud of points in the xy plane. In the next draw I point each of the symbols of the expansion.

<div align="center">
    <img src="{{site.baseurl}}/images/cloud_components.png">
</div>

Now lets do the same, but for a dataset of curves $x_{i}(t)$. 

$$ x_{i}(t) = \bar{x}(t) + \sum_{j=1}^{\infty}f_{ij}\xi_{j}(t).$$

<div align="center">
    <img src="{{site.baseurl}}/images/curves_components.png">
</div>

Real data are not continuous functions, so each curve is a series of points in time. Then this dataset of curves can be represented by the matrix $\boldsymbol{X}$, where the horizontal index represents time steps.

# Numerical method for functional PCA

To find the principal components, first I should introduce a couple of Functional Analysis concepts. In this branch of Mathematics there are functions and operators. Operators are objects that modify functions, the same way as functions modify numbers [^fn1]. For this interaction between operators and functions we can write an eigenvalue equation.

$$ V \xi = \rho \xi $$

where $V$ is called the covariance operator, $\rho$ is the eigenvalue, and $\xi$ is the eigenfunction. The way
$V$ operates on $\xi$ is as follow:

$$ V\xi = \int v(s, t)\xi(t)\,dt.$$

In the case of the functional eigenvalue problem, there is an infinity number of eigenvalues and eigenfunctions, which is accord with the Karhunen-Loéve expansion.

Writing this as a summation approximation for the discretized functions:

$$ \rho \xi(s_{j}) = \int v(s_{j}, s)\xi(s)\,dt \approx \Delta t \sum_{i=1}^{n} v(s_{j}, t_{i})\xi(s_{i})$$


Defining the matrix of centered functional data as

$$\boldsymbol{X}_{c}=\boldsymbol{X} - \bar{x},$$

and $\mathbf{K}_{XX}$ as its covariance matrix. We can write


$$ \mathbf{K}_{XX}\boldsymbol{u} = \lambda\boldsymbol{u}.$$

Here $\lambda = \frac{\rho}{\Delta t}$. And once the autovectors ,of $\mathbf{K}_{XX}$, are found, they should be transformed to the functional approximation form

$$\boldsymbol{\xi} = \Delta t^{-1/2}\mathbf{u}$$

This comes from the definition of inner product in functional analysis

$$ \int \xi_{i}(t) \xi_{j}(t)\,dt$$

Where the norm of $\xi(t)$ is 1, and then:

$$ \int \xi_{i}^{2}(t)\,dt \approx \Delta t \sum_{i=1}^{n} \xi^{2}(s_{i}) = 1$$

# Numerical method for multivariate functional PCA

In the multivariate case we calculate the join covariance between the variables. For this purpose, we need a block matrix of the variables curves.

Given the functional variables data $\mathbf{X}$, $\mathbf{Y}$ and $\mathbf{Z}$, we can create the block matrix
$$ \mathbf{M}= {\begin{bmatrix} \mathbf{X}&\mathbf{Y}&\mathbf{Z}\end{bmatrix}}$$

Given the functional variables data matrices $\mathbf {X}$ and $\mathbf {Y}$. Their cross-covariance $\mathbf{K}_{XY}$ has the property

$$\operatorname {cov} (\mathbf {X} ,\mathbf {Y} )=\operatorname {cov} (\mathbf {Y} ,\mathbf {X} )^{\rm {T}}$$

Therefore, after centering $\mathbf{M}$, the covariance matrix $\mathbf{K}_{MM}$ has the following block structure: 

$$\mathbf{K}_{MM} ={\begin{bmatrix}\mathbf{K}_{XX}&\mathbf{K}_{XY}&\mathbf{K}_{XZ}\\
\mathbf{K}_{XY}^{\rm{T}}&\mathbf{K}_{YY}&\mathbf{K}_{YZ}\\
\mathbf{K}_{XZ}^{\rm{T}}&\mathbf{K}_{YZ}^{\rm{T}}&\mathbf{K}_{ZZ}\end{bmatrix}}$$

The off-diagonal matrices has the information of the linear correlations. The diagonal elements are positive real, then the eigenvalue equation for $$\mathbf{K}_{MM}$$ can be solved, and the resulting eigenvectors $\mathbf{u}_{i}$ are also block composed. The first block represents the $x$ variable, the second represents $y$, and so on.

$$ \mathbf{u} = {\begin{bmatrix}\mathbf{u}_{x}\\\mathbf{u}_{y}\\\mathbf{u}_{z}\end{bmatrix}}.$$

The principal components should be also written as

$$ \boldsymbol{\xi} = \Delta t^{-1/2}\mathbf{u},$$

and the score $f_{ij}$ is the same for each of the principal variable components. The expansion can be written as

$$ Expansion in vector notation$$

The difference of the resulting scores between the uni-variate and the multi-variate solution, is that the scores will be Pearson uncorrelated for the multi-variate case. For the uni-variate case, will be Pearson correlate between variables, but uncorrelated between the scores of the same variable. Uncorrelated scores allow you to use the scores as the input for a linear model, that is the only advantage in supervised machine learning, but if you use any non-linear model you avoid collinearity issues.   


Given stacked matrices of functional data, where each matrix represent a variable, the resulting cross-covariance matrix has simetrycal properties.The diagonal has real positive elements, and the off-diagonal constituent matrices are the transpose of their mirror matrix. Then, this is a system of symmetric real matrices.

# Pythom implementation

Here, I am going to use just Numpy, and test the implementation with the classical functional Gait cycle dataset.

First, lets download the data curves of hip and knee. Get them from the following links:

[hip data]({{site.baseurl}}/datasets/hip.npy "download"), [knee data]({{site.baseurl}}/datasets/knee.npy "download"), [domain data]({{site.baseurl}}/datasets/domain.npy "download")

Load hip, knee, the time domain, and plot the curves.

```python
import numpy as np
import pandas as pd
import math
import matplotlib.pyplot as plt

domain = np.load("domain.npy")
hip = np.load("hip.npy")
knee = np.load("knee.npy")
```

The code for principal component calculation is as follow:


```python
n_comps = 3
# calculate mean
hip_mean = hip.mean(axis=0)
knee_mean = knee.mean(axis=0)
m_ccov = np.cov(hip, knee, rowvar=False)
v, u = np.linalg.eig(m_ccov)
w = 1/20
# eigenfunctions
phi = math.pow(w, -0.5)*u
# scores is the dot product times w.
data_block = np.block([hip, knee])
# center data
mean_vect = data_block.mean(axis=0)
scores = w*np.matmul(data_block - mean_vect, phi[:, :n_comps])
# multiple for perturbation
hip_tav = w*hip_mean.sum()
c_hip = np.linalg.norm(hip_mean - hip_tav)*math.pow(w, 0.5)
# perturbation of mean
hip_up = hip_mean + 0.5*c_hip*phi[:, 0][0:len(domain)]
hip_down = hip_mean - 0.5*c_hip*phi[:, 0][0:len(domain)]
```


# Synthetic data

Given the basis $\xi_{j}$, It allows us to build a random set of data generating random values for $f_{j}$. It could be points or curves. For example, a set
of arithmetic Brownian trajectories. 


References
----------

[^fn1]: J.O Ramsay, B.W Silverman. Functional Data Analysis, Second edition, Springer.

[^fn2]:  Limin Wang. Karhunen-Loeve Expansions and their Applications. Phd thesis 2008.The London School of Economics and Political Science.

[^fn3]: Jane-Ling Wang, Jeng-Min Chiou, Hans-Georg Muller. Review of Functional Data Analysis. Annu Rev Statist 2015



