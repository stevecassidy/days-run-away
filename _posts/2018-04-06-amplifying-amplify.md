---
id: 522
title: Amplifying Amplify
date: 2018-04-06
author: Steve Cassidy
layout: single
categories:
  - Language Resources
---
[Amplify](https://amplify.sl.nsw.gov.au/) is a project maintained by the State Library of NSW for crowdsourcing the transcription of Oral History recordings. It is being used to generate transcripts of some of their extensive catalogue of OH recordings. Amplify starts with the results of an Automatic Speech Recognition system and presents these to users of the platform for correction and validation. Amplify is great! I'd encourage you to go along and try out the correction process.

I'm interested in how OH transcripts might be enhanced with the application of Natural Language Processing techniques.  As part of the Alveo project, we're working on developing tools to help with recording, archiving and enhancing OH recordings.  As a platform for experimenting with this I've built a little web application that takes transcripts from the State Library's Amplify platform and puts them through a few NLP processes and then presents the results.  This is now available online as [Amplify Amplifier](https://amp.apps.alveo.edu.au/).

The app will process any transcript from Amplify that has a 100% completed rating.  This should mean that it has been corrected and verified by users but there are some wrinkles in Amplify (that SLNSW know about) that mean that often errors slip through the net.  The app applies three processes at the moment:

  * Topic segmentation using the NLTK implementation of [Marti Hearst's text-tiling algorithm](http://people.ischool.berkeley.edu/~hearst/research/tiling.html). This chunks the interview into topics based on the distribution of words in the text.
  * For each topic, we find keywords that are more common in that topic than in the rest of the text using the TF-IDF metric.  This is an attempt to identify the main concepts in each topic.
  * For each topic, we use the DB-Pedia Spotlight named entity linking service to find the names of people, places and concepts and link them to relevant entries in DBPedia (a machine readable version of Wikipedia).

All of this is presented in the page with buttons to allow you to play each topic.

The results are interesting. The topics that Text Tiling finds do give an idea of the overall structure of the interview, although in many cases it is over-segmenting.  Keywords give some idea of what each topic is about and sometimes seem to act as a nice summary; other times they are entirely un-useful, such as the single keyword &#8216;er' - although I guess this means the speaker is hesitating a lot in that chunk.

Named entities are perhaps the most random aspect of the result. This is not too surprising since we are using a system trained on very different kinds of text in the context of Australian oral histories.  So it will try to find the most common entity to link to a name or place and often end up with Harry Potter or some other popular icon rather than a local alternative.

There is a lot of scope here for improving the results that are generated. However, I'm interested in feedback from OH researchers and other readers of these transcripts to see if this kind of presentation is useful at all.   How could this be improved? What other elements of the text could be brought out in a useful way?

I'll continue to experiment with this and hopefully develop some more useful tools with some feedback from interested users.
