
Jointly Learning Latent Categorizations of Interview Prompts to Predict

Depression in Screening Interview Transcripts

Proceedings of ACL 2020

Alex Rinaldi - Department of Computer Science, UC Santa Cruz - arinaldi@ucsc.edu

Jean E. Fox Tree - Department of Psychology, UC Santa Cruz - foxtree@ucsc.edu

Snigdha Chaturvedi - Department of Computer Science, University of North Carolina at Chapel

Hill - snigdha@cs.unc.edu

This repository contains code to reproduce results from our ACL 2020 paper. A Python 3.6.7

environment with the following packages is required:

 mxnet 1.4.1

 gluonnlp 0.6.0

 scikit-learn 0.20.1

 scipy 1.1.0

 numpy 1.14.6

 tensorflow 1.13.1

Here is a description of pertinent files and folders:

 models – Class files for baseline and proposed models

 train\_export.py \- Train and output development and test predictions for any of our models

(instructions below)

 data\_provider.py - Parses and preprocesses conversation transcripts into a turn structure

 features\_provider.py - Extracts averaged glove embedding features for each prompt/response

 evaluate\_metrics.py - Evaluate the F1+, F1-, and accuracy scores for a trained model's

development or test predictions (instructions below)

 evaluate\_significance.py - Evaluate two models' performances for statistical significance

(instructions below).

Data

Access to the DAIC dataset must be requested individually. Once the data has been obtained, the csv

files for transcripts should be placed in the Data directory, keeping the default filenames

(<participant\_number>\_TRANSCRIPT.csv). Additionally, include the following files:

 full\_test\_split.csv

 test\_split\_Depression\_AVEC2017.csv

 train\_split\_Depression\_AVEC2017.csv

 dev\_split\_Depression\_AVEC2017.csv

The already-existing file participant\_indices.txt contains the pool of all participant IDs used to generate

the splits.

Train/test/development splits are chosen by a seeded random number generator. The file

reported\_splits.txt contains the splits used to report results for our paper.

Training

To reproduce results for our models, first train the model using one of the following commands:

1\. PO

python train\_export.py \--experiment\_prefix=glove\_reprod\_prompt\_baseline \--train\_size=0.7

\--dev\_size=0.2 \--batch\_size=10 \--num\_epochs\=2000 \--num\_tests=10 \--evaluate\_every=100

\--model=prompt\_baseline \--prompt\_latent\_type\_\_dropout\_keep=0.9 \--learning\_rate=0.003

\--save\_final\_epoch=True

2\. RO

python train\_export.py \--experiment\_prefix=glove\_reprod\_response\_baseline \--train\_size=0.7

\--dev\_size=0.2 \--batch\_size=10 \--num\_epochs\=2000 \--num\_tests=10 \--evaluate\_every=100

\--model=response\_baseline \--prompt\_latent\_type\_\_dropout\_keep=0.9 \--learning\_rate=0.003

\--save\_final\_epoch=True

3\. PR

python train\_export.py \--experiment\_prefix=glove\_reprod\_promptresponse\_baseline \--train\_size=0.7

\--dev\_size=0.2 \--batch\_size=10 \--num\_epochs\=2000 \--num\_tests=10 \--evaluate\_every=100

\--model=promptresponse\_baseline \--prompt\_latent\_type\_\_dropout\_keep=0.9 \--learning\_rate=0.003

\--save\_final\_epoch=True

4\. JLPC

python train\_export.py \--experiment\_prefix=reprod\_glove\_latenttype\_k11entropy \--train\_size=0.7

\--dev\_size=0.2 \--batch\_size=10 \--num\_epochs\=1700 \--num\_tests=10 \--evaluate\_every=100

\--model=prompt\_latent\_type\_latent\_entropy \--prompt\_latent\_type\_\_num\_channels=11

\--prompt\_latent\_type\_\_dropout\_keep=0.9 \--entropy\_coefficient\=0.1 \--learning\_rate=0.0005

\--save\_final\_epoch=True

5\. JLPCPre

python train\_export.py \--experiment\_prefix=glove\_latenttype\_deepconcatk11entropy \--train\_size=0.7

\--dev\_size=0.2 \--batch\_size=10 \--num\_epochs\=1400 \--num\_tests=10 \--evaluate\_every=100

\--model=prompt\_latent\_type\_concat\_latent\_entropy \--prompt\_latent\_type\_\_num\_channels\=11

\--prompt\_latent\_type\_\_dropout\_keep=0.9 \--entropy\_coefficient=1 \--learning\_rate=0.001

\--save\_final\_epoch=True

6\. JLPCPost

python train\_export.py \--experiment\_prefix=glove\_latenttype\_concatk11entropy \--train\_size=0.7

\--dev\_size=0.2 \--batch\_size=10 \--num\_epochs\=1300 \--num\_tests=10 \--evaluate\_every=100

\--model=prompt\_latent\_type\_concatv2\_latent\_entropy \--prompt\_latent\_type\_\_num\_channels=11

\--prompt\_latent\_type\_\_dropout\_keep=0.9 \--entropy\_coefficient\=0.1 \--learning\_rate=0.0005

\--save\_final\_epoch=True

Each model will output the following line when training is finished:

"Wrote everything to <path>/runs/<experiment\_prefix>/<results\_set\_number>"

Evaluation

To evaluate the results of a trained model, run one of the following commands:

1\. Dev set: F1 +/-, Accuracy

python evaluate\_metrics.py runs/<experiment\_prefix>/<results\_set\_number> 10 dev\_predictions.csv

2\. Test set: F1 +/- Accuracy

python evaluate\_metrics.py runs/<experiment\_prefix>/<results\_set\_number> 10 test\_predictions.csv

The outputs of these two commands are formated as:

F1 pos: <mean> (<stdev>) : <median>

F1 neg: <mean> (<stdev>) : <median>

Accuracy: <mean\> (<stdev>) : <median>

3\. Test set statistical significance

python evaluate\_metrics.py runs/<baseline\_experiment\_prefix\>/<results\_set\_number>

runs/<model\_experiment\_prefix>/<results\_set\_number> 10

The output for this command follows the format provided by the scipy package's ttest\_ind functionality.

![](data:image/png;base64,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)