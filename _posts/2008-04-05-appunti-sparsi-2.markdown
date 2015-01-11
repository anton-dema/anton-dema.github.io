---
author: dema
comments: true
date: 2008-04-05 11:15:36+00:00
layout: post
slug: appunti-sparsi-2
title: 'Appunti sparsi '
wordpress_id: 179
categories:
- '5645698'
- barcamp
- firefox
- flash
- fon
- fon sdk
- nspluginwrapper
- ubuntu
- urbino
- urbino wireless
tags:
- firefox
- fon
- fon sdk
- ubuntu
- urbino
- urbino wireless
---

In questa settimana ho messo da parte un po di argomenti in un angolo della mia mente per poi buttarli giù in un post , così  per promemoria personale .

L'età avanza e la memoria si fa sempre più fallace , quindi il solo fatto di averne scritto sul blog , alle volte mi fa ricordare di un ricordo ma non so quale (scusate , ma sto ascoltando in heavy rotation Studentessi di EELST )


##### FON e Università di Urbino


[![](http://dema.tv/wp-content/uploads/2008/04/ZZ33F6175D.jpg)](http://www.aghenorblog.com/2008/04/03/fon-e-universita-di-urbino/)

Venerdì 4 Aprile si è svolta ad Urbino la presentazione del progetto FON student.  Il progetto già presentato a Luglio 2007 a Firenze in occasione del WaveCamp da [Stefano Vitta](http://aghenorblog.com) e Alessandro Bogliolo finalmente si materializza e si propone di attivare una valida sinergia tra [FON](http://fon.com) , il network wifi più esteso al mondo e il [wireless campus di Urbino](http://www.wireless-campus.it/). Grazie a questa partnership , tutti gli studenti dell' ateneo e tutti gli abitanti della città di Urbino potranno godere di free roaming in tutti i FONSpot sparsi per il mondo e ovviamente tutti i Foneros potranno accedere alle strutture di Urbino Wireless. Attendo più informazioni da parte di Stefano , in quanto a malincuore non ho potuto partecipare all'evento.


#### FON SDK per sviluppatori


In una mail di venerdì 4 Aprile , Steven Leeman mi ha portato all'attenzione che dal suo blog , [Martin Varsavsky ](http://english.martinvarsavsky.net/general/and-the-winner-is-fonerabot.html#comments)ha annunciato il rilascio entro i prossimi 90 giorni di un firmware di sviluppo per la Fonera.

L'idea di un [FON SDK](http://www.zarrelli.org/blog/index.php/2008/04/04/in-arrivo-lsdk-per-fone-anche-oswave/) non sarebbe niente male e finalmente se ne potrebbero vedere delle belle !


#### Firefox freeze a causa di Flash


Lo so questo non è terreno mio e sicuramente  verrò smentito e demolito nei commenti , ma a me flash su Ubuntu 7.10 non funziona .

O meglio funziona,  ma se posto sotto stress con ripetuti reload di un filmato su youtube per esempio , congela firefox e l'unico modo per terminare la sessione è un kill -9 PID.

La scorsa settimana ho installato Fedora 8 e devo dire che non ho più sofferto di questi freeze.

La risposta credo sia che Fedora ha introdotto nspluginwrapper anche per la versione a 32bit , in modo da impedire  il crash del processo di firefox , anche in caso di malfunzionamento delle librerie del flashplugin.

[sourcecode language='cpp']

[dema@aronne ~]$ ps aux | grep nsplu
dema     14353  1.2  0.3  79344 15872 ?        S    12:51   0:04 /usr/lib/nspluginwrapper/npviewer.bin --plugin /usr/lib/mozilla/plugins/libflashplayer.so --connection /org/wrapper/NSPlugins/libflashplayer.so/13558-2
[/sourcecode]

Visto che le mie sono solo presupposizioni di un niubbo , se c'è un guru out there batta un colpo.

Buon fine settimana e pace e bene a tutti :)
