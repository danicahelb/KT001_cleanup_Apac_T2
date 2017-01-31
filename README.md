# KT001_cleanup_Apac_T2
cleanup time-point 2 data

```r
rm(list=ls())
setwd("C:\\Users\\helbd\\Documents\\Magic Briefcase\\LSHTM\\Kevin's array\\151118 data")
```

##########################################################################################
##Load array data:

```r
files <- sort(list.files(".\\Data\\Processed Data")[grep("import_data.RData", list.files(".\\Data\\Processed Data"))], decreasing=T)[1]
print(load(paste(".\\Data\\Processed Data\\", files, sep=""))) 
d1$spot[grep("std._", d1$spot)] <- gsub("std", "std0", d1$spot[grep("std._", d1$spot)])

temp.d1 <- d1[d1$study=="Apac_X2",]

#note that some samples were run 2x!
pasted <- unique(paste(temp.d1$slideN, temp.d1$sampleID, sep="_"))
dups <- gsub("^.+_", "", pasted)[duplicated(gsub("^.+_", "", pasted))]
slide_dups <- unique(gsub("_.+$", "", pasted)[duplicated(gsub("^.+_", "", pasted))])
temp.d1$sampleID[temp.d1$slideN==slide_dups] <- paste(temp.d1$sampleID[temp.d1$slideN==slide_dups], "b", sep="")
```

##########################################################################################
##create matrix of raw MEAN intensity values

```r
temp <- matrix(NA, nrow=length(unique(temp.d1$spot)), ncol=length(unique(temp.d1$sampleID)))
rownames(temp) <- sort(unique(temp.d1$spot))
colnames(temp) <- sort(unique(temp.d1$sampleID))

for(id in colnames(temp)){
  for(ag in rownames(temp)){
    print(id)
    print(ag)
    temp[rownames(temp)==ag, colnames(temp)==id] <- temp.d1[temp.d1$spot==ag & temp.d1$sampleID==id, "Raw.Mean"]
  }
}
X2_raw_mean <- temp
```

##########################################################################################
##create matrix of raw Median intensity values

```r
temp <- matrix(NA, nrow=length(unique(temp.d1$spot)), ncol=length(unique(temp.d1$sampleID)))
rownames(temp) <- sort(unique(temp.d1$spot))
colnames(temp) <- sort(unique(temp.d1$sampleID))

for(id in colnames(temp)){
  for(ag in rownames(temp)){
    print(id)
    print(ag)
    temp[rownames(temp)==ag, colnames(temp)==id] <- temp.d1[temp.d1$spot==ag & temp.d1$sampleID==id, "Raw.Median"]
  }
}

X2_raw_median <- temp
```

##########################################################################################
##save dara

```r
save(X2_raw_mean, X2_raw_median, file=".\\Data\\Processed Data\\0001.KTarray.Apac.cleaned_X2.RData")
```
