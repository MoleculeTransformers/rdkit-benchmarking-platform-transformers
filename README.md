# Benchmarking Platform

original version presented in
S. Riniker, G. Landrum, J. Cheminf., 5, 26 (2013),
DOI: 10.1186/1758-2946-5-26,
URL: http://www.jcheminf.com/content/5/1/26

extended version presented in
S. Riniker, N. Fechner, G. Landrum, J. Chem. Inf. Model., 53, 2829, (2013),
DOI: 10.1021/ci400466r,
URL: http://pubs.acs.org/doi/abs/10.1021/ci400466r

## Model List

Our released models are listed as following. You can import these models by using the `smiles-featurizers` package or using [HuggingFace's Transformers](https://github.com/huggingface/transformers).
| Model | Type |
|:-------------------------------|:--------:|AUROC| BEDROC|
| [shahrukhx01/smole-bert](https://huggingface.co/shahrukhx01/smole-bert) | `Bert`|0.615 $\pm$ 0.18| 0.225 $\pm$ 0.102|
| [shahrukhx01/smole-bert-mtr](https://huggingface.co/shahrukhx01/smole-bert-mtr) | `Bert`|0.621 $\pm$ 0.121| 0.262 $\pm$ 0.113|
| [shahrukhx01/smole-bart](https://huggingface.co/shahrukhx01/smole-bart) | `Bart`|0.660 $\pm$ 0.104| 0.263 $\pm$ 0.111|
| [shahrukhx01/muv2x-simcse-smole-bart](https://huggingface.co/shahrukhx01/muv2x-simcse-smole-bert) | `Simcse`|0.697 $\pm$ 0.091| 0.270 $\pm$ 0.109|
| [shahrukhx01/siamese-smole-bert-muv-1x](https://huggingface.co/shahrukhx01/siamese-smole-bert-muv-1x) | `SentenceTransformer`|0.673 $\pm$ 0.086| 0.274 $\pm$ 0.108|

## GENERAL USAGE NOTES

The virtual-screening process implemented by the benchmarking
platform is divided into three steps:

0. Setup

```bash
cd scoring/data_sets_1/
```

```bash
mkdir <name-of-fingerprints-directory>
```

1. Scoring

```bash
python calculate_scored_lists_transformers.py -n 5 -f config.txt -s EmbedCosine -m shahrukhx01/smole-bert -t bert -o <name-of-fingerprints-directory>
```

2. Validation

```bash
cd validation/data_sets_1/
```

```bash
cp -r ../scoring/data_sets_1/<name-of-fingerprints-directory> .
```

```bash
mkdir <name-of-validation-results-directory>
```

```bash
python calculate_validation_methods.py -m methods.txt -i fingerprints  -o <name-of-validation-results-directory> /
```

3. Analysis

```bash
cd analysis/data_sets_1/
```

```bash
cp -r ../validation/data_sets_1/<name-of-validation-results-directory> .
```

```bash
mkdir <name-of-analysis-results-directory>
```

```bash
python run_analysis.py -i validation_results -o <name-of-analysis-results-directory>/
```

The three steps are run separately and read in the output of the
previous step. In the scoring step, the data from the directories
compounds and query_lists is read in.

The directory compounds contains lists of compounds for 118 targets
from three public data sources: MUV, DUD and ChEMBL. The compound
lists contain the external ID, the internal ID and the SMILES of
each compound.

There are three subsets of targets available:

subset I:
88 targets from MUV, DUD & ChEMBL described in J. Cheminf., 5, 26 (2013)

subset I filtered:
69 targets from MUV, DUD & ChEMBL filtered for difficulty
described in JCIM (2013), online

subset II:
37 targets from ChEMBL designed for a second VS use case
described in JCIM (2013), online

The directory query_lists contains training lists for each target
with the indices of randomly selected active and inactive molecules.
Training lists with 5, 10 or 20 active molecules are available.
The number of training decoys is 20 % of the decoys for subsets I
and 10 % for subset II.

The scripts are written in Python and use the open-source
cheminformatics library RDKit (www.rdkit.org) and
machine-learning library scikit-learn (www.scikit-learn.org).

Running a script with the option [--help] gives a description of the
required and optional input parameters of the script.
