# Datasets for Mapping Exercises

## RNAseq from SEQC

## ChIPSeq based on RNAseq

### Transcript factor model

### Methylation Factor model

# Lecture Notes

## Introduction

### Non-mapping cases

- Exercise pipeline

- k-mer based methods

	- Kallisto, Sailfish

- Most bioinformatics pipeline have a mapping step

- Mapping is just sub-string find:
```
Given string s and G over some common alphabet
	Len(s)<=Len(G)
	find pos of s in G
```

which is conceptually a pretty simple problem. 

- But there are two wrinkles (complications):

	- Size (N): len(G)==N is enormouse (3,000,000+ for human)
	and while len(s)==m is usually tiny (50--200) we have $10^8$
	to $10^9$ of them
	
	- Imperfect and/or multiple matches: we want to find the best closest match
	
		- actually want both best and sub-optimal
		- sometimes just one, a few, all
		- most algorithm trade of speed for ability to find all
		
- Dealing with size issue (and mismatches partially)

	- Standard solution when searching in a large space; build index
	
	- For searching strings the index is usually a suffix tree
	
	> In computer science, a suffix tree (also called PAT tree or, in an earlier form, 	position tree) is a compressed trie containing all the suffixes of the given text as their 	keys and positions in the text as their values. Suffix trees allow particularly fast 	implementations of many important string operations.
	[Suffix tree - Wikipedia](https://en.wikipedia.org/wiki/Suffix_tree)
	
	![Wiki](images/495px-Suffix_tree_BANANA.svg.png "Suffix Tree (wiki)")
	
	- Pros: 
	
		- search for exact match is $O(m)$

		- search for regular expression is expect sublinear in $N$
		
	- Cons:
	
		- Size: Index way larger than $N$
		
			- Use of BWT transform drastically helps reduce the size issue
		
[//]: # ($\mathcal{O}(\mathrm{len}(s))$)

- Imperfect partial matches

	- Long history of work on this; the original sequence (proteins between species) searches look at highly divergent sequences.
	
	- Read Durbin
	
	- Pairwise alignments:
	
		- Dynamic Programming
	
		- Global: Needleman-Wunsch
		
		- Local: Smith-Waterman
		
	- BLAST
	
	- BLAT
	
	- Ultra-fast methods

		- BWA

		- Bowtie

		- SHRiMP
		
	- Spliced aligners
	
		- Get comp paper (Engstrom et al)
		
		- TopHat
		
		- STAR
		
			- Two pass from Engstrom
		
	- Discussion of speed vs quality. Fast alone is not a great thing
	
	- Mappless aligners
	
		- k-mer
		
		- Sailfish
		
		- Kallisto
		
- Genomes

	- Builds / version
	
		- Chromosome naming annoyance
		
			- Disagreements 
			
				- (chr1 vs 1)
				- chrM vs chrMT in human (not just different name)
				
			- Who thought this was a good idea (chrI, chrII)
			
	- Full builds (random / unassembled)
		
		- USE THEM!!
			
	- Decoy (for variant)
		
	- Hybrid genomes (xenografts)
	
		- With 64bit indexs can build 4Gb+ genomes
		
	- Sources
	
		- UCSC (old favorite; lots of annotation easily available)
		
		- NCBI/ENSEMBLE (less easy to use; but seems be becoming more standard)
		
		- Have agreed on genome names; not chromosome names
	
		- iGenome (if they have your org probably the best place)
		
	- Indexes
		
		- specific to aligner and usually version specific
		
		- Spliced aligners also have Gene Model Dependence
		
			- Star has read length also
			
	