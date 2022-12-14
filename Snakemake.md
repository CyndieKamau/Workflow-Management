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

An example of a command:

```
   shell:
     "wc -l {input} > {output}"
```

An example of a rule contaning a python script:

```
rule plot_results:
    input:
        isoforms=rules.quantify_transcripts.output.isoforms
    output:
        plot="plots/my_plot.png"
    script: "scripts/plot_things.py"
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


## **Use of wildcards in Snakemake**

The previous rules are all static, meaning to be reused with different inputs and outputs one has to change the rule.

For automation, the use of wildcards is necessary, since it does not specify specific input/output files to be used. Example:

```
rule align_reads:
    input:
        read1="raw_data/{file}"
        read2="raw_data2/{file}"
    output:
        "align_reads/{file}"
    shell:
        """
        STAR --genomeDir "/some/directory/to/reference" \
            --outFileNamePrefix "{file}/STAR/results" \
            --readFilesIn {input.file1} {input.file2}
        """
```

To run the process, only the output is defined in the command.

`snakemake align_reads/results.fastq`

Snakemake will reason backwards from the output, as its _declarative_; it acts like a map showing directions.


## **Workflow Visualization**

Visualization of workflows occur in [Directed acyclic graph](https://en.wikipedia.org/wiki/Directed_acyclic_graph) (DAG) format.

One can visualize their workflow with Snakemake by using the `--dag` command.

An example is:

`snakemake <rule> --dag | dot | display`


![DAG visualization](https://github.com/deto/Snakemake_Tutorial/raw/master/images/my_workflow.svg)


## **Containers**

One can specify different Docker or Singularity containers for each rule.


## **Generation of reports**

To summarize the obtained results and performed steps in a comprehensive report, one uses the built-in `report` function.

It is best practice to create reports in a separate rule that takes all desired results as input files and provides a single HTML file as output. Example:

```
rule all:
    input:
        "report.html"
```


## **Config files**

Config files are used for the workflow to be customizable, so that it can be easily adapted to new data.

Config files can be written in `JSON` or `YAML`, and loaded with the `configfile` directive.

Example, one can add `configfile: "config.yaml"` at the top of the Snakefile.

Snakemake will load the config file and store its contents into a globally available dictionary named `config`

