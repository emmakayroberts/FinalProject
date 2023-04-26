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
Mafft aligns DNA, RNA or protein sequences by aligning sequences pairwise and then combining them similarly to ClustalW. Mafft is said to be more efficient, scalable, and accurate than ClustalW though. Mafft was chosen given it is similar to ClustalW but thought to be just a bit better, and for this reason the trees being displayed later will be based around alignments derived from Mafft. However Mafft does still have limitations such as requiring computational time for large datasets, limited user support, and sensitivity to input sequences. Mafft also makes the following assumptions: no recombination events have taken place and that there are no indels in homologous regions. The Mafft software also includes some user choices. The user can alter specific alignment modes, gap penalties, scoring matrices, and sequence weightings (Katoh, 2002). 

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
ClustalW works to align DNA, RNA or protein sequences by aligning sequences pairwise and then combining them. ClustalW requires the formation of a guide tree which aids in ordering the pairwise alignments. ClustalW was chosen partially due to the inability to properly download TCoffee and MUSCLE, but also because ClustalW is known to be fast, accurate, consistent, and easy to use. ClustalW does have some limitations though such as fewer customization options and the tendency to create gaps in the alignment. It factors in several assumptions that must be taken into consideration. ClustalW assumes that: the sequences are homologous, evolutionary events occur independently at each site, positions in the alignment have the same evolutionary importance, and sequence’s similarities are proportional to their evolutionary distance. The ClustalW software also includes some user choices. The user can alter the scoring matrix or gap penalties, and the user can choose a single or multiple iteration method (Thompson, 2004), (Larkin 2007). 


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
For distance, the software R package ape was used. Ape (Analysis of Phylogeneics and Evolution) is one of the most widely used phylogenetic softwares and has a large variety of functions. Ape useful as it is user friendly, free to all, customizable, and powerful. Ape was chosen as the distance software because it is the most widely used and there was no exposure to choose a different method for this section. Distance methods, including Ape, do have limitations such as the sole reliance on pairwise distance calculations so aspects like substitutions and mutations can have a lack of consideration. Additionally, the trees can only come out as good as the alignment, so if the alignment quality is poor, that poor quality will perpetuate through here. A big user choice for Ape and distance methods is the chosen distance metric, which can be a limitation if chosen poorly. Commonly used distance metrics include Jukes-Cantor, Kimura distance, and more. Lastly, distance methods are based on the assumption of the molecular clock. This hypothesis assumes that DNA and protein sequence evolve at a relatively constant rate over time which can often be untrue based on a variety of biological or environmental events. (Jombart, 2008). 
For parsimony, the software R package Phangorn was used. Phangorn is another widely used phylogenetic software with a huge variety of functions. Phangorn can be useful for parsimony analyses as it is flexible, efficient, and includes functions to help visualize and interpret trees and results. Phangorn was chosen as the parsimony software because it is widely used and there was no exposure to choose a different method for this section. Phangorn does have some limitations. It can be computationally time consuming especially with large datasets. Additionally like with the distance methods, the quality here can only be as good as the quality of the initial alignment. Users of Phangorn can choose to alter weighing schemes, set the number of bootstrap replicates, and more that gets into specific attributes of speed, accuracy and complexity. There are a few essential assumptions that should be taken into account with parsimony methods and Phangorn. It is assumed that sequences change in a stepwise fashion. It is also assumed that sequences have a common origin and haven’t occurred because of parallel or convergent evolution. Additionally, it is assumed that different areas of the sequence evolved independently from other areas, however this can occur biologically. (Jombart, 2008). 

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





08APR2023 - Bayesian 

Bayesian inference is a method of phylogenetic analysis that allows for the estimation of posterior probability distribution of trees based on the data and evolution model used. Bayesian methods also require the definition of a prior, a probability distribution that reflects the user's idea about the parameters of interest. The Bayesian software used was MrBayes. MrBayes was chosen because it was extremely user friendly and commands were given for us to run MrBayes with. MrBayes “uses Markov Chain Monte Carlo (MCMC) techniques to sample from the posterior probability distribution.” (Ronquist and Huelsenbeck 2003) Some strengths of MrBayes are that it is highly customizable, and allows users to set heating parameters for heated chains, choose between most commonly accepted models of nucleic acid evolution, and even unlink branch length and topology parameters. Potential limitations of MrBayes could be computational intensity, sensitivity to priors, and convergence and mixing issues, especially for complex datasets. Additionally there are several assumptions that should be taken into account when looking at results from MrBayes. MrBayes assumes that sequences follow the Markov process, that the tree is binary, the prior is appropriate, and that the rate of mutations is constant throughout the tree. 


MrBayes Commands: 
Download
brew tap brewsci/bio
brew install mrbayes
Put all data in nexus files
Create a mrbayes block. Do this in a separate text file. Note that we need to add mcmc;sumt; at the end so that the mb block is executed. mcmc is the command to run MCMC and sumt is the command to obtain a summary tree.
begin mrbayes;
 set autoclose=yes;
 prset brlenspr=unconstrained:exp(10.0);
 prset shapepr=exp(1.0);
 prset tratiopr=beta(1.0,1.0);
 prset statefreqpr=dirichlet(1.0,1.0,1.0,1.0);
 lset nst=2 rates=gamma ngammacat=4;
 mcmcp ngen=10000 samplefreq=10 printfreq=100 nruns=1 nchains=3 savebrlens=yes; outgroup Anacystis_nidulans;
 Mcmc;
 Sumt;
end;
append the MrBayes block to the end of the nexus file with the dat
cat DATAFILE.nex MBBLOCKFILE.txt > DATAFILE-mb.nex (insert names of “data” and “MbBlock files” given the names)
run MrBayes:
mb DATAFILE-mb.nex



25APR2023
THE COALESCENT

Astral is one of the methods to run a coalescent based tree. An advantage to Astral is that it can analyze datasets with up to 1000 taxa and 1000 genes with running times that are reasonable. Astral is also said to be more accurate than similar models and statistically consistent. A potential limitation here could be that the consistency of standard summary models assumes that the gene trees are estimated without error which can limit the relevance of consistency and is always something that is working to be improved upon. Some user choices for Astral are that the user has to provide bi-paritions from their input sets, as well as which form of Astral to run based on the data set size and makeup. 
Paper: https://academic.oup.com/bioinformatics/article/31/12/i44/215524#394334969 

There is no installation required to run ASTRAL.
Just download from the zip file found in this link: https://github.com/smirarab/ASTRAL/blob/master/README.md#installation 

We will next run ASTRAL on an input dataset. From the ASTRAL directory, run:

java -jar astral.5.7.8.jar -i DATASET.gene.tre
The results will be outputted to the standard output. To save the results in an output file use the -o option:

java -jar astral.5.7.8.jar -i DATADET.gene.tre -o DATASET.tre



Final Paper read here: https://docs.google.com/document/d/12s1UK4_YL6GjTi_nZyEpFVIObcDtmfgotJD5NQo-R6E/edit
