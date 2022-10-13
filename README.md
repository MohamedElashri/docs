# docs
MKDocs UC-LHCb website source

## Usage

To write the docs you should follow [MkDocs documentation](https://www.mkdocs.org/)

## build 

The command used to build the website 

```bash
mkdocs build -t readthedocs --no-directory-urls
```

## deploy 

to deploy to github pages

```bash
mkdocs gh-deploy -m '<Commit message here>' -b gh-pages  -t readthedocs --no-directory-urls
```
