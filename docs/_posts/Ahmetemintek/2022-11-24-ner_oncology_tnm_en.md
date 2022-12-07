---
layout: model
title: Extract Entities Related to TNM Staging
author: John Snow Labs
name: ner_oncology_tnm
date: 2022-11-24
tags: [licensed, en, clinical, oncology, ner]
task: Named Entity Recognition
language: en
edition: Healthcare NLP 4.2.2
spark_version: 3.0
supported: true
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

This model extracts staging information and mentions related to tumors, lymph nodes and metastases.

## Predicted Entities

`Lymph_Node`, `Staging`, `Lymph_Node_Modifier`, `Tumor_Description`, `Tumor`, `Metastasis`, `Cancer_Dx`

{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
[Open in Colab](https://colab.research.google.com/github/JohnSnowLabs/spark-nlp-workshop/blob/master/tutorials/Certification_Trainings/Healthcare/27.Oncology_Model.ipynb){:.button.button-orange.button-orange-trans.co.button-icon}
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/clinical/models/ner_oncology_tnm_en_4.2.2_3.0_1669308699155.zip){:.button.button-orange.button-orange-trans.arr.button-icon}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python
document_assembler = DocumentAssembler()\
    .setInputCol("text")\
    .setOutputCol("document")

sentence_detector = SentenceDetectorDLModel.pretrained("sentence_detector_dl_healthcare","en","clinical/models")\
    .setInputCols(["document"])\
    .setOutputCol("sentence")

tokenizer = Tokenizer() \
    .setInputCols(["sentence"]) \
    .setOutputCol("token")

word_embeddings = WordEmbeddingsModel().pretrained("embeddings_clinical", "en", "clinical/models")\
    .setInputCols(["sentence", "token"]) \
    .setOutputCol("embeddings")                

ner = MedicalNerModel.pretrained("ner_oncology_tnm", "en", "clinical/models") \
    .setInputCols(["sentence", "token", "embeddings"]) \
    .setOutputCol("ner")

ner_converter = NerConverter() \
    .setInputCols(["sentence", "token", "ner"]) \
    .setOutputCol("ner_chunk")

pipeline = Pipeline(stages=[document_assembler,
                            sentence_detector,
                            tokenizer,
                            word_embeddings,
                            ner,
                            ner_converter])

data = spark.createDataFrame([["The final diagnosis was metastatic breast carcinoma, and it was classified as T2N1M1 stage IV. The histological grade of this 4 cm tumor was grade 2."]]).toDF("text")

result = pipeline.fit(data).transform(data)
```
```scala
val document_assembler = new DocumentAssembler()
    .setInputCol("text")
    .setOutputCol("document")
    
val sentence_detector = SentenceDetectorDLModel.pretrained("sentence_detector_dl_healthcare","en","clinical/models")
    .setInputCols("document")
    .setOutputCol("sentence")
    
val tokenizer = new Tokenizer()
    .setInputCols("sentence")
    .setOutputCol("token")
    
val word_embeddings = WordEmbeddingsModel().pretrained("embeddings_clinical", "en", "clinical/models")
    .setInputCols(Array("sentence", "token"))
    .setOutputCol("embeddings")                
    
val ner = MedicalNerModel.pretrained("ner_oncology_tnm", "en", "clinical/models")
    .setInputCols(Array("sentence", "token", "embeddings"))
    .setOutputCol("ner")
    
val ner_converter = new NerConverter()
    .setInputCols(Array("sentence", "token", "ner"))
    .setOutputCol("ner_chunk")

        
val pipeline = new Pipeline().setStages(Array(document_assembler,
                            sentence_detector,
                            tokenizer,
                            word_embeddings,
                            ner,
                            ner_converter))    

val data = Seq("The final diagnosis was metastatic breast carcinoma, and it was classified as T2N1M1 stage IV. The histological grade of this 4 cm tumor was grade 2.").toDS.toDF("text")

val result = pipeline.fit(data).transform(data)
```
</div>

## Results

```bash
| chunk            | ner_label         |
|:-----------------|:------------------|
| metastatic       | Metastasis        |
| breast carcinoma | Cancer_Dx         |
| T2N1M1 stage IV  | Staging           |
| 4 cm             | Tumor_Description |
| tumor            | Tumor             |
| grade 2          | Tumor_Description |
```

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|ner_oncology_tnm|
|Compatibility:|Healthcare NLP 4.2.2+|
|License:|Licensed|
|Edition:|Official|
|Input Labels:|[sentence, token, embeddings]|
|Output Labels:|[ner]|
|Language:|en|
|Size:|34.2 MB|

## References

In-house annotated oncology case reports.

## Benchmarking

```bash
              label   tp  fp  fn  total  precision  recall   f1
         Lymph_Node  570  77  77    647       0.88    0.88 0.88
            Staging  232  22  26    258       0.91    0.90 0.91
Lymph_Node_Modifier   30   5   5     35       0.86    0.86 0.86
  Tumor_Description 2651 581 490   3141       0.82    0.84 0.83
              Tumor 1116  72 141   1257       0.94    0.89 0.91
         Metastasis  358  15  12    370       0.96    0.97 0.96
          Cancer_Dx 1302  87  92   1394       0.94    0.93 0.94
          macro_avg 6259 859 843   7102       0.90    0.90 0.90
          micro_avg 6259 859 843   7102       0.88    0.88 0.88
```