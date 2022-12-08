# Back-translation and the Effects on Sentemce Similarity

## Introduction
Neural Machine Translation Models are widely used in the present day to connect the world together by breaking the language barriers. The access to information becomes as easy as ever with these pieces of technology. Despite the high performances of the current translation models, there are still various aspects of the model that can be improved and are waiting to be explored. With this knowledge, we decided to perform an experiment on the best method to get the most similar sentences when doing translation tasks. We used various methods of back-translation and compared whether there are potential candidates that can outperform the ‘baseline’ similarities of the original pair we achieved from parallel corpus. The test is performed on ten thousand English-Thai sentence pairs fetched from the English-Thai Machine Translation Dataset provided by VISTEC. We expected some degree of noise during the back-translation process (Khayrallah & Koehn, 2018) so the final result that we found out is quite the contrary of what we hypothesized in the first place. In the end, every back-translated sentences whether they are English-Translated English from Thai or Thai-Translated Thai from English outperforms the original English-Thai sentence pairs.

## Methodology
Our work consists of two important steps; translation and evaluation. The process starts with translation, where we translate selected sentences from the corpus. If English sentences are used as an input, the outputs of the translation are Thai sentences, vice versa. The translation methods that we selected are neural machine translation models, namely,
- Helsinki-NLP’s OPUS MT
- VISTEC’s English-Thai Machine Translation Model (thai2nmt)
- Google’s Google Translate.

Next, for the evaluation section, we compare the sentence similarity of sentence pairs to find the pair with the best similarity score. The inputs are the original English-Thai pair as the baseline, English-Translated English from Thai, and Thai-Translated Thai from English, while the outputs are the similarity score we get from each pair. Then we compare the similarity score between each translation model and the baseline to determine whether the translated counterpart performs better or not. The following are the evaluation methods we used:
- UKPLab’s sentence-transformers (SBERT)
- FreddeFrallan’s Multilingual-CLIP (MBERT)
- VISTEC’s WangchanBERTa

## Dataset
### *data*
We use the English-Thai Machine Translation Dataset provided by VISTEC Thailand Artificial Intelligence Research Institute. We choose 10,000 random pairs from the dataset from the total of 356,063 pairs that are translated by professional translators.

|Corpus| Sub-dataset | number of segment pairs | Selected|
|:---: |   :---:     |  :---: |  :---:           |
 English-Thai Machine Translation Dataset| Taskmaster-1 | 222,733| 10,000|
|| Product Reviews Translation | 133,330||

### *evaluation set*
For this step, the dataset is paraphrased by humans, using source data from Prachatai News Sites (Prachatai-67k dataset). After using the Sentence Tokenizer provided by PyThaiNLP, we choose the sample of each article, and hand-select them to be our samples, which results in 100 sentences from 100 different articles.

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
<u>คิดว่าเหมือน methodology. เลยไม่ทราบว่าต้องใส่อะไรค่ะ</u>


## Results

|Model|Translator|Translation|Comparison|Average Cosine|
|:---:|   :---:  |  :---:  |  :---: |:---:      |
|SBERT|baseline| -     | th-en |0.641  |
|     | thai2nmt|th to en  | en-en |**0.803**|
|     |Helsinki |th to en  | en-en |0.715  |
|     |Google   |th to en  | en-en |0.701  |
|     | thai2nmt|en to th  | th-th |*0.783*  |
|     |google   |en to th  |th-th  |0.697  |
|WangchanBERTa| baseline|None|None|None|
|     |thai2nmt |th to en | en-en |**0.778**|
|     |Helsinki |th to en|  en-en| 0.676 |
|     |Google   |th to en|  en-en| 0.664 |
|     |thai2nmt |en to th |th-th  |0.660  |
|     |Google   |en to th  |th-th  |0.513  |
|MBERT| baseline| -    | th-en |0.641  |
|     | thai2nmt|th to en | en-en |**0.864**|
|     |Helsinki |th to en |en-en  |0.798  |
|     |Google   |th to en|  en-en| 0.794 |
|     |thai2nmt |en to th | th-th |0.741  |
|     |Google   |en to th |th-th  |0.720  |

As we can see, the average cosine similarity score of baseline is the lowest of all when compared to the other translated counterparts. Moreover, the cosine similarity score of the English-English translated from Thai embedding pairs have the highest score, followed by the Thai-Thai translated from English sentences embedding pairs score. Additionally, pairs with thai2nmt tranlated sentences show the highest cosine similarity score which we hypothesize that it could be from the fact that thai2nmt uses the same dataset as our sampled data to train their model.

Thus, we can see that the cosine similarity of the sentences have the bias by being the same language. If the language of both pairs are the same, they will be more likely to have higher cosine similarity score. Even though the sentences may have noise from the translation process, the influence of the same language has outperformed the noise.

### *Test on Parapharsed dataset*
|Model |Thai-Thai (Original)|Thai-Eng (G)|Eng (G) - Eng (G) | 
|:---:|:---:|:---:|:---:|
|SBERT|0.907|0.591|0.760|
|WangChanBERTa|0.858|0.066|0.747|
|MBERT|0.892|0.674|0.908|

*G* = Using Google Translate as a translation tool. Google Translate provided an unbiased translation, both English to Thai and Thai to English translation.
   
After we have tested on the paraphrased dataset, most of the result was according to our finding, the same language embedding (English-English, Thai-Thai) has more cosine similarity score than the cross-language embedding (English-Thai).

However, it is worth to note that MBERT, Multilingual-BERT, might prefer the English sentence since the English-English translation receives higher similarity score than Thai-Thai (the original dataset).


## Conclusion
The back-translated sentences when compared with the original sentence in the same language will always outperform the original language pair in terms of sentence similarity. The result is quite surprising since we expected that the back-translated sentences will be affected by noise during the translation but in the end it is the opposite of what we initially hypothesized. However, we still don't know whether the evaluation models have bias when comparing the same language sentences or not so this can be conducted in future studies.


