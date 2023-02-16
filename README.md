# gwaslite - Lightweight genome-wide association analysis process software
---
**<font size=2 >Current Version: 2.0 Release date: November 22, 2022</font>**

Gwaslite is written in Python, and the software is used to perform genome-wide association analysis on files after variant calling detection, including population evolution analysis and association analysis. Only the population VCF file and the population phenotype file are input, and the TREE file, population STRUCTURE plot, population PCA plot,population LDdecay plot, and two genome-wide association analysis methods can be produced.
# lastest version:gwaslite V1.2
---
- **Major updates**

  The new version adds the Mapping function and the function of selecting the best K value of the affinity matrix for correlation analysis, and a number of input parameters are changed：
  ## Installation
  The latest version is installed using docker, and the installation command is:
```bash
    docker pull radiomumm/gwaslite:v1.2
```
  ## Quick Start run
  View help information：
 ```bash 
    docker run radiomumm/gwaslite:v1.2 /software/run_BSA_analysis_v1.2/run_BSA_analysis -h
  ```
  ## Start the analysis：
  **<font color=red >Using the docker runtime requires a mount folder, and the file path in fq.list needs to be changed to the mounted format.**
  ```bash
    docker run -v /Server sample folder/example:/input -v /Server result folder/result:/result \
    radiomumm/gwaslite:v1.2 /software/run_BSA_analysis_v1.2/run_BSA_analysis \
    -v /input/fq.list \
    -p /input/p.txt \
    -s 12 -n rice_1000 -t Q \
    -o /result/ \
    -R /input/Oryza_sativa.IRGSP-1.0.dna.toplevel.fa \
    -T1 \"-Xmx20G\"
  ```
  ### Required
  - **-v: VCF fiile**, The content of the input file has been changed, and the software no longer supports gVCF format input, and has been changed to FASTQ file input.You need to enter a `list/txt` file containing a fixed format, the first column is the sample name, the second column is the sequencing library type(Paired-end`PE`，Single-read`SE`), the third column is the FASTQ1 path of PE/SE type, the fourth column is the FASTQ2 path of PE type, and the SE type does not need to be filled in:.
                                                <table>
                                              <tr>
                                                <td>sample1</td>
                                                <td>PE</td>
                                                <td>sample1_1.fastq</td>
                                                <td>sample1_2.fastq</td>
                                              </tr>
                                              <tr>
                                                <td>sample2</td>
                                                <td>SE</td>
                                                <td>sample2.fastq</td>
                                                <td>  </td>
                                              </tr>
                                              <tr>
                                                <td>...</td>
                                                <td>...</td>
                                                <td>...</td>
                                                <td>...</td>
                                              </tr>
                                              </table>
  - **-s: species_chr**,Changing the speciesname of the previous version to the number of chromosomes of the species needs to be consistent with the number of reference genomes for the input parameter `-R`.
  - **-R: Reference genome**,Input species reference genome sequence files, including fa, fna, and fasta formats.
  ### Optional
  - **-q: q_value**, A threshold for screening significant SNP sites in the range of [0,+∞]. The given default value is 1/SNP number of bits.
  ### Output
  - **mapping dir:** Stores the result file after BWA software mapping.
  - **qc dir** Stores the SOAPnuke software quality inspection result file.
  
# version:gwaslite V1.0
---
## Installation
---
- **Support data**

   Gwaslite is designed to analyze population VCF files obtained by sequencing methods such as WGS, WES, RNA-seq, etc.

- **Script environment**

  [R](https://www.r-project.org/) >  = 4.2.0
   
   - [r-ggplot2](https://faculty.washington.edu/browning/beagle/beagle.html)
    
   - [r-CMplot](https://faculty.washington.edu/browning/beagle/beagle.html)
 
  [python3](https://www.python.org/download/releases/3.0/)

  [plink](https://zzz.bwh.harvard.edu/plink/)

  [admixture](http://dalexander.github.io/admixture/)

  [PopLDdecay](https://github.com/BGI-shenzhen/PopLDdecay)
  
  [tassel](https://tassel.bitbucket.io/)

  [Phylip](https://evolution.genetics.washington.edu/phylip.html)
  
  [emmax](http://csg.sph.umich.edu/kang/emmax/download/index.html)
  

  
  Before installing gwaslite, you need to install the above software and ensure that the software runs normally.

- **lcWGSbox does not require additional installation**
```bash
tar -xvf gwaslite.tar
cd /gwaslite
chmod 775 run_gwaslite
```
After the installation is successful, you can start the analysis using the `run_gwaslite` script in the installation folder.

You can use the `-h` command to check whether the script is installed successfully.
```bash
./run_gwaslite -h
```

## Quick start run
---

The example files are stored in the `example` under the installation folder.A quick test on example data can be performed using:


 **step1. Update the config file**

Enter the locally installed software installation path and reference genome path into the `config` configuration file, and if the software is installed by `conda`, you need to fill in the software name in the path field.

 **step2. Run the script**
```bash 
 # test on dog data ,no covariance is used
./run_lcWGS  -v  /example/re.pruned_coatColor_maf_geno.vcf  -p  new_coatColor.pheno.txt  -s  dog  -t  noQ   -o  /text/ 
 # test on dog data ,use the Q matrix as the covariance
./run_lcWGS  -v  /example/re.pruned_coatColor_maf_geno.vcf  -p  new_coatColor.pheno.txt  -s  dog  -t  Q   -o  /text/ 
 ```
 After successful running, the corresponding file is generated in the result folder.
 
## Options and help
---
```bash
run_gwaslite -h
```
### Required
- **-v: VCF fiile**, Population VCF file after variant detection, the sample information in this file needs to correspond to the sample in the phenotypic file.
- **-p: phenotypic files**, Phenotypic files, phenotypes to be analyzed in genome-wide association analysis. The table has no header and is divided into two columns, the first column is the sample name and the second column is the phenotype value. It is important to note that sample names cannot contain symbols such as `'/', '', '_', '-'`, etc.

<table >
  <tr>
      <td>Bombilla</td>
      <td>43270923</td>
  </tr>
  <tr>
      <td>Sipirasikkam</td>
      <td>35937250</td>
  </tr>
  <tr>
      <td>...</td>
      <td>...</td>
  </tr>
  </table>
  
- **-s: species_name**,Species information is required When using Plink software, this parameter provides species chromosome number information, and only supports a small number of species, such as rice, dogs, etc.
- **-o: output file path**, the output file is stored in a folder, and the script will create a `projectname_result` folder under the input path, and the result file will be stored in this folder. If you run the script continuously, the `projectname_result` folder is overwritten.
### Optional
- **-t: emmax_type**, Genome-wide association analysis using emmax software, this parameter provides the option to use the Q matrix as the covariance, currently the Q matrix is not used by default, and only the `Q3 matrix` information will be associated when using the Q matrix.
### Output
- **filter dir:** It is used to store the SNP filtered result file in the VCF，the filter parameters are `geno 0.2` and `MAF 0.05`
- **gwas dir** It is used to store PLINK bfiles and genome-wide association analysis results.
- **LDdecay dir** Used to store population LDdecay analysis results
- **pca dir** Used to store population PCA analysis results
- **structure dir** Used to store population STRUCTURE analysis results
- **tree dir** Used to store evolutionary tree analysis results，the result file is `outtree.nwk`


## Bug reports
---
For more detailed questions or other concerns please contact radiomumm 
[zhushuliang@genomics.cn]()