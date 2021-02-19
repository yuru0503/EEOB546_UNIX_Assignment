# UNIX Assignment

## Data Inspection

### Attributes of `fang_et_al_genotypes`

```
$ less fang_et_al_genotypes.txt
$ wc -l fang_et_al_genotypes.txt
$ ls -lh fang_et_al_genotypes.txt
$ cut -f1-8 fang_et_al_genotypes.txt | head -n 3
$ cut -f1-8 fang_et_al_genotypes.txt | column -t | head -n 3
$ tail fang_et_al_genotypes.txt | awk -F "\t" '{print NF; exit}'

```

By inspecting this file I learned that:

1. At the first of the file, it would be the names of the genes/makers, and I press j to down the lines at a time, then find the following is the nucletides genotype. 
2. There are 2783 lines in this 11 Mb file. 
3. Using cut command to have a glimpse of the file by columns, find out the first three columns are the Sample ID, JG_OTU, and Group. Also, if use column command, it would be easier to read by columns.
4. Using awk command to find out how many columns. There are 986 columns in this file. Also, it doesn't have # headers, so head/tail command has the same results.
5. 

or

* point 1
* point 2
* point 3

### Attributes of `snp_position.txt`

```
here is my snippet of code used for data inspection
```

By inspecting this file I learned that:

1. point 1
2. point 2
3. point 3

or

* point 1
* point 2
* point 3

## Data Processing

### Maize Data

```
here is my snippet of code used for data processing
```

Here is my brief description of what this code does


### Teosinte Data

```
here is my snippet of code used for data processing
```

Here is my brief description of what this code does