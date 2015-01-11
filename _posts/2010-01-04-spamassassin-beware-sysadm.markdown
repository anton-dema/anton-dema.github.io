---
author: dema
comments: true
date: 2010-01-04 08:18:27+00:00
layout: post
slug: spamassassin-beware-sysadm
title: Spamassassin, beware SysAdm
wordpress_id: 953
categories:
- sysadmin
tags:
- mail
- spam
- spamassassin
- sysadmin
---

Bentornati al lavoro, stimati colleghi amministratori di sistema.
Siccome che presumibilmente siete molto più attenti del sottoscritto,  vi sarete già premurati la sera di San Silvestro di aggiornare gli score di spamassassin.

Infatti con lo scattare del nuovo anno, il simpatico script ammazzaspam attiva la regola FH_DATE_PAST_20XX, assegnando un punteggio di 3.2 a tutte le email in arrivo e generando molti falsi positivi.

Per ovviare al problema basta inserire la seguente regola:

_ score FH_DATE_PAST_20XX 0_

in /etc/spamassassin/local.cf.

Ma tanto lo sapevate già, nevvero?
