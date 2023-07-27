# offsetter.py
Contains a function `toAbsolute()` which converts relative offset in template data to absolute addresses. This function must be called before returning the data from an artifact's module.

It consumes two lists:

* `template`: A list of lists, containing lists of ordered pairs (a, b) where `a` is the starting position and `b` is the size or the number of bytes for a particular template part. All lists of lists (which represent a new section in a template) must have the first list's `a` value set to 1 so `toAbsolute()` function can properly calculate absolute offsets for all the sections being templated.

* `sizes`: A list of matching corresponding sizes of the sections.