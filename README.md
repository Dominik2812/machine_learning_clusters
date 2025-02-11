# machine_learning_clusters

<p align="center">
  <img width="700"src="snapshots/abstract.png">
</p>

## Motivation
Visualize and classify complex multidimensional datasets with only two clicks.  The app can be used in combination with my **django_machine_learning_supervised_classification** repository. In contrast to the latter, the project here is build with Django's Class Based Views.


## How to use it
Load unclassified datasets from your local machine or remote resources via URL. 

<p align="center">
  <img src="snapshots/load_data.png" width="800" />
</p>

The dataset is then projected in two dimensions via Principal Component Analysis (**PCA**). A plotly express scatter plot will be displayed as in the following snapshot. Underneath you find the head of the data table you just loaded. Type the number of clusters (or increment by arrows) that you expect, meaning how many clusters do you see in the plot. Submit again.
<p align="center">
  <img src="snapshots/projection.png" width="800" /> 
</p>
Datapoints are now assigned to clusters in both, the plot and the data table underneath. The classification has taken place via the k_Means_Algorithm. The classified multidimensional data locally stored in the data folder for further processing. E.g. classify new data with my supervised machine learning application **DjangoMachineLearning**.

<p align="center">
  <img src="snapshots/clusters.png" width="800" />
</p>
Test the application with the iris data under data/iris-test-data.csv

## Crucial parts of the code

### views.py
In the following, I will roughly explain how the views.py works, for a detailed explanation please read the comments in the file itself. The views.py consists of two parts, one that processes your data set and one for the actual views. Throughout both parts, classes inherit from top to bottom. 

The first part has three classes. The first step is to read in a dataset which is achieved by the class **ClusterBaseData**. The dataset is then saved as a pickle object in the database for future analysis. The **DimReduction** class projects the multidimensional data into two dimensions via PCA and saves both, a data table and a plot of the new 2D dataset. This class is applied twice, for the raw (yet unclassified) data and the classified data. The latter will be delivered by the class **Cluster**, which applies the k-Means_method to classify the **DimReduction** objects. 

The second part of views.py contains two views. THe IndexView inherits from **FormView** and **TemplateView** to process the input form and the results at the same time. It also inherits all methods of the data processing part by the class **Clusters**. Initially, only the form is displayed. On submitting the URL, the 2D plot and the head of the data table of the original nonclassified data are shown. If then the number of expected clusters is submitted the class **ResultsView** is called which has basically the same structure but processes the clustered data instead of the nonclassified. 

The following graph illustrates the data flow within the views.py. circles, squares, and triangular represent variables, classes and templates respectively. Red, violet and green distinguish between classes from models.py, data processing classes and views. 


<p align="center">
  <img src="snapshots/dataflow.png" width="800" />
</p>


### models.py
If a dataset has already beíng processed by the app, the same calculation doesn't have to be done over and over again.  This is why all the results are saved as instances of two classes in the database. The attributes of the corresponding classes also serve as identifiers. Data frames and plots are stored as a PickledObjectField(). The class **BaseData** contains the original nonclassified dataset. The class **ProjectionIn2D** contains the PCA projection of both, the original (nonclassified) and the clustered (by k-Means classified) dataset. 

<p align="center">
  <img src="snapshots/models.png" width="800" />
</p>
