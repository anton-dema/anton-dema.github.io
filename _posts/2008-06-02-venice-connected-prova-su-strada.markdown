---
author: dema
comments: true
date: 2008-06-02 20:37:38+00:00
layout: post
slug: venice-connected-prova-su-strada
title: 'Venice connected , prova su strada '
wordpress_id: 216
categories:
- fon
- metro wifi
- muni wif
- venezia
- venice connected
- wifi
tags:
- fon
- metro wifi
- muni wifi
- venezia
- venice connected
- wifi
---

Per il ponte del 2 Giugno sono ritornato a Venezia dopo il [twitter+fooga camp](http://dema.tv/2008/04/21/twittercamp-e-foogacamp-un-successo/). Da bravo internet addicted , anche se mi ero imposto di lasciare i laptops a casa non ho fatto a meno di portarmi appresso l'iPhone come unico embedded internet device . Per essere più precisi , la mia connettività viene garantita da blackberry pearl 8100  per email e feeds rss e da iPhone per tutto il resto quando trovo una wifi aperta o un hotspot FON.

Insomma ero partito per un week-end **parzialmente off-line** , da godermi in spiaggia presso lo stabilimento di Venezia Spiagge dove mia madre da più di 20 anni ha un noleggio di pedalò . La mattina però , entrando nello stabilimento **ho notato un banchetto con un manifesto di Venice connected** , _in spiaggia ... on-line_ e la tentazione di provare questo nuovo servizio è stata troppo forte. Avevo già parlato di [Venice-Connected,](http://www.veniceconnected.it/ViewNews.aspx?lang=IT) la muni-wifi del comune di Venezia , che tramite la società Venis spa ha in progetto di coprire l'intero territorio in mesh wifi con backhauling in fibra entro la fine del 2009. L'area degli stabilimenti di Venezia Spiagge è ,  assieme al piazzale del Palazzo del Cinema e Piazza San Marco , il testbed per il servizio di prossima attuazione.

Le aree coperte al momento sono grosso modo queste:

[![](http://dema.tv/wp-content/uploads/2008/06/lido1.png)](http://dema.tv/wp-content/uploads/2008/06/lido1.png)

La prima stazione si trova nel mezzo della zona ombrelloni-camerini della ex Zona A e la seconda stazione nello stabilimento distaccato di San Nicolò .

Per la copertura radio la scelta di apparato è ricaduta su [Cisco aironet serie 1520](http://www.cisco.com/en/US/products/ps8368/index.html) . Una soluzione per outdoor access point  robusta , scalabile e  professionale.

[![](http://dema.tv/wp-content/uploads/2008/06/mast1.png)](http://dema.tv/wp-content/uploads/2008/06/mast1.png)

La **gestione degli accessi** è  affidata a [Colubris Network](http://www.colubris.com/). La soluzione di network management di Colubris fornisce il controllo degli accessi mediante **captive portal** , bilanciamento del carico di rete , **priorità dei pacchetti e qos** . Non so se [Venis spa](http://www.venis.it/) abbia attivato tutte queste opzioni presso Colubris o se abbia scelto solo di usare il loro sistema di autenticazione utenti .

Il **link alla rete internet** è fornito da Venis spa tramite interbusiness . **Non posso dire se in fibra o meno**. Le prove che ho potuto effettuare con l'iPhone non mi hanno permesso di effettuare uno speedtest ne tantomeno un test di latenza. La **sensazione di navigazione** era comunque quella di una normale linea dsl , nemmeno troppo performante.

L'autenticazione degli utenti avviene mediante **username e password contenuti in una scheda prepagata** del valore di 3 Euro per 24 ore di connessione. I titolari di capanna e ombrellone stagionali possono **godere della connessione gratis** , diversamente si puo' sottoscrivere un abbonamento wifi al costo di 20 Euro per il periodo che va dal 31/05 al 14/09 . Le tessere vengono **rilasciate previa presentazione di una fotocopia del documento di identità** per ottemperare alle richieste del decreto Pisanu in materia di antiterrorismo.

La prova su strada ha evidenziato una **particolare difficoltà di accesso tramite dispositivo mobile**. Safari per iPhone ha infatti renderizzato la pagina di login del captive portal come una normale pagina web , dovendo ricorrere alla funzione di pinch zoom per visualizzare chiaramente i campi di login. Una volta effettuato il login **la navigazione è stata fluida e abbastanza veloce**. Come ho detto dianzi avevo l'impressione di essere collegato mediante una normale adsl , niente che facesse presupporre una ampiezza di banda spropositata a monte. Ho effettuato una **prova di chiamata tramite fring** , sia con skype-out che con il mio server voip , e sono riuscito a conversare con il mio interlocutore , anche se la **latenza era ai limiti del sostenibile**. Ho riscontrato **un paio di volte la caduta del segnale wifi **, per una trentina di secondi circa  , ed al ritorno del segnale radio il link non richiedeva nuovamente la login . Una volta però che si riaccede alla rete dopo aver scollegato e ricollegato la scheda wifi dell'apparecchio viene nuovamente richiesta username e password.

Alla fine della giornata trascorsa in spiaggia **mi sono fermato a chiacchierare un po' con Sara** , che per tutta la giornata si è adoperata per far conoscere e promuovere il servizio ai clienti dello stabilimento. In due giorni di servizio è riuscita a distribuire solo 9 tessere gratuite ad affittuari stagionali di capanne , 2 tessere sempre gratuite  ad un hotel che riserva posti ombrellone per i suoi clienti e una login da 24 ore al sottoscritto.

Sara ha detto che **non ha riscontrato un eccessivo entusiasmo** per l'esistenza del wifi in spiaggia, forse per la mancanza di una clientela particolarmente giovane . Qualcuno addirittura ha chiesto se avrebbe **trovato il computer per navigare in internet sotto l'ombrellone** !!

Insomma , nonostante la pompa magna con cui il servizio è stato lanciato , l'impiego di tecnologie di tutto rispetto e lo scrupoloso rigore nel seguire le norme vigenti , il servizio wifi di Venice Connected non ha fatto molto di più di un qualsiasi altro servizio già presente in altri stabilimenti balneari d'Italia . Piuttosto una volta di più mi viene da pensare **come il modello del wifi partecipato** alla [Fon](http://fon.com) puo' venire incontro a queste Metro-Wifi . Se come dice [Quintarelli](http://blog.quintarelli.it/blog/2008/05/so-long-wifi.html) :


<blockquote>Wifi non e' una tecnologia pensata per un operatore.
i costi di esercizio sono alti, rispetto a tecnologie diverse (HSxPA o Wimax).
L'idea di farlo meshed, sulla base delle tecnologie attuali, in citta', non sta in piedi con i costi (IMHO).
**A meno che non sia fatta totalmente dal basso da persone competenti (ma la copertura non puo' essere ancora estesa); i costi si trasferiscono dall'operatore all'utente che opera il nodo**.</blockquote>


Non esiste alternativa , o la rete cittadina wifi nasce dal basso , generata dagli utenti , o ci si deve rassegnare ad un fallimento.

Insomma , rimane valida [l'offerta della pizza](http://dema.tv/2008/04/22/venice-connected/)
