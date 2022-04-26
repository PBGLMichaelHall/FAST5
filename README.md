# FAST5
# Tree Directory for processing and analyzing FAST5 files from a MinION run....

![777](https://user-images.githubusercontent.com/93121277/164006307-9d4b2f23-fe42-49d2-9aa6-cc64f737291b.png)


# From FAST5 FASTQ to GRANGES object where SNPS are within a CODING region with a particular GENE


``` r

# Downstream Analysis using Basecalling with Guppy

guppy_basecaller --compress_fastq -i fast5/guppy_out/ -f FLO-FLG001 -k SQK-LSK109 --cpu_threads_per_caller 4 --num_callers 1
 
guppy_basecaller --compress_fastq -i fast5/ -s fast5/guppy_out/ --flowcell FLO-FLG001 --kit SQK-LSK109 --cpu_threads_per_caller 4 --num_callers 1  


--compress_fastq                 Compress fastq output files with gzip.
-i                               Path to input fast5 files.
-s                               Path to save fastq files.
--flowcell                       Flowcell to find a configuration for
--kit                            Kit to find a configuration for
--cpu_threads_per_caller         Number of CPU worker threads per 
--num_callers                    Number of parallel basecallers to create.



################################################################################################################################

# Clone github graphmap repository and make modules

git clone https://github.com/isovic/graphmap.git
    cd graphmap
    make modules
    make
# System file path to graphmap executable program

 ~/graphmap/bin/Linux-x64/
     
# Locate fast5 and reference genome fasta files and invoke graphmap program to align

 ../../../graphmap/bin/Linux-x64/graphmap align -r CoffeeArabica_Cara/ncbi_dataset/data/GCF_003713225.1/Coffee.fna -t 4 -d fast5/guppy_out/pass/fastq_runid_94dd8d03aa43bb3ac59d61d6b73a0ac5e939526b_0_0.fastq.gz -o map_to_ref/nanopore.graphmap.sam > map_to_ref/nanopore.graphmap.sam.log 2>&1

-r reference genonme
-t Threads
-d Directory of FastQ file from FAST5 raw data
-o output directory to file ending with .sam
> create log of run

#####################################################################################################################################

# Invoke samtools stats on sam file

samtools stats -d -@ 4 nanopore.graphmap.sam > nanopore.graphmap.sam.stats
	
# Inpect first 40 lines of header

head -n 40 nanopore.graphmap.sam.stats 


######################################################################################################################################

# Invoke samtools view and condition on samtools sort to make a sorted bam file

samtools view -@ 4 -bS nanopore.graphmap.sam | samtools sort - -@ 4 -o nanopore.graphmap.sorted.bam

########################################################################################################################################

# Create a Qualimap Environment

conda create --name Qualimap

conda activate Qualimap

conda install -c bioconda qualimap

########################################################################################################################################

# Invoke qualimap on sorted bam file an HTML report should be generated automatically

qualimap bamqc -bam nanopore.graphmap.sorted.bam -nw 5000 -nt 4 -outdir nanopore.graphmap


#######################################################################################################################################


# Index Reference Genome

bwa index Coffee.fna

# make a Directory for vcf file

mkdir vcfplots

# Invoke samtools pileup to align indexed fasta to sorted bam

 samtools mpileup -g -f CoffeeArabica_Cara/ncbi_dataset/data/GCF_003713225.1/Coffee.fna map_to_ref/nanopore.graphmap.sorted.bam | bcftools call -mv -o vcfplots/all.vcf

# Filter for SNPs type and Biallelic sites only

 bcftools view -m2 -M2 -v snps -o BIALLELIC~ONLY.vcf all.vcf 

# How many variants were called

 bcftools view BIALLELIC~ONLY.vcf | wc -l

1468


#######################################################################################################################################3

# run IGV program on Reference Genome, Sorted Bam, and VCF


~/IGV_Linux_2.11.9
bash igv.sh

Load Coffee Arabica Genome


#########################################################################################################################################


# Use Rsubread package to find genes with mapped read


setwd("/home/michael/FAST5/CoffeeMinIon/20220414_1039_MN19654_AJF976_ed35bf91/map_to_ref")
FeatureCounts<-Rsubread::featureCounts(files = "nanopore.graphmap.sorted.bam", annot.ext = "../GCF_003713225.1_Cara_1.0_genomic.gff.gz",isGTFAnnotationFile = TRUE,GTF.featureType = "gene", GTF.attrType = "ID")

annotation <- FeatureCounts$annotation
stat <- FeatureCounts$stat
counts <- FeatureCounts$counts

ASC <- cbind(annotation,counts)


threshold <- 0
ASC <- ASC[(apply(counts,1,min)) > threshold,]
print(ASC)

                             GeneID         Chr    Start      End Strand Length nanopore.graphmap.sorted.bam
gene-LOC113688632 gene-LOC113688632 NC_039898.1  1392954  1395287      +   2334                           81
gene-LOC113697586 gene-LOC113697586 NC_039899.1   884021   886305      +   2285                          118
gene-LOC113697595 gene-LOC113697595 NC_039899.1   893020   895356      +   2337                          148
gene-LOC113731239 gene-LOC113731239 NC_039901.1 14523710 14529861      -   6152                            1
gene-LOC113734903 gene-LOC113734903 NC_039902.1 26262544 26264446      +   1903                            1
gene-LOC113697766 gene-LOC113697766 NC_039910.1 22728667 22733865      +   5199                            1
gene-LOC113708575 gene-LOC113708575 NC_039914.1  2245618  2250386      -   4769                            1
gene-LOC113708187 gene-LOC113708187 NC_039914.1  7384813  7386075      -   1263                            3
gene-LOC113707907 gene-LOC113707907 NC_039914.1  7430405  7432582      -   2178                          367
gene-LOC113708510 gene-LOC113708510 NC_039914.1  7473077  7475443      -   2367                          122
gene-LOC113709599 gene-LOC113709599 NC_039915.1  6555704  6558053      -   2350                            5
gene-LOC113710584 gene-LOC113710584 NC_039915.1  6598426  6623704      -  25279                            1
gene-LOC113710465 gene-LOC113710465 NC_039915.1  6674915  6677084      -   2170                          114
gene-LOC113710464 gene-LOC113710464 NC_039915.1  6714444  6716737      -   2294                           90
gene-CoarCp011       gene-CoarCp011 NC_008535.1    16850    21025      -   4176                            1


#####################################################################################################################################


```
# Look at IGV For the first entry printed ASC object 
GeneID:             gene-LOC113688632 
Chromosome:         NC_039898.1  
Start:              1392954  
End:                1395287   
Mapped Reads:     81

![IGV7](https://user-images.githubusercontent.com/93121277/165303921-de9d4cd1-4759-49f4-90fc-8c93ceb52049.png)



```r
# We want to make a GRanges object that provides variant calls in CODING regions/locations only


setwd("/home/michael/FAST5/CoffeeMinIon/20220414_1039_MN19654_AJF976_ed35bf91")



library(vcfR)
library(VariantAnnotation)
library(GenomicFeatures)

vcf <- readVcf(file = "vcfplots/BIALLELIC~ONLY.vcf")

rd <- rowRanges(vcf)

# convert annotations to TxDb object
GFFTXB<-makeTxDbFromGFF(file="GCF_003713225.1_Cara_1.0_genomic.gff.gz")


#Locate Variants
loc <- locateVariants(rd, GFFTXB, CodingVariants())

#How many variants were called in coding locations
length(loc)
38 ranges which means 38 total variants were located wihin a coding region with a particular geneID
#Inspect the head
head(loc,10)


GRanges object with 10 ranges and 9 metadata columns:
                              seqnames    ranges strand | LOCATION  LOCSTART    LOCEND   QUERYID        TXID
                                 <Rle> <IRanges>  <Rle> | <factor> <integer> <integer> <integer> <character>
   NC_039898.1:1393106_A/G NC_039898.1   1393106      + |   coding        23        23        23         186
   NC_039898.1:1393391_T/A NC_039898.1   1393391      + |   coding       160       160        24         186
   NC_039898.1:1395076_G/A NC_039898.1   1395076      + |   coding      1134      1134        26         186
    NC_039899.1:893176_A/G NC_039899.1    893176      + |   coding        23        23       288        3946
    NC_039899.1:893660_T/C NC_039899.1    893660      + |   coding       360       360       289        3946
    NC_039899.1:894131_G/A NC_039899.1    894131      + |   coding       574       574       293        3946
    NC_039899.1:894232_C/G NC_039899.1    894232      + |   coding       675       675       294        3946
    NC_039899.1:894271_G/A NC_039899.1    894271      + |   coding       714       714       295        3946
  NC_039901.1:14526465_T/A NC_039901.1  14526465      - |   coding      1144      1144       369       20947
  NC_039901.1:14526465_T/A NC_039901.1  14526465      - |   coding      1144      1144       369       20948
                                              CDSID       GENEID       PRECEDEID        FOLLOWID
                                      <IntegerList>  <character> <CharacterList> <CharacterList>
   NC_039898.1:1393106_A/G                      308 LOC113688632                                
   NC_039898.1:1393391_T/A                      309 LOC113688632                                
   NC_039898.1:1395076_G/A                      312 LOC113688632                                
    NC_039899.1:893176_A/G                    14627 LOC113697595                                
    NC_039899.1:893660_T/C                    14628 LOC113697595                                
    NC_039899.1:894131_G/A                    14629 LOC113697595                                
    NC_039899.1:894232_C/G                    14629 LOC113697595                                
    NC_039899.1:894271_G/A                    14629 LOC113697595                                
  NC_039901.1:14526465_T/A 100255,100256,100257,... LOC113731239                                
  NC_039901.1:14526465_T/A 100255,100256,100257,... LOC113731239     


#######################################################################################################################################

```

# So the head shows there are three variants called on the LOC113688632 gene



# All Variants in G Ranges Object in ALL locations

allvar <- locateVariants(rd, GFFTXB, AllVariants())

#How many total variants called

head(allvar,10)

GRanges object with 10 ranges and 9 metadata columns:
                              seqnames    ranges strand |   LOCATION  LOCSTART    LOCEND   QUERYID        TXID         CDSID      GENEID                                  PRECEDEID                                   FOLLOWID
                                 <Rle> <IRanges>  <Rle> |   <factor> <integer> <integer> <integer> <character> <IntegerList> <character>                            <CharacterList>                            <CharacterList>
  NC_039919.1:30804544_G/A NC_039919.1  30804544      * | intergenic      <NA>      <NA>         1        <NA>                      <NA> LOC113717563,LOC113717635,LOC113717636,... LOC113717566,LOC113717567,LOC113717568,...
  NC_039919.1:30804551_A/T NC_039919.1  30804551      * | intergenic      <NA>      <NA>         2        <NA>                      <NA> LOC113717563,LOC113717635,LOC113717636,... LOC113717566,LOC113717567,LOC113717568,...
  NC_039919.1:30804586_T/A NC_039919.1  30804586      * | intergenic      <NA>      <NA>         3        <NA>                      <NA> LOC113717563,LOC113717635,LOC113717636,... LOC113717566,LOC113717567,LOC113717568,...
  NC_039919.1:30804649_G/A NC_039919.1  30804649      * | intergenic      <NA>      <NA>         4        <NA>                      <NA> LOC113717563,LOC113717635,LOC113717636,... LOC113717566,LOC113717567,LOC113717568,...
  NC_039919.1:30804650_G/T NC_039919.1  30804650      * | intergenic      <NA>      <NA>         5        <NA>                      <NA> LOC113717563,LOC113717635,LOC113717636,... LOC113717566,LOC113717567,LOC113717568,...
  NC_039919.1:30804651_G/T NC_039919.1  30804651      * | intergenic      <NA>      <NA>         6        <NA>                      <NA> LOC113717563,LOC113717635,LOC113717636,... LOC113717566,LOC113717567,LOC113717568,...
  NC_039919.1:30804657_C/A NC_039919.1  30804657      * | intergenic      <NA>      <NA>         7        <NA>                      <NA> LOC113717563,LOC113717635,LOC113717636,... LOC113717566,LOC113717567,LOC113717568,...
  NC_039919.1:30804702_A/T NC_039919.1  30804702      * | intergenic      <NA>      <NA>         8        <NA>                      <NA> LOC113717563,LOC113717635,LOC113717636,... LOC113717566,LOC113717567,LOC113717568,...
  NC_039919.1:30804721_G/T NC_039919.1  30804721      * | intergenic      <NA>      <NA>         9        <NA>                      <NA> LOC113717563,LOC113717635,LOC113717636,... LOC113717566,LOC113717567,LOC113717568,...
  NC_039919.1:30804727_G/T NC_039919.1  30804727      * | intergenic      <NA>      <NA>        10        <NA>                      <NA> LOC113717563,LOC113717635,LOC113717636,... LOC113717566,LOC113717567,LOC113717568,...
  
```









# First we want to reorgainze the data structure so the RScript from https://github.com/roblanf/minion_qc can plot,interpret and analyze the MinION reads. Create a summary directory and place final_summary.txt and sequencing_summary.txt. Copy the MinIONQC.R Script into main directory and run the following command line. It will produce a standard output if successful. 


![SO](https://user-images.githubusercontent.com/93121277/164192115-6b8a1a95-7a78-41d3-91b1-a40f8a89bcbd.png)


Channel Summary 

![channel_summary](https://user-images.githubusercontent.com/93121277/164192571-f603793d-9c4d-417b-b52d-94ea9a3ecf11.png)

Flowcell Overview
# There are 16 X 32 = (512) Flowcells available, however in the plot 5 - 30 are missing.....

![flowcell_overview](https://user-images.githubusercontent.com/93121277/164192623-17d84cc5-5b28-4b8b-9895-55ffb70f86f9.png)

GB Per Channel Overview
# Again there is missing Channels notice all the white space

![gb_per_channel_overview](https://user-images.githubusercontent.com/93121277/164192665-6681a9b5-e627-4e4e-b279-bb50e9ac2abf.png)

Length By Hour

![length_by_hour](https://user-images.githubusercontent.com/93121277/164192699-b965045b-d31d-4fa8-a4d7-21af69475895.png)

Length Histogram

![length_histogram](https://user-images.githubusercontent.com/93121277/164192732-73f299a8-55e8-4158-a714-869848fc6e40.png)

Length VS. Quality

![length_vs_q](https://user-images.githubusercontent.com/93121277/164192770-462eb9b4-7608-4adf-84a4-25a1e5e6ccf9.png)

Quality by Hour

![q_by_hour](https://user-images.githubusercontent.com/93121277/164192811-cff611ab-f357-4be9-992c-1ebcb117263a.png)

Quality Histogram 

![q_histogram](https://user-images.githubusercontent.com/93121277/164192833-d4cb1298-4c1d-4ebb-847f-7fd0072d35ae.png)

Reads Per Hour

![reads_per_hour](https://user-images.githubusercontent.com/93121277/164192882-6c7e9354-6864-49d3-9e04-d1c8fa81dabb.png)

Yield By Length

![yield_by_length](https://user-images.githubusercontent.com/93121277/164192921-82f8ca45-cca7-4019-a5c9-a289407b23ed.png)

Yield Over Time

![yield_over_time](https://user-images.githubusercontent.com/93121277/164192967-1938c6c2-9b08-416d-b6a6-7897d7ad8af5.png)

# FASTQ Quality Summary for the FAILED Minion Run


```r
setwd("/home/michael/FAST5/CoffeeMinIon/20220414_1039_MN19654_AJF976_ed35bf91/fast5/guppy_out")
library(fastqcr)
fastqc_install()
fastqc(fq.dir = "/home/michael/FAST5/CoffeeMinIon/20220414_1039_MN19654_AJF976_ed35bf91/fast5/guppy_out/fail",threads = 4)
```
![sc](https://user-images.githubusercontent.com/93121277/164649983-92288f0e-7253-4b67-97cb-8af02b7dd0aa.png)

# A FASTQC Directory was automatically generated in /guppy_out/fail/ and in this directory is an html file giving the following images.

![fail1](https://user-images.githubusercontent.com/93121277/164201047-4dd7d379-fba7-47af-a0c9-e48334f09db7.png)

![fail2](https://user-images.githubusercontent.com/93121277/164201041-488485c3-a425-4277-884a-44d0db03362a.png)

# FASTQ Quality Summary for the PASSED Minion Run

```r
setwd("/home/michael/FAST5/CoffeeMinIon/20220414_1039_MN19654_AJF976_ed35bf91/fast5/guppy_out")
library(fastqcr)
fastqc_install()
fastqc(fq.dir = "/home/michael/FAST5/CoffeeMinIon/20220414_1039_MN19654_AJF976_ed35bf91/fast5/guppy_out/pass",threads = 4)

```

![sd](https://user-images.githubusercontent.com/93121277/164650846-6f56de56-1d35-4874-82e7-5f719bf9c5c9.png)


# A FASTQC Directory was automatically generated in /guppy_out/pass/ and in this directory is an html file giving the following images.

![pass1](https://user-images.githubusercontent.com/93121277/164201143-c3431342-9829-4bdc-8ff4-60e9cd494618.png)
![pass2](https://user-images.githubusercontent.com/93121277/164201142-a314f352-04ce-4923-8baa-d7a029683101.png)
![pass3](https://user-images.githubusercontent.com/93121277/164201140-50e91b57-3965-41cd-991b-59ffab8a1050.png)










