# sc-classification (WIP)

This project is a replication of: https://github.com/bradenkatzman/CellClassificationMachineLearning

## Programming Specs 

# Pre-processing RNA-Seq Data

1. **Load Raw Data:**
   - Import required libraries for processing, numerical calculations, and random number generation.
   - Open the raw RNA-seq data file. 
   - Read gene names from the first line of the file.
   - Read each subsequent line (representing a cell) and split the gene expressions per cell. 
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


# Defining Classifiers

**Neural Network (MLP from sklearn)*

**Global Declarations:**
- An MLPClassifier named 'mlpClf' is initialized globally with predefined hyperparameters such as activation function, solver algorithm, regularization parameter (alpha), batch size, learning rate, hidden layer sizes, maximum iterations, momentum, tolerance for optimization, validation fraction, and many others.

**Functions:**

1. **fitTrainingData(training_data, nSamples)**
   - **Input:** 
     - 'training_data': A 2D list where each row represents a sample (cell) and each column represents a feature (gene expression).
     - 'nSamples': A 1D list of labels or classifiers corresponding to each sample in 'training_data'.
   - **Process:**
     - Converts 'training_data' and 'nSamples' into NumPy arrays.
     - Calls the 'fit' method of 'mlpClf' to train the model on 'training_data' and 'nSamples'.
   - **Output:** 
     - No explicit output, but the 'mlpClf' object is fitted to the data, ready to make predictions.

2. **predictTestData(testing_data)**
   - **Input:** 
     - 'testing_data': A 2D list where each row represents a sample (cell) to be classified.
   - **Process:**
     - Converts 'testing_data' into a NumPy array.
     - Calls the 'predict' method of 'mlpClf' to predict the class labels for 'testing_data'.
   - **Output:**
     - 'predicted': A NumPy array of the predicted class labels for 'testing_data'.

**Random Forest Classifiera**

**Global Declarations:**
- A RandomForestClassifier named 'rfClf' is initialized globally with a predefined parameter, 'n_estimators', which indicates the number of trees in the forest.

**Functions:**

1. **fitTrainingData(training_data, nSamples)**
   - **Input:** 
     - 'training_data': A 2D list where each row represents a sample (cell) and each column represents a feature (gene expression).
     - 'nSamples': A 1D list of labels or classifiers corresponding to each sample in 'training_data'.
   - **Process:**
     - Converts 'training_data' and 'nSamples' from lists to NumPy arrays.
     - Calls the 'fit' method of 'rfClf' to train the model on 'training_data' and 'nSamples'.
   - **Output:** 
     - Does not return an explicit output, but it does train the 'rfClf' object with the provided data, readying it for prediction.

2. **predictTestData(testing_data)**
   - **Input:** 
     - 'testing_data': A 2D list where each row represents a sample (cell) to be classified.
   - **Process:**
     - Converts 'testing_data' from a list to a NumPy array.
     - Calls the 'predict' method of 'rfClf' to predict the class labels for 'testing_data'.
   - **Output:**
     - Returns 'predicted': A NumPy array of the predicted class labels for 'testing_data'.

**K-Nearest Neighbors (KNN) Classifier**

**Global Declarations:**
- A KNeighborsClassifier named 'knn' is initialized globally with default parameters.

**Functions:**

1. **initializeKnn(n_neighbors)**
   - **Input:** 
     - 'n_neighbors': The number of neighbors to use by default for k_neighbors queries.
   - **Process:**
     - Reinitializes the global 'knn' with specified parameters including the 'n_neighbors' parameter passed as an argument.
   - **Output:** 
     - Does not return an explicit output, but it does reconfigure the 'knn' object with the provided 'n_neighbors'.

2. **fitTrainingData(training_data, nSamples)**
   - **Input:** 
     - 'training_data': A 2D list where each row represents a sample (cell) and each column represents a feature (gene expression).
     - 'nSamples': A 1D list of labels or classifiers corresponding to each sample in 'training_data'.
   - **Process:**
     - Converts 'training_data' and 'nSamples' from lists to NumPy arrays.
     - Calls the 'fit' method of 'knn' to train the model on 'training_data' and 'nSamples'.
   - **Output:** 
     - Does not return an explicit output, but it does train the 'knn' object with the provided data, readying it for prediction.

3. **predictTestData(testing_data)**
   - **Input:** 
     - 'testing_data': A 2D list where each row represents a sample (cell) to be classified.
   - **Process:**
     - Converts 'testing_data' from a list to a NumPy array.
     - Calls the 'predict' method of 'knn' to predict the class labels for 'testing_data'.
   - **Output:**
     - Returns 'predicted': A NumPy array of the predicted class labels for 'testing_data'.


**]Support Vector Classification (SVC) with Radial Basis Function (RBF) Kernel**

**Global Declarations:**
- An SVC object, named 'rbfSVC', is initialized globally with specified parameters including the 'rbf' kernel and auto gamma.

**Functions:**

