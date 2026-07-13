# Setting up

Modelblocks relies on a core set of tools to ensure modules can be run on most platforms.
This section details which these tools are, why they are needed, and how to install them.

## Software environments: `pixi` or `miniconda`


Modelblocks modules rely on [`Snakemake`](https://snakemake.readthedocs.io/en/stable/index.html) for environment management and software execution.
`Snakemake` can be installed using either `pixi` or `miniconda`.
In general, we highly recommend `pixi`, as it is a faster and more modern tool.

???+ info "Setting up a project with `pixi`"

    Follow the installation instructions on the [`pixi` documentation](https://pixi.prefix.dev/latest/).

    Once installed:

    ```shell
    # Create and open your project directory
    pixi init my_project & cd my_project
    # Install snakemake at the project level
    pixi workspace channel add conda-forge bioconda
    pixi add snakemake-minimal conda
    ```


??? info "Setting up a project with `conda`"

    Follow the installation instructions on the [`conda` documenation](https://docs.conda.io/en/latest/).
    We recommend `miniconda` for a lightweight, minimal installation.

    Once installed:

    ```shell
    # Create and open your project directory
    mkdir ./my_project & cd ./my_project
    # Create a conda environment for your project
    conda create -n my_project -c conda-forge -c bioconda --strict-channel-priority snakemake-minimal
    conda activate my_project
    ```

## Project templating: Modelblocks or `Snakemake` standards

To setup your `Snakemake` project structure, we recommend using [our own project template](https://github.com/modelblocks-org/data-module-template) (using `pixi`) or the [official `Snakemake` template](https://github.com/snakemake-workflows/snakemake-workflow-template) (using `conda`).
Instructions for either template are available in their respective repositories.

??? info "What is the difference?"

    Both templates have a very similar structure, and will result in a well organised project.
    However, the Modelblocks version offers some additional advantages:

    - More modern setup.
    - Automated templated generation and updates via [`copier`](https://copier.readthedocs.io/en/stable/).
    - Tuned for modularisation, making it easier to convert your project to a Modelblocks module in the future.
