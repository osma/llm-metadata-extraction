# llm-metadata-extraction

Experiment on metadata extraction using large language models.

The experiment itself is in the Jupyter notebook
[ExtractMetadataUsingFinetunedGPT3.ipynb](ExtractMetadataUsingFinetunedGPT3.ipynb)
which you can view directly on GitHub without installing anything.

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
