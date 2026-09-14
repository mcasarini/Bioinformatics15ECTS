# Practice in Bioinformatics 15ECTS / 5MO0H
*Martina Casarini*

## General information
I started with the study of TCGA-PanCancer cohort in R to understand the properties of the different cancer types focusing on FOLH1 gene (encoding PSMA protein). Additionally, I have done RNA sequencing analysis on real samples too using mice tumour data. Then, in the recorded presentation I am only able to present the mice-related results for reason of time. 


# FOLH1 expression analysis in TCGA pan-cancer (Pan-cancer + Top10 + PRAD vs Normal)

## setup
base_dir <- "C:/Users/Martina/Desktop/TCGA/rawdata/0_TCGA-panCancer"
out_dir  <- "C:/Users/Martina/Desktop/TCGA/results"
dir.create(out_dir, showWarnings = FALSE, recursive = TRUE)
file_path <- file.path(base_dir, "TCGA-panCancer.rds")

## packages
library(SummarizedExperiment)
library(dplyr)
library(ggplot2)
library(openxlsx)
library(AnnotationDbi)
library(org.Hs.eg.db)
library(ggtext)
library(showtext)
font_add("Arial", "C:/Windows/Fonts/arial.ttf")
showtext_auto()

## load data 
se <- readRDS(file_path)
expr <- assay(se, "fpkm_uq_unstrand")
meta <- as.data.frame(colData(se))
rd   <- as.data.frame(rowData(se))
cat("Expression matrix:", dim(expr), "\n")

## gene mapping
ens <- gsub("\\..*", "", rd$gene_id)
symbols <- mapIds(
  org.Hs.eg.db,
  keys = ens,
  column = "SYMBOL",
  keytype = "ENSEMBL",
  multiVals = "first")
rd$symbol <- symbols
folh1_idx <- which(rd$symbol == "FOLH1")
stopifnot(length(folh1_idx) > 0)
folh1_expr <- expr[folh1_idx[1], , drop = TRUE]
stopifnot(length(folh1_expr) == ncol(expr))

## build dataset
df <- data.frame(
  sample = colnames(expr),
  FOLH1  = as.numeric(folh1_expr),
  group  = meta$sample_type,
  cancer = meta$project_id)
df <- na.omit(df)
df$logFOLH1 <- log2(df$FOLH1 + 1)
df_tumor <- df %>%
  filter(group == "Primary Tumor")
df_prad <- df %>%
  filter(cancer == "TCGA-PRAD",
         group %in% c("Primary Tumor", "Solid Tissue Normal"))
df_prad$group2 <- factor(
  ifelse(df_prad$group == "Primary Tumor", "Tumor", "Normal"),
  levels = c("Normal", "Tumor"))
df_tumor$highlight <- ifelse(df_tumor$cancer == "TCGA-PRAD", "PRAD", "Other")

## theme
theme_nature <- function(base_size = 14) {
  theme_classic(base_size = base_size) +
    theme(
      text = element_text(family="Arial"),
      plot.title = element_text(face = "bold"),
      axis.line = element_line(linewidth = 0.5),
      axis.ticks = element_line(linewidth = 0.5),
      legend.position = "none",
      axis.text.x = element_text(angle = 45, hjust = 1))}
df_tumor$cancer <- factor(df_tumor$cancer)
make_xlabels <- function(levels_vec){
  sapply(levels_vec, function(x){
    if (x == "TCGA-PRAD") {
      "<span style='color:red;'>TCGA-PRAD</span>"
    } else {
      x}})}

## PanCancer
order_pan <- df_tumor %>%
  group_by(cancer) %>%
  summarise(median = median(logFOLH1), .groups = "drop") %>%
  arrange(desc(median))
df_tumor$cancer <- factor(
  df_tumor$cancer,
  levels = order_pan$cancer)
df_tumor$highlight <- ifelse(df_tumor$cancer == "TCGA-PRAD", "PRAD", "Other")
xlabels_pan <- make_xlabels(levels(df_tumor$cancer))
p_pan <- ggplot(df_tumor, aes(cancer, logFOLH1, fill = highlight)) +
  geom_boxplot(outlier.shape = NA, width = 0.7) +
  geom_jitter(width = 0.2, alpha = 0.4, size = 0.6) +
  scale_fill_manual(values = c(PRAD = "red3", Other = "grey80")) +
  scale_x_discrete(labels = xlabels_pan) +
  theme_nature() +
  theme(axis.text.x = ggtext::element_markdown(angle = 45, hjust = 1)) +
  labs(
    title = "FOLH1 expression across TCGA cancers",
    x = NULL,
    y = "log2(FPKM-UQ + 1)")
