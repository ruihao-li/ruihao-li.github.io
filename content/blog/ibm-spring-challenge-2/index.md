---
title: "Quantum simulation of many-body physics - II"
date: 2022-06-18
draft: false
tags: []
categories: [quantum-computing]
---

As promised in the [previous blog post](/blog/ibm-spring-challenge-1/), we will now continue our journey into the world of many-body physics simulations with quantum computers. In this post, we want to address the question of how a particle such as an electron, propagates through an intrinsically quantum system, i.e., the 1-D quantum chain that we built previously. Remember everything we discussed so far has been without taking into account *disorder* that is generally present in realistic condensed matter systems. Disorder is something that generically breaks some symmetries of the system Hamiltonian and/or lead to deviations from the lattice periodicity. When disorder is present, all the sites in a lattice are no longer equivalent. So another question naturally arises: how would disorder affect the quantum transport? The exploration of this general question has led to wonderful discoveries of various localization effects in disordered systems. Here we will touch on one of them, Anderson localization, which was first discovered by the great physicist [Philip W. Anderson](https://en.wikipedia.org/wiki/Philip_W._Anderson) in 1958 [1]. Note that the dimensionality of a system has a direct impact on Anderson localization. For example, according to the scaling theory of localization [2], in 1- and 2-D, a system will be a perfect insulator in the thermodynamic limit (simply put, when the system size is taken to infinity). Therefore, Anderson localization must happen in one- and two-dimensional disordered systems regardless of the disorder strength! However, in 3-D, Anderson localization is a critical phenomenon where the system undergoes a [metal-insulator transition (MIT)](https://en.wikipedia.org/wiki/Metal%E2%80%93insulator_transition). This means that localization happens only when the disorder strength exceeds a certain threshold. Even though this is certainly one of the most interesting aspects of Anderson localization, we will not explore it here for the sake of simplicity. We will again stick to the 1-D system as in the previous post.

## Quantum random walk

Classical random walk is a random process that is prevalent in many phenomena in nature, such as the motion of macroscopic particles in liquids and gases known as [Brownian motion](https://en.wikipedia.org/wiki/Brownian_motion) and even the price of a fluctuating stock. In the most rudimentary version of a *symmetric* classical random walk on a lattice, at each time step, the probabilities of the particle jumping to any of the neighboring sites are the same. In 1-D, this means that the particle can move one site to the left or right with equal probability (50%) each time. A well-established result for a symmetric random walk is that in the continuous-time limit, the probability of finding the particle at time \\(t\\) at position \\(r\\) (from the origin) follows a Gaussian distribution:

{{< katex >}}
$$
P_\text{classical}(r, t) = \frac{1}{\sqrt{2\pi t}} e^{-r^2/2t},
$$

where we have assumed the distance of each jump to be 1. Therefore, the standard deviation of this probability distribution scales as \\(\sigma_\text{classical}\propto \sqrt{t}\\). Since \\(\langle r \rangle = 0\\), this suggests that the mean square displacement of the particle, which quantifies the spatial propagation of the particle relative to the origin, also scales *diffusively* with time as \\(\sqrt{\langle r^2\rangle} \propto \sqrt{t}\\).

Since we are interested in particle propagation in a quantum system, we will need to deal with *quantum random walks* instead. In this case, the intrinsic quantum nature including superposition and interferences among different wavefunctions will lead to a qualitative difference from the classical counterpart. Here is a somewhat intuitive way to think about the difference between them. Imagine a "classical walker" who decides whether to step left or right by tossing a coin with two possible outcomes \\(+\\) and \\(-\\), with probabilities \\(P_+\\) and \\(P_-\\), respectively. After each toss, they would look at the result and decide which way to do. So the classical random walk traces a single path within a decision tree. In contrast, a "quantum walker" flips their coin but never looks at the outcome. Instead, at each step, they step *simultaneously* to the left and right with some complex amplitudes \\(A_+\\) and \\(A_-\\) corresponding to probabilities \\(P_+ = \lvert A_+\rvert^2\\) and \\(P_- = \lvert A_-\rvert^2\\). After many iterations the quantum random walk results in an extended wavefunction of the quantum walker that spreads out to all positions in the tree with finite amplitudes. What's more incredible is that these complex amplitudes at different sites add up for any given path and depending on the phase differences, this creates constructive or destructive interferences when measuring the probabilities at the end. A diagram illustrating the ideas above is shown below (taken from [3]).

<img src="random_walks.png" width=60%/ class="center">

Due to the critical differences highlighted above, a symmetric quantum random walk in the continuous-time limit will lead to a probability distribution that follows a [Bessel function of the first kind](https://en.wikipedia.org/wiki/Bessel_function#Bessel_functions_of_the_first_kind:_J%CE%B1) [4]:

$$
P_\text{quantum}(r, t) = \lvert J_r(2t) \rvert^2.
$$

Below is a comparison of the two probability distributions for continuous-time classical and quantum random walks:

<img src="rw_dist.svg" width=70%/ class="center">

Moreover, a quantum random walk exhibits *ballistic* propagation with the mean square displacement scaling linearly with time, \\(\sqrt{\langle r^2\rangle} \propto t\\). The quadratic speed-up of quantum random walks versus classical random walks is analogous to the quadratic speed-up of the Grover search algorithm compared to a classical search!

Let us now simulate the quantum random walk first on a simulator and then on a real quantum computer to see if our theory holds true.

## Anderson localization



---

### References:

[1] [P. W. Anderson, Absence of Diffusion in Certain Random Lattices. Phys. Rev. 109, 1492 (1958).](https://journals.aps.org/pr/abstract/10.1103/PhysRev.109.1492)

[2] [E. Abrahams, P. W. Anderson, D. C. Licciardello, and T. V. Ramakrishnan, Scaling Theory of Localization: Absence of Quantum Diffusion in Two Dimensions. Phys. Rev. Lett. 42, 673 (1979).](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.42.673)

[3] [K. Manouchehri and J. B. Wang, Solid State Implementation of Quantum Random Walks on General Graphs. AIP Conf. Proc. 1074, 56 (2008).](https://aip.scitation.org/doi/abs/10.1063/1.3037138)

[4] [J.Kempe, Quantum random walks - an introductory overview. arXiv:quant-ph/0303081 (2003).](https://arxiv.org/abs/quant-ph/0303081)
