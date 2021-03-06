
+++
title = "refGene"
weight = 5
+++

## Ref Gene

The RefGene database was created from the UCSC database. RefGene specifies known human protein-coding and non-protein-coding genes taken from the NCBI RNA reference sequences collection (RefSeq). If you would like to annotate your variants to genes, you can use the simpler **refGene** database. If you would like to determine the exons that your variants are in, use the **refGene_exon** database. See the available annotation fields for each database below. 

Description of the database from [NCBI][1]: 

RefSeqGene, a subset of NCBI's Reference Sequence (RefSeq) project, defines genomic sequences to be used as reference standards for well-characterized genes. These sequences, labeled with the keyword RefSeqGene in NCBI's nucleotide database, serve as a stable foundation for reporting mutations, for establishing conventions for numbering exons and introns, and for defining the coordinates of other variations. RefSeq mRNA and protein sequences have long been used for this purpose, but have the obvious weakness of not providing explicit coordinates for flanking or intronic sequence. RefSeq chromosome sequences do provide explicit coordinates no matter the relationship to any gene annotation, but have awkwardly large coordinate values that will change when the sequence is updated because of a re-assembly. Sequences of the RefSeqGene project counter both of these drawbacks by providing more stable gene-specific genomic sequence for each gene, as well as including upstream and downstream flanking regions. If modifications must be made to any RefSeqGene sequence, it will be versioned and tools will be provided to facilitate conversion of coordinates. The RefSeqGene sequences are aligned to reference chromosomes, and current and previous chromosome coordinates are available because of that re-alignment. The Clinical Remap tool make that conversion easy. 

Although the majority of UCSC Known Genes (KG) are identical to RefSeq genes, there are some significant differences according to [this post][2]: 

*   Not every RefSeq is a KG. Some RefSeqs were filtered out because they did not pass our gene-check processing step (e.g. RefSeqs with no start or stop codons, or bad reading frames are filtered out). 
*   If there is a UniProt protein which maps well to a GenBank mRNA, and it passes the gene-check filter and there is no equal or better corresponding RefSeq, the mRNA/UniProt pair will be added to the KG data set. 
*   UCSC KG is updated once in a few months. Our RefSeq track is updated nightly. So the refGene table may contains some latest RefSeq updates that came after the last KG build. 



### 1. refGene

    % vtools show annotation refGene -v2
    

    Annotation database refGene (version hg19_20130904)
    Description:            Known human protein-coding and non-protein-coding
      genes taken from the NCBI RNA reference sequences collection (RefSeq).
    Database type:          range
    Number of records:      45,749
    Distinct ranges:        32,293
    Reference genome hg19:  chr, txStart, txEnd
    
    Field:                  name
    Type:                   string
    Comment:                Gene name
    Missing entries:        0
    Unique Entries:         42,286
    
    Field:                  chr
    Type:                   string
    Missing entries:        0
    Unique Entries:         49
    
    Field:                  strand
    Type:                   string
    Comment:                which DNA strand contains the observed alleles
    Missing entries:        0
    Unique Entries:         2
    
    Field:                  txStart
    Type:                   integer
    Comment:                Transcription start position
    Missing entries:        0
    Unique Entries:         28,789
    Range:                  6011 - 249200442
    
    Field:                  txEnd
    Type:                   integer
    Comment:                Transcription end position
    Missing entries:        0
    Unique Entries:         28,737
    Range:                  14409 - 249213345
    
    Field:                  cdsStart
    Type:                   integer
    Comment:                Coding region start
    Missing entries:        0
    Unique Entries:         30,038
    Range:                  6011 - 249211537
    
    Field:                  cdsEnd
    Type:                   integer
    Comment:                Coding region end
    Missing entries:        0
    Unique Entries:         30,147
    Range:                  14409 - 249212562
    
    Field:                  exonCount
    Type:                   integer
    Comment:                Number of exons
    Missing entries:        0
    Unique Entries:         113
    Range:                  1 - 363
    
    Field:                  score
    Type:                   integer
    Comment:                Score
    Missing entries:        0
    Unique Entries:         1
    Range:                  0 - 0
    
    Field:                  name2
    Type:                   string
    Comment:                Alternative name
    Missing entries:        0
    Unique Entries:         23,953
    
    Field:                  cdsStartStat
    Type:                   string
    Comment:                cds start stat, can be 'non', 'unk', 'incompl', and
                            'cmp1'
    Missing entries:        0
    Unique Entries:         3
    
    Field:                  cdsEndStat
    Type:                   string
    Comment:                cds end stat, can be 'non', 'unk', 'incompl', and
                            'cmp1'
    Missing entries:        0
    Unique Entries:         3
    



### 2. refGene_exon

    % vtools show annotation refGene_exon -v2
    

    Annotation database refGene_exon (version hg19_20130904)
    Description:            RefGene specifies known human protein-coding and non-
      protein-coding genes taken from the NCBI RNA reference sequences collection
      (RefSeq). This database contains all exome regions of the refSeq genes.
    Database type:          range
    Number of records:      443,218
    Distinct ranges:        240,821
    Reference genome hg19:  chr, exon_start, exon_end
    
    Field:                  name
    Type:                   string
    Comment:                Gene name
    Missing entries:        0
    Unique Entries:         42,286
    
    Field:                  chr
    Type:                   string
    Missing entries:        0
    Unique Entries:         49
    
    Field:                  strand
    Type:                   string
    Comment:                which DNA strand contains the observed alleles
    Missing entries:        0
    Unique Entries:         2
    
    Field:                  txStart
    Type:                   integer
    Comment:                Transcription start position
    Missing entries:        0
    Unique Entries:         28,789
    Range:                  6011 - 249200442
    
    Field:                  txEnd
    Type:                   integer
    Comment:                Transcription end position
    Missing entries:        0
    Unique Entries:         28,737
    Range:                  14409 - 249213345
    
    Field:                  cdsStart
    Type:                   integer
    Comment:                Coding region start
    Missing entries:        0
    Unique Entries:         30,038
    Range:                  6011 - 249211537
    
    Field:                  cdsEnd
    Type:                   integer
    Comment:                Coding region end
    Missing entries:        0
    Unique Entries:         30,147
    Range:                  14409 - 249212562
    
    Field:                  exonCount
    Type:                   integer
    Comment:                Number of exons
    Missing entries:        0
    Unique Entries:         113
    Range:                  1 - 363
    
    Field:                  exon_start
    Type:                   integer
    Comment:                exon start position
    Missing entries:        0
    Unique Entries:         235,886
    Range:                  6011 - 249211478
    
    Field:                  exon_end
    Type:                   integer
    Comment:                exon end position
    Missing entries:        0
    Unique Entries:         236,105
    Range:                  6168 - 249213345
    
    Field:                  score
    Type:                   integer
    Comment:                Score
    Missing entries:        0
    Unique Entries:         1
    Range:                  0 - 0
    
    Field:                  name2
    Type:                   string
    Comment:                Alternative name
    Missing entries:        0
    Unique Entries:         23,953
    
    Field:                  cdsStartStat
    Type:                   string
    Comment:                cds start stat, can be 'non', 'unk', 'incompl', and
                            'cmp1'
    Missing entries:        0
    Unique Entries:         3
    
    Field:                  cdsEndStat
    Type:                   string
    Comment:                cds end stat, can be 'non', 'unk', 'incompl', and
                            'cmp1'
    Missing entries:        0
    Unique Entries:         3

 [1]: http://www.ncbi.nlm.nih.gov/refseq/rsg/about/
 [2]: https://lists.soe.ucsc.edu/pipermail/genome/2006-November/012041.html