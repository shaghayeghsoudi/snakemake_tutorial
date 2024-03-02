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
