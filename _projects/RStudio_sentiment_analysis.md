---
layout: page
title: Sentiment Analysis using 'syuzhet' package in RStudio 
description: 
img: %assets/img/Cloud_BuenosAires.png
importance: 3
category: work
related_publications: false
class: text-center
---
The objective of this post is to perform a sentiment analysis on the novel "Buenos Aires en el año 2080 (1879)" by Aquiles Sioen, using the R package called `syuzhet`, developed by Matthew Jockers.

## Sentiment analysis


Sentiment analysis is an umbrella term used to describe a wide range of automated processes aimed at determining the degree of positivity or negativity of sentiments in a given text. There are numerous algorithms available in various programming languages. Additionally, there are various lexicons in multiple languages with terms associated with different sentiments.

Sentiment analysis has been used in various contexts such as politics or marketing. In literary studies, this technique has been met with some skepticism because it is difficult to attribute feelings to a subjective text. Furthermore, lexicons have been developed in English, and some of the versions available in other languages (such as Spanish in this case) are literal translations from English.

## Analysis of 'Buenos Aires en el año 2080'


The first step is installing the necessary packages for our analysis. In addition to the `syuzhet` package, we will install:

```R
# Install the Packages
install.packages("syuzhet")
install.packages("RColorBrewer")
install.packages("wordcloud")
install.packages("tm")

# Load the Packages
library(syuzhet)
library(RColorBrewer)
library(wordcloud)
library(tm)
```
Next, since we have already cleaned the text in TXT format in a <a href="https://carmen-carrasco.github.io/projects/spaCy/">previous post</a>, we have it like this:

```txt
5559 — Imprenta del "Porvenir,” calle Defensa 139. AL SEÑOR DON ANTONINO CAMBACERES PRESIDENTE DE LA ADMINISTRACION DEL FERRO-CARRIL DEL OESTE Señor: Este librito, en el que, á la manera de Julio Verne, de Mery y del autor anónimo de la batalla de Dorking, se hace un bosquejo del Porvenir que espera á vuestra República, no podía ménos que dedicarse á un gran Administrador, á un Político prudente, honrado y liberal; en fin, á un amante apasionado del Progreso bajo todas sus formas. Hé ahí, en verdad, las cualidades que habrán de sobresalir en vuestros hombres de Estado, si desean asegurar para la Patria Argentina la prosperidad que, sin temor de equavocarme, se la puede augurar, y que yo le deseo con todo mi corazon. ¿Quién, sinó vos, podría ser más acreedor á mi preferencia, Señor? Este librito podrá elevarse hasta los astros, si os dignais aceptar su dedicatoria, si el público le concede una pequeña parte de la merecida popularidad y de la alta consideracion con que os rodea. Dignaos admitir, Señor, con la seguridad de mi gratitud, la de mi profunda consideracion. Buenos Aires, Julio 23 de 1879. SEÑOR DON AQUILES SIOEN: Presente. Distinguido Señor: Carezco absolutamente de los méritos que V. tiene la bondad de atribuirme. Acepto, no obstante, gustoso, la dedicatoria de su libro, pero sólo como una prueba de la benevolencia que V. me manifiesta. [...]
```
Now we need to convert it into a string to analyze it in R. To do this, we need to open it with a filepath:

In Windows, you can convert it into a string using the path:

```R
text_string <- scan(file = "FILEPATH", fileEncoding = "UTF-8", what = character(), sep = "\n", allowEscapes = T)
```

In this case, it would be:

```R
text_string <- scan(file = "C:\\Users\\Carmen\\Desktop\\doc_modified.txt", fileEncoding = "UTF-8", what = character(), sep = "\n", allowEscapes = T)
```

### Getting tokens

To analyze the text with the algorithm, it needs to be in a format recognizable by it, which means tokens, in this case, will be words.
```R
text_words <- get_tokens(text_string)
head(text_words)
```
Resulting in:

```R
[1] "5559"     "imprenta" "del"      "porvenir" "calle"    "defensa" 
```
Now that we have the tokenized text, we can use the `get_nrc_sentiment` function to obtain the sentiment score for each word in the text.
```R
sentiment_scores <- get_nrc_sentiment(text_words, lang="spanish")
```
The results are automatically saved in a CSV file. To observe the beginning of the result, we can write: `head(sentiment_scores)`, and it will display:

{% include figure.liquid loading="eager" path="assets/img/head_buenosAires.png" title="Head sentiment scores" class="img-fluid rounded z-depth-1" %}

Next, we can obtain a chart by emotion to interpret our results:

```R
barplot(
  colSums(prop.table(sentiment_scores[, 1:8])),
  space = 0.2,
  horiz = FALSE,
  las = 1,
  cex.names = 0.7,
  col = brewer.pal(n = 8, name = "Set3"),
  main = "'Buenos Aires en el año 2080' by Aquiles Sioen, 1879 edition",
  sub = "Analysis by Carmen Carrasco",
  xlab="emotions", ylab = NULL)
```
The result is:

{% include figure.liquid loading="eager" path="assets/img/BuenosAiresChart.png" title="Chart by Emotion" class="img-fluid rounded z-depth-1" %}

## Emotions Cloud

To obtain an emotion cloud, first, we collect all the words with an emotion score greater than 0.

```R
cloud_emotions_data <- c(
  paste(text_words[sentiment_scores$sadness> 0], collapse = " "),
  paste(text_words[sentiment_scores$joy > 0], collapse = " "),
  paste(text_words[sentiment_scores$anger > 0], collapse = " "),
  paste(text_words[sentiment_scores$fear > 0], collapse = " "))

cloud_emotions_data <- iconv(cloud_emotions_data, "latin1", "UTF-8")
```
And we save it in `cloud_corpus <- Corpus(VectorSource(cloud_emotions_data))`. Next, we can create the emotion cloud.

```R
set.seed(750) 
comparison.cloud(cloud_tdm, random.order = FALSE,
                 colors = c("green", "red", "orange", "blue"),
                 title.size = 1, max.words = 50, scale = c(2.5, 1), rot.per = 0.4)
```
The result is as follows:

{% include figure.liquid loading="eager" path="assets/img/Cloud_BuenosAires.png" title="Chart by Emotion" class="img-fluid rounded z-depth-1" %}

