# SAM file post-processing

Nearly all mappers will output a very plain, un-sorted, un-compressed, un-processed SAM file. And most of the various down stream analysis you may want to do: mutation calling, peak calling, gene expression require a more processed output. The requirements are different for the different types of analysis. For mutation calling there is a rather complex serious of steps that need to be done to prepare the mapping files for the mutation calling algoritms. For gene expression counting some programs (like `htseq-count`) require nothing although it is still a good idea to compress the plain text SAM files to BAM's, which are compressed. 

For processing post processing SAM files there are two tools widely in use:

* PICARD (http://broadinstitute.github.io/picard/)

* SAMTOOLS (http://www.htslib.org)
Note the older version of samtools still ranks first in google (http://samtools.sourceforge.net). For the course we have loaded the newer version and I suggest sticking with that one. It has a lot on newer features. 

> *General tip*; you should balance sticking with a fixed copy of software versus continually updated to the latest. It is not an easy balance to find. And at some point in research you need to fix/freeze the pipeline in the same way there are data freezes. 

These two packages have a lot of overlap. One big different though is that PICARD does not have any way to stream files while SAMTOOLS does. I am a huge (HUGE) fan of streaming (pipeing) and it really bothers me that PICARD will not let me use that feature of UNIX. However, I **STRONGLY** recommend using PICARD whenever there is a choice. You will be much happier although you will have lots of intermediate files. 

The one exception; indexing. If you need a quick way to index `samtools index` is the way to go. 

## Simple PICARD post processing

As I said the exact post processing steps you need to do will depend on the down stream analysis being done; however there is a core set of steps that nearly everything needs:

* Sorting

* Compressing (SAM->BAM)

* Indexing

* Marking Duplicates

The first 3 are almost universially needed so most mapping pipelines will simple just do them. MarkDups is not always needed but you can do it in a reversable way (ie by marking rather than removing duplicates) so you can safely us it also.

PICARD actually has a module for each of these steps; however since most of the time you are manipulating a BAM file you also want to sort and index it you can do all three of the first steps with the sort command. 

First find the PICARD JAR file. I have a copy on the course folder at:
```bash
	$ROOT/Intro2NextGen/3_Tertiary/code/picard-tools-1.140
```

I recommend setting the environment variable `PICARDJAR` for ease of use:
```bash
	PICARDJAR=$ROOT/Intro2NextGen/3_Tertiary/code/picard-tools-1.140/picard.jar
```

To test if that worked type:
```bash
	java -jar $PICARDJAR
```

You should see a nice colorful (although that depends on your terminal) help screen. You might want to send the output through more (or less) but if you are getting the colors that will be messed up. And it goes to stderr not stdout. But try the following:
```bash
	java -jar $PICARDJAR 2>&1 | less -R
```

to keep the colors and get paging to work.

What we want to do is sort some of the SAM files we generated in the previous exercise. The command to do that is SortSam. PICARD is actually the exact opposite of STAR; while STAR is one of the most unfriendly (almost hostile) of programs I have seen PICARD is super nice and useful. Type
```bash
	java -jar $PICARDJAR SortSam
```

and it will tell you how to run the SortSam command. PICARD does not use the unix convention for command options `-x XYZ`, instead it uses a different one:
```bash
	java -jar $PIACARDJAR SortSam I=Input.sam O=Output.bam SO=coordinate
```

It also decides what to do based on the extension of the output file. `.bam` generates BAM's, `.sam` generates SAM's. `SO=coordinate` means sort the reads by there position along the genome. Alternative you can sort them by the name of the read (`queryname`) which is useful if you need to process paired end reads together. 

One last option to add. You can not see it because it is a generic option use the `-H` option (PICARD does use unix style options for some things) to see it. 
```bash
	java -jar $PIACARDJAR SortSam -H
```

The options is to create an index: `CREATE_INDEX=true`

So get the SAM files we made in the previous exercise ready for use in IGV with the following command.
```bash
	java -jar $PICARDJAR SortSam \
		CREATE_INDEX=true \
		SO=coordinate \
		I=inputFile.sam \
		O=outputFile.bam 
```

where you need to pick inputFile and outputFile names accordingly. Use this oppurtunity to give your files meaningful names. 


