# Example project utilizing conda-project

## Quick start

First install [conda-project](https://github.com/conda-incubator/conda-project)

```
conda install -c defusco conda-project
```

In the directory where you cloned this repository you can run a command directly
in either the `default` or `dev` environment. For example to run `pre-commit` (typically installed in your base conda env)
on the `dev` env

```
conda project run --environment=dev pre-commit
```

or you can activate the development enviroment and run commands

```
conda project activate dev
```

On first-run of `run` or `activate` the desired environment is installed from the supplied
conda-lock files into `./envs/`. You can have both `dev` and `default` installed into `./envs`
at the same time.

For more details about what conda-project can do see the [User Guide](https://github.com/conda-incubator/conda-project/blob/commands/docs/source/user_guide.md)

## In-depth

A conda project contains environment specification files, locked environment specification files,
and a `conda-project.yml` file.

```
> tree -a ./
├── .pre-commit-config.yaml
├── README.md
├── conda-project.yml
├── default.conda-lock.yml
├── dev-extras.yml
├── dev.conda-lock.yml
└── environment.yml
```


This project defines two locked environments. The default environment, in `environment.yml` specifies


```yaml
dependencies:
  - python=3.9
  - datadog
  - pygithub
  - boto3
  - prefect
  - python-json-logger
  - slack-sdk

channels:
  - defaults
  - conda-forge

platforms:
  - osx-64
  - linux-64
  - osx-arm64
  - win-64
```

And the `dev` environment extends these packages with

```yaml
dependencies:
  - pytest
  - types-setuptools
  - types-requests
  - types-pyyaml
dependencies:
  - pytest
  - types-setuptools
  - types-requests
  - types-pyyaml

channels:
  - defaults
  - conda-forge

platforms:
  - linux-64
  - osx-64
  - osx-arm64
  - win-64
```

Then in the `conda-project.yml` file the conda environments called `default` and `dev` are declared
from these two files

```yaml
name: example

environments:
  default:
    - environment.yml
  dev:
    - environment.yml
    - dev-extras.yml
```

You'll see that this project contains two `.conda-lock.yml` files. These files were constructed
by conda-project using [conda-lock](https://github.com/conda/conda-lock) to fully specify package
versions for each environment. You can forcibly re-run the locking step for all environments.

```
conda project lock --force --all
```

If you've modified either of the environment.yml files, when running `conda project run` or
`conda project activate` it will re-lock the environments.

