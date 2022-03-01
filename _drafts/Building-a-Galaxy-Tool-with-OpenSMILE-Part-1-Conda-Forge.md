---
id: 509
title: 'Building a Galaxy Tool with OpenSMILE: Part 1 Conda Forge'
date: 2018-01-12T21:09:38+00:00
author: Steve Cassidy
layout: post
---
This post documents my workflow for building a new [Galaxy](https://galaxyproject.org/) tool for speech analysis using the [OpenSMILE](http://audeering.com/technology/opensmile/) toolkit. I hope it will be useful and encourage others to develop tools for speech and language analysis in Galaxy.

Galaxy tools are simple wrappers around executable programs. A tool is defined by an XML file that includes the command line that must be called to run the tool, it also defines the inputs and outputs in a way that can be used by the system to build the user interface for the tool.  For some simple tools, the work can be done with a Python or R script, but in other cases, as in my example here, some other software package is needed to do the actual work.  This introduces a **dependency** in the tool &#8211; software that must be installed in the execution environment for the tool to run.  Galaxy handles dependencies in a number of ways but the most well developed is the use of the [conda](https://conda.io/docs/) package manager.

Conda is a packaging system for software that is widely used for scientific and research software packages.  It can manage packages for Python and R but also Java and any stand-alone executable software that you might want to build.  Conda is used by the popular [Anaconda](https://anaconda.org/) scientific software environment but it can also be used independently via the [minconda](https://conda.io/miniconda.html) package.    Anaconda maintains a large repository of packages but there are also discipline specific repositories like [BioConda](https://bioconda.github.io/) and other general purpose repositories like [conda-forge](https://conda-forge.org/).

If the package you want to use in your tool already exists in a conda repository, then you can move on to the next step and build your tool. However, in this case the tool I want to build uses OpenSMILE which has not yet been packaged for conda; so, my first step is to create a conda package and submit it to a repository.  I will use conda-forge since it is a general purpose repository with a really good workflow for package submission and maintenance.

## Submitting to Conda Forge

To submit the package to conda forge I need to write a recipe to build the software (there&#8217;s help on the conda package build process [here](https://conda.io/docs/user-guide/tasks/build-packages/index.html)).  Conda-forge provides the [staged-recipes](https://github.com/conda-forge/staged-recipes) repository on Github for this purpose.  I create a fork of this repository, check out a local copy and add a (git) branch for my new work.   I then copy the example recipe folder, renaming it to **opensmile** to hold my recipe.

The core of the recipe is [the **meta.yml** file](https://conda.io/docs/user-guide/tasks/build-packages/define-metadata.html). It contains package metadata and details of dependencies; it also has build instructions.   If the software you are packaging is a Python module available on PyPi, then this step is very easy as you will just modify the example recipe and change the names and details to match your package.   In this case I&#8217;m building a C++ package so it requires more customisation.

The first part is to name the package and identify where it can be downloaded.   Conda prefers to build from source rather than downloading binary packages to ensure binary compatibility between packages.  So I need to identify where to get the source. Here&#8217;s the first lines of **meta.yml**:

<pre>{% set name = "opensmile" %}
{% set version = "2.3.0" %}
{% set sha256 = "41adf2886089b76c000b74a479ac32ed983fc09bd81d1e9df4b0f4999ee71c7f" %}

package:
  name: {{ name|lower }}
  version: {{ version }}

source:
  fn: {{ name }}-{{ version }}.tar.gz
  url: http://audeering.com/download/1318/
  sha256: {{ sha256 }}</pre>

This file is processed for Jinja templates before being used, so we can use template constructs to insert variable values like {{version}} into the text.   These lines just define the package name and where the tar file can be downloaded. The next part defines how the package should be built.  In some cases the best option is to write a build.sh file (build.bat for windows) and have that do the work. Luckily for OpenSMILE there is already a build script in the distribution so we can just reference that:

<pre>build:
  number: 0
  skip: True  # [win]
  script: sh buildStandalone.sh -p {{PREFIX}}</pre>

Here I&#8217;m skipping the Windows build as I&#8217;m only really interested in a Linux version.  The build number gets incremented if we make a new version of the same package, but can stay at 0 in most cases.  The {{PREFIX}} variable is pre-defined to point to the location that Conda will look for the resulting binaries.

The next section refers to the software needed to build and possibly to run the package.  In this case we need a few packages to build OpenSMILE, namely the OpenCV libraries and the GNU compiler toolchain.   I specify this as follows:

<pre>requirements:
  build:
    - opencv
    - toolchain
    - autoconf
    - automake
    - libtool</pre>

There can also be a **run** and a **test** section here for packages that are needed at run time and for testing.  These names refer to other Conda packages, preferably ones that are available in conda-forge.   You might need to search to find the appropriate names.  The **toolchain** dependancy is special in that it defines the Conda toolchain needed to build software from source.

Next I have a test section. This defines a simple test that the build works. It doesn&#8217;t need to be a full unit-test of the software, in fact that is discouraged, just something that can verify that the build was successful.  In my case I just run **SMILExtract** with the -h flag to check that the executable works:

<pre>test:
  commands:
    - SMILExtract -h  # [unix]</pre>

Finally there is some metadata about the package.  This includes a reference to the license that the software is distributed under and should include the name of the license file within the source code.  In this case OpenSMILE has it&#8217;s own custom licence which is in the file JULIUS_LICENSE in the sources.

<pre>about:
  home: http://audeering.com/technology/opensmile/
  license: OpenSMILE
  license_family: OTHER
  license_file: JULIUS_LICENSE
  summary: Munich Versatile and Fast Open-Source Audio Feature Extractor

extra:
  recipe-maintainers:
    - stevecassidy</pre>

Now that this is complete we could test the build.  The best way is to use docker so that the build is done in a standardised environment.   A script is provided for this purpose in the staged-recipes project **.circleci/run\_docker\_build.sh. **To run this you need docker installed, the script will pull down the appropriate image (takes a while first time around) and then run the build exactly as it will be run for the production build.  You&#8217;ll see any error messages that are produced and can work on fixing them.

When I went through this the main issues were with getting all of the dependencies listed (eg. autoconf, automake) and getting the build command like correct (including the PREFIX flag).  After a few tries I had a successful build that left a conda package in the directory **build_artefacts/linux64** called **opensmile-2.3.0-0.tar.bz2.**

This is just the start of the journey! The next step is to commit the changes with Git to my new branch and push these changes to Github.   Once they are safely on Github I go to the web page for my repository and find a button inviting me to open a [Pull Request](https://help.github.com/articles/about-pull-requests/). This is a way for me to notify the original project that I have new work that they might be interested in.  In this case the pull request starts the process of submission of my recipe to Conda-Forge.  I [open a pull request](https://github.com/conda-forge/staged-recipes/pull/4779), add a note about what I&#8217;m doing and submit it.   This starts of the auto build process that will eventually result in my package being part of Conda Forge.

Sending the pull request triggers a [Continuous Integration](https://en.wikipedia.org/wiki/Continuous_integration) process that will first check my recipe for neatness (linting) and then try to build on Linux, Mac OS and Windows.   If the build works, we move on to the next stage which is a check by one of the conda-forge team.

The first process to run is the linter, this checks that your recipe is nicely written and has all the required parts.  In my case I was warned about bad spacing of comments and then this problem with the license description (I had tried removing the license line because openSMILE doesn&#8217;t use one of the standard licenses:

![Conda Forge]({{"/wp-content/uploads/2018/01/condforge-screenshot-lint.png"|relative_url}})

If you get a message like this on your pull request you need to revise your project (fix the problem), commit and push again to your repository. The pull request will then see the updated version of your code and re-start the CI process.

The messages from the linter are useful but I got a bit frustrated since I didn&#8217;t know what rules it was enforcing (some seem arbitrary such as not having the word &#8216;license&#8217; in the name of the license).  So I went and found the [source that is doing the checking](https://github.com/conda-forge/conda-smithy/blob/master/conda_smithy/lint_recipe.py) so that I could see what it was looking for.

Once the linting is done it moves on to running the build on all three platforms.  I&#8217;ve asked it to skip Windows so it will just build on Linux and mac OS.

In this case I hit a wall here. My build failed even though it had worked locally. The error message I got was confusing and I couldn&#8217;t see how to address it.  The only way forward here is to ask for help, which you can do on the [Conda-Forge Gitter channel](https://gitter.im/conda-forge/conda-forge.github.io) where many of the developers hang out and are very helpful.  I got the attention of one expert who was able to help me move forward (I hadn&#8217;t merged with a recent version of the staged-recipes repository so was out of date with the current build system).
