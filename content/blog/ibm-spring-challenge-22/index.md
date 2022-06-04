---
title: "Quantum Simulation of Many-body Physics"
date: 2022-06-03
draft: false
tags: []
categories: [quantum-computing]
---

Around two weeks ago I participated in the [IBM Quantum Spring Challenge 2022](https://research.ibm.com/blog/quantum-spring-challenge-2022), which was a fun challenge to do because the first topic covered is actually close to my heart, which is on quantum simulations of many-body systems in condensed matter physics. In these problems, we investigated a well-known phenomenon called [Anderson localization](https://en.wikipedia.org/wiki/Anderson_localization) and one called [many-body localization](https://en.wikipedia.org/wiki/Many_body_localization), which is still an active topic of research. The other topic was on quantum chemistry calculations with the variational quantum eigensolver (VQE), which I will not cover in this blog post. Interested readers are encouraged to read the original announcement linked above for more details. Here is the [official Github repository](https://github.com/qiskit-community/ibm-quantum-spring-challenge-2022) if you want to have a go at the challenge problems. Without further ado, let us begin our discussion.

## Tight-binding model of a quantum chain	

The tight-binding model would be the building block for studying the many-body physics that will be discussed throughout this post. In layman's terms, the tight-binding model describes a solid-state system where most electrons are "tightly bound" to their nuclei, which are called the lattice sites in this context. Only a few valence electrons are loosely bound and therefore can "hop" to the neighboring sites. This hopping action is what leads to an extended Bloch wavefunction, which is the electron wavefunction in the presence of a period lattice potential. The tight-binding model for a 1D chain (yes, we will focus only on the 1D system in our analysis) can be written in terms of the Pauli operators as
{{< katex >}}
$$
H_\text{tb}/\hbar = \sum_i \epsilon_i Z_i + J\sum_{\langle i,j\rangle}(X_i X_j + Y_i Y_j),
$$
where the first term corresponds to the on-site energy of each site, and the second term describes the hopping between neighboring sites, with \\(J\\) being the hopping strength.