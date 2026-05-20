# Binding affinity prediction network
 Multi Layer Perceptron network that can predict the binding affinity of potential drugs to a cell surface receptor\

 This network was built as a group project in the master course: Reproducible Data Science and Statistical Learning.\
 \

 # Aim
 The aim of this project is to construct a basic model that can predict the binding affinity of potential drugs to a cell surface receptor. For this model only one receptor is chosen, Epidermal growth factor receptor (EGFR). EGFR is often mutated or over expressed in many cancers, and therefore many drugs are designed to bind to it. To determine if a molecule will bind to EGFR or not, a model is trained with features of the molecues and what the binding affinity of these molecules are.

Most of the information about the molecules is given from so called Fingerprints, they are fixed-length binary vectors representing the presence of certain sub-structures in a molecule as well as its shape, size and which atoms are contained. Other information included is how well they bind to EGFR and other chemical properties such as molecular weight.

From the information about the molecules the purpose of this project is then to predict the pCHEMBL value which can be described as "how well the molecule binds to the EGFR". To create precitions of pCHEMBL values, a Neural Network has been created. It can be argued that something as advanced as a Neural Network is not actually needed for this quite simple problem. However, the idea of this project is to create a model which can be bulit upon. The model should be able to expand to include several different receptors, and more diverse molecules.

# Input data
Finding the input data
1. Go to the CHEMBL website https://www.ebi.ac.uk/chembl/
2. Search for the target molecule (EGFR), with ID CHEMBL203 and choose it in the list
3. Click on "Activity charts" or "Bioactivites" or similar in the menu to the left.
4. Click on the "Activity Types for CHEMBL203 (Epidermal growth factor receptor)"
5. Filter so that you only have the molecules with IC50 values\
6. In the top right klick on "csv", let it load and download the file with molecules with IC50 values.\
Description of the input data\
The data is downloaded from the CHEMBL webiste. The CHEMBL website is a public database of bioactive molecules with measured activities. The molecule EFGR was selected, and then a file containing molecules with known binding affinity, or lack thereof, to EFGR was downloaded. First molecules that had IC50 data was filtered, this is to make the molecules binding strength comparable to oneanother.
\
# MLP model using Keras
Before building the model, columns are cleaned from NaN values since the Keras models can't handle that. The data is also split into training (80%) and test data (20%). The type of neural network used is a Multi Layer Perceptron (MLP) network. The first layer is for normalization and centralisation, which means subtracting the mean and dividing with the variance for all values. This is because we don't want features that have naturally higher numbers to automatically get "stronger" weights.

The activation function used is ReLU, which is the best option since we use hidden layers in the model. With hiddens layers the ReLu function among other things helps to avoid the issue of vanishing gradients. For the last layer the activation function is linear instead of ReLu since we want a regression line as output. We use a dropout rate of 0.2 to minimize the risk of weights co-adapting. For each hidden layer a regularization layer is added that allows penalties to be applied to layer paramters and activity. A manual grid search was performed testing different values for L1 and L2. L1 (0, 1e-6, 1e-5, 1e-4, 1e-3) ) and L2 ({0, 1e-6, 1e-5, 1e-4, 1e-3) was all tested and the best combination chosen. Early stopping is added as a regularization method to prevent overfitting of the model. ADAM is used as an optimizer since its one of the more advanced ones, using both the methods of Momentum and RSMProp. Which means that the ADAM optimizer add a bit of the previous gradient to the current one, and adjusts the learning reate for each parameter based on the size of the recent gradients, and then also individually adjusts for each parameter.

