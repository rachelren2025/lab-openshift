# Lab OpenShift

Fire!

[![Build Status](https://github.com/nyu-devops/lab-openshift/actions/workflows/ci.yml/badge.svg)](https://github.com/nyu-devops/lab-openshift/actions)
[![codecov](https://codecov.io/gh/nyu-devops/lab-openshift/branch/master/graph/badge.svg?token=y6OUlCB4bC)](https://codecov.io/gh/nyu-devops/lab-openshift)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![made-with-python](https://img.shields.io/badge/Made%20with-Python-red.svg)](https://www.python.org/)
[![Open in Remote - Containers](https://img.shields.io/static/v1?label=Remote%20-%20Containers&message=Open&color=blue&logo=visualstudiocode)](https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/nyu-devops/lab-openshift)

NYU DevOps lab on **Lab OpenShift**

## Introduction

This lab introduces **Red Hat OpenShift** as a Platform As A Service (PaaS) solution to cloud computing. It also demonstrates how to create a simple RESTful service using Python Flask and PostgreSQL. The resource model is persistence using SQLAlchemy to keep the application simple. It's purpose is to show the correct API calls and return codes that should be used for a REST API.

**Note:** The base service code is contained in `routes.py` while the business logic for manipulating Pets is in the `models.py` file. This follows the popular Model View Controller (MVC) separation of duities by keeping the model separate from the controller. As such, we have two test suites: one for the model (`test_models.py`) and one for the service itself (`test_routes.py`)

## Prerequisite Software Installation

This lab uses Docker and Visual Studio Code with the Remote Containers extension to provide a consistent repeatable disposable development environment for all of the labs in this course.

You will need the following software installed:

- [Docker Desktop](https://www.docker.com/products/docker-desktop)
- [Visual Studio Code](https://code.visualstudio.com)
- [Remote Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension from the Visual Studio Marketplace

All of these can be installed manually by clicking on the links above or you can use a package manager like **Homebrew** on Mac of **Chocolatey** on Windows.

Alternately, you can use [Vagrant](https://www.vagrantup.com/) and [VirtualBox](https://www.virtualbox.org/) to create a consistent development environment in a virtual machine (VM).

You can read more about creating these environments in my article: [Creating Reproducable Development Environments](https://johnrofrano.medium.com/creating-reproducible-development-environments-fac8d6471f35)

## Bring up the development environment

To bring up the development environment you should clone this repo, change into the repo directory:

```bash
git clone https://github.com/nyu-devops/lab-openshift.git
cd lab-openshift
```

Depending on which development environment you created, pick from the following:

### Start developing with Visual Studio Code and Docker

Open Visual Studio Code using the `code .` command. VS Code will prompt you to reopen in a container and you should say **yes**. This will take a while as it builds the Docker image and creates a container from it to develop in.

```bash
code .
```

Note that there is a period `.` after the `code` command. This tells Visual Studio Code to open the editor and load the current folder of files.

Once the environment is loaded you should be placed at a `bash` prompt in the `/app` folder inside of the development container. This folder is mounted to the current working directory of your repository on your computer. This means that any file you edit while inside of the `/app` folder in the container is actually being edited on your computer. You can then commit your changes to `git` from either inside or outside of the container.

### Using Vagrant and VirtualBox

Bring up the virtual machine using Vagrant.

```shell
vagrant up
vagrant ssh
cd /vagrant
```

This will place you in the virtual machine in the `/vagrant` folder which has been shared with your computer so that your source files can be edited outside of the VM and run inside of the VM.

## Running the tests

As developers we always want to run the tests before we change any code. That way we know if we broke the code or if someone before us did. Always run the test cases first!

Run the tests using `pytest`

```shell
pytest
```

Nose is configured via the included `setup.cfg` file to automatically include the flags `--pspec` so that the R-Spec red-green-refactor is meaningful. If you are in a command shell that supports colors, passing tests will be green while failing tests will be red.

It also uses the flag `--cov=service` so that configured to automatically run the `coverage` tool and you should see a percentage-of-coverage report at the end of your tests. If you want to see what lines of code were not tested use:

```shell
coverage report -m
```

This is particularly useful because it reports the line numbers for the code that have not been covered so you know which lines you want to target with new test cases to get higher code coverage.

You can also manually run `pytest` with `coverage` (but `setup.cfg` does this already)

```shell
pytest --pspec --cov=service --cov-fail-under=95 --disable-warnings
```

Try and get as close to 100% coverage as you can.

It's also a good idea to make sure that your Python code follows the PEP8 standard. Both `flake8` and `pylint` have been included in the `requirements.txt` file so that you can check if your code is compliant like this:

```shell
flake8 . --count --max-complexity=10 --max-line-length=127 --statistics
pylint service tests --max-line-length=127
```

Visual Studio Code is configured to use `pylint` while you are editing. This catches a lot of errors while you code that would normally be caught at runtime. It's a good idea to always code with pylint active.

## Running the service

The project uses *honcho* which gets it's commands from the `Procfile`. To start the service simply use:

```shell
honcho start
```

You should be able to reach the service at: http://localhost:8000. The port that is used is controlled by an environment variable defined in the `.flaskenv` file which Flask uses to load it's configuration from the environment by default.

## Shutdown development environment

If you are using Visual Studio Code with Docker, simply existing Visual Studio Code will stop the docker containers. They will start up again the next time you need to develop as long as you don't manually delete them.

If you are using Vagrant and VirtualBox, when you are done, you can exit and shut down the vm with:

```shell
exit
vagrant halt
```

If the VM is no longer needed you can remove it with:

```shell
vagrant destroy
```

## What's featured in the project?

    * service/routes.py -- the main Service routes using Python Flask
    * service/models.py -- the data model using SQLAlchemy
    * tests/test_routes.py -- test cases against the Pet service
    * tests/test_models.py -- test cases against the Pet model

## License

Copyright (c) 2016-2023 John Rofrano. All rights reserved.

Licensed under the Apache License. See [LICENSE](LICENSE)

This repo is part of the NYU masters class: **CSCI-GA.2820-001 DevOps and Agile Methodologies** conceived, created and taught by *John Rofrano*
