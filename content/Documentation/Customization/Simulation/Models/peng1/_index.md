
+++
title = "Peng2014_ex1"
weight = 1
+++




## Simulations for the first example of Peng (2014) Genetic Epidemiology




### 1. Usage

    \\( vtools show simulation Peng2014_ex1
    
    Simulations of DNA sequences in specified regions using different simulation methods, for the
    first example in paper "Reproducible Simulations of realistic samples for next-gen sequencing
    studies using Variant Simulation Tools". The simulated populations are analyzed and discarded to
    save diskspace.
    
    Available simulation models: ms, neutral, resample, with_selection
    
    Model "ms":  This simulator calls ms to execute coalescent-based simulations, create a
    population with simulated dataset and run through the same statistical analysis as other
    pipeline.
      ms_0:               Check the version of variant tools. Version 2.3.1 or higher is required
                          for the execution of this simulation.
      ms_1:               Import required modules.
      ms_5:               Check the existence of command ms
      ms_10:              Create a new project if there is no existing project under the current
                          directory.
      ms_20:              Execute ms. NOTE that these parameters only matches the parameters used in
                          example 1 of Peng 2014.
      ms_30:              Create an empty simuPOP population for specified regions and import ms-
                          simulated genotypes. The segregating sites are distributed within the
                          specified region according to the positions (float numbers from 0 to 1)
                          assigned by ms. Mutants are assumed to be from A->C, C->G, G->T and T->A
                          for bases A, C, G and T respectively.
      ms_50:              Get allele frequency spectrum in a sample of 700 individuals.
      ms_999:             Remove intermediate populations to save diskspace.
    
    Model "neutral":  A neutral simulation using a Juke-Cantor (1969) DNA nucleotide mutation model,
    a mutation rate of 1.8x10^-8, a fine-scale recombination map, a demographic model that mimicks
    the European population, and no natural selelction.
      neutral_0:          Check the version of variant tools. Version 2.3.1 or higher is required
                          for the execution of this simulation.
      neutral_1:          Import required modules.
      neutral_10:         Create a new project if there is no existing project under the current
                          directory.
      neutral_20:         Link the refGene database to the project. This database is required to
                          parse the regions for gene structure.
      neutral_30:         Create an empty simuPOP population for specified regions.
      neutral_40:         Evolve and expand the population using a JC69 mutation model and a
                          demographic model that evolves a population of size 8100 for 81000
                          generations before it is expanded to size 900000 in 370 generations.
      neutral_50:         Get allele frequency spectrum in a sample of 700 individuals.
      neutral_999:        Remove intermediate populations to save diskspace.
    
    Model "resample":  A simulation model that extracts genotypes within the sepecified regions from
    the 1000 genomes project, and expands it very rapidly to mimick a resampling-based simulation.
      resample_0:         Check the version of variant tools. Version 2.3.1 or higher is required
                          for the execution of this simulation.
      resample_1:         Import required modules.
      resample_10:        Create a new project if there is no existing project under the current
                          directory.
      resample_20:        Link the refGene database to the project. This database is required to
                          parse the regions for gene structure.
      resample_25:        Extract genotypes of specified regions from 1000 genomes project. No
                          dependency check will be performed so the extracted file can be used by
                          other projects if you put is to a place accessible by other projects.
                          Location of the extracted file can be specified by option
                          --extracted_file.
      resample_30:        Create an empty simuPOP population for specified regions.
      resample_40:        Expand the population exponentially to reach a large population in 10
                          generations. Mutations and recombinations are allowed and a selection
                          model that only select against stopgain mutations are used.
      resample_50:        Get allele frequency spectrum in a sample of 700 individuals.
      resample_999:       Remove intermediate populations to save diskspace.
    
    Model "with_selection":  A simulation that uses identical models as the neutral model but use a
    protein selection model with selection pressure 0.005, 0.02 and 0.1 for missense, stoploss and
    stopgain mutations.
      with_selection_0:   Check the version of variant tools. Version 2.3.1 or higher is required
                          for the execution of this simulation.
      with_selection_1:   Import required modules.
      with_selection_10:  Create a new project if there is no existing project under the current
                          directory.
      with_selection_20:  Link the refGene database to the project. This database is required to
                          parse the regions for gene structure.
      with_selection_30:  Create an empty simuPOP population for specified regions.
      with_selection_40:  Evolve and expand the population using a JC69 mutation model and a
                          demographic model that evolves a population of size 8100 for 81000
                          generations before it is expanded to size 900000 in 370 generations.
      with_selection_50:  Get allele frequency spectrum in a sample of 700 individuals.
      with_selection_999: Remove intermediate populations to save diskspace.
    
    Model parameters:
      regions             One or more chromosome regions (separated by ',') in the format of chr
                          :start-end (e.g. chr21:33031597-33041570), or Field:Value from a region-
                          based annotation database (e.g. refGene.name2:TRIM2 or
                          refGene_exon.name:NM_000947). Please visit
                          http://varianttools.sourceforge.net/Simulation for detailed description of
                          this parameter. (default: chr17:41200001-41263000)
      scale               Scaling factor to speed up the simulation by scaling down the simulation
                          while boosting mutation, selection and recombination rates. (default: 10)
      extracted_vcf       Filename (with dir) to save genotypes (.vcf file extracted by tabix
                          command) for the resample model. This file will be automatically created
                          and reused if it already exists. You will need to remove this file if you
                          run the same pipeline using another region. (default: extracted.vcf)
    



### 2. Model

As a demonstration of the simulation engines of VST, we simulated the evolution of a sequence of 63,000 base pairs on chromosome 17 (chr17:41,200,001-41,263,000) using four simulation methods: a neutral coalescent simulation using ms \[Hudson 2002\] (wrapped by VST), a neutral forward-time simulation, a simulation with natural selection, and a resampling-based simulation. This region overlaps with five isoforms (NM\_007294, NM\_007297, NM\_007298, NM\_007299, NM_007300) of BRCA1, with 5337 nucleotides (8.47%) within the coding regions of one of the isoforms. All coalescent and forward-time simulations assumed a demographic model that mimics the evolution of the European population. This model evolves an initial population of 8,100 individuals for 81,000 generations and expands it to 900,000 individuals in 370 generations after a short bottleneck of 7,900 individuals [Kryukov, et al. 2007]. A mutation rate of 1.8 × 10–8 was used for ms. A Jukes-Cantor DNA evolution model with mutation rate 2.4 × 10–8 was used for the forward-time simulations because the Jukes-Cantor model with mutation rate μ mutates a nucleotide to any of the four nucleotides at equal probability and has an effective mutation rate of 0.75 μ. For VST simulations with natural selection, we used constant fitness values 0.005, 0.02, and 0.1 for missense, stoploss, and stopgain mutations, respectively. A multiplicative model was used to combine fitness values if an individual carried more than one non-synonymous mutation. This model applies strong purifying selection to stopgain mutations and practically disallows the existence of such mutations in the population. The resampling-based simulation extracts genotypes at this region from the 1000 Genomes Project and expands the population to 10,000 individuals in 10 generations, subject to the same mutation, recombination, and selection forces as the forward-time simulation. 

With 10,000 replicates for each simulation method, we drew a sample of 700 individuals from the simulated population. We counted the number of mutants at each locus and then the number of loci in each mutation frequency class. The results from two neutral models obtained from ms and VST differed in that the VST simulations yielded less singleton variants. This difference may be partly due to the different mutation models used by the two programs. Whereas ms uses an infinite-sites model in which new mutations always lead to a new segregating site, VST uses a finite-sites mutation model where mutations can happen at existing segregating sites (Figure 1a). The allele frequency spectra of neutral and selection simulations differ only slightly because only 8.47% of the simulated region is under the influence of natural selection. We can observe a more significant difference between allele frequency spectra if we limit the loci to coding regions (Figure 1b). 



### 3. Result

<img src = "fig1.png">

Except for resampling-based simulations, the allele frequency spectra of the simulated datasets are, however, different from what we observed from 700 random samples of the 1000 Genomes Project \[Abecasis, et al. 2012\] (Figure 1, blue bars). The observed allele frequency spectrum has more singletons than most simulated datasets, especially for coding regions of the VST simulations with natural selection. The reasons for this phenomenon can be multi-fold but we suspect that most rare variants were introduced during the rapid expansion of the European population which had a much stronger effect than natural selection in coding regions.