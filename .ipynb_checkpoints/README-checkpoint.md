# Extended Gravel Bedrock Eroder (EGBE) — Tutorial Notebook

A hands-on tutorial for the **Extended Gravel Bedrock Eroder (EGBE)** and **SoilGrading** Landlab components, which simulate the co-evolution of a gravel alluvium layer and underlying bedrock across a river network. The notebook walks through four worked examples of increasing complexity, covering sediment attrition, bedrock abrasion, channel-width assumptions, and landslide–river coupling.

---

## ⚠️ Installation — Special Branch Required

This tutorial depends on components that are **not yet available in the main Landlab release**. You must install Landlab directly from the development branch:

```bash
pip install git+https://github.com/landlab/landlab.git@ys-sm-gt/gbe-expanded
```

Or clone and install locally:

```bash
git clone --branch ys-sm-gt/gbe-expanded https://github.com/landlab/landlab.git
cd landlab
pip install -e .
```

> The branch is: [`ys-sm-gt/gbe-expanded`](https://github.com/landlab/landlab/tree/ys-sm-gt/gbe-expanded)

---

## Overview

### What is EGBE?

EGBE simulates drainage network evolution for rivers that have a **gravel alluvium layer overlying bedrock**, accounting for heterogeneous grain sizes and hardness. It is an advanced version of the Gravel Bedrock Eroder (GBE) described in:

> Gabel, V., Tucker, G. E., & Campforts, B. (2024). A mathematical model for bedrock incision in near-threshold gravel-bed rivers. *Earth Surface Processes and Landforms*, 49(13), 4168–4186.

EGBE must be used together with:
- A **flow routing component** (e.g. `FlowAccumulator` or `PriorityFloodFlowRouter`)
- The **SoilGrading component**, which tracks sediment mass across grain-size classes and simulates grain fragmentation due to weathering

### What is SoilGrading?

SoilGrading tracks sediment mass across multiple grain-size classes and simulates their fragmentation via weathering, based on the mARM model:

> Cohen, S., Willgoose, G., & Hancock, G. (2009). The mARM spatially distributed soil evolution model. *Journal of Geophysical Research: Earth Surface*, 114(F3).
>
> Cohen, S., Willgoose, G., & Hancock, G. (2010). The mARM3D spatially distributed soil evolution model. *Journal of Geophysical Research: Earth Surface*, 115(F4).

---

## Requirements

```
numpy
matplotlib
landlab  ← must be installed from the branch above
```

---

## Tutorial Examples

### Example 1 — High vs. Low Sediment Attrition
Two 1D river profiles (60 km, 500 m spacing) evolve under constant rock uplift. The models are identical except for the **sediment attrition rate**. Demonstrates how sediment hardness controls long-profile shape and river concavity.

### Example 2 — The Role of Bedrock Abrasion
Two 1D models compare configurations where the **sediment is softer than the bedrock** versus where the **bedrock is softer than the sediment**. Highlights how the relative hardness of the bedrock and sediment controls bedrock erosion rates and channel gradient.

### Example 3 — Fixed-Width vs. Dynamic-Width Channel
Explores the two channel-width assumptions available in EGBE:
- **Fixed width**: empirical power-law scaling of width with bankfull discharge
- **Dynamic width**: width adjustment that is based on the near-threshold principle and is dependent on discharge, slope, and median grain size

### Example 4 — EGBE + BedrockLandslider: Fine vs. Coarse Landslide Sediment
Couples EGBE with the **BedrockLandslider** component to simulate how landslide-derived sediment interacts with a river. A knickpoint triggers landslides; one model receives **fine** (1 cm) sediments, the other **coarse** (7 cm). Illustrates how grain size of landslide material controls post-landslide river incision rates and morphology.

---

## Key Components and Model Fields

### EGBE fields (updated by component)

| Field | Units | Description |
|---|---|---|
| `bedrock__elevation` | m | Bedrock surface elevation |
| `soil__depth` | m | Depth of alluvial/soil layer |
| `topographic__elevation` | m | Land surface elevation |
| `grains__weight` | kg/m² | Mass per area for each grain-size class |
| `bedrock__exposure_fraction` | — | Fractional exposure of bedrock at surface |
| `bedrock__plucking_rate` | m/y | Bedrock lowering by plucking |
| `bedrock__abrasion_rate` | m/y | Bedrock lowering by abrasion |
| `bedrock__lowering_rate` | m/y | Total bedrock lowering rate |
| `sediment__rate_of_change` | m/y | Rate of change of sediment thickness |
| `bedload_sediment__volume_influx` | m³/y | Incoming bedload flux |
| `bedload_sediment__volume_outflux` | m³/y | Outgoing bedload flux |

### Key EGBE parameters

| Parameter | Default | Description |
|---|---|---|
| `intermittency_factor` | 0.01 | Fraction of time bankfull flow occurs |
| `sediment_porosity` | 0.35 | Bulk porosity of bed sediment |
| `plucking_coefficient` | 1e-4 1/m | Erodibility coefficient for bedrock plucking |
| `depth_decay_scale` | 1.0 | Scale for depth decay in bedrock exposure function |
| `tau_star_c_median` | 0.045 | Dimensionless critical shear stress for median grain size |
| `alpha` | 0.68 | Empirical exponent for critical shear stress |
| `use_fixed_width` | True | Fixed (`True`) or dynamic (`False`) channel-width model |
| `mannings_n` | 0.05 | Manning's roughness coefficient |
| `tau_c_bedrock` | 10 | Critical shear stress for bedrock plucking |
| `abrasion_coefficients` | 0 | Sediment attrition coefficient |
| `bedrock_abrasion_coefficients` | 0.01 | Bedrock abrasion coefficient |

---

## Usage

Open the notebook in Jupyter:

```bash
jupyter notebook extended_gravel_river_eroder.ipynb
```

Each example is self-contained. Run all cells from top to bottom within an example section. The helper functions (`create_1d_grid`, `init_components`, `plot_profile`) defined at the top are shared across all examples and must be executed first.

---
