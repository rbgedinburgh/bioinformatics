This document describes the creation of the https://rbgedinburgh.github.io/bioinformatics/ website, which includes tutorials for bioinformatics pipelines and genomic data management for RBGE.

Flávia Fonseca Pezzini | fpezzini@rbge.org.uk | November 2021


# Creating the tutorials locally

The tutorials are created using [sphinx](https://www.sphinx-doc.org/en/master/) and its markup default language [reStructuredText](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html) (reST). All is written using VSCode.

```
sphinx-build --version   

sphinx-build 4.1.2
```
## Initial build

```
sphinx-quickstart docs
```

With the answers:

- Separate source and build directories (y/n): yes

- Project name: RBGE's pipelines

- Author name(s): Flavia Fonseca Pezzini

- Project release: 2021 Bioinformatics | Royal Botanic Garden Edinburgh

- Project language: Leave it empty (the default, English).


## Folder structure

The project is organised in three main directories: `source/`, `build/` and `docs/`. 

`source/` contains `conf.py` that is the configuration file of the project from which Sphinx documentation builder will read. In addition, `source/` contains all the `.rst` files, where the content is written. 

The folder `docs/` contains a copy of the `build/html/` folder that is read by the GitHub Pages, i.e., it is the source of what we see in the website https://rbgedinburgh.github.io/bioinformatics/. 

The `root` directory contains the `Makefile`. The command `make html` will build the `.html` files in the BUILDIR (`build/html`) by reading the `.rst` files from the SOURCEDIR (`souce/`). 

```
.
├── Makefile
├── build/
│   ├── doctrees/
│   └── html/
├── docs/
│   ├── README
│   ├── _panels_static/
│   ├── _sources/
│   ├── _static/
│   ├── aligning.html
│   ├── cutadapt.html
│   ├── fastqc.html
│   ├── genindex.html
│   ├── index.html
│   ├── installsoftware.html
│   ├── mapping.html
│   ├── objects.inv
│   ├── phylogeny.html
│   ├── rawreads.html
│   ├── search.html
│   ├── searchindex.js
│   ├── trimmomatic.html
│   ├── wheretorun.html
│   └── wheretostore.html
├── make.bat
├── master_RBGE_bioinformatics_website.md
└── source/
    ├── _build/
    ├── _static/
    ├── _templates/
    ├── aligning.rst
    ├── conf.py
    ├── cutadapt.rst
    ├── fastqc.rst
    ├── index.rst
    ├── installsoftware.rst
    ├── mapping.rst
    ├── phylogeny.rst
    ├── rawreads.rst
    ├── renaming.sh*
    ├── trimmomatic.rst
    ├── wheretorun.rst
    └── wheretostore.rst
   
```

## HTML theme

Used the [sphinx_rtd_theme](https://sphinx-rtd-theme.readthedocs.io/en/stable/). 

## Extensions

Included the extension [sphinx_panels](https://sphinx-panels.readthedocs.io/en/latest/) to create the dropdown menus.  


## How to build 

`make html` or `sphinx-build -b html source build/html`

## Workflow

1. Write edit the `.rst` files
2. Compile locally with `make html` 
3. Go to `build/html/` and open the `index.html` file in a browser
4. When happy with the results, `make github` to copy the contents of `build/html/` to `docs/`
5. Commit to remote repository https://github.com/rbgedinburgh/bioinformatics

## Upload of local tutorials to GitHub

First upload following this tutorial: https://www.docslikecode.com/articles/github-pages-python-sphinx/

Even though they create a second branch, I am using a **single branch**. The source for the published website is **main branch, docs/ folder**.

Include a `.nojekyll` file in the root of the directory you are reading from to overpass the default GitHub Pages layout: https://github.blog/2009-12-29-bypassing-jekyll-on-github-pages/


### Other relevant links

https://daler.github.io/sphinxdoc-test/includeme.html

https://lucasbardella.com/blog/2010/1/hosting-your-sphinx-docs-in-github

- Include `.DS_store` in `.gitignore`: https://stackoverflow.com/questions/107701/how-can-i-remove-ds-store-files-from-a-git-repository

- Include `.vscode` in `.gitignore` https://stackoverflow.com/questions/32964920/should-i-commit-the-vscode-folder-to-source-control

## Troubleshooting

https://python.plainenglish.io/how-to-host-your-sphinx-documentation-on-github-550254f325ae

https://stackoverflow.com/questions/47356997/pushed-nojekyll-file-to-github-pages-no-effect

