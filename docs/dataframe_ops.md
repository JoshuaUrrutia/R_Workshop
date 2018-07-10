## Performing basic operations on Data Frames

### Selecting Columns and Rows
Dataframe notation follows the following format:
`dataframe[rows,columns]`
```r
counts[1,]   # print first row, all columns
counts[,2]   # print second column, all rows
counts[,c("LNCaP")]   # also print second column, all rows but uses a name instead of index number
counts$LNCaP         # also print second column, all rows but uses shorthand notation
```

So we can pull out columns by name, but what about rows? Try running: `counts[c("KLK3"),]`. Did it return what you expected? What are the row names for this dataframe? Check with `rownames(counts)`. You can over-write the row names are try again:
```r
rownames(counts) <- counts$Gene   # rewrite the row names so they match the values in the Gene column (side note row names must be unique)
counts[c("KLK3"),]     # print the first row, all columns
```

### Filtering Columns and Rows
Lets say you're only interested in genes that achieve a particular expression level, and want to filter your dataframe. To filter the dataframe for genes that had counts higher than 100 in the LNCap cell line, you could enter:
```r
counts[counts$LNCaP > 100,]    # Return rows that have a read count greater than 100 in LNCaP, and every column
```
Or lets say your only interested in the expression of a handful of genes, you can filter for particular gene names in a column using the `%in%` function followed by a list of the genes you are interested in:
```r
counts[counts$Gene %in% c("KLK3","AR"),]
```
This command searches the `$Gene` column of the counts dataframe, and only returns rows where the gene name is equal to KLK3 or AR.

### Merging Dataframes
In many cases it is useful to merge dataframes by a common column. You can perform either an inner join (only save what's in common between two tables), or an outer join (all values from both tables are saved). Let's create a new dataframe of counts, we'll call it counts2:
```r
h <- c("POU3F2","AR","CHGA","SYP") # vector of gene names
i <- c(500,0,200,400)    # vector of gene counts for PC3
j <- c(20,100,0,300)    # vector of gene counts for 22RV
counts2 <- data.frame(h,i,j)  # combines these three vectors into a dataframe
names(counts2) <- c("Gene","PC3","22RV") # Changes column names to a descriptive name
```

Now lets merge our two counts dataframes:

```r
merge(counts,counts2)      # inner join
merge(counts,counts2,all=TRUE)    # outer join
merge(counts,counts2,all.x = TRUE)   # inner join preserving counts rows
merge(counts,counts2,all.y = TRUE)  # inner join preserving counts2 rows
```


Ok, that's enough with the small examples, lets work with some real data. So we have a dataframe of BRD4-ChIP peaks from MR42D + MDV treated cells [here](https://ohsu.box.com/s/37e6vnkz1p54um3yutwqss2541pce294). We also have a dataframe of differentially expressed genes with [BRD4-Knockdown in MR42D](https://ohsu.box.com/s/xea7lcz848idtttiw808vabvmja5jnj8). What genes are BRD4-bound, and are differentially expressed with siBRD4? I understand that seems like a big task, but lets break it down into steps:
1.   Download the Dataframes and import into R Studio
2.   Filter BRD4 peaks for +/- 5kb from TSS
3.   Filter siBRD4 genes for significance (p-value < 0.05)
4.   Merge the two dataframes


Importing into R Studio

```r
MACS2_unique_MDV_BRD4_3_peaks.narrowPeak_anno <- read.delim("~/Downloads/MACS2_unique_MDV_BRD4_3_peaks.narrowPeak_anno.txt")

`siBRD4.&.siCREB1.&.siEP300.&.siNTC_DESeq2` <- read.delim("~/Downloads/siBRD4 & siCREB1 & siEP300 & siNTC_DESeq2.txt")
```
Filter BRD4 peaks for +/- 5kb from TSS
```r
BRD4_Peaks <- MACS2_unique_MDV_BRD4_3_peaks.narrowPeak_anno[MACS2_unique_MDV_BRD4_3_peaks.narrowPeak_anno$Distance.to.TSS < 5000,]
BRD4_Peaks <- BRD4_Peaks[BRD4_Peaks$Distance.to.TSS > -5000,]
```
Filter for significantly de genes in siBRD4
```r
siBRD4_DE <- `siBRD4.&.siCREB1.&.siEP300.&.siNTC_DESeq2`[`siBRD4.&.siCREB1.&.siEP300.&.siNTC_DESeq2`$padj..siBRD4.siNTC. < 0.05,]
```
Merge the datasets

```r
BRD4_DE_Peaks <- merge(siBRD4_DE, BRD4_Peaks, by.x = 'Gene_Symbol', by.y = 'Gene.Name')
```

Congrats, `BRD4_DE_Peaks` is a list of genes are BRD4-bound, and differentially expressed with siBRD4!

R objects don't get saved automatically, so if you want to save this table for later we'll need to export the R object as a .txt or .csv file.
 You can do this with the `write.table` command, write.table is formatted:
`write.table(object_name, 'filename.txt', row.name = FALSE, col.names = TRUE, sep = '\t')`
```r
write.table(BRD4_DE_Peaks, "MR42D_BRD4_DE_Peaks.txt", col.names = TRUE, row.names = FALSE, sep = '\t')
```
col.names and row.names are TRUE if you want to save the row/column name, and false if you want to drop the names in the output file. The `sep = '\t'` argument, tells R to export the object as a tab-separated text file.

Next we'll create a heatmap of these genes.

<!---
Now let's take a look to see how many genes are duplicated:
```r
> length(BRD4_Peaks$Gene.Name)
[1] 5191
> length(unique(BRD4_Peaks$Gene.Name))
[1] 4363
```
<!---
```
peak_list<-unique(BRD4_Peaks$Gene.Name)
BRD4_DE_genes_with_peaks <- siBRD4_DE[siBRD4_DE$Gene_Symbol %in% peak_list,]
```
--->





Previous: [Introduction to R](intro_to_r.md) | Next: [Heatmaps](heatmaps.md) |Top: [Course Overview](../index.md)
