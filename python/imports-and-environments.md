```markdown
# Python Imports, Conda, Virtual Environments — How It Really Works

## Key Rule (Everything Else Follows)

> Python imports modules by scanning `sys.path` **in order**.  
> The **first match wins**.

Being inside a conda or virtual environment does **not** automatically
guarantee isolation or priority.

---

## What Is `sys.path`?

`sys.path` is an ordered list of directories that Python searches when you run:

```python
import transformers

## Typical Import Order, when you perform import X (any package X)

0. Current working directory
1. User site-packages (~/.local/lib/pythonX.Y/site-packages)
2. Conda / virtual environment site-packages
3. System site-packages (/usr/lib/...)
⚠️ User site-packages often come before the environment.

## Why Environments Are Not Fully Isolated

Activating an environment:

changes the Python executable

adds env site-packages to sys.path

But does NOT remove user site-packages.

Result:
user site  →  env site  →  system site

So a package installed with: pip install --user X
will override: conda packages, and virtualenv packages

## How to Check Where a Package Is Imported From

import transformers
print(transformers.__file__)

This is the single most reliable debugging step.

## How to Achieve True Isolation
Disable user site-packages: export PYTHONNOUSERSITE=1

Now:
~/.local/... is ignored
only env + system remain
