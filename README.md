# Mechanistic alignment in asymmetric hydrogenation

Code for **Beyond Structural Similarity: Mechanistic Alignment Guides Transfer Learning in Asymmetric Hydrogenation of Olefins**.

The notebooks analyze transfer learning from Ru-catalyzed asymmetric ketone hydrogenation (Ru-AHK) to asymmetric olefin hydrogenation (Ru-AHO), compare source-data selection methods, and evaluate predictions on an external Ru-AHO dataset.

## Notebooks

| Notebook | Contents |
| --- | --- |
| [0.1 — Data preparation](0_1_clean_ru_aho_and_build_ru_ahk_domains.ipynb) | Clean the original AHO database and select the Ru-AHO target reactions. Clean the Ru-AHK records, retain the supplied mechanism labels, remove duplicates, and export inner- and outer-sphere source datasets. |
| [0.2 — Dataset overview](0_2_dataset_overview_distribution.ipynb) | Summarize reaction and structure counts, selectivity distributions, substrate and ligand MACCS–PCA projections, exact structure overlap, and selectivity-distribution overlap. Export plots and their numerical source data. |
| [0.3 — External dataset preparation](0_3_clean_and_export_recent_AHO_dataset.ipynb) | Clean and deduplicate the recent Ru-AHO literature records and export the external validation table. |
| [1.1 — MACCS source comparison](1_1_maccs_source_comparison.ipynb) | Compare target-only training with inner- and outer-sphere Ru-AHK transfer using MACCS features, 10 regression algorithms, mixed transfer, and delta learning. Export pooled out-of-fold metrics and plots. The final section evaluates the combined Ru-AHK source. |
| [1.2 — SPOC source comparison](1_2_spoc_source_comparison.ipynb) | Perform the source comparison using SPOC features. Export pooled out-of-fold metrics and plots, and evaluate the combined Ru-AHK source in the final section. |
| [1.3 — MACCS source selection](1_3_maccs_source_domain_extension_deeper_blue.ipynb) | Compare mechanism-aligned selection with substrate-, ligand-, and reaction-representation-similarity selection using equal-sized source sets. Export prediction metrics, source selections, and PCA maps. |
| [1.4 — SPOC source selection](1_4_spoc_source_domain_extension_deeper_blue.ipynb) | Perform the source-selection comparison using SPOC features and export metrics, source selections, and PCA maps. |
| [2.1 — External validation](2_1_recent_AHO_external_validation_kde.ipynb) | Select ensemble members from the internal MACCS and SPOC results, refit them on the internal data, and evaluate their averaged predictions on the external dataset. Export ensemble configurations, predictions, metrics, and distribution and performance plots. |
| [2.2 — Individual reaction examples](2_2_individual_reaction_examples.ipynb) | Select illustrative reactions from the external ensemble predictions using absolute-error thresholds. Export candidate and selected records, prediction comparisons, and molecular-structure SVGs. This is a post hoc example selection; no models are trained. |

## Data

The original AHO database is not included in this repository. Obtain it from the original publication and the authors' associated resources:

- *Towards Data-Driven Design of Asymmetric Hydrogenation of Olefins: Database and Hierarchical Learning*. [DOI: 10.1002/anie.202106880](https://doi.org/10.1002/anie.202106880).
- [Original repository](https://github.com/licheng-xu-echo/AHO) and [AHO dataset archive](https://github.com/licheng-xu-echo/AHO/blob/main/AHO-Dataset.zip).
- [Database website listed by the original authors](http://asymcatml.net/).

Prepare the original AHO input as `data/raw_data/aho_dataset.csv` with the columns required by notebook 0.1. The upstream archive may contain multiple files; downloading it alone does not create this CSV.

All other raw datasets used in this study are provided in the repository:

| Input path | Contents |
| --- | --- |
| `data/raw_data/ahk_dataset_Ru.csv` | Ru-AHK literature records with mechanistic annotations |
| `data/raw_data/ru_aho_2020_2025.csv` | External Ru-AHO literature records |

Notebook 0.1 generates `data/transfer/ru_aho_inner.csv`, `data/transfer/ru_ahk_inner.csv`, `data/transfer/ru_ahk_outer.csv`, and `data/all_ahk_Ru/processed_ahk_dataset_Ru.csv`. Notebook 0.3 generates `data/recent_aho/recent_aho_dataset.csv`.

## Running the notebooks

Dependencies include Python, Jupyter, NumPy, pandas, SciPy, scikit-learn, RDKit, Matplotlib, joblib, and IPython. Use the study's software environment to reproduce its numerical results.

Keep the notebooks and the `data/` directory within the same project root. Run cells in order within each notebook.

1. Run **0.1** to prepare the internal datasets.
2. Run **0.2** for the dataset overview and **0.3** to prepare the external dataset.
3. Run **1.1** and **1.2** for the internal source comparisons.
4. Run **1.3** and **1.4** for the source-selection comparisons.
5. Run **2.1** after **0.3**, **1.1**, and **1.2**. It reads the internal metric tables to select ensemble members.
6. Run **2.2** after **2.1** to select and export reaction examples.

## Features and evaluation

MACCS features concatenate fingerprints in the order **substrate, product, additive, solvent, ligand**, followed by pressure, temperature, and S/C. SPOC uses the component order **substrate, product, ligand, solvent, additive**, combining fingerprints and molecular descriptors with reaction conditions and categorical encodings.

The internal comparisons use five target folds and pooled out-of-fold R², MAE, and RMSE. Similarity-based source selection uses the target training reactions within each fold. The final combined-source sections in 1.1 and 1.2 print R² and MAE and retain their results in memory.

External ensemble members are selected by internal out-of-fold performance. Each scenario uses an equal-weight average of three MACCS and three SPOC settings by default. The complete external set is used for performance evaluation; the examples in 2.2 are selected afterward.

## Outputs and caches

| Notebook | Output directory |
| --- | --- |
| 0.2 | `results/ru_ahk_aho_dataset_overview/` |
| 1.1 | `results/1_1_maccs_mechanism_transfer/` |
| 1.2 | `results/1_2_spoc_mechanism_transfer/` |
| 1.3 | `results/1_3_maccs_source_domain_extension/` |
| 1.4 | `results/1_4_spoc_source_domain_extension/` |
| 2.1 | `results/2_1_recent_aho_external_validation/` |
| 2.2 | `results/2_2_individual_reaction_examples/` |

The notebooks reuse cached features, predictions, or fitted models where configured. After changing input data or modeling settings, use the relevant recomputation switches to rebuild the affected caches. Plot labels and legends are controlled by the switches in each notebook.
