# Dense Passage Retrieval in Conversational Search

The code is splitted between 2 different plateforms, which are Google Collab and Compute Canada. It also contains 2 other repos that are used in the implenetation of this project, DPR repo for "Dense Passage Retrieval", CAsT Dataset, MSMARCo Dataset. Another repo used for evaluation of the CAsT.


## Features
1. BASH files to submit Jobs on different clusters on ComputeCanada.
2. NoteBooks to used on Google Collabs. 
3. Dense retriever model is based on bi-encoder architecture.
4. Dense Passage Retrieval  inspired by [this](https://arxiv.org/abs/2004.04906) paper.
5. Related data pre- and post- processing tools.
6. Dense retriever component for inference time logic is based on FAISS index for the DPR paper.

## 


## Installation

Installation from the source. Python's virtual or Conda environments are recommended.

```bash
git clone https://github.com/AhmedHussKhalifa/Dense_Passage_Retrieval_in_Conversational_Search
cd Dense_Passage_Retrieval_in_Conversational_Search
```

This project is tested on Python 3.6+, PyTorch 1.2.0+ and Transformers 3.5.



### 1. Setup ComputeCanada and run a single trial:

```bash
sbatch ComputeCanada/pyTorch_DPR.sh
```

You might need to setup run the following on line through the command line if the prvious file did not work.


```bash 
module load python/3.6.3

# Replace virtual_DPR with where ever you create your virtual env.
virtualenv --no-download virtual_DPR
source virtual_DPR/bin/activate

pip install torch --no-index
pip install --no-index torch torchvision torchtext torchaudio
pip install --no-index 'transformers==3.0.2'
pip install spacy[cuda] --no-index

module load nixpkgs/16.09 gcc/7.3.0 cuda/10.1
export LD_LIBRARY_PATH=$CUDA_HOME/lib64:$LD_LIBRARY_PATH

module load python/3.6.3
module load faiss/1.6.2
deactivate

source virtual_DPR/bin/activate
python --version

```


### 2. Download DPR trained on Trivia multi hybrid retriever results and HF bert-base-uncased model:

```bash
wget https://dl.fbaipublicfiles.com/dpr/checkpoint/reader/nq-trivia-hybrid/hf_bert_base.cp
```

### 3. Download Car dataset and split it

```bash
sbatch ComputeCanada/create-json-job.sh
sbatch ComputeCanada/reformat_car.sh
```

### 4. Split the data into training, testing and validation (MSMARCO & CAsT):

```bash
sbatch ComputeCanada/PreprocessDataNoNeg.sh
sbatch ComputeCanada/merge-job.sh
```


### 5. To generate the dense embbeding on compute canada.

```bash
sbatch ComputeCanada/ctx_1.sh
```

### 6. For Cast inference

```bash
sbatch ComputeCanada/cast_inference.sh
```

### 7. Other parts we have combined our scripts between ComputeCanada and GoogleCollabs. 


### Note:

If you had some errors working with working Huggingface to download pretrained model you can follow these steps:

```bash
mkdir data/models
cd data/models

wget https://s3.amazonaws.com/models.huggingface.co/bert/bert-base-uncased.tar.gz
tar -xzf bert-base-uncased.tar.gz
mv bert_config.json config.json

wget https://s3.amazonaws.com/models.huggingface.co/bert/bert-base-uncased-vocab.txt
mv bert-base-uncased-vocab.txt vocab.txt

#### change the hf_models.py in dpr/models

def get_bert_tokenizer(pretrained_cfg_name: str, do_lower_case: bool = True):
    return BertTokenizer.from_pretrained('/home/YOUR_USERNAME/scratch/DPR/data/models/vocab.txt', do_lower_case=do_lower_case)


class HFBertEncoder(BertModel):
   def init_encoder(cls, cfg_name: str, projection_dim: int = 0, dropout: float = 0.1, **kwargs) -> BertModel:
        #cfg = BertConfig.from_pretrained(cfg_name if cfg_name else 'bert-base-uncased')
        cfg = BertConfig.from_pretrained('/home/YOUR_USER_NAME/scratch/DPR/data/models/')
```