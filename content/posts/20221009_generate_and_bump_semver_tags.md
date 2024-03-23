+++
title = "Generate and Bump SemVer Tag"
date = 2022-10-09T00:00:00+07:00
description = ""
type = "post"
showTableOfContents = "true"
image = "https://miro.medium.com/v2/resize:fit:720/format:webp/0*X9VShoYk9M29hW42"
+++

![](https://miro.medium.com/v2/resize:fit:4800/format:webp/0*X9VShoYk9M29hW42)

As part of programmer’s job, whether it’s software development, web development, or other programming jobs, we will most likely deal with versioning. Versioning helps the user to identify the progress we’ve made on the program.

But what’s more important than a simple versioning, is a versioning that is intended by other programmers to use. This kind of versioning (I would like to call it “libraries’ versioning”) is important to determine whether there is a breaking change or not in the libraries or frameworks that we use. Therefore, it is important to have a standardized notation for the libraries’ versioning, so that we know if and when we should upgrade the version.

# A Brief Introduction to SemVer

One versioning standard that is quite popular (the most popular, I can say?) is the [Semantic Versioning](https://semver.org/) (often abbreviated as SemVer). SemVer is a standard for managing a software’s version, whether it’s for end users or it’s a library for other programmers to use.

Basically, SemVer format has a few parts:
1) Major: this part is to tell the users whether a change big enough is introduced, so that the usage changes. It could be breaking API contracts or major UI/UX change.
2) Minor: this part is to tell whether there is a new functionality being added, without changing the original flow. For example, adding API or adding feature into an application.
3) Patch: this part is to tell the user if there’s a bug being fix. Usually, programmers will upgrade the patch without changing anything in their programs.
4) Prerelease: this part is to tell the programmers about a program that is yet to be “released” (i.e. it still contain bugs and are not stable enough). You can try the new pre-released version, but do it at your own risk. Usually, this is where the term “alpha” and “beta” is inserted.
5) Build: this part is for a routine / nightly build. If you’re using this part, most likely it will be less stable in the pre-release version, since there are many bugs that are yet to be solved.

You can check the whole format at semver.org. The website even includes regex notations, so those who want to validate the tag using regex can directly use theirs.

One caveat of SemVer is that there is no standard guide on how to decide if a change is breaking or not. Or whether a change should be treated as patch or minor. It all comes to the library or app developer’s job to decide.

# The Program

In order to ease us in generating and bumping the SemVer tag, I made a simple command line interface in Python to generate and bump the tag. The tag then later can be used in either Github, Gitlab, or other version control platforms.

## Prerequisites

1) Git (to clone the code, although you can download to ZIP as well).
2) Python 3.x (in my local, I use Python 3.10).
3) PIP for Python 3 (`pip3`).

## Use the Program

1) Clone [this repository](https://github.com/iamdejan/tag-generator) and go to the cloned repo.
2) Follow the instructions. You can run `python3 main.py -h` to see the list of commands available.

Here are some demonstrations that I have done (taken from the repository):

![](https://miro.medium.com/v2/resize:fit:4800/format:webp/0*InAgo2D2M-d7HVjd.png)

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*sbstpmggu23g04ya.png)

![](https://miro.medium.com/v2/resize:fit:720/format:webp/0*AeObW-aato7hKYJ9.png)

# References

- [A Simple Guide To Command Line Arguments With ArgParse](https://towardsdatascience.com/a-simple-guide-to-command-line-arguments-with-argparse-6824c30ab1c3)
- [How to Build Command Line Interfaces in Python With argparse](https://realpython.com/command-line-interfaces-python-argparse/)
