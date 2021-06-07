---
title: Setup
---

> ## 1. Request a Palmetto compute node
>
> - Request an interactive session. This workshop is created for TensorFlow 2+. 
> This version of TensorFlow requires AVX+ support from CPU, which is not available on the older 
> nodes. It is important that you are aware of what type of CPU architecture is available on the node
> on which you first installed TensorFlow. 
> - We omit the discussion on GPU-support for this workshop due to complexity/variety in possible 
> combination (see https://www.tensorflow.org/install/source#linux).>
> - Request a node. For the purpose of the workshop, we will use nodes in phases `8a`, `8b`, `9`, and `10`. 
> 
> ~~~
> qsub -I -l select=1:ncpus=16:mem=60gb:interconnect=any:phase=8a,walltime=72:00:00
> ~~~
> {: .language-bash}
>
{: .slide}

> ## 2. Support modules
> 
> ~~~
> $ module load anaconda3/2019.10-gcc/8.3.1 
> ~~~
> {: .language-bash}
> 
{: .slide}

> ## 3. Installing TensorFlow in a Python virtual environment
>
> - Create a directory to store the Python virtual environment packages:
>
> ~~~
> $ mkdir -p ~/software/venv
> $ python -m venv ~/software/venv/tf_2.5
> ~~~
> {: .language-bash}
>
> - Activate the virtual environment:
>
> ~~~
> $ source ~/software/venv/tf_2.5/bin/activate
> ~~~
> {: .language-bash}
> 
> - Install TensorFlow:
>
> ~~~
> $ pip install --upgrade pip
> $ pip install tensorflow==2.5
> ~~~
> {: .language-bash}
> 
> - This will automatically install some packages that are required for Tensorflow, like SciPy or NumPy. 
> - To test that TensorFlow was imported properly, you can do the following:
>
> ~~~
> $ python
> >>> import tensorflow as tf
> >>> print(tf.__version__)
> >>> quit()
>
> ~~~
> {: .language-bash}
>
> - The result should be similar (maybe not completely matched up) with the screenshot below
> 
> <img src="../workshop-obj-detection/fig/setup/01.png" style="height:400px">
>
> - Install TensorFlow Jupyter Kernel:
>
> ~~~
> $ pip install ipykernel cython kaggle
> $ python -m ipykernel install --user --name tf_2.5 --display-name TF2.5
> ~~~
> {: .language-bash}
> 
{: .slide}


> ## 4. Setup object detection components
>
> - Create the directory and acquire the model files. We are using a copy of the tensorflow model Git repository downloaded 
> on April 2021. This is to ensure that the versioning of tensorflow is *frozen* for this workshop. 
> 
> ~~~
> $ mkdir ~/tensorflow
> $ cd ~/tensorflow
> $ git clone https://github.com/tensorflow/models.git
> ~~~
> {: .language-bash}
>
> - Install and test the COCO (Common Objects in Context) API
> - **It is important to keep in mind that you are doing this via a terminal that already source activated the virtual environment for TensorFlow**.
> 
> ~~~
> $ module load protobuf/3.11.2-gcc/8.3.1
> $ cd ~/tensorflow/models/research/
> $ protoc object_detection/protos/*.proto --python_out=.
> $ git clone https://github.com/cocodataset/cocoapi.git
> $ cd cocoapi/PythonAPI/
> $ make
> $ cp -r pycocotools ~/tensorflow/models/research/
> $ cd ~/tensorflow/models/research/
> $ cp object_detection/packages/tf2/setup.py .
> $ python -m pip install . tensorflow==2.5
> $ python object_detection/builders/model_builder_tf2_test.py 
> ~~~
> {: .language-bash}
> 
> - The final test results should look similar to the following screenshot
>
> <img src="../workshop-obj-detection/fig/setup/11.png" style="height:300px">
>
{: .slide}


> ## 5. Setup training directories
>
> ~~~
> $ cd ~/tensorflow
> $ cp -R /zfs/citi/tf_downloads/training_demo/ .
> ~~~
> {: .language-bash}
>
{: .slide}


> ## 6. Test the new TensorFlow environment on OpenOnDemand
>
> - In this part you are introduced to the upcoming OpenOnDemand (OOD) environment to interact 
> with Palmetto. 
> - At the moment, this interface is only accessible behind Clemson's VPN
> - Visit `https://openod02.palmetto.clemson.edu` and sign in. 
> 
> <img src="../workshop-obj-detection/fig/setup/02.png" style="height:300px">
>
> - Under `Interactive Apps`, select `Jupyter for TensorFlow`. 
> 
> <img src="../workshop-obj-detection/fig/setup/03.png" style="height:300px">
>
> - Fill in the form with allocation request parameters similar to the command line
> you had earlier, as show in the screenshot below. Click **Launch** when done. 
>   - `Number of resource chunk (select)`: 1
>   - `CPU cores per chunk (ncpus)`: 16
>   - `Amount of memory per chunk (mem)`: 62
>   - `Number of GPUs per chunk (ngpus)`: 2
>   - `GPU Model (gpu_model)`: K20
>   - `Interconnect`: any
>   - `Walltime`: 4 hours
>   - `Queue`: work1
>   - `List of modules to be loaded, separate by an empty space`: `anaconda3/2019.10-gcc/8.3.1 cuda/10.1.243-gcc/8.3.1 cudnn/7.6.5.32-10.1-linux-x64-gcc/8.3.1`
>   - `Path to Python virtual environment for TensorFlow`: `~/software/venv/tf_2.2`
>
> <img src="../workshop-obj-detection/fig/setup/04.png" style="height:800px">
>
> - Click **Connect to Jupyter** when the link becomes available
>
> <img src="../workshop-obj-detection/fig/setup/05.png" style="height:350px">
> <img src="../workshop-obj-detection/fig/setup/06.png" style="height:350px">
>
> - Launch a new notebook using the `TF2.2` kernel created earlier
> 
> <img src="../workshop-obj-detection/fig/setup/07.png" style="height:500px">
>
> - Run the commands in the shells as shown in the screenshot below to confirm a working notebook. 
> 
> <img src="../workshop-obj-detection/fig/setup/08.png" style="height:500px">
>
{: .slide}


{% include links.md %}
