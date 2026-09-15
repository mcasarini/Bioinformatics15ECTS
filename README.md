# Practice in Bioinformatics 15ECTS / 5MO0H
author: *Martina Casarini*

## General information
The main actors in this study are PINK1 (PTEN-induced kinase 1) and MTK458 (PINK1-activator) in PAAD (pancreatic adenocarcinoma) and FOLH1 gene (encoding PSMA protein) in PRAD (prostate adenocarcinoma). 

**PAAD** is one of the most lethal cancers, characterized by poor prognosis and limited therapeutic options. Increasing evidence suggests that metabolic adaptation, mitochondrial quality control, and cellular plasticity contribute to tumor progression and therapy resistance.
A key regulator of mitochondrial homeostasis is **PINK1**, a kinase that initiates mitophagy, the selective removal of damaged mitochondria. Since metabolic and mitochondrial dysfunction are hallmarks of cancer, alterations in PINK1 signaling may influence tumor behavior, cellular plasticity, and disease progression. **MTK458** is a small-molecule activator of PINK1, originally developed for the treatment of Parkinson's disease and other neurodegenerative disorders, that directly activates the PINK1 pathway and assesses how modulation of mitochondrial quality-control mechanisms affects global gene-expression programs in pancreatic cancer. 

**FOLH1** (folate hydrolase 1) encodes the prostate-specific membrane antigen (PSMA), a transmembrane glycoprotein highly expressed in **PRAD**, the most common prostate cancer subtype known for its tumor aggressiveness and molecular heterogeneity. FOLH1 is clinically important because PSMA is widely used as a diagnostic imaging target and an emerging therapeutic target in prostate cancer. Furthermore, variation in FOLH1 expression has been associated with tumor heterogeneity, disease progression, and clinically relevant molecular subtypes.

### workflow
I started with the study of TCGA-PanCancer cohort in R focusing on FOLH1 gene to understand how this gene is expressed across all cancer types. Then, I have deepened the analysis in TCGA-PAAD and TCGA-PRAD specifically to understand the clinical and biological relevance of PINK1 in PAAD and the role of FOLH1 in PRAD through expression, clinicopathological, pathway and tumour microenvironment analyses. Having done this preliminary investigation, I moved to the core of this project: transcriptomic analysis of a syngeneic mouse pancreatic tumor model treated with MTK458 (called MTK25 dataset). 

The analysis of MTK25 dataset includes:
- biomarker analysis of a predefined gene set identified thanks to a discussion with a pathologist;
- functional enrichment analysis (KEGG pathways, GO biological process, Reactome pathways and Hallmark GSEA);
- MTK458-mediated rescue analysis at gene level and pathway level;
- neuroendocrine signature scoring and correlation of neuroendocrine programs with PINK1;
- cell-type signature analysis;
- Hallmark pathway activity by GSVA;
- mitochondrial and mitophagy signature analysis.

In the recorded presentation I am presenting the mice tumour-related results compared to TCGA-PAAD cohort. 

### objectives
- Characterize FOLH1 expression across human cancers using TCGA-PanCancer cohort;
- Investigate the biological relevance of PINK1 in PAAD and FOLH1 in PRAD using the respective TCGA cohorts;
- Analyze transcriptomic changes induced by MTK458 in syngeneic mouse PAAD tumors;
- Identify molecular pathways in PAAD altered by MTK458 treatment (mitophagy, mitochondrial function, immune signalling and tumor microenvironment regulation);
- Evaluate neuroendocrine-associated gene programs and potential biomarkers associated with PINK1 activation in PAAD.

### syngeneic mice tumour data "MTK25" scheme 
<img width="624" height="285" alt="image" src="https://github.com/user-attachments/assets/2d28f16b-48c6-4aaa-8ef2-0124ed24c5d6" />
