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


## Description of Pipeline

The basic outline of the pipeline is as follows

* *Quality Trim* to make sure we have high enough Q's at the adapter region. We will trim to a high Q (Q>30) and since we will be using it in a subsequent step we want to make sure we do not trim away the INDEX sequence so set a minimum length after trimming to retain it.

* Convert from *FASTQ to FASTA*
	
* *Clip* Index Sequence, discard sequnces without an adapter (quality control) in them or shorter than the length of the shRNA piece.

* *Collapse* multiple occurances of the same sequence and get the counts

* Re-*format* the FASTQ file into a tabular format

* Join/annotate shRNA sequences.

The first 5 steps of this pipeline can be done either with programs from the FASTX tool kit or with a custom script/program. The last step will require some programming, but should be doable in R, python or perl and should be simple in any programming langage with decent I/O and string capapilities. 

The input for this pipeline is at:

```
	$ROOT/Intro2NextGen/1_Sequencing/data/shRNA_Experiment1.fastq.gz
```

and the output you should get is:

```
	$ROOT/Intro2NextGen/1_Sequencing/data/exercise_1_1.out  
```

where `$ROOT` is either you `$HOME` directory or wherever you installed the Intro2NextGen package.


You can find a manual for all the FASTX commands either oneline at:

* http://hannonlab.cshl.edu/fastx_toolkit/commandline.html

or there is a local copy in the course repository at

* /Intro2NextGen/man/FASTX/fastxMan.html

## Option 1

If you are not yet comfortable with the UNIX command line and programming then I suggest skipping to the next section: __Pipeline Walkthrough__. In that next section I will step through building this pipeline from the FASTX toolkit programs. 

However, for people already comfortable with the command line and programming I strongly suggest trying to figure out how to code the pipeline from the description above. Find where the FASTX tool kit is installed in your system with
```
	which fastq_to_fasta
```

if you see nothing then it is not on your path and you will need to find it and add it to your path. 

Then browse the commands available and there features by either `ls`-ing that directory and running the commands with the `-h` option: 
```
	fastq_to_fasta -h
```

or use the manual pages linked above and try to build the pipeline. Although you can get something built in less than steps you should use **5** different commands from the toolkit. 

If you run into a snag then just move to the next section. If you do complete the pipeine skip to __Final Steps__.

## Pipeline walkthrough

### Preamble

Although it is usually a really bad idea to do pipelines as command line one liners for pedagoical reasons we will do that here. The input file is small enough that repated re-running should not be an issue. However if you feel more comfortable encapsulating the commands in a script then please do so. But make sure if you are going to go the one liner route you understand how to use the command history; in particular how to repeat previous commands. Make sure either CTRL-P or uparrow work as expected.

First make sure the FASTX toolkit is on your path. You can do this typing:
```
	fastq_to_fasta -h
```

you should see something like
```

usage: fastq_to_fasta [-h] [-r] [-n] [-v] [-z] [-i INFILE] [-o OUTFILE]
Part of FASTX Toolkit 0.0.13.2 by A. Gordon (gordon@cshl.edu)

   [-h]         = This helpful help screen.
   [-r]         = Rename sequence identifiers to numbers.
   [-n]         = keep sequences with unknown (N) nucleotides.
                  Default is to discard such sequences.
```

... (your version number may be different)

If not or you get an error ask for help. Now check that you can re-run this command using one of the history re-run command methods: 

* Up-arrow

* CTRL-p

In fact re-run that command and then reselect it and edit it to run:
```
	fastx_renamer -h
```

if you new to unix and are having trouble with this ask for help. 

### Intermediates: pipes or files

The walk through is going to be using UNIX pipes to chain together the various commands. If this is too complex or confusing or you want to capture the intermediates you can use I/O redirection
```
COMMAND <INPUT >OUTPUT
```
	
or if you look at the help screen you will see that most the FASTX commands allow you to specific the input and output file with:
```
   [-i INFILE]  = FASTA/Q input file. default is STDIN.
   [-o OUTFILE] = FASTA/Q output file. default is STDOUT.
```

ie, they default to standard input and standard output but you can explicitly set the input and output files or just one or the other. 

### Step 1: find input, create a work space

First step is to find the input file: `shRNA_Experiment1.fastq.gz`. If the course files were installed in your home directory then it should be here:
```
	$HOME/Intro2NextGen/1_Sequencing/data/shRNA_Experiment1.fastq.gz
```

verify that is is there by doing the following:
```
file $HOME/Intro2NextGen/1_Sequencing/data/shRNA_Experiment1.fastq.gz	
```

and you should see something like
```
.../Intro2NextGen/1_Sequencing/data/shRNA_Experiment1.fastq.gz: gzip compressed data, from Unix, last modified: Thu Oct 15 18:09:05 2015, max compression
```


## Code

```bash
gzcat shRNA_Experiment1.fastq.gz \
	| $FASTX/fastq_quality_trimmer -t 30 -l 28 -Q33 -v \
	| $FASTX/fastq_to_fasta -Q33 -v \
	| $FASTX/fastx_clipper -a TACATC -c -l 22 \
	| $FASTX/fastx_collapser -v \
	| $FASTX/fasta_formatter -t \
	> exercise_1_1.out
```

## References

[^Zuber2011]: Toolkit for evaluating genes required for proliferation and survival using tetracycline-regulated RNAi. Nat Biotechnol. 2011 Jan; **29**(1): 79–83. [PMC3394154](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC3394154/)

