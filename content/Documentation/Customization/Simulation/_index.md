+++
title = "Simulation"
weight = 1
+++



## Variant Simulation Tools

<figure>
  <p><a href="VST.pdf"><img src="documents.jpg"></a>
  <figcaption>A presentation about variant simulation tools (Sep. 2014)</figcaption>
</figure>

<font color=#FF0000>
Variant Simulation Tools is still under development. Development (subversion) versions of Variant Tools and [simuPOP][3] are needed to use VST. Also, because of a lack of mature simulation models (only two examples in the submitted paper are available), users are strongly advised to [contact me][4] for the creation of any simulation model. 
</font>  
  


### 1. What is *Variant Simulation Tools*

*Variant Simulation Tools* (VST) is a simulation tool for post-GWAS genetic epidemiological studies using whole-genome or whole-exome next-gen sequencing data, with an emphasis on user-friendliness and reproducibility. Because simulating high-quality datasets for genetic epidemiological studies requires careful selection and validation of appropriate simulation method for particular applications, **VST aims to provide validated application-specific simulation methods and simulated datasets**. It distinguishes itself from other simulators in that 



*   **VST provides multiple simulation engines for different simulation methods** 

Because different simulation methods have their own pros and cons and application areas, VST does not limit itself to a particular simulation method. Instead, it provides simulation engines for theoretical, forward-time, coalescent, and resampling-based simulations. 



*   **VST separates simulation models from their implementations** 

VST uses a two-tier design. The core of VST is a set of functions to perform different tasks and is distributed with Variant Tools. Simulations are defined in **simulation specification files** that are stored in the variant tools repository. New simulations become available to all VST users as soon as they are uploaded to the repository. 



*   **VST encourages reproducible simulations** 

Each VST simulation encapsulates simulation methods and parameters that have been used for a particular application. It allows no or only a few parameters to make it easy to reproduce a simulation. In addition to simulation models, VST also distribute simulated datasets for some simulation models, making it easier for casual users to explore the properties of selected simulation models. 

Whereas it is easy to reproduce an existing VST simulation, [creating a new simulation][5] in VST requires some in-depth knowledge of [simuPOP][3] and [variant tools][6].<font color=#FF0000>
   It is strongly recommended that you discuss your simulation study with other users of VST in [the varianttools-user mailing list][7] before you write your own simulations. </font> You could also [write to me][4] directly if you would like to discuss your project in private. 

<img src = "repository.jpg">

Schematic illustration of the role of the variant tools repository in the distribution of simulation models and simulated datasets. The repository provides simulation models and simulated datasets that can be downloaded using VST. Casual users can download simulated datasets and use them without running VST. Users who need to use a model without existing simulated datasets or need more replicates of an existing simulation can simulate data using VST. Advanced users who write their own simulation methods can choose to share their models with others by uploading their models to the repository. 



### 2. Key features

*   **VST simulates real nucleotide sequences of the human genome** 

VST simulates human nucleotide sequences with common or rare mutations. Simulated variants (mutations) locate at specific locations of the genome (`chr` and `pos`) with reference (`ref`) and alternative alleles (`alt`). VST maps mutations to the human genome if the underlying simulation engine simulates SNP markers on hypothetical regions. 



*   **The forward-time simulation engine knows the regions it simulate** 

In comparison to previous forward-time simulations that simulate hypothetical genomic regions with random fitness effect, VST 
*   identifies all genes (isoforms) that locate within or overlap with the user-provided regions, locates exons, coding regions, and codons on the forward and reverse strands. 
*   recombine parental chromosomes using a fine-scale genetic map with recombination rates and hotspots provided by the HapMap project 
*   use a nucleotide mutation model to introduce new mutants to the population 
*   **determine the fitness of individuals from amino-acid changes of the translated protein sequences** 



*   **VST supports a large number of penetrance and quantitative trait models and sampling methods** 

The flexible design of VST allows the application of very complex penetrance and quantitative trait models to the simulated population and draw random, case control, extreme trait, and pedigree-based samples. 



*   **Datasets simulated by VST could be exported in VCF format, or analyzed directly using Variant Association Tools** 

Simulated datasets could be exported and analyzed by other programs, or imported into a variant tools project and analyzed directly using [Variant Association Tools][8]. 



### 3. Basic usages of VST

#### 3.1 Step 1: check available simulation models

After [installing variant tools][9], run 



    % vtools show simulations
    

    Peng2011_srv            Re-implementation of some of the simulations performed by
                            SRV (Peng & Liu, Simulating Sequences of the Human Genome
                            with Rare Variants, 2011).
    Peng2014_ex1            Simulations of DNA sequences in specified regions using
                            different simulation methods, for the first example in
                            paper "Reproducible Simulations of realistic samples for
                            next-gen sequencing studies using Variant Simulation
                            Tools". The simulated populations are analyzed and
                            discarded to save diskspace.
    ...
    ...
    
    

