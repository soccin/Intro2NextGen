## Map RNAseq data with star

### Build STAR index without GTF

- Build an index on HG19_test without GTF (or maybe reduced GTF)

- Map sequence 

	- need to generate transcriptome sequence

- Get junctions and rebuild index

- Remap

- Convert both SAMs to BAMs and save for Lab 4 (IGV)

### Map Proton Data

- Map proton data with just STAR

- Check mapping stats % mapped

### Extra Credit

- Build a two stage RNAseq mapper for Proton Data

	- STAR
	
	- unaligned reads to BWA (or bowtie)
	
	- merge
	

	
	