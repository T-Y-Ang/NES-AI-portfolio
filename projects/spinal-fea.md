# Lumbar Spine Finite Element Analysis

**Computational biomechanics | Spinal mechanics | Numerical simulation**

**Status:** Baseline FEA simulations completed; patient-specific CT reconstruction completed; disc reconstruction and patient-specific FEA in progress
**Datasets:** [Zenodo L1–L2 finite element mesh](https://zenodo.org/records/3961334) · VerSe 2019 (`sub-verse004`)
**Software:** Python · CalculiX 2.21 · SciPy · PyVista · ParaView · 3D Slicer 5.8.0

[← Back to AI portfolio](../README.md) · [Source-code repository](https://github.com/T-Y-Ang/spinal-fea) · [Validation report](https://github.com/T-Y-Ang/spinal-fea/blob/main/docs/validation_progress.md)

---

## Clinical question

**Can an open-source finite element workflow simulate lumbar spinal loading, and how strongly do vertebra–disc interface constraints influence the predicted mechanical response?**

Finite element analysis (FEA) offers a computational approach to investigating mechanical loading, deformation and stress distributions within spinal structures.

Potential applications include understanding vertebral compression fractures, progressive deformity and the biomechanical consequences of spinal interventions.

However, the reliability of spinal FEA depends on anatomical geometry, material properties, mesh formulation, boundary conditions and the representation of mechanical connections between anatomical structures.

This project investigates the development of a reproducible spinal FEA workflow, beginning with an existing L1–L2 anatomical mesh and simplified axial compression simulations.

## Study at a glance

| Component | Approach |
|---|---|
| Initial anatomical model | L1 and L2 vertebrae with intervening intervertebral disc |
| Source dataset | Existing MED-format finite element mesh from Zenodo |
| Mesh | 17,513 nodes; 62,247 first-order tetrahedral elements |
| Mesh preparation | Python-based MED-to-CalculiX conversion |
| Solver | CalculiX 2.21 |
| Material model | Homogeneous, isotropic, linear elastic |
| Mechanical loading | Prescribed 0.1 mm axial compression |
| Interface modelling | Four surface-based vertebra–disc tie constraints |
| Completed analysis | Baseline compression simulations, force-equilibrium assessment, interface sensitivity analysis, geometric investigation, and displacement and stress-field visualisation |
| Patient-specific extension | VerSe2019 L1 compression deformity; CT-derived T12–L2 vertebral surface reconstruction completed |
| Outstanding work | Anatomically guided disc reconstruction, patient-specific FEA, mesh convergence, interface refinement and biomechanical validation |

## Patient-specific extension: VerSe004 L1 compression fracture

Following the baseline finite element simulations, I began extending the workflow to patient-specific pathological spinal anatomy using the publicly available VerSe 2019 CT dataset.

The selected case, `sub-verse004`, demonstrates an L1 vertebral compression deformity. The objective is to reconstruct a T12–L2 spinal segment, including anatomically informed T12–L1 and L1–L2 intervertebral discs, for subsequent investigation of mechanical loading and stress distribution.

### CT-derived vertebral reconstruction

![Patient-specific reconstruction of T12, L1 and L2](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/verse004_t12_l1_l2_alignment.png)

*Figure. Three-dimensional reconstruction of T12, L1 and L2 from VerSe2019 CT segmentation data. The L1 vertebra demonstrates compression deformity. This is an anatomical surface reconstruction, not a completed patient-specific finite element simulation.*

The reconstructed surfaces contain:

| Vertebra | Surface triangles | Enclosed surface volume |
|---|---:|---:|
| T12 | 19,928 | 41,165 mm³ |
| L1 | 24,606 | 38,071 mm³ |
| L2 | 25,108 | 53,545 mm³ |

These volumes describe the reconstructed vertebral surfaces, including posterior elements, and should not be interpreted as isolated vertebral body volumes or measurements of fracture-related volume loss.

### Anatomical reconstruction challenges

Initial attempts to identify opposing vertebral endplates using surface-normal filtering and vertical ray tracing revealed important limitations. Curved endplates were incompletely selected, while some ray intersections involved posterior elements rather than the intended disc-contact surfaces.

Subsequent axial and sagittal investigations demonstrated that fixed-coordinate cropping could not reliably separate the L1 vertebral body from its posterior elements without either retaining unwanted structures or removing relevant anatomy.

The workflow has therefore progressed to CT-guided anatomical segmentation using 3D Slicer.

**Current status:** CT-derived T12–L2 surface reconstruction is complete. Anatomically guided vertebral-body isolation and intervertebral disc reconstruction remain in progress. Patient-specific volumetric meshing and mechanical simulation have not yet been completed.

[Detailed VerSe004 geometry reconstruction report](https://github.com/T-Y-Ang/spinal-fea/blob/main/docs/verse004_geometry_reconstruction.md)

## Baseline finite element model and simulations

The initial model was obtained from a publicly available anatomical finite element dataset containing L1, L2 and the intervening intervertebral disc.

The original MED-format mesh was inspected and converted into CalculiX-compatible input files using Python.

Mesh preparation included verification of tetrahedral element orientation, correction of inverted element connectivity, preservation of anatomical element groups and reconstruction of disc-facing vertebral endplate surfaces.

The converted mesh contained 17,513 nodes and 62,247 tetrahedral elements.

A simplified mechanical model was constructed using the following assumptions:

| Parameter | Value |
|---|---|
| Vertebral Young's modulus | 1,000 MPa |
| Vertebral Poisson's ratio | 0.30 |
| Disc Young's modulus | 10 MPa |
| Disc Poisson's ratio | 0.45 |
| Inferior boundary | L2 inferior surface fixed in all translational directions |
| Superior boundary | L1 superior surface prescribed +0.1 mm displacement along Z |
| Interface constraints | Four surface-based tie constraints |
| Unit system | N–mm–MPa |

### Rationale for material properties and boundary conditions

The material properties were selected to establish a simplified, reproducible baseline model rather than reproduce the complete mechanical behaviour of a physiological lumbar motion segment.

Published lumbar finite element models commonly distinguish cortical bone, cancellous bone, vertebral endplates, annulus fibrosus and nucleus pulposus, assigning different material properties to each component [1,2].

**Vertebral material properties.** A Young's modulus of 1,000 MPa and Poisson's ratio of 0.30 were assigned uniformly to the vertebral structures. The modulus represents a provisional effective stiffness within a heterogeneous bony structure, rather than a directly measured whole-vertebra property. For comparison, published models have used approximately 100 MPa for cancellous bone and 12,000 MPa for cortical bone [1,2]. The selected Poisson's ratio is consistent with values used for cortical bone in these models, although cancellous bone is often assigned a lower value.

**Intervertebral disc properties.** A Young's modulus of 10 MPa and Poisson's ratio of 0.45 were assigned uniformly to the disc. This simplified representation avoids introducing separate nucleus pulposus and annulus fibrosus material regions during initial workflow development. Published models have used an annulus ground-substance modulus of approximately 4.2 MPa with Poisson's ratio 0.45, while representing the nucleus pulposus using substantially softer or nearly incompressible material formulations [1,2]. Our selected modulus is therefore a provisional modelling assumption rather than a literature-validated effective modulus for the entire disc.

**Boundary conditions and loading.** The inferior L2 surface was fixed in all three translational directions, while a prescribed 0.1 mm inferior displacement was applied to the superior L1 surface. This displacement-controlled configuration provides a reproducible axial compression test and allows effective construct stiffness to be calculated from the resulting reaction force. The displacement magnitude was selected for this numerical demonstration and was not derived from a patient-specific physiological loading measurement.

**Vertebra–disc interfaces.** Four surface-based tie constraints were used to transfer mechanical loading between the vertebrae and disc. This assumes bonded interfaces without relative sliding or separation. The subsequent comparison of default and 0.05 mm tie tolerances was designed to investigate the sensitivity of the numerical solution to interface constraint generation, rather than to identify a physiologically optimal tolerance.

**Unit system.** The N–mm–MPa system was used consistently, with forces expressed in newtons, displacements in millimetres and stresses in N/mm² (equivalent to MPa).

These assumptions are appropriate for an initial computational workflow demonstration but do not establish physiological accuracy. In particular, homogeneous material assignments, idealised interface bonding and the use of first-order tetrahedral elements may influence predicted stiffness and local stress concentrations.

Two simulations were completed to investigate the effect of vertebra–disc tie tolerance on the predicted mechanical response.

**References**

[1] *Development and Kinematic Verification of a Finite Element Model for the Lumbar Spine: Application to Disc Degeneration.* 2013. https://pmc.ncbi.nlm.nih.gov/articles/PMC3591128/

[2] *Development and Validation of a Computationally Efficient Finite Element Model of the Human Lumbar Spine: Application to Disc Degeneration.* 2020. https://pmc.ncbi.nlm.nih.gov/articles/PMC7478021/

## Preliminary computational results

### Axial compression and force equilibrium

Both baseline simulations completed successfully.

Global reaction forces were evaluated at the superior and inferior boundaries to assess mechanical equilibrium.

| Measurement | Default tie tolerance | 0.05 mm tie tolerance |
|---|---:|---:|
| Superior axial reaction force | +397.320 N | +556.540 N |
| Inferior axial reaction force | −397.320 N | −556.540 N |
| Effective axial stiffness | 3,973.201 N/mm | 5,565.400 N/mm |
| Maximum nodal displacement | 0.133626 mm | 0.132768 mm |
| Maximum nodal von Mises stress | 0.987775 MPa | 1.579123 MPa |

The superior and inferior axial reaction forces were approximately equal and opposite in both simulations, demonstrating negligible global axial force imbalance.

Effective axial stiffness was calculated by dividing the magnitude of the inferior axial reaction force by the prescribed 0.1 mm displacement.

These checks support preliminary numerical verification but do not establish physiological accuracy.

### Displacement and stress-field visualisation

The 0.05 mm tie-tolerance simulation was post-processed using Python and PyVista to visualise displacement magnitude and von Mises stress across the L1–L2 construct.

![L1–L2 displacement magnitude](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/baseline_tolerance_005_displacement.png)

*Figure 1. Displacement magnitude under prescribed 0.1 mm axial compression. L1 is positioned superiorly, with L2 fixed at its inferior boundary.*

![L1–L2 von Mises stress](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/baseline_tolerance_005_von_mises.png)

*Figure 2. Von Mises stress distribution under axial compression, displayed using a fixed 0–1.6 MPa colour scale.*

![Cross-sectional von Mises stress](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/baseline_tolerance_005_max_stress_section.png)

*Figure 3. Cross-sectional von Mises stress distribution through the X-coordinate of the maximum-stress node (X = 189.187 mm).*

The maximum nodal von Mises stress was approximately **1.579 MPa**, occurring at node 10200. The cross-sectional visualisation demonstrates a localised region of elevated stress near the vertebra–disc interface.

These findings remain exploratory. Local stress concentrations may be influenced by mesh discretisation, material assumptions and interface constraints. They should not be interpreted as validated physiological stress predictions.

### Vertebra–disc interface sensitivity

The original simulation used the automatically determined CalculiX tie tolerance of approximately 0.01206 mm.

Solver warnings identified 228 disc nodes for which the intended tie constraints were not generated.

Increasing the tie tolerance to 0.05 mm reduced the number of untied nodes to 50.

However, this modification produced a **40.07% increase in calculated axial stiffness** and an approximately **59.9% increase in maximum nodal von Mises stress**.

These differences demonstrate that interface constraint selection substantially affects the predicted mechanical response.

The results should therefore be interpreted as an interface sensitivity investigation, rather than evidence that either tie configuration reproduces physiological spinal stiffness.

## Interface geometry and numerical reliability

To investigate the remaining 50 rejected tie nodes, the original MED mesh surface definitions were examined using Python, SciPy, VTK and PyVista.

The original mesh contained separately defined inner and outer disc interface surfaces.

Geometric and topological investigations demonstrated that the inner surfaces form central patches, whereas the outer surfaces form connected annular regions.

All 50 remaining rejected tie nodes occurred on the outer disc interfaces:

- 29 nodes at the L1–disc interface.
- 21 nodes at the disc–L2 interface.

### L1 inferior endplate–disc interface

![L1 inferior endplate and rejected tie nodes](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/l1_interface_tie_failures.png)

*Figure 4. Computational interface geometry showing the L1 inferior endplate, inner and outer disc interface surfaces, and 29 rejected tie nodes.*

### L2 superior endplate–disc interface

![L2 superior endplate and rejected tie nodes](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/l2_interface_tie_failures.png)

*Figure 5. Computational interface geometry showing the L2 superior endplate, inner and outer disc interface surfaces, and 21 rejected tie nodes.*

Further geometric investigation found that 10 rejected nodes were outside the XY-projected vertebral endplate footprints, while 40 remained within the projected footprints but exceeded the specified tie tolerance.

Peripheral disc extension may reflect anatomical geometry rather than an error in the original mesh. The investigations do not independently establish whether every peripheral node should be mechanically tied.

Consequently, the original mesh geometry was preserved, and further increases in tie tolerance were not adopted as a substitute for resolving the underlying interface modelling uncertainty.

Detailed investigations are documented in the [interface methodology report](https://github.com/T-Y-Ang/spinal-fea/blob/main/docs/interface_methodology.md).

## What I learned

This project demonstrated that producing a numerically converged finite element solution is only one part of developing a reliable biomechanical model.

Several lessons emerged:

1. **Anatomical mesh preparation matters.** Element orientation, surface definitions and interface geometry must be verified before mechanical results can be meaningfully interpreted.
2. **Mechanical interfaces can dominate predicted behaviour.** A relatively small change in tie tolerance produced a substantial difference in calculated axial stiffness.
3. **Global force equilibrium is necessary but insufficient.** A model can satisfy equilibrium while remaining sensitive to uncertain interface constraints.
4. **Geometric discrepancies require anatomical interpretation.** Peripheral disc geometry should not be altered solely to eliminate numerical warnings.
5. **Transparent documentation is part of computational research.** Unresolved numerical limitations should be reported alongside successful simulations.

## Limitations and future directions

The current model is an exploratory demonstration with several important limitations.

The vertebral bodies and disc are represented by simplified homogeneous, isotropic, linear-elastic materials. The model does not distinguish cortical and cancellous bone or represent the nucleus pulposus and annulus fibrosus as separate material regions.

It also omits several important spinal structures, including ligaments, facet-joint interactions and musculature.

The use of first-order tetrahedral elements with a nearly incompressible disc material may introduce numerical stiffness artefacts. Mesh convergence and experimental biomechanical validation have not been established.

The remaining peripheral tie failures and the sensitivity of axial stiffness to tie tolerance require further investigation.

Displacement and von Mises stress-field visualisations have now been completed for the baseline compression model. However, these results remain preliminary because mesh convergence, material-property sensitivity and the influence of interface constraints on local stress concentrations have not been fully evaluated.

The next development stage will focus on mesh convergence testing, assessment of numerical stiffness artefacts, refinement of vertebra–disc interface modelling and sensitivity analysis of the assigned material properties. These investigations will help establish the numerical reliability of the model before extending the workflow to patient-specific pathological anatomy.

A longer-term objective is to reconstruct pathological vertebral anatomy from publicly available CT imaging and investigate vertebral compression-fracture biomechanics.

If suitable longitudinal imaging becomes available, the workflow may subsequently be extended to investigate progressive vertebral collapse and kyphotic deformity.

No clinical conclusions regarding spinal stability, fracture progression or treatment selection can currently be drawn from these simulations.

## Explore the work

- [Spinal FEA GitHub repository](https://github.com/T-Y-Ang/spinal-fea)
- [Project README and computational workflow](https://github.com/T-Y-Ang/spinal-fea/blob/main/README.md)
- [Vertebra–disc interface methodology](https://github.com/T-Y-Ang/spinal-fea/blob/main/docs/interface_methodology.md)
- [Numerical validation progress](https://github.com/T-Y-Ang/spinal-fea/blob/main/docs/validation_progress.md)
- [Supporting literature](https://github.com/T-Y-Ang/spinal-fea/blob/main/docs/references.md)
- [Original L1–L2 mesh dataset](https://zenodo.org/records/3961334)
