---
title: "Quantum simulation of many-body physics - III"
date: 2022-06-26
draft: false
tags: [quantum-computing, physics]
categories: []
---

<!-- <img src="mbl_probs_W.svg" width=80%/ class="center">

<img src="mbl_imbal_W.svg" width=65%/ class="center">

<img src="mbl_entropy_W.svg" width=65%/ class="center">

<img src="mbl_probs_U.svg" width=80%/ class="center">

<img src="mbl_imbal_U.svg" width=65%/ class="center">

<img src="mbl_entropy_U.svg" width=65%/ class="center"> -->

{{< katex >}}

This is the third and final blog post of the 2022 IBM Spring Challenge series where I introduce some basic concepts and implementations of quantum simulation of many-body physics. Just a quick recap here: In the first post we set up the quantum system that we wanted to investigate using a 1-D tight-binding model, and we discussed how to utilize the Trotterization procedure to implement the time-evolution of the system on a gate-based quantum computer. In the second post, using the tools developed previously, we simulated the propagation of one particle excitation on a 1-D quantum chain with and without disorder on both a simulator and a real quantum computer. We saw the behavior of a quantum random walk when there is no disorder and Anderson localization when disorder is present. We emphasize that the tight-binding model that we have been working with so far does not include particle-particle interactions. So Anderson localization is really just a single-particle, i.e., non-interacting, effect. Then naturally we are prompted to ask the question of what happens when we do take interactions into account. This leads to the rich physics of [many-body localization (MBL)](https://en.wikipedia.org/wiki/Many_body_localization) that we will touch on in this post.

## Thermalization vs. Localization

To talk about many-body localization, it is beneficial to first introduce the concept of thermalization, or thermal equilibrium, following Ref. [1]. Thermalization in a closed classical system hinges on the powerful *ergodic hypothesis*, which states that over a long period of time all microstates of the system are accessed with equal probability. However, this notion of ergodicity cannot be directly translated to quantum systems. Here is a simple example to see why. Let us consider an isolated quantum many-body system with Hamiltonian \\(H\\). The generic initial non-equilibrium state \\(\ket{\psi(0)}\\) can be expanded over the basis of many-body eigenstates \\(\ket{\alpha}\\) as \\(\ket{\psi(0)} = \sum_\alpha A_\alpha \ket{\alpha}\\). After the quantum evolution over an arbitrarily long time \\(t\\), the state becomes

$$
\ket{\psi(t)} = e^{-iHt} \ket{\psi(0)} = \sum_\alpha A_\alpha e^{-iE_\alpha t} \ket{\alpha},
$$

where \\(E_\alpha\\) is the energy of the eigenstate \\(\ket{\alpha}\\). Then the probability of finding the system in a given eigenstate \\(\ket{\alpha}\\), \\(p_\alpha = \lvert A_\alpha\rvert^2\\), is set by the choice of the initial state and does not change over time. So unlike the classical case, the time evolution of a quantum system does not uniformly sample all states in the Hilbert space. In the quantum case, we should instead demand that thermalization implies that starting from a physical initial state the system's observables reach values given by the *microcanonical* ensembles at sufficiently long times. The infinite-time average of a physical observable described by an operator \\(O\\) is given by

$$
\langle O \rangle = \lim_{T\to\infty} \frac{1}{T}\int_0^T dt\ \langle\psi(t)\vert O\vert \psi(t)\rangle  = \sum_\alpha p_\alpha \langle\alpha\vert O\vert \alpha\rangle.
$$

Since \\(p_\alpha\\) are fixed by the initial state, the natural way to ensure that an observable \\(O\\) reaches a thermal expectation value at long times for generic initial states is to assume that the expectation values in individual eigenstates \\(\langle\alpha\vert O\vert \alpha\rangle\\) agree with the microcanonical ensemble. This is the essence of the [eigenstate thermalization hypothesis (ETH)](https://en.wikipedia.org/wiki/Eigenstate_thermalization_hypothesis), which is an important concept in this subject. More precisely, the ETH states that in ergodic systems individual many-body eigenstates have thermal observables, identical to microcanonical ensemble values at energy \\(E = E_\alpha\\), i.e., \\(\langle\alpha\vert O\vert \alpha\rangle \approx \langle O\rangle_\text{mc}(E)\\).

After making a fairly long digression, rather than diving further into the glory details of the ETH and its implications, we will come back to, in some sense, the other end of the spectrum, localization. Thermalization requires that different parts of ergodic systems exchange energy and particles, and consequently thermal systems must be conducting. On the other hand, localization leads to the absence of diffusion, suppressing transport. We have seen the example of Anderson localization, where a disorder potential can completely change the nature of single-particle eigenstates in a non-interacting system. However, interactions between particles are inevitable in realistic systems. It is conceivable that interactions may open up new transport channels, e.g., a high-energy localized state may decay to produce excitations at lower energies, potentially restoring transport. Therefore, to understand what the fate of localization would be in the presence of particle interaction is not a trivial problem. In particular, Basko, Aleiner, and Altshuler (BAA) first studied the stability of the Anderson insulator against short-ranged interactions and concluded that the interacting model becomes localized in arbitrary dimensions below some critical temperature [2]. The resulting insulator at nonvanishing temperature is termed the MBL phase. Later, by studying a 1-D disordered fermionic chain, it was pointed out that the MBL phase can persist even at infinite temperature [3]. More recent developments however challenged the conclusion from BAA. It was argued that in high dimensions \\(d>1\\) small thermal inclusions can trigger avalanches in the system that destroy the MBL phase [4,5]. The reconciliation of these two results is still an open problem.

## The Heisenberg model

We simulate the MBL phase in a XXZ Heisenberg spin chain...

It can be readily mapped to a spinless fermionic model via Jordan-Wigner transformation...

## References

1. [D. A. Abanin, E. Altman, I. Bloch, and M. Serbyn, Colloquium: Many-body localization, thermalization, and entanglement. *Rev. Mod. Phys.* 91, 021001 (2019).](https://journals.aps.org/rmp/abstract/10.1103/RevModPhys.91.021001)

2. [B. Basko, A. Aleiner, and A. Altshuler, Metal-insulator transition in a weakly interacting many-electron system with localized single-particle states. *Ann. Phys.* 321, 1126 (2006).](https://www.sciencedirect.com/science/article/pii/S0003491605002630?via%3Dihub)

3. [V. Oganesyan and D. V. Huse, Localization of interacting fermions at high temperature. *Phys. Rev. B* 75, 155111 (2007).](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.75.155111)

4. [W. De Roeck and F. Huveneers, Stability and instability towards delocalization in many-body localization systems. *Phys. Rev. B* 95, 155129 (2017).](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.95.155129)

5. [D. J. Luitz, F. Huveneers, and W. De Roeck, How a Small Quantum Bath Can Thermalize Long Localized Chains. *Phys. Rev. Lett.* 119. 150602 (2017).](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.119.150602)
   
<!-- 3. [R. Nandkishore and D. A. Huse, Many-Body Localization and Thermalization in Quantum Statistical Mechanics. *Annu. Rev. Condens. Matter Phys.* 6, 15 (2015).](https://www.annualreviews.org/doi/abs/10.1146/annurev-conmatphys-031214-014726) -->
