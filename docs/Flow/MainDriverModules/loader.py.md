# loader.py
This module contains all the artifact related functions which are responsible for checking the magic numbers in order to detect the artifact file structure, and proceed with appropriate artifact module to load the correct template.

For each submitted `artifact.py` module, there must be a function defined in `loader.py`:

* isArtifact()
> Check magic numbers at required offsets in the file structure and return a boolean value.
> After invoking this function from the `else-if` ladder already in place, make a call to the matched artifact template function present in the specific artifact's module, and set the return values as local variables hexdata, asciidata, markerdata and templatedata.

It consumes one parameter `hexdata` which is defined in `loader.py` initialized using `readPartialFile()`. `readPartialFile()` takes two parameters `file_path` and `numberofbytestoread`.

Must pass `hexdata` to read the artifact loaded only upto the number of bytes necessary to determine the artifact type.

This function is used to return a boolean value based on checking the magic bytes for an artifact. It must be called from the `else-if` ladder in the `loadFile()` function which is solely used to call the `artifactTemplate()` function from `artifact.py` module and store the returned data in four lists namely `hexdata`, `asciidata`, `templatedata` & `markerdata` in that order.

Chain your artifact block in an `elif` statement and set the variable `artifactsupported` to true.

```python
elif isArtifact(hexdata):
		artifactsupported = True
		hexdata, asciidata, templatedata, markerdata = artifactTemplate(file_path)
```

!!! note
    The code maintains a variable `maxmagicseekamongsupportedartifacts` which represents the maximum value to seek for reading a file partially, among all supported artifacts. If your particular artifact's magic number is located at an offset range which exceeds the current value of this variable, update it.
    
    For example, if the artifact's magic number is located at decimal offset 12 and its length is 4 bytes, then the range of the magic bytes would be 12-16. 16 being the largest, it becomes the new value for `maxmagicseekamongsupportedartifacts`.
