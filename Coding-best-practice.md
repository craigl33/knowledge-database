\<- [Home](home)

## Code quality

To ensure everybody can read and understand the code, it is important to follow some guidelines. This includes the use of a linter and formatter, to enforce a consistent style and to catch errors early. General good overview in the topic is given [here](https://realpython.com/python-code-quality/).

**For the sake of your colleagues, please document all code, especially your functions and classes!**

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

## TOML files

The vast majority of RISE Python code uses TOML files for configuration. 

More information on the formatting and options can be found here: https://github.com/toml-lang/toml

I also suggest to use some sort of TOML plugin in VS Code (or PyCharm if you use it) that allows for syntax highlighting and autocomplete with Github Copilot. For this I suggest [**Even Better TOML **](https://marketplace.visualstudio.com/items?itemName=tamasfe.even-better-toml)**-** this really is a game changer for managing these configuration files (!!)

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

## Jupyter Notebook clean up

Git does not behave well with jupyter notebooks. There are different approaches (see [here](https://www.reviewnb.com/git-jupyter-notebook-ultimate-guide), but the easiest is to just clean up metadata before committing. If you still want to keep the output to show results and examples via Gitlab, you can use the `nbstripout` tool:

```bash
pip install nbstripout
nbstripout <file>.ipynb # To clean metadata and output
nbstripout <file>.ipynb --keep-output # To clean only metadata
```