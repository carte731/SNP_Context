# SNP Context

    Corey Carter, St. Paul, MN - 09 January 2018 for Morrell Lab at the University of Minnesota - Twin Cities
    This bash script is designed to pull contextual sequence flanking a SNP (variants) from a reference genome.
    The window size for contextual sequence can be user specificed.
    The program checks for the presence of more than one variant within user specified windows.

## Inputs
----
SNP Context accepts .VCF files.

## Outputs
----
Snp Contexts generates numerous output files:
* .BAM
* .FASTA
* Rejected nuleotide positions from .VCF, .BAS and .FASTA files
* Word Counts tables
* Combined Counts tables

If he parameter `all data` is `true`, than all intermediate files are save in the output directory, if `all data` is `false` than only the Word and Combined Counts Tables and rejected files (.VCF, .BAM, .FASTA) are saved in the outout drictory.

