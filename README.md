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
- Further concordances for dataset are available in [dataset_concordances.ipynb](https://github.com/DigilabNLCR/AuthorGuesser/tree/main/training/dataset_concordances.ipynb)

## How to use this script

### Requirements
See [requirements.txt](https://github.com/DigilabNLCR/AuthorGuesser/tree/main/requirements.txt)
Essential libraries: os, requests, joblib, re, datetime, time, json, argparse, chardet, scikit-learn
### Gerenal info:
- You can either use default settings that works with these directories:
    - texts_to_guess
    - models
    - guessed_files
- Or you can run the script with arguments modifying these parameters:
    - -m = models path (default "models")
    - -i = input path (default "text to guess")
    - -o = output path (default "guessed_files")
    - -d = delete input? (default True) set y/n --> if True, input files are deleted after guessing process

1) Place texts you wish to guess into the directory "texts_to_guess".
    - Note that texts must be below 18 000 characters (10 standad pages). (Due to using UDPipe https://lindat.mff.cuni.cz/services/udpipe/ - we do not want to overload it; also, the models are trained on far shorter segments.)
    - Encoding must be UTF-8.
    - Texts can include headers, page numbers, footnotes etc. - these are clened by the script.
    - Texts can have hyphenated words at the end of the lines, the lines are connected by the script.
    - The higher the number of words, the higher probability of a right guess
2) Run authorguesser.py
    - The script has been built in Python 3.9.
    - requirements for python packages: os, requests, joblib, re, datetime, time, json, argparse, chardet
    - The script prepares the file (cleaning, connecting lines...) and delexicalize it (using UDPipe). The delexicalization in this case means: part-of-speech tags for autosemantic words (nouns (NOUN), proper nouns (PROPN), adjectives (ADJ), verbs (VERB), adverbs (ADV), and numbers (NUM)), other words are lemmatized.
    - Model is selected according to the segment length (in tokens), and applied.
3) Check for the results
    - You can see the results in the terminal, or:
    - Go to directory "guessed_files", where you can find a xml file that contains:
        - basic info: guessed author, orginal filename, model used, etc.
        - delexicalized text
        - original string (cleaned and connected)
    - Result are also save in "guessed_files" direcotry, JSON file 'results.json'
        - only the basic info is presented in this file.

### Training new models
- In the directory "training", you can find a script that facilitates trainning of the models
- In addition to the scrip, you also need a dataset for training
- to run the script, you need to provide following aguments:
    - required arguments:
        - -m = models_path; path to directory where models are to be stored
        - -d = data_path; path to directory where all the delexicalized data are stored.
        - -clf = model_name; model name as in the sklearn package, only some are available in this version.
            - for this, we suggest to use LinearSVC
        - -n = ngram_range; set ngram range, use both values even if they are the same; e.g., "1,2" or "2,2"
            - for this, we suggest to use 1,3
        - -c = model_configuration; write in model configuration just as you would in model settings, but do not use spaces; e.g., "C=1.0,gamma=0.001,kernel='rfb',seed=42"
            - for this, we use empty command, as we use the models in their default settings.
    - optional arguments:
        - -a = author_ids; set list of author ids you wish to train, including the "a", like ["a-01", "a-02"]
            - if not set, all authors are included
        - -b = book_ids; set list of book ids, including the "b", like ["b-01", "b-02"]
            - if not set, all books are included
        - -p = passage_type; set type of passages to train on (all, train, devel, test), train is default.
- example of training command which has been used to train models presented here (except for paths): py -3.9 train_model.py -m "C:\Users\NAME\authorguesser\models" -d "C:\Users\NAME\authorguesser\full_delexicalized_dataset_r-08" -clf LinearSVC -n "1,3" -c ""

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

# Updates
- 13th August 2024: updates on model selection process.
