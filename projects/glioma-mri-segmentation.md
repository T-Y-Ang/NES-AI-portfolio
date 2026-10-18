# Glioma MRI Segmentation

**Medical imaging | Deep learning | Computational neuro-oncology**  
**Status:** Completed experimental study and independent held-out evaluation  
**Dataset:** BraTS 2024 Glioma (GLI)  
**Framework:** Python · PyTorch · MONAI · 3D U-Net

[← Back to AI portfolio](https://github.com/T-Y-Ang/NES-AI-portfolio) · [Full research report](https://github.com/T-Y-Ang/glioma-mri-segmentation/blob/main/reports/glioma_segmentation_report.md) · [Source-code repository](https://github.com/T-Y-Ang/glioma-mri-segmentation)

---

## Clinical question

**Can a deep-learning model identify tumour-associated abnormalities and postoperative resection cavities on multimodal glioma MRI, and how reliably do its predictions correspond to clinically recognisable anatomy?**

Segmentation of glioma-related abnormalities is relevant to quantitative imaging research, longitudinal assessment and potential future surgical-planning tools. Manual delineation is labour-intensive, while automated segmentation raises an important question: **does numerical agreement with reference masks necessarily imply anatomical correctness?**

This project was developed to explore both the technical workflow of volumetric MRI segmentation and the limitations of interpreting model performance without reviewing the underlying imaging.

## Study at a glance

| Component | Approach |
| --- | --- |
| Imaging dataset | BraTS 2024 GLI; 1,350 MRI examinations from 613 subjects |
| MRI inputs | Native T1 (T1n), contrast-enhanced T1 (T1c), T2-weighted (T2w), and T2-FLAIR (T2f) |
| Model | Multilabel 3D U-Net, implemented with PyTorch and MONAI |
| Predicted regions | Whole tumour (WT), tumour core (TC), enhancing tumour (ET), and resection cavity (RC) |
| Model development | Initial training subset of 180 examinations; validation subset of 24 examinations |
| Independent evaluation | 198 held-out MRI examinations from 92 subjects |
| Safeguard against leakage | Subject-level separation between training, validation and held-out test cohorts |
| Primary evaluation | Region-specific Dice scores and macro-average Dice |
| Additional analyses | False-positive cavity predictions, validation-informed post-processing, paired comparisons, subject-clustered bootstrap uncertainty, anatomical failure review |

The initial training subset was intentionally limited to establish and evaluate a complete workflow on consumer workstation hardware. The held-out test cohort was larger and was not used to select the final checkpoint or tune post-processing.

## What I built

The pipeline loads four MRI sequences, standardises their orientation, performs intensity normalisation and foreground cropping, and converts reference labels into four segmentation targets. A 3D U-Net learns to predict the four regions from volumetric MRI patches. Full-volume predictions are reconstructed through sliding-window inference.

After training, I investigated errors in resection-cavity segmentation. The model sometimes labelled normal cerebrospinal-fluid spaces or heterogeneous tumour-associated tissue as a resection cavity. I then compared the original model with a **relative-confidence post-processing rule** designed to reduce conflicts between enhancing-tumour and resection-cavity predictions. The correction was chosen using validation data before evaluation on the held-out test set.

## Independent held-out results

**198 MRI examinations from 92 subjects** were evaluated using the pre-specified protocol.

| Region | Baseline mean Dice | After correction |
| --- | ---: | ---: |
| Whole tumour (WT) | 0.8126 | 0.8126 |
| Tumour core (TC) | 0.6298 | 0.6298 |
| Enhancing tumour (ET) | 0.6456 | 0.6456 |
| Resection cavity (RC) | 0.5221 | 0.5638 |
| **Macro-average across regions** | **0.6525** | **0.6630** |

*Regional mean Dice scores were calculated among examinations containing the corresponding reference region. The macro-average is the unweighted mean of these four regional means. The correction modified only the resection-cavity channel.*

Among **157 examinations with an annotated resection cavity**, post-processing increased mean RC Dice by **0.0417** (95% subject-clustered bootstrap confidence interval **+0.0227 to +0.0625**). RC Dice improved in **111 examinations**, deteriorated in **22**, and was unchanged in **24**. The intervention also reduced the total false-positive cavity voxel burden by **70.2%** across the 41 examinations without an annotated cavity, although none became completely free of false-positive cavity predictions.

These results show a positive average effect, but **not uniform improvement**. The mean paired RC Dice improvement (+0.0417) was much larger than the median (+0.0015), suggesting that larger improvements in a subset of examinations contributed substantially to the average.

## Anatomical failure analysis: why a better score is not always a better segmentation

![Held-out test anatomical failure case](https://github.com/T-Y-Ang/glioma-mri-segmentation/blob/main/outputs/figures/held_out_test_worst_case_reference_anatomy.png?raw=true)

*Representative held-out case selected for anatomical review. After post-processing, the resection-cavity Dice score fell from **0.5345 to 0.0046**. Review of the multimodal MRI and reference mask raised the possibility of ambiguity in the reference annotation; an annotation error has **not** been independently established.*

This case is particularly important because it challenges a simple interpretation of segmentation scores. A model can improve its **average** agreement with annotations while producing substantially worse predictions in individual examinations. Furthermore, apparent disagreement with a reference mask may warrant review of both the model output **and** the annotation, rather than automatically attributing every difference to the model.

Earlier validation review also identified false-positive cavity predictions involving normal CSF-containing spaces and tumour-associated tissue. These observations motivated the post-processing experiments and illustrated the value of combining quantitative evaluation with clinical neuroanatomical interpretation.

## What I learned

- **Data separation matters:** multiple MRI examinations from the same subject must not be distributed across training and evaluation cohorts.
- **A single metric is insufficient:** Dice scores among region-present cases can conceal false-positive predictions in examinations where a region is absent.
- **Post-processing has trade-offs:** reducing false-positive cavity voxels can also remove correctly predicted cavity tissue and reduce recall.
- **Average improvement can conceal individual harm:** paired case-level results and anatomical failure analysis are essential complements to aggregate metrics.
- **Reference annotations deserve scrutiny:** quantitative disagreement does not, by itself, prove that either the model or the annotation is anatomically correct.
- **Reproducibility is part of the research:** checkpoints, validation-informed decisions and a frozen test protocol help distinguish model development from independent evaluation.

## Limitations and future directions

This was a retrospective, single-benchmark-dataset experiment, with initial model training performed on a relatively small subset of the available examinations. It does not demonstrate generalisability to other institutions, scanners or clinical populations. The model has not undergone prospective or external clinical validation and **must not be used for patient care or surgical decision-making**.

Potential future work includes more extensive model training, better representation of difficult negative examples, anatomically constrained segmentation, external validation, and exploration of 3D visualisation or surgical-planning workflows. These are proposed directions, not completed clinical applications.

## Explore the work

- **[Read the full scientific report](https://github.com/T-Y-Ang/glioma-mri-segmentation/blob/main/reports/glioma_segmentation_report.md)** — detailed methodology, experiments, statistics and anatomical analysis.
- **[Browse the public GitHub repository](https://github.com/T-Y-Ang/glioma-mri-segmentation)** — source code, notebooks, documentation and selected outputs.
- **[Return to the main AI portfolio](https://github.com/T-Y-Ang/NES-AI-portfolio)** — TBI outcome prediction, glioma segmentation and planned EEG seizure detection.

*Educational and exploratory research only; not a clinically validated diagnostic or treatment tool.*
