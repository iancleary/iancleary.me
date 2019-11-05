---
title: An introduction to CI/CD with Travis CI and Python
date: 2019-11-03
summary: Umm - excuse me, if you have a minute, would you please build, test, and (if it all goes well) deploy my code for me - thanks!  Oh, and if you don't mind, would you please email me the status of it right when you finish. No questions asked, the first time I ask you - thanks! You can now proclaim to the world that you've read my snarky opener about how I've come to appreciate CI/CD tooling, and you can learn why you should give it a try!
tags: ['travis-ci', 'python'] #, 'docker', 'aws', 'ansible']
---

## Automate the boring stuff with Continuous Integration (CI), Continuous Deployment (CD)

I first learned about Travis-CI through venturing out into GitHub to checkout several open source projects. I've seen how they have continuous integration workflows set up test their code on every commit, branch, and pull request.

> That seemed so convenient so I went off to try it myself.
> I've documented what I've learned below to help get you started!

## Choosing a continuous integration service

### What to consider when creating a Python3 CI/CD pipeline

- **Python Community on GitHub** - support for all python3 🐍 versions.
- **Choose your testing framework** - Use it with any popular testing framework: pytest, unittest, etc.
- **Open vs. closed source** - I recommend whenever possible to choose an open source option. The below providers generally offer zero cost options for new open source projects with reasonable build 🛠 constraints. If you go closed source, you’re going to needs to fork over some 💵.

### Common providers

- **Travis-CI** - my first and has served me well
- **Circle CI** - seems to have a little saner syntax and finer configuration relative to Travis-CI
- **Gitlab** - leans open source where the others are closed source
- **GitHub Actions** - newer, native to GitHub, and offers Linux, Windows, and MacOS builds.

My learning started with Travis-CI and am using it since I am familiar with it. I haven't done a detailed comparison of the tools and it fits the needs of simpler python CI/CD projects. I want to look into GitHub actions, now that it’s coming out of Beta.

You are free to use what you prefer, feel is right, or would like to start with. This article is not a comparison, but rather an introduction into what the process looks like with one CI service.

