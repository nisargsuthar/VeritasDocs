# artifact.py

Some guidelines to follow while writing a parser for Veritas:

An artifact's module (such as `prefetch.py`) must also return a list containing `artifactmarkers`.
This list contains all the textual data representing the sections and bytes within, for an artifact's file structure.

So, for above example there would be a need to append three strings corresponding to the same order of template and size data appended.
```python
artifactmarkers = []
artifactmarkers.append("\n+9 This is the first section!\n")
artifactmarkers.append("\n+10 This is the second section!\n")
artifactmarkers.append("\n+32 This is the third section!\n")
```
!!! note
    Since the sizes were static for this example, the `+n` byte values are hardcoded in the strings. Whenever there is a variable length section, `artifact.py` must first calculate the dynamic length correctly and then use wildcard formatting in python to generate the strings as shown below.
```python
artifactmarkers.append("\n+{} This is a section of variable size!\n".format(variablesizecalculatedbyseeklogic))   
```

As for the inputs and outputs, an `artifact.py` module should take only one parameter `file_path` to read the file. This must be done using the `readFile()` function present in `primer.py`. There is also a `readPartialFile()` function which is used to only read necessary bytes in `loader.py` to determine the magic numbers to determine the type of artifact loaded. This was done for performance optimization.

We already looked at the two lists returned from `artifact.py`, which were `templatedata` and `artifactmarkers`.

The other two lists are called `formattedhexdata` and `formattedasciidata` which are to be populated by `readFile()` function. It performs some under the hood formatting to make all the data as it should be in a typical hex view.

So, at the very beginning of an `artifact.py` module the following code should be a common thing to see:
```python
artifacttemplate = []
artifactsizes = []
artifactmarkers = []

formattedhexdata, formattedasciidata, hexdata = readFile(file_path)
```

And at the end when returning data, the following code:
```python
templatedata = toAbsolute(artifacttemplate, artifactsizes)

return formattedhexdata, formattedasciidata, templatedata, artifactmarkers
```