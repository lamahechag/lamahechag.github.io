---
layout: post
title: Can the eigenvalues of a block matrix be written in terms of the eigenvalues of the constituent matrices?
date: 2023-12-01
datatable: true
published: True
---

Given stacked matrices of functional data, where each matrix represent a variable, te resulting cross-covariance matrix has simetrycal properties.The diagonal has real positive elements, and the off-diagonal constituent matrices are the transpose of their mirror matrix. Then, this is a system of symmetric real matrices.

Given the functional variables data matrices $\mathbf {X}$ and $\mathbf {Y}$. Their cross-covariance $\mathbf{K}_{XY}$ has the property

$$\operatorname {cov} (\mathbf {X} ,\mathbf {Y} )=\operatorname {cov} (\mathbf {Y} ,\mathbf {X} )^{\rm {T}}$$

Given the functional variables data $\mathbf{X}$, $\mathbf{Y}$ and $\mathbf{Z}$(or $n$ functional variables), we can create the block matrix
$$ \mathbf{M}= {\begin{bmatrix} \mathbf{X}&\mathbf{Y}&\mathbf{Z}\end{bmatrix}}$$

Whose covariance matrix $\mathbf{K}_{MM}$ has the following block structure: 

$$\mathbf{K}_{MM} ={\begin{bmatrix}\mathbf{K}_{XX}&\mathbf{K}_{XY}&\mathbf{K}_{XZ}\\
\mathbf{K}_{XY}^{\rm{T}}&\mathbf{K}_{YY}&\mathbf{K}_{YZ}\\
\mathbf{K}_{XZ}^{\rm{T}}&\mathbf{K}_{YZ}^{\rm{T}}&\mathbf{K}_{ZZ}\end{bmatrix}}$$

Here, each matrix is symmetric with real values. Therefore, their eigenvalues are real. Giving this, is it possible to solve the eigenvalues of the block matrix in terms of the constituents matrices?

The closest development, related to this eigenvalue problem, I have found is from this paper [Eigenvectors from eigenvalues: A survey of a basic identity in linear algebra](https://arxiv.org/abs/1908.03795)

Also because of the blocking strategy, new matrix multiplication algorithms  could be a hint [Discovering faster matrix multiplication algorithms with reinforcement learning](https://www.nature.com/articles/s41586-022-05172-4) 

<p xmlns:cc="http://creativecommons.org/ns#" xmlns:dct="http://purl.org/dc/terms/"><a property="dct:title" rel="cc:attributionURL" href="https://lamahechag.github.io/eigen-block-matrix/">Can the eigenvalues of a block matrix be written in terms of the eigenvalues of the constituent matrices?</a> by <a rel="cc:attributionURL dct:creator" property="cc:attributionName" href="https://lamahechag.github.io/about/">Luis Alejandro Mahecha</a> is licensed under <a href="http://creativecommons.org/licenses/by/4.0/?ref=chooser-v1" target="_blank" rel="license noopener noreferrer" style="display:inline-block;">Attribution 4.0 International<img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/cc.svg?ref=chooser-v1"><img style="height:22px!important;margin-left:3px;vertical-align:text-bottom;" src="https://mirrors.creativecommons.org/presskit/icons/by.svg?ref=chooser-v1"></a></p>