> Start small and build up what you learn. You will make progress, run into challenges, figure some out. Remember to sleep!!!!
![coding-feelings](./images/coding-feelings.jpg)
Photo credit: [4geeksacademy.co/feelings-learning-coding/](https://www.4geeksacademy.co/feelings-learning-coding/)

It's an exhilirating, rewarding, and sometimes stressful day to day. 😬

There isn't a wrong option on your path to learning more about these tools and process. Where you start doesn't have to be where you are in the future. My recommendation is that you pick one, don't over think it, and get it up and working!  

> Put your work out there and connect with others.  You don't have to create something from scratch. You can help your favorite existing projects set up or improve their CI/CD pipeline.
> It's better when we build together. 🌎🌈🌍🚀🌏


Knowledge compounds and you'll marvel at where you're at soon. 🗺️ 🚀 💻

## Prerequisites

You should have basic knowledge about Github, Python, testing frameworks, and how to use the [Terminal](https://www.linode.com/docs/tools-reference/tools/using-the-terminal/).  Knowledge of Pipenv (Pipfiles) and pytest is a plus, but not required.  CI/CD tools are a great way to learn both.  We will set up a Travis-CI account as well.

-----

## YAML Basics

Most CI services interact with your repositories using a YAML configuration file.

> In case you need a tune up on [YAML](https://en.wikipedia.org/wiki/YAML), here is a recommended link:
>
> - [YAML Syntax, from Red Hat Ansible](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html)
>
> *Note: Ansible uses the YAML format, and is is another useful tool. A more detailed guide is an encouraged topic for another time.*

-----

## Travis-CI YAML Example

Here is a full example `.travis.yml` file from one of my projects. We will break down each section.

```yaml
# .travis.yml

dist: xenial

language: python

cache: pip

python:
    - "3.6"
    - "3.7"
    - "3.8"
    - "nightly"

matrix:
    allow_failures:
        - python: "nightly"

install:
    - pip install pipenv --upgrade-strategy=only-if-needed
    - pipenv install --dev

script:
    - bash scripts/test.sh

after_script:
    - bash <(curl -s https://codecov.io/bash)
```

-----

## Let's explain each section

```yaml
dist: xenial
```

**dist** is where the Ubuntu Release codename is specified. `xenial` is Ubuntu 16.04 LTS codename - see: http://releases.ubuntu.com/ for a full list.

This specifies the  base operating system used for the rest of workflow.

```yaml
language: python
```

**language** is the programming language used. This tutorial uses python, but it very well could be `cpp`, `go`, `rust`, etc.

```yaml
cache: pip
```

**cache** allows for python package versions to be stored between runs, to speed up sequential builds.  Cache can apply to more than just python packages.

```yaml
python:
    - "3.6"
    - "3.7"
    - "3.8"
    - "nightly"
```

**python**, given the above language specification, is a key for a sequence of python versions to perform builds against. Generally most CI tools use the latest bug release version for each minor version.  The build logs will tell you the specific versions.

```yaml
matrix:
    allow_failures:
        - python: "nightly"
```

**matrix** allows for modifications in the above build sequence.  In this case, the allow failures key is specifies a reference to the **python** sequence above, and has the value of `"nightly"`, meaning that that version is allowed to fail.  Think of it as early detection and future python development.  *Note: there are also development versions of each python version, such as `"3.8-dev"`.  These are used when python version are in release candidate or beta stages.

```yaml
install:
    - pip install pipenv --upgrade-strategy=only-if-needed
    - pipenv install --dev
```

**install** is the first section that is performed inside each of the build targets (virtual environments for each of the python versions above).  

> Each item in the sequence is executed ***in order!***

### Pipenv and Pipfiles

In this example, we are using `pipenv` and a `Pipfile`.

Most Pipfiles have production dependencies and development dependencies. You will often find the packages required to run test cases or build documentation under the `dev-packages` section.  Below is an example:

```bash
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[dev-packages]
mypy = "*"
black = "*"
jupyter = "*"
better-exceptions = "*"
pytest = "*"
pytest-cov = "*"
isort = "*"
flit = "*"
autoflake = "*"
mkdocs = "*"
mkdocs-material = "*"

[packages]
pydantic = "==0.32.2"
```

The command `pip install --dev` installs all packages under `[packages]` and `[dev-packages]`.  `pip install` would only install the  packages under `[packages]`.

#### Requirement.txt files

> *Note: It is also common to use `pip` with a `requirements.txt` file.*

```bash
pip install -r requirements.txt
```

Python.org now recommends `pipenv` as part of its general [tool recommendations](https://packaging.python.org/guides/tool-recommendations/).

### Optional aside about different virtual environment options

`Pipenv` has the advantage of providing a mechanism to install your packages into a virtual environment without another package.  There are many alternative - one simple alternaive is `venv`.

A newer alternative is `poetry` (https://poetry.eustace.io/).  `poetry` aims to facilitate what both requirements.txt and Pipfiles do not bundle, and that is released dependencies when deploying to PyPi.  You will typically see `flit` and a `pyproject.toml` file, or a `setup.py` file with redundant depedency information.  That redundancy is what `poetry` is aiming to improve upon.

-----

## Now lets help you get your CI/CD setup

### 1. Get setup to install my python package project template

Ensure you have a compatible python 🐍 environment on your computer.

```bash

python3 --version
python3 -m pip --version
python3 -m pytest --version
```

If you are missing any of the above, you can add the packages to your main python install.

> Recommended reading on Python [main install vs. altinstall](https://realpython.com/installing-python/#compiling-python-from-source). 
> *If you only have or want one python3 version on your machine, there is no harm to run the following commands. There are nuances between Ubuntu versions (16.04 vs. 18.04 vs. latest) on which python3 version is the default install.*

```bash

sudo apt-get update
sudo apt-get install python3-pip
sudo apt-get install python3-pytest
```

Now that we have confirmed your base python setup, let’s go ahead and ensure the pipenv and cookiecutter python packages are installed.

```bash

python3 -m pip install --user pipenv
python3 -m pip install --user cookiecutter
```

### 2. Create a Python project and setup the Pipenv virtual environment

```bash
# make sure your path finds --user installs

## add `export PATH="$HOME/.local/bin:$PATH"` 
## to your ~/.bashrc, ~/.zshrc file on linux

cookiecutter https://github.com/iancleary/pypackage
# For the sake of brevity, repos on GitHub can just use the 'gh' prefix
cookiecutter gh:iancleary/pypackage
````

*Fill out the wizard!*

Use `cd new-directory` to change into the new directory you just created (replace `new-directory`)

Run `pipenv` to display it's options. Take a look at what’s offered.

Next, run `pipenv install --dev` to install the production and development dependecies specified in the `Pipfile`.

Run `pipenv shell` to load the virtual environment.

### 3. Run the tests locally and make sure they pass

> I recommend you open up VS code or your text editor of choice to view the folder structure

Execute `./scripts/tests.sh` from within the directory's Pipenv.

This command executes a bash script that does several things:

- runs `pytest` to check test cases and check test coverage
- checks formatting with the `black` package
- runs static type checking against your code base with `mypy`
- checks import sorting from standard lib, your application, and custom packages

These are all things that should be relegated to scripts to allow you to free your mind from simple formatting and other draining tasks.  Let it do that work for you!

### 4. Push the directory to a remote git repo

I recommend you follow the guide on the Github Help page:

- [Adding an existing project to GitHub using the command line](https://help.github.com/en/github/importing-your-projects-to-github/adding-an-existing-project-to-github-using-the-command-line)

### 5. Connect Travis-CI to the repo

Again, the documentation of both GitHub and Travis-CI are great to follow for this step.

Head over to [Travis-CI](https://travis-ci.org/) and sign in with your GitHub account:

- https://help.github.com/en/enterprise/2.16/admin/developer-workflow/continuous-integration-using-travis-ci

Once you login to Travis-CI and Enable GitHub Access, you'll want to configure Travis from your Github Settings page.

![travis-ci-github-configured-applications](./images/travis-ci-github-configured-applications-20191103.png)

![travis-ci-github-authorization](./images/travis-ci-github-authorizating-repo-20191103.png)

### 6. Now we are configured, let's start a build

Make a simple change to the repo, either a commit, or open a new branch to start a build!

Grab your 🍿 and let's watch the CI process start and complete! 😎

![travis-ci-log](./images/travis-ci-pypackage-build-summary-20191103.png)

It’s very cool to watch the process. 
The key to remember is the repeatability of the test suite.

Once complete, you will have your new project added to the Travis-CI dashboard. The three red boxes below are:

1. The connected repo that is currently selected.
2. The action that started a build (commit or pull request)
3. The build matrix visualized, with links to view more details.

![travis-ci-dashboard](./images/travis-ci-pypackage-dashboard-20191103.png)

### 7. Happy coding with CI/CD pipelines 🎉🙌

Your setup should give you and others confidence that the code will work under those conditions. If it’s an open source repo, there is no taking your word for it. The logs are right there!

> You will not want to go back to your life before it!  
It is such a quality improvement!

-----

## Where to go from here?

- Start offloading and automating your testing, deployments, and other activities.  There is so many opportunites to tie in different hooks or features, this is just the beginning.

### Some examples of what else can you automate

#### Pypi publishing

I haven't done much in this space yet, but I've seen other projects successfully use the [flit](https://flit.readthedocs.io/en/latest/) python package.

If that is your next step, checkout `flit`.  I know there are some other good tutorials out there to get started inside and outside of a CI/CD pipeline.

#### Jupyter Notebook image deployment with CI/CD to Docker Hub

I've been using Jupyter notebooks and Docker more and more recently. I significantly improved the maintenance model of my personal image by setting up an automated testing and deployment pipeline using Travis CI. Check out my [personal Jupyter notebook](https://github.com/iancleary/personal-notebook) repository!

Checkout the Docker image is [iancleary/personal-notebook](https://hub.docker.com/r/iancleary/personal-notebook).

```bash
docker pull iancleary/personal-notebook
```

#### Amazon EC2 instance deployments

I also am in the process of learning how to leverage continuous deployment onto Amazon EC2 instances.

You can find my notes on [connecting AWS CodeDeploy and Travis CI](https://github.com/iancleary/travis-ci-aws-notes) using a Flask project.

This example is a proof of concept, and I'm excited to use this workflow more in the future.

-----

## References

- I learned a lot from https://github.com/tiangolo/fastapi and credit them as authors within my cookicutter pypackage [AUTHORS.md](https://github.com/iancleary/pypackage/blob/master/%7B%7Bcookiecutter.repo_name%7D%7D/AUTHORS.md).  This is not only nice to do, but is required by many open source licenses.
