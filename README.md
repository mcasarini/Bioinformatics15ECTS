# Practice in Bioinformatics 15ECTS / 5MO0H
author: *Martina Casarini*

## General information
I started with the study of TCGA-PanCancer cohort in R to understand the properties of the different cancer types focusing on FOLH1 gene (encoding PSMA protein). Then, I have deepened the analysis in TCGA-PAAD and TCGA-PRAD. The core of the analysis was made on real samples using syngeneic mice tumour data named MTK25. 
In the recorded presentation I am presenting the "MTK25" mice tumour-related results compared to TCGA-PAAD cohort. 

### syngeneic mice tumour data "MTK25" scheme
<img width="624" height="285" alt="image" src="https://github.com/user-attachments/assets/2d28f16b-48c6-4aaa-8ef2-0124ed24c5d6" />

### biological importance
The main actors in this study are MTK458 and PINK1 (PTEN-induced kinase 1) in PAAD (pancreatic adenocarcinoma) and FOLH1 gene (encoding PSMA protein) in PRAD (prostate adenocarcinoma). 
*PAAD* is one of the most lethal cancers, characterized by poor prognosis and limited therapeutic options. Increasing evidence suggests that metabolic adaptation, mitochondrial quality control, and cellular plasticity contribute to tumor progression and therapy resistance.
A key regulator of mitochondrial homeostasis is PINK1, a kinase that initiates mitophagy, the selective removal of damaged mitochondria. Analysis of TCGA-PAAD data shows that high PINK1 expression is associated with a distinct and more aggressive tumor subtype displaying neuronal and neuroendocrine-like transcriptional features.
MTK458 is a small-molecule activator of PINK1 that stabilizes its active form and enhances PINK1/Parkin-mediated mitophagy. Although originally developed for neurodegenerative diseases, its effects on pancreatic tumor biology remain largely unexplored.
Another molecule of interest is FOLH1 (also known as PSMA), a membrane-associated glutamate carboxypeptidase implicated in tumor biology and cellular differentiation programs. In this study, FOLH1 was evaluated as a potential marker associated with MTK458-responsive transcriptional changes and neuroendocrine-like phenotypes.
The objective of this work was to investigate the clinical relevance of PINK1 in human PAAD and to determine how pharmacological activation of PINK1 by MTK458 influences gene expression, inflammatory signaling, tumor microenvironment remodeling, and neuroendocrine-associated programs in a mouse pancreatic cancer model.

### scripts & relative aims

TCGA-PAAD
How is FOLH1 (PSMA) expression associated with pancreatic cancer biology, tumor aggressiveness, clinical outcome, pathological progression, tumor microenvironment composition, immune infiltration, and molecular pathway activity in TCGA-PAAD?

TCGA-PRAD
How is FOLH1 (PSMA) expression associated with prostate cancer biology, tumor aggressiveness, clinical outcome, androgen receptor signaling, pathological progression, tumor microenvironment composition, immune infiltration, and molecular pathway activity in TCGA-PRAD?

TCGA-PanCancer_FOLH1
How does FOLH1 (PSMA) expression vary across human cancers, and how does prostate adenocarcinoma (TCGA-PRAD) compare with other tumor types and normal prostate tissue?

miceRNASeq_check & DESeq2
Which genes change expression between experimental conditions, treatments, genotypes, and PINK1 overexpression in several mouse studies?

miceRNASeq_MTK25
How does PINK1 deficiency (KO) alter transcriptional programs, neuroendocrine identity, cell-type composition, mitochondrial biology, biomarker expression, and biological pathways, and to what extent can MTK458 treatment rescue these molecular abnormalities?
