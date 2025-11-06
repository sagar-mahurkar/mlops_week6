# Week 2- Graded Assignment 2

### Assignment Objective

Incorporate DVC for the local data into the homework pipeline.

Setup DVC in IRIS Pipeline we have setup as part of Week-1 Assignment

1. Setup the git repository

2. Configure DVC to use Google Cloud storage bucket as Remote storage

3. Augment the IRIS data to simulate the data additions and start training

4. Demonstrate storing data and model files as part of DVC

5. Demonstrate the ability to traverse through data versions effortlessly
using dvc checkout


```shell
# Reference
# google cloud bucket uri to use as remote storage
# BUCKET_URI = f"gs://mlops-lively-nimbus-473407-m9"
# raw_data: https://raw.githubusercontent.com/IITMBSMLOps/ga_resources/refs/heads/week_1/data/raw/iris.csv
# v1_data: https://raw.githubusercontent.com/IITMBSMLOps/ga_resources/refs/heads/week_1/data/v1/data.csv
# v2_data: https://raw.githubusercontent.com/IITMBSMLOps/ga_resources/refs/heads/week_1/data/v2/data.csv
```

## Set up Git account
```shell
git config --global user.email "sagar.0.mahurkar@gmail.com"
git config --global user.name "sagar-mahurkar"
```

## 1. Set up a Git repository
```shell
git init
git remote -v
git remote remove origin
git remote add origin https://github.com/sagar-mahurkar/mlops_week2.git
git remote -v
git branch -M main
git push -u origin main
```

## 2. Configure dvc to use Google Cloud storage bucket as Remote storage

### Install dvc on virtual environment
```shell
pip install dvc
pip install dvc-gs
dvc init
```

### Add Google Cloud storage bucket as Remote storage
```shell
dvc remote add -d gcs_remote gs://mlops-lively-nimbus-473407-m9
```

### Create folder to store model locally
```shell
mkdir artifacts
```

## 3. Augment the IRIS data to simulate the data additions and start training

### Get the data
```shell
wget https://raw.githubusercontent.com/IITMBSMLOps/ga_resources/refs/heads/week_1/data/raw/iris.csv -O data.csv
echo "/data.csv" >> .gitignore
```

### Train the model
```shell
python train.py
```

## 4. Demonstrate storing data and model files as part of DVC

### Add files to dvc
```shell
dvc add data.csv artifacts/model.joblib
```

### Commit the current state:
```shell
git add .gitignore data.csv.dvc artifacts/model.joblib.dvc artifacts/.gitignore
git commit -m "First model, trained"
git tag -a "v1.0" -m "model v1.0"
```

### Push to both Git and DVC data
```shell
git push -u origin main
dvc push
```

## Get newer version of the data
```shell
wget https://raw.githubusercontent.com/IITMBSMLOps/ga_resources/refs/heads/week_1/data/v1/data.csv -O data.csv
```
Repeat steps 3 and 4

## Explicitly push all tags to Git
```shell
git push origin --tags
```

## 5. Switching between versions

### Version 1

#### switch to version 1
```shell
git checkout v1.0
dvc checkout
```

#### train the model
```shell
python train.py
```

### Version 2

#### switch to version 2
```shell
git checkout v2.0
dvc checkout
```

#### train the model
```shell
python train.py
```