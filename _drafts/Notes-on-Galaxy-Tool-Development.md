---
id: 455
title: Notes on Galaxy Tool Development
date: 2016-08-10T17:10:18+00:00
author: Steve Cassidy
excerpt: |
layout: post
guid: https://stevecassidy.net/?p=455
permalink: /?p=455
categories:
  - Uncategorized
---
Galaxy tools are executable scripts or applications that process input data and produce some kind of output file.  These are some notes on what I’ve learned about Galaxy tool development as I’ve started to work on tools to handle speech and language data for the Alveo project.  The intended audience is other developers of tools for Alveo, but they may be more generally useful.

A Galaxy tool has two parts &#8211; the executable program or script and an XML file describing how it can be called.  If you are creating a tool around an existing program or script, the main task is writing the XML file and deciding what input and output options your should have.  If you are writing a tool around a Python or R library, you also need to write the script that accepts the input options and files and writes out the results.

The tools I&#8217;ve been developing for Alveo are on github: [alveo-galaxy-tools](https://github.com/stevecassidy/alveo-galaxy-tools).  If you want to develop tools for Alveo, feel free to use this as a starting point, add your tools and send a pull request. Alternately, work in your own project but copy the structure from mine.

## Development Environment

Galaxy is a reasonably complex Python based web application and is very configurable, however the configuration is a bit complicated and not really what you want to be doing if you just want to develop a new tool. Luckily we have the [planemo](https://planemo.readthedocs.io/en/latest/) script to help with the development process &#8211; it takes care of running a Galaxy server so that you can test your tool on your local machine.  The planemo documentation has some good instructions on how to go about the tool development process.  I have read through that and refer back to it frequently as I try to work out some new wrinkle in the process.  I won&#8217;t try to replace that discussion, instead I&#8217;ll just document the development environment I&#8217;ve put together.

Firstly, I make use of [virtualenv](https://virtualenv.pypa.io/en/stable/) (and [virtualenvwrapper](https://virtualenvwrapper.readthedocs.io/en/latest/)) for Python development, it allows me to have different Python modules installed for different projects.   Since planemo is a python module you can install it in a virtualenv.  Create a new virtualenv for galaxy development:

<pre>% mkvirtualenv galaxy
% workon galaxy
% pip install planemo</pre>

(the workon command activates the galaxy virtualenv) Note that you could also install via brew or conda as per [the planemo documentation](https://planemo.readthedocs.io/en/latest/installation.html).

&nbsp;