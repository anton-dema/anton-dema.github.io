---
author: dema
comments: true
date: 2007-02-12 09:32:28+00:00
layout: post
slug: whisher-il-nuovo-wifi-sharing
title: Whisher il nuovo wifi sharing
wordpress_id: 1568
categories:
- ferran moreno
- fon
- fonera
- mike puchol
- ssid
- whisher
- wifi
- wifi sharing
---

![](http://dema.tv/wp-content/uploads/2007/02/clipboard01.jpg)

Appare nella scena movimentata del panorama della condivisione del wifi un nuovo attore.

[Whisher.com](http://whisher.com/en/index.php) è una nuova startup , capitanata da Ferran Moreno (ex partner FON ) e da [Mike Puchol](http://tech.am/). Lo scopo di Whisher è creare una comunità di condivisori di connettività wifi , senza l'uso di router proprietari o di firmware modificati. Tutto questo viene attuato mediante un'applicazione software client , che sostanzialmente mantiene in sincronia delle chiavi di accesso tra il nostro computer e il database centrale di Whisher.

Vediamo più in dettaglio come funziona.

<!-- more -->

Scaricando l'applicazione al sito di whisher.com ed installandola , otteniamo un software che assomiglia molto a [Netstumbler ](http://www.netstumbler.com/), una utility che fornisce la lista degli access point wireless attivi nelle vicinanze.

![](http://dema.tv/wp-content/uploads/2007/02/getting_started_5.jpg)

Una volta individuato il nostro access point privato , crittato mediante WEP o WPA , possiamo selezionarlo ed inserire la nostra chiave di accesso.

![](http://dema.tv/wp-content/uploads/2007/02/getting_started_6.jpg)

Dopodichè , occorre ottenere un account in whisher.com . Una volta registrati , sotto il vostro access point , apparirà un link per inserire il vostro punto d'accesso al network whisher. Iscrivendolo e posizionandolo nelle mappe , il database centrale di whisher , verrà informato circa la posizione geografica e la chiave d'accesso per il login , mediante uno scambio di dati criptato.

![](http://dema.tv/wp-content/uploads/2007/02/getting_started_8.jpg)

Questa è la prima parte del processo di affiliazione a whisher , la seconda è quella di avere la possibilità di accedere ad ogni punto d'accesso del network. Questo è possibile accedendo ad un menu dell'applicazione , dal quale si può selezionare l'area geografica di interesse , e completata la selezione , scaricare nel proprio computer le chiavi d'accesso dei diversi nodi.Quando ci si troverà a viaggiare , e lo stumbler di whisher riconoscerà un punto d'accesso del suo network , collegarsi ad esso sarà facile come un doppio click sul nome dell'access point.

![](http://dema.tv/wp-content/uploads/2007/02/connecting_to_wifi_1.jpg)

Oltre a questo , l'applicazione whisher è anche un client/server per scambio file peer2peer ad alta velocità tra computer registrate presso lo stesso nodo e un client di messaggistica istantanea tra i membri della comunità.

Ho provato di persona whisher alcuni giorni fa. Ho usato una fonera , che come tutti sanno possiede un doppio SSID . Per far funzionare il tutto , ho dovuto cambiare meccanismo di crypt , da WPA a WEP , dovuto ad un bug dell'applicazione whisher , che sembra non supporti crittature WPA.Tutto è filato abbastanza liscio , ad eccezione di una notevole lentezza di risposta da parte dell'applicazione. Il codice è ancora in early beta e i bug presenti , molto probabilmente , stressano troppo il processore. Ho avuto inoltre delle difficoltà nella sincronizzazione del database delle chiavi. Usando MacOSx , il file  Macintosh HD -> Library -> Preferences -> Whisher -> AccessPoints.xml che contiene tutti i dati dei nodi whisher , non veniva scritto, ma creando sia la directory che il file xml a mano , tutto ha funzionato a meraviglia. Il client ha molti bug , una volta agganciata una rete , non è  possibile passare ad un'altra , se non chiudendo e riaprendo l'applicazione , alcune icone scompaiono e non ritornano e qualche random crash chiude l'applicazione inaspettatamente.

Tralasciando i problemi di maturità del software , ed analizzando la filosofia di base di whisher , ho raccolto i punti favorevoli e contrari.

Favorevoli:

1)Indipendenza dall'hardware utilizzato. Tutti i router wifi e gli access point attualmente in commercio possono essere usati con whisher , senza nessuna modifica del firmware interno.

2)Sicurezza di navigazione tramite crittatura dei dati

3)Fortemente social oriented . L'applicazione punta a costruire un comunity anche se un'altro IM e un'altro client peer2peer non ha molto senso al giorno d'oggi. Mi ha ricordato molto Filetopia , un'applicazione p2p e IM spagnola di qualche tempo fa , ora non più sviluppata. Personalmente , avrei preferito solo uno stumbler per la connessione alla rete , disegnato magari senza tante animazioni , ma il mio approccio al software è decisamente NO eye-candy (ebbene sì , amo la riga di comando e la uso sempre quando posso)

Contrari:

1)Il software non è assolutamente maturo . Come ho detto sopra , i crash e la lentezza non permettono ancora un utilizzo giornaliero e continuo.

2)Anche se in futuro i bug verranno rimossi e l'applicazione raggiungerà un livello di usabilità soddisfacente , l'architettura di condivisione di whisher non mi convince. I dispositivi wireless infatti escono di fabbrica senza nessuna protezione firewall tra la WLAN e la LAN e quindi questo è un rischio per gli aspiranti whishers senza conoscenze di rete.Quindi forse Whisher si indirizza più ad una comunità di utilizzatori evoluti che siano in grado di stabilire le adeguate policy di accesso alla rete.

Voglio inoltre spendere un paio di parole circa l'integrazione tra Fon e Whisher , che come appare a tutti manifesto è molto semplice da attuare. Anzi a questo proposito , mi viene da pensare che forse il meccanismo di autenticazione è stato pensato appositamente , avendo in mente il doppio SSID della Fonera. Io prevedo nei prossimi giorni un attivo brainstorming ad Alcobendas per inibire la SSID della fonera alla registrazione presso whisher , ma francamente al momento , non vedo in che modo possa essere fatto.

Staremo a vedere.
