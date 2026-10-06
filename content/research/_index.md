---
title: "Research"
showAuthor: false
showDate: false
showReadingTime: false
showComments: false
layout: "simple"
---

## Overview

I study quantum algorithms for many-body state preparation, simulation, and optimization, trying to understand how physical structure can be used to guide algorithm and circuit design. My current work focuses on:

- **Gibbs-state preparation**

  - **[Spectral core-tail architecture](https://arxiv.org/abs/2609.09291).** Combines a structured thermal core with a unitary tail, yielding local-error bounds under explicit locality and thermal-response assumptions. Moreover, we provide a systematic Hamiltonian-residual reduction procedure for treating weak deformations around an exactly solvable Hamiltonian. 
  - **[Matrix-product-state-assisted variational preparation](https://arxiv.org/abs/2510.23546).** Uses matrix product states to optimize thermal purification circuits without full-statevector storage. Our benchmarks reveal trade-offs between thermal accuracy, circuit entropy capacity, and the classical costs of MPS assisted optimization.

- **Quantum optimization for protein structure prediction**

  - **[Face-centered cubic lattice encoding](https://arxiv.org/abs/2507.08955).** Represents protein conformations on an FCC lattice and optimizes them using polynomial-fitting or constrained variational methods. Experiments on noisy IBM processors recovered minimum-energy lattice conformations for a six-residue peptide sequence.
  - **[Problem-agnostic quantum circuits](https://arxiv.org/abs/2509.18263).** Combines quantum sampling with classical energy evaluation, avoiding explicit Hamiltonian construction and ancillary qubits. Benchmarks sequences of up to 26 residues across three different lattice models (tetrahedral, BCC, and FCC), including both first- and second-nearest-neighbor interactions.

My earlier theoretical-physics research examined how symmetry and band topology govern transport in Weyl and Dirac semimetals, including chiral-anomaly-induced nonlinear Hall response and tunable spin-charge conversion. I also studied anomaly-free and leptophobic dark matter models.

{{< alert "email">}}
I welcome collaborations and discussions. Please feel free to contact me through the links on my [homepage](/).
{{< /alert >}}

---

## Publications

† Equal contribution among the marked authors. \* All authors contributed equally and are listed alphabetically.

1. **R.-H. Li**, [*Spectral Core-Tail Architecture for Locally Certified Gibbs-State Preparation*](https://arxiv.org/abs/2609.09291), arXiv:2609.09291 (2026), under review.
2. K. Zheng, Y. Zhou, **R.-H. Li**, Z. Ding, Z. Liang, and S. Li, [*Q-Score: A Quantum-Native Scoring Function for Molecular Docking*](https://arxiv.org/abs/2607.09737), arXiv:2607.09737 (2026).
3. F. Cumbo, **R.-H. Li**, B. Raubenolt, J. Joshi, A. K. M. Masum, S. Aygun, and D. Blankenberg, [*Quantum hyperdimensional computing: a foundational paradigm for quantum neuromorphic architectures*](https://doi.org/10.1038/s44335-026-00064-6), npj Unconventional Computing **3**, 21 (2026).
4. **R.-H. Li**, S. Valgushev, and K. Najafi, [*Matrix-product-state-assisted variational Gibbs-state preparation*](https://arxiv.org/abs/2510.23546), arXiv:2510.23546 (2025), under review.
5. H. Linn†, **R.-H. Li**†, A. Holden, A. A. Saki, F. DiFilippo, T. Radivoyevitch, D. Blankenberg, L. García-Álvarez, and G. Johansson, [*Efficient Quantum Protein Structure Prediction with Problem-Agnostic Ansatzes*](https://arxiv.org/abs/2509.18263), arXiv:2509.18263 (2025), under review.
6. **R.-H. Li**†, H. Doga†, B. Raubenolt†, S. Mostame, N. DiSanto, F. Cumbo, J. Joshi, H. Linn, M. Gaffney, A. Holden, V. Kulkarni, V. Chaudhary, K. M. Merz Jr, A. A. Saki, T. Radivoyevitch, F. DiFilippo, J. Qin, O. Shehab, and D. Blankenberg, [*Quantum Algorithm for Protein Structure Prediction Using the Face-Centered Cubic Lattice*](https://arxiv.org/abs/2507.08955), arXiv:2507.08955 (2025), under review.
7. X. Li, V. R. Kulkarni, J. Nana, S. Pu, N. Xie, Q. Guan, **R.-H. Li**, S. Zhang, S. Xu, D. Blankenberg, and V. Chaudhary, [*Quantum Circuit Optimization for Protein Structure Prediction*](https://doi.org/10.1109/DSN-W65791.2025.00065), 2025 55th Annual IEEE/IFIP International Conference on Dependable Systems and Networks Workshops (DSN-W), pp. 220–223 (2025).
8. K. Blekos\*, D. Brand\*, A. Ceschini\*, C.-H. Chou\*, **R.-H. Li**\*, K. Pandya\*, and A. Summer\*, [*A review on Quantum Approximate Optimization Algorithm and its variants*](https://doi.org/10.1016/j.physrep.2024.03.002), Physics Reports **1068**, 1–66 (2024).
9. **R.-H. Li**†, P. Shen†, and S. S.-L. Zhang, [*Tunable spin-charge conversion in class-I topological Dirac semimetals*](https://doi.org/10.1063/5.0077431), APL Materials **10**, 041108 (2022).
10. **R.-H. Li**, O. G. Heinonen, A. A. Burkov, and S. S.-L. Zhang, [*Nonlinear Hall effect in Weyl semimetals induced by chiral anomaly*](https://doi.org/10.1103/PhysRevB.103.045105), Physical Review B **103**, 045105 (2021).
11. P. Fileviez Pérez\*, E. Golias\*, **R.-H. Li**\*, C. Murgui\*, and A. D. Plascencia\*, [*Anomaly-free dark matter models*](https://doi.org/10.1103/PhysRevD.100.015017), Physical Review D **100**, 015017 (2019).
12. P. Fileviez Pérez\*, E. Golias\*, **R.-H. Li**\*, and C. Murgui\*, [*Leptophobic dark matter and the baryon number violation scale*](https://doi.org/10.1103/PhysRevD.99.035009), Physical Review D **99**, 035009 (2019).

---

## Talks and panels {#talks}

### Invited talks and panels

- **Lattice-Based Protein Structure Prediction on Near-Term Quantum Computers** — [HAIQ 2026](https://haiq-workshop.org/haiq2026/index.html), Pittsburgh, PA, March 2026 (invited talk).
- **Quantum in Healthcare and Life Sciences** — [Rensselaer Polytechnic Institute Quantum Forum](https://dotcio.rpi.edu/quantum-computer-forum-2025), Troy, NY, April 2025 (panelist).

### Selected talks and tutorials

#### Conference talks

- **Protein Structure Prediction with Quantum Algorithms** — [APS Global Physics Summit](https://meetings-archive.aps.org/smt/2026/mar-p57/6/), Denver, CO, March 2026.
- **MPS-Enhanced Variational Quantum Gibbs State Preparation** — [APS Global Physics Summit](https://meetings-archive.aps.org/smt/2025/mar-g34/13/), Anaheim, CA, March 2025.
- **Tunable Spin-Charge Conversion in Topological Dirac Semimetals** — [APS March Meeting](https://meetings.aps.org/Meeting/MAR22/Session/N52), Chicago, IL, March 2022. [Slides](/files/Ruihao_Li_APS_22.pdf).
- **Tunable Spin-Charge Conversion in Topological Dirac Semimetals** — [Around-the-Clock Around-the-Globe Magnetics Conference (AtC-AtG)](https://ieeemagnetics.org/conferences/atc-atg-conference/atc-atg-2021), virtual, August 2021.
- **Chiral-Anomaly-Induced Nonlinear Hall Effect in Tilted Weyl Semimetals** — [APS March Meeting](https://meetings-archive.aps.org/mar/2021/a45/12/), virtual, March 2021.
- **Chiral-Anomaly-Induced Nonlinear Hall Effect in Weyl Semimetals** — [65th Annual Conference on Magnetism and Magnetic Materials (MMM)](https://events-siteplex.confcats.io/magnetism/wp-content/uploads/sites/82/2022/02/MMM2020_ProgramBook.pdf), virtual, November 2020. [Slides](/files/Ruihao_Li_MMM_20.pdf).
- **Chiral-Anomaly-Induced Nonlinear Hall Effect in Weyl Semimetals** — [Around-the-Clock Around-the-Globe Magnetics Conference (AtC-AtG)](https://ieeemagnetics.org/conferences/atc-atg-conference/atc-atg-2020), virtual, August 2020. **Best Presentation Award.**

#### Outreach and other talks

- **Introduction to (Qiskit) Quantum Machine Learning** — [Qiskit Fall Fest](https://qiskit.org/events/fall-fest/), CWRU, 2022 (co-organizer). [Slides](/files/QML_slides.pdf) · [Demo](https://github.com/Case-Quantum-Computing-Club/CQC-qiskit-fall-fest-22/blob/main/resources/Qiskit_ML/QML_demo.ipynb).
- **Introduction to Quantum Approximate Optimization Algorithm** — [QOSF Mentorship Program](https://qosf.org/qc_mentorship/) meeting, 2022. [Slides](/files/QOSF_Meeting.pdf).
- **Majorana Zero Modes in a Kitaev Chain** — CMP Journal Club, CWRU, Spring 2022. [Slides](/files/CMP_JC_Spring_22.pdf).
- **Fantastic Dark Matter and Where to Find Them: Indirect Detection** — [CERCA](https://cerca.case.edu/) Weekly Seminar, Spring 2019. [Slides](/files/CERCA_Spring_19.pdf).
- **Baryon Number Violation and Leptophobic Dark Matter** — [CERCA](https://cerca.case.edu/) Weekly Seminar, Fall 2018. [Slides](/files/CERCA_Fall_18.pdf).
- **Quantum Corrections in Left-Right Symmetric Seesaw Mechanisms** — Honours final presentation, October 2016. [Slides](/files/Honours_talk_16.pdf).

---

## Miscellaneous Notes

- [Quantum Field Theory in Curved Spacetime](/files/QFT_in_curved_spacetime.pdf) (2018). A short introduction to quantum field theory in curved spacetime.
- [Introduction to Quantum Field Theory](/files/QFT_course_16.pdf) (2016). Lecture notes compiled based on the Honours course on Quantum Field Theory.
- [Particle Cosmology and Baryonic Astrophysics](/files/PCBAP_course_16.pdf) (2016). Lecture notes compiled based on the Honours course on Particle Cosmology and Baryonic Astrophysics.
