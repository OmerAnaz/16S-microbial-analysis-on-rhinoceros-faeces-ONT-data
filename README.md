This page shows the steps on galaxy for doing a microbial analysis on the input data

![galaxy workflow](https://user-images.githubusercontent.com/81419117/122450875-cd8d2700-cfa7-11eb-951f-4b96126d1e82.png)

**Input file:** 
Oxford Nanopore Technologies fastq formated data of the 16S region

**Step 1:** 

Assess quality of data

FastQC tool on the (original) input data --> FastQC unprocessed : Raw and FastQC unprocessed : Web data for each input. MultiQC with settings "Which tool was used to generate logs?" set to FastQC. Input used is the raw files from FastQC output.
**Step 2:**

Improving the data quality

Porechop tool is used for adapter trimming with the settings "Input FASTA/FASTQ" = orignal data. "Output format for the reads" = fastq. The output is renamed trimmed barcode. 

**Step 3**

Sequence filtering

Fastp tool is used on the trimmed barcode data with the settings "Single-end or paired reads" = Single-end. In Adapter Trimming Options "Disable adapter trimming" = Yes.
In Filter Options/Quality filtering options "Qualified quality phred" = 9. In Length filtering  options "Length required" = 1000 and "Maximum Length" = 2000. 
In Read Modification Options "PolyG tail trimming" = Disable polyG tail trimming
The output is renamed to barcodes processed.
