# DNA mappers

## Preliminaries

### Check path to mapping programs

Most of the modern mappers are pretty easy to use; at least in default mode. First check that 

* `bwa`

* `bowtie2`

* `STAR`

are on your path. Just time the name of the command and you should get a nice help screen. For example:
```
$ bwa

Program: bwa (alignment via Burrows-Wheeler transformation)
Version: 0.7.12-r1039
Contact: Heng Li <lh3@sanger.ac.uk>

Usage:   bwa <command> [options]

Command: index         index sequences in the FASTA format
         mem           BWA-MEM algorithm
...
```

The one exception is `STAR`. Note only does it have a pretty useless default output
```
$ STAR

EXITING because of fatal input ERROR: could not open readFilesIn=Read1

Oct 18 21:38:26 ...... FATAL ERROR, exiting
```

it does not even have a `-h`, `--help`, `-m` or any of the typical options most unix commands have to get a help or man screen. There is actually on one quasi-useful one: 
```
$ STAR --version 
STAR_2.4.2a
```

and it leaves a mess of files. If you want clean up do
```
	rm -r Aligned.out.sam Log.out Log.progress.out _STARtmp/
```

But so long as you do not see:
```
$ STAR
-bash: STAR: command not found
```

they are on your path and executable.

Another way to tell if a command is on your path is to type:
```
	which CMD
```

and if it is on your path you will see the full directory path to the CMD. 

### Get test genome path

All of the data for this exercise used a reduced version of the human genome (UCSC build hg19) which contained just Chromosomes chr21 & chr22. This was the reduce the size of the files, the amount of memory needed and to reduce run time. At the time I write this I am not sure what the full path to the share drive that contains this genome but by the time your reads this I should know
```
	$GENOMEDIR_ROOT
```

Where ever that turns out to be I would define a variable
```
	$GENOMEDIR=$GENOMEDIR_ROOT/Compgen2015/Genomes/Homo_sapiens/UCSC/hg19_test2
```

which is the path to all the hg19_test2 genome files. This should have the folowing directories:

path | contents
-----|---------
Annotation/Gene|	Reduced gene GTF (chr21,22)
Sequence/BWAIndex|	Index for BWA (v7)
Sequence/Bowtie2Index|	Index for Bowtie2
Sequence/STARIndex|	Index for STAR (rna aligner)
Sequence/WholeGenomeFasta| The genome with associated index files

Make sure you can find this file. In particular make sure
```
	head $GENOMEDIR/Sequence/WholeGenomeFasta/genome.fa.fai
```

>chr21	48129895	7	50	51
>chr22	51304566	49092507	50	51


## Mapping synthetic DNA data.

To generate these datasets I used the program `wgsim` from the author of BWA. It is not installed but is prehaps the easiest program of all the ones you are looking at to install. At the end of this exercise I have the location for it for those you want to play with generating their own data. 



## wgsim

You can find a copy of wgsim here:
```
https://github.com/lh3/wgsim/archive/master.zip
```

Read the README and follow the Compliation instructions. Then read the rest of the README and the help screen for the program. Is is pretty simple but fairly quick.


