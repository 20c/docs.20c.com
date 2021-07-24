
Make sure to do each step as a separate commit, in case you run into issues later on.

Adding from the ctl template to a current project is not working yet, but you can use the base files as a starting point. See https://github.com/20c/ctl-tmpl-python

## Remove old python versions
Anything <3.6 can go
```
git checkout -b rmpy2
2to3 -wn
# DO: RUN TESTS
git commit -a -m "2to3"
```

## Add poetry
```
git checkout -b rmpy2
2to3 -wn
# DO: RUN TESTS
git commit -a -m "2to3"
```

## Add ctl

## Clean up code
```
ag future
ag past
ag XXX
# DO: fix as necessary
update tox.ini
add pre-commit

pre-commit run --all-files
# RUN TESTS
git commit -a -m "isort, pyupgrade, black"
```

## Spell check
```
# DO: spell check, at least readme
# DO: RUN TESTS
git commit -a -m "spell check"
```

### OLD
find . -type d \( -path ./.tox -o -path ./.venv \) -prune -false -o -name '*.py' -print0 | xargs -0 pyupgrade --py36-plus
git commit -a -m "pyupgrade"

find . -type d \( -path ./.tox -o -path ./.venv \) -prune -false -o -name '*.py' -print0 | xargs -0 grep future
find . -type d \( -path ./.tox -o -path ./.venv \) -prune -false -o -name '*.py' -print0 | xargs -0 grep past

black .
git commit -a -m "black"
```