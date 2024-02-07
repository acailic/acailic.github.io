---
title: Software Design Philosophy
layout: post
tags: [software, design]
date: 2021-04-02
---

# Classes should deep
# Deep interface : Unix file I/O
# Defines errors out of existance
###  Try to minimize the numberd of places we have to handle exceptions
## No erros eaither side - moving without errors. Exp. Unix vs Windows file delete.
#### Substring- java exception
#### Throwing exception further down the stream
# Tacticas vs Strategic promgraming
## Tactical
### goal: get the next feature/bug fix working ASAP
### A few shortcuts and kludges are ok ?
### result: bad desing, high complexity
### going into tornado.
## Strategic
### Goal: Great design
### Simplify future development
### Minimize complexity
### Must sweat the small stuff
## Investment mindset
### Take extra time today
### Pays back in long run
#### continual small investments (10-20%)
#### new code- careful desing, good documentation
### when changing code
#### improve somethings
#### dont settle at fewest modified lines of code
#### after change, system is the way it would have been if designed that way from start
