# Khipu-docs

Khipudocs is the documentation website of the HPC cluster Khipu made with [Hugo](https://gohugo.io/). We are using [Geekdocs](https://geekdocs.de/) as the theme for this website. To change any template or partial of this template, use its [repo](https://github.com/khipu-utec/hugo-geekdoc) 


## Setup Locally

First of all, you need to install Hugo in your local machine. You can follow the [hugo website](https://gohugo.io/getting-started/quick-start/#step-1-install-hugo) to do that. Also, you need NodeJs 16.0.4 to bundle the Hugo theme.

Then you need to run the following commands:

```bash
# Locate to the theme folder
cd themes/hugo-geekdoc
# Install theme dependencies
npm install 
# Bundle the theme
npm run build
# Locate to the root folder
cd ../..
# Run the hugo server
hugo server -D
```

## How to Deploy?

There is already a Github Actions Workflow to automate the build and deploy process. In order to activate that process you just need to push or merge your changes to the `main` branch. 
