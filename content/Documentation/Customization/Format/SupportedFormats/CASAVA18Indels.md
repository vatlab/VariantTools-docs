+++
title = "CASAVA18Indels"
weight = 11
+++

## CASAVA18IndelsCASAVA18Indels

### Sample data

    #	**	CASAVA	depth-filtered	indel	calls	**
    #$	CMDLINE	/filterSmallVariants.pl	--chrom=chr1
    #$	SEQ_MAX_DEPTH	chr1	143.988337798483
    #
    #$	COLUMNS	seq_name	pos	type	ref_upstream	ref/indel	ref_downstream	Q(indel)	max_gtype	Q(max_gtype)	depth	alt_reads	indel_reads	other_reads	repeat_unit	ref_repeat_count	indel_repeat_count
    chr1	10147	1D	CTAACCCTAA	C/-	CCCTAACCCT	70	het	70	6	2	3	1	C	4	3
    chr1	10231	1D	CTAACCCTAA	C/-	CCCTAACCCT	1203	het	284	53	7	30	17	C	4	3
    chr1	10353	1I	CCCTAACCCT	-/A	ACCCTAACCC	434	het	118	17	3	8	9	A	1	2
    chr1	10390	1D	CTAACCCTAA	C/-	CCCTAACCCC	765	het	399	39	9	19	12	C	4	3
    chr1	10397	1D	TAACCCCTAA	C/-	CCCTAACCCT	730	het	496	38	11	20	9	C	4	3
    chr1	10440	1D	CTAACCCTAA	C/-	CCCTAACCCT	774	het	302	31	7	21	3	C	4	3
    chr1	28327	1D	AAGCCTGTAG	T/-	TGCTCATCTG	3	het	3	2	1	1	0	T	2	1
    chr1	54711	1I	AAACCTTGTA	-/T	TTTTTCTTTC	37	het	37	21	8	2	12	T	5	6
    chr1	62240	2D	AGACACACAT	AC/--	ACACACACAC	100	het	100	22	16	4	2	AC	8	7
    chr1	83830	8D	AGAAAGAAAG	AGAAAGAA/--------	AGAAAGAAAG	273	het	161	13	3	6	4	AGAA	11	9
    chr1	108546	BP_RIGHT	N/A	------/CTATCA	AAAAAAAAAA	28	het	28	13	9	2	2	N/A	0	0
    chr1	123089	2D	TGTGGACATG	TA/--	TATATATATA	142	het	142	13	9	4	0	TA	6	5
    chr1	128590	1D	CTTCAAGTTC	A/-	CCCCCTTTTT	220	het	220	13	4	5	8	A	1	0
    chr1	129011	3D	GGGATGTAGA	ATG/---	ATAAGGCTCT	258	het	258	12	5	6	1	ATG	1	0
    chr1	136743	1I	GGTGAGGCAA	-/C	GGGCTCACAC	76	het	76	80	66	6	12	C	0	1
    chr1	136889	1D	TGTGAGGCAA	G/-	GGGCTCGGGC	205	het	205	41	29	8	8	G	4	3
    chr1	237577	1I	AAAGGGGGTT	-/C	ATTATCTAGG	60	het	60	51	45	4	2	C	0	1
    chr1	247917	3D	ACCCAACCTC	AGG/---	AGTTCAGGGC	69	hom	5	2	0	2	0	AGG	1	0
    chr1	255910	2I	TGTGTGTGTA	--/TG	TGTGTGTGTG	257	het	28	7	1	5	1	TG	10	11
    chr1	531809	2D	CACACTTATG	CA/--	CACATTCACA	327	het	327	25	17	8	1	CA	3	2
    chr1	532239	2D	TGTTCACATT	CA/--	CACTCATACA	325	het	325	64	53	10	2	CA	2	1
    chr1	532259	3D	CACAGCCCAA	AAT/---	AATATACACA	303	het	303	61	43	9	10	AAT	2	1
    chr1	537252	2D	AGCCACATGT	GG/--	GACAGGGCAG	6	hom	2	1	0	1	0	G	3	1
    chr1	537494	5I	CAGCGTCCAT	-----/GCCCA	GCCGGCCTCC	23	het	3	2	0	1	1	GCCCA	0	1
    chr1	537641	50D	ATCCCCCTCT	CCATCCCCCTCTCCATCTCCCTCTCCTTTCTCCTCTCTAGCCCCCTCTCC/--------------------------------------------------	TTTCTCCTCT	66	het	66	22	18	3	10	CCATCCCCCTCTCCATCTCCCTCTCCTTTCTCCTCTCTAGCCCCCTCTCC	1	0
    



## Example

    vtools import --format Illumina_INDEL Illumina_INDEL.txt --build hg18 
    
    INFO: Opening project f.proj
    INFO: Additional genotype fields: Q_indel
    INFO: Importing genotype from Illumina_INDEL.txt (1/1)
    Illumina_INDEL.txt: 30
    INFO: 25 new variants from 25 records are imported, with 0 SNVs, 7 insertions, 18 deletions, and 0 complex variants.
    

    vtools show table variant -l -1
    
    INFO: Opening project f.proj
    variant_id, bin, chr, pos, ref, alt
    1, 585, 1, 10147, C, -
    2, 585, 1, 10231, C, -
    3, 585, 1, 10353, -, A
    4, 585, 1, 10390, C, -
    5, 585, 1, 10397, C, -
    6, 585, 1, 10440, C, -
    7, 585, 1, 28327, T, -
    8, 585, 1, 54711, -, T
    9, 585, 1, 62240, AC, -
    10, 585, 1, 83830, AGAAAGAA, -
    11, 585, 1, 108546, -, CTATCA
    12, 585, 1, 123089, TA, -
    13, 585, 1, 128590, A, -
    14, 585, 1, 129011, ATG, -
    15, 586, 1, 136743, -, C
    16, 586, 1, 136889, G, -
    17, 586, 1, 237577, -, C
    18, 586, 1, 247917, AGG, -
    19, 586, 1, 255910, -, TG
    20, 589, 1, 531809, CA, -
    21, 589, 1, 532239, CA, -
    22, 589, 1, 532259, AAT, -
    23, 589, 1, 537252, GG, -
    24, 589, 1, 537494, -, GCCCA
    25, 589, 1, 537641, CCATCCCCCTCTCCATCTCCCTCTCCTTTCTCCTCTCTAGCCCCCTCTCC, -
