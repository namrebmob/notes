# Flask Blueprints

## Bulk Register Blueprints

<https://github.com/pycook/cmdb>

```py app.py
def register_blueprints(app):
    from flask import Blueprint

    for item in getmembers(api.views):
        if item[0].startswith("blueprint") and isinstance(item[1], Blueprint):
            app.register_blueprint(item[1])
```

<https://github.com/hustlzp/Flask-Boost>

```py app/__init__.py
def register_blueprints(app):
    from . import controllers
    from flask.blueprints import Blueprint

    for module in _import_submodules_from_package(controllers):
        bp = getattr(module, 'bp')
        if bp and isinstance(bp, Blueprint):
            app.register_blueprint(bp)
```

```py
import importlib
import pkgutil

package=yourPackageName

for importer, modname, ispkg in pkgutil.walk_packages(path=package.__path__, prefix=package.__name__+'.', onerror=lambda x: None):
    try:
        modulesource = importlib.import_module(modname)
        reload(modulesource)
        print("reloaded: {}".format(modname))
    except Exception as e:
        print('Could not load {} {}'.format(modname, e))
```

```py
import pkgutil
import importlib

def load_blueprints(package, search_path: list = None) -> None:
    for name in pkgutil.iter_modules(path=search_path):
        module = importlib.import_module("%s.%s" % (package, name))
        print(dir(module))
```

```py
import pkgutil

def load_blueprints(search_path: list = None):
    # Используйте None, чтобы увидеть все модули, импортируемые из sys.path
    all_modules = [x[1] for x in pkgutil.iter_modules(path=search_path)]
    print(all_modules)
```
