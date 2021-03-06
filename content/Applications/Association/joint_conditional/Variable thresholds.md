
+++
title = "variable thresholds"
weight = 4
+++



## Varible Thresholds Methods for Disease and Quantitative Traits 



### 1. Introduction

This implements the variable thresholds version of [aggregation methods][1]. Similar to the [VT][2] method, the `VariableThresholdsBt` and `VariableThresholdsQt` tests use a variable thresholds definition for the rare variants being considered such that multiple test statistics are calculated for an aggregation unit. The final statistic is taken as the one that gives the best result. Type I error is controlled due to the use of permutation testing. 

Results from variable thresholds methods have one additional column, i.e., an MAF column reporting the statistic of VT at which it is derived. 



### 2. Details

#### 2.1 Command interface

    vtools show test VariableThresholdsBt
    

    Name:          VariableThresholdsBt
    Description:   Variable thresholds method for disease traits, in the spirit of Price
                   et al 2010
    usage: vtools associate --method VariableThresholdsBt [-h] [--name NAME]
                                                          [-q1 MAFUPPER]
                                                          [-q2 MAFLOWER]
                                                          [--alternative TAILED]
                                                          [-p N] [--permute_by XY]
                                                          [--adaptive C]
                                                          [--NA_adjust]
                                                          [--moi {additive,dominant,recessive}]
    
    Variable thresholds in burden test for disease traits (in the spirit of Price
    et al 2010). The burden test statistic of a group of variants will be
    maximized over subsets of variants defined by applying different minor allele
    frequency thresholds. Significance of the statistic obtained is evaluated via
    permutation
    
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
                            ("2"). Default set to 1
      -p N, --permutations N
                            Number of permutations
      --permute_by XY       Permute phenotypes ("Y") or genotypes ("X"). Default
                            is "Y"
      --adaptive C          Adaptive permutation using Edwin Wilson 95 percent
                            confidence interval for binomial distribution. The
                            program will compute a p-value every 1000 permutations
                            and compare the lower bound of the 95 percent CI of
                            p-value against "C", and quit permutations with the
                            p-value if it is larger than "C". It is recommended to
                            specify a "C" that is slightly larger than the
                            significance level for the study. To disable the
                            adaptive procedure, set C=1. Default is C=0.1
      --NA_adjust           This option, if evoked, will replace missing genotype
                            values with a score relative to sample allele
                            frequencies. The association test will be adjusted to
                            incorporate the information. This is an effective
                            approach to control for type I error due to
                            differential degrees of missing genotypes among
                            samples.
      --moi {additive,dominant,recessive}
                            Mode of inheritance. Will code genotypes as 0/1/2/NA
                            for additive mode, 0/1/NA for dominant or recessive
                            model. Default set to additive
    



    vtools show test VariableThresholdsQt
    



    Name:          VariableThresholdsQt
    Description:   Variable thresholds method for quantitative traits, in the spirit of
                   Price et al 2010
    usage: vtools associate --method VariableThresholdsQt [-h] [--name NAME]
                                                          [-q1 MAFUPPER]
                                                          [-q2 MAFLOWER]
                                                          [--alternative TAILED]
                                                          [-p N] [--permute_by XY]
                                                          [--adaptive C]
                                                          [--NA_adjust]
                                                          [--moi {additive,dominant,recessive}]
    
    Variable thresholds in burden test for quantitative traits (in the spirit of
    Price et al 2010). The burden test statistic of a group of variants will be
    maximized over subsets of variants defined by applying different minor allele
    frequency thresholds. Significance of the statistic obtained is evaluated via
    permutation
    
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
                            ("2"). Default set to 1
      -p N, --permutations N
                            Number of permutations
      --permute_by XY       Permute phenotypes ("Y") or genotypes ("X"). Default
                            is "Y"
      --adaptive C          Adaptive permutation using Edwin Wilson 95 percent
                            confidence interval for binomial distribution. The
                            program will compute a p-value every 1000 permutations
                            and compare the lower bound of the 95 percent CI of
                            p-value against "C", and quit permutations with the
                            p-value if it is larger than "C". It is recommended to
                            specify a "C" that is slightly larger than the
                            significance level for the study. To disable the
                            adaptive procedure, set C=1. Default is C=0.1
      --NA_adjust           This option, if evoked, will replace missing genotype
                            values with a score relative to sample allele
                            frequencies. The association test will be adjusted to
                            incorporate the information. This is an effective
                            approach to control for type I error due to
                            differential degrees of missing genotypes among
                            samples.
      --moi {additive,dominant,recessive}
                            Mode of inheritance. Will code genotypes as 0/1/2/NA
                            for additive mode, 0/1/NA for dominant or recessive
                            mode. Default set to additive
    



