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

### Downsampling 

Downsampling is a technique commonly used to balance class distribution in a dataset. The goal is to equalize the number of samples for each class or category. If there's a large discrepancy in class sizes, models may become biased towards the majority class and perform poorly on the minority class, which can lead to misleading accuracy metrics. It's important to note that while downsampling can help mitigate class imbalance issues, it also discards potentially useful data.

We downsample by cluster size & the molecule count. 

**Cluster Size**
- Initialise 2 dictionaries to store a list for each cell type. These lists will contain the indices of cells & mol_count corresponding to that cell type. 
- Determine the smallest cluster size (cell type with the smallest number of cells).
- Randomly select cells from each cluster to match the smallest cluster size.
- Append the indexes of these randomly-selected cells across 9 cell types to 'all_random_indexes'
- Create a df 'downsampled_df' containing the gene exp. data from 'all_random_indexes'

**Molecule Count**
- Copy 'downsampled_df' into 'downsampled_mol'
- Make a list of the molecule count annotations for the randomly selected cells called 'mol_countDS'.
- Find the cell with the least number of molecules.
- Scale the gene expressions in each cell relative to the cell with the least number of molecules in 'downsampled_mol'.

2. Partitioning Data 

## Cross-validation

**Overview of CV**

The entire dataset is divided into 'k' equal parts or 'folds' (in this case k=10).
The model is trained k times. Each time, one of the k folds is used as the test set, and the remaining k-1 folds are combined to form the training set.
The performance of the model is then averaged over the k trials to provide a less biased estimate of its true performance.

**Steps**
- Define k, the number of folds and find fold size. 
- Shuffle 'all_random_indices'. 
- Initialise lists called 'folds_indices' and 'folds_data'. 
- Append each cell indice & corresponding cell data to lists 'temp_fold_indice' & 'temp_fold_data'. 
- Once fold_size is exceeded, append 'temp_fold_indice' to 'folds_indices' and 'temp_fold_data' to 'folds_data'. Clear lists 'temp_fold_indice' & 'temp_fold_data'. Append current cell to the next fold. 
- Add any remaining cells to the first fold. 

4. Defining Classifiers 
- MLP
- KNN
- RF
- SVM

6. Classification & Analysis
- Iterate through the folds & split into testing and training data accordingly
- Analysis metrics
  - Confusion Matrix: [truePositives, falsePositives, falseNegatives, trueNegatives]
  - Accuracy 
  - Sensitivity 
  - Specificity 
  - MCC

