# SNP Context
----------

## Summary

    Corey Carter, St. Paul, MN - 09 January 2018 for Morrell Lab at the University of Minnesota - Twin Cities
    This bash script is designed to pull contextual sequence flanking a SNP (variants) from a reference genome.
    The window size for contextual sequence can be user specificed.
    The program checks for the presence of more than one variant within user specified windows.

## Config file
SNP Context can be used from either the command line or through a config file, with the config file being the perferred method. Listed below are the script input parameters:

###General input for config file
*Project name: The name of your project, the working directory will share this name.
*input email: Sends user PBS jobs updates. If and only if you're using this script on the University Of Minnesota's supercomputing cluster. *reference genome: The reference genome for used for .VCF transformation into .FASTA files.
*input file: The input .VCF file. 
*output location: The output location of your files, a project dirtory is created at this location. If no locatation is specified, then directory and files will be saved at the same location as the input file.
*

###MSI options for config file
*MSI Mode: A boolean (true or false) question. If true, then the script will automatically submit your job to PBS job scheduler and allows for the user to save the output text files to MSI S3 tier two storage.

*PBS parameters: Allows you to change the time and amount of resources allocated to the PBS job. See [MSI PBS job submission](https://www.msi.umn.edu/content/job-submission-and-scheduling-pbs-scripts)

###Mutation Motif
*Window Length: Controls the window length expansion around the SNP location.

*Flanks: (0-2) Choose between 0 to 2 nucleotides around the SNP location for analysis. The window length must be equal or greater than the Flank size - otherwise if the window is smaller than the flanks, the window length expansion (parameter above) will default to the flank size.  

*indel maximum amount: The maxmimum allowed indels in the .FASTA file, beyond the SNP location and 5 base pairs around it. Any indels found within a 2 base pair flanking of the SNP location will be automatically filtered out. Minimum indel thershold is 25%.

###Saving Options
*Save all data: (boolean true or fale) There are many intermediate files created during SNP Context. This parameter allows you to save all intermediate files to your output directory; below is a complete list of output files if save all data is true: 

*Save S3: (interger, 0 - 3) This allows you to save the mutaion motif outputs files (Count Tables) and config files to S3. The user can choose between these values (0 - 3):
	0. Don't save to any S3 storage.
	1. Save to MSI's S3 tier two storage 
	2. Save to Amazons external S3 service (Needs .aws set up)
	3. Save to **both** MSI S3 storage and Amazons S3 storage platforms.

*Dropbox: (boolean, true or false) The user can save the unique output file (counts tables and config file) to their personal dropbox app. 

*dropbox auth: The authication code, needed for saving Dropbox files from the script. Follow this link for instructions on setting this up. **NOTE: this data is redacted when uploaded to S3, Dropbox or Github.**

*Github:  

## Outputs
Snp Contexts generates numerous output files:
* .BAM
* .FASTA
* Rejected nuleotide positions from .VCF, .BAS and .FASTA files
* Word Counts tables
* Combined Counts tables

If he parameter `all data` is `true`, than all intermediate files are save in the output directory, if `all data` is `false` than only the Word and Combined Counts Tables and rejected files (.VCF, .BAM, .FASTA) are saved in the outout drictory.

