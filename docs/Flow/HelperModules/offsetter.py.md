# offsetter.py
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
