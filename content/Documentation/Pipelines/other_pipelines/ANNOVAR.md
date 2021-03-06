
+++
title = "ANNOVAR"
weight =5
+++



## Gene-based annotation through ANNOVAR



### 1. Usage

    % vtools show pipeline ANNOVAR
    
    Pipeline to call ANNOVAR and import results as variant info fields.
    
    Available pipelines: geneanno
    
    Pipeline "geneanno":  This pipeline exports variants in specified variant
    table (parameter --var_table, default to variant), executes ANNOVAR's gene-
    based annotation (annotate_variantion.pl --geneanno), and imports specified
    fields from output of the command. Four fields (two for all variants and two
    for exonic variants) will be imported unless you disable some of them using
    parameters --variant_info and --exonic_info. No input or output file is
    required for this pipeline, but a snapshot could be specified, in which case
    the snapshot will be loaded (and overwrite the present project).
      geneanno_0:         Load specified snapshot if a snapshot is specified.
                          Otherwise use the existing project.
      geneanno_10:        Check the existence of ANNOVAR's annotate_variation.pl
                          command.
      geneanno_11:        Determine the humandb path of ANNOVAR
      geneanno_14:        Download gene database
      geneanno_20:        Export variants in ANNOVAR format
      geneanno_30:        Execute ANNOVAR annotate_variation.pl --geneanno
      geneanno_40:        Importing results from ANNOVAR output .variant_function
                          if --variant_info is specified
      geneanno_50:        Importing results from ANNOVAR output
                          .exonic_variant_function if --exonic_info is specified
    
    Pipeline parameters:
      var_table           Variant table for the variants to be analyzed.
                          (default: variant)
      annovar_path        Path to a directory that contains
                          annotate_variation.pl, if the script is not in
                          the default \\(PATH.
      dbtype              --dbtype parameter that will be passed to
                          annotate_variation.pl --dbtype. The default
                          value if refGene, but you can also use
                          knownGene, ensGene. (default: refGene)
      variant_info        Fields to import from the first two columns of
                          .variant_function output of ANNOVAR. (default:
                          region_type, region_name)
      exonic_info         Fields to import from the
                          .exonic_variant_function output of ANNOVAR. It
                          has to be zero, one or more of mut_type and
                          function. (default: mut_type, function)
    



### 2. Details

This pipeline makes it easier to use (a small portion of) ANNOVAR's gene-based annotation features to annotate variants, and update the *variant tools* project with the outputs. To use this pipeline, you should first download and install [ANNOVAR][1] somewhere, then execute a command similar to 



    % vtools execute ANNOVAR  geneanno --annovar_path ~/bin/annovar

    INFO: Executing step geneanno_0 of pipeline ANNOVAR: Load specified snapshot if a snapshot is specified. Otherwise use the existing project.
    INFO: Executing step geneanno_10 of pipeline ANNOVAR: Check the existence of ANNOVAR's annotate_variation.pl command.
    INFO: Command /Users/bpeng/bin/annovar/annotate_variation.pl is located.
    INFO: Executing step geneanno_11 of pipeline ANNOVAR: Determine the humandb path of ANNOVAR
    INFO: Running which /Users/bpeng/bin/annovar/annotate_variation.pl > cache/annovar.path
    INFO: Command "which /Users/bpeng/bin/annovar/annotate_variation.pl > cache/annovar.path" completed successfully in 00:00:01
    INFO: Executing step geneanno_14 of pipeline ANNOVAR: Download gene database
    INFO: Running /Users/bpeng/bin/annovar/annotate_variation.pl --buildver hg19 -downdb refGene /Users/bpeng/bin/annovar/humandb
    INFO: Command "/Users/bpeng/bin/annovar/annotate_variation.pl --buildver hg19 -downdb refGene /Users/bpeng/bin/annovar/humandb" completed successfully in 00:00:23
    INFO: Executing step geneanno_20 of pipeline ANNOVAR: Export variants in ANNOVAR format
    WARNING: Ignoring older existing output file cache/annovar_input.
    INFO: Running vtools export variant --format ANNOVAR --output cache/annovar_input
    INFO: Command "vtools export variant --format ANNOVAR --output cache/annovar_input" completed successfully in 00:00:01
    INFO: Executing step geneanno_30 of pipeline ANNOVAR: Execute ANNOVAR annotate_variation.pl --geneanno
    INFO: Running /Users/bpeng/bin/annovar/annotate_variation.pl --geneanno --dbtype refGene --buildver hg19 cache/annovar_input /Users/bpeng/bin/annovar/humandb
    INFO: Command "/Users/bpeng/bin/annovar/annotate_variation.pl --geneanno --dbtype refGene --buildver hg19 cache/annovar_input /Users/bpeng/bin/annovar/humandb" completed successfully in 00:00:12
    INFO: Executing step geneanno_40 of pipeline ANNOVAR: Importing results from ANNOVAR output .variant_function if --variant_info is specified
    INFO: Executing step geneanno_50 of pipeline ANNOVAR: Importing results from ANNOVAR output .exonic_variant_function if --exonic_info is specified
    INFO: Running vtools update variant --from_file cache/annovar_input.exonic_variant_function --format ANNOVAR_exonic_variant_function --var_info mut_type, function
    INFO: Using primary reference genome hg19 of the project.
    Getting existing variants: 100% [============================================] 1,611 175.7K/s in 00:00:00
    INFO: Updating variants from cache/annovar_input.exonic_variant_function (1/1)
    annovar_input.exonic_variant_function: 100% [=====================================] 55 9.4K/s in 00:00:00
    INFO: Fields mut_type, function of 55 variants are updated
    INFO: Command "vtools update variant --from_file cache/annovar_input.exonic_variant_function --format ANNOVAR_exonic_variant_function --var_info mut_type, function" completed successfully in 00:00:01
    

The options of this pipelines include 



*   `annovar_path` path to directory with `annotate_variation.pl` if the script is not in `$PATH`. 
*   `var_table` variant table whose variants will be exported and annotated. It is generally a bad idea to annotate millions of variants from a whole-genome sequencing project though. 
*   `dbtype` gene database to be used for the annotation, which can be `refGene` (default), `knownGene`, or `ensGene`. The pipeline will automatically download the required database for the analysis. 
*   `variant_info`: fields to be imported from the `.variant_info` output of `ANNOVAR`, which can be `region_type` and/or `region_name`. If you specify `variant_info` without value, no field will be imported from this file. The example above uses this trick to stop updating variant info from the `.variant_info` output. 
*   `exonict_info`: fields to be imported from the `.exonic_variant_info` output of `ANNOVAR`, which can be `mut_type` and/or `function`. If you specify `exonic_info` without value, no field will be imported from this file. 

After the execution of the pipeline, two variant info fields are added to the project. Not all variants are updated because `mut_type` and `function` are only added forexonic variants. 



    % vtools select variant 'mut_type is not NULL' --output chr pos ref alt mut_type function -l 10

    1	900505  	G	C	synonymous SNV   	KLHL17:NM_198317:exon12:c.G1863C:p.V621V,
    1	1686040 	G	T	nonsynonymous SNV	NADK:NM_001198993:exon8:c.C786A:p.N262K,NADK:NM_023018:exon8:c.C786A:p.N262K,NADK:NM_001198995:exon6:c.C690A:p.N230K,NADK:NM_001198994:exon10:c.C1221A:p.N407K,
    1	9034421 	G	A	nonsynonymous SNV	CA6:NM_001270500:exon8:c.G860A:p.G287E,
    1	9030964 	C	T	synonymous SNV   	CA6:NM_001270500:exon7:c.C768T:p.N256N,CA6:NM_001215:exon7:c.C768T:p.N256N,CA6:NM_001270502:exon5:c.C384T:p.N128N,CA6:NM_001270501:exon6:c.C588T:p.N196N,
    1	16893752	T	G	unknown          	UNKNOWN
    1	16913579	A	G	unknown          	UNKNOWN
    1	16918486	C	T	unknown          	UNKNOWN
    1	17085564	A	G	nonsynonymous SNV	MST1L:NM_001271733:exon9:c.T1157C:p.L386P,
    1	17083782	A	C	nonsynonymous SNV	MST1L:NM_001271733:exon15:c.T2015G:p.F672C,
    1	17087530	C	T	synonymous SNV   	MST1L:NM_001271733:exon2:c.G135A:p.A45A,

 [1]: http://www.openbioinformatics.org/annovar/