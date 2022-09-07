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









