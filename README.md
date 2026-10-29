# Experiment 1: Medical Image Tissue-Evolution Architecture Diagram

## Aim

Create and display a Graphviz diagram describing a proposed workflow for medical-image tissue evolution. The diagram shows data collection, preprocessing, a hybrid CNN and Vision Transformer (ViT) stage, ROI-based feature extraction, IB-SEM evolution, and postprocessing.

This experiment generates a **workflow diagram only**. It does not load medical images, train or run a CNN/ViT model, calculate features, or implement IB-SEM decision rules.

## Repository Contents

| File | Description |
| --- | --- |
| `Arch_Diagram.ipynb` | Jupyter/Colab notebook that builds and displays the architecture diagram. |
| `README.md` | Experiment aim, procedure, and program. |

## Requirements

- Python 3
- Graphviz system package
- Python `graphviz` package
- IPython display support (included in Jupyter and Google Colab)

In Google Colab, the notebook installs Graphviz and its Python package in the program cell. In another Jupyter environment, install Graphviz using that environment's package manager and install the Python package with `pip install graphviz`.

## Algorithm / Procedure

1. Install Graphviz and the Python `graphviz` package.
2. Import `Digraph` and the notebook image display helpers.
3. Create a directed graph configured to render as a PNG, and set the background and font.
4. Add the medical-image data-collection node.
5. Add a preprocessing cluster containing enhancement, resampling and normalization, and denoising nodes.
6. Add a hybrid CNN + ViT cluster containing local-feature, global-context, and segmentation-mask nodes.
7. Add the ROI-based feature extraction, IB-SEM evolution, and postprocessing/output nodes.
8. Place the feature extraction, IB-SEM, and postprocessing nodes at the same rank.
9. Add the directed edges and labels specified in the notebook.
10. Render the graph to `journal_architecture_final_layout.png` and display that PNG in the notebook.

## Program

The following is the diagram-generation program from `Arch_Diagram.ipynb`:

```python
!apt-get install graphviz -y
!pip install graphviz

from graphviz import Digraph
from IPython.display import Image, display

dot = Digraph(
    'Journal_Architecture_Final_v2',
    format='png',
    engine='dot'
)

dot.attr(
    bgcolor='#b7d7e8',
    fontname='Times New Roman'
)

# ---------------- DATA COLLECTION ----------------
dot.node(
    'Data',
    'DATA COLLECTION\\nMedical Images',
    shape='box',
    style='rounded'
)

# ---------------- STEP 1: PREPROCESSING ----------------
with dot.subgraph(name='cluster_pre') as pre:
    pre.attr(label='STEP 1: PREPROCESSING',
             style='rounded')
    pre.node('Enh', 'Enhancement')
    pre.node('Norm', 'Resampling & Normalization')
    pre.node('Den', 'Denoising')

# ---------------- STEP 2: CNN + ViT ----------------
with dot.subgraph(name='cluster_model') as model:
    model.attr(label='STEP 2: HYBRID CNN + ViT',
               style='rounded')
    model.node('CNN', 'CNN\\n(Local Features)')
    model.node('ViT', 'ViT\\n(Global Context)')
    model.node('Mask', 'Segmentation Mask')

# ---------------- STEP 3: FEATURE EXTRACTION ----------------
dot.node(
    'Feat',
    'STEP 3: FEATURE EXTRACTION\\nIntensity | Boundary | Texture\\n(ROI-based)',
    shape='box',
    style='rounded'
)

# ---------------- STEP 4: IB-SEM ----------------
dot.node(
    'IBSEM',
    'STEP 4: IB-SEM EVOLUTION\\nGrowth | Shrink | Diffusion\\nDecision Rules',
    shape='box',
    style='rounded'
)

# ---------------- STEP 5: POSTPROCESSING ----------------
dot.node(
    'Post',
    'STEP 5: POSTPROCESSING & OUTPUT\\nSmoothing | Final Result',
    shape='box',
    style='rounded'
)

# -------- FORCE STEP 3, 4, 5 ON SAME LEVEL --------
with dot.subgraph() as same_rank:
    same_rank.attr(rank='same')
    same_rank.node('Feat')
    same_rank.node('IBSEM')
    same_rank.node('Post')

# ---------------- CLEAN ARCHITECTURAL ARROWS ----------------
dot.edge('Data', 'Enh')
dot.edge('Den', 'CNN')
dot.edge('Mask', 'Feat', label='ROI')
dot.edge('Feat', 'IBSEM', label='Feature Interaction')
dot.edge('IBSEM', 'Post')

# Render
file_path = dot.render('journal_architecture_final_layout')
display(Image(filename=file_path))
```

## Output

The program renders `journal_architecture_final_layout.png` in the notebook's working directory and displays it.

## Note

The notebook contains a second code cell that tries to display `/content/tissue_evolution_architecture.png`. The program above does not create that path; it creates `journal_architecture_final_layout.png`.
