# Main Driver Modules

This type of modules are the entry point or main driving code for the base of the project.

`veritas.py` handles basic IO functions such as loading a file when the user picks one and populating the Kivy `RecycleViews` with offset, hex, ascii and marker data.

Along with `loader.py` which is responsible for holding the functions which check the signature of an artifact for detection and load the appropriate template, it also handles a callback function to check for supported artifacts by the project.