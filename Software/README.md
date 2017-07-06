# SD Plots Pipeline
This readme describes the software that was used to generate the data published in the manuscript *Sweep Dynamics (SD) plots: Computational identification of selective sweeps to monitor the adaptation of influenza A viruses*. The SD plots pipeline is embedded in a docker container and available for Linux, Windows and Mac systems.
Follow the steps below according to your operating system to deploy the pipeline yourself.

**Contents of this Readme:**
+ SD Plots on Linux
	+ Requirements
	+ Input Data
	+ Running the Pipeline 
+ SD Plots on Windows
	+ Requirements
	+ Input Data
	+ Running the Pipeline 
+ SD Plots on MAC
+ Output Files
+ Docker Advanced
+ Questions and Bug Reports

- - -
## SD Plots on Linux
### 1) Requirements

The software image runs on Docker. Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications. It is available for a number of Linux distributions like Ubuntu, Debian or Fedora.

1. **Install Docker**, if it is not already installed on your machine. Docker is available under [DockerStore (Community Edition)](https://store.docker.com/search?type=edition&offering=community "Docker Store") (the free community edition is sufficient). Please follow the instructions on the respective download page.

### 2) Input Data
1. **Create a folder called *sdplots*,** or, if you've run the pipeline before, you can re-use the old sdplots-folder. It does not matter where this folder is located on your machine, but it must be named *sdplots*. The docker image will mount this folder to access the data that is located in there. 
2. **Copy the file containing all sequences you want to analyze into the *sdplots* folder.** It must be named *[PREFIX]_cds.fa* where [PREFIX] can be any character sequence. If you want to provide the amino acid sequences along with the nucleotide sequences, also create a second file called *[PREFIX]_aa.fa* and place it in the *sdplots*-folder as well. The prefix must be the same!
> Please note that sequences without a full date (formatted as either yyyy-mm-dd or yyyy/mm/dd) will be ignored by the pipeline.
> Identifiers in the cds file must match the identifiers for the respective aa-sequence in the aa file.
> For a reference file of how [PREFIX]_cds.fa should look like, please refer to [this file](https://github.com/hzi-bifo/SDplots/blob/master/Software/Testdata/HA_cds.fa "HA_cds.fa").

### 3) Running the Pipeline
1. To run the SD plot pipeline, open your terminal and insert:

	$docker run -v [Complete/path/to/your/local/folder/]sdplots:/app/sdplots tklingenbifolab/sdplots:beta -i [PREFIX] -o [OUTPUT NAME] -r [ROOT SEQUENCE] [OTHER OPTIONS (see CMD Config below)]

2. To customize your pipeline-deployment, you can append the following to the run-command:
> 

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

3. **Example**

Say your input data is located in */home/johndoe/sdplots/HA_cds.fa* and the root sequence of your data is supposed to be *A/California/05/2009*. You want to set the output name to *test_run* and you want sampling to be enabled at 50 samples per season. You would then run
>
 	$sudo docker run -v /home/johndoe/sdplots:/app/sdplots tklingenbifolab/sdplots:beta -i HA -o test_run -r "A/California/05/2009" -g true -s 50

4. **Stopping the Pipeline**

If you want to terminate the pipeline, you have two options:
1. Close the terminal in which the pipeline is running.
2. More elegant: Open a second terminal, run `$sudo docker ps` and copy the container ID of the execution that you want to stop. Then run `$sudo docker stop [CONTAINER ID]`. It may take a few seconds befor the container terminates.

- - -
## SD Plots for Windows
### 1) Requirements

The software image runs on Docker. Docker is an open platform for developers and sysadmins to build, ship, and run distributed applications. It's available for Windows 10 Professional and Enterprise.

1. **Install Docker**, if it is not already installed on your machine. Docker is available under [DockerStore (Community Edition)](https://store.docker.com/search?type=edition&offering=community "Docker Store") (the free community edition is sufficient). Please follow the instructions on the respective download page.

### 2) Input Data
1. **Create a folder and name it *sdplots* **or, if you've run the pipeline before, you can re-use the old sdplots-folder. It does not matter where this folder is located on your machine, but it must be named *sdplots*.
2. **Copy the file containing all sequences you want to analyze into the *sdplots* folder.** It must be named *[PREFIX]_cds.fa* where [PREFIX] can be any character sequence. If you want to provide the amino acid sequences along with the nucleotide sequences, also create a second file called *[PREFIX]_aa.fa* and place it in the *sdplots*-folder as well. The prefix must be the same!
> Please note that sequences without a full date (formatted as either yyyy-mm-dd or yyyy/mm/dd) will be ignored.
> Identifiers in the cds file must match the identifiers for the respective aa-sequence in the aa file.
> For a reference file of how [PREFIX]_cds.fa should look like, please refer to [this file](https://github.com/hzi-bifo/SDplots/blob/master/Docker/data_cds.fa "data_cds.fa").

### 3) Running the Pipeline
1. You need to **share the drive** that contains the *sdplots*-folder in the Docker Settings (see [Shared Drives](https://docs.docker.com/docker-for-windows/#docker-settings "Docker Settings")). It's sufficient if you do this once the first time you use the pipeline.
2. To run the SD plot pipeline, open your PowerShell as admin and insert:
		
		$docker run -v [Complete/path/to/your/local/folder/]sdplots:/app/sdplots tklingenbifolab/sdplots:beta -i [PREFIX] -o [OUTPUT NAME] -r [ROOT SEQUENCE] [OTHER OPTIONS (see CMD Config below)]
		
3. To customize your pipeline-deployment, you can append the following to the run-command:
>

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

4. **Example**

Say your input data is located in */home/johndoe/sdplots/HA_cds.fa* and the root sequence of your data is supposed to be *A/California/05/2009*. You want to set the output name to *test_run* and you want sampling to be enabled at 50 samples per season. You would then run

	> 	$docker run -v C:\users\johndoe\Documents\sdplots:\app\sdplots tklingenbifolab/sdplots:beta -i HA -o test -r "A/California/05/2009" -g true -s 50

5. **Stopping the Pipeline**

If you want to terminate the pipeline, you have two options:
1. Close the terminal in which the pipeline is running.
2. More elegant: Open a second terminal as admin, run `$docker ps` and copy the container ID of the execution that you want to stop. Then run `$docker stop [CONTAINER ID]`. It may take a few seconds befor the container terminates.
---
## SD Plots on Mac
### 1) Requirements

To be completed soon.
---

## Output Files

The output is written into a new folder named by the `-o`-parameter, for example *test_run*. If there is already a folder named this way, the pipeline will tell you to pick a different name and terminate. This allows you to run multiple executions parallel on your machine if you need to. The folder is located in the *sdplots*-folder you created earlier. The original input-file remains in the *sdplots*-folder but is also copied into your output folder.

The output consists of the following files:
	
* **data.significant_positions.pdf**  SD plots of the significant positions
* **data** This folder contains the input data as well as intermediate data generated during the calculation.
* **output**  This folder contains further output and provides statistics about the frequencies of sweep-related changes.
    * **data.all_positions.pdf** SD plots of all positions
* **positionplots** This folder contains SD plots for every single position. If no amino acid change is detected at a position, the respective position plot will be empty.

---
## Docker Advanced
If you want to control the usage of resources of the running software, please refer to [Docker Runtime Constraints on Resources](https://docs.docker.com/engine/reference/run/#runtime-constraints-on-resources "docs.docker.com").

---
## Questions and Bug Reports
If you have questions about the SD plots pipeline or bugs to report, please contact Thorsten Klingen at thorsten.klingen@helmholtz-hzi.de.

