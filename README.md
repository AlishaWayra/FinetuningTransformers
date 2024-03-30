## Project Description
This is a **small version** of the original **NLP project about finetuning DistilBERT and DistilGPT2 on the Yelp review dataset** to predict the usefulness of reviews. Here, I show the finetuning part of **DistilBERT** on a smaller set of samples (i.e. only 3,000) so it can be run locally or on Google Colab without paying for additional GPU. Originally, DistilBERT was trained on roughly 5 Million reviews. Hence, this repository is to showcase the project's code only minorly adjusted to accomodate a smaller sample size. 

Overall, we **froze and used the pretrained weights of DistilBERT** to create review embeddings and then added a **3-linear-layer head** to predict the normalized usefulness score $[0,1]$ of each review. 

## Dataset
[validation](https://drive.google.com/file/d/1-0qgoygEN2Vd6U459-tmfQPu4r22B44q/view?usp=sharing) [test](https://drive.google.com/file/d/1-1eY1-sr3lTD_n5PEKICJaxSEy9jAjvJ/view?usp=sharing) [train](https://drive.google.com/file/d/13TaI26C05wC3Sim87trsTZXYJoEcw7z6/view?usp=sharing)
## Structure of Files

## Sources
