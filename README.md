# 📌 **Navigation**

- [src](src/): all files that run the code
- [dashboard](dashboard/): files that use the code, produce data, and plot figures
- [examples](dashboard/examples/): notebooks that explain in detail the basic way to run the code
- [recreate_data](dashboard/recreate_data/): code used to create and plot each figure in the paper
- [requirements](requirements.txt): info on packages required to run the code
- [docs](docs/): an `html` index for the entire code documentation

Please don't delete or displace the [setup](setup.py) and [init](src/__init__.py) files.

# 📖 **Introduction**

This code is built to perform exact calculations and variational simulations of quantum spins on a 2D lattice, using Neural Quantum States (NQS).

It is primarily used to calculate the dynamics of the Heisenberg model of interacting quantum spins in a rectangular lattice. Each of the $N$ lattice sites can be either in the $+$ or $-$ state; therefore, there are $2^N$ possible configurations. The spins are given in computational units of $\pm 1$, while energy (and frequency) are in the units of exchange interaction.

The Neural Quantum States simulations are performed with the Restricted Boltzmann Machine (RBM) neural network as a variational ansatz:
$\Psi (s) = \prod_{j=1}^M 2\cosh{(\theta_j (s))}$. Here, $M = \alpha N$ defines the expressivity of the neural network, $s$ is a configuration of the lattice, and $\theta_j = b_j + \sum_i s_i^z w_{ij}$ includes the biases $b_j$ and weights $w_{ij}$ of the network.

This repository was constructed as a supplement to the following [paper](https://doi.org/10.48550/arXiv.2507.17421). In this guide, we will explain how to enable the use of the code, provide a description of the code structure, and give a few examples of its usage.

# 💽 **Installation**

To use the code, it is highly recommended that you use a Python virtual environment. The calculations in this repository have been done with Python version `3.10.11`.

After you've created the virtual environment, you can install all the packages from the [requirements](requirements.txt) file. If you're using **pip**, this can be done by:
```
pip install -r requirements.txt
```

This will also make the files in the `src` folder visible everywhere in the environment, and to intellisense.

> ⚠️ Without this, the provided notebooks will not work. Please make sure that the files from `src` are available if they are being used.

# 🔨 **Basic use**

The code's main function is to perform three types of calculations:
- exact diagonalization (ED) dynamics following the Schrödinger equation,
- ground state optimization by gradient descent,
- dynamics with the Time Dependent Variational Principle (TDVP).

Additionally, it's possible to represent a series of ED wave functions with an RBM ansatz, using infidelity optimization.

> 📝 **Examples**
>- for basic calculations, please see the [dynamics](dashboard/examples/dynamics.ipynb) notebook.
>- for the infidelity optimization, please see the [infidelity](dashboard/examples/infidelity.ipynb) notebook.

>⚠️ **Note**
>
> Physical quantities are calculated by fully summing the Hilbert space of the problem. Therefore, this code is limited to only a small number of lattice sites - around $12$, depending on you computer's RAM.

# 🗃️ **Code structure**

This code's functionality is divided between six `.py` files. 

Out of those, three are built to perform calculations:
>- `exact`: contains the methods to run the exact diagonalization calculations,
>    - primary class is `ED`,
>- `groundstate`: performs the gradient descent to find the ground state,
>    - primary class is `descent`,
>- `dynamics`: does the TDVP calculation of dynamics
>    - primary class is `evolution`

The other three are built to enable and support the performing calculations, providing some basic mathematical elements:
>- `hilbert`: contains all the information about the system dimensions, model, and observables,
>    - primary classes are `lattice` and `space`,
>- `full`: builds all the elements of the TDVP equation of motion, like the $S$-matrix and >the energy gradient,
>    - primary class is `sampler`,
>- `rbm`: provides access to the Restricted Boltzmann Machine ansatz,
>    - primary class is also called `rbm`

For full documentation, please refer to [this](https://hvrcan.github.io/lattice_nqs/src.html) site.

## Options of note

The code provides many options for performing the above-mentioned calculations. Notably, some options for numerical time integration include:
- two integrators: `'heun'` and `'implicit_midpoint'`,
- three different formulations: `'regularization'`, `'diagonalization'`, and `'geometric'`.

These options are often analysed in the mentioned [paper](https://doi.org/10.48550/arXiv.2507.17421).

# ✍️ **Authors and acknowledgments**

This code was written by Hrvoje Vrcan, supervised by Johan H. Mentink, for research into numerical instabilities in Neural Quantum States.
