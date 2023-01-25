# llm-metadata-extraction

Experiment on metadata extraction using large language models.

The experiment itself is in the Jupyter notebook
[ExtractMetadataUsingFinetunedGPT3.ipynb](ExtractMetadataUsingFinetunedGPT3.ipynb)
which you can view directly on GitHub without installing anything. If you want to run it yourself, see the bottom of this page.

## Data set

The aim of the experiment is to extract metadata from recent (2021-2022) doctoral theses obtain from four Finnish universities (Åbo Akademi, University of Turku, University of Vaasa and Lappeenranta University of Technology), using only the raw text from the first few pages of the PDF.

The set of 192 documents (88% English, 7% Finnish, 5% Swedish language) will be split into two subsets (train: 149, test: 43). We will extract the text from around 5 pages of text, aiming for 500 to 700 words. The corresponding metadata, which has been exported from DSpace repositories of the universities, is represented in a simple textual "key: value" format, which should be easy enough for a language model to handle. The train set is used to create a data set which will then be used to fine-tune a GPT-3 Curie model. Subsequently the model can be used to generate similar metadata for unseen documents from the test set.

## Example output

Here is `diff` output showing the difference between the original (manually created) metadata (-, red) and the output of the model (+, green) with some comments after each example. These are all documents from the test set that the model has never seen before.

### University of Vaasa

```diff
 https://osuva.uwasa.fi/handle/10024/11207
 ---
 title: How to apply technology, knowledge and operations decision models for strategically sustainable resource allocation?
-title/alternative: Kuinka soveltaa teknologiaan, tietoon ja tuotantoon liittyvän päätöksenteon malleja strategisesti kestävään resurssien allokointiin?
+title/alternative: Kuinka soveltaa teknologiaan, tietoon ja tuotantoon liittyvää päätöksen teon malleja strategisesti kestävään resurssien allokointiin?
 contributor/faculty: fi=Tekniikan ja innovaatiojohtamisen yksikkö|en=School of Technology and Innovations|
 contributor/author: Tilabi, Sara
 contributor/organization: fi=Vaasan yliopisto|en=University of Vaasa|
 publisher: Vaasan yliopisto
 date/issued: 2020
 relation/issn: 2323-9123
 relation/issn: 0355-2667
 relation/isbn: 978-952-476-915-0
 relation/ispartofseries: Acta Wasaensia
 relation/numberinseries: 445
 ---
```
 
Comment: The generated metadata is perfect except for a small mistake in the title.
 
```diff
 https://osuva.uwasa.fi/handle/10024/10432
 ---
 title: Persoonallinen ajattelu päättelyssä ja päätöksenteossa
-title/alternative: Personal Thinking in reasoning and decision making
+title/alternative: Personal thinking in judgment and decision-making
 contributor/faculty: fi=Tekniikan ja innovaatiojohtamisen yksikkö|en=School of Technology and Innovations|
 contributor/author: Alho, Tapio
 contributor/organization: fi=Vaasan yliopisto|en=University of Vaasa|
 publisher: Vaasan yliopisto
 date/issued: 2020
 relation/issn: 2323-9123
 relation/issn: 0355-2667
 relation/isbn: 978-952-476-903-7
 relation/ispartofseries: Acta Wasaensia
 relation/numberinseries: 440
 ---
```
 
Comment: The language model seems to have translated the Finnish title into English on its own, instead of picking up the English language title from the publication as it was written by the author.

### University of Turku

```diff
 https://www.utupub.fi/handle/10024/152860
 ---
-title: Essays on income inequality and financial incentives to work
+title: Esa: Income inequality and financial incentives to work
 contributor/faculty: fi=Turun kauppakorkeakoulu|en=Turku School of Economics|
 contributor/author: Ollonqvist, Joonas
 publisher: fi=Turun yliopisto. Turun kauppakorkeakoulu|en=University of Turku, Turku School of Economics|
-date/issued: 2021
+date/issued: 2022
 relation/issn: 2343-3167
 relation/ispartofseries: Turun yliopiston julkaisuja - Annales Universitatis Turkuensis, Ser E: Oeconomica
 relation/numberinseries: 82
 ---
```

Comment: The title is a bit off and the model thinks the publication is newer than it really is. Both are clearly indicated in the PDF.

```diff
 https://www.utupub.fi/handle/10024/152852
 ---
-title: Run-time management of many-core SoCs: A communication-centric approach
+title: Run-time management of many-core systems – A communication-centric approach
 contributor/faculty: fi=Teknillinen tiedekunta|en=Faculty of Technology|
 contributor/author: Fattah, Mohammad
 publisher: fi=Turun yliopisto|en=University of Turku|
 date/issued: 2021
 relation/issn: 2736-9684
-relation/ispartofseries: Turun yliopiston julkaisua - Annales Universitatis Turkuensis, Ser. F: Technica - Informatica
+relation/ispartofseries: Turun yliopiston julkaisuja - Annales Universitatis Turkuensis, Ser. F: Technica - Informatica
 relation/numberinseries: 7
 ---
```

