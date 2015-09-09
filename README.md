# Podpora slovenčiny pre ElasticSearch

Repozitár pozostáva zo:
* __stop-words__ (pôvodný zdroj [code.google.com/p/stop-words/](https://code.google.com/p/stop-words/))
* __OpenThesaurus-SK__ - synonymický slovník vo formáte pre ElasticSearch / SOLR (prekonvertované z originálneho [OpenThesaurus-SK](http://sk-spell.sk.cx/thesaurus))
* __hunspell-sk__ pripravený pre `hunspell token filter` (zdroj [hunspell-sk](http://www.sk-spell.sk.cx/hunspell-sk))

Pre lepšie výsledky pri lematizácii odporúčame použiť [LemmaGen](https://github.com/vhyza/elasticsearch-analysis-lemmagen) _(licencia umožňuje použitie len v nekomerčných projektoch)_

Vyskúšať môžete aj [hunspell slovník od Essential Data](https://github.com/essential-data/hunspell-sk/releases/tag/1.1)


## Požiadavky a inštalácia

Otestované pre [ElasticSearch v1.3.4](https://www.elastic.co/downloads/elasticsearch)
Synonymický slovník je vo formáte použitelnom aj pre SOLR (zatiaľ netestované).

Obsah repozitáru stačí nakopírovať do priečinku `config/` vo vašej inštalácií ElasticSearch

```
   |-bin
   |-config
   |---hunspell
   |-----sk_SK
   |---stop-words
   |---synonyms
   |-libexec   
```

## Použitie

Príklad, ako si nastaviť analyzer:

```
{
  "settings": {
    "analysis": {
      "filter": {
        "lemmagen_filter_sk": {
          "type": "lemmagen",
          "lexicon": "sk"
        },
        "sk_SK" : {
          "type" : "hunspell",
          "locale" : "sk_SK",
          "dedup" : true,
          "recursion_level" : 0
        },
        "synonym_filter": {
          "type": "synonym",
          "synonyms_path": "synonyms/sk_SK.txt",
          "ignore_case": true
        },
        "stopwords_SK": {
          "type": "stop",
          "stopwords_path": "stop-words/stop-words-slovak.txt",
          "ignore_case": true
        }
      },
      "analyzer": {
        "slovencina_synonym": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": [
            "stopwords_SK",
            "lemmagen_filter_sk",
            "lowercase",
            "stopwords_SK",
            "synonym_filter",
            "asciifolding"
          ]
        },
        "slovencina": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": [
            "stopwords_SK",
            "lemmagen_filter_sk",
            "lowercase",
            "stopwords_SK",
            "asciifolding"
          ]
        }
      }
    }
  }
}
```
pozn. _tento príklad používa [LemmaGen](https://github.com/vhyza/elasticsearch-analysis-lemmagen)_

## Správa projektu

Tento projekt spravuje [lab.SNG](http://lab.sng.sk). Ak máte akékoľvek otázky, vytvorte _issue_ priamo tu alebo nám napíšte na [lab@sng.sk](mailto:lab@sng.sk).


## Licencia

* __OpenThesaurus-SK__ a __hunspell-sk__ používa dáta vydané pod GPLv2, LGPLv2.1, MPLv1.1

* __stop-words__ sú pod [public domain](http://unlicense.org/)