to get a list of available simulation models. 


{{% notice tip %}}
If you cannot find a simulation model that fits your need, please write to the [varianttools-user mailing list][7] and check if others have performed similar simulations using VST. You could also [write to me][4] directly for potential collaboration on simulation part of your study. 
{{% /notice %}}


#### 3.2 Step 2: Learn the details of a simulation model

A **simulation specification file** can define multiple simulation models. Command `vtools show simulation SPECFILE` lists all simulation models defined in `SPECFILE` and their descriptions, include parameters they accept. 



    % vtools show simulation Peng2014_ex1
    

    Simulations of DNA sequences in specified regions using different simulation
    methods, for the first example in paper "Reproducible Simulations of realistic
    samples for next-gen sequencing studies using Variant Simulation Tools". The
    simulated populations are analyzed and discarded to save diskspace.
    
    Available simulation models: ms, neutral, resample, with_selection
    
    Model "ms":  This simulator calls ms to execute coalescent-based simulations,
    create a population with simulated dataset and run through the same statistical
    analysis as other pipeline.
      ms_0:               Check the version of variant tools. Version 2.3.1 or higher
                          is required for the execution of this simulation.
      ms_5:               Check the existence of command ms
      ms_10:              Create a new project if there is no existing project under
                          the current directory.
      ms_20:              Execute ms. NOTE that these parameters only matches the
                          parameters used in example 1 of Peng 2014.
      ms_30:              Create an empty simuPOP population for specified regions and
                          import ms-simulated genotypes. The segregating sites are
                          distributed within the specified region according to the
                          positions (float numbers from 0 to 1) assigned by ms. Mutants
                          are assumed to be from A->C, C->G, G->T and T->A for bases A,
                          C, G and T respectively.
      ms_50:              Get allele frequency spectrum in a sample of 700 individuals.
      ms_999:             Remove intermediate populations to save diskspace.
    
    Model "neutral":  A neutral simulation using a Juke-Cantor (1969) DNA nucleotide
    mutation model, a mutation rate of 1.8x10^-8, a fine-scale recombination map, a
    demographic model that mimicks the European population, and no natural selelction.
      neutral_0:          Check the version of variant tools. Version 2.3.1 or higher
                          is required for the execution of this simulation.
      neutral_10:         Create a new project if there is no existing project under
                          the current directory.
      neutral_20:         Link the refGene database to the project. This database is
                          required to parse the regions for gene structure.
      neutral_30:         Create an empty simuPOP population for specified regions.
      neutral_40:         Evolve and expand the population using a JC69 mutation model
                          and a demographic model that evolves a population of size
                          8100 for 81000 generations before it is expanded to size
                          900000 in 370 generations.
      neutral_50:         Get allele frequency spectrum in a sample of 700 individuals.
      neutral_999:        Remove intermediate populations to save diskspace.
    
    Model "resample":  A simulation model that extracts genotypes within the sepecified
    regions from the 1000 genomes project, and expands it very rapidly to mimick a
    resampling-based simulation.
      resample_0:         Check the version of variant tools. Version 2.3.1 or higher
                          is required for the execution of this simulation.
      resample_10:        Create a new project if there is no existing project under
                          the current directory.
      resample_20:        Link the refGene database to the project. This database is
                          required to parse the regions for gene structure.
      resample_25:        Extract genotypes of specified regions from 1000 genomes
                          project. No dependency check will be performed so the
                          extracted file can be used by other projects if you put is to
                          a place accessible by other projects. Location of the
                          extracted file can be specified by option --extracted_file.
      resample_30:        Create an empty simuPOP population for specified regions.
      resample_40:        Expand the population exponentially to reach a large
                          population in 10 generations. Mutations and recombinations
                          are allowed and a selection model that only select against
                          stopgain mutations are used.
      resample_50:        Get allele frequency spectrum in a sample of 700 individuals.
      resample_999:       Remove intermediate populations to save diskspace.
    
    Model "with_selection":  A simulation that uses identical models as the neutral
    model but use a protein selection model with selection pressure 0.005, 0.02 and 0.1
    for missense, stoploss and stopgain mutations.
      with_selection_0:   Check the version of variant tools. Version 2.3.1 or higher
                          is required for the execution of this simulation.
      with_selection_10:  Create a new project if there is no existing project under
                          the current directory.
      with_selection_20:  Link the refGene database to the project. This database is
                          required to parse the regions for gene structure.
      with_selection_30:  Create an empty simuPOP population for specified regions.
      with_selection_40:  Evolve and expand the population using a JC69 mutation model
                          and a demographic model that evolves a population of size
                          8100 for 81000 generations before it is expanded to size
                          900000 in 370 generations.
      with_selection_50:  Get allele frequency spectrum in a sample of 700 individuals.
      with_selection_999: Remove intermediate populations to save diskspace.
    
    Model parameters:
      regions             One or more chromosome regions (separated by ',') in the
                          format of chr:start-end (e.g. chr21:33031597-33041570), or
                          Field:Value from a region-based annotation database (e.g.
                          refGene.name2:TRIM2 or refGene_exon.name:NM_000947). Please
                          visit http://varianttools.sourceforge.net/Simulation for
                          detailed description of this parameter. (default:
                          chr17:41200001-41263000)
      scale               Scaling factor to speed up the simulation by scaling down the
                          simulation while boosting mutation, selection and
                          recombination rates. (default: 10)
      extracted_vcf       Filename (with dir) to save genotypes (.vcf file extracted by
                          tabix command) for the resample model. This file will be
                          automatically created and reused if it already exists. You
                          will need to remove this file if you run the same pipeline
                          using another region. (default: extracted.vcf)
    

