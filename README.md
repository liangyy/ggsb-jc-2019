# ggsb-jc-2019
This repository is for presenting in GGSB journal club in 2019 @ UChicago

Useful video explaining multiplex idea: [link](https://www.youtube.com/watch?v=UPDIhhjuoR0)

Probes are ~20mer and each set of probes for a gene is a representative to the existence of a type of transcript (defined by sequence)
The idea to have shorter probes but more is that easier to synthesize (order from commercials) and it can carry more probes which makes it easier to be detected. It is more accurate (high specificity and sensitivity).

# Technology side notes

There are exciting improvements and applications:

## Technology side

1. [Expansion microscopy](https://science.sciencemag.org/content/347/6221/543): fixing molecule on gel and expand gel so that the object of interest is enlarged isotropically.
2. [SeqFISH+](https://www.nature.com/articles/s41586-019-1049-y): Whole transcriptome seqFISH is now achieved

## Application side

1. [Super-resolution chromatin structure imaging](https://science.sciencemag.org/content/362/6413/eaau1783)
2. [Dynamics and Spatial Genomics of the Nascent Transcriptome by Intron seqFISH](https://www.cell.com/cell/fulltext/S0092-8674(18)30647-0)
3. [Allele-specific RNA imaging shows that allelic imbalances can arise in tissues through transcriptional bursting](https://journals.plos.org/plosgenetics/article?id=10.1371/journal.pgen.1007874)


# Paper note

[Identification of spatially associated subpopulations by combining scrNAseq and sequential fluorescence in situ hybridization data](https://www.nature.com/articles/nbt.4260)

## Cell type mapping

Publicly available scRNAseq matching the tissue region -> 8 major cell types -> select a set of differentially expressed gene (feature selection) to classify cell types -> apply to seqFISH data -> spatial distribution of cell type (fig1e)

Sanity check: qq-plot of quantile normalized (? CHECK for detail?) seqFISH count and scRNAseq count

Result check:

1. Compare expression pattern of known marker gene for cell types in scRNAseq and the pattern in seqFISH with cell types assigned by the method (fig1c)
2. Correlation of matched cell type average profile in scRNAseq and seqFISH (fig1d)
3. Make use of the special staining of astrocytes and see if the staining agree with seqFISH assignment

## Identify multicellular niche

Some gene expression variation is due to spatial difference, so that it is spatially smooth. Define multicellular niche as spatially continuous region where micro-environment induced variation is small within the region and the variation is large between the regions.

Use spatial coherent genes (top 80 out of 125) -> filter out cell-type specific one (remove 11) -> fit HMRF (9 domains, fig2c)

Detailed steps:

1. Neighbourhood: the cut off is chosen such that each cell has roughly five neighbours
2. Gene selection: $\delta_g = \frac{1}{|L_1|} \sum_{s_i \in L_1} \frac{m_i - n_i}{\max(m_i, n_i)}$, $L_1$ highly expressed cells, $m_i$ average 'distance' to $L_0$, $n_i$ average 'distance' to $L_1$. Significance of $\delta_g$ is done by permutation.
3. Select the number of clusters: gap statistic, $gap(k) = \mathbb{E}[\log W_k] - \log W_k$, $W_k$ is the sum of average within cluster squared distance. Expectation is taken under the null (no cluster structure; obtained from Bootstrap). Criteria: smallest $k$ such that $gap(k+1) - sd(k+1) < gap(k)$ (namely no significant increase). Select $k$ by gap statistic under k-means
4. Implementation: EM algorithm (well studied in image segmentation). Assignment is given by MAP.

Check: Resulting pattern is independent to cell-type. figS3 (tSNE on the same set of genes, spatial pattern show up but not cell type pattern)

Gene expression signature of each domain: fig2d

Check: Spatial marker is consistent with ABA ISH (figS8) and different from cell-type specific marker (figS6,7)

meta-gene and figS9 confusing!

Conclusion: there is spatial structure in seqFISH data.

## Within cell type heterogeneity

Focus as glutamatergic neuron. Cell type marker profile is the same for all domains but meta-gene profile is not (fig3a). Between domain DEG on glutamatergic neuron identifies a gene sets which are specifically expressed spatially.

Morphological features are associated with domain, which is not by cell type.

Conclusion: the spatial difference is one source of the heterogeneity within cell type

## Re-analyze scRNAseq data

Focus on glutamatergic neuron, assign spatial domain using meta-gene identified from seqFISH, tSNE suggests consistent result as seqFISH (fig4a,b).

Identify more domain-specific gene within enlarged gene sets in scRNAseq (DEG). And gene set enrichment analysis. (fig4d)

Can spatial domain be explained or explain cell type subpopulation? No, in two-fold (fig14,16):

0. They share some gene markers, **BUT**
1. Within sub-population, cells are assigned to different domains
2. Within domain, cells are assigned to different sub-populations

## Domain-specific variation in astrocytes

Astrocyte markers are found to be expressed in domains-specific manner.

## Summary in brief

Single-cell technology is to profile and unreveal the heterogeneity of cells.

HMRF is the tool to uncover spatial structure and identify markers.

The application of HMRF needs not to be single-cell
