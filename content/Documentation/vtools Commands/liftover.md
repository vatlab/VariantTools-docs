
+++
title = "lifttover"
description = ""
weight = 5
+++




# Adding coordinates from an alternative reference genome 

## Usage

    % vtools liftover -h
    

    usage: vtools liftover [-h] [--flip] [-v STD[LOG]] build
    
    Convert coordinates of existing variants to alternative coordinates in an
    alternative reference genome. The UCSC liftover tool will be automatically
    downloaded if it is not available.
    
    positional arguments:
      build                 Name of the alternative reference genome
    
    optional arguments:
      -h, --help            show this help message and exit
      --flip                Flip primary and alternative reference genomes so that
                            the specified build will become the primary reference
                            genome of the project.
      -v STD[LOG], --verbosity STD[LOG]
                            Output error and warning (0), info (1) and debug (2)
                            information to standard output (default to 1), and to
                            a logfile (default to 2).
    



## Details

Vtools provides a command which is based on the tool of USCS liftOver to map the variants from existing reference genome to an alternative build. After executing of this command, The fields of chromosome, position reference and alternative of the variant in current and previous reference genomes are all in the master variant table. 



An illustration of the liftover process 

Attach:liftover.png 



*   This command adds `alt_chr` and `alt_pos` columns to the master variants table. 
*   Annotation databases that use the alternative reference genome can now be used. 
*   `vtools output` and `vtools export` can output alternative coordinates using parameter `--build`. 



1.  This feature is unavailable under windows because UCSC liftOver tool does not support windows. 
2.  Because the UCSC liftover tools does not guarantee complete translation, variants that failed to map will have missing alternative coordinates. 

(:toggleexample Liftover from hg18 to hg19:) The following example demonstrates how to liftOver a project from hg18 to hg19. Note that the UCSC liftOver tool and needed chain files are automatically downloaded if they are not available. 



    % vtools liftover hg19
    

    INFO: Downloading liftOver tool from UCSC
    INFO: Downloading liftOver chain file from UCSC
    INFO: Exporting variants in BED format
    Exporting variants: 100% [======================>] 12,994 151.9K/s in 00:00:00
    INFO: Running UCSC liftOver tool
    INFO: Reading liftover chains
    Mapping coordinates
    INFO: 280 records failed to map.
    Updating table variant: 100% [======================>] 12,714 31.2K/s in 00:00:00
    

After the liftOver operation, three more fields are added to the master variant table (alt\_bin, alt\_chr, alt_pos) 



    % vtools show table variant
    

    variant_id, bin, chr, pos, ref, alt, DP, alt_bin, alt_chr, alt_pos
    1, 585, 1, 533, G, C, 423, 585, 1, 10533
    2, 585, 1, 41342, T, A, 188, 585, 1, 51479
    3, 585, 1, 41791, G, A, 192, 585, 1, 51928
    4, 585, 1, 44449, T, C, 166, 585, 1, 54586
    5, 585, 1, 44539, C, T, 131, 585, 1, 54676
    6, 585, 1, 44571, G, C, 135, 585, 1, 54708
    7, 585, 1, 45162, C, T, 166, 585, 1, 55299
    8, 585, 1, 52066, T, C, 159, 585, 1, 62203
    9, 585, 1, 53534, G, A, 243, 585, 1, 63671
    10, 585, 1, 75891, T, C, 182, 585, 1, 86028
    (12984 records omitted, use parameter --limit to see more)
    

(:exampleend:) 

(:toggleexample Flipping primary and alternative reference genome:) 



    % vtools show
    

    Project name:                test
    Primary reference genome:    hg18
    Secondary reference genome:  hg19
    Database engine:             sqlite3
    Variant tables:              variant
    Annotation databases:
    

    % vtools liftover hg19 --flip
    

    INFO: Downloading liftOver tool from UCSC
    INFO: Downloading liftOver chain file from UCSC
    INFO: Exporting variants in BED format
    Exporting variants: 100.0% [=========>] 577 55.9K/s in 00:00:00
    INFO: Running UCSC liftOver tool
    INFO: Reading liftover chains
    Mapping coordinates
    INFO: 5 records failed to map.
    INFO: Flipping primary and alterantive reference genome
    Updating table variant: 100.0% [===========>] 572 12.7K/s in 00:00:00
    



Interruption of the flipping process will leave the project unusable because of mixed coordinates. 



    % vtools show
    

    Project name:                test
    Primary reference genome:    hg19
    Secondary reference genome:  hg18
    Database engine:             sqlite3
    Variant tables:              variant
    Annotation databases: 
    

    % vtools show table variant
    

    variant_id, bin, chr, pos, ref, alt, DP, alt_bin, alt_chr, alt_pos
    1, 585, 1, 10533, G, C, 423, 585, 1, 533
    2, 585, 1, 51479, T, A, 188, 585, 1, 41342
    3, 585, 1, 51928, G, A, 192, 585, 1, 41791
    4, 585, 1, 54586, T, C, 166, 585, 1, 44449
    5, 585, 1, 54676, C, T, 131, 585, 1, 44539
    6, 585, 1, 54708, G, C, 135, 585, 1, 44571
    7, 585, 1, 55299, C, T, 166, 585, 1, 45162
    8, 585, 1, 62203, T, C, 159, 585, 1, 52066
    9, 585, 1, 63671, G, A, 243, 585, 1, 53534
    10, 585, 1, 86028, T, C, 182, 585, 1, 75891
    (12984 records omitted, use parameter --limit to see more)
    

(:exampleend:)