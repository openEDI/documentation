# Eagle Jobs

## Description

HPC dataset with 11M+ jobs from NREL's Eagle supercomputer. These jobs were submitted to run on Eagle between Nov 2018 and Feb 2023. The data are sufficiently anonymized and do not include sensitive user or project data. HPC research community does not have many public, large, and complete job traces like this one, and release of this dataset should help address this gap.  

## Data

The data is available through OEDI: [https://data.openei.org/submissions/5860](https://data.openei.org/submissions/5860).

This link points to a repository that contains a single large file: `eagle_data.parquet`, which uses parquet, a column-oriented data file format with compression.

## References

Published Paper:
 
[Mastering HPC Runtime Prediction: From Observing Patterns to a Methodological Approach](https://www.nrel.gov/docs/fy23osti/86526.pdf)

For citing our research, please use:

```
@incollection{menear2023mastering,
  title={Mastering HPC Runtime Prediction: From Observing Patterns to a Methodological Approach},
  author={Menear, Kevin and Nag, Ambarish and Perr-Sauer, Jordan and Lunacek, Monte and Potter, Kristi and Duplyakin, Dmitry},
  booktitle={Practice and Experience in Advanced Research Computing},
  pages={75--85},
  year={2023}
}
```

You can also cite the dataset we produced and released:
 
```
@div{oedi_5860,
  title = {NREL Eagle supercomputer jobs},
  author = {Duplyakin, Dmitry, Menear, Kevin.},
  abstractNote = {High performance computing dataset with 11M+ jobs from NREL's Eagle supercomputer. These jobs were submitted to run on Eagle between Nov 2018 and Feb 2023. The data are sufficiently anonymized and do not include sensitive user or project data. HPC research community does not have many public, large, and complete job traces like this one, and releasing this dataset should help address this gap.},
  url = {https://data.openei.org/submissions/5860},
  place = {United States},
  year = {2023},
  month = {02}
}
```

## Disclaimer and Attribution

To be added later.
