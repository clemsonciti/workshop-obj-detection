---
title: "Preparing the workspace"
teaching: 0
exercises: 0
questions:
- "How should we organize directories to store various  raw, processed, training, testing, and validation data and other related contents"
objectives:
- "Being able to create and organize the hierarchy of directories."
keypoints:
- ""
---


> ## 1. Initial directory setup
> 
> - From the setup step, you should have a directory called `tensorflow` under your 
> home directory. 
> - Inside `tensorflow`, there is the `models` directory. 
> - Run the following commands to test and confirm the directory structure as follows
>
> ~~~
> $ cd
> $ ls -l ~/tensorflow
> $ ls -l ~/tensorflow/models | grep '^d'
> ~~~
> {: .language-bash}
>
> <img src="../fig/02-workspace/01.png" style="height:200px">
>
{: .slide}

> ## 2. Training space creation
> 
> - We are going to create a directory called `workspace` under `tensorflow`. 
> - Inside `workspace`, create another directory called `training_demo` and the additional 
> subdirectories inside `training_demo` as follows:
>
> ~~~
> $ mkdir ~/tensorflow/workspace
> $ cd ~/tensorflow/workspace
> $ cp -R /zfs/citi/tf_downloads/training_demo/ .
> $ cd training_demo
> ~~~
> {: .language-bash}
>
> - Directory Description:
>   - `images`: This folder contains a copy of all the images in our dataset, as well as the 
>   respective `*.xml` files produced for each one, once labelImg is used to annotate objects.
>   - `images/train`: This folder contains a copy of all images, and the respective *.xml files, which 
>   will be used to train our model.
>   - `images/test`: This folder contains a copy of all images, and the respective *.xml files, 
>   which will be used to test our model.
>   - `training`: This folder will contain a sub-folder for each of training job. Each subfolder will 
>   contain the training pipeline configuration file *.config, as well as all files generated during 
>   the training and evaluation of our model.
>   - `pre-trained-models`: This folder will contain the downloaded pre-trained models, which shall 
>   be used as a starting checkpoint for our training jobs.
>   - `create_coco_tf_record.py`,`generate_tfrecord.py`,`model_main_tf2.py`,`resize_images.py`,`xml_to_csv.py`: python scripts to help with generating metadata for the training activities. 
{: .slide}


{% include links.md %}

