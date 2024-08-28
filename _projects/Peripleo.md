---
layout: page
title: Cartography of 19th Century Speculative Fiction from Peru and Argentina 
description: 
img: assets/img/Cloud_BuenosAires.png
importance: 3
category: work
related_publications: false
class: text-center
---
The objective of this post is to show the results of the name entity recognigtion (NER) in the corpus of 19th century speculative fiction from Peru and Argentina 

## Peripleo


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
text_