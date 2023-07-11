---
title: Flow
description: Understand the project flow and working of the modules.
---

``` mermaid
graph TD
of[veritas.py > openFile] --> lfcb[veritas.py > loadFileCallback];

lfcb[veritas.py > loadFileCallback] --> lf[loader.py > loadFile];
lf[loader.py > loadFile] --> aa[loader.py > isArtifactA];
aa[loader.py > isArtifactA] -->|No| ab[loader.py > isArtifactB];
ab[loader.py > isArtifactB] -->|No| others[...];
aa[loader.py > isArtifactA] -->|Yes| as{Artifact Supported?};
ab[loader.py > isArtifactB] -->|Yes| as{Artifact Supported?};

others[...] -->|No| urv[veritas.py > updateRecycleViews];
others[...] -->|Yes| as{Artifact Supported?};
as{Artifact Supported?} -->|No| urv[veritas.py > updateRecycleViews];
as{Artifact Supported?} -->|Yes| can[loader.py > callArtifiactN];
can[loader.py > callArtifiactN] -->|calls| ant[artifactN.py > artifactNTemplate];

ant[artifactN.py > artifactNTemplate] --> |declares| template([artifactNtemplate]);
ant[artifactN.py > artifactNTemplate] --> |declares| size([artifactNsize]);
ant[artifactN.py > artifactNTemplate] --> |declares| markers([artifactNmarkers]);

template([artifactNtemplate]) --> |is populated by| abs[offsetter.py > toAbsolute];
size([artifactNsize]) --> |is populated by| abs[offsetter.py > toAbsolute];
abs[offsetter.py > toAbsolute] --> |returns| tdata([templatedata]);
tdata([templatedata]) -->  can[loader.py > callArtifiactN];
markers([artifactNmarkers]) --> can[loader.py > callArtifiactN];

ant[artifactN.py > artifactNTemplate] --> |calls| rf[primer.py > readFile];
rf[primer.py > readFile] --> |returns| fhd([formattedhexdata]);
rf[primer.py > readFile] --> |returns| fad([formattedasciidata]);

fhd([formattedhexdata]) --> can[loader.py > callArtifiactN];
fad([formattedasciidata]) --> can[loader.py > callArtifiactN];

can[loader.py > callArtifiactN] --> hd([hexdata]);
can[loader.py > callArtifiactN] --> ad([asciidata]);
can[loader.py > callArtifiactN] --> md([markerdata]);
hd([hexdata]) --> f([first]);
ad([asciidata]) --> f([first]);
md([markerdata]) --> s([second]);
f([first]) --> urv[veritas.py > updateRecycleViews];
s([second]) --> urv[veritas.py > updateRecycleViews];
```

## Way too dank? Let's break it down!

Types of files in the project structure:

* Main driver modules (veritas.py, loader.py)
* Helper modules (primer.py, offsetter.py)
* Artifact specific modules (prefetch.py)
* Kivy GUI related modules (veritas.kv, colors.py)

## veritas.py
The main file of the project which acts as an entry point. It works with veritas.kv file to load GUI components.

When a file is chosen by the user, the `loadFile()` function is called and a callback function is passed with it.

The callback function is named `updateRecyleViews()` which is responsible for updating the UI to reflect the data binded by RecycleViews' controllers.

There are two RecycleViews:

* Common for hex values and ascii values so they share common scrolling.
* Marker values, which specify what do the bytes represent according to file format specification or research.

Whether to load the file or not in the view is decided by the `loadFile()` function present in `loader.py`. This allows the Veritas to not load unsupported artifacts.

## veritas.kv
Kivy layout file, solely for GUI. Nothing much to explain here.

## loader.py
This module contains all the artifact related functions which are responsible for checking the magic numbers in order to detect the artifact file structure, and proceed with appropriate artifact module to load the correct template.
For each supported artifact, two functions must be defined:

* isArtifact()
> Check magic numbers at required offsets in the file structure and return a boolean value.
* callArtifact()
> Make a call to the matched artifact template function present in the specific artifact's module, and set the return values as local variables hexdata, asciidata, markerdata and templatedata.

!!! note
    Check the `artifact.py` documentation below for more information.

## offsetter.py
Contains a function `toAbsolute()` which converts relative offset in template data to absolute addresses. This function must be called before returning the data from an artifact's module.

It consumes two lists:

* `listofpairlist`: A list of lists, containing lists of ordered pairs (a, b) where `a` is the starting offset and `b` is the size or the number of bytes for a particular template part. All lists of lists must have the first list's `a` value set to 1 so `toAbsolute()` function can properly calculate absolute offsets for all the sections being templated.

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

* `listofsizes`: A list of matching corresponding sizes of the sections.
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

Both of the above lists are passed to the `toAbsolute()` function which returns a list containing the template data.
```python
templatedata = toAbsolute(artifacttemplate, artifactsizes)
```

!!! tip
    Please handle all possible versions/formats for a particular artifact to avoid bugs and errors.
    Test and validate your parser on a multivariate dataset of the artifact before creating a PR.
    This helps save time and resources preventing unnecessary debugging.

## artifact.py

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

As for the inputs and outputs, an `artifact.py` module should take only one parameter `file_path` to read the file. This must be done using the `readFile()` function present in `primer.py`. There is also a `readPartialFile()` function which is used to only read necessary bytes to determine the magic numbers to determine the type of artifact loaded. This was done for performance optimization.

For each submitted `artifact.py` module, there must be two functions defined in `loader.py`:

* isArtifact()
> Must use the `readPartialFile()` to read the artifact loaded only upto the number of bytes necessary to determine the artifact type. `readPartialFile()` takes two parameters `file_path` and `numberofbytestoread`.
* callArtifact()
> This function is solely used to call the `artifactTemplate()` function from `artifact.py` module and store the returned data in four lists which are also module-level global variables namely `hexdata`, `asciidata`, `templatedata` & `markerdata` in that order.

We already looked at the two lists returned from `artifact.py`, which were `templatedata` and `artifactmarkers`.

The other two lists are called `formattedhexdata` and `formattedasciidata` which are to be populated by `readFile()` function. It performs some under the hood formatting to make all the data as it should be in a typical hex view.

So, at the very beginning of an `artifact.py` module the following code should be a common thing to see:
```python
artifacttemplate = []
artifactsizes = []
artifactmarkers = []

getdata = readFile(file_path)
formattedhexdata = getdata[0]
formattedasciidata = getdata[1]
hexdata = getdata[2]
```

And at the end when returning data, the following code:
```python
templatedata = toAbsolute(artifacttemplate, artifactsizes)

return formattedhexdata, formattedasciidata, templatedata, artifactmarkers
```