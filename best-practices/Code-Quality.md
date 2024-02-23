<- [Home](home)

To ensure everybody can read and understand the code, it is important to follow some guidelines. This includes the use of a linter and formatter, to enforce a consistent style and to catch errors early.

## Linter & Formatter
A linter is a tool that analyzes code and flags programming errors, bugs, stylistic errors, and suspicious constructs.

A code formatter is a tool that automatically formats code according to a style guide.

[Ruff](https://github.com/astral-sh/ruff) is a linter and formatter for Python and should be used for all code which is pushed to this repository.

Full documentation here [Ruff Documentation](https://docs.astral.sh/ruff/).

### Setup
You can use a ruff as an IDE plugin or in the command line.

#### IDE Plugin
You can install Ruff in most IDEs:
- [PyCharm](https://plugins.jetbrains.com/plugin/20574-ruff)
- [VS Code](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff)

The linter will then automatically check your code and suggest changes via the IDE's interface. It is also possible to automatically format the code on save.

#### Command line
You can also use Ruff in the command line. To install it, run:
```bash
pip install ruff
```

### Usage
Here the terminal commands are explained, the IDE plugin is basically the same.

When installed you can run Ruff with:
```bash
ruff
ruff <file> # to check a specific file
```
This will check all files in the current directory and its subdirectories.

To fix all issues, which can be fixed automatically, run:
```bash
ruff --fix
```
This will change your code to match the style guide. It will never break the code. All issues which ruff cannot fix will be listed as warnings.

To format your code run the following command:
```bash
ruff format
ruff format <file> # to format a specific file
```

It is also possible to run it on jupyter notebooks. But those have to be explicitly enabled with the `--jupyter` flag. 


### Ruff settings
Ruff can be configured on which rules to enforce and which not. This is done via the `pyproject.toml` file. All repositories already include a section for that. You can change it if you want to enforce different rules.
