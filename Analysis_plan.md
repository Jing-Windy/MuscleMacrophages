# Analysis Plan for Integrative Profiling of Muscle Macrophages

## Reference Dataset Construction
- **Data Source**
  - Macrophage-enriched datasets combined with macrophages selected from high-quality muscle atlas will be used in reference dataset.
  - GSE195507: Hindlimb CD11b+ cells in yound and old mice
  - GSE226173: Hindlimb F4/80hi cells in yound mice
  - GSE142480: Skeletal muscle-resident macrophages sorted by FACS in mouse quadriceps & diaphragm
  - Atlas datasets pending...

- **Expected Dataset Size**
  - ~20,000 currently

- **Work pipeline**
  - Dataset preprocessing
    - Datasets will first be filtered to remove RNA contamination, cells with low quality and doublets. Then clustering and annotation will be applied to each dataset manually to identify macrophages.
  - Macrophage annotation<br>
    Macrophages will be manually annotated with selected markers:<br>
    macrophage_markers <- c('Ptprc','Adgre1', 'Itgam', 'Csf1r')
  - Macrophage subtype annotation<br>
    Macrophage will be extracted (if needed), reclustered and manually annotated based on the top-expressed genes and existing framework.
  - Dataset integration
    - Extracted macrophage datasets will be then integrated into one dataset. Harmony algorithm will be used to remove batch effects. Louvain algorithm will be applied in clustering.
  - Macrophage subtype characterization
    - Each cluster will be characterized by its marker genes and enriched functional pathways. FindMarkers() will be used to search marker genes, and clusterProfiler package will be used for pathway enrichment analysis.

## Reference Transfer to Individual Datasets
- **Transfer method**
  - Label transfer from the reference dataset to query datasets will be performed using FindTransferAnchors() and TransferData() in Seurat. Data imputation for query cells will also be achieved via TransferData(), and the imputed expression values will be used to compute UMAP projections for the query datasets.

- **Transfer quality evaluation**<br>
  The quality of label transfer will be evaluated by 3 complementary criteria:<br>
  - The distribution of the label transfer probability across clusters in query dataset, visualized by FeaturePlot() in Seurat, or probability density functions (PDFs) calculated by basic R.
  - The proportion of each transferred label within each query cluster will be assessed. The dominant label can then be assigned to the corresponding cluster.
  - The accuracy and precision of label transfer in a ground-true benchmark dataset.

- **Manual adjustment**<br>
  An unsatisfying result can occur as a result of either ill parameter settings or limitations in reference dataset. The former can be fixed by optimizing the parameters in FindTransferAnchors() and TransferData(), while manual annotation will be applied to each query dataset to reveal the differences between query and reference.

