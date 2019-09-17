# getting-data
Obtaining data sounds simple but rarely is (in my experience) so I've created a repository to remind me of the processes as this is something I don't do very often since I usually create my own data.

# NCBI-SRA archive
NCBI stores raw DNA sequences in .sra format, which need to be converted into fastq for whole genome processing pipelines. I followed the SRA Toolkit documentation but only managed to create files that were binary. Having no clue what this was about I went in search of someone who had also experienced this and found a GREAT article by the Edwards Lab at SDSU ***https://edwards.sdsu.edu/research/fastq-dump/*** that explains why the NCBI documentation is so bad and how to get the files you need. Thank you **https://github.com/linsalrob/EdwardsLab**!

## Required parameters
- **Splitting reads**

  *--split-spot* (splits spots into individual L and R reads, and puts them in the **same** singular file)
  
  *--split-files* (separates read into L and R ends, and puts FWD and REV reads in **two** separate files)
  
  *--split-3* (separates read into L and R ends but if a L has not got a matching R, or vice versa, they will be put into a **single** file)

- **Readids**
  
  *-I | --readids* (appends read ID after spot ID  as 'accession.spot.readid' on defline -> **ID.1** = FWD read and **ID.2** REV read)
  
     **NB.** *--readids* breaks BWA in downstream applications so this parameter may need to be omitted if BWA is in your pipeline. See *Original format* for more details.

- **Technical sequences**
  
  *--skip-technical* (dumps only biological reads)

- **Clipping**
  
  *-W | --clip* (applies L and R clips to remove SRA tags used in the whole genome amplification)
  
- **Read filtering**

  *--read-filter* (filters out reads recorded as N). Options are: **pass | reject | criteria | redacted**
  
- **Original format**
  
  *-F | --origfmt* (retains formatting of original definition line)
  
     SRA archive rewrites definition line in the seq. to include SRA ID and length if this parameter is omitted - this is a problem for BWA alignment as it doesn't recognize the new format and errors out >> i.e. ***[mem_sam_pe] paired reads have different names: "SRR849970.3.1", "SRR849970.3.2"***)

## Optional parameters
- **Sequence data formatting**
  
  *-B | --dumpbase* (ensures that output is A, T, C and G instead of color space (used for SOLiD))

- **Output to a specific directory**
  
  *-O | --outdir* <path>
  
- **Compression**

  *--gzip*
  
## Example
  
  *cd ~/PATH/sratoolkit/bin*
  
  *./fastq-dump* ***< SRRxxxxxx >*** *--outdir* ***< ~/PATH >*** *--gzip* *--skip-technical* *--readids* *--dumpbase* *--split-files* *--clip* *--origfmt* *--read-filter* ***pass***
