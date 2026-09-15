# Practice in Bioinformatics 15ECTS / 5MO0H
author: *Martina Casarini*

## General information
The main actors in this study are PINK1 (PTEN-induced kinase 1) and MTK458 (PINK1-activator) in PAAD (pancreatic adenocarcinoma) and FOLH1 gene (encoding PSMA protein) in PRAD (prostate adenocarcinoma). 

**PAAD** is one of the most lethal cancers, characterized by poor prognosis and limited therapeutic options. Increasing evidence suggests that metabolic adaptation, mitochondrial quality control, and cellular plasticity contribute to tumor progression and therapy resistance.
A key regulator of mitochondrial homeostasis is **PINK1**, a kinase that initiates mitophagy, the selective removal of damaged mitochondria. Since metabolic rewiring and mitochondrial dysfunction are hallmarks of cancer, alterations in PINK1 signaling may influence tumor behavior, cellular plasticity, and disease progression. To investigate this pathway pharmacologically, the effects of **MTK458**, a small-molecule activator of PINK1, were studied in mice syngeneic tumour models. MTK458 provides a tool to directly activate the PINK1 pathway and assess how modulation of mitochondrial quality-control mechanisms affects global gene-expression programs in pancreatic cancer.
In parallel, **FOLH1** (folate hydrolase 1), which encodes the prostate-specific membrane antigen (PSMA), was investigated in **PRAD**, the most common prostate cancer subtype known for its tumor aggressiveness and molecular heterogeneity. FOLH1 is clinically important because PSMA is widely used as a diagnostic imaging target and an emerging therapeutic target in prostate cancer. Furthermore, variation in FOLH1 expression has been associated with tumor heterogeneity, disease progression, and clinically relevant molecular subtypes.

### aims
I started with the study of TCGA-PanCancer cohort in R to understand the properties of the different cancer types focusing on FOLH1 gene (encoding PSMA protein). Then, I have deepened the analysis in TCGA-PAAD and TCGA-PRAD. The core of the analysis was made on real samples using syngeneic mice tumour data named MTK25. 
In the recorded presentation I am presenting the "MTK25" mice tumour-related results compared to TCGA-PAAD cohort. 

### syngeneic mice tumour data "MTK25" scheme
<img width="624" height="285" alt="image" src="https://github.com/user-attachments/assets/2d28f16b-48c6-4aaa-8ef2-0124ed24c5d6" />


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
