# Import

## Get modules in a module

```py
import os

fileslist = os.listdir("sources")

for item in fileslist:
    if item.endswith("py") and item != "__init__.py":
        exec ("from sources.%s import *" %(item.split('.')[0]))
```

## List all the modules that are part of a python package

```py
import pkgutil

# this is the package we are inspecting -- for example 'email' from stdlib
import email

package = email

for importer, modname, ispkg in pkgutil.iter_modules(package.__path__):
    print "Found submodule %s (is a package: %s)" % (modname, ispkg)
```

How to import them too? You can just use __import__ as normal:

```py
import pkgutil

# this is the package we are inspecting -- for example 'email' from stdlib
import email

package = email
prefix = package.__name__ + "."

for importer, modname, ispkg in pkgutil.iter_modules(package.__path__, prefix):
    print "Found submodule %s (is a package: %s)" % (modname, ispkg)
    module = __import__(modname, fromlist="dummy")
    print "Imported", module
```

The right tool for this job is `pkgutil.walk_packages`.

To list all the modules on your system:

```py
import pkgutil

for importer, modname, ispkg in pkgutil.walk_packages(path=None, onerror=lambda x: None):
    print(modname)
```

Be aware that `walk_packages` imports all subpackages, but not submodules.

If you wish to list all submodules of a certain package then you can use something like this:

```py
import pkgutil
import scipy

package=scipy

for importer, modname, ispkg in pkgutil.walk_packages(path=package.__path__, prefix=package.__name__+'.',onerror=lambda x: None):
    print(modname)
```

`iter_modules` only lists the modules which are one-level deep. `walk_packages` gets all the submodules. In the case of scipy, for example, `walk_packages` returns:

```py
scipy.stats.stats
```

while iter_modules only returns:

```py
scipy.stats
```

[documentation on pkgutil](http://docs.python.org/library/pkgutil.html).

import packages only from current workdir

```py
# x is ModuleInfo(module_finder, name: str, ispkg: bool)
[x[1] for x in pkgutil.iter_modules(path=["app"]) if x[2]]
```
