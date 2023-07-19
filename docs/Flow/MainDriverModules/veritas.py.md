# veritas.py
The main file of the project which acts as an entry point. It works with `veritas.kv` file to load GUI components.

When a file is chosen by the user, the `loadFile()` function is called and a callback function is passed with it.

The callback function is named `updateRecyleViews()` which is responsible for updating the UI to reflect the data binded by RecycleViews' controllers.

There are two RecycleViews:

* Common for hex values and ascii values so they share common scrolling.
* Marker values, which specify what do the bytes represent according to file format specification or research.

Whether to load the file or not in the view is decided by the `loadFile()` function present in `loader.py`. This allows the Veritas to not load unsupported artifacts.