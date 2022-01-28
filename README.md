# SNP Context
----------

## Summary

    Corey Carter, St. Paul, MN - 09 January 2018 for Morrell Lab at the University of Minnesota - Twin Cities
    This bash script is designed to pull contextual sequence flanking a SNP (variant) from a reference genome.
    The window size for contextual sequence can be user specificed.
    The program checks for the presence of more than one variant within user specified windows.

## Config file
SNP Context can be used with either a **config file** or the **command line**, with the config file being the *perferred* method. Listed below are the config file input parameters:

### General input for config file
* **Project name (String)**: The name of your project. The working directory will hold this name.

* **Input email (String)**: If and only if you're using this script on the University Of Minnesota's supercomputing cluster **(MSI)**, PBS job updates will be sent to this address.

* **Input file (String)**: The input .VCF file. 

* **referenceGenome (String)**: The complete path `(pwd -P)` of your reference genome.

* **Output location (String)**: The output location of your files. A project directory is created at this location. If no location is specified, then the directory and files will be saved at the same location as the input file.

### MSI options for config file
* **MSI Mode (Boolean, true or false)**: If true, then the script will automatically submit your job to PBS job scheduler and allows for the user to save the output text files to MSI S3 tier two storage.

* **PBS parameters (Integer)**: Allows you to change the time and amount of resources allocated to the PBS job. 
Follow the link for more info: [MSI PBS job submission](https://www.msi.umn.edu/content/job-submission-and-scheduling-pbs-scripts)

#### Generated PBS job script location
* A PBS job script is generated in MSI mode, that file is saved in the SNP Context script directory.

### Mutation Motif
* **Window Length (Integer, 0-99)**: Controls the window length expansion around the SNP location.

* **Flanks (integer, 0-2)**: Choose between 0 to 2 nucleotides around the SNP location for analysis. The window length must be equal or greater than the Flank size - otherwise if the window is smaller than the flanks, the window length expansion (parameter above) will default to the flank size.  

* **Indel maximum amount (Integer, 25-100)**: The maximum allowed indels in the .FASTA file, beyond the SNP location and 5 base pairs around it. Any indels found within a 2 base pair flanking of the SNP location will be automatically filtered out. Minimum indel threshold is 25%.

### Saving Options

#### Section Notes

*When using the various saving options, note that only the individual counts tables, combined counts table and the config are saved to Dropbox, S3 or GitHub.*

*When data is uploaded, any compromising data (username, passwords, etc) are replaced with asterisks "****".*

* **Save all data (Boolean, true or false)**: There are many intermediate files created during SNP Context. This parameter allows you to save all intermediate files to your output directory. At the bottom of the page there is a complete list of output files if save all data is true: 

#### MSI and Amazon S3

* **Save S3 (Integer, 0 - 3)**: This allows you to save the mutation motif outputs files (counts tables) and config files to S3. The user can choose between these values **(0 - 3)**:

	0\. **Don't** save to any S3 storage **(default setting)**.

	1\. Save to **MSI's** S3 tier two storage.

	2\. Save to **Amazons** external S3 service. (Needs .aws set up: [Setting up amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/setup-aws-cli.html))

	3\. Save to **both** MSI S3 storage and Amazons S3 storage platforms.

#### DropBox
* **DropBox (Boolean, true or false)**: The user can save the unique output files (counts tables and config file) to their personal DropBox app.
Follow the link to step up a DropBox app: [Setting up DropBox app](https://www.dropbox.com/developers/reference/getting-started#app%20console)

* **DropBox auth (String)**: The authentication code, needed for saving Dropbox files from the script. Follow this link for instructions on setting this up. **NOTE: this data is redacted when uploaded to S3, Dropbox or Github.**

#### GitHub

* **Github (Boolean, true or false)**: Do you want to save the output files (counts tables and config file) to a Github repository?

* **gitUser (String)** : Your GitHub username. **NOTE: this data is redacted when uploaded to S3, Dropbox or Github.**
* **gitPass (String)** : Your GitHub password: **NOTE: this data is redacted when uploaded to S3, Dropbox or Github.**

* **Myrepository (String)**: The name of the repository you will be saving the output files to.

* **owner (String)**: The owner (their username) of the repository. Leave blank if you are the owner.

## Command Line equivalents to the config file (can be run in any order)

Command line parameters allow this script to be plugged into a larger pipeline or program, but the preferred method is the *context.config* file.

**-w = Window Length (Integer, 0-99)**

**-em = Input email (String)**

**-i = Input file (String)**

**-o = Output location (String)**

**-p = Project name (String)**

**-f = Flanks (integer, 0-2)**

**-r = referenceGenome (String, complete path with 'pwd -P' terminal command)**

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

### Exit Codes

If you're using SNP Context in a larger pipeline or program, the exit codes can provide the larger program needed information:

* exit code **0**: SNP Context operations have been successfully executed.

* exit code **1**: An general error has occurred; check error logs in script execution directory.

* exit code **2**: A PBS job was submitted, MSI mode only.

## Outputs

If the parameter `all data` is `true`, than all intermediate files are saved in the output directory. If `all data` is `false` than only the individual counts tables, combined counts table and rejected files (.VCF, .BAM, .FASTA) are saved in the output directory.

### If 'all Data' is **TRUE**:
* Parsed .VCF files
* Parsed .BAM files
* Parsed .BED files
* .FASTA files
* Consolidated rejected nucleotide positions from .BAM file
* Consolidated rejected indels .FASTA file
* Word counts tables
* Combined counts tables

### If 'all Data' is **FALSE** (default):
* Consolidated rejected nucleotide positions from .BAM file
* Consolidated rejected indels .FASTA file
* Word counts tables
* Combined counts tables

## Misc Info

### Dependencies

* [BedOps - 2.4.35](https://bedops.readthedocs.io/en/latest/)

* [bedtools - 2.23.0](https://bedtools.readthedocs.io/en/latest/)

* [Python 3.6](https://www.python.org/)

	1. [Mutation Motif](https://bitbucket.org/pycogent3/mutationmotif)

	2. [boto3](https://boto3.amazonaws.com/v1/documentation/api/latest/index.html) (For Amazon S3)

	3. [DropBox](https://www.dropbox.com/developers/documentation/python) (For DropBox)

### Error Handling
* SNP Context catches any errors that may occur during runtime and saves them to a file called the **SNP_CONTEXT_ERROR_LOG.txt**. It is located in the SNP Context script directory.

