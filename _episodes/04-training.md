---
title: "Training the models"
teaching: 0
exercises: 0
questions:
- ""
objectives:
- "Train the model."
- "Observe the usage of GPU."
- "Observe the training process via TensorBoard."
keypoints:
- ""
---


> ## 1. Training
>
> - Run the training scripts
>
> ~~~
> $ python model_main_tf2.py --pipeline_config_path=training/ssd_efficientdet_d0_512x512_coco17_tpu-8.config --model_dir=training --alsologtostderr
> ~~~
> {: .language-bash}
>
>
> <img src="../fig/04-training/01.png" style="height:500px">
>
> - You can open another terminal, ssh to the node you requested, and run `nvidia-smi` to check that the training process is 
> using GPU
>
> ~~~
> $ nvidia-smi
> ~~~
> {: .language-bash}
>
> <img src="../fig/04-training/02.png" style="height:400px">
>
>
{: .slide}


> ## 2. Observation using TensorBoard
>
> - Open another terminal, ssh to the node you requested
> - Run the following commands to load the appropriate environment and launch the 
> `TensorBoard` service
>
> ~~~
> $ source software/venv/tf_gpu/bin/activate
> $ module load anaconda3/2019.10-gcc/8.3.1 
> $ module load cuda/11.0.3-gcc/7.5.0 cudnn/8.0.0.180-11.0-linux-x64-gcc/7.5.0 
> $ module load protobuf/3.11.2-gcc/8.3.1
> $ cd ~/tensorflow/workspace/training_demo
> $ tensorboard --logdir=training/train --host 0.0.0.0
> ~~~
> {: .language-bash}
>
> - You will be able to observe the training process
>
> <img src="../fig/04-training/03.png" style="height:500px">
>
{: .slide}


> ## 3. Export models
>
> ~~~
> $ cp ~/tensorflow/models/research/object_detection/exporter_main_v2.py .
> $ python exporter_main_v2.py --trained_checkpoint_dir training --output_directory inference_graph --pipeline_config_path training/ssd_efficientdet_d0_512x512_coco17_tpu-8.config
> ~~~
> {: .language-bash}
>
> - Open `object_detection.ipynb` to see how this can be loaded. 
>
{: .slide}

{% include links.md %}

