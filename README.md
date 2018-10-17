# SNP Context
----------

## Summary

    Corey Carter, St. Paul, MN - 09 January 2018 for Morrell Lab at the University of Minnesota - Twin Cities
    This bash script is designed to pull contextual sequence flanking a SNP (variants) from a reference genome.
    The window size for contextual sequence can be user specificed.
    The program checks for the presence of more than one variant within user specified windows.

## Config file
SNP Context can be used from either the command line or through a config file, with the config file being the perferred method. Listed below are the script input parameters:

### General input for config file
* **Project name (String)**: The name of your project, the working directory will share this name.

* **Input email (String)**: Sends user PBS jobs updates. If and only if you're using this script on the University Of Minnesota's supercomputing cluster. *reference genome: The reference genome for used for .VCF transformation into .FASTA files.

* **Input file (String)**: The input .VCF file. 

* **referenceGenome (String)**: The complete path (pwd -P) of your reference genome.

* **Output location (String)**: The output location of your files, a project dirtory is created at this location. If no locatation is specified, then directory and files will be saved at the same location as the input file.

### MSI options for config file
* **MSI Mode (Boolean, true or false)**: If true, then the script will automatically submit your job to PBS job scheduler and allows for the user to save the output text files to MSI S3 tier two storage.

* **PBS parameters (Intergers)**: Allows you to change the time and amount of resources allocated to the PBS job. See [MSI PBS job submission](https://www.msi.umn.edu/content/job-submission-and-scheduling-pbs-scripts)

### Mutation Motif
* **Window Length (Integer, 0-99)**: Controls the window length expansion around the SNP location.

* **Flanks (integer, 0-2)**: Choose between 0 to 2 nucleotides around the SNP location for analysis. The window length must be equal or greater than the Flank size - otherwise if the window is smaller than the flanks, the window length expansion (parameter above) will default to the flank size.  

* **Indel maximum amount (Integer, 25-100)**: The maxmimum allowed indels in the .FASTA file, beyond the SNP location and 5 base pairs around it. Any indels found within a 2 base pair flanking of the SNP location will be automatically filtered out. Minimum indel thershold is 25%.

### Saving Options
* **Save all data (Boolean, true or false)**: There are many intermediate files created during SNP Context. This parameter allows you to save all intermediate files to your output directory; below is a complete list of output files if save all data is true: 

#### MSI and Amazon S3

* **Save S3 (Interger, 0 - 3)**: This allows you to save the mutaion motif outputs files (Count Tables) and config files to S3. The user can choose between these values **(0 - 3)**:
	0. Don't save to any S3 storage.
	1. Save to MSI's S3 tier two storage 
	2. Save to Amazons external S3 service (Needs .aws set up)
	3. Save to **both** MSI S3 storage and Amazons S3 storage platforms.

#### DropBox
* **DropBox (Boolean, true or false)**: The user can save the unique output file (counts tables and config file) to their personal dropbox app. 

* **DropBox auth (String)**: The authication code, needed for saving Dropbox files from the script. Follow this link for instructions on setting this up. **NOTE: this data is redacted when uploaded to S3, Dropbox or Github.**

#### GitHub

* **Github (Boolean, true or false)**: Do you want to save the output files (counts tables and config file) to a Github repository

* **gitUser (String)** : Your GitHub username. **NOTE: this data is redacted when uploaded to S3, Dropbox or Github.**
* **gitPass (String)** : You GitHub password: **NOTE: this data is redacted when uploaded to S3, Dropbox or Github.**

* **Myrepository (String)**: The name of the repository you will be saving the output files to.

* **owner (String)**: The owner (their username) of the repository, leave blank if you're the owner

## Command Line equivalents of the config file (can be run in any other).

**-w = Window Length (Integer, 0-99)**

**NEED EMAIL (String)**

**-i = Input file (String)**

**-o = Output location (String)**

**NEED MSI MODE**

**NEED PBS PARAMETERS (Intergers)**

**-p = Project name (String)**

**-f = Flanks (integer, 0-2)**

**-r = referenceGenome (String)**

**-n = Indel maximum amount (Integer, 25-100)**

**-ad = Save all data (Boolean, true or false)**

**-msi = MSI Mode (Boolean, true or false)**

**-s3 = Save S3 (Interger, 0 - 3)**

**-dpa = DropBox (Boolean, true or false)**

**-dpba = DropBox auth (String) *REDACTED WHEN UPLOADED***

**-gh = Github (Boolean, true or false)**

**-ghu = gitUser (String) *REDACTED WHEN UPLOADED***

**-ghp = gitPass (String) *REDACTED WHEN UPLOADED***

**-gho = owner (String)**

**-ghr = Myrepository (String)**

## Outputs

If the parameter `all data` is `true`, than all intermediate files are save in the output directory, if `all data` is `false` than only the Word and Combined Counts Tables and rejected files (.VCF, .BAM, .FASTA) are saved in the output drictory.

### If 'all Data' is **TRUE**:
* Parsed .VCF files
* Parsed .BAM files
* Parsed .BED files
* .FASTA files
* Consolidated rejected nuleotide positions from .BAM file
* Consolidated rejceted Indels .FASTA files
* Word Counts tables
* Combined Counts tables

### If 'all Data' is **FALSE** (default):
* Consolidated rejected nuleotide positions from .BAM file
* Consolidated rejceted Indels .FASTA files
* Word Counts tables
* Combined Counts tables
