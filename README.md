# Benchmarking-TN-v-QC
[dataset] Dataset and plots of the benchmarking between TNs and QCs


In this benchmarking study, we analyze the performance of three classical simulation methods, that is state vector simulation, tensor network, and Pauli propagation, against quantum computation on real hardware. As testbed, we consider three distinct spin models: the transverse-field Ising model, the XXX random-bond model, and the Kitaev model with an external field. Below, we introduce these model and their Hamiltonians, including the interactions and topologies they define. We show a schematic of the models considered in the figure below.

![alt text](schematic_models.png?raw=true)

### Transverse-field Ising model
The transverse-field Ising model (TFIM) is one of the fundamental models in quantum many-body physics and serves as a paradigm for quantum phase transitions. The model is described by the Hamiltonian
```math
H_{\text{Ising}} = -J \sum_{\langle i,j \rangle} \sigma_i^z \sigma_j^z - h_x \sum_i \sigma_i^x ,
```
where $\sigma_i^{x, z}$ are Pauli matrices acting on site $i$, $J$ is the interaction strength between nearest-neighbor spins, and $h_x$ represents the strength of the external transverse field. The model is typically defined on a one-dimensional (1D) chain or a two-dimensional (2D) lattice, where the nearest-neighbor interaction $\langle i,j \rangle$ depends on the underlying topology. In our work, we consider models with open boundary conditions, hence with non-periodic topologies.

### XXX random-bond model
The XXX random-bond model is a variation of the Heisenberg model, incorporating randomness in the interactions. The system is described by the Hamiltonian
```math
H_{\text{rand}} = - J \sum_{\substack{\langle i,j \rangle_k \in \mathcal{I}_k \\ k \in \{x, y, z\}}} \sigma_i^k \sigma_j^k ,
```
where the interaction strengths $J$ relative to the different bonds directions are equally weighted (hence the XXX name). The union of the sets of random bonds in the different directions $\mathcal{I}\_i$, $\mathcal{I}\_y$, and $\mathcal{I}\_z$, defines the full set of nearest-neighbor interactions $\mathcal{I}$. The total number of bonds $N_\text{bonds}$ is evenly partitioned among $X$, $Y$, and $Z$, ensuring equal representation, i.e.
```math
\dim(\mathcal{I}_k)=N_\text{bonds}/3
```
where $k\in\{x,y,z\}$. This model is relevant for studying localization phenomena and quantum transport in disordered spin systems. In the 2D lattice we consider, the randomness of the bond type introduces disorder into the system breaking translational invariance. This translates into a highly unstructured system where classical methods cannot easily exploit symmetries for efficient computation.

### Kitaev model with external field
The Kitaev model is a highly anisotropic spin model that exhibits topological order and fractional excitations. We consider the model with an additional external magnetic field along the $z$-direction, with the following Hamiltonian,
```math
H_{\text{Kitaev}} = -J \sum_{\substack{\langle i,j \rangle_k \in \mathcal{K}_k \\ k \in \{x, y, z\}}} \sigma_i^k \sigma_j^k  - h_z \sum_i \sigma_i^z .
```
Here, the summations $\langle i,j \rangle_{k}$ run over nearest neighbors along three bond directions of a honeycomb lattice defining the bond sets $\mathcal{K}_k$, and $h_z$ represents the strength of the external field. Note that, although the interaction terms in the Kitaev Hamiltonian resemble those in the XXX random-bond Hamiltonian, the two models are noticeably different. In fact, while the XXX random-bond model defines a square lattice whose bonds are evenly partitioned but highly unstructured, the Kitaev model defines a honeycomb lattice whose bonds are evenly partitioned and follow a well-structured topology. Despite its structure, the introduction of the external field breaks the exact solvability of the pure Kitaev model but allows for a richer phase structure and connections to experimental realizations in materials with strong spin-orbit coupling.