#### 2.2 Application

<details><summary> Example using **snapshot** `vt_ExomeAssociation`</summary> 



    vtools associate rare status --covariates age gender bmi exposure -m "VariableThresholdsBt \
    --name VariableThresholdsBt --alternative 2 -p 5000 --permute_by X --adaptive 0.05" --group\
    _by name2 --to_db variablethresholdsBt -j8 > variablethresholdsBt.txt
    

    vtools show fields | grep variablethresholdsBt.txt
    

    head variablethresholdsBt.txt
    

    vtools associate rare bmi --covariates age gender exposure -m "VariableThresholdsQt --name \
    VariableThresholdsQt --alternative 2 -p 5000 --permute_by X --adaptive 0.05" --group_by nam\
    e2 --to_db variablethresholdsQt -j8 > variablethresholdsQt.txt
    
    INFO: 3180 samples are found
    INFO: 2632 groups are found
    INFO: Starting 8 processes to load genotypes
    Loading genotypes: 100% [=========================================================================================================================================] 3,180 34.2/s in 00:01:33
    Testing for association: 100% [================================================================================================================================] 2,632/147 2.8/s in 00:15:35
    INFO: Association tests on 2632 groups have completed. 147 failed.
    INFO: Using annotation DB variablethresholdsQt in project test.
    INFO: Annotation database used to record results of association tests. Created on Thu, 31 Jan 2013 22:54:27
    

    vtools show fields | grep variablethresholdsQt.txt

    variablethresholdsQt.name2   name2
    variablethresholdsQt.sample_size_VariableThresholdsQt sample size
    variablethresholdsQt.num_variants_VariableThresholdsQt number of variants in each group (adjusted for specified MAF
                                 upper/lower bounds)
    variablethresholdsQt.total_mac_VariableThresholdsQt total minor allele counts in a group (adjusted for MOI)
    variablethresholdsQt.beta_x_VariableThresholdsQt test statistic. In the context of regression this is estimate of
                                 effect size for x
    variablethresholdsQt.pvalue_VariableThresholdsQt p-value
    variablethresholdsQt.std_error_VariableThresholdsQt Empirical estimate of the standard deviation of statistic under the
                                 null
    variablethresholdsQt.num_permutations_VariableThresholdsQt number of permutations at which p-value is evaluated
    variablethresholdsQt.MAF_threshold_VariableThresholdsQt The minor allele frequency at which the test statistic is maximized
    

    head variablethresholdsQt.txt
    
    name2	sample_size_VariableThresholdsQt	num_variants_VariableThresholdsQt	total_mac_VariableThresholdsQt	beta_x_VariableThresholdsQt	pvalue_VariableThresholdsQt	std_error_VariableThresholdsQt	num_permutations_VariableThresholdsQt	MAF_threshold_VariableThresholdsQt
    ABCB10	3180	6	122	5.93777	0.247752	3.68504	1000	0.000157233
    ABCD3	3180	3	42	-1.48612	0.301698	0.740477	1000	0.00267296
    AADACL4	3180	5	138	-1.70538	0.151848	0.927952	1000	0.00157233
    AAMP	3180	3	35	2.352	0.0666445	0.923285	3000	0.00220126
    ABCB6	3180	7	151	1.98423	0.655345	3.83344	1000	0.000157233
    ABI2	3180	1	25	0	0.993007	0	1000	0.00393082
    ABHD1	3180	5	29	-1.81424	0.257742	1.19045	1000	0.000314465
    ABCG8	3180	12	152	-3.48381	0.143856	1.26176	1000	0.000786164
    ACAP3	3180	3	17	2.70541	0.281718	2.01218	1000	0.000314465
    

</details>

 [1]:   /applications/association/joint_conditional/aggre/
 [2]:   /applications/association/joint_conditional/variable-thresholds/