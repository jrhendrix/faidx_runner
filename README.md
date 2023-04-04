# faidx_runner

faidx_runner is a wrapper for the Samtools faidx tool to improve ease of use. 

Faidx extracts a sequence from a fasta file (faa, fna, etc.) using the sequence ID; however, because the command takes a list of sequence IDs, the command becomes unwieldy when extracting a large number of sequences. Further, a new command is required for each grouping of ssequence extracts.

This tool allows users to extract any number of sequences and group the output into unique files. Sequence can be specified by sequence IDs or gene names in the command line or listed in an input file. 


## Requirements
* Samtools v1.5
* python


## Usage
faidx_runner can extract sequences using three modes.

### Query by ID table
Each sequence element in the fasta file should have its own sequence ID.
```
python run_faidx.py table -f file.fasta -i lists.tsv
```
The text file is tab deliminated and should have the following format:
```
group_1 ID_1  ID_2  ... ID_n
group_2 ID_A  ID_B  ... ID_n
```

Where `group` is a unique identifier used to label the output file that contains the extracted sequences.  the group of sequences in the output for the group of sequence IDs. The output file of sequences will be  for the set of identifiers on that line. Ensure that after the group name and between each ID is a tab. For example, the output file group_1.fasta will contain the sequences for ID_1, ID_2, ... ID_n. There is no limit to the number of rows or columns within the text file. 


### Query by ID list
Coming soon

### Query by gene name


### Query by Roary output



TODO:
Search multiple files
Input by_list
