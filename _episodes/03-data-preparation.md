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


> ## 1. Demo image annotation software
> 
> - [labelImg](https://github.com/tzutalin/labelImg#usage) is one of the most common (and free)
> image annotation software. 
> - Installation is available for Windows, Mac, and Linux. 
> - The process is boring and tedious. 
> - Open a new terminal from the login node
>
> ~~~
> $ qsub -I -X
> $ singularity exec -B $TMPDIR:$TMPDIR /zfs/citi/tf_downloads/imgLabel.sif python3 /labelImg/labelImg.py 
> ~~~
> {: .language-bash}
>
{: .slide}

> ## 2. Training space creation
> 
> - We are going to create a directory called `workspace` under `tensorflow`. 
> - Inside `workspace`, create another directory called `training_demo` and the additional 
> subdirectories inside `training_demo` as follows:
>
> ~~~
> $ cd ~/tensorflow/workspace/training_demo/images/
> $ cp /zfs/citi/images/dogs/images/Images/n02085620-Chihuahua/* .
> $ cp /zfs/citi/images/dogs/annotations/Annotation/n02085620-Chihuahua/* .
> $ cp /zfs/citi/images/dogs/images/Images/n02087046-toy_terrier/* .
> $ cp /zfs/citi/images/dogs/annotations/Annotation/n02087046-toy_terrier/* .
> $ python /zfs/citi/tf_downloads/scripts/preprocessing/partition_dataset.py -x -i . -r 0.2
> ~~~
> {: .language-bash}
>
> <img src="../fig/02-workspace/02.png" style="height:300px">
>
> - What are the labels for the annotations?
> 
> ~~~
> $ cd ~/tensorflow/workspace/training_demo/annotations/
> $ cat ../images/n02085620_1502
> $ cat ../images/n02087046_2193
> ~~~
> {: .language-bash}
>
> - Create a file called `label_map.pbtxt` with the following contents
> 
> ~~~
> item {
>   id: 1
>   name: 'Chihuahua' 
> }
>
> item {
>   id: 2
>   name: 'toy_terrier'
> }
> ~~~
> {: .language-bash}

<!-->
$ python /zfs/citi/tf_downloads/scripts/preprocessing/generate_tfrecord.py -x ../images/train/ -l ./label_map.pbtxt -o ./train.record

$ python /zfs/citi/tf_downloads/scripts/preprocessing/generate_tfrecord.py -x ../images/test/ -l ./label_map.pbtxt -o ./test.record

$ cd ../pre-trained-models/

$ cp /zfs/citi/tf_downloads/pre-train-models/ssd_resnet50_v1_fpn_640x640_coco17_tpu-8.tar.gz .
$ tar xzf ssd_resnet50_v1_fpn_640x640_coco17_tpu-8.tar.gz

$ tar xzf ssd_resnet152_v1_fpn_640x640_coco17_tpu-8.tar.gz
$ cd ../models/
$ mkdir my_ssd_resnet50_v1_fpn
$ cp ../pre-trained-models/ssd_resnet50_v1_fpn_640x640_coco17_tpu-8/pipeline.config my_ssd_resnet50_v1_fpn/

$ cp ~/tensorflow/models/research/object_detection/model_main_tf2.py .
$ python model_main_tf2.py --model_dir=models/my_ssd_resnet50_v1_fpn --pipeline_config_path=models/my_ssd_resnet50_v1_fpn/pipeline.config

-->

{: .slide}


{% include links.md %}

