# Task 3 - De-novo Assembly
*de novo* is a Latin expression meaning "from the beginning," "afresh," "anew," "beginning again.". When we perform a de-novo assembly we try to reconstruct a genome or part of the genome from our reads without making any prior assumptions (in contrast to mapping where we compare our reads to what we think is a close reference sequence).

The advantage is that de-novo assembly can reveal completely novel results, e.g. identifying horizontal gene transfer events. The disadvantage is that it is difficult to get a good assembly from short reads, and it can be prone to misleading results due to contamination and mis-assembly.

## Generate an Assembly
We will be using an assembler called [SPAdes](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3342519/). It generally performs pretty well with a variety of genomes. It can also incorporate longer reads produced from PacBio sequencers that we will use later in our adventure.

One big advantage is that it is not just a pure assembler - it is a suite of programs that prepare the reads you have, assembles them and then refines the assembly.

SPAdes runs the modules that are required for a particular dataset and it produces the assembly with a minimum of preparation and parameter selection - making it very straightforward to produce a decent assembly. As with everything in bioinformatics you should try to assess the results critically and understand the implications for further analysis.

Let's start the assembler because it takes about 10 minutes to run (this might be a nice time to get a :coffee: or to stretch your legs :walking:). You will see a lot of output on the screen.
```bash
spades.py --careful -o spades_assembly -1 unmapped_r1.fastq -2 unmapped_r2.fastq
```

We are running the SPAdes assembly pipeline and specifying the "--careful" option to run a mismatch correction algorithm to reduce the number of errors; put the output in the "-o spades_assembly" directory and the read libraries (-2 and -2) to assemble. Just because SPAdes does a lot for you does not mean you should not try to understand the process...

A pretty old overview of how SPAdes differs from 'velvet', a very old assembly program, can be found [here](http://thegenomefactory.blogspot.co.uk/2013/08/how-spades-differs-from-velvet.html). Nonetheless, it outlines the overall process quite nicely:

1. Read error correction based on k-mer frequencies using ​BayesHammer.
2. De Bruijn graph assembly at ​multiple ​k-mer sizes, not just a single fixed one.
3. Merging of different k-mer assemblies (good for varying coverage).
4. Scaffolding of contigs from paired end/mate pair reads.
5. Repeat resolution from paired end/mate pair data using rectangle graphs.
6. Contig error correction based on aligning the original reads with ​BWA​ back to contigs.


Try to understand the steps in the context of the whole picture. Can you explain why error correction of reads becomes more important as k-mer length increases?

When the assembly is complete, change to the "spades_assembly"​ directory and have a look :eyes: at the output.

Let's take a look at some of the more important content.
### params.txt
This contains a summary of the parameters used for assembly - it is useful so you can repeat the exact analysis performed, or can remember you setting when you want to publish the genome.

### contigs.fasta
This contains the final results of the assembly in fasta format.

### scaffolds.fasta
This contains the final results after scaffolding (which means using paired end information to join contigs together with gaps). In this case the files are identical, probably because the sum of the lengths of our paired reads is not much smaller than our insert size (there are very few large gaps between reads).

### assembly_graph.fastg
Contains SPAdes assembly graph in FASTG format - this is a slightly different format that contains more information than fasta - for example it can contain alternative alleles in diploid assemblies. We don't need it here, but see ​[here](http://fastg.sourceforge.net/FASTG_Spec_v1.00.pdf) :file: if you might be working with diploid organisms. You can use the [Bandage](http://rrwick.github.io/Bandage) program to view this file.

# Go to [Task 4](https://github.com/guyleonard/genomics_adventure/blob/release/chapter_3/task_4.md)
