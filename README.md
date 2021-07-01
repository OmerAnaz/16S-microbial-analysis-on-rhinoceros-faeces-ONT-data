This page shows the steps on galaxy for doing a microbial analysis on the input data

![galaxy workflow](https://user-images.githubusercontent.com/81419117/122450875-cd8d2700-cfa7-11eb-951f-4b96126d1e82.png)

**Input file:** 
Oxford Nanopore Technologies fastq formated data of the 16S region


**Step 1:** 

Assess quality of data

<b>FastQC</b> tool on the (original) input data. Outputs are
>FastQC unprocessed : Raw 
>FastQC unprocessed : Web data for each input. 

<b>MultiQC</b> with settings : 
>"Which tool was used to generate logs?" set to FastQC. 
Input used is the raw files from FastQC output.


**Step 2:**

Improving the data quality

<b>Porechop</b> tool is used for adapter trimming with the settings : 
>"Input FASTA/FASTQ" = orignal data. 
>"Output format for the reads" = fastq. 

The output is renamed trimmed barcode.


**Step 3**

Sequence filtering

<b>Fastp</b> tool is used on the trimmed barcode data with the settings : 
>"Single-end or paired reads" = Single-end. 
>In Adapter Trimming Options "Disable adapter trimming" = Yes.

In Filter Options/Quality filtering options "Qualified quality phred" = 9. In Length filtering  options "Length required" = 1000 and "Maximum Length" = 2000. 

In Read Modification Options "PolyG tail trimming" = Disable polyG tail trimming

The output is renamed to barcodes processed.


**Step 4**

Asses quality of processed data

<b>FastQC</b> tool is run with the barcodes processed data as input. Output is renamed to Processed Raw and Processed Web

<b>MultiQC</b> tool is run with the Processed Raw data as input with the settings : 
>"Which tool was used to generate the logs?" = FastQC


**Step 5**

Taxonomic classification

<b>Kraken2</b> tool is run with the barcoodes processed data as input and with the settings : 
>"Single or paired reads" = Single. 
>"Print scientific names instead of just taxids" = Yes. 
>"Confidence" = 0.1

>In Create Report "Print a report with aggregate counts/clade to file" = Yes. 
>"Format report output like Kraken 1's kraken-mpa-report" = Yes

The "Select a Kraken2 database" is the new Silva database for 16s. In this case Silva (Created: 2020-06-24T164526Z, kmer-len=35, minimizer-len=31, minimizer-spaces=6)


**Step 6**

Preparing the taxonomic data

<b>Reverse</b> tool is used on the Kraken2 report data

<b>Replace Text</b> tool is used on the output from the Reverse tool with settings : 
>in Replacement / Insert Replacement 
>"Find pattern" = \| 
>"Replace pattern = \t

<b>Remove beginning</b> tool is used on the output from the Replace Text tool.


**Step 7**

Visualizing the data

<b>Krona pie chart</b> tool was used on the output from the Remove beginning tool with the settings : 
>"What is the type of your input data" = Tabular. 
>"Provide a name for the basal rank" = Bacteria


Appendices
Fastqc on raw barcodes
![image](https://user-images.githubusercontent.com/81419117/124190576-cd6e4a80-dac2-11eb-84a4-90ff2cd98eac.png)
Fastqc of raw barcode_01_06
 
