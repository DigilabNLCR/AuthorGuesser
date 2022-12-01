# DigiLab-AuthorGuesser
This repository includes scripts that run author guessing with pretrained models.
- So far, there is no possible selection of models:
    - It uses LinearSVC model, with features of uni to tri-grams.
    - The models were trained on delexicalized datasets, using UDPipe. The delexicalization in this case means: part-of-speech tags for autosemantic words (nouns (NOUN), proper nouns (PROPN), adjectives (ADJ), verbs (VERB), adverbs (ADV), and numbers (NUM)), other words are lemmatized.
- Trained on these authors (dictionary of authors to author IDs):
author_to_id = {'A. Stašek': 'a-01',
                'J. Neruda': 'a-02',
                'J. Arbes': 'a-03',
                'K. Klostermann': 'a-04',
                'F. X. Šalda': 'a-05',
                'T. G. Masaryk': 'a-06',
                'A. Jirásek': 'a-07',
                'Č. Slepánek': 'a-08',
                'E. Krásnohorská': 'a-09',
                'F. Herites': 'a-10',
                'I. Olbracht': 'a-11',
                'J. Vrchlický': 'a-12',
                'J. S. Machar': 'a-13',
                'J. Zeyer': 'a-14',
                'K. Čapek': 'a-15',
                'K. Nový': 'a-16',
                'K. Sabina': 'a-17',
                'K. V. Rais': 'a-18',
                'K. Světlá': 'a-19',
                'S. K. Neumann': 'a-20',
                'V. Hálek': 'a-21',
                'V. Vančura': 'a-22',
                'Z. Winter': 'a-23'}

## How to use this script
1) Place texts you wish to guess into the directory "texts_to_guess".
    - Note that texts must be below 18 000 characters (10 standad pages). (Due to using UDPipe https://lindat.mff.cuni.cz/services/udpipe/ - we do not want to overload it; also, the models are trained on far shorter segments.)
    - Encoding must be UTF-8.
    - Texts can include headers, page numbers, footnotes etc. - these are clened by the script.
    - Texts can have hyphenated words at the end of the lines, the lines are connected by the script.
2) Run authorguesser.py
    - The script has been built in Python 3.9.
    - requirements for python packages: os, requests, joblib, re, datetime, time
    - The script prepares the file (cleaning, connecting lines...) and delexicalize it (using UDPipe). The delexicalization in this case means: part-of-speech tags for autosemantic words (nouns (NOUN), proper nouns (PROPN), adjectives (ADJ), verbs (VERB), adverbs (ADV), and numbers (NUM)), other words are lemmatized.
    - Model is selected according to the segment length (in tokens), and applied.
3) Check for the results
    - You can see the results in the terminal, or:
    - Go to directory "guessed_files", where you can find a xml file that contains:
        - basic info: guessed author, orginal filename, model used, etc.
        - delexicalized text
        - original string (cleaned and connected)

## Training new models
- In the directory "training", you can find a script that facilitates trainning of the models, but for that you need also some delexicalized dataset... the script for the creation of a delexicalized dataset will be published later.
- For now, this is only for you to see the process of training.

## **Roles**

* **František Válek** - *principal programmer*
* **Jan Hajič** - *principal programmer, experiment design*
* **Jiří Szromek** -  *supporting programmer*
* **Martin Holub** -  *statistical and mathematical conception*
* **Zdenko Vozár** -  *project lead, solution conception and supervision*


## **More**
More information about project implementation: https://github.com/DigilabNLCR/AuthorshipAttributionLine

# Dedication
National Library of the Czech Republic

_Realized with the support of institutional research of National Library of the Czech Republic funded by Ministry of Culture of the Czech Republic as part of the framework of Longterm conception developement of scientific organization._

Národní knihovna České Republiky

_Realizováno v rámci institucionálního výzkumu Národní knihovny České republiky financovaného Ministerstvem kultury ČR v rámci Dlouhodobého koncepčního rozvoje výzkumné organizace._
