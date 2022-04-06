# ukr-spacy

SpaCy official Ukrainain model proposal.

Ukrainain model is trained on <a href="https://huggingface.co/datasets/ukr-models/Ukr-Synth">large silver standard Ukrainain corpus</a> annotated with morphology tags, syntax trees and PER, LOC, ORG NER-tags (available under MIT license).
Pretrained transformer encoder is <a href="https://huggingface.co/ukr-models/xlm-roberta-base-uk">XLM-Roberta base model (cut to using only Ukrainian/English tokens)</a>.
Pretrained word embeddings for Ukrainain language are <a href="https://lang.org.ua/static/downloads/models/ubercorpus.cased.tokenized.word2vec.300d.bz2">Word2vec embeddings pretrained on Ubercorpus</a> (prepared by <a href="https://lang.org.ua/">lang-uk community</a>).
This repo is available under MIT license.


## Install

Transformer-based model can be downloaded from Huggingface repository (SpaCy 3.* version should be installed)
```
pip install https://huggingface.co/ukr-models/uk_core_news_trf/resolve/main/uk_core_news_trf-any-py3-none-any.whl
```


## Usage 

See examples of model usage.

```python
>>> import spacy
>>> nlp = spacy.load('uk_core_news_trf')

>>> text = "24 лютого до острова Зміїний підійшли дві нерозпізнані цілі. Окупанти вимагали від військовослужбовців Ізмаїльського прикордонного загону здатися по радіоканалу з безпеки судноплавства. Прикордонник Роман Грибов на це відповів: «Русский военный корабль, иди на хуй!»"
>>> doc = nlp(text)

### NER ###

>>> for ent in doc.ents:
...     print(ent.text, ent.start_char, ent.end_char, ent.label_)
		
острова Зміїний 13 28 LOC
Ізмаїльського прикордонного загону 103 137 ORG
Роман Грибов 199 211 PER

### MORPH ###

>>> for token in doc:
...     print(token.text.ljust(20), token.pos_.ljust(10), token.morph)

24                   ADJ        Case=Gen|Gender=Neut|NumType=Ord|Number=Sing|Uninflect=Yes
лютого               NOUN       Animacy=Inan|Case=Gen|Gender=Masc|Number=Sing
до                   ADP        Case=Gen
острова              NOUN       Animacy=Inan|Case=Gen|Gender=Masc|Number=Sing
Зміїний              PROPN      Animacy=Inan|Case=Nom|Gender=Masc|Number=Sing
підійшли             VERB       Aspect=Perf|Mood=Ind|Number=Plur|Tense=Past|VerbForm=Fin
дві                  NUM        Case=Nom|Gender=Fem|NumType=Card
нерозпізнані         ADJ        Aspect=Perf|Case=Nom|Number=Plur|VerbForm=Part|Voice=Pass
цілі                 NOUN       Animacy=Inan|Case=Nom|Gender=Fem|Number=Plur
.                    PUNCT      
Окупанти             NOUN       Animacy=Anim|Case=Nom|Gender=Masc|Number=Plur
вимагали             VERB       Aspect=Imp|Mood=Ind|Number=Plur|Tense=Past|VerbForm=Fin
від                  ADP        Case=Gen
військовослужбовців  NOUN       Animacy=Anim|Case=Gen|Gender=Masc|Number=Plur
Ізмаїльського        ADJ        Case=Gen|Gender=Masc|Number=Sing
прикордонного        ADJ        Case=Gen|Gender=Masc|Number=Sing
загону               NOUN       Animacy=Inan|Case=Gen|Gender=Masc|Number=Sing
здатися              VERB       Aspect=Perf|VerbForm=Inf
по                   ADP        Case=Loc
радіоканалу          NOUN       Animacy=Inan|Case=Loc|Gender=Masc|Number=Sing
з                    ADP        Case=Gen
безпеки              NOUN       Animacy=Inan|Case=Gen|Gender=Fem|Number=Sing
судноплавства        NOUN       Animacy=Inan|Case=Gen|Gender=Neut|Number=Sing
.                    PUNCT      
Прикордонник         NOUN       Animacy=Anim|Case=Nom|Gender=Masc|Number=Sing
Роман                PROPN      Animacy=Anim|Case=Nom|Gender=Masc|NameType=Giv|Number=Sing
Грибов               PROPN      Animacy=Anim|Case=Nom|Gender=Masc|NameType=Sur|Number=Sing
на                   ADP        Case=Acc
це                   PRON       Animacy=Inan|Case=Acc|Gender=Neut|Number=Sing|PronType=Dem
відповів             VERB       Aspect=Perf|Gender=Masc|Mood=Ind|Number=Sing|Tense=Past|VerbForm=Fin
:                    PUNCT      
«                    SYM        
Русский              ADJ        Case=Nom|Gender=Masc|Number=Sing
военный              ADJ        Case=Nom|Gender=Masc|Number=Sing
корабль              NOUN       Animacy=Inan|Case=Nom|Gender=Masc|Number=Sing
,                    PUNCT      
иди                  VERB       Aspect=Imp|Mood=Imp|Number=Sing|Person=2|VerbForm=Fin
на                   ADP        Case=Acc
хуй                  NOUN       Animacy=Inan|Case=Acc|Gender=Masc|Number=Sing
!                    SYM        
»                    SYM

### SYNTAX ###

>>> for token in doc:
...     print(token.text, token.dep_, token.head.text)

24                   obl        підійшли
лютого               nmod       24
до                   case       острова
острова              obl        підійшли
Зміїний              flat:title острова
підійшли             ROOT       підійшли
дві                  nummod     цілі
нерозпізнані         amod       цілі
цілі                 nsubj      підійшли
.                    punct      підійшли
Окупанти             nsubj      вимагали
вимагали             ROOT       вимагали
від                  case       військовослужбовців
військовослужбовців  obl        вимагали
Ізмаїльського        amod       загону
прикордонного        amod       загону
загону               nmod       військовослужбовців
здатися              xcomp      вимагали
по                   case       радіоканалу
радіоканалу          obl        здатися
з                    case       безпеки
безпеки              nmod       радіоканалу
судноплавства        nmod       безпеки
.                    ROOT       .
Прикордонник         nsubj      відповів
Роман                flat:title Прикордонник
Грибов               flat:name  Роман
на                   case       це
це                   obl        відповів
відповів             ROOT       відповів
:                    punct      корабль
«                    punct      корабль
Русский              amod       корабль
военный              amod       корабль
корабль              parataxis  відповів
,                    punct      иди
иди                  parataxis  відповів
на                   case       хуй
хуй                  obl        иди
!                    ROOT       !
»                    ROOT       »
```


## Training

Model is trained using <a href="https://spacy.io/usage/projects">SpaCy projects</a>.

Download, uncompress training data.
```bash
spacy project assets
spacy project run extract
```

Convert training data to SpaCy binary format.
```bash
spacy project run corpus
```

Convert and prune embeddings table (not necessary for transformer-based model).
```bash
spacy project run vectors
```

Train model.
```bash
spacy project run train
```

Build package.
```bash
spacy project run package
```
