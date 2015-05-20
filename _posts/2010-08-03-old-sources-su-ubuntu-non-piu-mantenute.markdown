---
author: dema
comments: true
date: 2010-08-03 07:33:55+00:00
layout: post
slug: old-sources-su-ubuntu-non-piu-mantenute
title: Old sources su Ubuntu non più mantenute
wordpress_id: 1201
categories:
- appunti
tags:
- apt-get
- linux
- lts
- sources.list
- ubuntu
---

Certe volte capita di dover lavorare su vecchie installazioni di Ubuntu, ancora perfettamente funzionanti ma non di classe LTS e quindi ormai abbandonate.

Può capitare di voler aggiungere qualche pacchetto, ma essendo scaduto il supporto, apt-get install non funziona più.

La soluzione è cambiare il file sources.list, elencando come target old-releases.ubuntu.com. Ecco un esempio con Gutsy 7.10  :

  deb http://old-releases.ubuntu.com/ubuntu/ gutsy main restricted
  deb http://old-releases.ubuntu.com/ubuntu/ gutsy-updates main restricted
  deb http://old-releases.ubuntu.com/ubuntu/ gutsy universe
  deb http://old-releases.ubuntu.com/ubuntu/ gutsy-updates universe
  deb http://old-releases.ubuntu.com/ubuntu/ gutsy multiverse
  deb http://old-releases.ubuntu.com/ubuntu/ gutsy-updates multiverse
  deb http://old-releases.ubuntu.com/ubuntu/ gutsy-security main restricted
  deb http://old-releases.ubuntu.com/ubuntu/ gutsy-security universe
  deb http://old-releases.ubuntu.com/ubuntu/ gutsy-security multiverse

Dopo un apt-get update possiamo nuovamente aggiungere il pacchetto preferito con apt-get install.
