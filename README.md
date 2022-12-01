# multilingual-sentence-embedding

## Introduction
- Motivation
- What other have done + What models have they used: A short Literature Review (About 1-2 paragraphs)
- Summarize your idea
- Summarize your result

## Approach/Methodology/Model
Our work consists of two important steps; translation and evaluation. The process starts with translation, where we translate selected sentences from the corpus. If English sentences are used as an input, the outputs of the translation are Thai sentences, vice versa. The translation methods that we selected are neural machine translation models, namely,
- Helsinki-NLP’s OPUS MT
- VISTEC’s English-Thai Machine Translation Model
- Google’s Google Translate.

Next, for the evaluation section, we compare the sentence similarity of sentence pairs to find the pair with the best similarity score. The inputs are the original English-Thai pair as the baseline, English-Translated English from Thai, and Thai-Translated Thai from English, while the outputs are the similarity score we get from each pair. Then we compare the similarity score between each translation model and the baseline to determine whether the translated counterpart performs better or not. The following are the evaluation methods we used:
- UKPLab’s sentence-transformers (SBERT)
- FreddeFrallan’s Multilingual-CLIP (MBERT)
- VISTEC’s WangchanBERTa

## Dataset
### *data*
We have been using *English-Thai Machine Translation Dataset* provided by VISTEC-depa Thailand Artificial Intelligence Research Institute. Conditionally have to be translated by professional translators, about 356,063 segment pairs from 2 sub-datasets was left. Then 10,000 pairs were randomly selected to be our data.

|Corpus| Sub-dataset | number of segment pairs | Selected|
|:---: |   :---:     |  :---: |  :---:           |
 English-Thai Machine Translation Dataset| Taskmaster-1 | 222,733| 10,000|
|| Product Reviews Translation | 133,330||

### *evaluation set*
The dataset was paraphrased by human, using source data from Prachatai News sites (Prachatai-67k dataset). After using sentences tokenizer provided by pythainlp, chose the sample of each article, and hand selected by human. Resulted as 100 sentences from 100 different articles. 

|Corpus             |Number of articles  | Selected     |
|   :---:           |   :---:            |   :---:      |
|Phachatai-67K dataset |   67889 articles   | 100 sentences|


<u>Example</u>  
- Original data   &nbsp; &nbsp; &nbsp; : &nbsp; เมื่อบริษัทกัลฟ์ ฯ นำเรือออกสำรวจปลาวาฬ แต่ชาวบ้านให้นำเรือมาตรวจสอบก่อน  
- Paraphrased data :&nbsp; ชาวบ้านให้นำเรือมาตรวจสอบก่อน ก่อนบริษัทกัลฟ์ ฯ นำเรือออกสำรวจวาฬ
  


## Experiment Setup
- Which pre-trained model? How did you pre-train embeddings?
- How long?
- Hyperparameter tuning? Dropout? How many epochs?


## Results
- How did it go? + Interpret results (which one is the best with explaination; read the table to reinforce the result)

|Model|Translator|Translate|Compare|Average Cosine|
|:---:|   :---:  |  :---:  |  :---: |:---:      |
|SBERT|base-line| -| th-en|0.591|
|     | SCB | th-en| en-en | 0.803|
|     |Helsinki |  th-en|    en-en|0.715|
|     |google|th-en|en-en|0.701|
|     |google|en-th|th-th|0.697|
|WangchanBERTa| base-line|
|      |SCB | th-en| en-en | 0.778|
|     |Helsinki |  th-en|    en-en| 0.676|
|     |Google   |
|     |SCB| en_th|**th-th|0.668|
|     |Helsinki| en-th|th-th|??|
|     |Google   |
|**MBERT| base-line| -| th-en|0.651|
|| SCB| th-en| en-en |0.842|
|     |Helsinki| th-en|en-en|0.787|
|     |SCB     | en-th | th-th|0.757|
|     |

*Due to accident, the result of MBERT, th-th SCB and google was not yet finished. If it's has '\*' sign in the table means that it's was only 100 sample evaluation.  

As we can see, the base-line average cosine similarity scores was the lowest compared to the translated one. And the cosine similarity of the english-english embedding pairs seems to have the highest score, followed by the thai-thai sentences embedding pairs score.

//เรื่องว่ามี bias เรื่องภาษาเดียวกัน ค่า  similarity ของ embedding สูงกว่า 
### *Test on Parapharsed dataset*
|Model |Thai-Thai(Original)|Thai-Eng(G)|Eng(G)-Eng(G) | 
|:---:|:---:|:---:|:---:|
|SBERT|0.907|0.577|0.760|
|WangChanBERTa|0.858|0.066|0.747|
|MBERT|0.892|0.666|0.908|





## Conclusion
- What did we do?
- What task?
- Summary of result
- Literally re-phrasing your work
