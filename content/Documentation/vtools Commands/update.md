
+++
title = "Update"
description = ""
weight = 3
+++

# Add new or update existing variant and genotype info fields


## Usage

    % vtools update -h
    

    usage: vtools update [-h] [--from_file FROM_FILE [FROM_FILE ...]]
                         [--build BUILD] [--format FORMAT] [-j N]
                         [--sample_name [SAMPLE_NAME [SAMPLE_NAME ...]]]
                         [--set [EXPR [EXPR ...]]] [--from_stat [EXPR [EXPR ...]]]
                         [-s [COND [COND ...]]] [--genotypes [COND [COND ...]]]
                         [-v {0,1,2}]
                         table
    
    Add or update fields of existing variants and genotype from other fields,
    statistics of genotypes and genotype info, or files that annotate variants or
    their locations (e.g. Read annotation from ANNOVAR output files, import
    additional variant or genotype fields from .vcf files).
    
    positional arguments:
      table                 variants to be updated.
    
    optional arguments:
      -h, --help            show this help message and exit
      -v {0,1,2}, --verbosity {0,1,2}
                            Output error and warning (0), info (1) and debug (2)
                            information to standard output (default to 1).
    
    Update from files:
      --from_file FROM_FILE [FROM_FILE ...]
                            A list of files that will be used to add or update
                            existing fields of variants. The file should be
                            delimiter separated with format described by parameter
                            --format. Gzipped files are acceptable. If input files
                            contains genotype information, have been inputted
                            before, and can be linked to the samples they created
                            without ambiguity (e.g. single sample, or samples with
                            detectable sample names), genotypes and their
                            information will also be updated.
      --build BUILD         Build version of the reference genome (e.g. hg18) of
                            the input files, which should be the primary (used by
                            default) or alternative (if available) reference
                            genome of the project. An alternative reference genome
                            will be added to the project if needed.
      --format FORMAT       Format of the input text file. It can be one of the
                            variant tools supported file types such as
                            ANNOVAR_output (cf. 'vtools show formats'), or a local
                            format specification file (with extension .fmt). Some
                            formats accept parameters (cf. 'vtools show format
                            FMT') and allow you to update additional or
                            alternative fields from the input file.
      -j N, --jobs N        Number of processes to import input file. Variant
                            tools by default uses a single process for reading and
                            writing, and can use one or more dedicated reader
                            processes (jobs=2 or more) to process input files. Due
                            to the overhead of inter-process communication, more
                            jobs do not automatically lead to better performance.
      --sample_name [SAMPLE_NAME [SAMPLE_NAME ...]]
                            Name of the samples to be updated by the input files.
                            If unspecified, headers of the genotype columns of the
                            last comment line (line starts with #) of the input
                            files will be used (and thus allow different sample
                            names for input files). Sample names will be used to
                            identify samples to be updated. Filename will be used
                            to uniquely identify a sample if mutliple samples with
                            the same name exist in the project. No genotype info
                            will be updated if samples cannot be unquely
                            determined.
    
    Set value from existing fields:
      --set [EXPR [EXPR ...]]
                            Add a new field or updating an existing field using a
                            constant (e.g. mark=1) or an expression using other
                            fields (e.g. freq=num/120, refgene=refGene.name). If
                            multiple values are returned for a variant, only one
                            of them will be used. Parameter samples could be used
                            to limit the affected variants. In addition, special
                            function are provided, including 'HWE_exact' (exact
                            test of Hardy-Weinberg Equilibrium) and 'Fisher_exact'
                            (Fisher's exact test for case/ctrl association).
    
    Set fields from sample statistics:
      --from_stat [EXPR [EXPR ...]]
                            One or more expressions such as meanQT=avg(QT) that
                            aggregate genotype info (e.g. QT) of variants in all
                            or selected samples to specified fields (e.g. meanQT).
                            Functions sum, avg, max, and min are currently
                            supported. In addition, special functions #(GT),
                            #(hom), #(het), #(alt), #(other), #(missing), #(wtGT),
                            #(mutGT), and maf(), are provided to count the number
                            of valid genotypes (not missing), homozygote
                            genotypes, heterozygote genotypes, alternative alleles
                            (#(het) + 2*#(hom) + #(other)), genotypes with two
                            different alternative alleles, missing genotypes
                            (number of samples - #(GT)), number of non-missing
                            wildtype genotypes (#(GT) - #(hom) - #(het) -
                            #(other)), number of non-wildtype genotypes (#(hom) +
                            #(het) + #(other)), and minor allele frequency. The
                            maf() function treats chromosomes 1 to 22 as
                            autosomes, X and Y as sex chromosomes, and other
                            chromosomes as single-copy manifolds. It requires a
                            phenotype named sex or gender that codes male/female
                            by 1/2, M/F or Male/Female if maf of variants on sex
                            chromosomes are calculated. This function by default
                            calculates allele frequency among existing-alleles,
                            but will treat all missing values as wild type alleles
                            if runtime option treat_missing_as_wildtype is set to
                            true.
      -s [COND [COND ...]], --samples [COND [COND ...]]
                            Limiting variants from samples that match conditions
                            that use columns shown in command 'vtools show sample'
                            (e.g. 'aff=1', 'filename like "MG%"').
      --genotypes [COND [COND ...]]
                            Limiting variants from samples that match conditions
                            that use columns shown in command 'vtools show
                            genotypes' (e.g. 'GQ>15').
    



## Details

Command `` `vtools update `` updates **variant info fields** (and to a lesser extend **genotype info fields**) by adding more fields or updating values at existing fields. It does not add any new variant or genotype, and does not change existing variant, sample, or genotype. Using three parameters `--from_file`, `--from_stat`, and `--set`, variant information fields could be updated from external file, sample genotypes, and existing fields. 



An illustration about `` `vtools update `` ▸ 

Attach:update.png 



### Import variant info from external files in standard formats (`--from_file`)

Option `--from_file` allows command `vtools update` to update variant and/or genotype info fields by reading info from an external file. This process is similar to `vtools import` but has two major differences: 

*   `` `vtools update `` does not import any new variant even if the external file has more variants. 
*   `` `vtools update `` can add or update fields from **position, range or field based input files**. That is to say, you could update fields from input files that describes variants at particular locations or chromosomsal regions. Variants belonging to the same location or region will share the updated info. 

The variant or genotype fields that will be added or updated depends on the file format (cf. `vtools show formats`). File formats accept parameters to specify what fields to import. For example, parameters `--var_info` and `--geno_info` can be used with [format vcf][1] to specify which variant and genotype info fields to be updated from a `vcf` file. 

(:toggleexample Examples: create a project:) 

Let us create a directory update and import an empty project with a few test vcf files `V1.vcf`, `V2.vcf` and `V3.vcf`, 

    % mkdir update
    % cd update
    % vtools init update
    % vtools admin --load_snapshot vt_testData
    

The project does not have any variant so we import some from these VCF files: 



    % vtools import V*.vcf --build hg18
    

    INFO: Importing variants from V1.vcf (1/3)
    V1.vcf: 100% [========================================================] 1,000 10.9K/s in 00:00:00
    INFO: 989 new variants (989 SNVs) from 1,000 lines are imported.
    INFO: Importing variants from V2.vcf (2/3)
    V2.vcf: 100% [========================================================] 1,000 11.7K/s in 00:00:00
    INFO: 352 new variants (352 SNVs) from 995 lines are imported.
    INFO: Importing variants from V3.vcf (3/3)
    V3.vcf: 100% [========================================================] 1,000 13.2K/s in 00:00:00
    INFO: 270 new variants (270 SNVs) from 1,000 lines are imported.
    Importing genotypes: 100% [============================================] 2,995 1.5K/s in 00:00:02
    Copying genotype: 100% [==================================================] 3 11.2K/s in 00:00:00
    INFO: 1,611 new variants (1,611 SNVs) from 2,995 lines (3 samples) are imported.
    

(:exampleend:) 

(:toggleexample Examples: import additional fields from source files:) As we can see from the output of `vtools show fields`, this project does not have any variant info field, 



    % vtools show fields
    

    variant.chr
    variant.pos
    variant.ref
    variant.alt
    

Because the input file has two variant info fields `DP` (depth of coverage) and `NS` (number in sample), we can add these two fields to our project using commands 



    % vtools update variant --from_file V*.vcf --var_info DP NS
    

    INFO: Using primary reference genome hg18 of the project.
    Getting existing variants: 100% [====================================] 1,611 170.4K/s in 00:00:00
    INFO: Updating variants from V1.vcf (1/3)
    V1.vcf: 100% [=========================================================] 1,000 7.0K/s in 00:00:00
    INFO: Fields DP, NS of 989 variants are updated
    INFO: Updating variants from V2.vcf (2/3)
    V2.vcf: 100% [=========================================================] 1,000 7.4K/s in 00:00:00
    INFO: Fields DP, NS of 990 variants are updated
    INFO: Updating variants from V3.vcf (3/3)
    V3.vcf: 100% [=========================================================] 1,000 6.8K/s in 00:00:00
    INFO: Fields DP, NS of 988 variants are updated
    INFO: Fields DP, NS of 2,967 variants are updated
    

The project now has two variant info fields `DP` and `NS`, 

    % vtools show fields 
    

    variant.chr
    variant.pos
    variant.ref
    variant.alt
    variant.DP
    variant.NS
    

and we can output them with variants 

    % vtools output variant chr pos ref alt DP NS -l 5
    

    1	4540	G	A	9	1
    1	5683	G	T	7	1
    1	5966	T	G	24	1
    1	6241	T	C	23	1
    1	9992	C	T	13	1
    

(:exampleend:) 

(:toggleexample Examples: add genotype info field:) We have just added two variant info fields `DP` and `NS` from input vcf files, but what we did probably does not make any sense. This is because `DP` and `NS` describes variants in each sample so the same variants from different samples will have different depth, and sample count should probaby be added together to reflect the total sample count. That is to say, `DP` here should better be considered as genotype field instead of variant field. 

Variant tools is flexible enough to handle such a situation (check `vtools show format vcf` for details). By specifying field `DP` in parameter `--geno_info`, this field will be added as genotype info field and be processed for each sample: 

    % vtools update variant --from_file V*.vcf --geno_info DP
    

    INFO: Using primary reference genome hg18 of the project.
    Getting existing variants: 100% [====================================] 1,611 176.1K/s in 00:00:00
    INFO: Updating variants from V1.vcf (1/3)
    V1.vcf: 100% [=========================================================] 1,000 7.6K/s in 00:00:00
    INFO: Fields  of 0 variants and geno fields of 989 genotypes are updated
    INFO: Updating variants from V2.vcf (2/3)
    V2.vcf: 100% [=========================================================] 1,000 7.7K/s in 00:00:00
    INFO: Fields  of 0 variants and geno fields of 990 genotypes are updated
    INFO: Updating variants from V3.vcf (3/3)
    V3.vcf: 100% [=========================================================] 1,000 7.5K/s in 00:00:00
    INFO: Fields  of 0 variants and geno fields of 988 genotypes are updated
    INFO: Fields  of 0 variants and geno fields of 2,967 genotypes are updated
    

The project now has a genotype info field `DP`, with coverage depth for each genotype. 



    % vtools show genotypes
    

    sample_name	filename	num_genotypes	sample_genotype_fields
    SAMP1	V1.vcf	989	GT,DP
    SAMP1	V2.vcf	990	GT,DP
    SAMP1	V3.vcf	988	GT,DP
    



Variant tools depends on filename to determine which sample to update from each input file so **the original input files must be used to update genotype info fields**. 

(:exampleend:) 

(:toggleexample Examples: adding annotation info from ANNOVAR output:) 

Suppose that we would like to use ANNOVAR to annotate our variants. The basic step is to export the variants in ANNOVAR input format and call ANNOVAR. 



    % vtools export variant --format ANNOVAR > ann.in
    

    vtools export variant --format ANNOVAR > ann.in
    Writing: 100% [=======================================================] 1,611 45.1K/s in 00:00:00
    INFO: 1611 lines are exported from variant table variant 
    



    % annotate_variation.pl ann.in humandb/
    

    NOTICE: The --geneanno operation is set to ON by default
    NOTICE: The --buildver is set as 'hg18' by default
    NOTICE: Reading gene annotation from humandb/hg18_refGene.txt ... Done with 41481 transcripts (including 7217 without coding sequence annotation) for 23669 unique genes
    NOTICE: Reading FASTA sequences from humandb/hg18_refGeneMrna.fa ... Done with 14 sequences
    WARNING: A total of 318 sequences will be ignored due to lack of correct ORF annotation
    NOTICE: Finished gene-based annotation on 1611 genetic variants in ann.in
    NOTICE: Output files were written to ann.in.variant_function, ann.in.exonic_variant_function
    

We then want to update our project with ANNOVAR annotations. Because gene annotation of ANNOVAR generates two output files, two output file formats are available. One of the formats is `ANNOVAR_exonic_variant_function`, 



    % vtools show format ANNOVAR_exonic_variant_function
    

    Format:      ANNOVAR_exonic_variant_function
    Description: Output from ANNOVAR, generated from command
      "path/to/annovar/annotate_variation.pl annovar.txt
      path/to/annovar/humandb/". This format imports chr, pos, ref, alt
      and ANNOVAR annotations. For details please refer to
      http://www.openbioinformatics.org/annovar/annovar_gene.html
    
    Columns:
      None defined, cannot export to this format
    
    variant:
      chr          Chromosome
      pos          1-based position, hg18
      ref          Reference allele, '-' for insertion.
      alt          Alternative allele, '-' for deletion.
    
    Variant info:
      mut_type     the functional consequences of the variant.
    
    Other fields (usable through parameters):
      genename     Gene name (for the first exon if the variant is in more than one
                   exons, but usually the names for all exons are the
                   same).
      function     the gene name, the transcript identifier and the sequence change in
                   the corresponding transcript
    
    Format parameters:
      var_info     Fields to be outputted, can be one or both of mut_type and function.
                   (default: mut_type)
    

As you can see, this is a variant based format. It has a default variant info field `mut_type`, but you can add more fields using parameter `var_info`. To update variants, 



    % vtools update variant --format ANNOVAR_exonic_variant_function --from_file ann.in.exonic_variant_function
    

    INFO: Using primary reference genome hg18 of the project.
    Getting existing variants: 100% [===============================================================] 1,611 154.0K/s in 00:00:00
    INFO: Updating variants from ann.in.exonic_variant_function (1/1)
    ann.in.exonic_variant_function: 100% [===============================================================] 23 2.5K/s in 00:00:00
    INFO: Field mut_type of 23 variants are updated
    

By default, field `mut_type` is added. 



    % vtools show fields
    

    variant.chr
    variant.pos
    variant.ref
    variant.alt
    variant.DP
    variant.NS
    variant.mut_type
    

To update additional variant/genotype fields, use `--var_info` of the `ANNOVAR_exonic_variant_function` format: 

    % vtools update variant --format ANNOVAR_exonic_variant_function \
                           --from_file ann.in.exonic_variant_function \
                           --var_info mut_type function 
    

    INFO: Using primary reference genome hg18 of the project.
    Getting existing variants: 100% [===============================================================] 1,611 180.6K/s in 00:00:00
    INFO: Updating variants from ann.in.exonic_variant_function (1/1)
    ann.in.exonic_variant_function: 100% [===============================================================] 23 1.8K/s in 00:00:00
    INFO: Fields mut_type, function of 23 variants are updated
    

Fields specified by option `--var_info` are added. Now we have one more field `function` 



    % vtools show fields
    

    variant.chr
    variant.pos
    variant.ref
    variant.alt
    variant.DP
    variant.NS
    variant.mut_type
    variant.function
    

(:exampleend:) 



### Import variant info from external files in customized formats (`--from_file`)

During the analysis of variants, it is common to have annotations in different formats from various sources (e.g. from annotation servers such as [regulome DB][2]). Although variant tools has a growing number of [formats][3][?][3], it is sometimes needed to create your own format files to import annotations. 

(:toggleexample Examples: Update annotation for variants presented by rsnames:) Let us first create some data with rsname. Because this sample project uses hg18, we cannot use the default `dbSNP` database and has to use 



    % vtools use dbSNP-hg18_130
    

    INFO: Downloading annotation database from annoDB/dbSNP-hg18_130.ann
    INFO: Downloading annotation database from http://vtools.houstonbioinformatics.org/annoDB/dbSNP-hg18_130.DB.gz
    dbSNP-hg18_130.DB.gz: 100% [================================================] 617,049,496.0 1.3M/s in 00:08:07
    INFO: Decompressing /Users/bpeng/.variant_tools/annoDB/dbSNP-hg18_130.DB.gz
    INFO: Using annotation DB dbSNP in project update.
    INFO: dbSNP version 130
    

Let us find some variants with rsname and output it 



    % vtools select variant 'dbSNP.func = "intron"' --output dbSNP.name dbSNP.func > dbSNP.info
    

The file looks like: 



    % head -10 dbSNP.info
    



    rs3131969	intron
    rs3131968	intron
    rs3131967	intron
    rs3115859	intron
    rs3131966	intron
    rs3131964	intron
    rs3115858	intron
    rs12567639	intron
    rs3131963	intron
    rs3115853	intron
    

To update your variants with this information, you will need a format file that knows how to get `chr`, `pos`, `ref` and `alt` from `rsname`. Fortunately, the [map][4][?][4] format provides such an example so we can adapt it to the following format: 



    [format description]
    description=a format that determins chr, pos, ref, alt from rsname using a dbSNP database
    variant=chr,pos,ref,alt
    variant_info=func
    
    [DEFAULT]
    db_file=dbSNP.DB
    
    #
    # NOTE: Functor FieldFromDB(db, res, fields) matches input(s) from
    # columns 'index' (column 1 in this example), compare it with field
    # 'fields' (name), and return results 'res' (chr, start, refNCBI,
    # alt). You can get a list of fields by 'vtools use dbSNP-hg18_130',
    # and run 'vtools show fields'
    #
    [chr]
    index=1
    type=VARCHAR(20)
    adj=FieldFromDB("%(db_file)s", "chr", "name")
    
    [pos]
    index=1
    adj=FieldFromDB("%(db_file)s", "start", "name")
    type=INTEGER NOT NULL
    
    [ref]
    index=1
    adj=FieldFromDB("%(db_file)s", "refNCBI", "name")
    type=VARCHAR(255)
    
    [alt]
    index=1
    type=VARCHAR(255)
    adj=FieldFromDB("%(db_file)s", "alt", "name")
    
    [func]
    index=2
    type=VARCHAR(255)
    

We can then use this format (named `rsname.fmt`) to import variant info `func` to the project: 



    % vtools update variant --format rsname --from_file dbSNP.info --db_file dbSNP-hg18_130.DB
    

    INFO: Using primary reference genome hg18 of the project.
    Getting existing variants: 100% [====================================] 1,611 229.7K/s in 00:00:00
    INFO: Updating variants from dbSNP.info (1/1)
    dbSNP.info: 100% [=======================================================] 209 3.0K/s in 00:00:00
    INFO: Field func of 209 variants are updated
    

The project now has another field `variant.func` (to differentiate from `dbSNP.func`), 



    % select variant 'dbSNP.func = "intron"' --output chr pos ref alt dbSNP.name variant.func  -l 10
    

    1	744045	A	G	rs3131969	intron
    1	744055	A	G	rs3131968	intron
    1	744197	T	C	rs3131967	intron
    1	744366	G	A	rs3115859	intron
    1	744827	C	T	rs3131966	intron
    1	745750	C	G	rs3131964	intron
    1	745753	A	T	rs3115858	intron
    1	746131	G	A	rs12567639	intron
    1	746243	T	A	rs3131963	intron
    1	747503	G	A	rs3115853	intron
    

(:exampleend:) 

(:toggleexample Examples: Update annotation from position-based data:) 

Now, suppose we have another file with only `chr` and `pos` information: 



    % vtools output variant chr pos dbSNP.molType dbSNP.class > moltype.txt
    

This file looks like 

    % head -10 moltype.txt
    

    1	4540	NA	NA
    1	5683	genomic	single
    1	5966	NA	NA
    1	6241	genomic	single
    1	6241	genomic	single
    1	9992	genomic	single
    1	9993	genomic	single
    1	10007	NA	NA
    1	10098	genomic	single
    1	10098	genomic	single
    

Because this file does not have `ref` and `alt` information, we will have to treat it as position-based. That is to say, if there are multiple variants at a chromosomal location, they will updated with the same information. The format to use, is therefore 



    [format description]
    description=A position based format to update variant information
    position=chr,pos
    variant_info=molType, molClass
    
    [chr]
    index=1
    type=VARCHAR(20)
    
    [pos]
    index=2
    type=INTEGER NOT NULL
    
    [molType]
    index=3
    type=VARCHAR(255)
    
    [molClass]
    index=4
    type=VARCHAR(255)
    

With `position.fmt` saved in the project directory, the following command can be used to update variants with additional fields `molType` and `molClas`: 



    % vtools update variant --format position --from_file moltype.txt
    

    INFO: Using primary reference genome hg18 of the project.
    Getting existing variants: 100% [====================================] 1,611 226.1K/s in 00:00:00
    INFO: Updating variants from moltype.txt (1/1)
    moltype.txt: 100% [====================================================] 1,679 8.3K/s in 00:00:00
    INFO: Fields molType, molClass of 1,689 variants are updated
    



    % vtools output variant chr pos ref alt variant.molType molClass -l 10
    

    1	4540	G	A	NA	NA
    1	5683	G	T	genomic	single
    1	5966	T	G	NA	NA
    1	6241	T	C	genomic	single
    1	9992	C	T	genomic	single
    1	9993	G	A	genomic	single
    1	10007	G	A	NA	NA
    1	10098	G	A	genomic	single
    1	14775	G	A	genomic	single
    1	16862	A	G	genomic	single
    

(:exampleend:) 



### Calculate genotype statistics for each variant (`--from_stat`)

Option `--from_stat` adds fields to variant tables with summary statistics from sample genotypes. This parameter accepts expressions in the format of `VAR_INFO=FUNC(GENO_INFO)` where `VAR_INFO` is the variant information field to be added or updated, `FUNC` is a function or expression, and `GENO_INFO` is genotype or genotype info. If you are only interested in genotype statistics, some specially defined `FUNC(GENO_INFO)` can be used. A list of acceptable aggregating functions is available [here][5]. 

(:toggleexample Examples: statistics of genotype info fields:) Each genotype in our samples has a field `DP`, which is the coverage depth. To assess the quality of a variant, it is useful to calculate average depth across all samples as a variant info field. The aggregating function to use is `avg`: 



    % vtools update variant --from_stat 'avg_DP=avg(DP)'
    

    Counting variants: 100% [==================================================] 3 110.9/s in 00:00:00
    INFO: Adding field avg_DP
    Updating variant: 100% [===============================================] 1,611 49.2K/s in 00:00:00
    INFO: 1610 records are updated
    

Depth of genotypes across three samples are averaged and save to a new variant info field `avg_DP`. It is different from the `DP` we saved before because that `DP` is the depth of one of the genotypes: 



    % vtools output variant chr pos ref alt DP avg_DP -l5
    

    1	4540	G	A	9	7.5
    1	5683	G	T	7	7.0
    1	5966	T	G	24	25.3333333333
    1	6241	T	C	23	20.5
    1	9992	C	T	13	12.0
    

(:exampleend:) 

A locus has a wildtype allele and can have one or more alternative alleles. Assuming a wildtype allele 0, an alternative allele 1, and an additional alternative allele 2 if exists, a sample can have genotype 0/0, 0/1, 0/2, 1/1, 2/2, and 1/2 at a locus on an autosome. `vtools update --from_stat` counts such genotype for all (or selected) variants using the following special functions: 



*   `#(GT)`: the total number of genotypes 
*   `#(alt)`: number of alternative alleles (non-zero alleles 1 or 2) 
*   `#(hom)`: number of homozygote (genotype 1/1 or 2/2) 
*   `#(het)`: number of heterozygote (genotype 0/1 or 0/2) 
*   `#(other)`: number of heterozygote with two alternative alleles (genotype 1/2). 
*   `#(wtGT)`: number of wildtype genotypes (`#(GT) - #(hom) - #(het) - #(other)`) 
*   `#(mutGT)`: number of non-wildtype genotypes (`#(hom) + #(het) + #(other)`) 
*   `#(missing)`: number of missing genotypes (`sample size - #(GT)`) 
*   `maf()`: minor allele frequency. The frequency is the smaller of #(alt) / #all\_alleles and 1 - #(alt) / #all\_alleles. The total number of alleles is determined by type of chromosome, sex of samples, and how to treat missing values. More specifically, if runtime option `treat_missing_as_wildtype` is set to `False` (default), `#all_alleles` is 
    *   `2 * #(GT)` for variants on autosomes 
    *   `# GT for females + #GT for males` for variants on chromosome X 
    *   `# GT for males` for variants on chromosome Y, and 
    *   `#(GT)` for variants on other chromosomes. 

If `treat_missing_as_wildtype` is set to true, all missing values are counted as wildtype allele so `#all_alleles` is 
*   `2 * number of samples` for variants on autosomes 
*   `2 * number of males + number of females` for variants on chromosome X 
*   `number of males` for variants on chromosome Y 
*   `number of samples` for varants on other chromosomes 

Note that genotype 1/1 an 2/2 are counted for different variants (0,1) and (0,2), and `#(other)` is counted for both variants. Therefore, `#(alt)` equals `#(hom)` X 2 + `#(het)` + `#(other)`*2. 

(:toggleexample Examples: sample statistics:) Using these special functions, we can calculate many sample statistics as various variant info fields: 



    % vtools update variant --from_stat 'total=#(GT)' 'num=#(alt)' 'hom=#(hom)' 'het=#(het)' 'other=#(other)'
    

    Counting variants: 100% [==================================================] 3 238.8/s in 00:00:00
    INFO: Adding field num
    INFO: Adding field hom
    INFO: Adding field het
    INFO: Adding field other
    INFO: Adding field total
    Updating variant: 100% [===============================================] 1,611 30.9K/s in 00:00:00
    INFO: 1610 records are updated
    

As you can see, genotypes are not available in all samples and the first five variants are all appear as heterozygotes. 

    % vtools output variant chr pos ref alt total num hom het other -l5
    

    1	4540	G	A	2	2	0	2	0
    1	5683	G	T	1	1	0	1	0
    1	5966	T	G	3	3	0	3	0
    1	6241	T	C	2	2	0	2	0
    1	9992	C	T	2	2	0	2	0
    

You can use the `maf` function to calculate minor allele frequency 



    % vtools update variant --from_stat 'maf=maf()'
    

    INFO: 0, 0, and 0 variants on X, Y and other non-autosome chromosomes are identifield
    Counting variants: 100% [============================================] 3 316.5/s in 00:00:00
    INFO: Adding variant info field maf with type FLOAT
    Updating variant: 100% [=========================================] 1,611 45.8K/s in 00:00:00
    INFO: 1611 records are updated
    

This case is easy because all variants are on an autosome. 

    % vtools output variant chr pos ref alt total num hom het maf -l5
    

    1	4540	G	A	2	2	0	2	0.5
    1	5683	G	T	1	1	0	1	0.5
    1	5966	T	G	3	3	0	3	0.5
    1	6241	T	C	2	2	0	2	0.5
    1	9992	C	T	2	2	0	2	0.5
    

However, because this dataset does not record wildtype alleles, the minior allele frequencies are `0.5` if all genotypes are heterozygotes. To calculate minor allele frequency for all samples, you should set runtime option `treat_missing_as_wildtype` as `true`, 



    % vtools admin --set_runtime_option treat_missing_as_wildtype=true
    

    INFO: Option treat_missing_as_wildtype is set to True
    

In this case, missing genotypes are counted, 

    $ vtools update variant --from_stat 'maf1=maf()'
    

    Updating variant: 100% [============================] 1,611 5.1K/s in 00:00:00
    INFO: 1611 records are updated
    

and the results (in field `maf1`) are number of aleternative alleles devided by `6`. 



    % vtools output variant chr pos ref alt num "2*total" maf maf1 -l 5
    

    1	4540	G	A	2	4	0.5	0.333333333333
    1	5683	G	T	1	2	0.5	0.166666666667
    1	5966	T	G	3	6	0.5	0.5
    1	6241	T	C	2	4	0.5	0.333333333333
    1	9992	C	T	2	4	0.5	0.333333333333
    

(:exampleend:) 

All statistics are by default calculated for all samples in the project. But we can limit calculations on subset of samples by `--samples` and/or `--genotypes`. 



*   Option `--samples "[condition]"` can be used to limit the statistics to a subset of samples, for example, all affected individuals. `[condition]` should be an SQL expression using one or more columns shown in `vtools show samples`. For example, 
    
    *   `aff='1'` select all samples with affection status equals to 1. 
    *   `some_measure>0.95` select all samples with some_measure greater than 0.95. 
    *   `filename like 'MG%'` select all samples from files with filename starts with "MG". 
    *   `sample_name like 'CEU%'` select all samples with sample name starts with "CEU". 
    

*   Option `--genotypes "[condition]"` can be used to limit the statistics to a subset of samples having genotypes satisfying a `[condition]` defined by an SQL expression using one or more columns shown in `vtools show genotypes`. For example, 
    
    *   `GT=0` select only the wildtype genotypes 
    *   `GQ>20` select variants having genotype quality greater than 20 

(:toggleexample Examples: statistics for subsets of samples and genotypes:) This project has three samples: 



    % vtools show samples
    

    sample_name	filename
    SAMP1	V1.vcf
    SAMP1	V2.vcf
    SAMP1	V3.vcf
    

Suppose two samples from files `V1.vcf` and `V2.vcf` are affected and we would like to calculate genotype count for only these two samples, we could use parameter `--samples` to limit our calculation: 



    % vtools update variant --from_stat "cases_het=#(het)" --samples "filename in ('V1.vcf', 'V2.vcf')"
    

    INFO: 2 samples are selected
    Counting variants: 100% [==================================================] 2 233.5/s in 00:00:00
    INFO: Adding field cases_het
    Updating variant: 100% [===============================================] 1,341 35.3K/s in 00:00:00
    INFO: 1340 records are updated
    

We can also limit the statistics to genotypes that satisfy certain conditions (e.g. with high coverage): 



    % vtools update variant --from_stat "cases_het_highDP=#(het)" --samples "filename in ('V1.vcf', 'V2.vcf')" --genotypes 'DP>15'
    

    INFO: 2 samples are selected
    Counting variants: 100% [===================================================] 2 80.5/s in 00:00:00
    INFO: Adding field cases_het_highDP
    Updating variant: 100% [=================================================] 621 42.8K/s in 00:00:00
    INFO: 620 records are updated
    



    % vtools output variant chr pos ref alt cases_het cases_het_highDP -l5
    

    1	4540	G	A	1	0
    1	5683	G	T	1	0
    1	5966	T	G	2	1
    1	6241	T	C	1	1
    1	9992	C	T	1	0
    

(:exampleend:) 

(:toggleexample Examples: update variant info of subsets of variants:) If you are interested only in variants in a variant table, you could also only update statistics for variants in specified variant tables. For example, we can select all variants that belong to all three samples and creates a table `in_all`: 

    % vtools select variant 'total=3' -t in_all
    

    Running: 1 71.8/s in 00:00:00                                                                      
    INFO: 511 variants selected.
    

Then we can add a field `case_hom` to count the number of homozygotes for only these variants: 

    % vtools update in_all --from_stat 'case_hom=#(hom)' --samples  "filename in ('V1.vcf', 'V2.vcf')"
    

    INFO: 2 samples are selected
    Counting variants: 100% [===================================================] 2 50.8/s in 00:00:00
    INFO: Adding field case_hom
    Updating in_all: 100% [==================================================] 511 67.1K/s in 00:00:00
    INFO: 510 records are updated
    



    % vtools output in_all chr pos ref alt case_hom -l 5
    

    1	5966	T	G	0
    1	10007	G	A	0
    1	20723	G	C	0
    1	20786	G	T	2
    1	31705	A	G	2
    

(:exampleend:) 



### Add fields based on other variant or annotation fields (`--set`)

Option `--set` evaluates expressions from existing variant info fields and assign results to a new or existing variant info field. For example, once you have calculated allele count, you could calculate allele frequency based on sample size using expression `--set 'maf=m/(n*2.0)'` where *m* is field name calculated as "m=#(alt)" and *n* is sample size, which might not be `"n=#(GT)"` if there are missing data. Note that the denominator should be `n*2.0`, not `n*2`, because SQL requires the denominator be a FLOAT type, not INTEGER. 

(:toggleexample Examples: set fields from variant info fields:) We can try to calculate the allele frequency using number of aleternative alleles and number of genotypes as follows: 



    % vtools update variant --set "maf=num/(total*2.0)"
    

    INFO: Adding field maf
    

However, because samples from this project is called individually and wildtype alleles are not recorded, there are a lot of missing data. If we assume missing data as non-recorded wildtype alleles, we should add missing data to the denominator of the expression. To do this, we need to first calculate a field for missing genotypes: 



    % vtools update variant --from_stat 'missing=#(missing)'
    

    Counting variants: 100% [=====================================================================] 3 289.1/s in 00:00:00
    INFO: Resetting values at existing field missing
    Updating variant: 100% [==================================================================] 1,611 49.5K/s in 00:00:00
    INFO: 1610 records are updated
    

And then calculate the real allele frequency 



    % vtools update variant --set "real_maf=num/((total+missing)*2.0)"
    

    INFO: Adding field real_maf
    

Two allele frequencies are different 



    % vtools output variant chr pos ref alt num total missing maf real_maf -l5
    

    1	4540	G	A	2	2	1	0.5	0.333333333333
    1	5683	G	T	1	1	2	0.5	0.166666666667
    1	5966	T	G	3	3	0	0.5	0.5
    1	6241	T	C	2	2	1	0.5	0.333333333333
    1	9992	C	T	2	2	1	0.5	0.333333333333
    

(:exampleend:) 

In addition to variant info fields, annotation fields could also be used in these expressions and set variant info fields. There is usually no such need to copy annotation fields to variant info fields though. Moreoever, because one variant might have more than one annotation value for an annotation field (e.g. a variant might belong to two isoforms of a gene), copying annotation fields to variant info fields might loss information. 

(:toggleexample Examples: set variant info field from annotation fields:) 



    % vtools use refGene-hg18_20110909
    % vtools update variant --set refgene=refGene.name
    

    INFO: Adding field refgene
    Updating variant: 100% [=================================================================] 11,984 52.6K/s in 00:00:00
    

We can select variants that belong to a gene and output it 



    % vtools select variant 'refGene is not NULL' -t in_gene
    

    Running: 0 0.0/s in 00:00:00                                                                                          
    INFO: 144 variants selected.
    



    % vtools output in_gene chr pos ref alt refGene -l 5
    

    1	887188	G	C	NM_198317
    1	887427	T	C	NM_198317
    1	889872	C	G	NM_198317
    1	890115	G	A	NM_198317
    1	890593	G	A	NM_198317
    

(:exampleend:)

 [1]: http://www.1000genomes.org/node/101
 [2]: http://regulome.stanford.edu/
 [3]: http://localhost/~iceli/wiki/pmwiki.php?n=Format.HomePage?action=edit
 [4]: http://localhost/~iceli/wiki/pmwiki.php?n=Format.Map?action=edit
 [5]: http://www.sqlite.org/lang_aggfunc.html