#### 3.3 Step 3: Perform simulations



    % vtools simulate -h
    

    usage: vtools simulate [-h] [--seed SEED] [--replicates REPLICATES] [-j JOBS]
                           [-v {0,1,2}]
                           SPECFILE [MODELS [MODELS ...]]
    
    Simulate case control or family-based samples using specified simulation
    models.
    
    positional arguments:
      SPECFILE              Name of a model specification file, which can be the
                            name of an online specification file, or path to a
                            local .pipeline file. Please use command "vtools show
                            simulations" to get a list all available simulation
                            models.
      MODELS                Name of one or more simulation models defined in
                            SPECFILE, which can be ignored if the SPECFILE only
                            defines one simulation model. Please use command
                            "vtools show simulation SPECFILE" for details of
                            available models in SPECFILE, including model-specific
                            parameters that could be used to change the default
                            behavior of these models.
    
    optional arguments:
      -h, --help            show this help message and exit
      --seed SEED           Random seed for the simulation. A random seed will be
                            used by default but a specific seed could be used to
                            reproduce a previously executed simulation.
      --replicates REPLICATES
                            Number of consecutive replications to simulate
      -j JOBS, --jobs JOBS  Maximum number of concurrent jobs to execute, for
                            steps of a pipeline that allows multi-processing.
      -v {0,1,2}, --verbosity {0,1,2}
                            Output error and warning (0), info (1) and debug (2)
                            information to standard output (default to 1).
    

Each simulation specification file can contain more than one simulation models. You can use commands such as 



    % vtools simulate Peng2014_ex1 neutral
    

to perform one of them. Some models accept parameters. For example, the following command performs the simulation on gene `BRCA1` with locations defined in the `refGene` database. 



    % vtools simulate Peng2014_ex1 neutral --regions refGene.name2:BRCA1
    

    INFO: Starting simulation cache/Peng2014_ex1_neutral_273426005.cfg
    INFO: Executing Peng2014_ex1.neutral_0: Check the version of variant tools. Version 2.3.1 or higher is required for the execution of this simulation.
    INFO: Executing Peng2014_ex1.neutral_10: Create a new project if there is no existing project under the current directory.
    INFO: Executing Peng2014_ex1.neutral_20: Link the refGene database to the project. This database is required to parse the regions for gene structure.
    INFO: Running vtools use refGene
    INFO: Downloading annotation database from annoDB/refGene.ann
    INFO: Downloading annotation database from http://vtools.houstonbioinformatics.org/annoDB/refGene-hg19_20130904.DB.gz
    INFO: Using annotation DB refGene as refGene in project Peng2014_ex1.
    INFO: Known human protein-coding and non-protein-coding genes taken from the NCBI RNA reference sequences collection (RefSeq).
    INFO: Command "vtools use refGene" completed successfully in 00:00:01
    INFO: Executing Peng2014_ex1.neutral_30: Create an empty simuPOP population for specified regions.
    INFO: Regions to be simulated (81189 bp): 17:41196312-41277500
    INFO: Saving created population to cache/ex1_neutral_init_273426005.pop
    INFO: Executing Peng2014_ex1.neutral_40: Evolve and expand the population using a JC69 mutation model and a demographic model that evolves a population of size 8100 for 81000 generations before it is expanded to size 900000 in 370 generations.
    INFO: Regions to be simulated (81189 bp): 17:41196312-41277500
    INFO: Simulated regions with 81,189 basepair have 23,556 A, 17,899 C, 16,955 G, 22,779 T, and 0 N on reference genome.
    INFO: Regions to be simulated (81189 bp): 17:41196312-41277500
    INFO: Map distance of 71 markers within region 17:41196312-41277500 are found
    INFO: Total map distance from 17:41196312-41277500 (81189 bp) is 4.0144e-03 cM (r=4.9446e-08/bp)
    INFO: Start evolving...
    Statistics outputted are
        1. Generation number,
        2. population size (a list),
        3. number of segregation sites,
        4. average number of mutants per individual
        5. average allele frequency * 100
        6. average fitness value
        7. minimal fitness value of the parental population
    INFO:  1690 [810]   285 60.677778 10.645224 0.000000 0.000000
    INFO:  3030 [810]   307 73.462963 11.964652 0.000000 0.000000
    INFO:  4275 [810]   305 82.586420 13.538757 0.000000 0.000000
    INFO:  5393 [810]   371 108.137037 14.573725 0.000000 0.000000
    INFO:  6433 [810]   324 102.554321 15.826284 0.000000 0.000000
    INFO:  7620 [810]   325 92.483951 14.228300 0.000000 0.000000
    INFO:  8040 [810]   358 95.301235 13.310228 0.000000 0.000000
    INFO:  8137 [90000]  5614 99.777544 0.888649 0.000000 0.000000
    
    INFO: Simulated population has 90000 individuals, 5614 segregation sites. There are on average 99.0 mutants per individual. Mean allele frequency is 0.8886%.
    
    INFO: Population simulation takes 219.77 seconds
    INFO: Executing Peng2014_ex1.neutral_50: Get allele frequency spectrum in a sample of 700 individuals.
    INFO: Loading population from ex1_neutral_evolved_273426005.pop
    INFO: Executing Peng2014_ex1.neutral_999: Remove intermediate populations to save diskspace.
    INFO: Replace ex1_neutral_evolved_273426005.pop with ex1_neutral_evolved_273426005.pop.file_info
    INFO: Replace cache/ex1_neutral_init_273426005.pop with cache/ex1_neutral_init_273426005.pop.file_info
    



