# Benchmarking-TN-v-QC
[dataset] Dataset and plots of the benchmarking between TNs and QCs


In this benchmarking study, we analyze the performance of three classical simulation methods, that is state vector simulation, tensor network, and Pauli propagation, against quantum computation on real hardware. As testbed, we consider three distinct spin models: the transverse-field Ising model, the XXX random-bond model, and the Kitaev model with an external field. Below, we introduce these model and their Hamiltonians, including the interactions and topologies they define. We show a schematic of the models considered in the figure below.

![alt text](plots/schematic_models.png?raw=true)

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

### Kitaev results

![alt text](plots/kitaev_results.png?raw=true)

We now benchmark the classical simulation methods against quantum simulation for the three models previously discussed: transverse-field Ising, XXX random-bond and honeycomb Kitaev model with external field. For each simulation method, the task is to start with an initial state, perform time evolution of a Hamiltonian before evaluating the expectation value of some observable. To specify, we have a Hamiltonian $\hat{H} = \sum_i c_i \hat{h}_i$, and we wish to calculate $\langle \hat{O} \rangle = \langle \psi|\hat{O}|\psi\rangle$, where

```math
|\psi\rangle = \left(\prod_i e^{-i\Delta_t c_i \hat{h}_i}\right)^r|\psi_0\rangle
```
Here, $|\psi_0\rangle$ is the initial state, $\Delta_t$ is some timestep and $r$ is the number of trotter steps. It is important to note that for the Ising we are performing a first-order Trotter-type evolution; in other words, in each Trotter step we perform all the $ZZ$ rotations before applying the $X$ rotations. Similarly, for the Kitaev model we perform all $XX$ rotations, followed by $YY$ then finally the $ZZ$ and $Z$ rotations. This contrasts with the random XXX model, where we perform each two-body evolution based on the lexicographic ordering of the lattice bonds, rather than grouping the rotations into sets of $XX$, $YY$ and $ZZ$. For example, the randomisation may make enforce us to perform a $ZZ$ rotation on qubits (1,2) followed by a $YY$ rotation on qubits (1,5) followed by a $XX$ rotation on qubits (2,3), etc.

For the Kitaev Hamiltonian, we define the model on a ladder of 5 plaquettes (22 sites), with $J=1$, $hz=0.001$ and $\Delta_t=0.1$. For the Kitaev simulations we measure the total magnetisation (normalised), the nearest neighbour, the furthest neighbour correlator, and the sum of correlators on every bond (also normalised).

For each model, we compare statevector simulation with finite shots, Pauli Propagation at different minimum coefficient thresholds, tensor networks at different bond dimensions and quantum hardware implementations. Our results are shown in the figure above.

### Kitaev resources

![alt text](plots/kitaev_resources.png?raw=true)

As a further comparison, we record the resources (time and memory) used for the different simulation methods, and these results are shown in the above figure. Focusing on Pauli Propagation, we generally see an exponential increase in both time and memory as the number of Trotter steps increases. Increasing the depth of the evolution circuit increases the number of Pauli strings created within the Heisenberg evolution of the observable, resulting in a propagation taking increasing spatial and temporal resources. 

For the given system size, the Kitaev model was simulable with Pauli propagation with the default minimum coefficient threshold $10^{-10}$, but the memory estimates at a large number of Trotter steps (over 10 GB) suggests that the limit for near-exact Pauli Propagation simulation may have been reached. Indeed, for the Kitaev model with an extra plaquette (28 sites), using the default $10^{-10}$ threshold also gives rise to an "out of memory" error.

