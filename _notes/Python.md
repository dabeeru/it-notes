---
title: Python
creation_date: 2023-01-22
tags:
	- IT
	- Programming
	- Python
---
# Modules
- `dir()` - show current namespace
- modules are only loaded on the first import in current interprete session
- modules can be also written in C
- there are native modules as part of interpereter
## Importing
- `sys.path` - modules path
	- can be extended with path modules from other project
- `importlib.reload(mod)` - reloading module
- you cannot just import package, but only
    - `from pkg.mod import name`
    - `from pkg import mod`
- `__init__.py` - can be used to load modules from package automatically
- `from pkg import *` - requires `__all__` list in `__init__.py` containing list of module
- `..pkg.mod` - import name - relative importtor
