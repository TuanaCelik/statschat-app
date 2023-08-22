<img src="https://github.com/datasciencecampus/awesome-campus/blob/master/ons_dsc_logo.png">

# `StatsChat`
[![Stability](https://img.shields.io/badge/stability-experimental-orange.svg)](https://github.com/mkenney/software-guides/blob/master/STABILITY-BADGES.md#experimental)
[![codecov](https://codecov.io/gh/datasciencecampus/Statschat/branch/haystack-legacy/graph/badge.svg?token=QALqIC7CDX)](https://codecov.io/gh/datasciencecampus/Statschat)
[![Twitter](https://img.shields.io/twitter/url?label=Follow%20%40DataSciCampus&style=social&url=https%3A%2F%2Ftwitter.com%2FDataSciCampus)](https://twitter.com/DataSciCampus)
[![Shared under the MIT License](https://img.shields.io/badge/license-MIT-green)](https://github.com/datasciencecampus/Statschat/blob/haystack-legacy/LICENSE)
[![Mac-OS compatible](https://shields.io/badge/MacOS--9cf?logo=Apple&style=social)]()

## Code state

**Tested on OSX only**

**Peer-reviewed**

**Depends on external API's**

**Legacy branch**


## Introduction

This is an experimental application for semantic search of ONS statistical publications.
It uses [Haystack](https://docs.haystack.deepset.ai/docs) to implement a fairly simple embedding search and QA information retrieval
process.  Upon receiving a query, documents are returned as search results
using embedding similarity to score relevance.  Additionally, the relevant text is
passed to a locally-hosted Large language Model (LLM), which is prompted to write an
answer to the original question, if it can, using only the information contained within
the documents.

For this prototype, the program is run entirely locally; relevant web pages are scraped and the data
stored in `data/bulletins`, the docstore / embedding store that is created is likewise
in local folders and files, and the LLM and all other code is run in memory on your
desktop or laptop.

The search program should be able to run on a system with 16GB of ram.  The LLM is
set up to run on CPU at this research stage.  Different models from the Hugging Face
repository can be specified for the search and QA functions.



## Installation

The project requires specific versions of some packages so it is recommended to
set up a virtual environment.  Using venv and pip:

```shell
python3.10 -m venv env
source env/bin/activate

python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

### Pre-commit actions
This repository contains a configuration of pre-commit hooks. These are language agnostic and focussed on repository security (such as detection of passwords and API keys). If approaching this project as a developer, you are encouraged to install and enable `pre-commits` by running the following in your shell:
   1. Install `pre-commit`:

      ```shell
      pip install pre-commit
      ```
   2. Enable `pre-commit`:

      ```shell
      pre-commit install
      ```

Once pre-commits are activated, whenever you commit to this repository a series of checks will be executed. The pre-commits include checking for security keys, large files and unresolved merge conflict headers. The use of active pre-commits are highly encouraged and the given hooks can be expanded with Python or R specific hooks that can automate the code style and linting. For example, the `flake8` and `black` hooks are useful for maintaining consistent Python code formatting.

**NOTE:** Pre-commit hooks execute Python, so it expects a working Python build.

## Usage

By default, flask will look for a file called `app.py`, you can also name a specific python program to run.
With `--debug` in play, flask will restart every time it detects a saved change in the underlying
python files.

The first time you run the app, any ML models specified in the code will be downloaded
to your machine.  This will use a few GB of data and take a few minutes.

App and search pipeline parameter are stored and can be updated by editing `app_config.toml`.

### To webscrape the documents from ONS
#### We have removed this script, and for the sake of demonstration included some example scrape results so that the process can be continued from the next step below

```shell
# python statschat/webscraping/main.py
```

### To start the app, including creating the document store if it doesn't already exist
```shell
flask --debug run
```
or
```shell
python app.py
```

### Accessing the web UI and API

The flask app is set respond to https requests on port 5000. To use the user UI navigate in your browser to http://localhost:5000.

The API default url would be http://localhost:5000/api. See [API endpoint documentation](docs/api/README.md) for more details.

### Alternatively, to run the search evaluation pipeline

The StatsChat pipeline is currently evaluated based on small number of test question. The main 'app_config.toml' determines pipeline setting used in evaluation and results are written to `data/model_evaluation` folder.

```shell
python statschat/model_evaluation/evaluation.py
```

## Testing

Preferred unittesting framework is PyTest:

```shell
pytest
```

# Data Science Campus
At the [Data Science Campus](https://datasciencecampus.ons.gov.uk/about-us/) we apply data science, and build skills, for public good across the UK and internationally. Get in touch with the Campus at [datasciencecampus@ons.gov.uk](datasciencecampus@ons.gov.uk).

# License

<!-- Unless stated otherwise, the codebase is released under [the MIT Licence][mit]. -->

The code, unless otherwise stated, is released under [the MIT License][mit].

The documentation for this work is subject to [© Crown copyright][copyright] and is available under the terms of the [Open Government 3.0][ogl] licence.

[mit]: LICENSE
[copyright]: http://www.nationalarchives.gov.uk/information-management/re-using-public-sector-information/uk-government-licensing-framework/crown-copyright/
[ogl]: http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/