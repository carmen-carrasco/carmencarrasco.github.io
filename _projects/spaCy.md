---
layout: page
title: Named entity recognition with spaCy in a corpus of 19th century literature in Spanish 
description: 
img: assets/img/future.png
importance: 2
category: work
related_publications: false
class: text-center
---

One of the objectives of my project ([a library of speculative fiction literature from the 19th century from Peru and Argentina](https://carmen-carrasco.github.io/projects/SF_Peru_Argentina/)) is to analyze the literary works using the tools of Natural Language Processing (NLP) and computational linguistics.

One of the analyses we present here involves geoparsing. This means that we will analyze which places in the future world are mentioned in our 19th-century texts, and then organize them into geographic coordinates and maps.

This process is developed in different stages, which can be summarized in three:

I. Named Entities Recognition: Places

II. Georeferencing of the corpus

III. Development of the cartographies

Here, only the first stage consisting of the recognition of named entities will be presented.
All this stage was carried out using Python through Jupyter lab.

Firstly, we start with raw text after OCRization, which is not clean as many words are split across lines.

```txt
5559 — Imprenta del "Porvenir,” calle Defensa 139.
AL SEÑOR DON ANTONINO CAMBACERES
PRESIDENTE DE LA ADMINISTRACION DEL FERRO-CARRIL DEL OESTE
Señor:
Este librito, en el que, á la manera de Julio
Verne, de Mery y del autor anónimo de la ba¬
talla de Dorking, se hace un bosquejo del Por¬
venir que espera á vuestra República, no podía
ménos que dedicarse á un gran Administrador,
á un Político prudente, honrado y liberal; en
fin, á un amante apasionado del Progreso bajo
todas sus formas.
Hé ahí, en verdad, las cualidades que habrán
de sobresalir en vuestros hombres de Estado, si
desean asegurar para la Patria Argentina la
prosperidad que, sin temor de equavocarme, se la
puede augurar, y que yo le deseo con todo mi
corazon.
¿Quién, sinó vos, podría ser más acreedor á
mi preferencia, Señor? Este librito podrá ele¬
varse hasta los astros, si os dignais aceptar su
dedicatoria, si el público le concede una pequeña
parte de la merecida popularidad y de la alta
consideracion con que os rodea.
Dignaos admitir, Señor, con la seguridad
de mi gratitud, la de mi profunda considera¬
cion.
Buenos Aires, Julio 23 de 1879.
SEÑOR DON AQUILES SIOEN:
Presente.
Distinguido Señor:
Carezco absolutamente de los méritos que V.
tiene la bondad de atribuirme. Acepto, no obs-
tante, gustoso, la dedicatoria de su libro, pero
sólo como una prueba de la benevolencia que V.
me manifiesta.
[...]
```
That's why, to clean the text, we need to remove unnecessary line breaks and join split words, then save the result into a new document. For this, I used the following code:

```python
# Read the file
with open('doc.txt', 'r', encoding='utf-8') as file:
    text = file.readlines()

# Remove unnecessary line breaks and join split words
text_modified = ""
for line in texto:
    line = line.strip()
    if line.endswith("¬"):
        text_modified += line[:-1]
    else:
        text_modified += line + " "

# Write the modified text back to the file
with open('doc_modified.txt', 'w', encoding='utf-8') as file:
    file.write(text_modified)

print("Process completed. The modified text has been saved in 'doc_modified.txt'.")
```
The result is a clean text like the following:

```txt
5559 — Imprenta del "Porvenir,” calle Defensa 139. AL SEÑOR DON ANTONINO CAMBACERES PRESIDENTE DE LA ADMINISTRACION DEL FERRO-CARRIL DEL OESTE Señor: Este librito, en el que, á la manera de Julio Verne, de Mery y del autor anónimo de la batalla de Dorking, se hace un bosquejo del Porvenir que espera á vuestra República, no podía ménos que dedicarse á un gran Administrador, á un Político prudente, honrado y liberal; en fin, á un amante apasionado del Progreso bajo todas sus formas. Hé ahí, en verdad, las cualidades que habrán de sobresalir en vuestros hombres de Estado, si desean asegurar para la Patria Argentina la prosperidad que, sin temor de equavocarme, se la puede augurar, y que yo le deseo con todo mi corazon. ¿Quién, sinó vos, podría ser más acreedor á mi preferencia, Señor? Este librito podrá elevarse hasta los astros, si os dignais aceptar su dedicatoria, si el público le concede una pequeña parte de la merecida popularidad y de la alta consideracion con que os rodea. Dignaos admitir, Señor, con la seguridad de mi gratitud, la de mi profunda consideracion. Buenos Aires, Julio 23 de 1879. SEÑOR DON AQUILES SIOEN: Presente. Distinguido Señor: Carezco absolutamente de los méritos que V. tiene la bondad de atribuirme. Acepto, no obstante, gustoso, la dedicatoria de su libro, pero sólo como una prueba de la benevolencia que V. me manifiesta. [...]]
```
The next step is to install the spaCy package.

```python
# Now we can use Spacy to get the places' names

import spacy

# Load the Spanish language model
nlp = spacy.load("es_core_news_sm")
```
Now we can extract the names of places from our document using spaCy:

```python

# Read the clean text from the file
with open('doc_modified.txt', 'r', encoding='utf-8') as file:
    text = file.read()

# Process the text with SpaCy
doc = nlp(text)

# Extract all named entities of type "LOC" (location)
locations = [ent.text for ent in doc.ents if ent.label_ == 'LOC']

# Print the recognized location names
print("Recognized location names:")
for location in locations:
    print(location)
```
The result is as follows:
```txt
Recognized location names:
Porvenir
á
Porvenir
Administrador
á
Progreso
Dignaos
Buenos Aires
Julio
Acepto
Buenos Aires
BUENOS AIRES
¡Ay
BUENOS AIRES
á
BUENOS AIRES
mañana
Provincia de Coluguape
Patagonia Central
Buenos Aires
la Rioja
Provincia
San Cristobal
Coluguape
Carancho
Buenos Aires
Americana
Argentina
Estrecho de Magallanes á Rio-Janeiro
Buenos Aires
Asuncion
[...]
```
As you can observe, spaCy recognizes some words as places due to syntax, even though they are not truly places. Therefore, we will have to move on to the next stage, which involves machine learning to train the computer to recognize places, minimizing errors as much as possible.


