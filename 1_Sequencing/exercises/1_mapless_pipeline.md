# Pooled RNAi pipeline

## Intro

While most next generation sequencing analysis pipelines require the data to be mapped to the target genome there are some pipelines that can be done without any alignment step. One such problem is the analysis of pooled shRNA experiments such as in *Zuber, et. al, 2011*[^Zuber2011]

In this experiment a library of hairpins is introduced into a population of cells and depending on which hairpin gets encorporated that particular cell my grow or die. By sequencing using primers specific to the hairpin contructs you can determine which shRNA lead to proliferation and survival or arrest. 

## Sequence design

A typical shRNA construct will look as follows;

![](../images/shRNAconstruct.png "shRNAconstruct")

For this excercise we will focus on a core region that contains the following sequence elements:

![](../images/shRNAseqLayout.png "shRNAseqLayout")

The shRNA sequence goes from position 1 to 22. There is an index sequence that for this example is fixed and is from position 23 to 28 and then an adapter sequence. 

Given a FASTQ file of sequences we want to count the abundance of the shRNA sequences in it. shRNA's with higher abudances may indicate that the gene targeted by them potentially responsible for limited cell proliferation (i.e., repression them turns growth on)


## Pipeline

The basic outline of the pipeline is as follows

* *Quality Trim* to make sure we have high enough Q's at the adapter region. We will trim to a high Q (Q>30) and since we will be using it in a subsequent step we want to make sure we do not trim away the INDEX sequence so set a minimum length after trimming to retain it

* Convert from *FASTQ to FASTA*
	
* *Clip* Index Sequence, discard sequnces without an adapter (quality control) in them or shorter than the length of the shRNA piece.

* *Collapse* multiple occurances of the same sequence and get the counts

* Re-*format* the FASTQ file into a tabular format

* Join/annotate shRNA sequences.

The first 5 steps of this pipeline can be done either with programs from the FASTX tool kit. The last step will require some programming, but should be doable in R, python or perl and should be simple in any programming langage with decent I/O and string capapilities. 

The input for this pipeline is at:

```
	$PATH_TO_INPUT/$FILENAME
```

and the output you should get is:

```
	$PATH_TO_OUTPUT
```



## Code

```bash
gzcat DDR_4.fastq.gz \
	| $FASTX/fastq_quality_trimmer -t 30 -l 28 -Q33 -v \
	| $FASTX/fastq_to_fasta -Q33 -v \
	| $FASTX/fastx_clipper -a TACATC -c -l 22 \
	| $FASTX/fastx_collapser -v \
	| $FASTX/fasta_formatter -t \
	> output1.tbl
```

## References

[^Zuber2011]: Toolkit for evaluating genes required for proliferation and survival using tetracycline-regulated RNAi. Nat Biotechnol. 2011 Jan; **29**(1): 79–83. [PMC3394154](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3394154/)

