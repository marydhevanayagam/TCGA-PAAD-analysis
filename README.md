# **Biomarker Discovery for Early Cancer Detection of Pancreatic Adenocarcinoma (PAAD)**

## 1. **Project Objective**

In this project, RNA-seq data for pancreatic cancer (PAAD) from The Cancer Genome Atlas (TCGA) was analyzed to identify potential biomarkers for early cancer detection. This project focuses on finding genes that are significantly expressed in early-stage pancreatic cancer samples compared to normal samples. 

## 2. **GitHub repository folders**

* Code \- contains the R script of the code for data preprocessing, differential expression analysis (DEA), and functional enrichment analysis (FEA). 
* Data \- contains the data obtained after pre-processing and DEA.  
* Results \- contains the results generated from DEA and FEA.  

## 3. **Requirements**

The following R libraries were used:
* TCGAbiolinks 
* SummarizedExperiment
* dplyr
* reshape2  
* edgeR 
* org.Hs.eg.db  
* reshape2  
  
These can be installed by running:  
install.packages(c(“dplyr”, “reshape2”))

To install Bioconductor packages:
* install.packages("BiocManager")  
* BiocManager::install("TCGAbiolinks")   
* BiocManager::install("SummarizedExperiment")  
* BiocManager::install("edgeR")  
* BiocManager::install("org.Hs.eg.db")

## 4. **Methodology \- Code**

### **4.1.  Data Acquisition** 

* RNA-seq data of PAAD patient samples ("**TCGA-PAAD**" project) was acquired from TCGA using the **TCGAbiolinks** package functions in R.   
* A query was prepared to retrieve "**Gene Expression Quantification**" data from the "**Transcriptome Profiling**" data category of the "**TCGA-PAAD**" project using **GDCquery()** function. 
* Using **GDCdownload()** and **GDCprepare()** functions of the TCGAbiolinks package, the sample sets were downloaded and prepared for analysis.  
* From the retrieved data, metadata was obtained with the sample "**barcode**", "**tissue\_type**" and "**ajcc\_pathologic\_stage**" fields.  
* The barcodes were selected for early stage (stage 1) pancreatic cancer samples and normal tissue samples.

### **4.2.  Data Preprocessing** 

* The unstranded data of the selected samples was obtained for analysis.  
* The **TCGAanalyze\_Normalization()** function was used to normalize the gene expression data by gene length and read depth.  
* The **TCGAanalyze\_Filtering()** function was used to eliminate low-expression genes from the normalized data with the cutoff set at the first quantile (0.25).  
* The final filtered data was analyzed downstream for differential gene expression analysis (DEA) and functional enrichment analysis (FEA).
  
### **4.3.  Differential Expression Analysis (DEA)** 

* The **TCGAanalyze\_DEA()** function was used to compare the gene expression levels between early stage pancreatic cancer tumors and normal samples using the **edgeR** pipeline.   
* Genes were categorized as significantly upregulated and significantly downregulated based on a log fold-change (logFC) threshold of \>1.5 or \<(-1.5) and a false discovery rate (FDR) cut-off of \<0.01.
* The **mapIds()** function in the **org.Hs.eg.db** package was used to convert the Ensembl IDs of the DGEA results to gene symbols.    
* The functions of the top 5 significantly up-regulated and down-regulated genes were obtained using the [GeneCards](https://www.genecards.org/) database.

### **4.4. Functional Enrichment Analysis (FEA)**

* The **TCGAanalyze\_EAcomplete()** function was used to conduct functional enrichment analysis and pathway analysis for the genes with significant upregulation and downregulation.  
* The results of enrichment analysis were presented as bar plots using the **TCGAvisualize\_EAbarplot()** function to highlight the top 5 most enriched terms based on fold enrichment and FDR values for biological processes (BP), cellular components (CC), molecular functions (MF) and pathways.  
