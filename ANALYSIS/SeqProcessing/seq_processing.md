# Sequence Wrangling

## Introduction
The first part of any microbiome analysis is download, demultiplex, clean, and cluster the data. Here, we will give some basic instructions on how to complete these tasks. 

## Before starting
If you want to follow along with this tutorial, you will need to download a few things
	
-[BananaStand](https://github.com/bulksoil/BananaStand) is a pipeline and wrapper for conducting many of the steps involved in the up-front sequence processing.
- [PANDAseq](https://github.com/neufeld/pandaseq) is used by BananaStand to merge paired end amplicons into contiguous sequences.
- [NINJA-OPS](https://github.com/GabeAl/NINJA-OPS) is the OTU calling pipeline used to quickly cluster sequences.
- [BIOM Format](http://biom-format.org/) is the output format of the OTU table from NINJA-OPS. You will eventually need to convert it to a .tsv file

Visit the homepages for each of these packages/software and follow their installation instructions. If you have any problems installing BananaStand please contact Joe Edwards at edwards@ucdavis.edu.


## The Data
The data used for this tutorial were published in the recent paper by Santos-Medellín et al. [Drought stress results in a compartment-specific restructuring of the rice root-associated microbiomes. MBio, 2017](http://mbio.asm.org/content/8/4/e00764-17.short) where the authors survey the root associated microbiota of rice plants grown in multiple soils and exposed to well-watered or drought conditions. Here, we are only including the data taken from the well-watered plants.

To download the data, please visit this [Google Drive Folder](https://goo.gl/tCMLBn). You will notice that there are two folders - Lib1 and Lib2. The data collected for this experiment were sequenced across two separate MiSeq runs. The barcodes used between the two experiments overlap, therefore the files can not simply be concatenated prior to processing.