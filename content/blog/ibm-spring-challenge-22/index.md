---
title: "Quantum simulation of many-body physics"
date: 2022-06-03
draft: false
tags: []
categories: [quantum-computing]
---

A few weeks ago I participated in the [IBM Quantum Spring Challenge 2022](https://research.ibm.com/blog/quantum-spring-challenge-2022), which was a fun challenge to do because the one of the topics covered is actually close to my heart, which is on quantum simulations of many-body systems in condensed matter physics. In these problems, we investigated a well-known phenomenon called [Anderson localization](https://en.wikipedia.org/wiki/Anderson_localization) and one called [many-body localization](https://en.wikipedia.org/wiki/Many_body_localization), which is still an active topic of research. The other topic was on quantum chemistry calculations with the variational quantum eigensolver (VQE), which I will not cover in this blog post. Interested readers are encouraged to read the original announcement linked above for more details. Here is the [official Github repository](https://github.com/qiskit-community/ibm-quantum-spring-challenge-2022) if you want to have a go at the challenge problems. Without further ado, let us begin our discussion.

## Tight-binding model of a quantum chain	

The tight-binding model would be the building block for studying the many-body physics that will be discussed throughout this post. In layman's terms, the tight-binding model describes a solid-state system where most electrons are "tightly bound" to their nuclei, which sit at the fixed lattice sites. Only a few valence electrons are loosely bound and therefore can "hop" to the neighboring sites. This hopping action is what leads to an extended Bloch wavefunction, which is the electron wavefunction in the presence of a period lattice potential. The tight-binding model for a 1D quantum chain (yes, we will focus only on the 1D system in our analysis) can be written in terms of the Pauli operators as
{{< katex >}}
\begin{equation}
H_\text{tb}/\hbar = \sum_i \epsilon_i Z_i + J\sum_{\langle i,j\rangle}(X_i X_j + Y_i Y_j),
\end{equation}
where the first term corresponds to the on-site energy of each site, and the second term describes the hopping between neighboring sites, with \\(J\\) being the hopping strength. 

One of the things we care about in quantum simulations is the time-evolution of quantum states, which is determined by the unitary operator \\(e^{-iHt/\hbar}\\) in quantum mechanics, where \\(H\\) is the time-independent Hamiltonian, which is \\(H_\text{tb}\\) in the case of our 1D quantum chain. Now, to execute the time evolution unitary on a gate-based quantum computer, one must decompose it into a product of one- and two-qubit gates that can be implemented on the quantum computer. One method to accomplish this is called [Trotterization](https://qiskit.org/documentation/stubs/qiskit.synthesis.SuzukiTrotter.html), which is essentially a discretized approximation to the continuous time evolution. To demonstrate it, let us consider a simple 3-site system. We will only concern about the second term in Eq. (1) above since the exponentiation of the Pauli-\\(Z\\) operator in the first term is just the \\(R_Z\\) gate, which is a common one-qubit gate that can be readily implemented on a quantum computer. On the other hand, the second term contains two-site interactions that will lead to some two-qubit gates. In particular, the time-evolution unitary of this system is given by
\begin{equation}
\begin{split}
U_\text{tb}(t) &= \exp\left[-it(H_\text{tb}^{(0,1)} + H_\text{tb}^{(1,2)})\right] \\\\
&\approx \left[\exp\left(\frac{-it}{n}H_\text{tb}^{(0,1)}\right) \exp\left(\frac{-it}{n}H_\text{tb}^{(1,2)}\right) \right],
\end{split}
\end{equation}
where in the second step we have applied Trotterization and \\(n\\) is the number of Trotter steps, i.e., discrete time steps.