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
$ cut -f 3 fang_et_al_genotypes.txt | uniq -c | wc -l
$ cut -f 3 fang_et_al_genotypes.txt | uniq -c | sort -n

```

By inspecting this file I learned that:

1. At the first of the file, it would be the names of the genes/makers, and I press j to down the lines at a time, then find the following is the nucletides genotype. 
2. There are 2783 lines in this 11 Mb file. 
3. Using cut command to have a glimpse of the file by columns, find out the first three columns are the Sample ID, JG_OTU, and Group. Also, if use column command, it would be easier to read by columns.
4. Using awk command to find out how many columns. There are 986 columns in this file. Also, it doesn't have # headers, so head/tail command has the same results.
5. There are 16 different kinds of groups in column 3.
6. The least number of the group is ZMXNT, which is 4. The largest number of the group is ZMMLR, which is 1256.


### Attributes of `snp_position.txt`

```
$ wc -l snp_position.txt
$ tail snp_position.txt | awk -F "\t" '{print NF; exit}'
$ ls -lh snp_position.txt
$ cut -f 1-4  snp_position.txt |column -t | head -n 5
$ cut -f 3 snp_position.txt | sort | uniq -c | sort -n

```

By inspecting this file I learned that:

1. There are 984 rows and 15 column in this 81k file.
2. The first 4 columns are SNP_ID, cvv_marker_id, Chromosome, and Position.
3. Chromosome 1 has the most SNP marker, which is 155. The interesting thing is that chromosome 5 has 122 makers which is in 3rd more than chromosome 3 and 4. Also, there are 2 categories are unknown and multiple, the number of SNP in which are 27 and 6, respectively.




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