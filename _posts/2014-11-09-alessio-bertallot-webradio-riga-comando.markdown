---
author: dema
comments: true
date: 2014-11-09 16:28:24+00:00
layout: post
slug: alessio-bertallot-webradio-riga-comando
title: Alessio Bertallot, la webradio e la riga di comando.
wordpress_id: 2614
categories:
- radio
---

A me piace molto la nuova idea di Alessio Bertallot. La disintermediazione totale dai media mainstream, l'affidamento alla diffusione via web dei contenuti che una persona ha da proporre.

In una parola, il futuro della radio.  

Potete trovare tutto questo e molto altro sul [sito web di Alessio](http://www.bertallot.com/).

Quello che però volevo scrivere qui, in questo blog dimenticato e pieno di ragnatele, è un modo cool per ascoltare Alessio, almeno se hai linux e ami la riga di comando.

I requisiti sono:  

1. [zsh](http://www.zsh.org/)  

2. [mplayer](http://www.mplayerhq.hu/)  

3. [screen](http://www.gnu.org/software/screen/)

Per prima cosa crea un alias al tuo _.zshrc_


    
    <code>echo "alias alessio='mplayer http://dinky.do/tP'" >> ~/.zshrc ¹
    </code>



Poi apri un terminale, digita `screen` seguito da `alessio`. Lascia che la musica continui in sottofondo, abbandonando la sessione di screen con `CTRL+A` e `D`.

Ovviamente, questo funziona con qualsiasi altra url di streaming radio, ma il post mi è venuto di getto, nell'esigenza di ascoltare con un semplice comando da shell [Casa Bertallot](http://www.spreaker.com/user/bertallot)

¹ `Nota: l'url dinky.do punta alla più lunga url http://api.spreaker.com/listen/user/bertallot/episode/latest/shoutcast.mp3`
