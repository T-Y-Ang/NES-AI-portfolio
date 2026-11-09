# Lumbar Spine Finite Element Analysis

**Computational biomechanics | Spinal mechanics | Numerical simulation**

**Status:** Exploratory study — baseline compression simulations completed; mechanical validation ongoing  
**Dataset:** Existing L1–L2 finite element mesh, [Zenodo record 3961334](https://zenodo.org/records/3961334)  
**Software:** Python · CalculiX 2.21 · SciPy · PyVista · ParaView

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
| Completed analysis | Baseline compression simulations, force-equilibrium assessment, interface sensitivity analysis and geometric investigation |
| Outstanding work | Stress-field visualisation, mesh convergence, interface refinement and biomechanical validation |

## What I built

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

These parameters were selected for an exploratory computational demonstration and do not represent a fully validated physiological spinal model.

Two simulations were completed to investigate the effect of vertebra–disc tie tolerance on the predicted mechanical response.

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

*Figure 1. Computational interface geometry showing the L1 inferior endplate, inner and outer disc interface surfaces, and 29 rejected tie nodes.*

### L2 superior endplate–disc interface

![L2 superior endplate and rejected tie nodes](https://raw.githubusercontent.com/T-Y-Ang/spinal-fea/main/outputs/figures/l2_interface_tie_failures.png)

*Figure 2. Computational interface geometry showing the L2 superior endplate, inner and outer disc interface surfaces, and 21 rejected tie nodes.*

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

The next development stage will focus on displacement and stress-field visualisation, assessment of numerical artefacts and further mechanical verification.

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