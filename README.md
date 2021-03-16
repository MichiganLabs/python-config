## MichiganLabs Python Configurations

Any changes to these files for a project should be pushed back to this repository. It is recommended that this
repository be tracked as a git submodule so it can be synced between this repository and other projects easily:

```bash
git submodule add https://github.com/MichiganLabs/python-config.git .ml-python-configs
```

To update the settings in your project, run:

```bash
git submodule foreach git pull
```

Then commit the changes to .gitmodules

## [Pylint](http://pylint.pycqa.org/en/latest/)
This `.pylintrc` file should be used as the configuration for pylint on all projects. To use it in your project you will need to use the `--rcfile`  flag. For example:

```bash
pylint --rcfile=.ml-python-configs/.pylintrc src
```

There is also an included configuration specifically for tests. This should also be used and eliminates a couple of the more annoying configurations like `"too-many-locals"`, or `"unused-argument"` which tends to have false positives on pytest fixtures.

```bash
pylint --rcfile=.ml-python-configs/.tests-pylintrc tests
```

## [Pytest](https://docs.pytest.org/en/stable/) and [coverage.py](https://coverage.readthedocs.io/en/latest/)
This `.coveragerc` file should be used on all python projects going forward. It requires
100% test coverage. Although this number may be adjusted if the project was not created by MichiganLabs, anything below 100% becomes somewhat meaningless.

```bash
poetry run pytest --cov=your_package --cov-config=.ml-python-configs/.coveragerc tests
```

The minimum coverage value can be overridden from the commandline if adding to an existing project with the `--cov-fail-under` flag.

To integrate it with VS Code, add the following to your `.vscode/settings.json` file in your project:

```json
"python.testing.pytestEnabled": true,
"python.testing.pytestPath": "${workspaceFolder}/.venv/bin/py.test",
"python.testing.pytestArgs": [
    "--cov=your_package",
    "--cov-config=.coveragerc",
    "tests",
    "-s"
]
```

## [isort](https://pycqa.github.io/isort/)
This tool organizes and sorts python imports as configured by `.isort.cfg`. Our configuration has a few changes from the default settings which make it easier to coexist with git.

To integrate it with VS Code, add the following to your `.vscode/settings.json` file in your project:

```json
"python.sortImports.path": "${workspaceFolder}/.venv/bin/isort",
"python.sortImports.args": [
    "--settings-path", "${workspaceFolder}/.ml-python-configs/.isort.cfg",
    "--project", "your_package",
    "--project", "tests",
    "--virtual-env", "${workspaceFolder}/.venv",
],
"[python]": {
    "editor.codeActionsOnSave": {
        "source.organizeImports": true
    }
}
```

Remember to switch `your_package` to the name of whatever your package is. This is important because otherwise `isort` assumes your code is a 3rd party dependency and will lump those imports together.

## [mypy](https://mypy.readthedocs.io/en/latest/introduction.html)
Mypy will perform static type checking on your code.

To integrate it with VS Code, add the following to your `.vscode/settings.json` file in your project:

```json
"python.linting.mypyEnabled": true,
"python.linting.mypyPath": "${workspaceFolder}/.venv/bin/mypy",
"python.linting.mypyArgs": [
    "--show-error-codes",
    "--config-file",
    "${workspaceFolder}/.ml-python-configs/.mypy.cfg",
],
```
