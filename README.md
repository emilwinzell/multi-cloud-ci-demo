# Steps to this here project
* Create repo
* Launch any cloud environment. e.g. AWS Cloud9 or Azure Cloud Shell
* Create ssh key in environment
  ```
  ssh-keygen -t rsa
  cat <PATH>/.ssh/id_rsa.pub
  ```
* Upload ssh key to GitHub
* Create scaffolding
  * Makefile
  * Requirements
  
  Makefile should look like this:
```
install:
  pip install --upgrade pip &&\
		pip install -r requirements.txt

install-gcp:
	pip install --upgrade pip &&\
		pip install -r requirements-gcp.txt

install-aws:
	pip install --upgrade pip &&\
		pip install -r requirements-aws.txt

install-amazon-linux:
	pip install --upgrade pip &&\
		pip install -r amazon-linux.txt
lint:
	pylint --disable=R,C hello.py

format:
	black *.py

test:
	python -m pytest -vv --cov=hello test_hello.py

all: install lint test

  ```
  Several requirements files can be created for different environments:

  requirements.txt
  ```
pytest
click
pylint
pytest-cov
  ```
  requirements-aws.txt
```
pytest
```
  requirements_gcp.txt, 
  amazon-linux.txt
```
pytest
click
pylint
pytest-cov
black
```
* Create a python virtual environment and source it
```
python3 -m venv ~/.myrepo
source ~/.myrepo/bin/activate
```
* Create initial files e.g `hello.py` and `test_hello.py`
hello.py
```python

def add(x, y):
    """This is an add function, it adds two numbers and returns the result"""

    return x + y

def mod(x, y):
    """This is a modulus function, it does modulus """

    return x % y

print(add(1, 1))
print(add(2,-4))
print(mod(4,9))

```
test_hello.py
```python
from hello import add, mod


def test_add():
    assert 2 == add(1, 1)

def test_mod():
    assert 4 == mod(9,5)

```
* Run `make install` or equvalent of environment, then `make lint` and `make test`
* Setup GitHub actions in a .yml file for chosen environments
For AWS:
```yml
name: AWS 3.6
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
    - name: Install dependencies
      run: |
        make install-aws
```
For AWS Linux:
```yml
name: AWS Amazon Linux 2 3.7.9
on: [push]
jobs:
  build:
    runs-on: amazonlinux
    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 3.7.9
      uses: actions/setup-python@v1
      with:
        python-version: 3.7.9
    - name: Install dependencies
      run: |
        make install-amazon-linux
    - name: Lint
      run: |
        make lint
    - name: Test
      run: |
        make test
    - name: Format
      run: |
        make format
```
For GCP:
```yml
name: GCP Python 3.7
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 3.7
      uses: actions/setup-python@v1
      with:
        python-version: 3.7
    - name: Install dependencies
      run: |
        make install-gcp
    - name: Lint
      run: |
        make lint
    - name: Test
      run: |
        make test
    - name: Format
      run: |
        make format
        
```
For Azure:
```yml
name: Azure Python 3.6
on: [push]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up Python 3.6
      uses: actions/setup-python@v1
      with:
        python-version: 3.6
    - name: Install dependencies
      run: |
        make install
    - name: Lint
      run: |
        make lint
    - name: Test
      run: |
        make test
```
* Commit changes and push to Github
* Verify Github Actions Test Software

Video for setting up Azure with GitHub Actions:

[![Skaarmavbild 2022-10-07 kl  12 52 35](https://user-images.githubusercontent.com/67626018/194538015-be6767ea-2f66-4bb1-b6d0-a1c4be6cbe61.png)(https://www.youtube.com/watch?v=rXXtJpcVems)

