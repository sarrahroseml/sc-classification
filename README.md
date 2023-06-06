# sc-classification

## Programming Specs 

# Pre-processing RNA-Seq Data

1. **Load Raw Data:**
   - Import required libraries for processing, numerical calculations, and random number generation.
   - Open the raw RNA-seq data file. 
   - Read cell names from the first line of the file.
   - Read each subsequent line (representing a gene) and split the gene expressions per cell. 
   - Check whether the gene name corresponds to one of the 9 classifying genes. If it does, print a message.
   - If the total read count for the gene (sum of gene expressions across all cells) is greater than 25, add this gene's expressions to the corresponding cells in the data. If not, ignore this gene and increment a counter for the number of ignored genes.
   - Repeat these steps until all lines in the file have been read.
   - Return the list of cells with their respective gene expressions.

2. **Load Cell Identifier Annotations:**
   - Open the annotation file.
   - Skip the first line.
   - Split the second line into individual cell identifiers.
   - Remove unnecessary elements and ensure the number of identifiers matches the number of cells.
   - Return the list of cell identifiers.

3. **Load Molecule Count Annotations:**
   - Open the molecule count file.
   - Skip the first two lines.
   - Split the third line into individual molecule counts.
   - Remove unnecessary elements and ensure the number of molecule counts matches the number of cells.
   - Return the list of molecule counts.

4. **Downsample by Cluster Size:**
   - Initialize counters for the 9 cell types.
   - Scan through the list of cell identifiers to count the number of cells for each cell type and record the indices of these cells.
   - Determine the smallest cluster size (cell type with the smallest number of cells).
   - Randomly select cells from each cluster to match the smallest cluster size.
   - Create a new data set with only these randomly selected cells.
   - Return the new data set and the indices of the randomly selected cells.

5. **Downsample by Molecule Count:**
   - Make a list of the molecule count annotations for the randomly selected cells.
   - Find the cell with the least number of molecules.
   - Scale the gene expressions in each cell relative to the cell with the least number of molecules.
   - Create a new data set with these scaled gene expressions.
   - Return the new data set.
   
# Handling RNA-Seq Data 

**RNASeqData Class Specification**

The `RNASeqData` class performs operations on RNA sequence data. It assigns and manipulates data related to the different types of cells involved in the sequencing process.

1. **Initialization** (`__init__`):
   - When an object of the class is instantiated, it takes two arguments, `dataFilePath` and `cellAnnotationFilePath`.
   - The `dataFilePath` argument specifies the path to the file containing the RNA sequence data.
   - The `cellAnnotationFilePath` argument specifies the path to the file containing cell type annotations.
   - During initialization, the given data file and cell annotation file are loaded, and specific attributes are initialized.

2. **Downsampling data** (`downsampleData`):
   - It takes an integer argument `downsampleNumber` that represents the number of cells to be downsampled.
   - The downsampling process is carried out separately for each cell type. For each type, a random selection of cells is performed based on the provided downsample number.
   - After downsampling, the downsampled data and the corresponding cell identifiers are stored in class-wide variables.

3. **Partitioning data into training and testing sets** (`makeTrainingAndTestingData`):
   - It calculates the number of cells constituting 70% of the raw data and randomly selects this quantity of indices.
   - These indices are used to segregate the raw data into training and testing datasets.
   - Each index is checked, and the corresponding cell is added to either the training dataset or the testing dataset.
   - The cell identifiers for these cells are also added to the respective target value lists.
   - At the end of this method, the class will have defined class-wide variables containing the training data, testing data, and their respective target values. 
   - Finally, it prints the number of cells in the training and testing datasets and a reference for comparison.

4. **Partitioning data into folds for cross-validation** (`makeCrossValidationTrainingAndTestingData`):
   - Depending on the `downSampleFlag`, this method divides either the downsampled data or the raw data into ten equally sized folds.
   - Each fold will contain a tenth of the data and a corresponding key list of cell identifiers.
   - The indices of the data are randomly shuffled before dividing into folds.
   - It iterates through each index, adding the corresponding cell to the current fold until it reaches the fold size.
   - Once the fold is filled, it is added to the class-wide `folds` list and the `foldsKey` list.
   - The method then starts on a new fold until all the data has been assigned to a fold.
   - Any remaining cells and their corresponding keys are added to the first fold.
   - Finally, the `folds` and `foldsKey` lists are set as class-wide variables.

5. **Getter methods**:
   - Several getter methods are implemented to retrieve various class-wide variables.
   - These methods include: `getRawDataFileName`, `getAnnotationsFileName`, `getRandIndices`, `getRawData`, `getDSClusterData`, `getDSCluster_MoleculeData`, `getDSTrainingData`, `getDSTestingData`, `getDSTargetValues`, `getDSTestingDataTargetValues`, `getTrainingData`, `getTrainingDataTargetValues`, `getTestingData`, `getTestingDataTargetValues`, `getFolds`, `getFoldsKey`, `getCellIdentifierAnnotations`, `getMoleculeCountAnnotations`, `getNumCellsRaw`, `getNumCellsDSCluster`, `getNumGenesRaw`, `getNumGenesDSCluster`.

6. **Data Partition** (`partitionData`):
   - This method simply prints a message indicating that data is being partitioned. The actual partitioning is presumably carried out by other methods not shown in this provided code.
