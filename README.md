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
aligned all .txt files from "Desktop/FinalProject/Data
to align in the terminal:
    1. locate the working folder
    2. type in "mafft" and enter
    3. type in input file, for example, "ND5.txt"
    4. type in output file for example, "ND5 output.txt"
    5. selected "1" for "Clustal format" as output format
    6. selected "1" for "auto" as strategy
    7. left additional arguments blank
repeated for each gene txt file