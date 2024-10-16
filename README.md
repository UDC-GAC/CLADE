<img align="left" width="120" alt="Logo GAC" src="https://github.com/UDC-GAC/CLADE/assets/78569753/e79aa629-94ec-4597-b2b0-4049e6cb30a4">
<img align="right" width="250" alt="Logo GAC" src="https://github.com/user-attachments/assets/3ce42b5f-6730-406d-882b-f22fdc4e1829">

# Cloud Lakehouse Architecture as Digitalization Enabler (CLADE) 

This project, “Cloud Lakehouse Architecture as Digitalization Enabler (CLADE)”, addresses the challenges of current Data Lakehouse architectures. This new architecture proposes the combination of the data lake and data warehouse with advanced business analytics, open direct-access data formats, out-of-the box machine learning support and it provides outstanding performance. The main cloud data platforms used in this project are Snowflake and Databricks, which are actually trying to become a complete data lakehouse. All these concepts and platforms are explained in the CLADE documentation available via MKDocs in this repository.

## Organisation of the project

This repository serves for the CLADE documentation, being the basis of the project. In addition to this repository, CLADE is made up of [CLADE-TPCDS](https://github.com/torusware/clade-tpcds): python framework for running the TPC-DS benchmark on Snowflake and Databricks.

## Funding

Project funded by the Ministry of Science, Innovation and Universities of Spain (ref. TED2021-129177B-I00/MCIN/AEI/10.13039/501100011033) and by the European Union “NextGenerationEU”/PRTR".

<img width="618" alt="LogosNG(PlantillaInformeFinal)" src="https://github.com/user-attachments/assets/9bb2f9dd-0226-42ed-ba40-c4d211112e4b">
<img align="right" width="275" alt="Logo UDC" src="https://github.com/user-attachments/assets/db27406f-df7d-40e4-ac13-40d9fd947d62">

## Documentation usage

### Requirements
All necessary Python requirements are detailed in the `requirements.txt` file. To facilitate the installation process, enter the following command:
````
pip install -r requirements.txt
````

### MkDocs

There's a single configuration file named `mkdocs.yml`, and a folder named `docs` that contains the documentation source files.

MkDocs comes with a built-in dev-server that lets you preview the documentation as you work on it. Make sure you're in the same directory as the `mkdocs.yml` configuration file, and then start the server by running the `mkdocs serve` command:

````
$ mkdocs serve
INFO    -  Building documentation...
INFO    -  Cleaning site directory
INFO    -  Documentation built in 0.22 seconds
INFO    -  [15:50:43] Watching paths for changes: 'docs', 'mkdocs.yml'
INFO    -  [15:50:43] Serving on http://127.0.0.1:8000/
````
