# SC2 Development Environment Setup, and more

This repository contains the [mkdocs](https://github.com/mkdocs/mkdocs/) source for a guide on how to set up yourself for success while developing projects for the SC2 engine.

Initially written by `folk`.

Copyright belongs to the respective authors of each commit. All raw code (`SC2Layout`, `XML`, `galaxy`, and anything else that might be considered a programming language) snippets and art (JPEG, PNG, movies, sounds, entire `.SC2Map` files, and anything else that would be considered such by a Reasonable person) assets (unless owned by Blizzard) from all contributing authors are forcefully relicensed under the [UNLICENSE](http://unlicense.org/) upon being added of their free will to this source repository.

Contributions by `folk` are as restricted as possible. All Copyright is retained by `folk`. No license is granted to do anything with them, excepting those required to perform the functions outlined in the initial revision of this `README.md` document.

Any author publishing to this repository needs to add their own paragraph that outlines how their contributions may be used.

Please take a look at docs/index.md for more information, as that file is compiled and part of the `mkdocs` output.

# Setup

The documentation must be "compiled" as such, and this occurs locally. I have no knowledge of how to do any of this on Windows or Mac. Here are instructions for a pip-enabled Linux.

1. `pip3 install --user mkdocs mkdocs-material pygments`
1. `git clone repo-url`
1. `cd repo-url`
1. `mkdocs build --clean`

At this point, a new folder will appear, called `site`. In this folder, `index.html` contains the entry point to the generated documentation.

In order to perform work on the documentation of any kind, you may wish to simply run `mkdocs serve`, and point your browser to the URL that it outputs (probably `http://127.0.0.1:8000`). After that, any change made to files in the `docs` folder will (provided you've enabled JavaScript for local sites, which you should normally have disabled) reload the page automatically.

# Publishing

This repository may be published automatically or manually, to `https://sc2mapster.github.io/` and any URL below it.
Currently, the way to do this is to `mkdocs build --clean` and then simply add the output of this command manually to the git repository at the correct URL.