ggsave(file.path(out_dir, "1_TCGA-PanCancer_FOLH1_panCancer.pdf"),
       p_pan, width = 12, height = 6)

pan_summary <- df_tumor %>%
  group_by(cancer) %>%
  summarise(
    N = n(),
    Mean = mean(logFOLH1, na.rm=TRUE),
    Median = median(logFOLH1, na.rm=TRUE),
    SD = sd(logFOLH1, na.rm=TRUE),
    .groups = "drop")
pan_summary$significant_flag <- ifelse(pan_summary$Median > median(pan_summary$Median), "High", "Low")

## top10 cancers
top10 <- df_tumor %>%
  group_by(cancer) %>%
  summarise(med = median(logFOLH1), .groups = "drop") %>%
  arrange(desc(med)) %>%
  slice(1:10)
df_top <- df_tumor %>%
  filter(cancer %in% top10$cancer)
df_top$cancer <- factor(df_top$cancer, levels = top10$cancer)
xlabels_top <- make_xlabels(levels(df_top$cancer))
df_top$highlight <- ifelse(df_top$cancer == "TCGA-PRAD", "PRAD", "Other")
p_top10 <- ggplot(df_top,
                  aes(cancer,
                      logFOLH1,
                      fill = highlight)) +
  geom_boxplot(outlier.shape = NA,
               width = 0.7) +
  geom_jitter(width = 0.2,
              alpha = 0.4,
              size = 0.6) +
  scale_fill_manual(values = c(PRAD="red3",
                               Other="grey80")) +
  scale_x_discrete(labels = xlabels_top) +
  theme_nature() +
  theme(axis.text.x =
          ggtext::element_markdown(angle = 45,
                                   hjust = 1)) +
  labs(
    title = "Top 10 TCGA cancers by FOLH1",
    x = NULL,
    y = "log2(FPKM-UQ + 1)")
ggsave(file.path(out_dir, "2_TCGA-PanCancer_FOLH1_top10.pdf"),
       p_top10, width = 10, height = 6)

top10_summary <- df_top %>%
  group_by(cancer) %>%
  summarise(
    N = n(),
    Mean = mean(logFOLH1, na.rm=TRUE),
    Median = median(logFOLH1, na.rm=TRUE),
    SD = sd(logFOLH1, na.rm=TRUE),
    .groups = "drop")
top10_summary$significant_flag <- ifelse(top10_summary$Median > median(top10_summary$Median), "High", "Low")

## PRAD vs NORMAL
ttest <- t.test(logFOLH1 ~ group2,data = df_prad)
pval <- ttest$p.value
sig_label <- function(p) {
  if (p < 0.001) "***"
  else if (p < 0.01) "**"
  else if (p < 0.05) "*"
  else "ns"}
stars <- sig_label(pval)
p_prad <- ggplot(df_prad, aes(x = group2, y = logFOLH1, fill = group2)) +
  geom_boxplot(outlier.shape = NA, width = 0.6) +
  geom_jitter(width = 0.15, size = 0.7, alpha = 0.6) +
  scale_fill_manual(values = c(Normal = "grey80", Tumor = "red3")) +
  theme_nature() +
  labs(
    title = "FOLH1 expression in PRAD",
    subtitle = paste0(
      "t = ", round(ttest$statistic, 2),
      " | p = ", format.pval(pval, digits = 3),
      " | ", stars),
    y = "log2(FPKM-UQ + 1)",
    x = NULL)
ggsave(file.path(out_dir, "3_TCGA-PanCancer_FOLH1_PRADvsnormal.pdf"),
       p_prad, width = 5, height = 5)

prad_stats <- data.frame(
  Comparison = "PRAD Tumor vs Normal",
  t_statistic = unname(ttest$statistic),
  p_value = pval,
  significance = stars)

## summary
nrow(df)
nrow(df_tumor)
nrow(df_prad)
wb_info <- createWorkbook()
addWorksheet(wb_info, "Summary")
writeData( wb_info,"Summary",data.frame(
    Metric = c(
      "Dataset",
      "Expression assay",
      "Gene analysed",
      "Expression values",
      "Transformation",
      "Total genes",
      "Total samples",
      "Primary Tumor samples",
      "PRAD Tumor samples",
      "PRAD Normal samples"),
    Value = c(
      "TCGA Pan-Cancer",
      "fpkm_uq_unstrand",
      "FOLH1",
      "FPKM-UQ",
      "log2(FPKM-UQ + 1)",
      nrow(expr),
      ncol(expr),
      sum(meta$sample_type == "Primary Tumor"),
      sum(meta$project_id == "TCGA-PRAD" &
            meta$sample_type == "Primary Tumor"),
      sum(meta$project_id == "TCGA-PRAD" &
            meta$sample_type == "Solid Tissue Normal"))))
