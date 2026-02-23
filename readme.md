# CAD B-Rep to Face-Adjacency Graphs

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)

A step-by-step Jupyter notebook that converts CAD STEP files into **face-adjacency graphs** with parametric UV-sampled features, following the **UV-Net (CVPR 2021)** representation for geometric deep learning on boundary representations (B-Rep).

Think of it as: *CAD geometry → structured graph data → ML-ready*, with some visualization and curiosity-driven exploration along the way.

---

## 📖 Overview

This repository provides an educational (and slightly experimental) implementation of the UV-Net preprocessing pipeline, transforming raw CAD models into graph-structured data ready for Graph Neural Networks (GNNs).

Each node in the graph represents a B-Rep face with sampled surface geometry, and edges represent face adjacencies with sampled curve geometry — i.e., the topology your CAD kernel always knew, now packaged for ML.

### What This Project Does

| Input | Processing | Output |
|-------|------------|--------|
| CAD STEP file (.stp) | 1. Extract B-Rep topology<br>2. Build face adjacency graph<br>3. Sample UV grids on faces<br>4. Sample curves on edges<br>5. Package as DGL graph | DGL graph with:<br>• `ndata["x"]`: (N×N×7) face features<br>• `edata["x"]`: (N×6) edge features |

---

## 🎯 Key Features

- **Faithful to UV-Net** — same representation as the CVPR 2021 paper  
- **Educational** — step-by-step notebook with sanity checks and visuals  
- **B-Rep Preservation** — keeps both analytic geometry *and* topology intact  
- **Visual Validation** — 3D overlays + topology plots (because graphs deserve pictures too)  
- **Dataset Ready** — works with TMCAD, MechCAD, or any STEP-based CAD dataset  

---

## 🏗️ Architecture

```

STEP File → B-Rep Extraction → Face Adjacency Graph → UV Grid Sampling → DGL Graph
↓                              ↓
Topology Stats                 Feature Tensors
↓                              ↓
Visualization                  ML-Ready Format

````

---

## 📊 Output Format

### Graph Structure
- **Nodes**: B-Rep faces (one node per face)
- **Edges**: Face adjacency via shared edges (bidirectional)

### Node Features (`ndata["x"]`)
Shape: `(num_faces, N, N, 7)` where N = 10 (configurable)

| Channel | Description |
|---------|-------------|
| 0-2 | Surface point (x, y, z) |
| 3-5 | Surface normal (nx, ny, nz) |
| 6 | Trim mask (1 if inside face, 0 otherwise) |

### Edge Features (`edata["x"]`)
Shape: `(num_edges, N, 6)` where N = 10 (configurable)

| Channel | Description |
|---------|-------------|
| 0-2 | Curve point (x, y, z) |
| 3-5 | Curve tangent (tx, ty, tz) |

---

## 🚀 Quick Start

### Prerequisites

Create and activate a virtual environment, then install dependencies from the requirements file.

```bash
# create venv
python -m venv venv

# activate
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate

# install dependencies
pip install -r requirements.txt
```
3. Run all cells to see the pipeline in action
4. Point `DATASET_ROOT` to your CAD dataset

### Basic Code Snippet

```python
from step_to_graph import load_step, brep_to_dgl_graph

shape = load_step("path/to/model.stp")
graph, centroids = brep_to_dgl_graph(shape, N=10)

print(f"Nodes: {graph.num_nodes()}, Edges: {graph.num_edges()}")
print(f"Face features: {graph.ndata['x'].shape}")
print(f"Edge features: {graph.edata['x'].shape}")
```

---

## 📁 Project Structure

```
cad-brep-graphs/
├── STEP_to_UVNet_Graph.ipynb   # Main tutorial notebook
├── README.md                   # This file
├── DATASET.md                  # TMCAD dataset notes
├── requirements.txt            # Dependencies
```

---

## 📚 Datasets

This notebook works with any STEP-based CAD dataset, but for this project I used **TMCAD**.  

- **TMCAD Dataset** — 9,799 mechanical CAD models (used in this project)  
- MechCAD — multi-class mechanical parts  
- Fusion 360 Gallery — Autodesk design dataset  

For more details on datasets and how to prepare them, see the separate [DATASET.md](DATASET.md) file.

## 🔬 Applications

The generated graphs can be used for:

| Task               | Description              | Example                         |
| ------------------ | ------------------------ | ------------------------------- |
| **Classification** | Predict part category    | “Is this a gear or a bracket?”  |
| **Segmentation**   | Label faces by feature   | “Hole vs pocket vs planar face” |
| **Retrieval**      | Find similar parts       | “Show me parts like this”       |
| **Generation**     | Create new designs       | “Complete this CAD model”       |
| **DFM Analysis**   | Manufacturability checks | “CNC-friendly or not?”          |

---

## 📖 Related Work

* **UV-Net** — Jayaraman et al., CVPR 2021
  [https://arxiv.org/abs/2006.10211](https://arxiv.org/abs/2006.10211)

* **BRepNet** — Lambourne et al., CVPR 2021
  [https://arxiv.org/abs/2104.00706](https://arxiv.org/abs/2104.00706)

* **TMCAD** — Zou & Zhu, Computer-Aided Design 2025

---

## 📬 Contact

Questions, ideas, or CAD-ML curiosity welcome — open an issue.

---

*The future of engineering design isn’t CAD or AI alone — it’s both working together, so neural networks can finally learn the language of B-Reps (and maybe complain less about meshes).*

