# tasmanian-devil-gene-enrichment-analysis

The Tasmanian devil is endangered due to Devil Facial Tumour Disease (DFTD), a contagious cancer with no current
treatment. [Previous study](https://www.nature.com/articles/s41598-023-39901-0) has investigated the anticancer properties of Tasmanian devil cathelicidins which are an-
timicrobial peptides that may also have immunomodulatory and anticancer effects. Four cathelicidins (Saha-CATH3,
4, 5, and 6) significantly reduced cell viability and impacted gene expression related to DNA replication and cell cycle
processes. This report intends to reproduce the enrichment analysis results of the study and further analyze their data.

Keywords: Tasmanian Devil, Cancer, RNASeq
∗Corresponding author
Email address: arash.salmoslehian@epfl.ch(Arash Sal
Moslehian)

# 1. Introduction

The Tasmanian devil (Sarcophilus harrisii) is endangered
due to Devil Facial Tumour Disease (DFTD), a conta-
gious cancer spread through biting that evades immune
responses. Despite various attempts with medications
and vaccines, no treatments have advanced to clinical
trials. This study explores the anticancer potential of
the devil’s own cathelicidins which are antimicrobial
peptides with diverse biological functions. To test their
eﬀicacy, researchers conducted a cytotoxicity assay on
the DFT1 cell line 1426, using seven cathelicidins at
different intervals. They found that four (Saha-CATH3,
4, 5, and 6) significantly reduced cell viability and caused
stress. RNAseq analysis showed these peptides downreg-
ulated genes related to DNA replication and cell cycle
progression, particularly noting that Saha-CATH5 also
affected the ERBB and Hippo signaling pathways. This
indicates that Saha-CATH5 might act similarly to Recep-
tor Tyrosine Kinase (RTK) inhibitors, which are known
to be effective against DFTD. These findings suggest
that Tasmanian devil cathelicidins could have promising
anti-cancer and immune-modulating properties, requiring
further investigation as potential DFTD treatments. [ 1 ]
In this study, I first reproduce the Over-Representation
Analysis (ORA) results from the original paper, and then
augment the analysis with aditional discriptive plots and
Gene Set Enrichment Analysis (GSEA). GSEA evaluates
whether a particular gene set shows statistically significant
differences in expression between two biological states, fo-
cusing on the entire distribution of gene expression rather
than just the most differentially expressed genes.


# 2. Methods

The previous study investigated the anticancer activity of
Tasmanian devil cathelicidin peptides on DFT1 1426 cells
using RNA sequencing (RNAseq). Confluent cells were
treated with each peptide (Saha-CATH1 to 7) over 12, 18,
24, and 36 hours and RNA was extracted. Sequencing and
pre-processing of 24 RNA samples, corresponding to three
treaments per peptide, produced a set of gene counts. [ 1 ]
In this reproducability study, gene counts were utilized
as input for differential expression analysis in R. There
were seven conditions and one control each containing
three samples. To enhance biological relevance and
statistical power, genes with fewer than 50 counts across
all samples were excluded from the analysis. Initially,
the data was normalized using the trimmed mean of M
values (TMM) method in edgeR v4.0.16, which adjusts
for composition bias between libraries and provides an
effective library size for further analysis. To examine
variation between treatments, multidimensional scaling
(MDS) was conducted with limma v3.58.1. Subsequently,
expression levels were normalized using upper-quartile
normalization in EDAseq v2.36.0 to account for differ-
ences in sequencing depth and distribution across lanes.
The differential expression analysis was then performed
using the voom function in the limma v3.58.1 package.
For each treatment, a false discovery rate (FDR) cutoff
of0.02was applied, and genes with a fold change greater
than 1.5 (either upregulated or downregulated) were
selected for further analysis. These genes were subjected
to Gene Ontology (GO) and Gene Set Exrpession Anal-
ysis (GSEA). Both Over-representation analysis and
Gene set expression analysis of Biological Processes were
conducted using clusterProfiler v4.10.1, with statistical
significance adjusted for multiple comparisons via the
Benjamini–Hochberg method. GO terms were considered
Preprint submitted to Elsevier May 20, 2024
significant when p-adj < 0.05, and to refine the results,
gene sets larger than 200 were removed, and redundant
GO terms were eliminated using the simplify function
with p-adj cutoff of0.7using the Wang measure.
As the terms in Gene Ontology are constatly being up-
dated and changed, reproducing the exact figures from the
original paper might not be feasible. I tried different ver-
sions oforg.Hs.eg.dbpackage and version v3.12.0 seems
to be the one that matches the most. The simplification of
similar terms can also be a factor of difference between the
original paper and the reproduced results. Even though
the original paper had reported a1.5fold change, I found
that in order to get the same numbers for differentially
expressed genes as those outlined in the paper I had to
tune the fold change threshold to1.4948. Moreover, the
Mar-02 gene was duplicated in the dataset and the clone
with the least amount of counts was manually removed.

# 3. Results and Discussion

Out of 15547 genes, 12401 showed differential expression
(DE) across all seven treatments compared to the con-
trol. The Saha-CATH5 treatment had the highest num-
ber of DE genes, with 11513 (74.05%). Other toxic treat-
ments had lower DE percentages: Saha-CATH3 with 1965
(12.64%), Saha-CATH4 with 2915 (18.75%), and Saha-
CATH6 with 2419 (15.56%). The non-toxic treatments
(Saha-CATH1, 2, and 7) showed less than 1% DE genes,
with Saha-CATH7 showing none under the quality filters.
Treating DFT1 cells with Saha-CATH3, 4, and 5 led
to the suppression of genes involved in DNA replication,
cell cycle progression, and checkpoints as confirmed by
both GO (Figure 1 ) and GSEA analysis (Figure 4 ). Saha-
CATH5 also influenced the ERBB and Hippo signaling
pathways. Saha-CATH6 induced Endoplasmic Reticulum
(ER) stress in DFT1 cells through various mechanisms (gly-
cosylation inhibition, protein hydroxylation, and calcium
depletion) according to GO analysis (Figure 2 ). Addition-
ally, Saha-CATH 6 upregulated genes linked with cytokine
expression and immune signaling pathways.
The volcano plots in Figure 3 show that indeed we do
not see much differentially expressed genes in Saha-CATH
1 and 7 treatment. Saha-CATH 5 shows large amounts of
differentially downregulated genes which is in line with the
fact that Saha-CATH5 displayed the most rapid cytotoxic
activity against DFT1 cells according to the original paper
[ 1 ].
Figure 4 , Figure 5 , Figure 6 , Figure 7 show the GSEA
results. Saha-CATH3, 4, and 6 all have down regulation
of DNA replication (a negative enrichment score). Saha-
CATH3 shows activity in immune response pathways (pos-
itive regulation of immune effector process and adaptive
immune response) and inflammatory responses (negative
regulation of cytokine production). Saha-CATH4 downreg-

ulates several critical biological processes and cell prolifer-
ation pathways. Saha-CATH5 seems to be affecting mi-
tochondrial activity and energy production, which is vital
for cellular function. It also seems to be regulating devel-
opmental and signaling pathways. Saha-CATH6 regulates
pathways related to ER and activates immune response
pathways. The downregulation of responses to bacterial
components and chondroitin sulfate-related processes sug-
gests a shift in the cellular focus from bacterial defense to
other cellular priorities.

# 4. Conclusion

In this study, I reproduced the results obtained in [ 1 ] and
confirmed their results in pinpointing four Tasmanian devil
cathelicidins (Saha-CATH3, 4, 5, and 6) that can reduce
DFT1 cell viability in labratory tests. Saha-CATH3 and
4 induced cell cycle arrest, Saha-CATH5 caused oncogenic
pathway inhibition, and Saha-CATH6 caused ER stress.
By analyzing RNAseq data, I found that these catheli-
cidins may trigger inflammatory pathways in DFT1 cells
like increase cytokine expression. I also included volcano
plots for better visualization of differential gene expression
of the conditions and further analyzed the results through
Gene Set Expression Analysis and highlighted the differ-
ent pathways some of which were also present in ORA of
the original paper.
All in all, I successfuly reproduced the main results of the
previous studies and confrimed the effect of these catheli-
cidins on DFTD cells.

![image](https://github.com/user-attachments/assets/d8776574-2881-4f3e-aa10-69088221ce16)

![image](https://github.com/user-attachments/assets/c98abbad-0020-4696-8930-0310ab17e07f)

![image](https://github.com/user-attachments/assets/95192645-4dcb-4c9d-a3e7-f9f81bc00eec)

![image](https://github.com/user-attachments/assets/7cff8dda-3b60-44a4-846c-8a2a4ac3245b)

![image](https://github.com/user-attachments/assets/32a86ebe-bea6-4e10-9f07-6b2777d220cd)

![image](https://github.com/user-attachments/assets/3be85b14-f922-47d8-935a-bf0d41b08d1a)

![image](https://github.com/user-attachments/assets/907ae6f8-307b-4a36-a349-ebef5c8817ae)





