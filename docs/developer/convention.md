# Convention

This page specifies the Modelblocks convention related to this documentation release version.

It should be followed by both **Core**{ .badge-core } and **Community**{ .badge-community } modules.

## Software requirements

All modules should comply with the following requirements at a software design / execution level.

### Environment management

#### `pixi` centralisation

The `pixi.lock` file should be the primary source of truth when it comes to all the dependencies of the software.

Other dependency definitions (such as `Snakemake`'s pinned environment files) should be generated using `pixi.lock`.

The software should export the necessary environments to `Snakemake` via the `pixi run export-snakemake-env [environment_name]` task command.

#### Environment separation

The project should separate the environment used for module development (`default`) from the environment used during rule execution (`module`) to keep module execution lean on the user side.

#### `conda` exclusivity

The module's `Snakemake` workflow should exclusively rely on `conda`  as a software package repository.

Other indexers, such as `pypi`/`pip` are **explicitly discouraged** as the long-term stability of the software is not guaranteed when repository sources are mixed.

### Interfacing

#### Standardised default input / output locations

Modules should expect user inputs and place processed outputs in the following locations by default.

- User inputs should be placed in `resources/user/`.
- Intermediate module files should be placed in `resources/automatic/`.
- Module outputs should be placed in `results/`.
- Log files should be placed in `logs/`.

#### `pathvar` rewiring

All the input / output files of the module should have the ability to be easily rewired using `Snakemake`'s `pathvar` functionality, with the following characteristics.

- Each user input file should have its own exclusive `pathvar`.
- Each module output file should have its own exclusive `pathvar`.
- The module should provide directory-wide `pathvars` for `resources/`, `results/` and `logs/`.

### Input validation

#### Configuration

Projects should validate input configurations using a standardised schema located in `workflow/internal/config.schema.yaml`, and provide a configuration example with recommended defaults in `config/config.yaml`.

#### User files

When possible, modules should validate input files using data tools like [`pandera`](https://pandera.readthedocs.io/en/stable/) or equivalents.

## Repository requirements

All modules show comply with the following requirements at a repository level.

### Licensing

Projects must use either Apache-2.0 or MIT open-source licenses, which are [OSI approved](https://opensource.org/licenses).

Projects should maintain an `AUTHORS` file detailing licensing attribution of the software.

### Code hosting

Projects must be openly available on platforms compatible with [`Snakemake`'s code hosting calls](https://snakemake.readthedocs.io/en/stable/snakefiles/modularization.html#code-hosting-providers).
At the time of writing this includes only GitHub and GitLab.

### Versioning

Projects must be version controlled with official releases, which can be used to specify the version of the project used in a study and/or dataset.
Project developers are free to choose their preferred approach (e.g., [SemVer](https://semver.org/) or [CalVer](https://calver.org/)).

### Documentation

Projects must provide documentation to ensure the methods and reasoning behind their code can be understood.
We recommend a pragmatic approach that balances explainability against maintenance burdens.

#### General README

This file, located in `README.md`, should provide the following sections:

- A summary of the module's purpose with a brief mention of `Snakemake` to aid in automated indexing in the [workflow catalog](https://snakemake.github.io/snakemake-workflow-catalog/).
- An overview of the module's processing stages.
- An overview or reference to the module's interfacing and configuration requirements.
- An brief description of how to initialise the software for development tasks and local testing.
- References related to the module's methodology.
- Contributor attribution following [All Contributors](https://allcontributors.org/) standards.

#### Configuration README

This file, located in `config/README.md`, should provide an overview of the module's configuration options and point users to the relevant schema files.

#### INTERFACE.yaml

This file should detail the module's input/output file structure from a user's perspective, with the following structure:

- `pathvars`: describes the pathvars provided by the module in terms of default value and description, grouped into:
    - `snakemake_defaults`: defines logs, resources and results.
    - `user_resources`: user input files starting with the prefix `<resources>/user/`.
    - `results`: module outputs starting with the prefix `<results>/`.
- `wildcards`: Descriptions of {wildcards} used in resource or result paths. Every declared wildcard must be used, and every used wildcard must be declared.
- `convention_version`: The Modelblocks convention version, validated as semantic versioning.

### Continuous integration

Projects must comply with the following testing requirements at minimum.
These tests should execute successfully before changes are integrated into the repository.
The template should provide pre-configured pull request test setups in `.github/workfows/pr-ci.yml`.

#### Module testing

Projects must ensure that the provided `integration_test.py` file excecutes successfully in **all major platforms** (Linux, MacOS, Windows).

This test should evaluate the following:

- The module's `conda` environments match their `pixi` counterparts.
- The module's `INTERFACE.yaml` file complies with the schema defined in this convention.
- Essential interfacing and documentation files (as defined by the template) are present in the repository.
- A minimal integration test simulating a user importing and executing the module, defined in `tests/integration/Snakefile`.

#### Repository health

Projects should comply with a set of minimal linting and formatting tests to aid in the mantainability of the codebase.
These tests are defined in `.pre-commit-config.yaml`, and make use of [pre-commit.ci](https://pre-commit.ci/).

Among other things, these tests evaluate the following:

- Machine redability issues such as removing trailing whitespaces and adding end-of-file newlines.
- Repository quality issues such as large files, the use of GitHub submodules, illegal Windows filenames, and malformed configuration files (e.g., .json or .yaml).
- Code quality for python files (using [`ruff`](https://docs.astral.sh/ruff/)) and `Snakemake` files (using [`snakefmt`](https://github.com/snakemake/snakefmt)).
- Spelling mistakes (using [`codespell`](https://github.com/codespell-project/codespell)).


## Templating requirements

The Modelblocks initiative should maintain a template that respects this Convention with the goal of avoiding chores and reduce human errors.

### Answers file

Projects should retain the `.copier-answers.yml` file generated by the templater.
This file should **not** be modified manually to prevent issues during template updates.

### Periodic updates

The template should provide automated scheduled checks to identify if a new version of the template has been released.
