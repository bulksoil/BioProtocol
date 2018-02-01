# Sequence Wrangling

## Introduction
The first part of any microbiome analysis is download, demultiplex, clean, and cluster the data. Here, we will give some basic instructions on how to complete these tasks. 

## Before starting
If you want to follow along with this tutorial, you will need to download a few things
	
- [BananaStand](https://github.com/bulksoil/BananaStand) is a pipeline and wrapper for conducting many of the steps involved in the up-front sequence processing.
- [PANDAseq](https://github.com/neufeld/pandaseq) is used by BananaStand to merge paired end amplicons into contiguous sequences.
- [NINJA-OPS](https://github.com/GabeAl/NINJA-OPS) is the OTU calling pipeline used to quickly cluster sequences.
- [BIOM Format](http://biom-format.org/) is the output format of the OTU table from NINJA-OPS. You will eventually need to convert it to a .tsv file

Visit the homepages for each of these packages/software and follow their installation instructions. If you have any problems installing BananaStand please contact Joe Edwards at edwards@ucdavis.edu.


## The Data
The data used for this tutorial were published in the recent paper by Santos-Medellín et al. [Drought stress results in a compartment-specific restructuring of the rice root-associated microbiomes. MBio, 2017](http://mbio.asm.org/content/8/4/e00764-17.short) where the authors survey the root associated microbiota of rice plants grown in multiple soils and exposed to well-watered or drought conditions. Here, we are only including the data taken from the well-watered plants.

To download the data, please visit this [Google Drive Folder](https://goo.gl/tCMLBn). You will notice that there are two folders - Lib1 and Lib2. The data collected for this experiment were sequenced across two separate MiSeq runs. The barcodes used between the two experiments overlap, therefore the files can not simply be concatenated prior to processing. In each folder there are 5 files. 

- Undetermined_S0_L001_R1_001.fastq.gz A 250 bp read starting from the end of the 515F primer. Reads 5' to 3'
- Undetermined_S0_L001_R2_001.fastq.gz A 250 bp read starting from the 806R primer. Reads 3' to 5'
- Undetermined_S0_L001_I1_001.fastq.gz The 12 bp 5' barcode
- Undetermined_S0_L001_I2_001.fastq.gz Te 12 bp 3' barcode
- lib1_map.tsv (or lib2_map.tsv) The mapping file which contains the information about each sample including the barcode. The barcode column is simply a concatenation of the forward and reverse barcode used for each sample.

It is important to note that the fastq files do not need to be unzipped for processing. Please note that if the user would like to replicate this workflow in their own data, then the SampleID column should be first and the barcode column should be second in the tab separated file.

## Sequence Processing

We will use the mpipe.py pipeline from the BananaStand repo to process the data. Let's start with Lib1. 

```
mpipe.py -m lib1_map.tsv --read1 Undetermined_S0_L001_R1_001.fastq.gz -- read2 Undetermined_S0_L001_I1_001.fastq.gz --read3 Undetermined_S0_L001_I2_001.fastq.gz --read4 Undetermined_S0_L001_R2_001.fastq.gz -m lib1
```

The pipeline is first demultiplexing the sequences and assigning each sequence to a sample. It next forms contiguous reads using PANDAseq. Then it filters out reads with low quality bases or reads that are too long. At the end of the processing there should be a file `lib1seqs.fa`. This is the file containing the final seqs for this library.

Next, run the exact same thing from within the Lib2 directory.
```
mpipe.py -m lib2_map.tsv --read1 Undetermined_S0_L001_R1_001.fastq.gz -- read2 Undetermined_S0_L001_I1_001.fastq.gz --read3 Undetermined_S0_L001_I2_001.fastq.gz --read4 Undetermined_S0_L001_R2_001.fastq.gz -m lib2
```

Now there should be a lib2seqs.fa file present.


