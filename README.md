# SEGs

### This repo is to host the R code of the analytical framework we proposed for identifying stably expressed genes (SEGs) from single-cell RNA-seq (scRNA-seq) data.

This framework a component of **scMerge** and it is used for identifying SEGs that can be subsequently used for scRNA-seq data normalisation and integration of multiple scRNA-seq data. For detail of **scMerge**, please refer to https://github.com/SydneyBioX/scMerge


The following is an example on using the framework to identify SEGs from a scRNA-seq dataset (E-MTAB-3929) that profiles early human development (Petropoulos et al. Cell, 2016). 

Note: the example data is a subset of the original dataset as to fulfil the upload size limit of github repo.

### Input 
The scRNA-seq dataset is assumed to be an log(TPM+1) (or log(RPKM+1), log(CPM+1), log(FPKM+1) etc.) matrix with rows correspond to genes and columns correspond to cells. Row names of the matrix should be gene symbols (or gene IDs).

When `cell_type` information is provided, F-statistics will be calculated and integrated into SEG index. If no such information is provided `cell_type = NULL`, the framework will use purely the expression characteristics of genes in single cells to calculate SEG index.

### Example
Please download `scSEGIndex.R` and `human_E_MTAB_3929_subset.RData` (both are hosted in this repo), and install all required packages (listed below) before testing the code below.

```{r}
require(distr)
require(scales)
require(parallel)
require(foreach)
require(doSNOW)

# source the functions
source("scSEGIndex.R")

# load the example dataset. This is a log(TPM) matrix with rows as genes and columns as cells
load("human_E_MTAB_3929_subset.RData")

# extract cell type information from colnames
cellTypeInfo <- colnames(logTPM)

# calculating stable expression index of each gene
SEGIndex.table <- scSEGIndex(logTPM, cell_type = cellTypeInfo, ncore = 2)

# sort the output table and display the top-20 genes
SEGIndex.table[order(SEGIndex.table[,1], decreasing=TRUE),][1:20,]
```