### 4. Some technical details

#### 4.1 Option `--seed` and `--replicates`

A random seed will be used by default for each simulation. For each simulation, a file `cache/MODEL_SEED.cfg` will be created as the input file for the execution of the simulation pipeline. You can, however, use the `--seed` option to specify the seed of the simulation to reproduce a previous simulation. 


{{% notice tip %}}
For reproducibility reasons, all simulated datasets provided my VST will be simulated with seeds `1`, `2`, `3`, .... 
{{% /notice %}}

The `vtools simulate` command by default only simulates one replicate of the simulation. It will repeat itself a number of times if option `--replicates` is specified. `SEED`, `SEED+1`, `SEED+2` etc will be used as seed for subsequent simulations. 


{{% notice warning %}}
If you need to run multiple instances of `VST` at the same time, please use separate directories for different instances of the `vtools simulate` command. 
{{% /notice %}}


#### 4.2 Specification of regions (option `--regions`)

Because VST simulates true genomic regions of the human genome, a key parameter of many simulation models is `--regions`. You can, for example, pass a region in `chr:start-end` format as 



    --regions chr17:540000-570000
    

or multiple regions as 



    --regions chr17:540000-570000,20000-210000,chr20:123450-134500
    

Here multiple regions are separated by "`,`", and chromosome name can be ignored if it is the same as the previous region. 

More interestingly, you can use value of a field-based annotation database to specify regions. For example, 



    --regions refGene.name2:BRCA1
    

locates all genes (isoforms) in the `refGene` database with common name `name2` equals to `BRCA1`. You can specify regions for multiple genes using 



    --regions refGene.name2:BRCA1,BRCA2,P53
    

You can use a different annotation database for this purpose. For example 



    % vtools simulate MODELFILE MODEL --regions refGene_exon.name2:BRCA1,BRCA2
    

simulates exon regions of `BRCA1` and `BRCA2`. Command `vtools use refGene_exon` will be automatically called if `refGene_exon` is not linked to the current project. 

The `--regions` option allows the use of set operations (`|`, `&`, `-`, and `^` for union, intersection, difference, and symmetric difference, respectively) to modify specified regions. For example, if you need to specify intron regions of a gene, you can use 



    --regions 'refGene.name2:BRCA1 - refGene_exon.name2:BRCA1'
    

Another example would be to limit, for example, a penetrance model to certain chromosome regions: 



    --regions 'refGene.name2:BRCA1 & chr17:41196312-41200000'


 [3]: http://simupop.sourceforge.net
 [4]: mailto:bpeng@mdanderson.org
 [5]:    /documentation/customization/simulation/
 [6]: https://vatlab.github.io/vat-docs/
 [7]: https://lists.sourceforge.net/lists/listinfo/varianttools-user
 [8]:   /applications/association/
 [9]:  /installation/