1. **fitTrainingData(training_data, nSamples)**
   - **Input:** 
     - 'training_data': A 2D list where each row represents a sample (cell) and each column represents a feature (gene expression).
     - 'nSamples': A 1D list of labels or classifiers corresponding to each sample in 'training_data'.
   - **Process:**
     - Converts 'training_data' and 'nSamples' from lists to NumPy arrays.
     - Calls the 'fit' method of 'rbfSVC' to train the model on 'training_data' and 'nSamples'.
   - **Output:** 
     - Does not return an explicit output, but it does train the 'rbfSVC' object with the provided data, readying it for prediction.

2. **predictTestData(testing_data)**
   - **Input:** 
     - 'testing_data': A 2D list where each row represents a sample (cell) to be classified.
   - **Process:**
     - Converts 'testing_data' from a list to a NumPy array.
     - Calls the 'predict' method of 'rbfSVC' to predict the class labels for 'testing_data'.
   - **Output:**
     - Returns 'predicted': A NumPy array of the predicted class labels for 'testing_data'.

Sure, here's a detailed programming spec combining the two given specifications:

**RNA-seq Data Classification**

**Command-Line Arguments:**

1. `raw_data_file`: The path to the raw data file.
2. `annotations_file`: The path to the annotations file.
3. `classifier`: An integer that indicates the classifier to use: 1 for RBF SVM, 2 for MLP, 3 for KNN, 4 for RF.
4. `downSampleFlag`: A binary indicator (0 or 1) of whether to down-sample the data.
5. `crossValidateFlag`: A binary indicator (0 or 1) of whether to perform cross-validation.
6. `n_neighbors` (optional): The number of neighbors to use if KNN is chosen as the classifier.

**Functions:**

1. `rbfSVC(trainingData, testingData, trainingDataTargets, testingDataTargets)`: Trains an RBF SVM classifier on the training data and returns the predicted values for the test data.
2. `mlp(trainingData, testingData, trainingDataTargets, testingDataTargets)`: Trains a MLP classifier on the training data and returns the predicted values for the test data.
3. `knn(trainingData, testingData, trainingDataTargets, testingDataTargets)`: Trains a KNN classifier on the training data and returns the predicted values for the test data.
4. `rf(trainingData, testingData, trainingDataTargets, testingDataTargets)`: Trains a RF classifier on the training data and returns the predicted values for the test data.

**Data Class:**

The main function initializes an `RNASeqData` object with the raw data file and annotations file. This object is responsible for loading and processing the data.

- `RNASeqData.loadData()`: Loads raw RNA seq data and annotations into memory.
- `RNASeqData.processData()`: Applies preprocessing, including optional downsampling.
- `RNASeqData.createFolds()`: Creates 10-fold cross-validation datasets if `crossValidateFlag` is set.
- `RNASeqData.partitionData()`: Partitions the data into 70% training and 30% testing if `crossValidateFlag` is not set.

**Main Function:**

The main function processes command-line arguments, decides which classifier to use, and whether to down-sample or cross-validate the data. Then it calls the corresponding function to preprocess the data, perform the classification, and send the results to an analysis module for evaluation and writing the results to a file.

1. Parse command-line arguments to get `raw_data_file`, `annotations_file`, `classifier`, `downSampleFlag`, `crossValidateFlag`, and `n_neighbors`.
2. Initialize `RNASeqData` object with `raw_data_file` and `annotations_file`.
3. Load raw RNA seq data and annotations into memory using `RNASeqData.loadData()`.
4. Apply preprocessing to the data with `RNASeqData.processData()`. If `downSampleFlag` is set, also downsample the data.
5. If `crossValidateFlag` is set, create 10-fold cross-validation datasets using `RNASeqData.createFolds()`. For each fold:
   - Separate the current fold for testing and the rest for training.
   - Depending on the classifier chosen (RBF SVM, MLP, KNN, RF), call the corresponding function with the training data and labels and the testing data and labels.
   - Use the returned predictions and the true labels to calculate evaluation metrics.
   - Write the results and evaluations to a file.
6. If `crossValidateFlag` is not set, partition the data into 70% training and 30% testing using `RNASeqData.partitionData()`. Then, similar to the above step:
   - Depending on the classifier chosen (RBF SVM, MLP, KNN, RF), call the corresponding function with the training data and labels and the testing data and labels.
   - Use the returned predictions and the true labels to calculate evaluation metrics.
   - Write the results and evaluations to a file.
7. Print the total execution time of the program.

## Notes:
- All classifiers are implemented as separate functions and accept training data, testing data, and their corresponding target values. They output predicted values for the testing data.
- The analysis method `analyzeAndWriteToFile` is used to compute evaluation metrics and write the results to a file.
- If KNN is chosen as the classifier, the `n_neighbors` parameter should be provided.
- The `RNASeqData` object handles all data loading and preprocessing tasks, including optional downsampling and creating training and testing partitions.
- If the `crossValidateFlag` is set, the program performs 10-fold cross-validation. Otherwise, it simply partitions the data into 70% training and 30% testing.
- The final results of the program are both displayed in the console (or command line) and written to a file.