addWorksheet(wb_info, "Sample_types")
writeData( wb_info,"Sample_types",as.data.frame(table(meta$sample_type)))
addWorksheet(wb_info, "Cancer_distribution")
writeData(wb_info,"Cancer_distribution", as.data.frame(table(meta$project_id)))
addWorksheet(wb_info, "FOLH1_data")
writeData(wb_info,"FOLH1_data",df)
addWorksheet(wb_info, "Tumor_only")
writeData(wb_info, "Tumor_only",df_tumor)
addWorksheet(wb_info, "PRAD_vs_Normal")
writeData(wb_info,"PRAD_vs_Normal",df_prad)
addWorksheet(wb_info, "PanCancer_Summary")
writeData(wb_info, "PanCancer_Summary", pan_summary)
addWorksheet(wb_info, "Top10_Summary")
writeData(wb_info, "Top10_Summary", top10_summary)
addWorksheet(wb_info, "PRAD_Statistics")
writeData(wb_info, "PRAD_Statistics", prad_stats)
saveWorkbook(wb_info,
  file.path(out_dir, "0_TCGA-PanCancer_FOLH1_summary.xlsx"),
  overwrite = TRUE)
write.csv(df,
          file.path(out_dir, "1_TCGA-PanCancer_FOLH1_panCancer.csv"),
          row.names = FALSE)
write.csv(df_tumor,
          file.path(out_dir, "2_TCGA-PanCancer_FOLH1_top10.csv"),
          row.names = FALSE)
write.csv(df_prad,
          file.path(out_dir, "3_TCGA-PanCancer_FOLH1_PRADvsnormal.csv"),
          row.names = FALSE)


# GLEASON ANALYSIS in TCGA-PRAD: Is the distribution of normalized FOLH1 expression significantly different among low-, intermediate-, and high-Gleason prostate tumors? Is PSMA (FOLH1) expression associated with prostate cancer aggressiveness?

## setup
base_dir <- "C:/Users/Martina/Desktop/TCGA"
setwd(base_dir)
out_dir  <- "C:/Users/Martina/Desktop/TCGA/results"
dir.create(out_dir, showWarnings = FALSE, recursive = TRUE)

## packages
library(SummarizedExperiment)
library(dplyr)
library(ggplot2)
library(ggpubr)
library(DESeq2)
library(openxlsx)
library(AnnotationDbi)
library(org.Hs.eg.db)
library(rstatix)

## load TCGA-PRAD
prad <- readRDS("TCGA_PRAD_clinical.rds")
se <- prad
assayNames(se)

## gene extraction
rd <- as.data.frame(rowData(se))
ens <- gsub("\\..*", "", rd$gene_id)
symbols <- mapIds(
  org.Hs.eg.db,
  keys = ens,
  column = "SYMBOL",
  keytype = "ENSEMBL",
  multiVals = "first")
rowData(se)$symbol <- symbols
folh1_idx <- which(rowData(se)$symbol == "FOLH1")
stopifnot(length(folh1_idx) > 0)

## normalization + VST
dds <- DESeqDataSetFromMatrix(
  countData = assay(se, "unstranded"),
  colData = colData(se),
  design = ~ 1)
dds <- dds[rowSums(counts(dds)) > 0, ]
dds <- estimateSizeFactors(dds)
vsd <- vst(dds, blind = TRUE)
folh1_expr <- assay(vsd)[folh1_idx, ]

## build dataframe
cd <- as.data.frame(colData(se))
df <- data.frame(
  sample  = colnames(se),
  FOLH1   = as.numeric(folh1_expr),
  gleason = cd$paper_Reviewed_Gleason_sum,
  group   = cd$sample_type)
df <- df %>%
  filter(group == "Primary Tumor") %>%
  filter(!is.na(FOLH1) & !is.na(gleason))

## Gleason groups
df$gleason_group <- case_when(
  df$gleason <= 6 ~ "Low (≤6)",
  df$gleason == 7 ~ "Intermediate (7)",
  df$gleason >= 8 ~ "High (≥8)")
df$gleason_group <- factor(
  df$gleason_group,
  levels = c(
    "Low (≤6)",
    "Intermediate (7)",
    "High (≥8)"))

## statistics 
overall_stats <- df %>%
  summarise(
    n = n(),
    mean = mean(FOLH1),
    sd = sd(FOLH1),
    median = median(FOLH1),
    IQR = IQR(FOLH1),
    min = min(FOLH1),
    max = max(FOLH1))

group_stats <- df %>%
  group_by(gleason_group) %>%
  summarise(
    n = n(),
    mean = mean(FOLH1),
    sd = sd(FOLH1),
    median = median(FOLH1),
    IQR = IQR(FOLH1),
    min = min(FOLH1),
    max = max(FOLH1),
    .groups = "drop")

