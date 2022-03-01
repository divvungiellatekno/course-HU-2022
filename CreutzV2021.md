# LT for minority languages & the GiellaLT infrastructure

Main sections:

* introduction
* minority languages and requirements for LT development
* GiellaLT infra
* lexc & twolc as linguistic programming languages
* development
* tools
* demo
* summary
* links

# Introduction

## About me

* Sjur Nørstebø Moshagen
* Linguistics, nordic languages & computer science
* Lingsoft
* Sámi Parliament
* UiT the Arctic University of Norway
* heading the Divvun group at UiT

## What is language technology

### From cuneiform to speech recognition

The first language technology:

![Cuneiroms](images/cuneiform.jpg)

Some later instances of long-lasting language technology:

![Runes](images/jelling runestone.jpg)

![Gutenberg](images/printpress.jpg)

Today we talk about information technology — Internet a.o.

![Internet](images/internet.jpg)

and the term language technology is restricted to actual processing of language data - be it speech or text or video (as when processing signed languages). The ultimate dream of language technology is speech-to-speech machine translation:

![Traveling](images/spoken-MT-images.jpg)

In all cases language (and information) technology has been pretty transformative.

Another typical characteristic of language technology is that it is divisive:

* those with access
* those without

![digital divide](images/293-2067-1-PB.jpg)

empovering those with access, leaving those without behind. As such it can easily be a driver in language death: to take part in the society at large, you can't use your own language because the society expects use of certain technologies:

* a certain alphabet or writing system — ie literacy
* access to a printing press
* access to computers
* access to your letters on that computer

For speakers of most of the languages of the world (there are about 7000) one or several of the points above are **not true**, and will only add to all the other factors driving language death.

One of the main objectives of the **GiellaLT** infrastructure is to help counter this, by developing language technology for such languages, to make them easy to use on digital devices.

Our starting point and main focus is the Sámi languages, but everything that we make is language independent (except for the linguistic data, obviously), and we actively cooperate with other groups to extend the reach of our technology.

# Minority languages and requirements for LT development

