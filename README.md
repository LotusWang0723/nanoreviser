
# NanoReviser: An Error-correction Tool for Nanopore Sequencing Based on a Deep Learning Algorithm

* [Introduction](#introduction)
* [Requirements](#requirements)
* [Version](#version)
* [Installation](#installation)
* [Usage](#usage)
    * [NanoReviser.py](#NanoReviser.py)
    * [NanoReviser_train.py](#NanoReviser_train.py)
* [Examples](#examples)
    * [For basecalling revising](#for-basecalling-revising)
    * [For training NanoReviser](#for-training-nanoreviser)
* [Citation](#citation)
* [Contact](#contact)

## Introduction

Nanopore sequencing is regarded as one of the most promising third-generation sequencing (TGS) technologies. However, the nanopore sequencing reads are susceptible to a fairly high error rate owing to the difficulty in identifying the DNA bases from the complex electrical signals. Here we proposed a DNA basecalling reviser, **NanoReviser**, based on a deep learning algorithm to correct the basecalling errors introduced by current basecallers provided by default. In our module, we re-segmented the raw electrical signals based on the basecalled sequences provided by the default basecallers. By employing convolution neural networks (CNNs) and bidirectional long short-term memory (Bi-LSTM) networks, we took advantage of the information from the raw electrical signals and the basecalled sequences from the basecallers. 

**NanoReviser**, as a post-basecalling reviser, significantly improving the basecalling quality. The NanoReviser package is freely available at https://github.com/pkubioinformatics/NanoReviser.


## Requirements

+ [Anaconda](https://www.anaconda.com/)
+ [Python version >= 3.6.9](https://www.python.org/)
+ [numpy version >= 1.17.3](http://www.numpy.org/)
+ [pandas, version >= 0.25.0](http://pandas.pydata.org/)
+ [TenserFlow, version >= 1.12.0](https://www.tensorflow.org/)
+ [Keras, version 1.2.2](https://https://github.com/keras-team/keras/)
+ [h5py, version >= 2.5.0](http://www.h5py.org/)
+ [GraphMap, version >= 0.5.2](https://github.com/isovic/graphmap/)

## Version

+ NanoReviser 1.0 (Tested on MacOS 10.14.6 and Linux_64, including CentOS 6.5 and Ubuntu 16.04)


## Installation


#### Install NanoReviser using git

clone NanoReviser package

    $ git clone https://github.com/pkubioinformatics/nanoreviser.git
    
change directory to NanoReviser

    $ cd nanoreviser

If you currently have TensorFlow installed on your system, we would advise you to create a virtual environment to install NanoReviser. If you want to do so, we recommend the user-friendly [Anaconda](https://www.anaconda.com/).

You will create an eviroment named nanorev for NanoReviser and install all dependencies through conda

**Note : you need to replace the path where you installed anaconda**


For linux with gpu 
    
    $ conda env create -n nanorev /**Your_Path_to_Anaconda**/envs/nanorev/ -f ./enviroment/NanoReviser.yaml 
	  $ conda activate nanorev
 

For linux just with cpu
	
	$ conda env create -n nanorev /**Your_Path_to_Anaconda**/envs/nanorev/ -f ./enviroment/NanoReviser_cpu.yaml 
	$ conda activate nanorev
	$ conda install tensorflow==1.12.0



Please run the unitest in oder to make sure NanoReviser installed properly.

    $ sh unitest.sh
    


## Usage


#### NanoReviser.py

An ONT basecalling reviser based on deep learning

    usage:
           python NanoReviser.py [options] -d <fast5_files> -o <output_path> -f <output_format, default=fasta>

           [example]
           python NanoReviser.py -d ./unitest/test_data/fast5/ -o ./unitest/nanorev_output/ -f fasta

	usage: 
           python NanoReviser.py [options]

	An ONT basecalling reviser based on deep learning

	Options:
    --version                                              show program's version number and exit
    -h, --help                                             show this help message and exit
    -d FAST5_BASE_DIR, --fast5_base_dir=FAST5_BASE_DIR     path to the fast5 files
    -o OUTPUT_DIR, --output_dir=OUTPUT_DIR                 path to store the output files
    --model1_predict_dir=MODEL1_PREDICT_DIR                model dirs for model1
    --model2_predict_dir=MODEL2_PREDICT_DIR                model dirs for model2
    -f OUTPUT_FORMAT, --output_format=OUTPUT_FORMAT        format of the output files, default is fasta
    --thread=THREAD                                        thread, default is 100
    -t TEMP_DIR, --tmp_dir=TEMP_DIR                        path to the tmp dir, which is used to store the 
                                                           preprocessing files
    -e FAILED_READS_FILENAME                               document to log the failed reads, default is
                                                           failed_read.txt
    --basecall_group=BASECALL_GROUP                        attrs for finding the events file in fast5 file, 
                                                           default is Basecall_1D_000
    --basecall_subgroup=BASECALL_SUBGROUP                  attrs for finding the events file in fast5 file,
                                                           default is BaseCalled_template
    


#### NanoReviser_train.py

A training tools for generation model files for NanoReviser

    usage:
           python NanoReviser_train.py [options] -d <fast5_files> -m <output_model>

           [example]
           python NanoReviser.py -d ./unitest/test_data/fast5/ -m ./unitest/unitest_model/ -f fasta

	usage: 
           python NanoReviser_train.py [options]

	An ONT basecalling reviser based on deep learning

	Options:
    --version                                              show program's version number and exit
    -h, --help                                             show this help message and exit
    -d FAST5_BASE_DIR, --fast5_base_dir=FAST5_BASE_DIR     path to the fast5 files
    -o OUTPUT_DIR, --output_dir=OUTPUT_DIR                 path to store the output files
    --model1_predict_dir=MODEL1_PREDICT_DIR                model dirs for model1
    --model2_predict_dir=MODEL2_PREDICT_DIR                model dirs for model2
    -f OUTPUT_FORMAT, --output_format=OUTPUT_FORMAT        format of the output files, default is fasta
    --thread=THREAD                                        thread, default is 100
    -t TEMP_DIR, --tmp_dir=TEMP_DIR                        path to the tmp dir, which is used to store the 
                                                           preprocessing files
    -e FAILED_READS_FILENAME                               document to log the failed reads, default is
                                                           failed_read.txt
    --basecall_group=BASECALL_GROUP                        attrs for finding the events file in fast5 file, 
                                                           default is Basecall_1D_000
    --basecall_subgroup=BASECALL_SUBGROUP                  attrs for finding the events file in fast5 file,
                                                           default is BaseCalled_template

## Example


For revising the fast5 files in ./unitest/test_data/fast5/ in order to get .fasta files,the command line would be:

    $ conda activate nanorev  #activate the python enviroment for nanoreviser
    $ pyton -d ./unitest/test_data/fast5/ -o ./unitest_nanorev_results/ -F fasta

For revising the fast5 files in ./unitest/test_data/fast5/ in order to get .fastq files,the command line would be:

    $ conda activate nanorev  #activate the python enviroment for nanoreviser
    $ pyton -d ./unitest/test_data/fast5/ -o ./unitest_nanorev_results/ -F fastq

Please run the following command in oder to get the entire fasta or fastq file contains all reads in fasta5's dir:

    $ cat ./nanorev_results/*.fasta > nanorev_results.fasta 
or
    
    $ cat ./nanorev_results/*.fastq > nanorev_results.fastq


## Citation


Luotong Wang, Li Qu, Longshu Yang, Yiying Wang Huaiqiu Zhu; NanoReviser: An Error-correction Tool for Nanopore Sequencing Based on a Deep Learning Algorithm


## Contact


Please direct your questions to: Dr. Huaiqiu Zhu, [hqzhu@pku.edu.cn](hqzhu@pku.edu.cn)


