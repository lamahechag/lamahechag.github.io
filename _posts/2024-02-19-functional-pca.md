---
layout: post
title: Principal Component Analysis for functional data
date: 2024-01-19
datatable: true
published: true
---

# Motivation
A few years ego in my job, I was dealing with datasets of time curves. It was needed to transform those curves to simple data-points, that allowed the use of a classical machine learning model. R and Python offered several package alternatives, but were not stable repositories(for production) and without the specific solution for multivariate 1d domain curves.

This challenged me to understand in deep multivariate functional PCA. Fortunately I had some background of Functional analysis. I got just a 3.5/5; and never thought, the little I learned, would be helpful. 

Here I develop the methods of chapter 8 from the book \[[^fn1]\]. I translate the integral approximations, of functional eigen-problem, into a more friendly matrix representation of blocks, that reduces the functional PCA to the classical eigen-value matrix calculation.   

# Introduction
We usually use PCA for dimensionality reduction or to find the most important
component of variation within a dataset. We use to do it with data-points, but real data are not just points, are curves(time series), are surfaces(images). Here is where functional analysis brings a general approach to find the 
principal components and reduce the data.

<div align="center">
    <img src="{{site.baseurl}}/images/fda/from0d_2d.png">
</div>

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

The scores are calculated as:

$$ f_{ij} = \int x_{ci}(t)\xi_{j}(t)\,dt \approx \Delta t \boldsymbol{\xi}_{j} \cdot \mathbf{x}_{ci}$$

And can be written, more general, as the product of the centered data-matrix with the column matrix of principal components.

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

$$ {\begin{bmatrix}\mathbf{x}_{i}\\\mathbf{y}_{i}\\\mathbf{z}_{i}\end{bmatrix}} = {\begin{bmatrix}\bar{x}\\\bar{y}\\\bar{z}\end{bmatrix}} + \sum_{j=1}^{\infty} f_{ij}\Delta t^{-1/2}{\begin{bmatrix}\mathbf{u}_{jx}\\\mathbf{u}_{jy}\\\mathbf{u}_{jz}\end{bmatrix}}$$

The difference of the resulting scores between the uni-variate and the multi-variate solution, is that the scores will be Pearson uncorrelated for the multi-variate case. For the uni-variate case, will be Pearson correlate between variables, but uncorrelated between the scores of the same variable. Uncorrelated scores allow you to use the scores as the input for a linear model, that is the only advantage in supervised machine learning, but if you use any non-linear model you avoid collinearity issues \[[^fn4]\].   

# Pythom implementation

Here I am going to use just Numpy, and test the implementation with the classical functional Gait cycle dataset.

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

<div align="center">
    <img src="{{site.baseurl}}/images/fda/hip_knee.png">
</div>

The code for principal component calculation is as follow:


```python
n_comps = 3
# calculate mean
hip_mean = hip.mean(axis=0)
knee_mean = knee.mean(axis=0)
# covariance matrix and solve eigen-value problem.
m_ccov = np.cov(hip, knee, rowvar=False)
v, u = np.linalg.eig(m_ccov)
w = 1/20
# eigenfunctions
phi = math.pow(w, -0.5)*u
# create block matrix
data_block = np.block([hip, knee])
# center data. Scores are the dot product times w.
mean_vect = data_block.mean(axis=0)
scores = w*np.matmul(data_block - mean_vect, phi[:, :n_comps])
# multiple for perturbation
hip_tav = w*hip_mean.sum()
c_hip = np.linalg.norm(hip_mean - hip_tav)*math.pow(w, 0.5)
# perturbation of mean
hip_up = hip_mean + 0.5*c_hip*phi[:, 0][0:len(domain)]
hip_down = hip_mean - 0.5*c_hip*phi[:, 0][0:len(domain)]
```
Note that the code implements all the matrix operations developed in the last 2 sections. The resulting components
can be seen in the following figures:

<p align="center">
  <img alt="Light" src="{{site.baseurl}}/images/fda/pc1_hip.png" width="45%">
&nbsp; &nbsp; &nbsp; &nbsp;
  <img alt="Dark" src="{{site.baseurl}}/images/fda/loop_component.png" width="45%">
</p>

The first univariate plot represents the hip principal component. The loop represents the first components of hip(var 1) and knee(var 2) jointly. This is the same result obtained by Ramsay and Silverman \[[^fn1]\] pages 167-170. 

