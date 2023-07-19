---
title: Flow
description: Understand the project flow and working of the modules.
---

## Diagram

``` mermaid
graph TD
of[veritas.py > openFile] --> lfcb[veritas.py > loadFileCallback];

lfcb[veritas.py > loadFileCallback] --> lf[loader.py > loadFile];
lf[loader.py > loadFile] --> aa[loader.py > isArtifactA];
aa[loader.py > isArtifactA] -->|No| ab[loader.py > isArtifactB];
aa[loader.py > isArtifactA] -->|Yes| as{Artifact Supported?};
ab[loader.py > isArtifactB] -->|Yes| as{Artifact Supported?};
ab[loader.py > isArtifactB] -->|No| others[...];

others[...] -->|No| urv[veritas.py > updateRecycleViews];
others[...] -->|Yes| as{Artifact Supported?};
as{Artifact Supported?} -->|No| urv[veritas.py > updateRecycleViews];
as{Artifact Supported?} -->|Yes| an[loader.py > isArtifiactN];
an[loader.py > isArtifiactN] -->|calls| ant[artifactN.py > artifactNTemplate];

ant[artifactN.py > artifactNTemplate] ---> |is populated by contributor| template([artifactNtemplate]);
ant[artifactN.py > artifactNTemplate] ---> |is populated by contributor| size([artifactNsizes]);
ant[artifactN.py > artifactNTemplate] ---> |is populated by contributor| markers([artifactNmarkers]);

template([artifactNtemplate]) --> |passed to| abs[offsetter.py > toAbsolute];
size([artifactNsizes]) --> |passed to| abs[offsetter.py > toAbsolute];
abs[offsetter.py > toAbsolute] --> |returns| tdata([templatedata]);
tdata([templatedata]) --> |returns| an[loader.py > isArtifiactN];
markers([artifactNmarkers]) --> |returns| an[loader.py > isArtifiactN];

ant[artifactN.py > artifactNTemplate] ----> |calls| rf[primer.py > readFile];
rf[primer.py > readFile] --> |returns| fhd([formattedhexdata]);
rf[primer.py > readFile] --> |returns| fad([formattedasciidata]);

fhd([formattedhexdata]) --> |returns| an[loader.py > isArtifiactN];
fad([formattedasciidata]) --> |returns| an[loader.py > isArtifiactN];

an[loader.py > isArtifiactN] --> od([offsetdata]);
an[loader.py > isArtifiactN] --> hd([hexdata]);
an[loader.py > isArtifiactN] --> ad([asciidata]);
an[loader.py > isArtifiactN] --> md([markerdata]);
od([offsetdata]) --> f([first]);
hd([hexdata]) --> f([first]);
ad([asciidata]) --> f([first]);
md([markerdata]) --> s([second]);
f([first]) --> urv[veritas.py > updateRecycleViews];
s([second]) --> urv[veritas.py > updateRecycleViews];
```

Way too dank? Let's break it down!

There are primarily 4 types of files in the project structure:

## Main driver modules

* [veritas.py](./MainDriverModules/veritas.py.md)
* [loader.py](./MainDriverModules/loader.py.md)

## Artifact specific modules

* [artifact.py](./ArtifactSpecificModules/artifact.py.md)

## Helper modules

* [primer.py](./HelperModules/primer.py.md)
* [offsetter.py](./HelperModules/offsetter.py.md)

## Kivy GUI related modules

* [veritas.kv](./KivyGUIRelatedModules/veritas.kv.md)
* [colors.py](./KivyGUIRelatedModules/colors.py.md)