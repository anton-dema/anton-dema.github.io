---
author: dema
comments: true
date: 2010-11-24 13:43:24+00:00
layout: post
slug: ilpost-it-su-kindle
title: 'ilpost.it su kindle '
wordpress_id: 1307
categories:
- howto
tags:
- calibre
- feed
- google reader
- ilpost.it
- kindle
- notizie
- rss
---

Kindle è un meraviglioso dispositivo per la lettura, già lo sapete. Non esiste nessuna esperienza simile, sia via tablet retroilluminato, sia tramite altri lettori ebook a inchiostro elettronico.

Il bisogno che nasce quasi subito,  però,  è  avere una fonte di notizie fresche aggiornate quotidianamente e per questo l'offerta italiana è molto limitata, anzi, forzata alla sola Stampa di Torino disponibile in Kindle edition.

Per fortuna esiste [Calibre](http://calibre-ebook.com/), il perfetto compagno del nostro dispositivo di lettura preferito. Ebbene, Calibre, tra le altre cose, è in grado di scaricare ad intervalli regolari un feed rss, trasformarlo in formato mobi e consegnarlo via email al nostro indirizzo nomeutente@kindle.com.

Grazie a queste funzioni possiamo costruire il nostro aggregatore di notizie personalizzato e depositare ogni mattina le notizie sul nostro Kindle in maniera automatizzata. Chiaramente questo non è un processo facile e richiede alcuni trucchetti, ma niente di impossibile. Vediamo insieme come procedere, prendendo per esempio l'ottimo news remixer italiano [ilpost.it](http://ilpost.it).

Per prima cosa abbiamo bisogno di un abbonamento a Google reader. Google reader è l'aggregatore di feed rss più completo in circolazione, permette la consultazione dei feed, la loro catalogazione in tag e soprattutto la possibilità di rendere pubblici i tag selezionati. Quest'ultima caratteristica di google reader è quella che useremo per fare il parsing dei feed de ilpost.it. Di default infatti ilpost.it rende disponibile solo i primi 10 articoli nella home page, tramite feedburner. I feed sono rilasciati in maniera integrale e non troncati, quindi perfetti per la lettura del contenuto.

Usando google reader come buffer, possiamo tranquillamente aggirare questa limitazione ed attingere quindi ad un maggiore numero di feed, utile nel caso in una giornata vengano pubblicati più di 10 posts.

Per prima cosa assegniamo un tag alla fonte rss de ilpost.it, banalmente chiamata "il post"

[![](http://dema.tv/wp-content/uploads/2010/11/primo1.jpg)](http://dema.tv/wp-content/uploads/2010/11/primo1.jpg)

Successivamente nei settaggi di google reader rendiamo pubblica l'etichetta appena creata.

[![](http://dema.tv/wp-content/uploads/2010/11/secondo1.jpg)](http://dema.tv/wp-content/uploads/2010/11/secondo1.jpg)

Ed andiamo a visitare la public page dell'etichetta "il post"

[![](http://dema.tv/wp-content/uploads/2010/11/terzo11.jpg)](http://dema.tv/wp-content/uploads/2010/11/terzo11.jpg)

Ricaviamo ora l'indirizzo del feed rss di questa etichetta fornita via google reader, cliccando sull'icona del feed rss o sul link atom feed. Ecco quello che otteniamo:

[![](http://dema.tv/wp-content/uploads/2010/11/quarto1.jpg)](http://dema.tv/wp-content/uploads/2010/11/quarto1.jpg)

La parte che ci interessa è l'indirizzo del feed atom ovviamente. Anche qui dobbiamo adottare un piccolo trucchetto per ottenere più articoli, in quanto di default il feed atom di google reader restituisce solo i primi 20 articoli. Per ottenerne di più basta aggiungere alla fine dell'url la stringa ?n=x dove con x indichiamo il numero degli articoli desiderati. Forse è più chiaro con un esempio:

[sourcecode language="bash"]www.google.com/reader/public/atom/user/08725434140128325765/label/il%20post?n=40[/sourcecode]

Fornisce 40 articoli de ilpost.it dal più recente al più vecchio. Il limite delle API di Google è di 100 articoli, quindi abbiamo ampi spazi di manovra.

Ottenuto l'indirizzo del feed rss completo e composto dagli articoli desiderati, dobbiamo mettere all'opera Calibre.

Questa parte è un po' hackish, vuoi perché abbiamo la necessità di avere un computer sempre acceso, vuoi perché l'ideale sarebbe farlo girare in un ambiente virtuale in modo da non occupare risorse al nostro desktop. In sostanza, dobbiamo mettere su un servizio e non tutti hanno la possibilità di avere a disposizione queste risorse.

Io per la parte Calibre mi sono affidato ad una virtual machine che avevo, sonnacchiosa, residente nello Xen qui in azienda. E' una Lucid 10.4 desktop per la quale esistono i pacchetti di Calibre e sono installabili direttamente da software manager o via apt-get install.

Una volta installato Calibre, configuriamolo per dispositivi Kindle, in modo che crei di default files in formato .mobi. Durante il wizard ci verrà anche chiesto il nostro indirizzo @kindle.com e i parametri per l'invio dei documenti via email.

Sistemati questi parametri dobbiamo passare a configurare lo scaricamento dei feed rss usando l'indirizzo che abbiamo ricavato poc'anzi
[![](http://dema.tv/wp-content/uploads/2010/11/quinto1.jpg)](http://dema.tv/wp-content/uploads/2010/11/quinto1.jpg)

Riempiamo i campi : titolo ricetta - articolo più vecchio - numero massimo di articoli per feed (la traduzione è alquanto bizzarra, specialmente per quanto riguarda "ricetta" che evoca, ahimè, cotto e mangiato della Parodi)

Inseriamo il titolo del feed e l'url che abbiamo ricavato dalla pagina shared item di google reader, ricordando di aggiungere ?n=x alla fine con x uguale al numero di articoli che vogliamo prelevare.

Per finire chiudiamo con il pulsante aggiungi/aggiorna ricetta e close.

A questo punto dobbiamo schedulare lo scaricamento.

[![](http://dema.tv/wp-content/uploads/2010/11/sesto1.jpg)](http://dema.tv/wp-content/uploads/2010/11/sesto1.jpg)

Qui ho bisogno di prove sul campo. Infatti se è vero che con i settaggi evidenziati nell'immagine qui sopra recupero tutti gli articoli alle 6.00 e che grazie al parametro articolo più vecchio 1 giorno, non dovrei avere fastidiose sovrapposizioni, non ho ancora testato il reale funzionamento del processo automatizzato. Magari ne darò conto nei prossimi giorni nei commenti.

Abbiamo fatto. E' sufficiente lasciare Calibre in esecuzione sulla macchina virtuale e terminare la sessione di desktop remoto (nx, via, siamo sotto linux o no?) e il programma dovrebbe eseguire in automatico le seguenti operazioni:

1)ogni mattina alle 06.00 scaricare i feed rss dalla nostra pagina shared items
2)controllare che non ci siano articoli più vecchi delle 06.00 del giorno precedente
3)convertire il feed xml in formato .mobi
4)inviarlo come allegato per posta al nostro indirizzo @kindle.com

A questo punto è sufficiente accendere la wifi o il 3g del Kindle per vedersi recapitare ilpost.it pronto per essere letto con soddisfazione con il nostro reader preferito.

[![](http://dema.tv/wp-content/uploads/2010/11/iphone11.jpg)](http://dema.tv/wp-content/uploads/2010/11/iphone11.jpg)

[![](http://dema.tv/wp-content/uploads/2010/11/iphone21.jpg)](http://dema.tv/wp-content/uploads/2010/11/iphone21.jpg)

[![](http://dema.tv/wp-content/uploads/2010/11/iphone31.jpg)](http://dema.tv/wp-content/uploads/2010/11/iphone31.jpg)

Una considerazione finale che mi preme fare è che, molto probabilmente, questo metodo di parsing delle notizie va contro gli interessi dell'editore de ilpost.it in questo caso, in quanto in questo modo si enuclea completamente il contenuto pubblicitario, linfa di sostentamento per il sito. Ma non è una questione di furbizia per evitare noiose pubblicità, che tra l'altro ne ilpost.it non sono neppure troppo invasive, ma una semplice procedura per soddisfare un bisogno che si era manifestato per quanto mi riguarda.

E' normale che se ilpost.it fosse stato disponibile in formato push per kindle, anche senza passare dallo store Amazon (vedi [instapaper](http://www.instapaper.com/)) anche ad una cifra congrua per abbonamento, avrei evitato di pensare a questa soluzione, che tra l'altro, non ho controllato, ma potrebbe già essere stata adottata da altri, documentata su blog e così via.

Fatemi sapere cosa ne pensate nei commenti.
