### Neurosurgery & Artificial Intelligence Portfolio

I am a clinician currently working in neurosurgery with interests in neuroscience, medical technology, machine learning, and computational modelling.

This portfolio documents projects investigating how computational methods can be applied to clinical prediction, medical imaging, cerebrovascular haemodynamics, and spinal biomechanics.

The emphasis is not only on predictive performance or simulation results, but also on clinical question formulation, methodological validity, numerical verification, interpretability, and the limitations involved in translating computational research into clinical practice.


---

| Project | Clinical question | Methods | Status |
|---|---|---|---|
| **[Traumatic Brain Injury Outcome Prediction](projects/tbi-outcome-prediction.md)** | Can early clinical and imaging information predict 90-day functional outcome after TBI? | Logistic regression, Random Forest, XGBoost, SHAP | **Completed** |
| **[Glioma MRI Segmentation](projects/glioma-mri-segmentation.md)** | Can deep learning segment tumour-related regions on postoperative multimodal glioma MRI? | Multimodal MRI preprocessing, 3D U-Net, Dice-based evaluation, statistical analysis | **Completed** |
| **[Intracranial Aneurysm CFD](projects/aneurysm-cfd.md)** | How does aneurysm geometry influence local blood-flow patterns and wall shear stress? | OpenFOAM, Gmsh, ParaView, finite-volume simulation | **Exploratory — preliminary results; paused** |
| **[Lumbar Spine Finite Element Analysis](projects/spinal-fea.md)** | How do vertebra–disc interface constraints and patient-specific vertebral geometry influence simulated spinal biomechanics? | CalculiX, Python, CT-derived 3D reconstruction, tetrahedral FEA, interface sensitivity analysis | **Exploratory — baseline FEA completed; patient-specific geometry reconstruction ongoing** |
| **EEG Seizure Detection** | Can seizure activity be detected from EEG recordings using signal processing and machine learning? | Signal processing, machine learning, deep learning | **Future project** |
| **Intracranial Pressure Crisis Forecasting** | Can physiological time-series data provide early warning of intracranial hypertension? | Time-series analysis, machine learning, deep learning | **Future project** |

---

## Research Highlights

The following figures illustrate selected results from four projects spanning clinical prediction, medical image analysis, cerebrovascular haemodynamics and spinal biomechanics.

