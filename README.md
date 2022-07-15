# DeTox-Dataset

This repository contains the DeTox-Dataset, a German Offensive Language and Conversation Analysis dataset. It arose from
the research project DeTox ([https://projects.fzai.h-da.de/detox/](https://projects.fzai.h-da.de/detox/)) in 2022, which
targeted at detection of toxicity and aggression in comments in the internet.

## Content

- public dataset without comment texts (you can [request](#request-the-complete-dataset) the full version) as sqlite
  database
- database description/documentation (html)
- Jupyter Notebook to get started with the database in Python
- annotation guidelines (original German version and English translation)

## About the Dataset

This is a German offensive language and conversation analysis dataset ("DeTox-Dataset") containing 10,278 annotated
Twitter comments. The data was collected in the first half of 2021. The comments were annotated by three annotators each
with 12 different labels. The dataset contains all single annotations including meda data (e.g. annotation duration) as
well as a proposed gold standard.
For all details please refer to our [paper](https://aclanthology.org/2022.woah-1.14/).

## Get Started

The dataset comes in an SQLite database. The database tables and attributes are described in the documentation
file ([DeTox-Dataset_doc.html](DeTox-Dataset_doc.html)).
An example how to access the database using python is given below and in the [Jupyter-Notebook](get-started.ipynb).

```python
import pandas as pd
from pathlib import Path
import sqlite3 as sqlite

database_path = Path("DeTox-Dataset_public.sqlite3") # path to your database file
dbconnect = None
cursor = None

if not database_path.is_file():
    print(f"Database {database_path} does not exist. Creating a new database now ...")
try:
    # open database connection
    dbconnect = sqlite.connect(database_path, detect_types=sqlite.PARSE_DECLTYPES | sqlite.PARSE_COLNAMES)
    cursor = dbconnect.cursor()
    # Check Foreign-Key Constraints can be switched off if needed with the following line:
    # cursor.execute("PRAGMA foreign_keys = OFF;")
except sqlite.Error as e:
    # if errors occur
    print("Error %s:" % e.args[0])

# Database request
annotations = pd.read_sql_query("SELECT * from Goldstandard;", con=dbconnect)
annotations.head()

# close connection
dbconnect.close()
``` 

## Request the complete dataset

Unfortunately, we can't publish the complete dataset including the commtents text here (this version here is missing the
text, it only contains the Twitter-IDs of the comments). But we are happy to provide the complete data to you, if you
send us an email to [melanie.siegel@h-da.de](mailto:melanie.siegel@h-da.de?subject=[GitHub]%20DeTox-Dataset%20Request)
 describing in short for what you need the dataset.

## Citation

If you use the dataset, please cite our respective paper "DeTox: A Comprehensive Dataset for German Offensive Language and Conversation Analysis", which was presented on the [6th Workshop on Online Abuse and Harms](https://www.workshopononlineabuse.com/) on 14th July 2022 as part of the [NAACL](https://2022.naacl.org/) conference.

**ACL-Style:**  
Christoph Demus, Jonas Pitz, Mina Schütz, Nadine Probol, Melanie Siegel, and Dirk Labudde. 2022. [DeTox: A Comprehensive Dataset for German Offensive Language and Conversation Analysis.](https://aclanthology.org/2022.woah-1.14/) In Proceedings of the Sixth Workshop on Online Abuse and Harms (WOAH), pages 143–153, Seattle, Washington (Hybrid). Association for Computational Linguistics.

**BibTeX:**
```text
@inproceedings{demus-etal-2022-comprehensive,
    title = "DeTox: A Comprehensive Dataset for {G}erman Offensive Language and Conversation Analysis",
    author = {Demus, Christoph  and
      Pitz, Jonas  and
      Sch{\"u}tz, Mina  and
      Probol, Nadine  and
      Siegel, Melanie  and
      Labudde, Dirk},
    booktitle = "Proceedings of the Sixth Workshop on Online Abuse and Harms (WOAH)",
    month = jul,
    year = "2022",
    address = "Seattle, Washington (Hybrid)",
    publisher = "Association for Computational Linguistics",
    url = "https://aclanthology.org/2022.woah-1.14",
    pages = "143--153",
    abstract = "In this work, we present a new publicly available offensive language dataset of 10.278 German social media comments collected in the first half of 2021 that were annotated by in total six annotators. With twelve different annotation categories, it is far more comprehensive than other datasets, and goes beyond just hate speech detection. The labels aim in particular also at toxicity, criminal relevance and discrimination types of comments.Furthermore, about half of the comments are from coherent parts of conversations, which opens the possibility to consider the comments{'} contexts and do conversation analyses in order to research the contagion of offensive language in conversations.",
}
```
