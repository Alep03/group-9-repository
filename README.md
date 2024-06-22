# group-9-repository
KT24602 SOFTWARE ENGINEERING [2-2023/2024]
Project Overview 

A straightforward yet effective command-line tool and library, Automatic Text Summarizer is made to extract summaries from plain text or HTML pages. By providing both extractive and abstractive summarization techniques, it makes summarizing extensive text easier. Additionally, the library offers a simple assessment system for determining how well text summaries are written. 

Features: 

Extractive Summarization: Selects and extracts the most important sentences from the original text. 

Abstractive Summarization: Generates new sentences that convey the main ideas of the original text. 

Command-Line Utility: Easy-to-use command-line interface for quick summarization tasks. 

Evaluation Framework: Built-in tools to evaluate the effectiveness of generated summaries. 

Documentation: Comprehensive documentation describing the implemented summarization methods. 

Alternative Implementations: Maintains a list of alternative summarizer implementations in various programming languages, providing flexibility and adaptability. 

 

This tool is ideal for anyone looking to quickly grasp the main points of large texts, whether for academic, professional, or personal use. 

 

 

 

 

 

Installation Instructions 

Prerequisites 

Python 3.6 or higher 

pip (Python package installer) 

 

1. Clone Repository 

 

git clone https://github.com/Alep03/group-9-repository.git 

cd group-9-repository 

 

2. Create a Virtual Environment 

Managing dependencies in a virtual environment is a smart practice. Use PowerShell or Command Prompt to execute: 

python -m venv venv 

Venv\Scripts\activate 

3. Install Required Package 

$ [sudo] pip install -r requirements.txt 

4. Download NLP Models 

Spacy : 

python -m spacy download en_core_web_sm 

 

NLTK  :  

import nltk 

nltk.download('punkt') 

 

Transformers  : 

pip install transformers 

 

 

Usage 

Command-Line Interface 

The summarizer can be run from the command line with various options: 

--input: Path to the input text file. 

--method: Summarization method ('extractive' or 'abstractive'). 

--length: Desired length of the summary (number of sentences or characters, depending on implementation). 

 

Sumy contains a command line utility for quick summarization of documents : 

 

$ group-9-repository lex-rank --length=10 --url=https://en.wikipedia.org/wiki/Automatic_summarization # what's summarization? 

 

$ group-9-repository lex-rank --language=uk --length=30 --url=https://uk.wikipedia.org/wiki/Україна 

 

$ group-9-repository luhn --language=czech --url=https://www.zdrojak.cz/clanky/automaticke-zabezpeceni/ 

 

$ group-9-repository edmundson --language=czech --length=3% --url=https://cs.wikipedia.org/wiki/Bitva_u_Lipan 

 

$ group-9-repository --help  

 

Various evaluation methods for some summarization methods can be executed by commands below: 

$group-9-repository_eval lex-rank reference_summary.txt --url=https://en.wikipedia.org/wiki/Automatic_summarization 

 

$ group-9-repository_eval lsa reference_summary.txt --language=czech --url=https://www.zdrojak.cz/clanky/automaticke-zabezpeceni/ 

 

$ group-9-repository_eval edmundson reference_summary.txt --language=czech --url=https://cs.wikipedia.org/wiki/Bitva_u_Lipan 

$ group-9-repository_eval --help  

If you don't want to bother with the installation, you can try it as a container: 

$ docker run --rm misobelica/group-9-repository lex-rank --length=10 --url=https://en.wikipedia.org/wiki/Automatic_summarization 

Python API 

# -*- coding: utf-8 -*-  

 

from __future__ import absolute_import 

from __future__ import division, print_function, unicode_literals  

 

from group-9-repository.parsers.html import HtmlParser 

from group-9-repository.parsers.plaintext import PlaintextParser 

from group-9-repository.nlp.tokenizers import Tokenizer 

from group-9-repository.summarizers.lsa import LsaSummarizer as Summarizer 

from group-9-repository.nlp.stemmers import Stemmer 

from group-9-repository.utils import get_stop_words 

 

LANGUAGE = "english" 

SENTENCES_COUNT = 10 

 

if __name__ == "__main__": 

url = "https://en.wikipedia.org/wiki/Automatic_summarization" 

parser = HtmlParser.from_url(url, Tokenizer(LANGUAGE)) 

# or for plain text files 

 # parser = PlaintextParser.from_file("document.txt", Tokenizer(LANGUAGE)) 

# parser = PlaintextParser.from_string("Check this out.", Tokenizer(LANGUAGE)) 

stemmer = Stemmer(LANGUAGE) 

summarizer = Summarizer(stemmer) 
summarizer.stop_words = get_stop_words(LANGUAGE) 

for sentence in summarizer(parser.document, SENTENCES_COUNT): 
print(sentence) 
