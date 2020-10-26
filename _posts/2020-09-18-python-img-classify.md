---
layout: post
title:  "Using GitHub Actions to Automate Image Classification with Microsoft Azure Custom Vision Services"
summary: Setting up GitHub Actions to automate the process of image-classification using Azure.
author: Mritunjay Sharma
date: '2020-09-18 23:45:23 +0530'
category: GitHub-Actions, Automating, Python
thumbnail: /assets/img/posts/img-classify.jpeg
keywords: GitHub-Actions, python, workflows, yaml
permalink: /blog/python-img-classify
---

[Instructions]: # (To submit to the GitHub Actions x DEV Hackathon, please fill out all sections.)

### My Workflow
[Note]: # (Make sure to include the name of your GitHub Action and share where it's being used!)

I created this new GitHub Action available [here](https://github.com/marketplace/actions/automating-image-classification-with-microsoft-azure-custom-vision-training-and-prediction) with which we can automate the entire process of image classification with Azure Custom Vision.
With this - all a user needs to do is:
* Add the 4 SECRETS which are basically Azure Custom Vision Services Credentials. The process is explained in the [README](https://github.com/mritunjaysharma394/autoCustomVision/blob/master/README.md)
* Upload Image Dataset in the GitHub Repo and modify the `tags`, `tagsVar` and `trainSize` accordingly in the below `yml` file.

Here's a sample workflow YML file using this Action:

```yaml
name: Auto Custom Vision Classifier
on:
  push:
    paths:
    - '**.yml'
jobs:
  build_model:
    runs-on: ubuntu-latest
    steps:
    - name: Automating Image Classification with Microsoft Azure Custom Vision Training and Prediction
      uses: mritunjaysharma394/autoCustomVision@v1.0
      with:
        tags: "[Hemlock,Japanese Cherry]" # Rename it according to the folder name under images/ which will also be our name to the tags
        tagsVar: "[hemlock_,japanese_cherry_]" # Rename it according to the symmetry of file names under images/tag/ 
        trainSize: "10" # Train Size of Each Tag
        endpoint: ${{ secrets.AZURE_ENDPOINT }}
        trainingKey: ${{ secrets.AZURE_TRAINING_KEY }}
        predictionKey: ${{ secrets.AZURE_PREDICTION_KEY }}
        predictionResourceid: ${{ secrets.AZURE_PREDICTION_RESOURCE_ID }}
```

#### Working Video Example:

