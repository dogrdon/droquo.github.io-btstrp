---
layout: project_post
title: "Wikisewer"
description: ""
category: project 
comments: false
tags: [wikisewer, python, nodejs]
---
{% include JB/setup %}

<p>Wikisewer is a fork of <a href="https://github.com/edsu/wikistream">Wikistream</a>. Being intrigued by digital ephemera, I took this opportunity to start collecting instances of Wikipedia vandalism, which - when caught - get promptly removed from the record.</p>

<p>This project, like Wikistream, monitors the recent changes wikipedia irc channel. It then scrapes the pages identified as having some vandalism on them, collects the proper metadata like date, time, the information for the wikipedia page, the parties involved, and the content of the vandalism and stores it in mongodb.</p>

<a class="source" href="https://github.com/droquo/wikisewer">You can see more at the github repository</a>