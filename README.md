# FinalProject
Final Project for Botany 563

Data Description:
    TAXA: 12 cats from the Pantherine lineage, 3 cats from the domestic lineage, and 2 cats from the outgroup
    GENES
        Nuclear genes: beta-Fibrinogen intron, IRBP exon, TTR intron
        Mitochondrial genes: ND2, ND4, 16SrRNA, ND5
    Sourced from Table 1 within: https://www.sciencedirect.com/science/article/pii/S1055790305000357?via%3Dihub#bib2
    QC completed


20Feb2023
Aligned Data today!
Used mafft for alignment
aligned all .txt files from Desktop/FinalProject/Data (without "output" in their title)
to align in the terminal:
    1. locate the working folder
    2. type in "mafft" and enter
    3. type in input file, for example, "ND5.txt"
    4. type in output file for example, "ND5 output.txt"
    5. selected "1" for "Clustal format" as output format
    6. selected "1" for "auto" as strategy
    7. left additional arguments blank
repeated for each gene .txt file


21Feb2023
Aligned Data today part 2!
aligned all .txt files from Desktop/FinalProject/Data (without "output" in their title)
to align in the terminal:
    1. locate the working folder
    2. type in "clustalw" and enter
    3. select "1"
    4. insert file name, make sure it contains ".txt" or it won't work
    5. select  "2" 
    6. select "1"
    7. type in output name
    8. enter until it is done! 
repeated for each .txt file




02Mar2023
Distance and Parsimony Trees
converted .txt files of aligned data to .fasta files from phylip to fasta format
used terminal with function cp "file_name.txt" "file_name.fasta"
Then can use following functions in R to obtain distance trees with .fasta file of data:
1) Installing necessary packages:
install.packages("adegenet", dep=TRUE)
install.packages("phangorn", dep=TRUE)

2) Loading the packages
library(ape)
library(adegenet)
library(phangorn)

3) Loading the sample data
dna <- fasta2DNAbin(file="http://adegenet.r-forge.r-project.org/files/usflu.fasta")

4) Computing the genetic distances. They choose a Tamura and Nei 1993 model which allows for different rates of transitions and transversions, heterogeneous base frequencies, and between-site variation of the substitution rate (more on Models of Evolution).
D <- dist.dna(dna, model="TN93")

5) Get the NJ tree
tre <- nj(D)

6) Before plotting, we can use the ladderize function which reorganizes the internal structure of the tree to get the ladderized effect when plotted
tre <- ladderize(tre)

7) We can plot the tree
plot(tre, cex=.6)
title("A simple NJ tree")

Then can use following functions in R to obtain parsimony trees with .fasta file of data:
1) Installing necessary packages (if you have not installed them for the distance section above)
install.packages("adegenet", dep=TRUE)
install.packages("phangorn", dep=TRUE)

2) Loading
library(ape)
library(adegenet)
library(phangorn)

3) Loading the sample data and convert to phangorn object:
dna <- fasta2DNAbin(file="http://adegenet.r-forge.r-project.org/files/usflu.fasta")
dna2 <- as.phyDat(dna)

4) We need a starting tree for the search on tree space and compute the parsimony score of this tree (422)
tre.ini <- nj(dist.dna(dna,model="raw"))
parsimony(tre.ini, dna2)

5) Search for the tree with maximum parsimony:
> tre.pars <- optim.parsimony(tre.ini, dna2)
Final p-score 420 after  2 nni operations

6) Plot tree:
plot(tre.pars, cex=0.6)