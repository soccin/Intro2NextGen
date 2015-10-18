# DNA mappers

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
