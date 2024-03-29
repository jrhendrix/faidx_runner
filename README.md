# faidx_runner

runFaidx is a wrapper for the Samtools faidx tool to improve ease of use. 

Samtool faidx extracts a sequence from a fasta file (faa, fna, etc.) using the sequence ID; however, because the command takes a list of sequence IDs, the command becomes unwieldy when extracting a large number of sequences. Further, a new command is required for each grouping of sequence extracts.

This tool allows users to extract any number of sequences and group the output into unique files. Sequences can be specified by sequence IDs or gene names in the command line or listed in an input file. 


## Requirements
* Samtools v1.5
* python


## Usage
runFaidx.py can extract sequences using four modes.

runFaidx.py is currently unable to search multiple files for the sequence IDs. If the sequences are present across multiple files, first join the files into one using the `cat` command.
```cat *faa > all.faa```


### Query by ID table
Each sequence element in the fasta file should have its own sequence ID.
```
python runFaidx.py table -f file.fasta -i lists.tsv
```
The text file is tab deliminated and should have the following format:
```
group_1 ID_1  ID_2  ... ID_n
group_2 ID_A  ID_B  ... ID_n
```

Where `group` is a unique identifier used to label the output file that contains the extracted sequences. The output file of sequences will be  for the set of identifiers on that line. Ensure that after the group name and between each ID is a tab. For example, the output file group_1.fasta will contain the sequences for ID_1, ID_2, ... ID_n. There is no limit to the number of rows or columns within the text file.

### Query by gene name
As input, this mode requires a Prokka generated .tsv file that describes the genes to parse. 
```
python runFaidx.py gene -g genename -f file.fasta -i prokka.tsv
```
runFaidx.py gene offers three modes for finding the genes of interest that can be specified using the `-m` flag. `exact` requires an exact match. `gene` mode will match only the prefix of the gene. `close` will grab any gene that contains the given string.

Let a genome contain the genes esxA_1, esxA_2, and esxH. Running runFaidx.py gene with the parameters `-g esxA_1 -m exact` will extract the sequence for esxA_1. Using the parameters `-g esxA -m gene` will extract the sequences for esxA_1 and esxA_2. Running with the parameters `-g esx -m close` will export the sequences for all three genes.


### Query using a file of IDs
As input, this mode takes a text file where each line contains a single gene ID. When using this mode, do not include a header line.
```
python runFaidx.py file -f file.fasta -i file.txt
```

### Query by Roary output
Roary is a pan-genome analysis tool that outputs a file called gene_presence_absence.csv where the first column contains a unique identifier and the presence of that identified element in a set of genomes is indicated in subsequent columns. 

runFaidx.py roary can accepts the original Roary file or a copy containing a subset of rows, as long as the columns are in tact and the file is comma deliminated. 

```
python runFaidx.py roary -f file.fasta -i gene_presence_absence.csv
```

### Pangenome mode
runFaidx.py pangenome mode will extract one instance of each core and accessory gene sequence present in a collection of genomes. As input, this mode requires the Roary output file gene_presence_absence.csv or a copy of this file containing a subset of the rows. A subsetted copy must have the same columns and be comma deliminated. 

```
python runFaidx.py pangenome -f file.fasta -l gene_presence_absence.csv
```
runFaidx.py pangenome will output two files: `access.fasta` and `core.fasta`. `core.fasta` will contain a representative sequence for each gene that is found in all samples in the input file while `access.fasta` will contain a representative of each gene missing from at least one of the samples. The representative sequence is determined by the sequence ID of the first sample to contain a sequence for that set. For example, consider columns1 and 15-17 of an example roary output:

```
Gene   sample_1  sample_2  sample_3
gene1   g1_ID1    g1_ID2    g1_ID3
gene2             g2_ID2    g2_ID3
```
In this case, `core.fasta` will contain the sequence for g1_ID1 and `access.fasta` will contain the sequence for g2_ID2.

### Runtime specifics
runFaidx.py will export the sequences to a directory called `subset_faidx`. To change the name of this directory use the `-o` flag. By default, runFaidx.py will create this directory in the current working directory. To change the path, use the `-p ` flag.
Each exported file will have the prefix `subset` but this can be changed using the `-s` flag.
