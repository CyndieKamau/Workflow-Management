# **Importance of Workflow Management Systems**

A workflow is a sequence of operations used to complete a process.

A _Workflow Management System_ (WMS) is a software that provides an infrastructure to setup, execute, and monitor scientific workflows.

They help carry out and automate complex processes on larger volumes of heterogenous data. They also help in reproducibility, as workflows can be published and shared.

Workflows are visualized as follows:

* Input
* Output
* Service (command)
* Data flow

There are several WMSs currently in use;

* Galaxy
* Nextflow
* Snakemake

We'll be discussing Snakemake as a workflow management system.


##  _**What is Snakemake?**_

Snakemake is a text-based workflow system which is based on the command-line, and uses python syntax since python has a pseudo-code format.

It decomposes workflows to a set of rules, eg how to obtain output files from input files after running a set of commands.


## **Snakemake Basics**

### Installing Snakemake

Snakemake can be installed in a Conda environment, using:

`conda install snakemake`


In the directory having one's research files, say fastq files, a textfile called `Snakefile` should be created. It's where the workflow will be created.


A general Snakemake command is as follows:

```
rule countlines:
  input:
     "input/file/path"
  output:
     "output/file/path"
  shell:
     "wc -l {input} > {output}"

```


Rules generally specify the following;

_**Name of the process undertaken :**_ In this case our process is called `countlines` 

_**Input :**_ The files which the whole process will work on; They are also called **dependencies**.

```
  input:
        "data/genome.fa"
```

_**Output :**_ The files to be created after the whole process is done.

```
   output:
        "counted_reads/A.bam"
```

_**An action :**_ A command to run the whole process. Can either be;

* Bash Script
* Python `.py` script
* R `.R` script
* R markdown `.Rmd` file
* Inline python code

```
   shell:
     "wc -l {input} > {output}"
```


Rules can also be defined further as follows;

* There can be multiple inputs/outputs, example:

```
   input:
        "data/genome.fa",
        "data/samples/A.fastq"   
```

* They can be named (using the = syntax), or just listed in order, example:

```
   output:
        reads="sample1.fastq.gz"
        reads="raw_data/sample1.fastq.gz"
```

* These files can be referred to in the shell command (or python/R scripts)
* One can refer to the inputs/outputs of other rules 









