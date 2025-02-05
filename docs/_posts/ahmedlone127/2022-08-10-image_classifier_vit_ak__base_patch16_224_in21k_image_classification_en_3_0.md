---
layout: model
title: English image_classifier_vit_ak__base_patch16_224_in21k_image_classification ViTForImageClassification from amitkayal
author: John Snow Labs
name: image_classifier_vit_ak__base_patch16_224_in21k_image_classification
date: 2022-08-10
tags: [vit, en, images, open_source]
task: Image Classification
language: en
edition: Spark NLP 4.1.0
spark_version: 3.0
supported: true
annotator: ViTForImageClassification
article_header:
  type: cover
use_language_switcher: "Python-Scala-Java"
---

## Description

Pretrained VIT  model, adapted from Hugging Face and curated to provide scalability and production-readiness using Spark NLP.`image_classifier_vit_ak__base_patch16_224_in21k_image_classification` is a English model originally trained by amitkayal.


## Predicted Entities

`1647932708266`, `1647153073068`, `1648120851131`, `1647323870424`, `1647324831223`, `1648529040870`, `1647667510657`, `1647838548516`, `1647491988625`, `1647068900550`, `1647241002660`, `1647155940977`, `1648444866188`, `1648287279256`, `1648360065352`, `1646891748795`, `1647175174130`, `1647667513009`, `1647503446037`, `1648616524252`, `1647241522127`, `1648454681973`, `1647581857611`, `1647233597062`, `1647933757599`, `1647420385320`, `1648361316938`, `1647856085470`, `1647243922705`, `1647497162634`, `1647237935761`, `1648369037326`, `1648115339027`, `1647153746047`, `1648273037858`, `1647150662937`, `1646893741640`, `1647845708343`, `1647147746930`, `1648366090438`, `1647156193650`, `1648537457250`, `1647149733777`, `1648443306030`, `1648646879260`, `1648001685069`, `1648528121469`, `1647156345180`, `1648456544611`, `1648107120561`, `1648359826755`, `1648366661601`, `1647666899143`, `1647935446369`, `1647668429439`, `1647936918167`, `1647235612241`, `1648041520955`, `1647243637048`, `1647680921307`, `1647081327122`, `1647087753595`, `1648528673166`, `1648710643516`, `1647945116578`, `1647846670493`, `1648536302434`, `1647761641671`, `1647325936506`, `1647325395017`, `1647234311073`, `1647759532201`, `1647241685563`, `1647935761690`, `1647846942176`, `1648698605364`, `1647933173332`, `1647420602021`, `1647159771571`, `1647324549954`, `1647065648037`, `1648536755030`, `1647924696448`, `1647927510285`, `1646892894926`, `1647580734898`, `1648287633901`, `1648442962568`, `1648368434262`, `1646988520214`, `1648279394220`, `1647150432271`, `1648643933549`, `1647448253535`, `1647929786244`, `1648370352609`, `1647330838532`, `1647147396410`, `1648644032811`, `1647140664832`, `1648536664835`, `1647410160165`, `1647164611497`, `1648183419560`, `1647773005511`, `1646034720737`, `1647328253213`, `1647155473555`, `1647953067595`, `1648538213890`, `1647409255750`, `1647682028193`, `1648116419206`, `1647329803500`, `1647154529007`, `1648099843046`, `1647248948963`, `1648279061829`, `1648296194026`, `1648108046332`, `1648113127522`, `1648455583722`, `1647761642926`, `1648533677853`, `1647940472293`, `1648701651498`, `1648456403645`, `1647752727972`, `1647494398054`, `1647674319450`, `1646887547179`, `1647158162746`, `1647176402807`, `1647065469305`, `1647838279102`, `1647674991869`, `1648113569157`, `1647067760203`, `1648365815895`, `1647330963213`, `1647405478288`, `1648372401817`, `1648103720352`, `1648115162538`, `1647784242846`, `1647402768175`, `1647490410305`, `1648286780106`, `1648625933250`, `1648534124563`, `1647173945909`, `1647235889634`, `1648525350277`, `1647236596892`, `1648292928751`, `1648706018510`, `1648024698508`, `1648707239343`, `1647767885862`, `1647240848064`, `1648280849373`, `1648406457408`, `1647236473208`, `1647157232763`, `1647147535719`, `1648706506117`, `1648706245642`, `1647933556772`, `1648616269757`, `1646809025517`, `1647420348512`, `1647399162926`, `1647843749559`, `1647242632376`, `1648182014371`, `1647321722450`, `1648369375824`, `1647331957791`, `1647494265176`, `1647252866283`, `1647930894634`, `1646809256434`, `1648537084774`, `1648450201448`, `1646885340637`, `1647760733996`, `1648287529293`



{:.btn-box}
<button class="button button-orange" disabled>Live Demo</button>
<button class="button button-orange" disabled>Open in Colab</button>
[Download](https://s3.amazonaws.com/auxdata.johnsnowlabs.com/public/models/image_classifier_vit_ak__base_patch16_224_in21k_image_classification_en_4.1.0_3.0_1660167449128.zip){:.button.button-orange.button-orange-trans.arr.button-icon}
[Copy S3 URI](s3://auxdata.johnsnowlabs.com/public/models/image_classifier_vit_ak__base_patch16_224_in21k_image_classification_en_4.1.0_3.0_1660167449128.zip){:.button.button-orange.button-orange-trans.button-icon.button-copy-s3}

## How to use



<div class="tabs-box" markdown="1">
{% include programmingLanguageSelectScalaPythonNLU.html %}
```python

image_assembler = ImageAssembler() \
    .setInputCol("image") \
    .setOutputCol("image_assembler")

imageClassifier = ViTForImageClassification \
    .pretrained("image_classifier_vit_ak__base_patch16_224_in21k_image_classification", "en")\
    .setInputCols("image_assembler") \
    .setOutputCol("class")

pipeline = Pipeline(stages=[
    image_assembler,
    imageClassifier,
])

pipelineModel = pipeline.fit(imageDF)

pipelineDF = pipelineModel.transform(imageDF)
```
```scala

val imageAssembler = new ImageAssembler()
.setInputCol("image")
.setOutputCol("image_assembler")

val imageClassifier = ViTForImageClassification
.pretrained("image_classifier_vit_ak__base_patch16_224_in21k_image_classification", "en")
.setInputCols("image_assembler")
.setOutputCol("class")

val pipeline = new Pipeline().setStages(Array(imageAssembler, imageClassifier))

val pipelineModel = pipeline.fit(imageDF)

val pipelineDF = pipelineModel.transform(imageDF)

```
</div>

{:.model-param}
## Model Information

{:.table-model}
|---|---|
|Model Name:|image_classifier_vit_ak__base_patch16_224_in21k_image_classification|
|Compatibility:|Spark NLP 4.1.0+|
|License:|Open Source|
|Edition:|Official|
|Input Labels:|[image_assembler]|
|Output Labels:|[class]|
|Language:|en|
|Size:|322.4 MB|
