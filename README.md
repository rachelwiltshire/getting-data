# getting-data
Obtaining data sounds simple but rarely is (in my experience) so I've created a repository to remind me of the processes as this is something I don't do very often since I create my own data.

# NCBI-SRA archive
NCBI stores raw DNA sequences in .sra format, which need to be converted into fastq for whole genome processing pipelines. I followed the SRA Toolkit documentation but only managed to create files that were binary. Having no clue what this was about I went in search of someone who had also experienced this and found a GREAT article by the Edwards Lab at SDSU ***https://edwards.sdsu.edu/research/fastq-dump/*** that explains why the NCBI documentation is so bad and how to get the files you need. Thank you **https://github.com/linsalrob/EdwardsLab**!

## Required parameters
- **Splitting reads**

  *--split-spot* (splits spots into individual reads)
  
  *--split-files* (dump each read into a separate file whose suffix corresponds to the read number)

- **Readids**
  
  *--readids* (append read ID after spot ID  as 'accession.spot.readid' on defline -> ID.1 = FWD read and ID.2 REV read)

- **Technical sequences**
  
  *--skip-technical* (dump only biological reads)

- **Clipping**
  
  *--clip* (removes SRA tags used in the whole genome amplification)

## Optional parameters
- **Sequence data formatting**
  
  *--dumpbase* (ensures that output is A, T, C and G instead of color space (used for SOLiD))

- **Output to a specific directory**
  
  *--outdir* <path>
  
- **Compression**

  *--gzip*
  
## Example
  
  *./fastq-dump* ***< SRRxxxxxx >*** *--outdir* ***< ~/PATH >*** *--gzip* *--skip-technical* *--readids* *--dumpbase* *--split-files* *--clip*
