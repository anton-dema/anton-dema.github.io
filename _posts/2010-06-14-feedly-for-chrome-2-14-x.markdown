---
author: dema
comments: true
date: 2010-06-14 09:17:13+00:00
layout: post
slug: feedly-for-chrome-2-14-x
title: Feedly for Chrome 2.14.x
wordpress_id: 1144
categories:
- minihowto
tags:
- chrome
- estensioni
- feedly
- feeds
- get satisfaction
- google
- reeader
- rss
---

Per la lettura dei feed RSS da più di un mese a questa parte sto usando [Feedly](http://www.feedly.com/).

Feedly è un'estensione per Mozilla Firefox, Google Chrome e Safari che si interfaccia al nostro account Google Reader e tramite una serie di javascript ridisegna il layout dell'aggregatore rendendolo del tutto simile ad una pagina web stile magazine.

E' compatibile con le shortcuts di Greader, è impreziosito da AJAX ed ha tutti gli strumenti per la condivisione su social network, offline reading (tramite Instapaper) e mille altre fighetterie.

Insomma il feed reader definitivo, secondo me.

Quello che però lo rende instabile, almeno su Google Chrome, è il costante aggiornamento di release. Ogni giorno viene infatti caricata una nuova versione che purtroppo, a partire dalla 2.15.x, sembra aver tralasciato una delle features per me più importanti e che l'ha reso per me inutilizzabile: la navigazione con i tasti N e P.

Nella schermata qui sotto è riprodotto il mio uso tipico di feedly: L'apertura di una categoria di feed, un'occhiata d'insieme alla pagina, uso del tasto N per selezionare l'articolo che ritengo possa essere interessante e tasto invio per espandere il post.

[caption id="attachment_1146" align="alignnone" width="510" caption="Nel cerchio si può notare l'ombreggiatura che notifica all'utente che l'articolo è stato selezionato"][![](http://dema.tv/wp-content/uploads/2010/06/feedly11.jpg)](http://dema.tv/wp-content/uploads/2010/06/feedly11.jpg)[/caption]

Ebbene, dalla versione 2.15.x di Feedly l'ombreggiatura che indica all'utente quale articolo è stato selezionato tramite i tasti N o P è sparita, rendendo di fatto impossibile usare Feedly tramite tastiera, almeno per quanto mi riguarda.

Mi sono chiesto quindi come poter effettuare un downgrade, come poter tornare alla versione 2.14.x che funzionava secondo me in maniera perfetta. Ho cercato di trovare qualche repository con lo storico dell'estensione cercando con Google e sulla pagina [Get Satisfaction](http://www.getsatisfaction.com/feedly) di Feedly, ma invano.

Ieri, per caso, ho acceso una virtual machine con Ubuntu 10.4, Chrome e Feedly che non usavo da circa un mese. Prontamente ho disattivato l'accesso ad internet per evitare l'update automatico delle estensioni ed ho recuperato Feedly 2.14.267.

Immediatamente l'ho salvato ed ho provveduto ad aggiornare tutte le mie installazioni di Chrome seguendo questa procedura:



	
  1. Ho disinstallato Feedly dal pannello _estensioni_ di Chrome

	
  2. Ho copiato la cartella 2.14.267 in una directory nascosta (Linux) chiamata .feedlyext

	
  3. Ho aperto nuovamente il pannello _estensioni_ di Google Chrome e scelto _carica estensione non pacchettizzata_

	
  4. Ho scelto il path della directory nascosta precedentemente creata e ricaricato la mia estensione preferita che, non essendo pacchettizzata, non è più soggetta ad update automatico.


La procedura è analoga anche per Windows e MacOS 10 .

Se anche voi avete riscontrato questi problemi, ho approntato per il download un file zip con la versione 2.14.267 pronto per essere scompattato ed installato secondo la procedura appena descritta.

Il file può essere prelevato [qui](http://dema.tv/wp-content/uploads/2010/06/2.14.267.zip).
