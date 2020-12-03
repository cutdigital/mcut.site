# MCUT project site

This is the repository for the [MCUT project page](https://cutdigital.github.io/mcut.github.io/)

The actual MCUT repository can be found [here](https://github.com/cutdigital/mcut.git)

## Local setup for development 

* Ensure you have `pip3` which is necessary to install an MkDocs theme that we use called [Material](https://github.com/squidfunk/mkdocs-material))
    - `curl https://bootstrap.pypa.io/get-pip.py | python`
    - `pip3 install --upgrade setuptools`
* Install MkDocs theme called "Material"
    - `sudo pip3 install mkdocs-material`

## Previewing the site

* Serve website locally (http://127.0.0.1:8000/)
    - `mkdocs serve`

## Deploying the site

* Deploy/publish the website by
    - `mkdocs gh-deploy`

