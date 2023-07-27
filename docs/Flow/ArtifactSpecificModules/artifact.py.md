# artifact.py

Some guidelines to follow while writing a parser for Veritas:

An artifact's module (such as `prefetch.py`) must return following lists:

* A list containing formatted hex data, `formattedhexdata`. (Returned by the `readFile()` function)
* A list containing formatted ascii data, `formattedasciidata`. (Returned by the `readFile()` function)
* A list containing the template data, `templatedata`.
* A list containing the marker data, `artifactmarkers`.

The first two lists are called `formattedhexdata` and `formattedasciidata` which are to be populated using the `readFile()` function. It performs some under the hood formatting to make all the data as it should be in a typical hex view.

## formattedhexdata
`artifact.py` should return it as returned from `readFile()`.

## formattedasciidata
`artifact.py` should return it as returned from `readFile()`.

## templatedata

This list is generated using `toAbsolute()` function from the `offsetter.py` module.

It consumes two lists which are to be built by the corresponding module author:

* `artifacttemplate`: 

For example, there are only three sections to template in a particular artifact assuming they are all continuous in the file structure. Then the templatedata would be generated in the following manner:
```python
artifacttemplate = []
sectionone = [[1, 4], [5, 3], [8, 2]] # 3 sub-sections within section one of sizes 4, 3 and 2 bytes respectively.
sectiontwo = [[1, 8], [9, 2]]         # 2 sub-sections within section two of sizes 8 and 2 bytes respectively.
sectionthree = [[1, 16], [17, 8], [25, 8]] # 3 sub-sections within section one of sizes 16, 8 and 8 bytes respectively.
# Upto however many sections in a file format specification.
artifacttemplate.append(sectionone) # [[[1, 4], [5, 3], [8, 2]]]
artifacttemplate.append(sectiontwo) # [[[1, 4], [5, 3], [8, 2]], [[1, 8], [9, 2]]]
artifacettemplate.append(sectionthree) # [[[1, 4], [5, 3], [8, 2]], [[1, 8], [9, 2]], [[1, 16], [17, 8], [25, 8]]]
```

* `artifactsizes`: 
```python
artifactsizes = []
sectiononesize = 9 # 4 + 3 + 2
sectiontwosize = 10 # 8 + 2
sectionthreesize = 32 # 16 + 8 + 8
# Upto however many sections in a file format specification.
artifactsizes.append(sectiononesize) # [9]
artifactsizes.append(sectiontwosize) # [9, 10]
artifactsizes.append(sectionthreesize) # [9, 10, 32]
```
!!! note
    Apply conditional logic for a section's presence/absence anywhere necessary to yield dynamic templates for flexibility.
    Refer to prefetch.py for the `hashstring` section's conditional existence.

Before finally returning all the lists, both of the above lists are passed to the `toAbsolute()` function present in `offsetter.py` which returns a list containing the absolute template data.
```python
templatedata = toAbsolute(artifacttemplate, artifactsizes)
```

!!! tip
    Please handle all possible versions/formats for a particular artifact to avoid bugs and errors.
    Test and validate your parser on a multivariate dataset of the artifact before creating a PR.
    This helps save time and resources preventing unnecessary debugging.


## artifactmarkers

This list contains all the textual data representing the sections and bytes within, for an artifact's file structure.

So, for above example there would be a need to append three strings corresponding to the same order of template and size data appended.
```python
artifactmarkers = []
artifactmarkers.append("\n+9 This is the first section!\n")
artifactmarkers.append("\n+10 This is the second section!\n")
artifactmarkers.append("\n+32 This is the third section!\n")
```
!!! note
    Since the sizes were static for this example, the `+n` byte values are hardcoded in the strings. Whenever there is a variable length section, `artifact.py` must first calculate the dynamic length correctly and then use format strings in python to generate the strings as shown below.
```python
artifactmarkers.append(f"\n+{variablesizecalculatedbyseeklogic} This is a section of variable size!\n")   
```

As for the inputs, an `artifact.py` module should take only one parameter `file_path` to read the file. This must be done using the `readFile()` function present in `primer.py`. There is also a `readPartialFile()` function which is used to only read necessary bytes in `loader.py` to determine the magic numbers to determine the type of artifact loaded. This was done for performance optimization.

All in all, at the very beginning of an `artifact.py` module the following code should be a common thing to see:
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