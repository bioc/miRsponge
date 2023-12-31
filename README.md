# miRsponge R package

# Introduction
This package provides several functions to study miRNA sponge (also called ceRNA or miRNA decoy), including popular methods for identifying miRNA sponge interactions, and the integrative method to integrate miRNA sponge interactions from different methods, as well as the functions to validate miRNA sponge interactions, and infer miRNA sponge modules, conduct enrichment analysis of modules, and conduct survival analysis of modules.

# Installation
```{r echo=FALSE, results='hide', message=FALSE}
if (!requireNamespace("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
BiocManager::install("miRsponge", version = "3.8")
```

# A quick example to use miRsponge package
```{r echo=FALSE, results='hide', message=FALSE}
# Load the package
library(miRsponge)

# Identifying miRNA sponge interactions using the miRHomology method
miR2Target <- system.file("extdata", "miR2Target.csv", package="miRsponge")
miRTarget <- read.csv(miR2Target, header=TRUE, sep=",")
miRHomologyceRInt <- spongeMethod(miRTarget, method = "miRHomology")

# Validation of the identified miRNA sponge interactions
Groundtruthcsv <- system.file("extdata", "Groundtruth.csv", package="miRsponge")
Groundtruth <- read.csv(Groundtruthcsv, header=TRUE, sep=",")
spongenetwork_validated <- spongeValidate(miRHomologyceRInt[, 1:2], directed = FALSE, Groundtruth)

# Module identification from miRNA sponge interaction network
spongenetwork_Cluster <- netModule(miRHomologyceRInt[, 1:2], modulesize = 2)

# Disease and functional enrichment analysis of miRNA sponge modules
sponge_Module_DEA <- moduleDEA(spongenetwork_Cluster)
sponge_Module_FEA <- moduleFEA(spongenetwork_Cluster)

# Survival analysis of miRNA sponge modules
sponge_Module_Survival <- moduleSurvival(spongenetwork_Cluster, 
    ExpData, SurvData, devidePercentage=.5)
```

# License
GPL-3
