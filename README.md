# Reconstruction of Complex Networks: Missing and Spurious Interactions

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![NetworkX](https://img.shields.io/badge/NetworkX-Complex%20Networks-orange.svg)](https://networkx.org/)

A Bayesian computational framework for inferring missing interactions (false negatives) and identifying spurious links (false positives) in noisy, incomplete complex network datasets, based on Stochastic Block Models (SBM) and Markov Chain Monte Carlo (MCMC) partition sampling (Guimerà & Sales-Pardo, 2009).

---

## 📌 Overview & Theory

Real-world complex networks across biological, social, and technological domains are often observed through noisy or incomplete measurements. Standard network metrics (degree distributions, clustering, path lengths) are highly sensitive to unobserved links or false interactions.

This repository implements Bayesian network inference by sampling network partitions $P$ from the ensemble of possible block models. The log-likelihood score $\mathcal{H}(P)$ of a partition $P$ given an observed network $A^O$ is derived from the link densities between groups $\alpha$ and $\beta$:

$$\mathcal{H}(P) = -\ln P(A^O | P) = \sum_{\alpha \le \beta} \left[ \ln(l_{\alpha\beta} + 1) + \ln(r_{\alpha\beta} - l_{\alpha\beta} + 1) - \ln(r_{\alpha\beta} + 2) \right]$$

where:
- $l_{\alpha\beta}$ is the number of observed edges between node group $\alpha$ and group $\beta$.
- $r_{\alpha\beta}$ is the maximum possible number of edges between group $\alpha$ and group $\beta$.

Using **Metropolis-Hastings MCMC sampling**, we estimate the marginal reliability $R_{ij}$ for every node pair $(i, j)$:

$$R_{ij} = \left\langle \frac{l_{\sigma(i)\sigma(j)} + 1}{r_{\sigma(i)\sigma(j)} + 2} \right\rangle_{P}$$

Unobserved pairs $(i, j) \notin E_{obs}$ with high $R_{ij}$ are predicted as **missing links**, whereas observed links $(i, j) \in E_{obs}$ with low $R_{ij}$ are flagged as **spurious links**.

---

## 🔬 Features & Implementation Details

- **MCMC Partition Sampler (`Metro`)**: Metropolis-Hastings Markov Chain Monte Carlo algorithm exploring the space of network group partitions.
- **Link Perturbation Engine (`link_remover`)**: Synthetic noise generator to benchmark reconstruction accuracy by stripping specified percentages of edges.
- **Marginal Reliability Matrix ($R_{ij}$)**: Computes posterior connection probabilities across partition ensembles.
- **Performance Benchmarking (`acc`)**: Evaluates link prediction performance against ground-truth adjacency matrices ($A$) vs. noisy matrices ($A_f$).

---

## 📁 Repository Structure

```
mojee13-reconstruction-of-complex-networks/
├── mojtaba.ipynb                                  # Main computational notebook & MCMC implementation
├── A.npy                                          # Ground-truth network adjacency matrix
├── Af.npy                                         # Perturbed / filtered adjacency matrix (missing links)
├── Guimera_reconstruction_complex_networks (1).pdf # Reference paper (Guimerà & Sales-Pardo, 2009)
├── README.md                                      # Project documentation
└── .gitignore                                     # Git ignore configuration
```

---

## 🛠️ Installation & Setup

### Prerequisites
- Python 3.8+
- Jupyter Notebook / JupyterLab

### Dependencies
Install the required packages:

```bash
pip install numpy scipy matplotlib networkx tqdm
```

---

## 🚀 Usage Guide

1. Clone the repository:
   ```bash
   git clone https://github.com/mojee13/mojee13-reconstruction-of-complex-networks.git
   cd mojee13-reconstruction-of-complex-networks
   ```

2. Open the analysis workspace:
   ```bash
   jupyter notebook mojtaba.ipynb
   ```

3. Run notebook cells to:
   - Load ground truth `A.npy` and perturbed `Af.npy` adjacency matrices.
   - Run Metropolis-Hastings MCMC partition sampling.
   - Calculate node-pair reliability matrices $R_{ij}$.
   - Rank unobserved pairs and plot reconstruction precision/ROC curves.

---

## 📚 References & Literature

- **Guimerà, R., & Sales-Pardo, M. (2009)**. *Missing and spurious interactions and the reconstruction of complex networks*. Nature, 461(7266), 963-967.
  📄 Documented in [`Guimera_reconstruction_complex_networks (1).pdf`](./Guimera_reconstruction_complex_networks%20(1).pdf)
