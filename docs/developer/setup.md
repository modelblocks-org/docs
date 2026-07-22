# Setting up

Developing a Modelblocks modelblocks module requires careful handling of software dependencies, `Snakemake` requirements, and datasource locations.
This section details how a Modelblocks developer should handle each of these cases to ensure their module is reproducible in the long term.

## Software environments: `pixi`

The first step is ensuring you have an appropriate tool to manage your software environments.
We _require_ developers to use [`pixi`](https://pixi.prefix.dev/latest/) as their package management tool because it provides much needed stability to software projects.
In particular:

- It automatically maintains a stable lockfile (`pixi.lock`).
This helps ensure that modules can be rerun using the _exact_ versions of installed packages, and avoids human errors when updating dependencies.
- It provides multi-platform support which helps with our aim to develop modules that can run on the most popular operating systems (Windows, MacOS, Linux).

???+ info "Installing `pixi`"

    Follow the installation instructions on the [`pixi` documentation](https://pixi.prefix.dev/latest/).

    For unix-like systems, you can do:

    ```shell
    curl -fsSL https://pixi.sh/install.sh | sh
    ```

## Project templating: Modelblocks template

The second step is to ensure that your module has little friction with other modules.
We _require_ developers to use our standardised [`Snakemake` project template](https://github.com/modelblocks-org/data-module-template), which has the following advantages:

- It follows our [Convention][convention] in terms of file structure, interfacing, tests, and environment management.
- Provides pre-made continuous integration via [pre-commit.ci](https://pre-commit.ci/) and [GitHub actions](https://github.com/features/actions), tested in all major platforms.
- Enables developers to easily keep up with the [Convention][convention] via [`copier`'s update functionality](https://copier.readthedocs.io/en/stable/).
- Allows your module to be featured in the [Modelblocks website](https://www.modelblocks.org/).


??? info "Running the template"

    Up to date instructions are available in the [template's README](https://github.com/modelblocks-org/data-module-template#how-to-use-this-template).

    Generally:

    ```shell
    pixi global install copier # (1)!
    mkdir module_name & cd module_name # (2)!
    copier copy https://github.com/modelblocks-org/data-module-template.git # (3)!
    ```

    1. Install `copier` as a global tool in your machine via `pixi`.
    2. Create and open your project folder
    3. Start the templating process

    The templater will ask you a few questions, which will be used to pre-fill files in your project.

    ```html
    >🎤 Please enter your module's human readable name.
    module_hydropower_potential
    >🎤 Please enter your module's human readable name.
    Hydropower potential
    ```
