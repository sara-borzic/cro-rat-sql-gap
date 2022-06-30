# cro-RAT-SQL+GAP - Multilingual version of the RAT-SQL+GAP

Based on: RAT-SQL+GAP: [Github](https://github.com/awslabs/gap-text2sql). Paper: [AAAI 2021 paper](https://arxiv.org/abs/2012.10309)


## Abstract

cro-RAT-SQL+GAP is a multilingual version of the RAT-SQL+GAP, wich start with Croatian Language. Here is available the code, dataset and the results.


## Directory Structure
```bash
cd text2sql-cro/cro-rat-sql-gap
```

## Conda mtext2slq Environment Setup
```bash
conda create --name text2sql-cro python=3.7
conda activate text2sql-cro
conda install pytorch=1.5 cudatoolkit=10.2 -c pytorch
pip install gdown
conda install -c conda-forge jsonnet
pip install -r requirements.txt
python -c "import nltk; nltk.download('stopwords'); nltk.download('punkt')"
conda install jupyter notebook
conda install -c conda-forge jupyter_contrib_nbextensions

```



## Setup Script
Just run this script below, it will copy the datasets.
The original version of the Spider dataset is distributed under the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/legalcode) license.
The modified versions (translated to Portuguese, Spanish, French, double-size(English and Portuguese) and quad-size (English, Portuguese, Spanish and French)) of train_spider.json, train_others.json, and dev.json are distributed under the [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/legalcode) license, respecting ShareAlike.

```bash
chmod +x setup.sh
./setup.sh
```

## Specific setup
The models and checkpoints have big files (GBytes), so if you have enough disk space you can run all shell scripts. To understand how things work, run just BART_large.sh and after run the others.
```bash
./BART_large.sh
./mBART50MtoM-large.sh
./BERTic.sh
```

## Environment Test
Now the environment is ready for training (fine-tune) and inferences. The training is very slow more than 60 hours for BART, BERTic and mBART50.

### Preprocess dataset
This preprocess step is necessary both for inference and for training. It will take some time, maybe 40 minutes.
I will use the script for BART, but you can use the other, look the directory experiments/spider-configs
```bash
python run.py preprocess experiments/spider-configs/spider-BART-large-en-train_en-eval.jsonnet
```
You can see the files processed in the paths:
`data/spider-en/nl2code-1115,output_from=true,fs=2,emb=bart,cvlink`

## Inference
I will use the script for BART again. 
Note: We are making inferences using the checkpoint already trained (directory logdir) and defined in:
`experiments/spider-configs/spider-BART-large-en-train_en-eval.jsonnet`
`logdir: "logdir/BART-large-en-train",` and  
`eval_steps: [40300],`
```bash
python run.py eval experiments/spider-configs/spider-BART-large-en-train_en-eval.jsonnet
```

You then get the inference results and evaluation results in the paths:

`ie_dirs/BART-large-en-train/bart-large-en_run_1_true_1-step41000.infer` 

and 

`ie_dirs/BART-large-en-train/bart-large-en_run_1_true_1-step41000.eval`.

## Training
Execute if it is really necessary, if you want to fine-tune the model, this will take a long time. But if you have a good machine available and want to see different checkpoints in the logdir, do it.

```bash
python run.py train experiments/spider-configs/spider-BART-large-en-train_en-eval.jsonnet
```
You then get the training checkpoints in the paths:
`logdir/BART-large-en-train`

## Results

All intermediate files of the results are in the directory [ie_dirs]

## Security

See [CONTRIBUTING](CONTRIBUTING.md#security-issue-notifications) for more information.

## License

This project is licensed under the Apache-2.0 License.
