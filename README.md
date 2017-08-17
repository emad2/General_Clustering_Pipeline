# KnowEnG's General Clustering Pipeline 
This is the Knowledge Engine for Genomics (KnowEnG), an NIH BD2K Center of Excellence, General Clustering Pipeline.

This pipeline **clusters** the columns or rows of a given spreadsheet

There are four clustering methods that one can choose from:


| **Options**                                      | **Method**                           | **Parameters** |
| ------------------------------------------------ | -------------------------------------| -------------- |
| hierarchical clustering                          | nmf                                  | hclustering    |
| K-means                                          | bootstrapping with nmf               | kmeans         |


* * * 
## How to run this pipeline with Our data
* * * 
### 1. Clone the General_Clustering_Pipeline Repo
```
 git clone https://github.com/KnowEnG-Research/General_Clustering_Pipeline.git
```
 
### 2. Install the following (Ubuntu or Linux)
  ```
 pip3 install pyyaml
 pip3 install knpackage
 pip3 install scipy==0.18.0
 pip3 install numpy==1.11.1
 pip3 install pandas==0.18.1
 pip3 install matplotlib==1.4.2
 pip3 install scikit-learn==0.17.1
 
 apt-get install -y python3-pip
 apt-get install -y libfreetype6-dev libxft-dev
 apt-get install -y libblas-dev liblapack-dev libatlas-base-dev gfortran
```

### 3. Change directory to General_Clustering_Pipeline

```
cd General_Clustering_Pipeline
```

### 4. Change directory to test

```
cd test
```
 
### 5. Create a local directory "run_dir" and place all the run files in it
```
make env_setup
```

### 6. Use one of the following "make" commands to select and run a clustering option:


| **Command**           | **Option**                                       | 
|:--------------------- |:------------------------------------------------ | 
| make run_hclustering  | Clustering with linkage                          |
| make run_kmeans       | Clustering with k-means                          |

 
* * * 
## How to run this pipeline with Your data
* * * 

__***Follow steps 1-3 above then do the following:***__

### * Create your run directory

 ```
 mkdir run_directory
 ```

### * Change directory to the run_directory

 ```
 cd run_directory
 ```

### * Create your results directory

 ```
 mkdir results_directory
 ```
 
### * Create run_paramters file  (YAML Format)
 ``` 
 Look for examples of run_parameters in the General_Clustering_Pipeline/data/run_files zTEMPLATE_cc_net_nmf.yml
 ```
### * Modify run_paramters file  (YAML Format)
Change processing_method to one of: serial, parallel depending on your machine.
```
processing_method: serial
```

set the data file targets to the files you want to run, and the parameters as appropriate for your data.


### * Run the Samples Clustering Pipeline:

  * Update PYTHONPATH enviroment variable
   ``` 
   export PYTHONPATH='../src':$PYTHONPATH    
   ```
   
  * Run
   ```
  python3 ../src/samples_clustering.py -run_directory ./run_dir -run_file zTEMPLATE_cc_net_nmf.yml
   ```

* * * 
## Description of "run_parameters" file
* * * 

| **Key**                    | **Value**                   | **Comments**                                   |
| -------------------------  | --------------------------- | ---------------------------------------------- |
| method                     | **hclustering**, **kmeans** | Choose clustering method                       |
| spreadsheet_name_full_path | directory+spreadsheet_name  |  Path and file name of user supplied gene sets |
| results_directory          | directory                   | Directory to save the output files             |
| number_of_clusters         | 3                           | Estimated number of clusters                   |

spreadsheet_name = ProGENI_rwr20_STExp_GDSC_500.rname.gxc.tsv</br>

* * * 
## Description of Output files saved in results directory
* * * 

* Output files of all four methods save genes by sample heatmap variances per row with name **row{column}_variance_{method}_{timestamp}_viz.tsv**.</br>

 |  |**variance**|
 | :--------------------: |:--------------------:|
 | **row{col} 1**         |float                 |
 |...                     |...                   |
 | **row{col} m**         | float                |

* Output files of all the methods save row{col} by col{row} heatmap with name **row{col}_by_col{row}_heatmp_{method}_{timestamp}_viz.tsv**.</br>

 |  |**col{row} 1**|...|**col{row} n**|
 | :--------------------: |:--------------------:|:--------------------:|:--------------------:|
 | **row{col} 1**         |float                 |...                   |float                 |
 |...                     |...                   |...                   |...                   |  
 | **row{col} m**         |float                 |...                   |float                 |

 
* Output files of all  methods save col{row} to cluster map with name **col{row}_labeled_by_cluster_{method}_{timestamp}_viz.tsv**.</br>

 |    |**cluster**|
 | :--------------------: |:--------------------:|
 | **col{row} 1**         |int                   |
 |...                     |...                   |
 | **col{row} n**         |int                   |
 
* Output files of all  methods save row{col} scores by cluster with name **row{col}_averages_by_cluster_{method}_{timestamp}_viz.tsv**.</br>

 |  |**cluster 1**|...|**cluster k**|
 | :--------------------: |:--------------------:|:--------------------:|:--------------------:|
 | **row{col} 1**         |float                 |...                   |float                 |
 |...                     |...                   |...                   |...                   |
 | **row{col} m**         |float                 |...                   |float                 |
 
* Output files of all  methods save spreadsheet with top ranked genes per sample with name **top_row{col}_by_cluster_{method}_{timestamp}_download.tsv**.</br>

 |  |**cluster 1**|...|**cluster k**|
 | :--------------------: |:--------------------:|:--------------------:|:--------------------:|
 | **row{col} 1**         |1/0                   |...                   |1/0                   |
 |...                     |...                   |...                   |...                   |
 | **row{col} m**         |1/0                   |...                   |1/0                   |
  
* All  methods save **silhouette number of clusters** and **corresponding silhouette score** with name silhouette_average\_{method}\_{timestamp}\_viz.tsv.</br>
 ```
 File Example: 
 silhouette number of clusters = 3, corresponding silhouette score = 1
 ```