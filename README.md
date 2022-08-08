# minorAntigen

For exploring the minor antigens, we start with common SNPs. Below are the definition of **common SNPs** -

dbSNP uses frequency data from the 1000 Genomes Project, and considers a variant COMMON if it has a MAF of at least 0.01 in any of the five super-populations:

* African (AFR)
* Admixed American (AMR)
* East Asian (EAS)
* European (EUR)
* South Asian (SAS)

In build 151, dbSNP marks approximately 38M variants as COMMON; 23M of those have a global MAF < 0.01.

## Step 1

The dbSNP has been downloaded at 
**/home/p_eclipse_combio/commonSNP/00-common_all.vcf**
```bash
-rwxr-x--- 1 p_eclipse_combio eclipse_platform-lab 8.8G Sep  8  2021 00-common_all.vcf
```

## Step 2
Split the vcf file by chromosome to speed up the annotation.

## Step 3
Normalize the vcf file to make sure that each row of the vcf files contains only one alternative allele.

For example in the orginial file, there are rows like:
```bash
1	13550	rs554008981	G	A,C	.	.	RS=554008981;RSPOS=13550;dbSNPBuildID=142;SSR=0;SAO=0;VP=0x050000000005040026000100;GENEINFO=DDX11L1:100287102;WGT=1;VC=SNV;ASP;VLD;KGPhase3;CAF=0.9966,0.003395,.;COMMON=1;TOPMED=0.99221139143730886,0.00778064475025484,0.00000796381243628
```
This will be transformed into two rows, with last column simplied.
```bash
1	13550	rs554008981	G	A	0.003395	.	0.003395
1	13550	rs554008981	G	C	.	.	.
```

This can be easily done using the Rscript **myNorm.R**.
Next, further filter the vcfs by frequency (i.e., MAF>0.04). This was done using the Rscript **filterFreq.R**

## Step 4. 
Annotate the vcf files using vep. Since the vcf has been split by chromosomes, this can done using Snakemake in a parallel way. See **mafOnly.sh** and **vcf2maf.sh**.

## Step 5.
Retrieve the flanking sequencing for the following mutation categorties:
* 'Frame_Shift_Del',
* 'Frame_Shift_Ins',
* 'In_Frame_Del',
* 'In_Frame_Ins',
* 'Missense_Mutation',
* 'Nonsense_Mutation',
* 'Nonstop_Mutation'

by the R script 


