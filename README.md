## Project Description
This is a **scaled-down version** of the original **NLP project about finetuning DistilBERT and DistilGPT2 on the Yelp review dataset** to predict the number of usefulness votes a review may receive for a given business. Here, I show the finetuning part of **DistilBERT** on a smaller set of samples (i.e. only 3,000), making it feasible to run locally or on Google Colab without the need for additional GPU resources. Originally, DistilBERT was trained on roughly 5 Million reviews. Hence, this repository is to showcase the embedding and training procedure only minorly adjusted to accomodate a smaller sample size. 

Overall, the **pretrained weights of DistilBERT were frozend and used** to create review embeddings based on the linguistic characteristics of the review text. A **3-linear-layer head** was then trained on the DistilBERT embeddings to predict the normalized usefulness score between $[0,1]$ of each review. 

## Dataset
The [Yelp Open Dataset](https://www.yelp.com/dataset) is publicly available and consists of **6,990,280 reviews** given to 150,000 businesses located in 11 metropolitan areas in the US, as of Wednesday 8th February 2023. It consists of several JSON data files, of which the *review.json* and *business.json* files were used. They contain review texts in full and their associated vote tallies by category, as well as additional business metadata, respectively. Here are some sample review texts along with the number of usefulness votes received. 

| Review Text                                          | Useful        |
| ---------------------------------------------------- | ------------- |
| I really enjoyed eating here and the at...           | 0             |
| I have been a long time Panera lover ...             | 2             |
| What a good little spot to eat! They ...             | 1             |
| I'd like to begin this review by ment ...            | 6             |
| Plus three stars for the food and for ...            | 1             |

The **distribution of usefulness votes is subject to statistical skew**, as **55% of reviews were assigned 0 votes**, with a **mean of 1.185 +/- 3.254 usefulness votes** and the maximum number of votes received by a review is 1,182. Hence, the data was preprocessed to **compute a normalized usefulness vote** as a metric to evaluate reviews by dividing the review usefulnes vote counts by the maximum such vote count received by the business. The dataset was split into training (70%), validation (10%) and test (20%) sets. Each dataset was further subdivided across several 200-MB CSV files. For the scaled-down version, only one of the 200-MB CSV files was considered for each in training (3,000 reviews), validation (400 review) and test (800 reviews).

The datasets for [training](https://drive.google.com/file/d/13TaI26C05wC3Sim87trsTZXYJoEcw7z6/view?usp=sharing), [validation](https://drive.google.com/file/d/1-0qgoygEN2Vd6U459-tmfQPu4r22B44q/view?usp=sharing), and [test](https://drive.google.com/file/d/1-1eY1-sr3lTD_n5PEKICJaxSEy9jAjvJ/view?usp=sharing) can be found on Google Drive.

## Model Fine Tuning
First, the pretrained DistilBERT tokenizers and model were used to tokenize and precompute the embeddings for the review texts and saved for later use, allowing to experiment with different heads. From the initial experiments, it was found that the following 3-linear-layer head led to the best performance in predicting the target values from the embeddings:

**Head**
- linear(768,384) + ReLU + BatchNorm
- linear(384,384) + ReLU + BatchNorm
- linear(384,1) + Sigmoid

## Structure of Files
The jupyter notebook *distilbert-embeddings.ipynb* imports the raw csv-files and applies DistilBERT tokenizer and pretrained weigths to create review embeddings. Those are then saved in binary form using the Hierarichal Data Format 5 (HDF5). The benefit of HDF is that it compresses the embeddings into a smaller data files and those take up less memory during training. This file has to be run separately to create the embeddings for the train, validation and test dataset, and the DATASET and BATCHSIZE variable have to be adjusted accordingly. In file *distilbert-training.ipynb*, HDF5 embeddings are imported and reformatted, followed by a training loop finetuning the weights of the 3-layer head to the review embeddings. At the end, the model is evaluated against the test embeddings with some common metrics.

## Sources
[1] Lo, J., Relwani, A., Hadarean, C., Amrein, A. (2023) A Comparison of Using DistilBERT & DistilGPT-2 to Predict the Usefulness of Yelp Review. NLP Project - UCL. 
