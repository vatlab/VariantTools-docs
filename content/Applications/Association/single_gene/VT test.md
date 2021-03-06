
+++
title = "VT test"
weight = 6
+++




## Variable Thresholds Test for Case Control Data Analysis 



### 1. Introduction

The variable thresholds method (VT, Price et al 2010) tests for association between phenotypic values (case control or quantitative traits) with individuals' genotype "score" subject to a variable MAF threshold. It assumes that there exists a fixed yet unknown MAF threshold on a given genetic region which is related to the cutoff for the causality of variants on that loci. In testing the association for the genetic region, for each possible MAF threshold a genotype score is computed based on given collapsing theme, and is tested for association between the phenotype of interest; the final MAF threshold is chosen such that the association signal is strongest. Permutation procedure has to be used to control for type I error due to multiple testing. 

The VT strategy creates a very flexible framework that can be applied to many association tests, including the use of external information such as weight theme by annotation scores, as the VT paper suggested. The `VTtest` method implements the test for case control phenotype using the CMC coding theme and Fisher's exact test, in addition to the original VT statistic. Permutation procedure is optimized due to the use of Fisher's exact test: the minimum <script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script> \\(p\\) value resulted from Fisher's exact test on the original dataset have to exceed the expected significance level in order to enter the permutation test procedure; otherwise it will be reported as it is. This trick reduces the computation time the original VT test would require. 

Please refer to `VariableThresholdsBt` and `VariableThresholdsQt` for more versatile versions of VT method, which tests for both case control and quantitative traits, with/without presence of phenotype co-variates, and is capable of incorporating functional information. 



### 2. Details

#### 2.1 Command interface

    vtools show test VTtest

    Name:          VTtest
    Description:   VT statistic for disease traits, Price et al 2010
    usage: vtools associate --method VTtest [-h] [--name NAME] [-q1 MAFUPPER]
                                            [-q2 MAFLOWER] [--alternative TAILED]
                                            [-p N] [--adaptive C] [--cfisher]
                                            [--midp]
                                            [--moi {additive,dominant,recessive}]
    
    Variable thresholds test for disease traits, Price et al 2010. The burden test
    statistic of a group of variants will be maximized over subsets of variants
    defined by applying different minor allele frequency thresholds. This
    implementation provides two different statistics: the original VT statistics
    in Price et al 2010 (default) and an adaptive VT statistic combining the
    CFisher method (via "--cfisher" option). p-value is estimated by permutation
    test. The adaptive VT statistic will not generate uniformly distributed
    p-value. For a more generalized version of VT test, type "vtools show test
    VariableThresholdsBt / VariableThresholdsQt".
    
    optional arguments:
      -h, --help            show this help message and exit
      --name NAME           Name of the test that will be appended to names of
                            output fields, usually used to differentiate output of
                            different tests, or the same test with different
                            parameters.
      -q1 MAFUPPER, --mafupper MAFUPPER
                            Minor allele frequency upper limit. All variants
                            having sample MAF<=m1 will be included in analysis.
                            Default set to 1.0
      -q2 MAFLOWER, --maflower MAFLOWER
                            Minor allele frequency lower limit. All variants
                            having sample MAF>m2 will be included in analysis.
                            Default set to 0.0
      --alternative TAILED  Alternative hypothesis is one-sided ("1") or two-sided
                            ("2"). Two sided test is only valid with "--cfisher"
                            option evoked (please use "VariableThresholdsBt"
                            otherwise). Default set to 1
      -p N, --permutations N
                            Number of permutations
      --adaptive C          Adaptive permutation using Edwin Wilson 95 percent
                            confidence interval for binomial distribution. The
                            program will compute a p-value every 1000 permutations
                            and compare the lower bound of the 95 percent CI of
                            p-value against "C", and quit permutations with the
                            p-value if it is larger than "C". It is recommended to
                            specify a "C" that is slightly larger than the
                            significance level for the study. To disable the
                            adaptive procedure, set C=1. Default is C=0.1
      --cfisher             This option, if evoked, will use an adaptive VT test
                            via Fisher's exact statistic. For more details, please
                            refer to the online documentation.
      --midp                This option, if evoked, will use mid-p value
                            correction for one-sided Fisher's exact test. It is
                            only applicatable to one sided test with "--cfisher"
                            option.
      --moi {additive,dominant,recessive}
                            Mode of inheritance. Will code genotypes as 0/1/2/NA
                            for additive mode, 0/1/NA for dominant or recessive
                            model. Default set to additive
    
    



#### 2.2 Application

<details><summary> Example using **snapshot** `vt_ExomeAssociation`</summary> 



    vtools associate rare status -m "VTtest --name vt -p 5000" --group_by name2 --to_db vt -j8 \
    > vt.txt

    INFO: 3180 samples are found
    INFO: 2632 groups are found
    INFO: Starting 8 processes to load genotypes
    Loading genotypes: 100% [=========================================================================================================================================] 3,180 14.6/s in 00:03:37
    Testing for association: 100% [================================================================================================================================] 2,632/591 5.4/s in 00:08:04
    INFO: Association tests on 2632 groups have completed. 591 failed.
    INFO: Using annotation DB vt in project test.
    INFO: Annotation database used to record results of association tests. Created on Thu, 31 Jan 2013 20:48:50
    
    
    vtools show fields | grep vt

    vt.name2                     name2
    vt.sample_size_vt            sample size
    vt.num_variants_vt           number of variants in each group (adjusted for specified MAF
    vt.total_mac_vt              total minor allele counts in a group (adjusted for MOI)
    vt.statistic_vt              test statistic.
    vt.pvalue_vt                 p-value
    vt.std_error_vt              Empirical estimate of the standard deviation of statistic
    vt.num_permutations_vt       number of permutations at which p-value is evaluated
    



    head vt.txt
    

    name2	sample_size_vt	num_variants_vt	total_mac_vt	statistic_vt	pvalue_vt	std_error_vt	num_permutations_vt
    ABCD3	3180	3	42	-0.0717229	0.753247	0.316505	1000
    ABCB10	3180	6	122	0.37788	0.396603	0.339246	1000
    ABCG5	3180	6	87	0.204207	0.58042	0.337788	1000
    ABCB6	3180	7	151	0.180786	0.609391	0.328099	1000
    AADACL4	3180	5	138	-0.284233	0.986014	0.302561	1000
    AAMP	3180	3	35	0.158666	0.474525	0.306232	1000
    ABHD1	3180	5	29	-0.0677963	0.771229	0.343744	1000
    ABL2	3180	4	41	0.0378648	0.593407	0.34231	1000
    ABCG8	3180	12	152	0.187745	0.612388	0.310029	1000
    

    vtools associate rare status -m "VTtest --name vtfisher --cfisher --midp -p 5000" --group_b\
    y name2 --to_db vtfisher -j8 > vtfisher.txt
    

    vtools show fields | grep vtfisher
    
    
    head vtfisher.txt
    



    

</details>

### Reference

Alkes L. Price, Gregory V. Kryukov, Paul I.W. de Bakker, Shaun M. Purcell, Jeff Staples, Lee-Jen Wei and Shamil R. Sunyaev (2010) **Pooled Association Tests for Rare Variants in Exon-Resequencing Studies**. *The American Journal of Human Genetics* doi:`10.1016/j.ajhg.2010.04.005`. <http://linkinghub.elsevier.com/retrieve/pii/S0002929710002077>