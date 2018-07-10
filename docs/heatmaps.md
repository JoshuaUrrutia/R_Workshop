# Heatmaps in R

R does not natively support heatmaps. Luckily there are multiple R packages that support heatmap generation. One of my favorites is complex heatmap.
We will install the complex heatmap package from bioconductor. Bioconductor distrubites a variety of R packages that are associated with biology, to run the install type:
```r
source("https://bioconductor.org/biocLite.R")
biocLite("ComplexHeatmap")
```
Complex heatmap is now installed on your computer, and you won't need to install it again. However, each time you want to use complex heatmap, you will need to load it into you R enviornment. You can do this by typing:
```r
library(ComplexHeatmap)
```

Now, let's import our `MR42D_MRD4_DE_Peaks` dataframe from yesterday. There are a lot of columns in our dataframe that we don't want in the final heatmap: p-values, expression levels in individual samples, ChIP peak regions, etc. For the most part, we just want to save gene names and the log2FC values. You can drop columns similar to subsetting, just add a minus sign `-`.
```r
de_heatmap <- MR42D_BRD4_DE_Peaks[,-c(2,3,5,7,9:39)]   # selects only the columns we want to save
de_heatmap <- unique(de_heatmap)    # removes rows that are exactly the same
```
Now all that's left is to generate the heatmap, we'll leave off the gene name column so we'll only pass the actual expression values:
```r
Heatmap(de_heatmap[,-1])
```

Great, but that's a little ugly and we do want to know the gene names. `ComplexHeatmap` automatically uses the row names as labels, so lets try resetting our rownames:
```r
rownames(de_heatmap) <- de_heatmap$Gene_Symbol
```
Hmm, it seems our row names are not unique. Can anyone figure out why?

```r
de_heatmap[de_heatmap$Gene_Symbol == 'SRSF8',]
siBRD4_DE[siBRD4_DE$Gene_Symbol == 'SRSF8',]
```

So it looks like some of the same genes have different ensemble ids (transcript ids), so we have different log2FC values for each transcript id. We'll need to aggregate rows that have the same gene name, let's do this by taking the mean of the Log2FC values. We'll use the aggregate function in R. The way aggregate works is
`aggregate(dataframe_to_aggregate, by = field_to_aggregate_on, function, na.rm = get_rid_of_na_values?)`
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

That's still too many genes to be able to read, so let's subset the `de_heatmap` dataframe to create a heatmap of only the genes that have a log2FC value of less than -0.5 in the siBRD4 condition:
```r
Heatmap(de_heatmap[de_heatmap$siBRD4 < -0.5,-1], col = colorRamp2(c(-1, 0, 1), c("blue", "white", "red")))
```

Now that's a nice looking heatmap!

Previous: [Dataframe Operations](dataframe_ops.md) | Next: [Advanced Integrations](advanced_integrations.md) |Top: [Course Overview](../index.md)
