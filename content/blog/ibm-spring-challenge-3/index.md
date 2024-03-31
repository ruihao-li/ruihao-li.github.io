---
title: "Quantum simulation of many-body physics - III"
date: 2022-06-26
draft: false
tags: [quantum computing, physics]
categories: []
---

<!-- <img src="mbl_probs_W.svg" width=80%/ class="center">

<img src="mbl_imbal_W.svg" width=65%/ class="center">

<img src="mbl_entropy_W.svg" width=65%/ class="center">

<img src="mbl_probs_U.svg" width=80%/ class="center">

<img src="mbl_imbal_U.svg" width=65%/ class="center">

<img src="mbl_entropy_U.svg" width=65%/ class="center"> -->

{{< katex >}}

This is the third and final blog post of the 2022 IBM Spring Challenge series where I introduce some basic concepts and implementations of quantum simulation of many-body physics. Just a quick recap here: In the first post we set up the quantum system that we wanted to investigate using a 1-D tight-binding model, and we discussed how to utilize the Trotterization procedure to implement the time-evolution of the system on a gate-based quantum computer. In the second post, using the tools developed previously, we simulated the propagation of one particle excitation on a 1-D quantum chain with and without disorder on both a simulator and a real quantum computer. We saw the behavior of a quantum random walk when there is no disorder and Anderson localization when disorder is present. We emphasize that the tight-binding model that we have been working with so far does not include particle-particle interactions. So Anderson localization is really just a single-particle, i.e., non-interacting, effect. Then naturally we are prompted to ask the question of what happens when we do take interactions into account. This leads to the rich physics of [many-body localization (MBL)](https://en.wikipedia.org/wiki/Many_body_localization) that we will touch on in this post. Please feel free to check out [part I](/blog/ibm-spring-challenge-1) and [II](/blog/ibm-spring-challenge-2) before continuing.

## Thermalization vs. Localization

To talk about many-body localization, it is beneficial to first introduce the concept of thermalization, or thermal equilibrium, following Ref. [[1]](#references). Thermalization in a closed classical system hinges on the powerful *ergodic hypothesis*, which states that over a long period of time all microstates of the system are accessed with equal probability. However, this notion of ergodicity does not directly apply to quantum systems. Here is a simple example to see why. Let us consider an isolated quantum many-body system with Hamiltonian \\(H\\). The generic initial non-equilibrium state \\(\ket{\psi(0)}\\) can be expanded over the basis of many-body eigenstates \\(\ket{\alpha}\\) as \\(\ket{\psi(0)} = \sum_\alpha A_\alpha \ket{\alpha}\\). After the quantum evolution over an arbitrarily long time \\(t\\), the state becomes

$$
\ket{\psi(t)} = e^{-iHt} \ket{\psi(0)} = \sum_\alpha A_\alpha e^{-iE_\alpha t} \ket{\alpha},
$$

where \\(E_\alpha\\) is the energy of the eigenstate \\(\ket{\alpha}\\). Then the probability of finding the system in a given eigenstate \\(\ket{\alpha}\\), \\(p_\alpha = \lvert A_\alpha\rvert^2\\), is set by the choice of the initial state and does not change over time. So unlike the classical case, the time evolution of a quantum system does not uniformly sample all states in the Hilbert space. Therefore, for quantum ergodicity, we should instead demand that starting from a generic initial state the system's observables (few-body operators) settle to values given by the *microcanonical* ensembles at sufficiently long times. The infinite-time average of a physical observable \\(O\\) is given by

$$
\langle O \rangle = \lim_{T\to\infty} \frac{1}{T}\int_0^T dt\ \langle\psi(t)\vert O\vert \psi(t)\rangle  = \sum_\alpha p_\alpha \langle\alpha\vert O\vert \alpha\rangle.
$$

Since \\(p_\alpha\\) are fixed by the initial state, the natural way to ensure that an observable \\(O\\) reaches a thermal expectation value at long times for generic initial states is to assume that the expectation values in individual eigenstates \\(\langle\alpha\vert O\vert \alpha\rangle\\) agree with the microcanonical ensemble. This is the essence of the [eigenstate thermalization hypothesis (ETH)](https://en.wikipedia.org/wiki/Eigenstate_thermalization_hypothesis), which is an important concept in this subject. More precisely, the ETH states that in ergodic systems individual many-body eigenstates have thermal observables that are identical to microcanonical ensemble values at energy \\(E = E_\alpha\\), i.e., \\(\langle\alpha\vert O\vert \alpha\rangle \approx \langle O\rangle_\text{mc}(E)\\). The microcanonical ensemble average can be written as

$$
\langle O\rangle_\text{mc}(E) = \lim_{\Delta E\to 0}\frac{1}{N(E, \Delta E)}\sum_{\alpha: E_\alpha\in [E-\Delta E, E+\Delta E]} \langle\alpha\vert O\vert \alpha\rangle,
$$

where \\(N(E, \Delta E)\\) is the number of eigenstates in the system that are within \\(\Delta E\\) of energy \\(E\\).

After a fairly long digression, rather than diving further into the glory details of the ETH and its implications, we will come back to, in some sense, the other end of the spectrum, MBL, which is known to violate the ETH and hence does not thermalize. Generally speaking, thermalization requires that different parts of ergodic systems exchange energy and particles, and consequently leads to conduction. On the other hand, localization leads to the absence of diffusion, suppressing transport. We have seen the example of Anderson localization, where a disorder potential can completely change the nature of single-particle eigenstates in a non-interacting system. However, interactions between particles are inevitable in realistic systems. It is conceivable that interactions may open up new transport channels, e.g., a high-energy localized state may decay to produce excitations at lower energies, potentially restoring transport. Therefore, to understand the fate of localization in the presence of particle interaction is not a trivial problem. In particular, Basko, Aleiner, and Altshuler (BAA) first studied the stability of the Anderson insulator against short-ranged interactions and concluded that the interacting model enters a localized phase termed the MBL phase in arbitrary dimensions below a certain critical temperature [[2]](#references). Later, by studying a 1-D disordered fermionic chain, it was pointed out that the MBL phase can persist even at infinite temperature [[3]](#references). More recent developments however challenged the conclusion of BAA. It was argued that in high dimensions \\(d>1\\) small thermal inclusions can trigger avalanches in the system that destroy the MBL phase [[4, 5]](#references). The reconciliation of these two results is still an open problem.

## The Heisenberg model

Following the history of this field, the introduction of the fermionic and spin-chain lattice models has definitely opened the door to studying many interesting properties of MBL in (classical) numerical simulations. This is exactly the system we have built up in the previous parts of the Challenge. Here we will do something different from the setup in the original IBM Challenge Problem 3. On top of the tight-binding model used to study Anderson localization in [part II](/blog/ibm-spring-challenge-2/), we will add an additional \\(ZZ\\)-interaction term. So the tight-binding Hamiltonian is given by

\begin{equation}
H_\text{tb}/\hbar = J\sum_{i=0}^{n-2} (X_i X_{i+1} + Y_i Y_{i+1}) + U \sum_{i=0}^{n-2} Z_i Z_{i+1} +\sum_{i=0}^{n-2}\epsilon_i Z_i,\quad
\end{equation}

where \\(n\\) is the number of sites in the 1-D chain. Readers with a condensed matter physics background may recognize that this is exactly the so-called [Heisenberg XXZ model](https://en.wikipedia.org/wiki/Quantum_Heisenberg_model). In fact, the Heisenberg model has become the paradigmatic model for studying MBL physics (among many other phenomena). The reason for the additional term should be obvious by doing the [Jordan-Wigner transformation](https://en.wikipedia.org/wiki/Jordan%E2%80%93Wigner_transformation), which maps the spin-chain Hamiltonian above to a spinless fermionic chain in the second-quantization form given by

$$
H_\text{tb}/\hbar = J\sum_{i=0}^{n-2} (c_i^\dag c_{i+1} + \text{h.c.}) + U \sum_{i=0}^{n-2} n_i n_{i+1} + \sum_{i=0}^{n-2} \epsilon_i n_i,
$$

where \\(n_i \equiv c_i^\dag c_i\\) is the density operator for site \\(i\\). Hence, we see that the additional \\(U\\)-term gives rise to the two-body interaction between neighboring sites. Without it, the tight-binding model is a free-fermion model, which is suited for the study of Anderson localization but not many-body localization. So here we will simulate the MBL phase in a disordered Heisenberg XXZ spin chain. We will again set \\(J = 1\\) and the model thus has two free parameters: the *interaction strength* \\(U\\) and the *disorder strength* \\(W\\). Recall that \\(W\\) controls the onsite potentials through the Aubry-Andre model, \\(\epsilon_i = W\cos(2\pi\beta i)\\), with \\(\beta\\) being related to the quasicrystal periodicity. 

One interesting aspect of the 1-D Heisenberg XXZ model is its phase diagram. In the limit \\(U\to 0\\) with some finite \\(W\\), i.e., when there is no interaction, this model is equivalent to free fermions moving in a disordered potential and therefore, the states are Anderson localized. Turning on the interaction will result in the MBL phase. Furthermore, for fixed and not very strong disorder (\\(W/U \sim 1\\)), it was found that tuning the interaction strength \\(U\\) above some critical value \\(U^\ast\\) will lead to delocalization. On the other hand, if we fix \\(U\\) and increase the disorder strength \\(W\\), we will see a transition from a delocalized (thermal) phase to the MBL phase when \\(W\\) goes above a critical value \\(W^\ast\\). Therefore, in 1-D interacting systems there exists a metal-insulator transition, which is distinct from non-interacting systems that always Anderson localize in the thermodynamic limit, as mentioned in the [previous post](/blog/ibm-spring-challenge-2). A schematic illustrating the phase diagram of the XXZ spin chain is shown below.

<figure>
    <img src="XXZ_phase.png" width=70%/ class="center">
    <figcaption align="center"> Phase diagram of Heisenberg XXZ spin chain as a function of interaction (top) and disorder strength (bottom). Here <i>J<sub>z</sub></i> denotes the interaction strength, equivalent to <i>U</i> in our formalism. Figure taken from Ref. [1]. </figcaption>
</figure>

## Putting things into action

Okay, enough theory. Let's actually build the quantum circuit for the Heisenberg model and simulate its dynamics. In this case, we will simulate three particles in a 12-site spin chain, where particle excitations are represented by \\(\ket{1}\\) and empty sites represented by \\(\ket{0}\\). Like before, to better visualize (de)localization, we would like to track the probability of each site being in the \\(\ket{1}\\) state over the course of the entire Trotterized time evolution. In addition, we also keep track of two other quantities. One is the *imbalance*, which is one of the signatures of the breakdown of thermalization. The system imbalance is defined as 

$$
\mathcal I = \Bigg\langle \frac{N_e - N_o}{N_e + N_o} \Bigg\rangle,
$$

where \\(N_e\\) and \\(N_o\\) are the populations at the even and odd sites of the system, respectively, and the expectation value is defined with respect to a particular quantum state, i.e. \\(\langle \cdots \rangle = \langle \psi \lvert \cdots \rvert \psi \rangle\\). In a thermalized system, we expect each site of the lattice to be occupied by the same average number of particles after reaching steady state. Therefore, the imbalance is close to zero. However, when localization happens, we should expect a deviation from zero. Here we define a function that calculates the imbalance for a given state:

```python
def get_imbalance(state):
    """
    Calculate the imbalance of a state.
    
    Args:
        state (qiskit.quantum_info.Statevector): The state vector.

    Returns:
        imbalance_val (float): The imbalance of the state.
    """
    imbalance_val = 0
    state_dict = state.to_dict()
    
    for basis, amp in state_dict.items():
        Ne, No = 0, 0
        # Make sure to skip calculating the |00...0> state
        for i in range(len(basis)):
            if i % 2 == 0 and basis[i] == '1':
                Ne += 1
            elif i % 2 == 1 and basis[i] == '1':
                No += 1
        if Ne + No != 0:
            imbalance_val += np.abs(amp)**2 * (Ne - No) / (Ne + No)
    return imbalance_val
```

The other quantity to probe is the entanglement entropy formed in the lattice as a result of particle propagation. One can imagine that while the created particle excitations are initially separable from the the rest of the lattice, their propagation will lead to the creation and distribution of entanglement throughout the lattice. For simplicity, we will only keep track of the entanglement entropy of part of the system, say the first lattice site. This can be quantified by its von Neumann entropy, which is defined as

$$
\mathcal S_\text{vn}(\rho_A) = -\text{tr}(\rho_A \ln\rho_A),
$$

where \\(A\\) refers to the subsystem of interest (i.e., the first lattice site), and \\(\rho_A = \text{tr}_{B}\rho\\) is the reduced density matrix of the subsystem \\(A\\) after "tracing out" the rest of the system which we call \\(B\\). If the subsytem \\(A\\) is fully entangled with the rest of the system, \\(\mathcal S _\text{vn}(\rho_A) = \ln 2\\), whereas if the subsytem is completely separable with respect to the rest, \\(\mathcal S _\text{vn}(\rho_A) = 0\\). One can probe entanglement at a larger scale by using more sophisticated measures such as the concurrences and global entanglement, but we will not pursue these here.

Let us now get straight into the simulation. Some necessary imports here: 

```python
import numpy as np
import matplotlib.pyplot as plt
from qiskit import QuantumCircuit, QuantumRegister, transpile, Aer
from qiskit.circuit import Parameter, Instruction
import qiskit.quantum_info as qi
from tqdm.notebook import tqdm
```

Similar to what was done in [part I](/blog/ibm-spring-challenge-1), we will first define the Trotterized quantum circuit for MBL based on the tight-binding Hamiltonian Eq. (1) above.

```python
def Trot_qc_mbl(num_qubits, t, J, deltas):
    """
    Creates the Trotterized quantum circuit at a given time for 
    the 1-D Heisenberg XXZ model.

    Args:
        num_qubits (int): The number of qubits in the circuit.
        t (Parameter): time.
        J (Parameter): The interaction strength.
        deltas (List[Parameter]): The list of the disorder 
        parameters.

    Returns:
        qiskit.circuit.QuantumCircuit: The Trotterized 
        quantum circuit.
    """

    def ZZ_gate(J, t):
        ZZ_qr = QuantumRegister(2)
        ZZ_qc = QuantumCircuit(ZZ_qr, name='ZZ')
        ZZ_qc.cnot(0,1)
        ZZ_qc.rz(2 * J * t, 1)
        ZZ_qc.cnot(0,1)
        # Convert custom quantum circuit into a gate
        ZZ = ZZ_qc.to_instruction()
        return ZZ

    def XX_gate(t):
        XX_qr = QuantumRegister(2)
        XX_qc = QuantumCircuit(XX_qr, name='XX')
        XX_qc.ry(np.pi/2, [0,1])
        XX_qc.append(ZZ_gate(1, t), [0,1])
        XX_qc.ry(-np.pi/2, [0,1])
        XX = XX_qc.to_instruction()
        return XX

    def YY_gate(t):
        YY_qr = QuantumRegister(2)
        YY_qc = QuantumCircuit(YY_qr, name='YY')
        YY_qc.rx(-np.pi/2, [0,1])
        YY_qc.append(ZZ_gate(1, t), [0,1])
        YY_qc.rx(np.pi/2, [0,1])
        YY = YY_qc.to_instruction()
        return YY
    
    Trot_qr = QuantumRegister(num_qubits)
    qc = QuantumCircuit(Trot_qr, name='Trot')

    for i in range(num_qubits - 1):
        qc.append(XX_gate(t), [Trot_qr[i], Trot_qr[i+1]])
        qc.append(YY_gate(t), [Trot_qr[i], Trot_qr[i+1]])
        qc.append(ZZ_gate(J, t), [Trot_qr[i], Trot_qr[i+1]])
    
    for i in range(num_qubits):
        qc.rz(2 * deltas[i] * t, i)
        
    return qc
```

Next we define the function that records the Trotterized circuits at all time steps. Note that we will start with 3 particle excitations at sties 0, 4, and 8.

```python
def U_trot_circuits_mbl(delta_t, trotter_steps, num_qubits, U, W, beta):
    """
    Record a list of Trotterized quantum circuits for many body localization 
    at all Trotter steps.

    Args:
        delta_t (float): Duration of individual time steps.
        trotter_steps (array): Array of intermediate times.
        num_qubits (int): The total number of qubits.
        U (float): The interaction strength.
        W (float): The disorder strength.
        beta (float): The quasicrystal periodicity of the AA model.

    Returns:
        disorder_circuits (list): List of Trotterized quantum 
        circuits for MBL.
    """

    t = Parameter('t')
    J = Parameter('J')
    deltas = [Parameter('delta_{:d}'.format(idx)) for idx in range(num_qubits)]


    AA_pattern = np.cos(2*np.pi*beta*np.arange(num_qubits))
    disorders = W * AA_pattern
    mbl_circuits = []

    for n_steps in trotter_steps:
        qr = QuantumRegister(num_qubits)
        cr = ClassicalRegister(num_qubits)
        qc = QuantumCircuit(qr, cr)

        qc.x([0, 4, 8])  # three particle excitations

        for _ in range(n_steps):
            qc.append(Trot_qc_mbl(num_qubits, t, J, deltas), qr)
            
        qc = qc.bind_parameters({t: delta_t})
        qc = qc.bind_parameters({deltas[idx]: disorders[idx] for idx in range(num_qubits)})
        qc = qc.bind_parameters({J: U})
        mbl_circuits.append(qc)
    return mbl_circuits
```

We first fix the value of the interaction strength \\(U = 1.0\\) and simulate the system at four different values of disorder strength \\(W = [0.2,\ 2,\ 4,\ 8]\\).

```python
delta_t = 0.15
trotter_steps = np.arange(1, 25, 1) 
num_qubits = 12
U = 1.0
beta = (np.sqrt(5)-1)/2

circuits = {}
Ws = [0.2, 2, 4, 8]

for W in Ws:
    circuits[W] = U_trot_circuits_mbl(
        delta_t=delta_t, 
        trotter_steps=trotter_steps, 
        num_qubits=num_qubits, 
        U=U, 
        W=W, 
        beta=beta
    )
```

We can then simulate these Trotterized circuits on Qiskit's `statevector_simulator` backend and track the time evolution of the probability of finding a particle for all the lattice sites, the imbalance of the system, and the von Neumann entropy of the first site:

```python
backend_sim = Aer.get_backend('statevector_simulator')

probability_densities = {}
state_vector_imbalances = {}
vn_entropies = {}

for W in tqdm(Ws):
    probability_densities[W] = []
    state_vector_imbalances[W] = []
    vn_entropies[W] = []
    
    for circ in circuits[W]:

        transpiled_circ = transpile(circ, backend_sim, optimization_level=3)
        job_sim = backend_sim.run(transpiled_circ)
        # Grab the results from the job.
        result_sim = job_sim.result()
        outputstate = result_sim.get_statevector(transpiled_circ, decimals=6)

        # extract the probability densities
        ps = []
        for idx in range(num_qubits):
            ps.append(np.abs(qi.partial_trace(outputstate, [i for i in range(num_qubits) if i!=idx]))[1,1]**2)
        
        # extract the density matrix of qubit 0 by tracing out all other qubits
        entropy = 0
        rho_0 = qi.partial_trace(outputstate, range(1, num_qubits))
        entropy += qi.entropy(rho_0, base=np.exp(1))
        
        # calculate the imbalance of the system
        imbalance = 0        
        imbalance += get_imbalance(outputstate)
        
        vn_entropies[W].append(entropy)
        probability_densities[W].append(ps)
        state_vector_imbalances[W].append(imbalance)
```

Below we show the results of this simulation. First is the probability densities at different disorder strengths.

<img src="mbl_probs_W.svg" width=90%/ class="center">

We see that with weak disorder the system is delocalized but as \\(W\\) increases, MBL kicks in and the particles are localized around their initial positions over time. Next we look at the imbalance as a function of time.

<img src="mbl_imbal_W.svg" width=70%/ class="center">

Again, as expected, when \\(W\\) is small, the system tends to thermalize and the average imbalance is close to 0. But at strong disorder, we see a large deviation from 0, indicated by the green and red curves in the plot. Finally, here is the result of the von Neumann entropy.

<img src="mbl_entropy_W.svg" width=70%/ class="center">

We see the average entanglement entropy over time decreases as the disorder strength increases since the system is transitioning into the MBL phase.

As the final simulation, we will fix the disorder strength \\(W = 3.5\\) and vary the interaction strength \\(U = [0.2,\ 1,\ 3,\ 5]\\) to see how the behavior of the system changes. The execution is similar to the one above, so I won't bore you with basically the same codes again. Let us directly look at the results.

<figure>
    <img src="mbl_probs_U.svg" width=90%/ class="center">
    <figcaption align="center"> Probability densities at different interaction strengths. </figcaption>
</figure>

<figure>
    <img src="mbl_imbal_U.svg" width=70%/ class="center">
    <figcaption align="center"> Evolution of the imbalance at different interaction strengths. </figcaption>
</figure>

<figure>
    <img src="mbl_entropy_U.svg" width=70%/ class="center">
    <figcaption align="center"> Evolution of the von Neumann entropy of site 0 at different interaction strengths. </figcaption>
</figure>

Based on the phase diagram discussed in the previous section, we expect that the system will transition from MBL to delocalization as \\(U\\) increases, when the disorder is not very strong. This can be observed in the probability densities, where the sign of delocalization can be seen starting from \\(U = 3\\), especially on the first qubit. This is also supported by considering the average imbalance and entanglement entropy at different interaction strengths. Although it is not as clear as the previous case with a fixed \\(U\\) and varying \\(W\\), we can still see that overall the magnitude of imbalance is smaller when the system becomes delocalized (i.e., with larger \\(U\\)), while the entanglement entropy gets larger. These simulation results agree with the theoretical expectations.

## Conclusion

Here we conclude the quantum simulations of many-body localization as well as the blogging about the IBM Quantum Spring Challenge 2022 as a whole. We have explored parts of the phase diagram of a 1-D disordered Heisenberg XXZ chain. We saw the somewhat competing effect between particle interaction and disorder, leading to transition between thermal and MBL phases. Our quantum simulations have successfully captured some of the key features of this phase diagram. However, it is fair to say that the simulations showcased here barely scratch the surface of the fascinating physics of MBL and non-equilibrium quantum systems as a whole. There are other profound topics of the MBL systems which we did not touch upon and some of them are under active investigation, including the emergent integrability and even a whole new class of MBL-protected phases of matter. 

With that, I want to thank you for taking the time to follow along and I hope you enjoyed this journey into quantum simulations of many-body physics as much as I did!

## References

1. [D. A. Abanin, E. Altman, I. Bloch, and M. Serbyn, Colloquium: Many-body localization, thermalization, and entanglement. *Rev. Mod. Phys.* 91, 021001 (2019).](https://journals.aps.org/rmp/abstract/10.1103/RevModPhys.91.021001)

2. [B. Basko, A. Aleiner, and A. Altshuler, Metal-insulator transition in a weakly interacting many-electron system with localized single-particle states. *Ann. Phys.* 321, 1126 (2006).](https://www.sciencedirect.com/science/article/pii/S0003491605002630?via%3Dihub)

3. [V. Oganesyan and D. V. Huse, Localization of interacting fermions at high temperature. *Phys. Rev. B* 75, 155111 (2007).](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.75.155111)

4. [W. De Roeck and F. Huveneers, Stability and instability towards delocalization in many-body localization systems. *Phys. Rev. B* 95, 155129 (2017).](https://journals.aps.org/prb/abstract/10.1103/PhysRevB.95.155129)

5. [D. J. Luitz, F. Huveneers, and W. De Roeck, How a Small Quantum Bath Can Thermalize Long Localized Chains. *Phys. Rev. Lett.* 119. 150602 (2017).](https://journals.aps.org/prl/abstract/10.1103/PhysRevLett.119.150602)
   
<!-- 3. [R. Nandkishore and D. A. Huse, Many-Body Localization and Thermalization in Quantum Statistical Mechanics. *Annu. Rev. Condens. Matter Phys.* 6, 15 (2015).](https://www.annualreviews.org/doi/abs/10.1146/annurev-conmatphys-031214-014726) -->
