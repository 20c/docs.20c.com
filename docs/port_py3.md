
Make sure to do each step as a separate commit, in case you run into issues later on.


Create a virtualenv and install 2to3, and possibly tox or whatever you'll need.

```sh
python3 -m venv ~/.local/pyenv/2to3
. ~/.local/pyenv/2to3/bin/activate
pip install -U pip 2to3 tox
```

Adding from the ctl template to a current project is not working yet, but you can use the base files as a starting point. See https://github.com/20c/ctl-tmpl-python

## Run tests

Before starting, make sure the existing tests pass. Generally, tox is used for testing, so you'll have to look for how it's tested, but if it's tox, just test in a single env.

Look in the tox.ini file and see what the environments are under `envlist`, you might have to specify more for the env.

e.g. #1
```sh
tox -e py36
```
e.g. #2
```sh
tox -e py38-django30-drf
```


## Remove old python versions
Anything <3.6 can go
```bash
git checkout -b rmpy2
2to3 -wn
# DO: RUN TESTS
git commit -a -m "2to3"
```

## Add poetry
```toml
[tool.poetry]
name = "{{ project_name }}"
repository = "{{ project_repo }}"
readme = "README.md"
version = "{{ version }}"
description = "{{ project_description }}"
authors = [ "20C <code@20c.com>",]
license = "{{ project_license }}"
classifiers = [
  "Topic :: Software Development", 
  "License :: OSI Approved :: Apache Software License", 
  "Programming Language :: Python :: 3.6", 
  "Programming Language :: Python :: 3.7", 
  "Programming Language :: Python :: 3.8", 
  "Programming Language :: Python :: 3.9",
]

[tool.poetry.dependencies]
python = "^3.6"

[tool.poetry.dev-dependencies]
# test
pytest = ">=6.0.1"
pytest-django = ">=3.8.0"
pytest-cov = "*"

# lint
bandit = "^1.6.2"
black = "^20.8b1"
isort = "^5.7.0"
flake8 = "^3.8.4"

# docs
markdown = "*"
markdown-include = ">=0.5,<1"
mkdocs = ">=1.0.0,<2.0.0"

# ctl
ctl = "^1.0.0"
jinja2 = "^2.11.2"
tmpl = "^0.3.0"

[tool.poetry.plugins."markdown.extensions"]
pymdgen = "pymdgen.md:Extension"

[build-system]
requires = [ "poetry>=0.12",]
build-backend = "poetry.masonry.api"

[tool.isort]
profile = "black"
multi_line_output = 3
```

## Add ctl

## Clean up code
```bash
ag future
ag past
ag XXX
# DO: fix as necessary
update tox.ini
add pre-commit

poetry run pre-commit run --all-files
# RUN TESTS
git commit -a -m "isort, pyupgrade, black"
```

## Spell check
```bash
# DO: spell check, at least readme
# DO: RUN TESTS
git commit -a -m "spell check"
```

### OLD
```bash
find . -type d \( -path ./.tox -o -path ./.venv \) -prune -false -o -name '*.py' -print0 | xargs -0 pyupgrade --py36-plus
git commit -a -m "pyupgrade"

find . -type d \( -path ./.tox -o -path ./.venv \) -prune -false -o -name '*.py' -print0 | xargs -0 grep future
find . -type d \( -path ./.tox -o -path ./.venv \) -prune -false -o -name '*.py' -print0 | xargs -0 grep past

black .
git commit -a -m "black"
```