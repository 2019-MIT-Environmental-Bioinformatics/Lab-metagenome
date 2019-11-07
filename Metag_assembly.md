# Metagenomic assembly and binning

In class today, we are going to use the assembler **megahit**.  MEGAHIT is an ultra-fast and memory-efficient NGS assembler. It is optimized for metagenomes, but also works well on generic single genome assembly (small or mammalian size) and single-cell assembly.

https://github.com/voutcn/megahit
https://academic.oup.com/bioinformatics/article/31/10/1674/177884

### Install megahit
Create a new conda environment called *assembly*, activate it and install ```megahit```

```conda create --name assembly```
```conda activate assembly```
```conda install -c bioconda megahit```

## Quality Assessment  (meta)Genome Assemblies
To evaluate the metagenomic assembly, we will use **quast**

https://academic.oup.com/bioinformatics/article/29/8/1072/228832
http://quast.bioinf.spbau.ru/manual.html

### Install quast
Deactivate the previous environment and create a new calles *quast* you created and install *quast*

```conda install -c conda-forge -c bioconda quast```


## Assembly metagenomes from hot springs
We are going to use the datasets from Saxena et al., 2017
https://www.frontiersin.org/articles/10.3389/fmicb.2016.02123/full

You will find the trimmed paired-end datasets in 

```/vortexfs1/omics/env-bio/collaboration/common-materials/metag-seqs```

The sequences starting with the SRR* are the individual metagenomic datasets. For your convenience, I also created the concatenated files (*prings_paired_1.fastq.gz* and *prings_paired_2.fastq.gz*), because today, we will do a **co-assembly** (you will find out later why)

_______________________________________________________________________________
Go to your *Lab-metagenome* folder, start an srun using **scavenger**. Request **2h**, **36** cpus, and **40gb** of memory

``` srun -p scavenger --time=12:00:00 --ntasks-per-node=36 --mem=40gb --pty bash```

And start the assembly using megahit:
```conda activate assembly```
```megahit -1``` *path to reads _1* ```-2```  *path to reads _2* ```-t 36 --k-min 61 --k-max 121 --min-contig-len 2000 -o Megahit_springs```

When you the assembly is completed, go into the folder Megahit_springs. The assembled contigs are called "final.contigs.fa".

In order to evaluate the assembly, we will use *quast*
```conda deactivate```
```conda activate quast```
```python /vortexfs1/home/mpachiadaki/miniconda3/envs/quast/bin/quast -o quast_out -t 36 final.contigs.fa```

Quast produces the outcome is several formats. Go inside the *quast_out* folder and open *report.txt"

### N50
Given a set of contigs, the N50 is defined as the sequence length of the shortest contig at 50% of the total genome length.

https://en.wikipedia.org/wiki/N50,_L50,_and_related_statistics
http://www.acgt.me/blog/2013/7/8/why-is-n50-used-as-an-assembly-metric.html

### L50
Given a set of contigs, the L50 count is defined as the smallest number of contigs whose length sum makes up half of genome size.

http://www.acgt.me/blog/2015/6/11/l50-vs-n50-thats-another-fine-mess-that-bioinformatics-got-us-into
