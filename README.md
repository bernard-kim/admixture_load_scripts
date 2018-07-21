# admixture_load_scripts
Scripts for simulating the admixture models described in Kim et al. This is a brief overview. Please see the paper for specific details on what we did.

Instead of providing straight SLiM simulation scripts (*.slim files), we have provided shell scripts that can be used to generate SLiM simulation scripts for a variety of parameters. Those SLiM files can then be used to recreate the simulated datasets in our paper.

We have done this for two reasons: first, this makes it easier to change small bits of the simulations. Second, this made things easier to run on our cluster. To avoid issues with random number generation for job arrays, I used a job ID plus a random integer as the random number seed for each simulation replicate. Therefore, a simulation ID should be passed to SLiM when running these scripts:

slim -seed ${JOB_ID} <slimscript.slim>

Most of these scripts can work with SLiM 2.x, but we recommend using SLiM 3.0 for compatibility with tree sequence recording. Tree sequence recording allows us to accurately track ancestry and made some simulations run faster. In addition, all simulations are scaled to some factor which is defined in the simulation script files.

Msprime, pyslim, and pandas are required to run the Python scripts that parse the tree sequence output.

## 5Mb_simulations

This folder contains the 5Mb chunks that were simulated. This outputs a CSV file which tracks population genetic statistics of interest over time. Those statistics are calculated within SLiM.

p1 is the source population and p2 is the recipient population.

## human_chr1_simulations

This folder contains simulation code for 100Mb of human chromosome 1. This outputs tree sequence files from which we parse out the local ancestry proportions in the genome.

The recombination map and exon definitions used for the simulations are found in sim_seq_info.txt.

Additional steps are required to go from tree sequences to useful data. Details on how to do this can be found in [SLiM Manual, Chapter 16](http://benhaller.com/slim/SLiM_Manual.pdf) and utilize the Python library [pyslim](https://github.com/tskit-dev/pyslim), for which the paper can be found [here](https://www.biorxiv.org/content/early/2018/06/07/248500 "Kelleher et al. bioRxiv") . 

For parsing local ancestry into a genomic map of introgression, we use parseancestrytrees.py, which should output some csv file with the extension .ancestry. Then, we use blocklengthdistribution.py to parse out the blocks of ancestry.

## arabidopsis_selfing

This folder contains simulation code for 29.1 Mb of Arabidopsis chromosome 1. This outputs VCF files from which we parse out the local ancestry proportions in the genome. We did not use tree sequence recording because it was prohibitively slow due to higher recombination rates, i.e. there were more trees to record. Instead, we used marker mutations to track introgressed ancestry.

The recombination map and exon definitions used for the simulations are found in sim_seq_info.txt.