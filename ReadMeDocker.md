# SDPlots Docker Image
The Docker image contains the software pipeline that was used to generate the data published in the manuscript 'Sweep Dynamics (SD) plots: Computational identification of selective sweeps to monitor the adaptation of influenza A viruses' and a docker instance of the SD plots pipeline.

The pre-print of this study is available under 

	http://biorxiv.org/content/early/2017/02/21/110528


## Requirements

The image runs on Docker, thus Docker must be installed. Docker is available under [DockerStore (Community Edition)](https://store.docker.com/search?type=edition&offering=community "Docker Store") (the free community edition is sufficient) for a number of operating systems (Windows 10 Professional or Enterprise, Apple Mac OS Yosemite 10.10.3 or above, as well as a number of Linux distributions like Ubuntu, Debian or Fedora). Please follow the instructions on the respective download page and run 

	$docker version 			to check the version and
	$docker run hello-world 		to verify that Docker can pull and run images.

## Input Data

Your input data must be contained in a file called "data_cds.fa", located within a folder named "data".
Please note that sequences without a full date (formatted as either yyyy-mm-dd or yyyy/mm/dd) will be ignored.
For a reference file of how data_cds.fa should look like, please refer to

	TODO add link to github file with valid data

## Running the Pipeline

To run the SDPlot pipeline, open your cmd-line tool or terminal and insert:

	$docker run -v Complete/Path/to/your/local/folder/data:/app/data hzibifo/sdplots:release 
	(TODO: imagename depends on publication)

The -v Path/.../data:/app/data mounts your local folder on the data-folder contained within the image. The pipeline writes the output in your local folder.

Depending on your operating system and your system settings, you might need to run the terminal as admin or type $sudo docker run... if your run into errors like "Got permission denied while trying to connect to the Docker daemon socket".


## CMD Config

To customize your pipeline-deployment, you can append the following to the run-command:

	-l, --l	  : translate cds to aa (true or false)
	-g, --g   : sample sequences (true or false)
	-s, --s   : sample size (sequences per season)
	-r, --r   : isolate name of root sequence
	-ha, --ha : analyze ha and adjust numbering (true or false)
	-n, --n   : numbering, number of amino acids in the signal peptide
	-w, --w   : plot width in inches
	-o, --o   : format of plot (pdf or png)
	-f, --f   : show only significant results (true or false)
	-h	  : show help (shows this list)
	
TODO: add default values

## Output Files

The final output consists of the following files: 

TODO add detailed description of relevant files, content and interpretation of the content:
	
* data.significant_positions.pdf		Plots ...
* data.all_positions.pdf			More plots ...



