## Performing basic operations on Data Frames

### Selecting Columns and Rows
Dataframe notation follows the following format:
```
# dataframe[rows,columns]
counts[1,]   # print first row, all columns
counts[,2]   # print second column, all rows
counts[,c("LNCaP")]   # also print second column, all rows but uses a name instead of index number
counts$LNCaP         # also print second column, all rows but uses shorthand notation
```

Try running: `counts[c("KLK3"),]`. Did it return what you expected? What are the row names for this dataframe (`rownames(counts)`), you can over-write the row names are try again:
```
rownames(counts) <- counts$Gene   # rewrite the row names so they match the values in the Gene column (side note row names must be unique)
`counts[c("KLK3"),]`     # print the first row, all columns
```

### Filtering Columns and Rows
Lets say you're only interested in genes that achieve a particular expression level, and want to filter your dataframe.
```
counts[counts$LNCaP > 100,]    # Return rows that have a read count greater than 100 in LNCaP, and every column
```

### Merging Dataframes
Outer join vs inner join.
Renaming Columns
using columns with different names

Ok, that's enough with the small examples, lets work with some real data. So we have a dataframe of BRD4-ChIP peaks from MR42D + MDV treated cells [here](https://ohsu.box.com/s/37e6vnkz1p54um3yutwqss2541pce294). We also have a dataframe of differentially expressed genes with [BRD4-Knockdown in MR42D](https://ohsu.box.com/s/xea7lcz848idtttiw808vabvmja5jnj8). What genes are BRD4-bound, and going down with siBRD4? I understand that seems like a big task, but lets break it down into steps:
1.   Download the Dataframes and import into R Studio
2.   





Previous: [Introduction to R](intro_to_r.md) | Next: [Dataframe Operations](dataframe_ops.md) |Top: [Course Overview](../index.md)
