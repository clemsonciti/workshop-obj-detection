---
title: Setup
---

> ## 1. Setup TensorFlow 2.0 for GPU on Palmetto
>
> - Request an interactive session on a GPU node. This workshop is created for TensorFlow 2+. 
> This version of TensorFlow requires AVX2 support from CPU, which is not available on the older 
> nodes. Currently Palmetto nodes from Phase 12 and up support AVX2.
> - Request a node. As mentioned earlier, we should pay attention to our CPU/GPU combination
>   - `chip_type=e5-2680v3` and `gpu_model=k40`
>   - `chip_type=e5-2680v4` and `gpu_model=p100`
>   - `chip_type=6148g` and `gpu_model=v100`. 
> - For the purpose of the workshop, we will only use one GPU card. 
> - We will intentionally halve the number of CPU and the amount of memory so that we can have two participants
> per node. 
> 
> ~~~
> qsub -I -l select=1:ncpus=10:mem=60gb:ngpus=1:gpu_model=k40:interconnect=10ge:chip_type=e5-2680v3,walltime=72:00:00 -q deep
> ~~~
> {: .language-bash}
> 
> - Load the necessary module:
>
> ~~~
> $ module load anaconda3/2019.10-gcc/8.3.1 
> $ module load cuda/11.0.3-gcc/7.5.0 cudnn/8.0.0.180-11.0-linux-x64-gcc/7.5.0
> ~~~
> {: .language-bash}
> 
> - Create a directory to store the Python virtual environment packages:
>
> ~~~
> $ mkdir -p ~/software/venv
> $ python3 -m venv --system-site-packages ./software/venv/tf_gpu
> ~~~
> {: .language-bash}
>
> - Activate the virtual environment:
>
> ~~~
> $ source ~/software/venv/tf_gpu/bin/activate
> ~~~
> {: .language-bash}
> 
> - Install TensorFlow:
>
> ~~~
> $ pip install --upgrade pip
> $ pip install tensorflow==2.4
> ~~~
> {: .language-bash}
> 
> - This will automatically install some packages that are required for Tensorflow, like SciPy or NumPy. 
> - To test that TensorFlow was imported properly, you can do the following:
>
> ~~~
> $ python
> >>> import tensorflow as tf
> >>> print(tf.test.gpu_device_name())
> ~~~
> {: .language-bash}
>
> - The result should be similar (maybe not completely matched up) with the screenshot below
> 
> <img src="../fig/setup/01.png" style="height:500px">
>
> - Install TensorFlow Jupyter Kernel:
>
> ~~~
> $ python3 -m ipykernel install --user --name tf_gpu --display-name TensorflowGPU
> $ echo "module load anaconda3/2019.10-gcc/8.3.1 cuda/11.0.3-gcc/7.5.0 cudnn/8.0.0.180-11.0-linux-x64-gcc/7.5.0" >> ~/.jhubrc
> ~~~
> {: .language-bash}
> 
{: .slide}


> ## 2. Setup object detection components
>
> - Log into Palmetto from a terminal
> - Request a node. As mentioned in the previous slide, we should pay attention to our CPU/GPU combination
>   - `chip_type=e5-2680v3` and `gpu_model=k40`
>   - `chip_type=e5-2680v4` and `gpu_model=p100`
>   - `chip_type=6148g` and `gpu_model=v100`. 
> - For the purpose of the workshop, we will only use one GPU card. 
> - We will intentionally halve the number of CPU and the amount of memory so that we can have two participants
> per node. 
> 
> ~~~
> qsub -I -l select=1:ncpus=10:mem=60gb:ngpus=1:gpu_model=k40:interconnect=10ge:chip_type=e5-2680v3,walltime=72:00:00 -q deep
> ~~~
> {: .language-bash}
>
> - Activate the python environment for tensorflow GPU and load the relevant modules
> 
> ~~~
> $ source software/venv/tf_gpu/bin/activate
> $ module load anaconda3/2019.10-gcc/8.3.1 
> $ module load cuda/11.0.3-gcc/7.5.0 cudnn/8.0.0.180-11.0-linux-x64-gcc/7.5.0 
> $ module load protobuf/3.11.2-gcc/8.3.1
> ~~~
> {: .language-bash}
>
> - Create the directory and acquire the model files. We are using a copy of the tensorflow model Git repository downloaded 
> on April 2021. This is to ensure that the versioning of tensorflow is *frozen* for this workshop. 
> 
> ~~~
> $ mkdir ~/tensorflow
> $ cd ~/tensorflow
> $ cp /zfs/citi/tf_downloads/models.tgz .
> $ tar xzf models.tgz
> ~~~
> {: .language-bash}
>
> - Install and test the COCO (Common Objects in Context) API
> 
> ~~~
> $ cd ~/tensorflow/models/research/
> $ protoc object_detection/protos/*.proto --python_out=.
> $ git clone https://github.com/cocodataset/cocoapi.git
> $ cd cocoapi/PythonAPI/
> $ make
> $ cp -r pycocotools ~/tensorflow/models/research/
> $ cd ~/tensorflow/models/research/
> $ cp object_detection/packages/tf2/setup.py .
> $ python -m pip install --user .
> $ python object_detection/builders/model_builder_tf2_test.py 
> ~~~
> {: .language-bash}
> 
> - The final test results should look similar to the following screenshot
>
> <img src="../fig/setup/02.png" style="height:500px">
>
> - Setup `labelImg` for data preparation (you can also do this locally)
>
> ~~~
> $ pip install --user labelImg
> $ export PATH=/home/$USER/.local/bin:$PATH
> $ 
{: .slide}

{% include links.md %}
