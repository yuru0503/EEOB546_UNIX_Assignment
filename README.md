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

1. At the first of the file by `less` command, it would be the names of the genes/makers, and I press j to down the lines at a time, then find the following is the nucletides genotype. 
2. There are 2783 lines in this 11 Mb file. 
3. Using `cut` command to have a glimpse of the file by columns, find out the first three columns are the Sample ID, JG_OTU, and Group. Also, if use `column` command, it would be easier to read by columns.
4. Using `awk` command to find out how many columns. There are 986 columns in this file. Also, it doesn't have # headers, so `head/tail` command has the same results.
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

### Spliting origanal data to maize and teosinte Data
```
$ cut -f 1,3,4 snp_position.txt | sort -k1,1 > snp_infor.txt
$ grep -E "(ZMMIL|ZMMLR|ZMMMR|Group)" fang_et_al_genotypes.txt | cut -f 1,4-986 |awk -f transpose.awk  > maize_genotype.txt
$ sed 's/Sample_ID/SNP_ID/' maize_genotype.txt | sort –k1,1 > maize_sgenotype.txt
$ grep -E "(ZMPBA|ZMPIL|ZMPJA|Group)" fang_et_al_genotypes.txt | cut -f 1,4-986 |awk -f transpose.awk > teosinte_genotype.txt  
$ sed 's/Sample_ID/SNP_ID/' teosinte_genotype.txt | sort -k1,1 > teosinte_sgenotype.txt
```
* `cut` command is to extract the column we need and 'sort' by first column to be a new SNP information file.
* `grep` command is to search for the certain names of the maize types in the group, then `cut` their ID and genotypes, then use the tranpose.awk function to transpose the file, and then to save as `maize_genotype.txt` file.
* `sed` command is to change the header in genotype file to the same with SNP file, then `sort` it by the 1st column to save as `maize_sgenotype.txt` file, so that we can join them later.
* Do the same coding for the teosinte data as well. 


### Combind genotype data with SNP information data

```
$ join -1 1 -2 1 -t $'\t' snp_infor.txt maize_sgenotype.txt > maize_joint.txt
$ join -1 1 -2 1 -t $'\t' snp_infor.txt teosinte_sgenotype.txt > teosinte_joint.txt
```
* `join` command is to combine the 1st column of `snp_infor.txt` and the 1st column of `maize_sgenotype.txt`, `-t $'\t'` option is to use tab as field seperator.
* Do the same coding for the teosinte data as well.


### Maize Data

```
$ for i in {1..10} ; do (awk '$1 ~ /SNP/' maize_joint.txt && awk '$2 == '$i'&& $3 != "multiple"' maize_joint.txt) > maize_chr$i.txt ; done
$ (awk '$1 ~ /SNP/' maize_joint.txt && awk '$3 == "unknown"' maize_joint.txt )> maize_unknown.txt
$ (awk '$1 ~ /SNP/' maize_joint.txt && awk '$2 == "multiple" || $3 == "multiple"' maize_joint.txt )> maize_multiple.txt
$ for i in {1..10}; do (head -n 1 maize_chr$i.txt && tail -n +2 maize_chr$i.txt | sort -k3,3n )> SNP_increase_maize_chr$i.txt ; done
$ for i in {1..10}; do (head -n 1 maize_chr$i.txt && tail -n +2 maize_chr$i.txt | sort -k3r,3n ) | sed 's/?/-/g' > SNP_decrease_maize_chr$i.txt ; done

```

* `for` command is to have a loop for 10 chromosomes, and then new file saved as maize_chr$i.txt , in which i is the number of chromosome.
* `awk` command is to print out SNP and then print out the records which feature pattern that field 2 is the same with value of i but without multiple in field 3 for each of chromosomes.
* Using `awk` to extract the specific pattern of unknown and multiple position markers to save as two new files as well.
* Using `for` to get a loop to `sort` SNP position in each of chromosomes. 




### Teosinte Data

```
$ for i in {1..10} ; do (awk '$1 ~ /SNP/' teosinte_joint.txt && awk '$2 == '$i' && $3 != "multiple"' teosinte_joint.txt) > teosinte_chr$i.txt ; done
$ (awk '$1 ~ /SNP/' teosinte_joint.txt && awk '$3 == "unknown"' teosinte_joint.txt )> teosinte_unknown.txt
$ (awk '$1 ~ /SNP/' teosinte_joint.txt && awk '$2 == "multiple" || $3 == "multiple" ' teosinte_joint.txt) > teosinte_multiple.txt
$ for i in {1..10}; do (head -n 1 teosinte_chr$i.txt && tail -n +2 teosinte_chr$i.txt | sort -k3,3n )> SNP_increase_teosinte_chr$i.txt ; done
$ for i in {1..10}; do (head -n 1 teosinte_chr$i.txt && tail -n +2 teosinte_chr$i.txt | sort -k3r,3n ) | sed 's/?/-/g' > SNP_decrease_teosinte_chr$i.txt ; done

```

* As it has been divided the genotype data processed into two `maize_joint.txt` and `teosinte_joint.txt` file. Therefore, it did the same command lines for `teosinte_joint.txt` to 22 files in according to the SNP postion in the each of chromosomes.  



### Genotype folders for maize and teosinte data

```
$ mkdir Maize_data Teosinte_data
$ mv SNP_increase_maize* SNP_decrease_maize* maize_m* maize_u* Maize_data/
$ mv SNP_increase_teosinte* SNP_decrease_teosinte* teosinte_m* teosinte_u* Teosinte_data/
$ mkdir Processed_file
$ mv maize* teosinte* Processed_file/

```
* `mkdir` command to have new directories.
* `mv` command to move the files to the directories

