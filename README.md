# snakemake_tutorial

## Overview

- What does Snakemake do?
- Exploring a simple workflow
- Extending this workflow with {wildcards}
- Tour of other useful features

## What does Snakemake do?

Snakemake is a Workflow Management System developed specifically for data analysis pipelines.


Now, for a simple workflow, and if everything you need to use is in one language (e.g., Python or R), you might just put this all in one script:

```
# Define some functions up here

# Run the analysis
raw_data = load_data()
processed_data = process_data()
create_plot1(processed_data)

clusters = cluster_data(processed_data)

for cluster in clusters:
    analyze_subset(cluster)

```

And for some projects this is just fine!
Your workflow might look something like:

Create pipeline
Inspect results
Modify something
Repeated 2 and 3 until graduated==True
This is fine...until things become more complicated

- Data gets bigger. Individual steps start to take a long time
- Different steps are in different languages
- Workflows become more complicated

So what do you do?


## A simple workflow

### Structure of a Rule

Let's take a look at the Snakefile. Inside this file, each step is represented by a 'rule'.

The first two rules look like this:

```
rule download_data:
    output:
        reads="sample1.fastq.gz"
        reads="raw_data/sample1.fastq.gz"
    shell:
        """
        mkdir -p raw_data
        cd raw_data
        wget https://raw.githubusercontent.com/deto/Snakemake_Tutorial/master/misc/sample1.fastq.gz
        """

rule fastqc:
    input:
        reads=rules.download_data.output
    output:
        "fastqc/results.html"
    shell:
        """
        fastqc --outdir="fastqc" {input.reads}
        """
```

Rules generally specify:

- nputs: Files that the rule operates on (i.e. dependencies)
- Outputs: Files that the rule creates
- An action: Some command to run. This can be either:







### Extending this workflow with {wildcards}

we use Snakemake wildcards to scale up this workflow for many samples.

Here multiple read files are aligned with STAR, quantified with rsem, and then the gene expression estimates are merged into a single table with an R script.

To see how wildcards work - lets look at the align_reads rule:

```
rule align_reads:
    input:
        read1="raw_data/{id}_R1.fastq.gz",
        read2="raw_data/{id}_R2.fastq.gz"
    output:
        "{id}/STAR/results.bam"
    shell:
        """
        STAR --genomeDir "/some/directory/to/reference" \
            --outFileNamePrefix "{wildcards.id}/STAR/results" \
            --readFilesIn {input.read1} {input.read2}
        """
```


