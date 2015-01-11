---
author: dema
comments: true
date: 2008-11-04 07:43:08+00:00
layout: post
slug: mini-tip-su-postgresql-php-e-lighttpd
title: Mini Tip su Postgresql , PHP e Lighttpd
wordpress_id: 503
categories:
- appunti
- tips
tags:
- lighttpd
- php
- postgresql
---

Ormai è palese che uso questo spazio anche come blocco note per futuri usi .

Ecco un appunto veloce su postgresql , php e lighttpd.

Pacchetti da installare : _aptitude install php5 php5-cgi php5-pgsql postgresql-8.3 postgresql-contrib lighttpd._

Modificare il file _/etc/lighttpd/lightpd.conf _ed aggiungere con l'editor preferito la riga

[sourcecode language='cpp']"mod_fastcgi"[/sourcecode]

sotto

[sourcecode language='cpp']server.modules              = ( [/sourcecode]

Modificare il file _/etc/php5/cgi/php.ini_ inserendo con l'editor preferito la riga

[sourcecode language='cpp']extension=pgsql.so[/sourcecode]

Ora siamo in grado di usare correttamente la funzione _pg_connect_ risparmiando un pomeriggio di bestemmie intense.
