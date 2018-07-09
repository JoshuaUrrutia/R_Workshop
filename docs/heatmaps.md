R does not natively support heatmaps. Luckily there are multiple R packages that support heatmap generation. One of my favorites is complex heatmap.
We will install the complex heatmap package from bioconductor. Bioconductor distrubites a variety of R packages that are associated with biology, the run the install type:
```r
source("https://bioconductor.org/biocLite.R")
biocLite("ComplexHeatmap")
```
Complex heatmap is now installed on your computer, and you won't need to install it again. However, each time you want to use complex heatmap, you will need to load it into you R enviornment. You can do this by typing:
```r
library(ComplexHeatmap)
```

Now, there are a lot of columns in our dataframe that we don't want in the heatmap. P-values, expression levels in individual samples. For the most part, we just want to save gene names and the log2FC values. You can drop columns similar to subsetting, just add a minus sign `-`.
```r
de_heatmap <- BRD4_DE_genes_with_peaks[,-c(1,3,5,7,9:21)]
```
Now all that's left is to generate the heatmap, we leave off the gene names so it only gets column names and values

```r
Heatmap(de_heatmap[,-1])
```

Great, but that's a little ugly and we do want to know the gene names. `ComplexHeatmap` automatically uses the row names as labels, so lets try resetting our rownames:
```r
rownames(de_heatmap) <- de_heatmap$Gene_Symbol
```
Hmm, it seems our row names are not unique. Can anyone figure out why?

We'll need to aggregate rows that have the same gene name, let's do this by taking the mean of the Log2FC values:
```r
de_heatmap <- aggregate(de_heatmap[, -c(1)], by = list(de_heatmap$Gene_Symbol), mean, na.rm = TRUE)
```

Now we can set the rownames, rename our columns to have shorter labels, and rescale the coloring on our heatmap so it is centered on 0:

```r
rownames(de_heatmap) <- de_heatmap$Gene_Symbol
colnames(de_heatmap) <- c("Gene_Symbol", "siBRD4", "siCREB1", "siEP300")
library(circlize)
Heatmap(de_heatmap[,-1], col = colorRamp2(c(-2, 0, 2), c("blue", "white", "red")))
```