kruskal_res <- kruskal.test(FOLH1 ~ gleason_group, data = df)
kruskal_table <- data.frame(
  test = "Kruskal-Wallis",
  statistic = unname(kruskal_res$statistic),
  df = unname(kruskal_res$parameter),
  p_value = kruskal_res$p.value)

spearman_res <- cor.test(df$FOLH1, df$gleason, method = "spearman", exact = FALSE)
spearman_table <- data.frame(
  method = "Spearman",
  rho = unname(spearman_res$estimate),
  p_value = spearman_res$p.value)

pairwise_res <- pairwise_wilcox_test(
  df,
  FOLH1 ~ gleason_group,
  p.adjust.method = "BH")

anova_res <- aov(FOLH1 ~ gleason_group, data = df)
anova_summary <- summary(anova_res)[[1]]
anova_table <- data.frame(anova_summary)

tukey_res <- TukeyHSD(anova_res)
tukey_df <- as.data.frame(tukey_res$gleason_group)
tukey_df$comparison <- rownames(tukey_df)

eta_sq <- anova_summary$`Sum Sq`[1] / sum(anova_summary$`Sum Sq`)
effect_size <- data.frame(
  metric = "eta_squared",
  value = eta_sq)

get_stars <- function(p) {
  ifelse(p <= 0.0001, "****",
         ifelse(p <= 0.001, "***",
                ifelse(p <= 0.01, "**",
                       ifelse(p <= 0.05, "*", "ns"))))}

method_comparison <- data.frame(
  test = c("Kruskal-Wallis", "ANOVA"),
  statistic = c(
    unname(kruskal_res$statistic),
    anova_summary$`F value`[1]),
  p_value = c(
    kruskal_res$p.value,
    anova_summary$`Pr(>F)`[1]),
  significance = c(
    get_stars(kruskal_res$p.value),
    get_stars(anova_summary$`Pr(>F)`[1])))

## summary
wb <- createWorkbook()
addWorksheet(wb, "Raw_data")
writeData(wb, "Raw_data", df)
addWorksheet(wb, "Overall_stats")
writeData(wb, "Overall_stats", overall_stats)
addWorksheet(wb, "Group_stats")
writeData(wb, "Group_stats", group_stats)
addWorksheet(wb, "Spearman")
writeData(wb, "Spearman", spearman_table)
addWorksheet(wb, "Kruskal_Wallis")
writeData(wb, "Kruskal_Wallis", kruskal_table)
addWorksheet(wb, "Pairwise_Wilcoxon")
writeData(wb, "Pairwise_Wilcoxon", pairwise_res)
addWorksheet(wb, "ANOVA")
writeData(wb, "ANOVA", anova_table)
addWorksheet(wb, "Tukey_HSD")
writeData(wb, "Tukey_HSD", tukey_df)
addWorksheet(wb, "Effect_Size")
writeData(wb, "Effect_Size", effect_size)
addWorksheet(wb, "Method_Comparison")
writeData(wb, "Method_Comparison", method_comparison)
saveWorkbook(wb,
  file.path(out_dir, "4b_PRAD-TCGA_FOLH1_summary.xlsx"),
  overwrite = TRUE)

## plot
p <- ggplot(df, aes(x = gleason_group, y = FOLH1, fill = gleason_group)) +
  geom_boxplot(width = 0.6, outlier.shape = NA, alpha = 0.85) +
  geom_jitter(width = 0.15, size = 1, alpha = 0.4) +
  scale_fill_manual(
    values = c(
      "Low (≤6)" = "grey70",
      "Intermediate (7)" = "orange",
      "High (≥8)" = "red3"),
    name = "Gleason group" ) +
  stat_compare_means(
    comparisons = comparisons,
    method = "wilcox.test",
    label = "p.signif",
    hide.ns = FALSE,
    step.increase = 0.1) +
  theme_bw(base_size = 18) +
  theme(
    panel.grid.minor = element_blank(),
    panel.grid.major = element_line(color = "grey85", linewidth = 0.4),
    plot.title = element_text(hjust = 0.5, face = "bold"),
    axis.text.x = element_text(size = 13),
    axis.title = element_text(size = 14),
    legend.position = "right") +
  labs(
title = "FOLH1 expression\nacross Gleason groups (TCGA-PRAD)",
    x = "Gleason group",
    y = "VST-normalized FOLH1 expression")
print(p)
ggsave(
  file.path(out_dir, "4b_PRAD-TCGA_FOLH1_Primary_GleasonScore.pdf"),
  p,
  width = 8,
  height = 5.5)
