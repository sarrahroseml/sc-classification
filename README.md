# sc-classification

## Programming Specs 

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

6. **Data

 Partition** (`partitionData`):
   - This method simply prints a message indicating that data is being partitioned. The actual partitioning is presumably carried out by other methods not shown in this provided code.
