# Patient-Derived Intracranial Aneurysm Computational Fluid Dynamics

**Computational haemodynamics | Neurovascular imaging | Numerical simulation**

**Status:** Exploratory study — preliminary results; further validation required  
**Dataset:** Royal Brisbane TOF-MRA Intracranial Aneurysm dataset, OpenNeuro ds005096  
**Software:** Python · Gmsh · TetGen · OpenFOAM 14 · ParaView

[← Back to AI portfolio](../README.md) · [Full interim technical report](https://github.com/T-Y-Ang/aneurysm-cfd/blob/main/docs/patient_mca_cfd_interim_report.md) · [Source-code repository](https://github.com/T-Y-Ang/aneurysm-cfd)

---

## Clinical question

**Can an open-source computational fluid dynamics workflow characterise blood-flow patterns and wall shear stress within a patient-derived intracranial aneurysm geometry?**

Intracranial aneurysm haemodynamics are of interest because local blood-flow patterns, wall shear stress and vascular geometry may contribute to aneurysm development and remodelling.

Computational fluid dynamics (CFD) provides a method of investigating these phenomena through numerical simulation. However, the reliability of a simulation depends on the anatomical model, computational mesh, boundary conditions and numerical assumptions.

This project explores both the feasibility of developing a patient-derived CFD workflow and the challenges involved in obtaining interpretable haemodynamic results.

## Study at a glance

| Component | Approach |
|---|---|
| Initial demonstration | Synthetic sidewall aneurysm |
| Patient-derived anatomy | Left middle cerebral artery aneurysm, OpenNeuro ds005096, subject `sub-038` |
| Geometry preparation | Surface cleaning, vessel clipping, boundary reconstruction and remeshing |
| Volume meshing | Tetrahedral meshing using TetGen |
| CFD solver | OpenFOAM Foundation 14 |
| Flow model | Steady-state, incompressible, laminar flow |
| Vessel model | Rigid walls with no-slip boundary conditions |
| Baseline mesh | 117,490 tetrahedral elements |
| Refined mesh | 221,743 tetrahedral elements |
| Completed analysis | Baseline flow simulation, global mass-conservation assessment and preliminary wall shear stress |
| Outstanding work | Outlet geometry correction, mesh-quality improvement, mesh-convergence testing and physiological sensitivity analysis |

## What I built

The project began with a synthetic sidewall aneurysm demonstration before progressing to an openly available patient-derived vascular surface model.

The patient-derived geometry underwent surface cleaning, vessel clipping, reconstruction of one inlet and two outlets, and surface remeshing. A closed, watertight computational surface was generated and used to construct a three-dimensional tetrahedral volume mesh.

![Patient-derived MCA aneurysm geometry](https://raw.githubusercontent.com/T-Y-Ang/aneurysm-cfd/main/outputs/figures/patient_aneurysm_geometry.png)

*Figure 1. Patient-derived left MCA aneurysm geometry prepared for preliminary CFD analysis. The outlet geometry remains under review.*

The baseline volume mesh contained 117,490 tetrahedral elements and 26,919 nodes. It was imported into OpenFOAM, with geometric dimensions converted from millimetres to metres.

A preliminary steady-state simulation was then performed using simplified blood-flow properties and boundary conditions.

| Parameter | Value |
|---|---|
| Blood density | 1,060 kg/m³ |
| Kinematic viscosity | 3.3019 × 10⁻⁶ m²/s |
| Inlet velocity | Uniform 0.10 m/s in the global X direction |
| Outlet conditions | Zero gauge kinematic pressure at both outlets |
| Vessel walls | Stationary, rigid and no-slip |

These assumptions were selected to establish a functioning numerical workflow. They do not represent subject-specific physiological measurements.

## Preliminary computational results

### Baseline flow simulation

The baseline OpenFOAM simulation completed at iteration 663.

Velocity streamlines were visualised to examine the computed flow field within the reconstructed aneurysm and branching vessels.

![Patient-derived MCA aneurysm velocity streamlines](https://raw.githubusercontent.com/T-Y-Ang/aneurysm-cfd/main/outputs/figures/patient_aneurysm_streamlines.png)

*Figure 2. Preliminary velocity streamlines from the baseline steady-state simulation. These patterns remain subject to geometric and numerical uncertainty.*

### Global mass conservation

The computed boundary flow rates were:

| Boundary | Volumetric flow rate |
|---|---:|
| Inlet | −8.3324684 × 10⁻⁷ m³/s |
| Outlet 1 | +3.4642152 × 10⁻⁷ m³/s |
| Outlet 2 | +4.8682530 × 10⁻⁷ m³/s |

The relative global mass-flow imbalance was approximately **0.0000024%**, based on the reported boundary fluxes.

This indicates excellent global mass conservation in the computed solution. However, mass conservation alone does not demonstrate anatomical accuracy, mesh independence or physiological validity.

### Wall shear stress

Wall shear stress was calculated from the OpenFOAM wall-shear-stress field and converted to pascals using the assumed blood density.

![Preliminary wall shear stress](https://raw.githubusercontent.com/T-Y-Ang/aneurysm-cfd/main/outputs/figures/patient_mca_wall_shear_stress.png)

*Figure 3. Preliminary wall shear stress distribution in the patient-derived MCA aneurysm model.*

Visualised wall shear stress values extended approximately from **0 to 19 Pa**.

These values have not been tested for sensitivity to mesh resolution, outlet geometry, boundary conditions or flow pulsatility. They cannot be used to infer aneurysm rupture risk.

## Mesh refinement and numerical reliability

A refined tetrahedral mesh was subsequently generated.

| Mesh characteristic | Baseline | Refined |
|---|---:|---:|
| Tetrahedral elements | 117,490 | 221,743 |
| Nodes | 26,919 | 43,840 |
| Enclosed volume | 169.834 mm³ | 169.834 mm³ |
| Underdetermined cells | 290 | 316 |
| Low-interpolation-weight faces | 43 | 56 |
| Faces exceeding 70° non-orthogonality | 3 | 5 |

Although the refined mesh preserved the enclosed volume and improved some aggregate quality measures, numerical-quality warnings remained.

The refined mesh was imported into OpenFOAM, but **a corresponding refined-mesh flow simulation has not yet been completed**. Consequently, mesh independence has not been established.

Further geometry inspection also raised concern that part of an original oblique surface cap may have remained near one vessel outlet after clipping. If present, this could artificially restrict the outlet and alter the predicted flow distribution.

The suspected artefact has not been confirmed or corrected. This uncertainty is an important reason for withholding physiological interpretation of the preliminary results.

## What I learned

**A completed numerical simulation is not equivalent to a validated model.** Solver completion and global mass conservation establish useful aspects of computational performance, but they do not demonstrate that the geometry or local haemodynamic predictions are reliable.

**Geometry preparation is central to computational haemodynamics.** Small changes in vessel clipping, outlet reconstruction and surface processing can potentially alter the computational lumen and resulting flow field.

**Mesh refinement must be evaluated rather than assumed to improve accuracy.** Increasing the number of elements does not automatically eliminate mesh-quality problems or establish convergence.

**Boundary conditions influence interpretation.** Uniform inlet velocity and equal-pressure outlets are useful simplifications for a demonstration, but patient-specific flow and downstream vascular resistance may be important for more realistic modelling.

**Clinical interpretation requires a higher standard of verification.** Wall shear stress visualisations are informative computational outputs, but their clinical significance cannot be established without appropriate geometric, numerical and physiological validation.

## Limitations and future directions

The main limitations are the unresolved outlet geometry concern, persistent mesh-quality warnings, absence of a mesh-convergence study and simplified steady-state boundary conditions.

The model also assumes Newtonian, incompressible flow through rigid vessel walls and does not account for pulsatility or vessel-wall deformation.

Future development would prioritise correcting the suspected outlet artefact, rebuilding the computational meshes, comparing baseline and refined solutions, and testing more physiologically appropriate inlet and outlet conditions.

These are proposed investigations rather than completed analyses.

**The present findings are exploratory and must not be used for aneurysm rupture-risk prediction, treatment selection or individual patient management.**

## Explore the work

- **[Read the full interim technical report](https://github.com/T-Y-Ang/aneurysm-cfd/blob/main/docs/patient_mca_cfd_interim_report.md)** — geometry preparation, meshing, numerical methods, preliminary results and outstanding limitations.
- **[Browse the CFD repository](https://github.com/T-Y-Ang/aneurysm-cfd)** — Python scripts, OpenFOAM configurations, mesh diagnostics and ParaView visualisation files.
- **[Return to the main AI portfolio](../README.md)** — other projects in clinical machine learning and computational neurosurgery.

**Project status:** Exploratory computational demonstration, temporarily paused while spinal finite element analysis is prioritised.

*Educational and research demonstration only; not a validated clinical haemodynamic model.*
