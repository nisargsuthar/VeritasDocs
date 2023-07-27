# primer.py

Contains helper functions for manipuating/formatting data as required.

## colorBytes
Responsible for injecting the Kivy markup color tags `[color=XXXXXX]` and `[/color]`.

## escapeMarkup
Responsible for escaping the characters `&`, `[` and `]` occuring in data for Kivy markup.

## listToString
Responsible for converting a list to a string.

## readPartialFile
Responsible for reading file loaded only upto the bytes required to seek out the magic numbers. This reduces unnecessary read operations and improves performance.

## readFile
Responsible for reading the file being loaded and formatting it at the same time. The only formatting being done here is inserting newline characters every 16th byte to achieve the traditional hex-ascii view.

## joinOffsetHexAscii
Responsible for joining offset data, hex data and ascii data and returning it as a dictionary as required by `RecycleViews`.

## getOffsets
Responsible for returning a list containing all the offsets depending on the 16 byte breaks.

## fixColorTags
Responsible for injecting extra Kivy markup color tags which were broken due to the newlines. Read the comments in `loader.py` module for more.