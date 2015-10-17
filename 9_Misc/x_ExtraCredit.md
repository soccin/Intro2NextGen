# C.elegans test genome

- Use samtools to extract two chromosomes to make a reduced genome

	- Be neat, create a dedicate folder to hold local genomes
	
	- Suggestion from iGenomes: `$HOME/Genomes/$SPECIES/$SOURCE/$BUILD`

- Index it and build the sequence dictionary (*hint* PICARD)

- Build indexes for bwa and STAR

- Create some test data for mapping:

	- find, download, and install the program `wgsim`
		
		- *hint* get zipfile version
	
	- read manual and generate 100,000 reads 35mers with default settings
	
- Map with bwa

- Visualize with IGV