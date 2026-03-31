# Modelblocks documentation

`modelblocks` aims to be a collection modular tools and modularisation guidelines that help energy modellers produce high-quality research that is repeatable, understanable, and easy to use.

Please read all about it in our [documentation](https://modelblocks.readthedocs.io/en/stable/)!

## `modelblocks` documentation development

We rely on [`pixi`](https://pixi.sh/latest/) for development and maintenance.
To install, simply run the following:

```bash
git clone git@github.com:modelblocks-org/docs.git
cd docs
pixi install --all
```

## Helper commands

- `pixi run build-docs`: build a local version of the documentation
- `pixi run serve-docs`: render the documentation on your browser
- `pixi run style`: run CI linting, refractoring and spellchecking
- `pixi run test`: quick local test of the documentation