<!-- 2019 was the UN [International Year of Indigenous languages](https://en.iyil2019.org/). Our work directly supports the goals of IYIL 2019. -->

## Characteristics of minority language development

Typically, minority languages share a number of characteristics:

* few or non-existing digital resources
* restricted availability of dictionaries and grammar, or no at all
* often complex morphology or morphophonology or both

## Ownership

It is important that language communities have control over language resources relating to their language, in the sense that no private entity can block access to those resources. Otherwise the society will risk vendor lock-in, and expensive redevelopment of existing tools and resources. — Despite being aware of this, we have experienced it **twice**!

The best solution is to ensure that everythig is **open source**. All resources and tools in the GiellaLT infra are open source, unless forced to by software we integrate with (MS Office is one such case). Also, some language communities do not want their language to be openly accessible, due to a history of being colonialised, oppressed and their language becoming stigmatised. In such cases we of course accept their decission.

![Open Source](images/opensourceimages.jpg)

## Reuse and multiuse

Because of the costs of language technology projects, it is important to build your infra and resources with reuse in mind, and also plan them so that everything is prepared for multiple usage scenarios.

E.g. in the GiellaLT infrastructure, we have standardised conventions that makes it easy to build both normative and descriptive tools from the same codebase.

* **normative:** tools that adhere strictly to an agreed-upon norm for writing, and try to correct text so that deviations are brought in line with the norm, like spelling checkers and grammar checkers.
* **descriptive:** tools that try to process all texts in a language, irrespective of the normative properties of the text

## Mainly rule-based

Language technology comes in several flavours:

* rule-based
* statistical
* stocastic
* neural nets

Typical for all but the rule-based one is that they require large amounts of raw data to be trained on.

Rule based technologies on the other hand, in principle only requires a mother tongue speaker and a linguist (which in the best of cases is one and the same person).

![Rule-based](images/fst_fst.png)

## Main sources for building the grammars and language resources

* digital dictionaries
* grammars
* korpus
* native speakers

# The GiellaLT infrastructure

* language independent infrastructure
* scalability in two dimensions: languages x tools/products
* standardised dir & file structure
* encourages and fascilitates international cooperation
* ~130 languages in our infra (at various stages), 30+ in active development
    * almost all of them minority languages
    * majority language grammars and LT resources mainly to support the minority languages

![International cooperation](images/gtlangs_circumpolar_names.png)

Some of the language repositories, with build and bug status, license and maturity:

![The registry](images/registry.png)

## Standardised dir structure

Every language has the following (slightly simplified) directory structure:

```
.
├── devtools
├── docs
├── src
│   ├── cg3
│   ├── filters
│   ├── fst
│   ├── hyphenation
│   ├── orthography
│   ├── phonetics
│   ├── scripts
│   ├── tagsets
│   └── transcriptions
├── test
│   ├── data
│   ├── src
│   └── tools
└── tools
    ├── analysers
    ├── grammarcheckers
    ├── hyphenators
    ├── mt
    ├── shellscripts
    ├── spellcheckers
    └── tokenisers
```

We are working on improving the direcotry structure and file organisation.

## Scalability

* for languages:
    * template for all resources needed
* for tools:
    * add support for a new tool to the template, and propagate it to all existing languages
* core design principle:
    * separate language independent processing from language-specific processing

The templating system and the split between language independent and specific code ensures that we can add as many languages as we want, and easily add support for new tools and technologies.

# «Linguistic programming»

Formalisms / technologies used:

* **morphology / morphophonology:** Hfst / Foma / Xerox
    * lexc
    * twolc
    * xfst rewrite rules
    * Xeroxs-style pmatch scripts
* **syntax:** Constraint grammar (in the form of *VISLCG3* )

All of these are open source except for the Xerox tools (which are free, though). Foma does not support TwolC (see further down).

## LexC

* an excellent formalism for concatenative morphology
* typically, you specify stems and affixes in different lexicons
* ... to allow for abstractions over stem classes and inflections
* it is in essence a programming language for linguists
* ... where you spell out the morphology of a language such that a compiler can turn it into an executable program (with the help of a run-time engine)

## TwolC

* Formalism developed by Kimmo Koskenniemi in the early 80's to describe phonological processes
* resembles quite closely generative rewrite rules of the form `A -> B / C _ D`
* rules are unordered and applied in parallel

## Xfst rewrite rules

* another formalism to describe phonology
* main difference to TwolC: rules are ordered and applied in sequence

Both TwolC and Xfst rewrite rules are supported by the GiellaLT infrastructure, compilation support is dependent on the compiler tool used: Foma does not support Twolc, everything else is supported by all tools.

## Xeroxs-style pmatch scripts

Hfst only, this formalism is an extension of the xfst rewrite rules, and are a reimplementation of work by Xerox around 10 years ago. It allowes for more complex text processing, and with a few modifications we have turned the formalism into a tokeniser-and-morphological-analyser that will also output ambiguous tokens. Such ambiguity can then be resolved using Constraint Grammar, followed by a simple reformatter that rewrites tokens that are split in two.

Using this setup it is possible to get the tokenisation almost perfect. In practice we still have some work to do, but we are already well above the alternative methods.

The pmatch scripts are key to a recent addition to our infrastructure: rule-based grammar checking. We are also now developing text-to-speech systems using the pmatch scripts + VISLCG3 processing to turn raw text into disambiguated IPA text streams that can be fed to the synthesis engine.

## Constraint grammar

* formalism developed at Helsinki university by Fred Karlsson, later extended by Tapanainen (CG2), and even furthery by the VISL project (CG3)
* main idea is to remove or select specific possible readings of ambiguous words given context constraints:
    * in the context of a subject personal pronoun, select a verb reading that agrees with the pronoun in person and number
* used a lot in text parsers in combination with morphological analysers, giving very good results
* also used in language technology tools and products such as machine translation and grammar checking since the late 1990's

## Testing

Systematic testing is essential, and the infrastructure supports several types of tests:

* classes of words/inflections/alternations
* lemmas
* in-source test data

Example test data (South Sámi):

```
Tests:

  Verb - båetedh: # verb I, stem -ie, root vowel -åe-
    båetedh+V+IV+Inf: båetedh
    båetedh+V+IV+Ind+Prs+Sg1: båatam
    båetedh+V+IV+Ind+Prs+Sg2: båatah
    båetedh+V+IV+Ind+Prs+Sg3: båata
    båetedh+V+IV+Ind+Prs+Du1: båetien
    båetedh+V+IV+Ind+Prs+Du2: [båeteden, båetiejidien]
    båetedh+V+IV+Ind+Prs+Du3: båetiejægan
    båetedh+V+IV+Ind+Prs+Pl1: [båetebe, båetiejibie]
    båetedh+V+IV+Ind+Prs+Pl2: [båetede, båetiejidie]
    båetedh+V+IV+Ind+Prs+Pl3: båetieh
```

This can be used both as a development gold standard, and as regression testing later.

# Tools

## keyboards (desktop & mobile)

A very simple syntax (mobile keyboard shown):

```
modes:
  mobile-default: |
    á š e r t y u i o p ŋ
    a s d f g h j k l đ ŧ
       ž z č c v b n m
  mobile-shift: |
    Á Š E R T Y U I O P Ŋ
    A S D F G H J K L Đ Ŧ
       Ž Z Č C V B N M
```

This + a few more technical details is used to produce ready-to-use installers and keyboard apps. One can also add a speller file (fst-based spell checker), and get spelling correction as part of your mobile keyboard. The end result looks like this:

![North Sámi mobile keyboard](images/sme-keyboard-speller.jpeg)

And we of course support dark mode:

![Dark North Sámi mobile keyboard](images/sme-keyboard-speller-dark.jpeg)

The speller is exactly the same fst-based speller as described below, with slight adaptions of the error model to fit the keyboard layout and the errors typically made.

### Locale registration

As part of the desktop keyboard installers, the locale of the keyboard is added to the system, so that languages unknown to Windows and macOS is subsequently known and can be used for spell checking:

![Plains cree keyboard menu entry](images/crk-Latn.png)

![Plains Cree in MS Word](images/PlainsCreeWord.png)

## spellers

A speller is made up of two parts:

1. an acceptor - is this a correct word or not?
1. an error model - if this is not a word, how is it most likely to be corrected?

In our infrastructure, both are finite state transducers. The acceptor is built from our general analyser, but restricted to only normatively correct forms.

The error model contains a standard permutation fst for the relevant alphabet, with language specific additions based on likely errors made by writers.

### Short turnaround during development

1. add a word, correct some part of the morphology
1. compile
1. test in e.g. LibreOffice or on the command line

Compilation time varies a lot depending on the language and the size and complexity of the lexicon, the morphology and the morphophonology.

### Host app integration

* MS Word (Windows, macOS coming)
* LibreOffice (all OS's)
* System wide spellers (Windows, macOS, Linux)
* mobile keyboard apps
* web server

![Speller online](images/speller_online.png)

## hyphenation

* uses rewrite rules to identify syllable structure = hyphenation points
* uses analyser (lexicon) to find word boundaries and exceptional hyphenation

## grammar checkers

* morphological analyser for analysis and tokenisation
* includes disambiguation of multiword expressions
* a tagger for whitespace errors
* runs the spelling checker on unknown words
* constraint grammars for both disambiguation and error detection, as well as for selecting or filtering speller suggestions based on context
* uses valency info and semantic tags to avoid reliance on (faulty) morphology and syntax
* new research comming out of this:
  * improvements to sentence border detection (near-perfect results possible)
  * improvements to tokenisation and whitespace handling - we can detect compounds erroneously written apart (not very well handled or not at all by most other grammar checkers)

### Grammar checker flow chart:

![Grammar checker](images/GramCheckFlow2.0.png)

Works in:

* LibreOffice
* MS Word (web version for now, Win and Mac coming later)
* GoogleDocs
* planned support: macOS (system wide), possibly Windows

### Screen shot from LibreOffice:

![Grammar checker](images/LO-gram.png)

### Screen shot from MS Word (web version):

![Grammar checker](images/Word-gram.png)

<!-- ### Screen shot from online grammar checker:

![Grammar checker](images/gram-gram.png) -->

### Demo

## Text-to-speech systems

* recordings and text available
* technology unfortunately from a commercial company = closed source code
  * we have now been hunted by this - they are closing down the macOS version
  * we had fortunately already planned a new project for Julev Sámi that is completely built using open source, so we should be good in a couple of years
* quality very good
* the original plan was to use our own text processing for conversion to IPA or similar,
  we are doing that in our new project
* the idea is to use roughly the same text processing as we use for the grammar checker to produce a phonetic transcription, and feed that to the synthesis engine

## dictionaries

* content from several sources
* morphological analysis to enable looking up directly in text
    * web browsers
    * macOS and Windows apps

![NDS](images/neahttadigisaanit_reader_bubble_cutted.png)

## language learning

* analysing reader input
* adapting suggested forms according to user preferences

## Korp

* database and interface for searching an analysed corpus
* morphological analysis, disambiguation, syntactic parsing using our tools
* corpus data available in many languages

![Korp](images/korp_boahtit.png)

# Summary

* one source for everything
* reuse and multiple usages
* summarised in the following illustration:

![House overview](images/hus_eng_ny.pdf)

# Links

Everything easily accessible in GitHub, everyone can edit and contribute.

- Divvun tools & download: [divvun.no](https://divvun.no/) & [divvun.org](https://divvun.org/)
- Language resources & source code: [github.com/giellalt](https://github.com/giellalt/)
- Tool source code: [github.com/divvun](https://github.com/divvun/)
- Korp:  [gtweb.uit.no/korp/](http://gtweb.uit.no/korp/)
- Machine translation: [jorgal.uit.no](http://jorgal.uit.no/)
- Overview + build status: [github.com/divvun/registry](https://github.com/divvun/registry)