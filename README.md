# multilingual-sentence-embedding

## Introduction
Neural Machine Translation Models are widely used in the present day to connect the world together by breaking the language barriers. The access to information becomes as easy as ever with these pieces of technology. Despite the high performances of the current translation models, there are still various aspects of the model that can be improved and are waiting to be explored. With this knowledge, we decided to perform an experiment on the best method to get the most similar sentences when doing translation tasks. We used various methods of back-translation and compared whether there are potential candidates that can outperform the ‘baseline’ similarities of the original pair we achieved from parallel corpus. The test is performed on ten thousand English-Thai sentence pairs fetched from the English-Thai Machine Translation Dataset provided by VISTEC. We expected some degree of noise during the back-translation process (Khayrallah & Koehn, 2018) so the final result that we found out is quite the contrary of what we hypothesized in the first place. In the end, every back-translated sentences whether they are English-Translated English from Thai or Thai-Translated Thai from English outperforms the original English-Thai sentence pairs.

## Methodology
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
<u>คิดว่าเหมือน methodology. เลยไม่ทราบว่าต้องใส่อะไรค่ะ</u>


## Results

|Model|Translator|Translate|Compare|Average Cosine|
|:---:|   :---:  |  :---:  |  :---: |:---:      |
|SBERT|base-line| -     | th-en |0.641  |
|     | SCB     |th-en  | en-en |**0.803**|
|     |Helsinki |th-en  | en-en |0.715  |
|     |google   |th-en  | en-en |0.701  |
|     | SCB     |en-th  | th-th |*0.783*  |
|     |google   |en-th  |th-th  |0.697  |
|WangchanBERTa| base-line|None|None|None|
|     |SCB      | th-en | en-en |**0.778**|
|     |Helsinki |  th-en|  en-en| 0.676 |
|     |Google   |  th-en|  en-en| 0.664 |
|     |SCB      | en-th |th-th  |0.660  |
|     |Google   |en-th  |th-th  |0.513  |
|MBERT| base-line| -    | th-en |0.641  |
|     | SCB     | th-en | en-en |**0.864**|
|     |Helsinki | th-en |en-en  |0.798  |
|     |Google   |  th-en|  en-en| 0.794 |
|     |SCB      | en-th | th-th |0.741  |
|     |Google   |en-th  |th-th  |0.720  |

*Due to an accident, the result of MBERT, th-th SCB and google is not yet finished. '\*' sign means that it's was only 100 samples evaluation.  

As we can see, the base-line average cosine similarity scores was the lowest compared to the translated one. And the cosine similarity of the english-english embedding pairs seems to have the highest score, followed by the thai-thai sentences embedding pairs score.  


Thus, we can see that the cosine similarity of the sentences are having the influences of the language, If the language are the same, the more of the similarity its have. Eventhough, the sentences may have noise from the translation process, the influences of the same language has out perform the noise.
### *Test on Parapharsed dataset*
|Model |Thai-Thai(Original)|Thai-Eng(G)|Eng(G)-Eng(G) | 
|:---:|:---:|:---:|:---:|
|SBERT|0.907|0.591|0.760|
|WangChanBERTa|0.858|0.066|0.747|
|MBERT|0.892|0.674|0.908|

*G* = Using Google translate as a translation tool. As google translate provided an unbias translation and both English to Thai and Thai to English translation.
   
After we have test on the paraphrased dataset, most of the result was accordingly to our finding, the same langauge embedding (Thai-Thai, Eng-Eng) are having more cosine similarity than the cross-language embedding.(Thai-Eng)

However, It's noteworthy that MBERT ,*Multilingual*-BERT, might prefer english languauge more than other languages as the English-English translated acquired more similarity than Thai-Thai (the original dataset).  


## Conclusion
The back-translated sentences when compared with the original sentence in the same language will always outperform the original language pair in term of sentence similarity. The result is quite surprising since we expected that the back-translated sentences will be affected by noise during the translation but the in the end it is the opposite of what we initially hypothesized. However, we still don't know whether the evaluation models have bias on when comparing the same language sentences or not so this can be conducted in future studies.