| Clinical Prediction & Medical Imaging | Computational Haemodynamics & Biomechanics |
|---|---|
| **[Traumatic Brain Injury Outcome Prediction](projects/tbi-outcome-prediction.md)** | **[Intracranial Aneurysm CFD](projects/aneurysm-cfd.md)** |
| ![Comparison of TBI prediction model performance](assets/tbi/model_performance_comparison.png) | ![Patient-derived intracranial aneurysm velocity streamlines](https://raw.githubusercontent.com/T-Y-Ang/aneurysm-cfd/main/outputs/figures/patient_aneurysm_streamlines.png) |
| Held-out model comparison; best clinical-plus-imaging Random Forest ROC-AUC: **0.854**. | Preliminary patient-derived MCA aneurysm flow visualisation; numerical and physiological validation remain outstanding. |
| **[Glioma MRI Segmentation](projects/glioma-mri-segmentation.md)** | **[Lumbar Spine Finite Element Analysis](projects/spinal-fea.md)** |
| ![Postoperative glioma MRI anatomical failure analysis](https://raw.githubusercontent.com/T-Y-Ang/glioma-mri-segmentation/main/outputs/figures/held_out_test_worst_case_reference_anatomy.png) | ![Lumbar spine finite element stress distribution](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/baseline_tolerance_005_max_stress_section.png) |
| Postoperative MRI segmentation; held-out macro-average Dice: **0.6630** after post-processing. | Baseline L1–L2 simulation demonstrating sensitivity to vertebra–disc interface constraints; biomechanical validation remains outstanding. |

*The CFD and FEA findings are preliminary computational demonstrations and should not be interpreted as clinically validated predictions.*

---

## Project 1 — Traumatic Brain Injury Outcome Prediction

**Status: Completed**

A prognostic machine-learning study using the multinational GNS-I traumatic brain injury cohort.

The project examines whether information available during early assessment can predict unfavourable functional outcome at 90 days, and whether structured imaging findings provide additional prognostic information beyond clinical assessment alone.

**Data source:** Global Neurosurgical Study-1 (GNS-I)  
[Original Nature Medicine study →](https://doi.org/10.1038/s41591-026-04600-6)

### Key findings

- Early clinical information provided useful discrimination for 90-day functional outcome.
- Random Forest and XGBoost demonstrated higher discrimination than the logistic-regression baseline.
- The clinical + imaging Random Forest achieved a held-out ROC-AUC of **0.854**, average precision of **0.706**, and Brier score of **0.114**.
- Adding imaging information produced relatively modest improvements in discrimination.
- Glasgow Coma Scale, pupil reactivity and age were the most influential predictors in the interpreted Random Forest model.

### Model Performance

![TBI model discrimination comparison](assets/tbi/model_performance_comparison.png)

*Comparison of held-out ROC-AUC across three model families using clinical-only and clinical-plus-imaging predictors. Adding imaging provided relatively modest improvements in discrimination.*

### Project Resources

**[View the TBI project showcase →](projects/tbi-outcome-prediction.md)**

*The underlying research repository is maintained privately while the work is under consideration for academic dissemination.*

---

## Project 2 — Glioma MRI Segmentation

**Status: Completed**

A deep-learning study investigating automated segmentation of postoperative glioma MRI using a multilabel three-dimensional U-Net.

Using the BraTS 2024 GLI dataset, the project investigates multimodal MRI preprocessing, segmentation of postoperative tumour-related regions, model development, and evaluation on a held-out test cohort with subject-level separation. Particular attention was given to resection-cavity segmentation, which presents challenges in postoperative neuro-oncological imaging.

### Key findings

- The selected model was evaluated on **198 held-out MRI examinations from 92 subjects**, with subject-level separation from the development cohorts.
- Baseline macro-average Dice across four segmentation regions was **0.6525**, increasing to **0.6630** following validation-informed resection-cavity post-processing.
- Among **157 examinations containing a reference resection cavity**, mean resection-cavity Dice improved from **0.5221 to 0.5638**.
- The mean paired improvement was **+0.0417** (95% subject-clustered bootstrap confidence interval: **+0.0227 to +0.0625**).
- Anatomical failure-case analysis identified important limitations, including deterioration in individual cases despite improved average performance.

The study illustrates the importance of evaluating medical-imaging models beyond aggregate accuracy, particularly when considering their potential clinical relevance.

### Clinical Failure Analysis — Anatomical Interpretation

![Held-out postoperative glioma MRI case illustrating reference anatomy and a potential annotation discrepancy](https://raw.githubusercontent.com/T-Y-Ang/glioma-mri-segmentation/main/outputs/figures/held_out_test_worst_case_reference_anatomy.png)

*Representative held-out test case examined during anatomical failure analysis. The model's resection-cavity Dice score deteriorated markedly following post-processing (0.5345 to 0.0046). Review of the MRI and reference segmentation raised the possibility of annotation ambiguity, although an annotation error has not been independently confirmed.*

This case illustrates an important limitation of automated medical-image segmentation: quantitative performance metrics must be interpreted alongside the underlying anatomy and the quality of reference annotations.

### Project Resources

**[Read the Glioma MRI Segmentation project overview →](projects/glioma-mri-segmentation.md)**

[Read the full scientific report →](https://github.com/T-Y-Ang/glioma-mri-segmentation/blob/main/reports/glioma_segmentation_report.md)

[Explore the public GitHub repository →](https://github.com/T-Y-Ang/glioma-mri-segmentation)

---

## Project 3 — Intracranial Aneurysm Haemodynamics (CFD)

**Status: Exploratory Study — Preliminary Results; Temporarily Paused**

A computational fluid dynamics study investigating blood-flow behaviour within intracranial aneurysms using synthetic and patient-derived vascular geometries.

The project progressed from synthetic sidewall aneurysm modelling to a patient-derived left middle cerebral artery aneurysm from the publicly available Royal Brisbane TOF-MRA Intracranial Aneurysm dataset (OpenNeuro ds005096).

### Key achievements

- Developed a computational workflow involving vascular geometry processing, tetrahedral meshing, OpenFOAM simulation and ParaView visualisation.
- Generated a patient-derived baseline mesh containing **117,490 tetrahedral elements**.
- Completed a preliminary steady-state laminar-flow simulation and verified global mass conservation.
- Visualised velocity streamlines, vorticity and preliminary wall shear stress.
- Generated a refined mesh containing **221,743 tetrahedral elements** for subsequent numerical comparison.
- Identified unresolved outlet-geometry and mesh-quality issues requiring correction before further physiological interpretation.

### Patient-Derived Aneurysm — Preliminary Flow Visualisation

![Patient-derived MCA aneurysm CFD velocity streamlines](https://raw.githubusercontent.com/T-Y-Ang/aneurysm-cfd/main/outputs/figures/patient_aneurysm_streamlines.png)

*Preliminary velocity streamlines from the patient-derived left MCA aneurysm simulation. Results have not undergone mesh-independence testing, and an unresolved outlet-geometry concern may influence the computed flow distribution.*

### Current limitations

Although the baseline simulation was completed and global mass conservation was demonstrated, **mesh independence and physiological validity have not been established**.

The computational model uses simplified steady-state boundary conditions and rigid vessel walls. Both baseline and refined meshes retain quality warnings, and a possible residual surface-cap fragment may restrict one vessel outlet.

The refined-mesh flow simulation and formal sensitivity analyses remain outstanding.

**These results are exploratory and cannot be used to predict aneurysm rupture risk or guide clinical management.**

### Project resources

**[Read the Aneurysm CFD project report →](projects/aneurysm-cfd.md)**

[Read the full interim technical report →](https://github.com/T-Y-Ang/aneurysm-cfd/blob/main/docs/patient_mca_cfd_interim_report.md)

[Explore the public GitHub repository →](https://github.com/T-Y-Ang/aneurysm-cfd)

---

## Project 4 — Lumbar Spine Finite Element Analysis

**Status: In Progress — Synthetic FEA Proof of Concept Completed; Patient-Derived Modelling Underway**

An exploratory computational biomechanics study investigating the mechanical behaviour of the lumbar spine under simplified axial compression, with particular emphasis on vertebra–disc interface modelling.

The initial proof of concept used an existing L1–L2 anatomical mesh from a publicly available Zenodo dataset. This mesh was converted into a CalculiX-compatible finite element model, allowing the development and testing of structural simulation, interface constraint analysis, and stress/displacement post-processing workflows.

The subsequent phase extends this work towards patient-derived spinal anatomy using CT-based vertebral reconstruction from the VerSe dataset.

### Key achievements

- Converted an existing MED-format anatomical mesh into a CalculiX-compatible finite element model containing **17,513 nodes and 62,247 tetrahedral elements**.
- Completed two axial compression simulations using simplified linear-elastic material properties and a prescribed **0.1 mm displacement**.
- Verified approximate global axial force equilibrium in both simulations.
- Investigated the effect of vertebra–disc tie tolerance on mechanical response.
- Identified a **40.07% increase in calculated axial stiffness** when the tie tolerance was increased to 0.05 mm.
- Investigated the geometry and topology of the vertebra–disc interfaces to understand residual tie-constraint failures.
- Developed a Python post-processing workflow to extract nodal displacement vectors and stress tensor components from CalculiX FRD results and convert them into PyVista-compatible datasets.
- Generated three-dimensional displacement and von Mises stress visualisations, including a cross-sectional investigation of the maximum-stress region.
- Identified a maximum nodal von Mises stress of **1.579123 MPa** and greatest axial compressive stress (SZZ) of **−1.52658 MPa**, both occurring at node 10200 in the 0.05 mm tie-tolerance simulation.


**Principal methodological finding:** Increasing the vertebra–disc tie tolerance from the baseline configuration to 0.05 mm increased the calculated axial stiffness by 40.07%. This demonstrates how assumptions about mechanical coupling between anatomical structures can substantially influence finite element results, even when the underlying geometry and applied loading remain unchanged.

The finding reflects sensitivity to numerical interface constraints and should not be interpreted as a physiological change in spinal stiffness.


### Vertebra–Disc Interface Investigation

![L1–L2 spinal FEA anatomical orientation](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/spinal_fea_anatomical_orientation.png)

*Anatomical orientation of the L1–L2 finite element model, providing spatial context for the vertebra–disc interface investigation.*

![Rejected vertebra–disc interface tie nodes](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/rejected_tie_nodes_by_interface.png)

*Distribution of rejected tie nodes at the L1–disc and disc–L2 interfaces following adjustment of the tie tolerance to 0.05 mm. Residual failures were concentrated at the peripheral disc interfaces.*

### Preliminary Displacement and Stress Analysis

The completed 0.05 mm tie-tolerance simulation was post-processed to investigate displacement and stress distributions across the L1–L2 model.

![L1–L2 spinal FEA cross-sectional von Mises stress distribution](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/baseline_tolerance_005_max_stress_section.png)

*Cross-sectional von Mises stress distribution through the maximum-stress node (10200). The localised stress concentration near a vertebral endplate–disc interface warrants further investigation. The results are preliminary and have not undergone mesh-convergence testing or experimental biomechanical validation.*

The maximum calculated nodal displacement magnitude was **0.132768 mm**, while the maximum von Mises stress was **1.579123 MPa**.

The observed stress concentration may be influenced by local geometry, material assumptions, interface constraints or numerical stress extrapolation. It should not be interpreted as a validated predictor of vertebral failure.

### Patient-Specific Vertebral Reconstruction — VerSe004

The project has progressed to patient-specific anatomical reconstruction using the publicly available VerSe 2019 CT dataset.

Case `sub-verse004`, demonstrating L1 vertebral compression deformity, was selected for an exploratory T12–L1–L2 model.

The aim is to reconstruct the pathological vertebral anatomy and develop anatomically informed intervertebral discs before proceeding to volumetric meshing and mechanical simulation.

#### CT-Derived Vertebral Reconstruction

Three-dimensional surfaces of T12, L1 and L2 were reconstructed from the CT segmentation data.

![CT-derived T12–L1–L2 vertebral reconstruction](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/verse004_t12_l1_l2_alignment.png)

*Reconstructed T12, L1 and L2 vertebrae demonstrating their relative anatomical alignment. These surfaces form the basis of the planned patient-specific FEA model.*

#### Endplate Identification and Disc-Space Investigation

Initial endplate identification using surface-normal filtering revealed important limitations when applied to curved and pathological vertebral anatomy.

Further investigation demonstrated that filtered surface patches could exclude relevant portions of the inferior L1 endplate.

![L1 inferior endplate selection compared with complete vertebral surface](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/verse004_l1_inferior_selection_vs_full_surface.png)

*Comparison of the initially selected inferior endplate region against the complete reconstructed vertebral surface, illustrating the limitations of geometric filtering.*

Vertical ray tracing was subsequently used to investigate separation between the complete L1 and L2 surfaces.

Across 400 sampling locations, 286 yielded positive gap measurements. The median estimated separation was **8.42 mm**, but anomalous measurements reached **44.61 mm**, highlighting the need to distinguish anatomically relevant endplate intersections from unintended surface intersections.

![L1–L2 complete-surface gap map](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/verse004_l1_l2_complete_surface_gap_map.png)

*Spatial distribution of estimated vertical separation between the reconstructed vertebral surfaces. These measurements are preliminary geometric estimates, not validated anatomical disc heights.*

#### Anatomically Informed Disc Reconstruction

Initial elliptical disc footprints were investigated but were considered insufficient to represent the reconstructed vertebral anatomy.

The revised approach is to derive a smoothed, kidney-shaped disc footprint from the CT-derived vertebral body contours, incorporating a modest circumferential bulge as an explicit modelling assumption.

The disc geometry has not yet been completed.

#### Methodological Lessons

This investigation illustrates several challenges in patient-specific computational biomechanics:

- Geometrically closest surfaces may not correspond to opposing vertebral endplates.
- Surface-normal and connected-component filtering can exclude anatomically relevant regions.
- Vertical ray tracing may produce multiple intersections and implausible gap measurements.
- Simplified disc footprints may inadequately represent anatomical morphology.
- Anatomical interpretation and geometric quality control are essential before mechanical simulation.

The reconstruction and troubleshooting process has been documented, including the diagnostic figures, numerical measurements, methodological limitations and revised modelling strategy.

**[Read the VerSe004 geometry reconstruction and troubleshooting report](https://github.com/T-Y-Ang/spinal-fea/blob/main/docs/verse004_geometry_reconstruction.md)**

The patient-specific vertebra–disc assembly, volumetric mesh and mechanical simulation remain under development.

### Current limitations

Although both simulations completed successfully and demonstrated approximate global axial force equilibrium, **biomechanical validity and mesh convergence have not been established**.

The model uses simplified material properties, first-order tetrahedral elements and idealised loading conditions. Its predicted mechanical stiffness and stresses remain sensitive to interface constraint assumptions.

The remaining peripheral tie failures require further investigation before interpreting the model as a reliable representation of physiological spinal loading.

**These results demonstrate computational feasibility and interface sensitivity, not clinically validated spinal biomechanics.**

### Project resources

**[Read the Spinal FEA project report →](projects/spinal-fea.md)**

[Read the VerSe004 geometry reconstruction and troubleshooting report →](https://github.com/T-Y-Ang/spinal-fea/blob/main/docs/verse004_geometry_reconstruction.md)

[Read the baseline vertebra–disc interface methodology →](https://github.com/T-Y-Ang/spinal-fea/blob/main/docs/interface_methodology.md)

[Explore the public GitHub repository →](https://github.com/T-Y-Ang/spinal-fea)

---

## Portfolio Approach

These projects are developed around clinically relevant questions rather than algorithm or software selection alone.

Across machine-learning projects, I aim to consider:

- appropriate clinical question formulation;
- data quality, missing information and information leakage;
- comparison with interpretable baseline models;
- discrimination, calibration and model interpretability;
- rigorous validation and generalisability.

For computational simulation projects, additional considerations include:

- anatomical geometry and modelling assumptions;
- material properties and boundary conditions;
- mesh quality and numerical convergence;
- reproducibility and sensitivity analysis;
- the distinction between simulated physical behaviour and clinically validated predictions.

The projects are intended as educational and research work and should not be interpreted as clinically validated decision-support tools.

---

## Current Development

The traumatic brain injury outcome prediction and glioma MRI segmentation studies are completed. Exploratory computational demonstrations have also been established for intracranial aneurysm haemodynamics and lumbar spinal biomechanics.

**Current development is focused on patient-specific spinal finite element modelling using CT-derived vertebral anatomy.**

### Spinal Finite Element Analysis

The spinal FEA project has progressed through two stages:

1. **Baseline FEA simulation:** Completed simplified L1–L2 axial compression simulations using CalculiX, including investigation of vertebra–disc interface constraints, displacement fields and stress distributions.
2. **Patient-specific reconstruction:** Reconstructed T12, L1 and L2 from the VerSe004 CT dataset, investigated vertebral endplate geometry and intervertebral spacing, and documented the limitations of several geometric reconstruction methods.

The immediate priority is to develop anatomically informed T12–L1 and L1–L2 intervertebral discs using CT-derived vertebral body contours.

This will be followed by volumetric meshing, exploratory mechanical simulation and numerical verification.

The [VerSe004 technical report](https://github.com/T-Y-Ang/spinal-fea/blob/main/docs/verse004_geometry_reconstruction.md) documents the reconstruction process, troubleshooting investigations, geometric measurements and outstanding limitations.

### Intracranial Aneurysm CFD

The intracranial aneurysm CFD project is temporarily paused following completion of a patient-derived baseline simulation and preliminary haemodynamic visualisations.

Further work will be required to resolve outlet-geometry uncertainty, improve mesh quality and establish numerical reliability before physiological interpretation.

### Future Directions

Potential future investigations include longitudinal vertebral compression-fracture analysis, EEG seizure detection, intracranial pressure forecasting and other applications of computational methods to neurosurgical research.

Across these projects, the emphasis remains on reproducible computational workflows, transparent reporting of methodological limitations and the distinction between successful numerical simulations and clinically validated models.