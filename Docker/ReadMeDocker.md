# SD Plots Docker Image
The Docker image contains the software pipeline that was used to generate the data published in the manuscript *Sweep Dynamics (SD) plots: Computational identification of selective sweeps to monitor the adaptation of influenza A viruses*. Follow the steps below according to your operating system to deploy the pipeline yourself.

**Contents of this ReadMe:**
+ SD Plots on Linux
	+ Requirements
	+ Input Data
	+ Running the Pipeline 
+ SD Plots on Windows
	+ Requirements
	+ Input Data
	+ Running the Pipeline 
+ Output Files
+ Docker Advanced
+ Errors (aka: Output other than the desired results) and Troubleshooting
+ Questions and Bug Reports

- - -
## SD Plots on Linux
### I | Requirements

The software image runs on Docker. Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications. It's available for a number of Linux distributions like Ubuntu, Debian or Fedora.

1. **Install Docker**, if it is not already installed on your machine. Docker is available under [DockerStore (Community Edition)](https://store.docker.com/search?type=edition&offering=community "Docker Store") (the free community edition is sufficient). Please follow the instructions on the respective download page.

2. To **check if Docker is running properly**, open the terminal and type:

       $docker version
       $docker run hello-world
> If you are running into an error like `Got permission denied while trying to connect to the Docker daemon socket`, try to run

	$sudo docker version
	$sudo docker run hello-world

### II | Input Data
1. **Create a folder and name it *sdplots*,** or, if you've run the pipeline before, you can re-use the old sdplots-folder. It does not matter where this folder is located on your machine, but it must be named *sdplots*.
2. **Copy the file containing all sequences you want to analyze into the *sdplots* folder.** It must be named *[PREFIX]_cds.fa* where [PREFIX] can be any character sequence. If you want to provide the amino acid sequences along with the nucleotide sequences, also create a second file called *[PREFIX]_aa.fa* and place it in the *sdplots*-folder as well. The prefix must be the same!
> Please note that sequences without a full date (formatted as either yyyy-mm-dd or yyyy/mm/dd) will be ignored.
> Identifiers in the cds file must match the identifiers for the respective aa-sequence in the aa file.
> For a reference file of how [PREFIX]_cds.fa should look like, please refer to [this file](https://github.com/hzi-bifo/SDplots/blob/master/Docker/data_cds.fa "HA_cds.fa").

### III | Running the Pipeline
1. To run the SD plot pipeline, open your terminal and insert
:

	$docker run -v [Complete/path/to/your/local/folder/]sdplots:/app/sdplots tklingenbifolab/sdplots:beta -i [PREFIX] -o [OUTPUT NAME] -r [ROOT SEQUENCE] [OTHER OPTIONS (see CMD Config below)]

> Again, if you are running into an error like `Got permission denied while trying to connect to the Docker daemon socket`, try to run `$sudo docker run ...`

>**Example**
Say your input data is located in *johndoe/Documents/sdplots/HA_cds.fa* and the root sequence of your data is supposed to be *A/California/05/2009* (please refer to the CMD Config part below to see more about cmd-line options). You want to set the output name to *test* and you want sampling to be enabled at 50 samples per season. You would then run 

	$sudo docker run -v johndoe/Documents/sdplots:/app/sdplots tklingenbifolab/sdplots:beta -i HA -o test -r "A/California/05/2009" -g true -s 50

>**CMD Config**
> To customize your pipeline-deployment, you can append the following to the run-command:

	-i, --i   : name of input files, must be set
	-o, --o   : name of output folder, must be set
	-r, --r   : isolate name of root sequence, must be set
	-l, --l   : translate cds to aa (true or false), default: true
	-g, --g   : sample sequences (true or false), default: false
	-s, --s   : sample size (sequences per season, should be larger than 10 and smaller than the maximum number of sequences in a season), default: 300
	-ha, --ha : analyze ha and adjust numbering (true or false), default: true
	-n, --n   : numbering, number of amino acids in the signal peptide (dependent on subtype, e.g. 17 for pH1N1 and 16 for H3N2), default: 17
	-w, --w   : plot width in inches, default: 12
	-p, --p   : format of plot (pdf or png), default:"pdf"
	-f, --f   : show only significant results (true or false), default: true
	-h        : show help (shows this list)

*[See section **Output Files** below **SD Plots for Windows** for further specification of the output.]*

**Stopping the Pipeline**
If you want to terminate the pipeline, you have two options:
1. Close the terminal in which the pipeline is running.
2. More elegant: Open a second terminal, run `$sudo docker ps` and copy the container ID of the execution that you want to stop. Then run `$sudo docker stop [CONTAINER ID]`. It may take a few seconds befor the container terminates.

- - -
## SD Plots for Windows
### I | Requirements

The software image runs on Docker. Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications. It's available for Windows 10 Professional and Enterprise.

1. **Install Docker**, if it is not already installed on your machine. Docker is available under [DockerStore (Community Edition)](https://store.docker.com/search?type=edition&offering=community "Docker Store") (the free community edition is sufficient). Please follow the instructions on the respective download page.

2. To **check if Docker is running properly**, open the PowerShell as admin and type:

       $docker version
       $docker run hello-world
> If you are running into an error like `Got permission denied while trying to connect to the Docker daemon socket`, you *need* to run the command in the PowerShell as admin. If you don't have admin rights on your machine, contact your system admin.

### II | Input Data
1. **Create a folder and name it *sdplots* **or, if you've run the pipeline before, you can re-use the old sdplots-folder. It does not matter where this folder is located on your machine, but it must be named *sdplots*.
2. **Copy the file containing all sequences you want to analyze into the *sdplots* folder.** It must be named *[PREFIX]_cds.fa* where [PREFIX] can be any character sequence. If you want to provide the amino acid sequences along with the nucleotide sequences, also create a second file called *[PREFIX]_aa.fa* and place it in the *sdplots*-folder as well. The prefix must be the same!
> Please note that sequences without a full date (formatted as either yyyy-mm-dd or yyyy/mm/dd) will be ignored.
> Identifiers in the cds file must match the identifiers for the respective aa-sequence in the aa file.
> For a reference file of how [PREFIX]_cds.fa should look like, please refer to [this file](https://github.com/hzi-bifo/SDplots/blob/master/Docker/data_cds.fa "data_cds.fa").

### III | Running the Pipeline
1. You need to **share the drive** that contains the *sdplots*-folder in the Docker Settings (see [Shared Drives](https://docs.docker.com/docker-for-windows/#docker-settings "Docker Settings")). It's sufficient if you do this once the first time you use the pipeline.
1. To run the SD plot pipeline, open your PowerShell as admin and insert:
		
		$docker run -v [Complete/path/to/your/local/folder/]sdplots:/app/sdplots tklingenbifolab/sdplots:beta -i [PREFIX] -o [OUTPUT NAME] -r [ROOT SEQUENCE] [OTHER OPTIONS (see CMD Config below)]
		
> Again, if you are running into an error like `Got permission denied while trying to connect to the Docker daemon socket`, you need to run the PowerShell as admin. Contact your system admin if you don't have admin rights.

>**Example**
Say your input data is located in *johndoe/Documents/sdplots/HA_cds.fa* and the root sequence of your data is supposed to be *A/California/05/2009* (please refer to the CMD Config part below to see more about cmd-line options). You want to set the output name to *test* and you want sampling to be enabled at 50 samples per season. You would then run 

	> 	$docker run -v C:\users\johndoe\Documents\sdplots:\app\sdplots tklingenbifolab/sdplots:beta -i HA -o test -r "A/California/05/2009" -g true -s 50

>**CMD Config**
> To customize your pipeline-deployment, you can append the following to the run-command:

	-i, --i   : name of input files, must be set
	-o, --o   : name of output folder, must be set
	-r, --r   : isolate name of root sequence, must be set
	-l, --l   : translate cds to aa (true or false), default: true
	-g, --g   : sample sequences (true or false), default: false
	-s, --s   : sample size (sequences per season, should be larger than 10 and smaller than the maximum number of sequences in a season), default: 300
	-ha, --ha : analyze ha and adjust numbering (true or false), default: true
	-n, --n   : numbering, number of amino acids in the signal peptide (dependent on subtype, e.g. 17 for pH1N1 and 16 for H3N2), default: 17
	-w, --w   : plot width in inches, default: 12
	-p, --p   : format of plot (pdf or png), default:"pdf"
	-f, --f   : show only significant results (true or false), default: true
	-h        : show help (shows this list)

**Stopping the Pipeline**
If you want to terminate the pipeline, you have two options:
1. Close the terminal in which the pipeline is running.
2. More elegant: Open a second terminal as admin, run `$docker ps` and copy the container ID of the execution that you want to stop. Then run `$docker stop [CONTAINER ID]`. It may take a few seconds befor the container terminates.
---
## Output Files

The output is written into a new folder named by the `-o`-parameter, for example *test*. If there is already a folder named this way, the pipeline will tell you to pick a different name and terminate. This allows you to run multiple executions parallel on your machine if you need to. The folder is located in the *sdplots*-folder you created earlier. The original input-file remains in the *sdplots*-folder but is also copied into your output folder.

The output consists of the following files:
	
* **data.significant_positions.pdf**  SD plots of the significant positions
* **data** This folder contains the input data as well as data generated during the calculation.
* **output**  This folder contains further output and provides deeper insight into the results.
    * **data.all_positions.pdf** SD plots of all positions
    * ...
* **positionplots** This folder contains SD plots for every single position. If a position shows no mutations in the given data, no plot will be shown in the respective pdf.

---
## Docker Advanced
If you want to control the usage of resources of the running software, please refer to [Docker Runtime Constraints on Resources](https://docs.docker.com/engine/reference/run/#runtime-constraints-on-resources "docs.docker.com").

---
## Errors (aka: Output other than the desired results) and Troubleshooting
If you run into issues, please have a look at the problems listed below and follow the instructions to tackle your problem.

### "Got permission denied while trying to connect to the Docker daemon socket"
**Output:**

	Got permission denied while trying to connect to the Docker daemon socket [...]
	
**Why it happens:** This is a docker-related error. It means than you don't have the permission to run this command.  
**What to do:** Re-run the command with root-/admin-privileges, depending on your operating system. If you are not root/admin, contact your system-administrator.

### "Not a directory"
**Output:**

	[...]starting container process caused[...]not a directory[...]Error response from daemon: oci runtime error:[...]
	
**Why it happens:** You propably tried to mount a file onto the directory in the pipeline, and not the data-folder.  
**What to do:** Make sure that you have `[Path/to/your/folder/]sdplots:/app/sdplots` set correctly.

### "I want to update the image to the newest version available."
**Why it happens:** Maybe the pipeline was updated, and you want the newer version.  
**What to do:** If the versions don't differ by their tag, you have to remove your local copy of the image.  
1. You need the Image ID to remove the image. To find the image and its ID (a 12-character-string), type `$docker images`
2. To remove the image, type `$docker rmi -f [IMAGE ID]`
2. Run the image as described above. When Docker in unable to find the image locally, it will pull it from the repository.

> **What is a tag?** 
> In the case of `tklingenbifolab/sdplots:beta`, `tklingenbifolab` is the user, `sdplots` is their repository and `beta` is the tag of the image

### There is no file such as *data.significant_positions.pdf*
**Why it happens:** When the *data.all_positions.pdf* is in your output folder, the pipeline worked fine. However, it failed to find significant positions, most likely because your input file (*data_cds.fa*) contained too few sequences for it to find anything of significance.
**What to do:** Use more sequences. Expand your file by sequences from the predeceasing season for example, or from both hemispheres.

---
## Questions and Bug Reports
If you have questions about the SD plots pipeline or bugs to report, please contact Thorsten Klingen at thorsten.klingen@helmholtz-hzi.de.