Each curve can be build approximately with the first $k$ components, lets say the $k$ components that carry the 90% of the variance. In this bivariate set, we write

$$ {\begin{bmatrix}\mathbf{x}_{i}\\\mathbf{y}_{i}\end{bmatrix}} \approx {\begin{bmatrix}\bar{x}\\\bar{y}\end{bmatrix}} + \sum_{j=1}^{k} f_{ij}\Delta t^{-1/2}{\begin{bmatrix}\mathbf{u}_{jx}\\\mathbf{u}_{jy}\end{bmatrix}}.$$




# Synthetic data

Given the basis $\xi_{j}$, It allows us to build a random set of data, generating random values for $f_{j}$. It could be points or curves. For example, a set
of arithmetic Brownian trajectories. 

The arithmetic Brownian motion, continuous in time, is called Wiener process, and its representation as a Fourier expansion is:

$$ W_{t}={\sqrt {2}}\sum _{i=1}^{\infty }f_{i}{\frac {\sin \left(\left(i-{\frac {1}{2}}\right)\pi t\right)}{\left(i-{\frac {1}{2}}\right)\pi }} $$

where $f_{k}$ is Gaussian with zero mean and variance one. The sine functions are the components $\xi_{i}(t)$. Each component has equal variance importance. The approximate form is

$$ W_{t} \approx {\sqrt {2}}\sum _{i=1}^{k}f_{i}{\frac {\sin \left(\left(i-{\frac {1}{2}}\right)\pi t\right)}{\left(i-{\frac {1}{2}}\right)\pi }}.$$

Lets create a code, and see how this Fourier summation approaches an arithmetic random walk as $k$ increases.

```python
k_comp = 10
xis = np.random.normal(loc=0, scale=1, size=k_comp)
t = np.arange(0, 1, 0.001)
sin_curves = []
for k in range(1, k_comp+1):
    sin_curves.append(xis[k-1]*np.sin((k-0.5)*np.pi*t)/((k-0.5)*np.pi))

instance = math.sqrt(2)*(np.array(sin_curves)).sum(axis=0)
```

<div align="center">
    <img src="{{site.baseurl}}/images/fda/comps_weiner_fourier.png">
</div>

The synthetic functional dataset for this example will have 10 components and 100 curves:

<div align="center">
    <img src="{{site.baseurl}}/images/fda/weiner_curves.png">
</div>

The curves follow a diffusion process as expected for a random walk.

To create a correlated bivariate functional data, It is needed two independent datasets $x$ and $y$. Then, the two variables can be correlate with the following operation\[[^fn2]\]:

$$ {\begin{bmatrix}\mathbf{x'}_{i}\\\mathbf{y'}_{i}\end{bmatrix}} ={\begin{bmatrix} \sqrt{\frac{1+\rho}{2}} & \sqrt{\frac{1-\rho}{2}}\\ \sqrt{\frac{1+\rho}{2}} & -\sqrt{\frac{1-\rho}{2}}\end{bmatrix}} {\begin{bmatrix}\mathbf{x}_{i}\\\mathbf{y}_{i}\end{bmatrix}}$$

Where $\rho$ is the correlation. 


<div align="center">
    <img src="{{site.baseurl}}/images/fda/cov_map_nocorr.png">
</div>

<div align="center">
    <img src="{{site.baseurl}}/images/fda/cov_map_corr.png">
</div>


<p align="center">
  <img alt="Light" src="{{site.baseurl}}/images/fda/x_p1_fourier.png" width="45%">
&nbsp; &nbsp; &nbsp; &nbsp;
  <img alt="Dark" src="{{site.baseurl}}/images/fda/xy_p1_fourier.png" width="45%">
</p>

References
----------

[^fn1]: J.O Ramsay, B.W Silverman. Functional Data Analysis, Second edition, Springer.

[^fn2]:  Limin Wang. Karhunen-Loeve Expansions and their Applications. Phd thesis 2008.The London School of Economics and Political Science.

[^fn3]: Jane-Ling Wang, Jeng-Min Chiou, Hans-Georg Muller. Review of Functional Data Analysis. Annu Rev Statist 2015

[^fn4]: [Multivariate principal component analysis for data observed in diﬀerent domains Clara Happ and Sonja Greven](https://doi.org/10.1080/01621459.2016.1273115)

