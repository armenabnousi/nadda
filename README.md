# NADDA: No-Alignment Domain Detection Algorithm for protein sequences </br>


######This file will guide you through the steps to install and run the NADDA conserved region detection program.
-----------------------------------------------------------------------------------------------------------
The current release includes three different modules for data set generation and for conserved region detection.
You will need to provide a fasta file (and the Pfam output if you want to use your own training set) to the 
dataset generation modules and then feed the generated datasets to the prediction module for detection of 
conserved regions. Here we will address each of these two steps separately.</br>
A default trained model is also provided. If you want to use our pre-trained model, 
unzip model.default.zip file and make sure the resulting model.default file is in the same directory as
your nadda.py file is. 

----------------------------------------------------------------------------------------------------------
**Dataset Generation (MapReduce):** </br>
Run `make` to generate profile executable file. You will need to have MRMPI and BOOST libraries installed.
(Set the path to MRMPI library in the Makefile.)</br>
There are two script files that use profiler, one for generating training datasets and the other for generating
test datasets.</br>
</br>
The `train_dataset_gen.sh` is used for generating training datasets. You are asked to enter the fasta file for 
training sequences and the file for the Pfam output of these sequences.
</br>
The `test_dataset_gen.sh` is used for generation of test sets. You are asked to enter the fasta file including the
sequences that you want to run the NADDA on.
</br>
</br>
Set the name for following parameters in the script file (dataset_gen.sh):</br>
*Fasta file name (make sure there is no multiple copies of one sequence name in the file.)</br>
*pfam output for the given file (for train_dataset_gen.sh) (make sure names in pfam output does exactly match the names in fasta file).</br>
*You can also set the kmer size and dataset file name here. By default it kmer size is set to 6.</br>
*A default threshold of "1" is set in this script file. This is the minimum number of sequences required to 
include a domain in order for that domain to be marked as a domain region (based on Pfam) in the data set.
This value is suggested to be set to a larger number (e.g. 50) when generating a  training dataset.</br>
For the test dataset, the threshold will only affect the scores of prediction when compared to Pfam, but does
not change the results of prediction.</br>
*The parameter sep_delta is used by the MRMPI library. Make sure its value is larger than the longest sequence in 
the fasta file.</br>
</br>
*You will also need to make any changes required to submit a job your cluster. The current files use mpiexec and are
set to use 32 processors.</br>

-----------------------------------------------------------------------------------------------------------
**Conserved region detection: (NADDA)**</br>
You will need python v3+ installed. You will also need to install scikit-learn and pickle on your python.
Run the nadda.py file in the following format:

```
python nadda.py -i (training dataset file) -t (test dataset file) -m (mss) -v (max features) -w (window size) -M (model file) -k (kmer size)
```

Test file (-t) is the only mandatory field to run the program. </br>

Make sure that the kmer size is the same used for generation of the data sets. A default value of 6 is used here.</br>

A default model is provided with this release. Without entering any training file and model file, the default
model will be loaded.</br>

If a training file and model file name are provided, the model trained using the training set will be stored 
under the given model name. Otherwise a default name using the training file name will be used.</br>

If a training file is not provided but a model file name is entered, the script will try to load a model from 
the disk stored under that name.</br>

The -m, -v and -w options are classifier parameters and are optional.
