# Model Collection Philosophy

**Important note: at the moment, lots of code is purely being duplicated, awaiting for more UC to really abstract mutualizable data processing logic.**

This project contains a collection of models developed by the Atos AI4sim R&D team and is intended for research purposes.

The current workflow is based entirely on NumPy, PyTorch, PyG and Lightning. 

To take care of the boiler-plate, early stopping, and tensorboard logging, we integrate directly with PyTorch Lightning. For each new UC, we use the same file tree, namely:

* `configs/` contains the experiments configuration files in the Lightning CLI format. An experiment designates a specific training run with a specific model and a specific dataset + split.
* `data/` contains the raw and processed data, including normalization factors and explicit train / val / test split files.
* `tests/` contains unit tests modules.
* `notebooks/` contains example Jupyter notebooks, to illustrate the use case code usage.
* `config.py` exposes global paths and path specific to current experiment.
* `data.py` deals with dataset and datamodule creation.
* `models.py` deals with model and module creation, including training logic. Declare models to the Lightning Model Registry. Users should only expose arguments that they wish to perform HPO on in their model constructor.
* `plotters.py` takes care of plots generation for the test set.
* `trainer.py` is the main entrypoint responsible for creating a Trainer object, a CLI, and saving artifacts in the experiment directory.
* `noxfile.py` is the Nox build tool configuration file that defines all targets available for the use case.

# Quick Start
Each _Model Collection_ use case can be experimented in an easy and quick way using predifined command exposed through the Nox tool.

## Requirements
The following procedures only require the Nox tool to be installed.
````shell
pip install nox
````
[Nox](https://nox.thea.codes/en/stable/) is a python build tool, that allows to define targets (in a similar way that Make does), to simplify command execution in development and CI/CD pipeline.

Each nox target is executed, by default, in a specific virtualenv that ensure the partitioning and the reproducibility of the commands.
Nevertheless, if required, it is possible to re-execute a target without recreating its virtualenv using the `-r` flag in the `nox` command.
More information about the nox usage can be found on the nox [command line usage](https://nox.thea.codes/en/stable/usage.html) web page.

## Experiment a use case
Several Nox targets allow to handle easily an experimentation of any use case on a demo dataset and configuration.

Choose the _Model Collection_ use case you want to experiment and go in.
````shell
cd weather_forcast/gwd
````
There you can display the list of the available targets with:
````shell
nox --list
````
Please note, some of them are experimentation oriented, while other ones are CI/CD oriented.

*Coming soon ...*

You can launch a demo training on the model use case with:
````shell
nox -s train
````

## Development mode
The nox target are also very useful to launch generic command during development phase.

### Run unit tests
You can run the whole unit test suite of a use case, using ``pytest``, with:
````shell
nox -s tests
````
This target also prints out the coverage report and save a xml version in ``.ci-reports/``.

### Run linting
You can run the python linting of the code use case, using ``flake8``, with:
````shell
nox -s lint
````