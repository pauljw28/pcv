# Procrustes cross-validation

This repository contains implementation of Procrustes cross-validation in R and MATLAB. The current version of the code is *0.1.1*. The code can also be downloaded as zip archive from [Release](https://github.com/svkucheryavski/pcv/releases) section.

## Reference

You can find detailed description of the method with numerous examples in this paper:

Kucheryavskiy, S., Zhilin, S., Rodionova, O., & Pomerantsev, A. *Procrustes Cross-Validation—A Bridge between Cross-Validation and Independent Validation Sets.* Analytical Chemistry,  92 (17), 2020. pp.11842–11850. DOI: [10.1021/acs.analchem.0c02175](https://doi.org/10.1021/acs.analchem.0c02175)

Please cite this paper if you use this method in your research.

The next paper shows applicability and efficiency of the proposed method in case of short datasets:

Pomerantsev A. L., Rodionova O. Ye. *Procrustes Cross-Validation of short datasets in PCA context.*
Talanta, 226, 2021. DOI: [10.1016/j.talanta.2021.122104](https://doi.org/10.1016/j.talanta.2021.122104)

## Short description

Procrustes cross-validation is a new approach for validation of chemometric models. It makes possible to generate a new dataset, named *pseudo-validation set* and use it for validation of models in the same way as with an independent test set. The generation is done using following algorithm:

1. A global PCA model is created using calibration set **X** and *A* components
2. The rows of matrix **X** are split into *K* segments using venetian blinds splitting
3. For each segment *k* from *{1, 2, ..., K}*:
    * a local PCA model is created using the rows from the segment
    * an angle between the local and the global model is estimated
    * rows from the current segment are rotated in original variable space by the angle
4. All rotated measurements are then combined into a matrix with pseudo-validation set

So, pseudo-validation set is built on top of the calibration set but has its own sampling error. Since it is not independent from the calibration set we recommend to limit its use by model optimization and do not use it for assessment of performance of the final model.

## How to use in R

All code is located in `pcv.R`, no extra packages are needed. Simply copy the file to your working directory and use `source("pcv.R")` inside your script to load all necessary functions to the environment. Then the syntax is follows:

```r
pvset <- pcv(X, ncomp, nseg, scale)
```

Here `X` is your calibration set (as a numerical matrix), `ncomp` is a number of principal components for PCA decomposition (use at lease enough to explain 95-99% of variation of the data values), `nseg` is the number of segments for cross-validation procedure. So far only systematic cross-validation (venetian blinds) is supported, so make sure that rows of `X` are sorted correctly or shuffled. Parameter `scale` allows to standardize your data prior to the generation, which is useful if your variables have different nature and/or units. The generated data will be unscaled though.

File `demo.R` contains a demo code based on *NIRSim* dataset from the paper. See comments in the code for more details.

## How to use in MATLAB

See details for R implementation above.  To use it in MATLAB you have to copy file `pcv.m` to your script folder (or any other folder MATLAB knows path to). Then use the following syntax:

```matlab
pvset = pcv(X, nComp, nSeg, Scale);
```

File `demo.m` contains a demo code based on *NIRSim* dataset from the paper. See comments in the code for more details.

## Bugs and improvements

The code will be improved and extended gradually. If you found a bug please report using issues or send an email.
