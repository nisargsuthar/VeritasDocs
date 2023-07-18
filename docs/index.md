---
title: Home
description: A hex viewer for the sleuths!
---

<h1 align="center">
  <br>
  <a href="https://github.com/nisargsuthar/Veritas"><img src="https://raw.githubusercontent.com/nisargsuthar/Veritas/main/images/Veritas.png" width="25%"></a>
  <br>
  Veritas
  <br>
</h1>

<h3 align="center">Veraciously Effective Renderer In Typical Artifact Structures</h3>

<p align="center">
   <a href="LICENSE" alt="License">
   <img src="https://img.shields.io/github/license/nisargsuthar/Veritas?style=flat" /></a>
   <a href="https://github.com/nisargsuthar/Veritas/issues" alt="Issues">
   <img src="https://img.shields.io/github/issues/nisargsuthar/Veritas?style=flat" /></a>
   <a href="https://github.com/nisargsuthar/Veritas/graphs/contributors" alt="Contributors">
   <img src="https://img.shields.io/github/contributors/nisargsuthar/Veritas?style=flat" /></a>
   <a href="https://github.com/nisargsuthar/Veritas/pulls?q=is%3Apr+is%3Aclosed" alt="Closed PRs">
   <img src="https://img.shields.io/github/issues-pr-closed/nisargsuthar/Veritas?style=flat" /></a>
   <a href="https://github.com/nisargsuthar/Veritas/network/members/" alt="Forks">
   <img src="https://img.shields.io/github/forks/nisargsuthar/Veritas?style=flat" /></a>
   <a href="https://github.com/nisargsuthar/Veritas/stargazers/" alt="Stars">
   <img src="https://img.shields.io/github/stars/nisargsuthar/Veritas?style=flat" /></a>
   <a href="https://github.com/nisargsuthar/Veritas/watchers/" alt="Watchers">
   <img src="https://img.shields.io/github/watchers/nisargsuthar/Veritas?style=flat" /></a>
</p>

!!! abstract

    A hex viewer made for parsing and color coding artifact file structures for visualization using dynamic templates, to make validation process easier.

## About
Tired of always having to look up different versions of file structures & manually mapping the bytes while working with hex?

Veritas is an elementary **WIP** hex viewer made for forensicators, which automatically identifies different artifacts and applies the correct template. These templates are generated dynamically to accurately highlight data with appropriate color markers.

## Last Still
![StillOne.png](https://raw.githubusercontent.com/nisargsuthar/Veritas/main/images/StillOne.png?)
![StillTwo.png](https://raw.githubusercontent.com/nisargsuthar/Veritas/main/images/StillTwo.png?)
![StillThree.png](https://raw.githubusercontent.com/nisargsuthar/Veritas/main/images/StillThree.png?)

## Disclaimer
Veritas is not meant to be an advanced hex viewer with functionalities that a real hex editor might have. It **solely** aims for one thing, convenience for data validation. It is suggested that this hex viewer be used in accordance with other good hex editors that offer searching and goto functions. Over time, Veritas will support more file structures; but as of now I'm the only one working on this project when I'm able to.

## Features
* Dynamic artifact templates.
* Color coded artifact file structure with sub-sections.
* TODO: Multiple tabs.
* TODO: Finish popups.

## Supported Artifacts
* Prefetch: XP, Vista, 7, 8.1, 10, 11
* TODO: $MFT
* TODO: Registry files
* TODO: Images: png, jpeg, jpg, bmp, gif
* TODO: Documents: pdf

## Requirements
* Python >= 3.10.4
* Kivy >= 2.1.0

## Installation
**Step 1**: Create a virtual environment using:
```python
python3 -m venv veritas
```

**Step 2**: Depending on your OS, activate the virtual environment using:

* Windows: `.\veritas\Scripts\activate`
* Linux: `source veritas/Scripts/activate`

**Step 3**: Install kivy using:
```python
python3 -m pip install "kivy[base]"
```

## Special Thanks
* [Gary Kessler](https://www.linkedin.com/in/garykessler){target=_blank}'s [File Signatures Table](https://www.garykessler.net/library/file_sigs.html){target=_blank}, for the very handy cheatsheet for various file signatures.
* [Andrew Rathbun](https://twitter.com/bunsofwrath12){target=_blank}'s [DFIRArtifactMuseum](https://github.com/AndrewRathbun/DFIRArtifactMuseum){target=_blank}, for providing numerous artifact samples to validate proper parsing.
* [Forensics Wiki](https://forensicswiki.xyz/page/Main_Page){target=_blank}, for additional information on file structures.
* [Joachim Metz](https://github.com/joachimmetz){target=_blank}'s [Libscaa](https://github.com/libyal/libscca){target=_blank}, for the prefetch file structure documentation.
* [el3phanten](https://github.com/el3){target=_blank}, for invaluable assistance in kivy.
