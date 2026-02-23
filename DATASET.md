# TMCAD Dataset

This document introduces the **TMCAD (Truly Mechanical CAD)** dataset used in this project.

## Dataset Versions

- **TMCAD v2** (2025.11.02) - Cleaned and verified collection
- **TMCAD v1** (2025.02.10) - Original release

---

## Dataset Description

TMCAD v2 contains **9,799 CAD models** spanning **10 standardized mechanical part categories**.  
All models are provided in **STEP (.stp)** format with standardized naming conventions.

### Category Distribution

| Category    | # of Models |
|-------------|------------:|
| Bearing     |         857 |
| Bolt-Screw  |       2,070 |
| Bracket     |       1,102 |
| Flange      |         979 |
| Gear        |         962 |
| Nut         |         899 |
| Shaft       |         893 |
| Coupling    |         474 |
| Pulley      |         544 |
| Spring      |       1,019 |
| **Total**   |   **9,799** |

### Improvements in v2
- Removed mistakenly collected samples
- Corrected wrong or ambiguous class labels
- Verified and repaired invalid geometries
- Merged visually similar classes (`bolt` + `screw`)
- Ensured clean hierarchical directory structure

---

## Download Links

| Version | Date | Download Link |
|---------|------|---------------|
| TMCAD v2 | 2025.11.02 | [Download v2](https://pan.zju.edu.cn/share/218d10a88e8c18f5b96e94a7e0) |
| TMCAD v1 | 2025.02.10 | [Download v1](https://pan.zju.edu.cn/share/305e1697a37277e6a9ec60dded) |

---

## Directory Structure

After downloading and extracting, organize the data as follows:

```
data/
└── TMCAD_v2/
    ├── bearing/
    │   ├── bearing_0.stp
    │   ├── bearing_1.stp
    │   └── ...
    ├── bolt-screw/
    │   ├── bolt_0.stp
    │   ├── bolt_1.stp
    │   └── ...
    ├── bracket/
    ├── flange/
    ├── gear/
    ├── nut/
    ├── shaft/
    ├── coupling/
    ├── pulley/
    └── spring/
        ├── spring_0.stp
        ├── spring_1.stp
        └── ...
```

---

## Usage in Code

### 1. Set the dataset path in your notebook/config

```python
from pathlib import Path

# Update this path to where you extracted TMCAD
DATASET_ROOT = Path("data/TMCAD_v2")
```

### 2. Load samples by category

```python
# Load all bearing models
bearing_files = list((DATASET_ROOT / "bearing").glob("*.stp"))

# Or pick random samples per class
import random
samples = []
for class_dir in sorted(DATASET_ROOT.iterdir()):
    if class_dir.is_dir():
        files = list(class_dir.glob("*.stp"))
        if files:
            samples.append(random.choice(files))
```

### 3. Process a single model

```python
from step_to_graph import load_step, brep_to_dgl_graph

shape = load_step("data/TMCAD_v2/bearing/bearing_0.stp")
graph, centroids = brep_to_dgl_graph(shape, N=10)
```

---

## Applications

TMCAD v2 supports various 3D deep learning tasks:

- **Classification** — Predict part category from B-Rep
- **Segmentation** — Label faces as features (pockets, holes, etc.)
- **Retrieval** — Find similar mechanical parts
- **Generation** — Learn to generate valid B-Reps
- **Self-supervised learning** — Pre-train on large CAD collections

---

## License

The dataset is released under **GPL-3.0 license**.

---

## Citation / Attribution

This project uses the **TMCAD (Truly Mechanical CAD) v2** dataset, created and released by **Zou & Zhu (2025)**. Please acknowledge the dataset authors if you use it in your work:

```bibtex
@article{zou2025bringing,
  title={Bringing attention to CAD: Boundary representation learning via transformer},
  author={Zou, Qiang and Zhu, Lizhen},
  journal={Computer-Aided Design},
  pages={103940},
  year={2025},
  publisher={Elsevier}
}
```