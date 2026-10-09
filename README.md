# Bridging the Micro-Macro Gap in Hyper-Dense Crowd Dynamics: Trajectory Dataset

[![Format: CSV](https://img.shields.io/badge/Format-CSV-blue.svg)](#dataset-schema-and-features)
[![Status: Pre-print / Under Review](https://img.shields.io/badge/Status-Under%20Review-orange.svg)](#citation)
[![Domain: Crowd Dynamics](https://img.shields.io/badge/Domain-Pedestrian%20Dynamics-green.svg)](#overview)

This repository provides open-access benchmark trajectory datasets modeling hyper-dense crowd circumambulation within the authentic geometric boundaries of the Mataf concourse. Generated via a bi-directionally coupled microscopic-macroscopic simulation architecture, these records model up to 30,000 concurrent, demographically heterogeneous individuals across up to 40,000 discrete integration steps ($\Delta t = 0.05\,\text{s}$).

---

## Dataset Card

### Overview & Metadata
- **Domain:** Extreme-density crowd dynamics, pedestrian simulation, circumambulation geometry.
- **Spatial Scale:** Coordinate domain within the authentic polygonal boundary constraints of the Mataf concourse ($1000 \times 1000$ coordinate reference frame, centered at $(506, 479)$).
- **Temporal Discretization:** Integration step $\Delta t = 0.05\,\text{s}$.
- **Populations:** Three scaled experimental cohorts (10,000, 20,000, and 30,000 agents) partitioned into sequential shards for efficient distributed handling.
- **Demographics:** Mass distributions ($30.0\,\text{kg} \le m_i \le 100.0\,\text{kg}$) and gender-parameterized stride limitations ($s_{\max} \in \{0.670, 0.762\}\,\text{m}$).

### File Inventory

| Filename | Population ($N$) | Total Steps | Spatial Bounds | Observed Dynamic Regimes |
| :--- | :--- | :--- | :--- | :--- |
| `agents_trajectories_10000_25000_1.csv` | 10,000 agents | 25,000 steps (Part 1/2) | Mataf Boundary | Free-flow dominant, unconstrained streaming |
| `agents_trajectories_10000_25000_2.csv` | 10,000 agents | 25,000 steps (Part 2/2) | Mataf Boundary | Free-flow dominant, unconstrained streaming |
| `agents_trajectories_20000_35000_1.csv` | 20,000 agents | 35,000 steps (Part 1/4) | Mataf Boundary | Transition regime, structured lane emergence |
| `agents_trajectories_20000_35000_2.csv` | 20,000 agents | 35,000 steps (Part 2/4) | Mataf Boundary | Transition regime, structured lane emergence |
| `agents_trajectories_20000_35000_3.csv` | 20,000 agents | 35,000 steps (Part 3/4) | Mataf Boundary | Transition regime, structured lane emergence |
| `agents_trajectories_20000_35000_4.csv` | 20,000 agents | 35,000 steps (Part 4/4) | Mataf Boundary | Transition regime, structured lane emergence |
| `agents_trajectories_30000_40000_1.csv` | 30,000 agents | 40,000 steps (Part 1/7) | Mataf Boundary | Hyper-dense packing, contact-dominated bottlenecks |
| `agents_trajectories_30000_40000_2.csv` | 30,000 agents | 40,000 steps (Part 2/7) | Mataf Boundary | Hyper-dense packing, contact-dominated bottlenecks |
| `agents_trajectories_30000_40000_3.csv` | 30,000 agents | 40,000 steps (Part 3/7) | Mataf Boundary | Hyper-dense packing, contact-dominated bottlenecks |
| `agents_trajectories_30000_40000_4.csv` | 30,000 agents | 40,000 steps (Part 4/7) | Mataf Boundary | Hyper-dense packing, contact-dominated bottlenecks |
| `agents_trajectories_30000_40000_5.csv` | 30,000 agents | 40,000 steps (Part 5/7) | Mataf Boundary | Hyper-dense packing, contact-dominated bottlenecks |
| `agents_trajectories_30000_40000_6.csv` | 30,000 agents | 40,000 steps (Part 6/7) | Mataf Boundary | Hyper-dense packing, contact-dominated bottlenecks |
| `agents_trajectories_30000_40000_7.csv` | 30,000 agents | 40,000 steps (Part 7/7) | Mataf Boundary | Hyper-dense packing, contact-dominated bottlenecks |

---

## Dataset Schema and Features

Each shard is organized in a comma-separated format (`.csv`) with the schema detailed below:

```csv
Agent,Gender,Mass,Velocity,Model Step,X,Y
0,0,50.000000,0.670,0,264.86685,705.53080
1,1,62.724014,0.762,0,493.20062,856.90220
2,0,78.000000,0.670,0,554.47340,123.59248
3,0,30.000000,0.670,0,263.75867,711.60060
4,0,30.000000,0.670,0,482.34200,849.22930
```

### Feature Descriptions

| Field | Data Type | Physical Units | Description |
| :--- | :--- | :--- | :--- |
| `Agent` | Integer | ID index | Unique numerical pedestrian identifier active in the simulation pool. |
| `Gender` | Categorical | Binary (`0` or `1`) | Demographic classification (`0` = male, `1` = female), specifying stride length bounds ($0.670\,\text{m}$ vs $0.762\,\text{m}$). |
| `Mass` | Float | Kilograms ($\text{kg}$) | Individual body mass ($30.0\text{--}100.0\,\text{kg}$). Sets personal body envelope radius $r_i = 0.3 + \frac{m_i}{200}$. |
| `Velocity` | Float | Meters per second ($\text{m/s}$) | Instantaneous scalar speed evaluated at the active integration step. |
| `Model Step` | Integer | Iteration index | Discrete temporal integration index ($\text{Physical Time } t = \text{Model Step} \times 0.05\,\text{s}$). |
| `X` | Float | Meters ($\text{m}$) | Concourse Cartesian horizontal coordinate. |
| `Y` | Float | Meters ($\text{m}$) | Concourse Cartesian vertical coordinate. |

---

## Macro Calibration & Empirical Validation

The simulation models collective transport dynamics calibrated against empirical crowd flow formulations (e.g., Weidmann's macroscopic speed-density relation) using dynamic Voronoi polygon tessellation ($d_i = 1 / |\mathcal{V}_i|$):

$$v_{\text{empirical}}(d) = v_0 \left(1 - \exp\left[-\lambda \left(\frac{1}{d} - \frac{1}{d_{\max}}\right)\right]\right)$$

- Unconstrained free-flow speed: $v_0 = 1.34\,\text{m/s}$
- Physical jam density: $d_{\max} = 5.4\,\text{ped/m}^2$
- Empirical shape parameter: $\lambda = 1.913\,\text{ped/m}^2$

### Summary Benchmark Metrics

| Evaluation Metric | Free-Flow / Intermediate (10k--20k) | Hyper-Dense Regime (30k Cohort) | Unified Global Domain ($N = 449{,}041$) |
| :--- | :--- | :--- | :--- |
| **Voronoi Density Span** | $0.15\text{--}0.60\,\text{ped/m}^2$ | $0.75\,\text{ped/m}^2$ | $4.85\,\text{ped/m}^2$ |
| **Macroscopic RMSE** | $0.54\text{--}0.61\,\text{m/s}$ | $0.52\,\text{m/s}$ | $0.313\,\text{m/s}$ |
| **Macro $R^2$** | N/A (Free drift window) | N/A (Free drift window) | $0.422$ |
| **Wasserstein Divergence ($\mathcal{W}$)** | $0.61\text{--}0.62\,\text{m/s}$ | $0.599\,\text{m/s}$ | $0.604\,\text{m/s}$ |

---

## Getting Started

### 1. Requirements

Install required dependencies:

```bash
pip install pandas numpy scipy matplotlib scikit-learn
```

### 2. Loading and Filtering Trajectories

```python
import glob
import pandas as pd

# Load an entire cohort across its partitioned shards (e.g., 10k cohort)
cohort_files = sorted(glob.glob("agents_trajectories_10000_25000_*.csv"))
df = pd.concat((pd.read_csv(f) for f in cohort_files), ignore_index=True)

print(f"Loaded {len(df):,} total records across {len(cohort_files)} shards.")
print(df.head())

# Filter Agent 0 trajectory across time
agent_0_trajectory = df[df["Agent"] == 0].sort_values("Model Step")

# Extract steady-state spatial frame at step 20,000
steady_state_frame = df[df["Model Step"] == 20000]
print(f"Active agents in concourse at step 20,000: {len(steady_state_frame):,}")
```

### 3. Computing Instantaneous Voronoi Density

```python
import numpy as np
from scipy.spatial import Voronoi

def compute_step_voronoi_density(frame_df):
    coords = frame_df[["X", "Y"]].to_numpy(dtype=float)
    jitter = 1e-5 * np.random.randn(*coords.shape)
    vor = Voronoi(coords + jitter)
    densities = np.zeros(len(coords))
    
    for i, reg_idx in enumerate(vor.point_region):
        region = vor.regions[reg_idx]
        if not region or -1 in region:
            densities[i] = 1.0 / 25.0
            continue
        polygon = vor.vertices[region]
        x_p, y_p = polygon[:, 0], polygon[:, 1]
        area = 0.5 * np.abs(np.dot(x_p, np.roll(y_p, 1)) - np.dot(y_p, np.roll(x_p, 1)))
        densities[i] = 1.0 / np.clip(area, 0.12, 25.0)
        
    return np.clip(densities, 0.04, 7.5)
```

---

## Intended Use and Scope

- **Applicability:** Benchmarking multi-agent trajectory prediction, validating macroscopic crowd continuum formulations, analyzing lane formation and self-organization entropy, and evaluating bottleneck bypass algorithms.
- **Limitations:** The trajectories are simulated via a hybrid microscopic social force and macroscopic Eulerian continuum model rather than extracted from optical CCTV surveillance trackers. Kinematic maximum speeds reflect model stride parameters.

---

## License

This dataset is released under the [MIT License](LICENSE).

---

## Citation

If you use this dataset or the simulation methodology in your research, please cite:

```bibtex
@article{rahim2026bridging,
  title={Bridging the Micro-Macro Gap in Hyper-Dense Crowd Dynamics},
  author={Rahim, Mussadiq Abdul},
  journal={Preprint / Submitted for Publication},
  year={2026}
}
```
