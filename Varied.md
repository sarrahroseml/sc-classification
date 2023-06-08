## Objective 

<img width="575" alt="image" src="https://github.com/sarrahroseml/sc-classification/assets/133075789/b2f44405-ed96-45d0-8798-cc3749e5ee46">

This project aims to evaluate the effectiveness of 4 basic ML models (MLP, KNN, RF and SVM) on classifying RNA-Seq data into 9 different cell-types. They are: Thy1, Gad1, Tbr1, Spink8, Mbp, Aldoc, Aif1, Cldn5, and Acta2. This is a replication of an earlier project with adapted code [https://github.com/bradenkatzman/CellClassificationMachineLearning/blob/master/README.md]. 

Data Processing: 
A. Downsampling & Cross Validation 
B. Downsampling 
C. Cross Validation 
D. None


# A. Downsampling & Cross Validation 

## Steps Taken
1. Pre-processing

### Set-up
- Import required libraries for processing, numerical calculations, and random number generation.
- Open the raw RNA-seq data file.
- Read cell names from the first line of the file.
- Initialise a list called 'data', containing substrings for each cell. 

### Remove Lowly-Expressed Genes
- Read the list of gene expression values in each row. If the total read count for the gene (sum of gene expressions across all cells) > 25, add this gene's expressions to the corresponding cells in the data. If not, ignore this gene and increment a counter for the number of ignored genes.

### Obtaining data from annotations file 
- Open the annotations file.
- CELL_IDs: Split the 6th line (containing the cell_ids) into individual cell identifiers & remove the first 2 irrelevant 
- Mol_Counts: Split the 1st line into mol coounts
Downsample by Cluster Size:

Initialize counters for the 9 cell types.
Scan through the list of cell identifiers to count the number of cells for each cell type and record the indices of these cells.
Determine the smallest cluster size (cell type with the smallest number of cells).
Randomly select cells from each cluster to match the smallest cluster size.
Create a new data set with only these randomly selected cells.
Return the new data set and the indices of the randomly selected cells.
Downsample by Molecule Count:

Make a list of the molecule count annotations for the randomly selected cells.
Find the cell with the least number of molecules.
Scale the gene expressions in each cell relative to the cell with the least number of molecules.
Create a new data set with these scaled gene expressions.
Return the new data set.
2. Partitioning Data 
3. Defining Classifiers 
4. Classification & Analysis
