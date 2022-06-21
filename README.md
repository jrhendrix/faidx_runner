# faidx_runner

Wrapper for the Samtools faidx tool to improve ease of use. Faidx extracts a sequence from a fasta file (faa, fna, etc.) using the sequence ID; however, because the command takes a list of sequence IDs, the command becomes unwieldy when extracting a large number of sequences. This tool allows users extract desired sequences using sequence IDs or gene names and offers options for saving sets of sequences into a single file.

## Requirements
* Samtools v1.5
* python


## Usage
There are currently three modes for running faidx_runner

### Query by ID list
Coming soon

### Query by ID table
Each sequence element in the fasta file should have its own sequence ID.
```
python run_faidx.py by_id -f file.fasta -i lists.tsv
```
The .tsv file should have the following format:
group_1 ID_1  ID_2  ... ID_n
group_2 ID_A  ID_B  ... ID_n

Where `group` is a unique identifier for the set of identifiers on that line. Ensure that after the group name and between each ID is a tab.
Using this file format, one .fasta file will be produced for each group and that group file will contain all of the sequences for elements specified on that line. For single sequence files, give each sequence its own line and group name or see query by ID list.

### Query by gene name


### Query by Roary output



TODO:
Search multiple files
Input by_list
