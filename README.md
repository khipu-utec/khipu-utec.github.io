# Khipu-docs

Khipudocs is the documentation website of the HPC cluster Khipu made written on Markdown and converted to HTML/CSS/JS with [mkdocs](https://www.mkdocs.org/). The theme used is [Material for Mkdocs](https://squidfunk.github.io/mkdocs-material/) with custom modifications. 

## Setup Locally

You must have a working `python3` installation with `pip3` package manager. Then you need to run the following commands:

1. Download a copy of the repository:

    ```bash
    git clone git@github.com:khipu-utec/khipu-utec.github.io.git
    cd khipu-utec.github.io
    ```

2. Create a python virtual environment and activate it. 

    ```bash
    # Create virtual environment
    python3 -m venv venv
    source venv/bin/activate
    ```
 3. Install the python dependencies

    ```bash
    pip install -r requirements.txt
    ```

## Edit with live preview

After you finish the setup above, you can server the website running the following commands:

```shell
mkdocs serve
```
This command will server the site on http://127.0.0.1:8000/. Every markdown file you edit inside `docs/` folder will be reloaded automatically.

## How to Deploy?

There is already a Github Actions Workflow to automate the build and deploy process. In order to activate that process you just need to push or merge your changes to the `main` branch. 
