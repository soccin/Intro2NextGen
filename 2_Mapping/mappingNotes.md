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

	- Size (N): len(G) is enormouse (3,000,000+ for human)
	and while len(s) is usually tiny (50--200) we have $10^8$
	to $10^9$
	
	- Imperfect matches: we want to find the closest match
	
		- actually want both best and sub-optimal
		- sometimes just one, a few, all
		
- Dealing with size issue

	- Standard solution when searching in a large space; build index
	
	- For searching strings the index is a suffix tree
		