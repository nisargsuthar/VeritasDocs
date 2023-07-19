# Helper Modules

This type of modules facilitate tedious tasks via some generic and some specific helper functions. This makes the other important modules readable and hence more manageable.

`primer.py` contains two important functions for reading files, loaded by `loader.py` and `artifact.py` modules:

* `readPartialFile()`
> Reads the loaded file only upto the maximum bytes required for detecting the artifact signature.
* `readFile()`
> Reads the entire loaded file, and returns formatted data which will be further processed by `loader.py` and eventually be fed to the `RecycleViews`.