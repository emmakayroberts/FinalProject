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



28Mar2023

I chose to use IQ-Tree for my project data set. IQ-Tree, according to Journal Article by Lam-Tung Nguyen, et al, is “a fast and sophisticated algorithm for finding ML trees. The core idea is to perform an efficient sampling of local optima in the tree space. Here, the best local optimum found represents the reported ML tree. To this end, we combine elements of hill-climbing algorithms, random perturbation of current best trees, and a board sampling of initial start trees.” 
IQ-Tree has several strengths for why I chose it: IQTree2 has a number of features that reduce computational time (Ultrafast Bootstrap approximation, improved quartet likelihood mapping, etc), IQTree samples several starting trees and randomly perturbs trees during the hill climbing process to prevent the ML hill-climbing algorithm from getting “stuck” in local optima, IQTree uses a fast NNI hill climbing algorithm, faster than traditional NNI hill climbing algorithms (hence the name); IQTree2 implements non-time-reversible models, allowing for inference of rooted phylogenies. Lastly, IQ-Tree is highly versatile, with a lot of additional functionality added in IQTree2. 
A few assumptions for IQ-Tree include: only samples local optima, assuming the top optima found represents the ideal tree; general time reversible model for nucleotide sequences; WAG (Whelan and Goldman) model for amino acids; and the assumed model can only generate unrooted trees, so IQ-TREE assumes alignments are always unrooted. 
Additionally there are a few limitations of IQ-Tree: it primarily uses fast NNI, which is thought to produce less accurate trees than SPR. Stochastic NNI perturbations stop after a maximum number of 100 perturbations. Even with random sampled initial trees and perturbations, there is no guarantee that the output phylogeny represents the global optimum. 

IQ-Tree Commands:

Downloaded IQ-Tree software. 
Confirmed successful installation: 
Opened Terminal
Go into IQ-TREE folder by entering:
 cd Downloads/iqtree-1.6.12-MacOSX
cd Dropbox/software/iqtree-1.6.12-MacOSXbin/iqtree -s NAMEOFFILE.phy
Repeated for all files