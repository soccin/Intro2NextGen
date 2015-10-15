---
title: 'Browsers/Annotation: Exercises'
author: "Nicholas D. Socci"
date: "18 Oct 2015"
output: 
  html_document:
          toc: true
          toc_depth: 1
          number_sections: true
---

# Materials to get

ChIP BAM for hg19_test
RNAseq BAM for hg19_test
TCGA files (CGH, VCF)??

DEBUG BAMS (bams with specific)

SIGNAL BAMS (bams with the following signals: fusion, MUTATION, PEAKS)

# Setup

* Make sure IGV is installed and runable. Can get a copy here
[www.broadinstitute.org/igv](https://www.broadinstitute.org/igv/ "IGV")

* Get sample BAM files from repository or website [Berlin2015](http://qbio.mskcc.org/public/SocciN/Berlin2015/)


# Load files

* Load the sample bam file [XXX]. Try loading both as a file and also as a URL ([URL])

# Browse Regions

* Browser to various regions in the genome. Note for the sample BAM files there is only data on chr21, chr22. Browse both by entering coordinates in the region box and also browse by Gene Names. To get the name of genes on chr21 and chr22 use the UCSC browser. 

* Note to see reads in BAM file you need to zoom in to a certain point depending on how much memory you have and the size of the pileup.

* Try moving around with the mouse. Zoom in and zoom out. 

* _send them to specific locations_

## View Controls
* _have them play with settings; right menu_, color, sort, pairing, ...
  (IMAGES)

## Hover info
* explan pop up info feature
(IMAGES)

# Saving Images

* Image saving exercise

# Sessions

* Session saving exercise

# IGV as BAM file debugger

_TODO_ Make BAM with defective events. 

* Go to the following regions and try to identify what is wrong with the reads in this particular BAM file. You may need to adjust view settings

	* Examples:
		* STRAND
		* DUPLICATES
		* MAPQ
		* ERRORS
		* BASE QUALITY
	
* Go to the following region and try to identify what genomic event has occured there.

	* Examples:
		* FUSION/STRUCTURAL (hint split view)
		* Mutation
		* Peaks
		
* Remember to save images of anything interesting or noteworthy and also save a session or two of the state of IGV that helps diagnose the problem. 


# _Advanced_ IGV Batch Files

Given a BED file and a BAM file create IGV batch file(s) that will go to each region in the BAM file and save a snapshot of that region. 

You can either do this using bedtools or by writing a script. 

