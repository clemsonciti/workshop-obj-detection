---
title: "Preparing the data"
teaching: 0
exercises: 0
questions:
- "How do we annotate images and save the resulting xml files."
objectives:
- ""
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

> ## 2. Image organization
> 
> - The `training_demo` directory provide a template to be reused in different project. 
> - The annotated images should be divided into two directories inside `training` and `testing`
> under the `images` subdirectory. 
>   - The ratio should be roughly 80% for training and 20% for testing. 
> - The `xml_to_csv.py` will help generate the `.csv` files listing the images for `training`
> and `testing`. 
>
> ~~~
> $ cd ~/tensorflow/training_demo
> $ python xml_to_csv.py
> ~~~
> {: .language-bash}
>
> - Next, generate the `.record` files. 
>
> ~~~
> python generate_tfrecord.py --csv_input=images/train_labels.csv --image_dir=images/train --output_path=train.record
> python generate_tfrecord.py --csv_input=images/test_labels.csv --image_dir=images/test --output_path=test.record
> ~~~
> {: .language-bash}
>
>
> - Modify the `ssd_efficientdet_d0_512x512_coco17_tpu-8.config` file in side the `training` directory as follows:
>   - Line 13: change the number of classes to number of objects you want to detect (4 in this case)
>   - Line 141: change `fine_tune_checkpoint` to the path of the model.ckpt file: 
>     - `/home/lngo/tensorflow/workspace/training_demo/pre-trained-models/efficientdet_d0_coco17_tpu-32/checkpoint/ckpt-0`: For 
>     this particular workshop, change `lngo` to your username. 
>   - Line 143: Change `fine_tune_checkpoint_type` to `detection`.
>
{: .slide}


{% include links.md %}

