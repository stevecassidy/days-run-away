---
id: 484
title: Docker and MAUS
date: 2016-10-20T05:48:55+00:00
author: Steve Cassidy
layout: single
guid: http://web.science.mq.edu.au/~cassidy/?p=484
categories:
  - Workflows
---
Today's problem was to write a wrapper for the MAUS automatic segmentation system in preparation for including it as a Galaxy tool.   MAUS comes from [Florian Schiel](https://www.phonetik.uni-muenchen.de/institut/mitarbeiter/schiel/Schiel.html) in Munich and is a collection of shell scripts and Linux executables that take a sound file and an orthographic transcription and generate an aligned phonetic segmentation.  The core of this process is the HTK speech recognition system and getting it to work on anything other than Linux is a pain that is best not lived with. <!--more-->

Galaxy runs on a server and so the executables would be ok in the production environment, but to do development I need to be able to run things locally.   The solution is to build a Docker container based on a base linux container (debian) with just the minimal set of tools installed to allow MAUS to run.  This turned out to be very simple. The only pre-requisite that I needed to add was sox (for sound file manipulation); I had to make sure that the container was able to run the 32 bit binaries that are included in the MAUS distribution and it all worked ok.

I'm getting used to working with Docker.  So far all of the images I've worked with have been for web services so a significant issue has been forwarding the right ports to the local system.  In this case, the container is intended just to run a single command and then exit, so the setup is much simpler.  The only thing that is required is to share a directory on the local system with the container so that we can pass data files to MAUS and get the results back.

Here's an example of running MAUS over a single audio file:

<pre>docker run -v <span class="pl-s"><span class="pl-pds">`</span><span class="pl-c1">pwd</span><span class="pl-pds">`</span></span>:/export  stevecassidy/maus \
    /home/maus/maus OUT=/export/test.TextGrid \
    OUTFORMAT=TextGrid \
    SIGNAL=/export/test/1_1119_2_22_001-ch6-speaker16.wav \
    BPF=/export/test/1_1119_2_22_001.bpf \
    LANGUAGE=aus</pre>

The Dockerfile is available on GitHub ([stevecassidy/docker-maus](https://github.com/stevecassidy/docker-maus)) and the image is on Docker Hub ([stevecassidy/maus](https://hub.docker.com/r/stevecassidy/maus/)).  If you have docker installed, the above command should download the image from the hub the first time you run it.

The next step is to write some wrapper code to make it easier to incorporate this into a workflow. I already have Python code that wraps around the regular command line version so the choice I face is whether to put this inside the container or call the container from the script.