Comment: The title is a bit off - the model has turned "SoCs" (meaning systems-on-chip) into "systems" with no good reason. The series title is different too, but in this case the original metadata had a typo and the model got it right!

### Åbo Akademi University

```diff
 https://www.doria.fi/handle/10024/181280
 ---
-title: Production and testing of magnesium carbonate hydrates for thermal energy storage (TES) application
+title: Production and Testing of Magnesium Carbonate Hydrates for Thermal Energy Storage (TES) Application
 contributor/faculty: Faculty of Science and Engineering
 contributor/faculty: Fakulteten för naturvetenskaper och teknik
 contributor/faculty: Luonnontieteiden ja tekniikan tiedekunta
 contributor/author: Erlund, Rickard
-contributor/opponent: Professor Brian Elmegaard, Technical University of Denmark, Lyngby, Denmark
-contributor/supervisor: Professor Ron Zevenhoven, Åbo Akademi University, Turku
+contributor/opponent: Professor, Technical University of Denmark, Lyngby, Denmark
+contributor/supervisor: Professor, Åbo Akademi University, Turku
+contributor/supervisor: Professor, Åbo Akademi University, Turku
 publisher: Åbo Akademi University
 date/issued: 2021
 ---
```

Comment: The title differs in case only. The model couldn't extract the names of the opponent and supervisor.

```diff
 https://www.doria.fi/handle/10024/181139
 ---
-title: Coulometric Transduction Method for Solid-Contact Ion-Selective Electrodes
+title: Coulometric transduction method for solid-contact ion-selective electrodes
 contributor/faculty: Faculty of Science and Engineering
 contributor/faculty: Fakulteten för naturvetenskaper och teknik
 contributor/faculty: Luonnontieteiden ja tekniikan tiedekunta
 contributor/author: Han, Tingting
 contributor/opponent: Professor Agata Michalska, University of Warsaw, Warsaw, Poland
 contributor/supervisor: Professor Johan Bobacka, Åbo Akademi University, Åbo
 contributor/supervisor: Dr. Ulriika Mattinen, Åbo Akademi University, Åbo
 contributor/supervisor: Docent Zekra Mousavi, Åbo Akademi University, Åbo
 publisher: Åbo Akademi University
 date/issued: 2021
 ---
```

Comment: The title differs in case only.

### Lappeenranta University of Technology

```diff
 https://lutpub.lut.fi/handle/10024/163304
 ---
 title: Responsible business practices in internationalized SMEs
 contributor/faculty: fi=School of Business and Management|en=School of Business and Management|
 contributor/author: Uzhegova, Maria
 contributor/organization: Lappeenrannan-Lahden teknillinen yliopisto LUT
 contributor/organization: Lappeenranta-Lahti University of Technology LUT
 contributor/opponent: Rialp-Criado, Alex
 contributor/reviewer: Rialp-Criado, Alex
 contributor/reviewer: Andersson, Svante
+publisher: Lappeenranta-Lahti University of Technology LUT
 date/issued: 2021
 relation/issn: 1456-4491
 relation/ispartofseries: Acta Universitatis Lappeenrantaensis
 ---
```

Comment: The model extracted the publisher, which was missing from original metadata.

```diff
 https://lutpub.lut.fi/handle/10024/163258
 ---
-title: Life cycle cost-driven design for additive manufacturing : the frontier to sustainable manufacturing in laser-based powder bed fusion
+title: Life cycle cost-driven design for additive manufacturing: the frontier to sustainable manufacturing in laser-based powder bed fusion
 contributor/faculty: fi=School of Energy Systems|en=School of Energy Systems|
 contributor/author: Nyamekye, Patricia
 contributor/organization: Lappeenrannan-Lahden teknillinen yliopisto LUT
 contributor/organization: Lappeenranta-Lahti University of Technology LUT
 contributor/opponent: Hryha, Eduard
 contributor/reviewer: Hryha, Eduard
 contributor/reviewer: Mazzi, Anna
 publisher: Lappeenranta-Lahti University of Technology LUT
 date/issued: 2021
 relation/issn: 1456-4491
 relation/ispartofseries: Acta Universitatis Lappeenrantaensis
 ---
```

Comment: The only difference is the use of a colon with or without preceding space in the title.


## Installation

This has been tested using Python 3.10 on Ubuntu 22.04. Other recent Python
versions (3.8, 3.9) will probably work too.

Create a virtual environment and install the dependencies listed
`requirements.txt`:

    python3 -m venv venv
    source venv/bin/activate
    pip install -r requirements.txt

## Running the notebook

You will need to register an OpenAI account (same that you can use for
ChatGPT) and generate an API key in the OpenAI user interface. Store the key
in an environment variable:

    export OPENAI_API_KEY=<my-generated-api-key>

Then start Jupyter Notebook:

    jupyter notebook
