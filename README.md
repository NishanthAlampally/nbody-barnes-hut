# N-Body Simulation: Barnes–Hut & GPU Acceleration

A 3D gravitational N-body simulator built from scratch in Python, exploring numerical integration, algorithmic optimisation, parallel computing, and GPU acceleration.

The project progresses from a brute-force gravitational simulation to the Barnes–Hut approximation, with implementations using NumPy, Numba, and CUDA. It investigates the trade-offs between computational complexity, numerical accuracy, and performance as the number of bodies increases.

## Features

- **Gravitational simulation:** Computes the mutual gravitational forces between bodies in three-dimensional space.
- **Leapfrog integration:** Evolves positions and velocities over time using a symplectic numerical integration method.
- **Brute-force algorithm:** Implements direct pairwise force calculations as a reference for validating optimised methods.
- **Barnes–Hut algorithm:** Uses a hierarchical octree to approximate distant gravitational interactions, targeting \(O(N\log N)\) computational complexity.
- **CPU acceleration:** Uses Numba JIT compilation and parallel execution to accelerate numerical computations.
- **GPU acceleration:** Implements a CUDA kernel for parallel brute-force force calculations.
- **Numerical validation:** Investigates energy conservation, orbital trajectories, and agreement between different force-computation methods.

## Methods and Implementation

### 1. Brute-Force Simulation

The reference implementation calculates the gravitational force between every pair of bodies. Its computational complexity is \(O(N^2)\) per force evaluation.

This implementation serves as a baseline for validating the correctness of optimised algorithms.

### 2. Leapfrog Integration

The leapfrog integrator updates the positions and velocities of bodies over discrete time steps. It is used to investigate the stability of simulated trajectories and the conservation of mechanical energy over time.

### 3. Barnes–Hut Algorithm

The Barnes–Hut method organises bodies into a hierarchical octree. Distant groups of bodies can be approximated using their combined mass and centre of mass, reducing the number of individual interactions that must be evaluated.

The approximation is controlled by the opening-angle parameter \(\theta\), which determines when a node can be treated as a single distant mass.

The notebook explores both conventional and optimised implementations of the Barnes–Hut force calculation.

### 4. CPU Parallelisation with Numba

Numba is used to compile numerical functions and parallelise independent force calculations. Array-based representations of positions and masses reduce reliance on Python objects in performance-critical computations.

### 5. CUDA Acceleration

A CUDA kernel assigns individual bodies to GPU threads. Each thread computes the gravitational force acting on one body by iterating over the other bodies, allowing independent force calculations to execute in parallel.

The notebook also contains an experimental CUDA implementation of the Barnes–Hut method, which remains a work in progress.

## Units and Assumptions

The simulation uses normalised units:

- Gravitational constant: \(G=1\)
- Positions: normalised distance units
- Masses: normalised mass units
- Time: normalised time units

The simulation assumes Newtonian gravitational interactions. The current implementation does not include gravitational softening or relativistic corrections.

## Validation

The project uses several approaches to investigate correctness and numerical stability:

- **Figure-eight three-body orbit:** Tests whether the numerical integrator reproduces the characteristic periodic trajectory of a known three-body configuration.
- **Energy conservation:** Tracks kinetic energy, potential energy, and total mechanical energy throughout a simulation.
- **Cross-implementation comparisons:** Compares force calculations from the original brute-force implementation with optimised CPU and GPU implementations.
- **Barnes–Hut comparisons:** Uses direct force calculations as a reference for evaluating the accuracy of the tree-based approximation, including the \(\theta=0\) limiting case.

These tests help distinguish implementation errors from numerical integration error and approximation error.

## Performance

The project investigates how computational performance changes with the number of bodies and the choice of algorithm and hardware.

A preliminary benchmark reported the following single-run timings for the brute-force implementations:

| Implementation | Bodies | Reported runtime |
|---|---:|---:|
| Parallel CPU | 10,000 | ~12 s |
| CUDA GPU | 10,000 | ~0.4 s |

This corresponds to an approximate **30× speedup** in the reported test.

These figures are preliminary, hardware-dependent results rather than definitive benchmark measurements. Reproducible comparisons require identical inputs, consistent timing boundaries, GPU synchronisation, and repeated runs. Kernel execution time should also be distinguished from host-to-device and device-to-host transfer time.

The Barnes–Hut method is intended to improve scalability beyond the quadratic cost of direct pairwise force computation.

## Project Structure

The current project is organised as a Jupyter notebook containing the implementations, experiments, and comparisons.

| Section | Contents |
|---|---|
| Brute-force without Numba | Reference implementation using Python objects |
| Brute-force with Numba | Compiled and parallelised CPU implementations |
| Brute-force with CUDA | GPU-accelerated direct force computation |
| Simulation visualisation | Plotting simulated trajectories |
| Barnes–Hut without Numba | Reference octree-based implementation |
| Barnes–Hut with Numba | Optimised tree traversal and force computation |
| Barnes–Hut with CUDA | Experimental GPU implementation |
| Method comparisons | Comparisons between simulation approaches |

## Requirements

- Python 3
- NumPy
- Matplotlib
- Numba
- Plotly
- Jupyter Notebook
- A CUDA-compatible NVIDIA GPU and compatible CUDA software stack for GPU experiments

Some GPU functionality requires additional hardware and software configuration. CPU-only experiments can be run without a CUDA-compatible GPU.

## Getting Started

1. Clone the repository:

   ```bash
   git clone <YOUR_REPOSITORY_URL>
   cd <YOUR_REPOSITORY_NAME>
   ```

2. Create and activate a virtual environment:

   ```bash
   python -m venv .venv
   ```

   On Windows:

   ```bash
   .venv\Scripts\activate
   ```

   On macOS or Linux:

   ```bash
   source .venv/bin/activate
   ```

3. Install the dependencies:

   ```bash
   pip install numpy matplotlib numba plotly jupyter
   ```

4. Launch Jupyter:

   ```bash
   jupyter notebook
   ```

5. Open `main.ipynb` and execute the notebook sections in order. CUDA sections require a compatible GPU environment.

## Current Status

- [x] Brute-force gravitational force computation
- [x] Leapfrog numerical integration
- [x] Three-body figure-eight simulation
- [x] Energy diagnostics
- [x] Barnes–Hut octree implementation
- [x] Numba-accelerated force computation
- [x] CUDA brute-force force-computation kernel
- [x] Initial CPU/GPU performance comparisons
- [ ] Comprehensive, repeatable benchmarking across multiple body counts
- [ ] Complete and validate the CUDA Barnes–Hut implementation
- [ ] Further optimisation and scalability analysis

## Limitations and Future Work

- The brute-force algorithm scales quadratically with the number of bodies.
- Barnes–Hut accuracy depends on the opening-angle parameter and tree implementation.
- Gravitational softening is not currently implemented.
- The CUDA Barnes–Hut implementation is experimental and requires further validation.
- Benchmark results need systematic repetition and consistent measurement of computation and data-transfer overhead.
- Further testing is needed across different body counts, hardware configurations, and integration time steps.

## Acknowledgements

This project was developed as an exploration of computational physics, numerical methods, algorithmic optimisation, and heterogeneous computing.

## License

This project is intended for educational and research purposes. See the `LICENSE` file for licensing terms.
