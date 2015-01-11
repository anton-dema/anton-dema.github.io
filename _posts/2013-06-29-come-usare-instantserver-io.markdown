---
author: dema
comments: false
date: 2013-06-29 14:01:27+00:00
layout: post
slug: come-usare-instantserver-io
title: Come usare instantserver.io
wordpress_id: 2371
---

Conoscete instantserver.io? Sì? Allora potete saltare direttamente alla descrizione su come stabilire un tunnel ssh SOCKSv5 e ridirigere tutti i protocolli che supportano tale metodo di tunneling (irc e http/s, ad esempio) attraverso questo canale.
Sapete già come instaurare questo tipo di connessione? Allora potete tranquillamente ignorare questo post :-)


### Instantserver.io


Dicevamo, [instantserver.io](http://instantserver.io). Il servizio in questione permette di lanciare un'istanza EC2 micro di Ubuntu 13.04 presso AWS, con la sola pressione di un bel bottone verde. Questa istanza sarà a nostra disposizione per un totale di 31 minuti in maniera gratuita, con la possibilità di estendere per altri 5 minuti, sempre gratuitamente, la nostra sessione. Qualora avessimo bisogno di più tempo, possiamo acquistare un'estensione, a pacchetti che partono da 4 us$ al giorno.

[![instantserver.io](https://dema.tv/wp-content/uploads/2013/06/Istantanea-29062013-141547.jpg)](https://dema.tv/wp-content/uploads/2013/06/Istantanea-29062013-141547.jpg)

Non appena avremo pigiato il bottone "Get a Free Server" ci verranno indicati i parametri utili per collegarci

[![parametri instantserver.io ](https://dema.tv/wp-content/uploads/2013/06/Istantanea-29062013-142944.jpg)](https://dema.tv/wp-content/uploads/2013/06/Istantanea-29062013-142944.jpg)

Bene, ora, che cosa possiamo fare con questo server _temporaneo_? In giro si sono sentiti molti casi d'uso, dal test di repository PPA di ubuntu, da piccoli cloni istantanei di server web — bisogna essere svelti, però — fino a torrent daemon temporanei.
L'uso, secondo me, più immediato e proficuo è quello di usare questa istanza come destinazione di un tunnel ssh SOCKSv5 per bypassare eventuali filtri web o per anonimizzare, parzialmente, la navigazione web.
Vediamo come fare, con microsoft windows.


### Tunnel ssh SOCKSv5 con PuTTY


Procuriamoci il software PuTTY al seguente indirizzo [http://the.earth.li/~sgtatham/putty/latest/x86/putty.exe](http://the.earth.li/~sgtatham/putty/latest/x86/putty.exe)
Lanciamo l'eseguilbile putty.exe

[![putty screenshot](https://dema.tv/wp-content/uploads/2013/06/putty1.png)](https://dema.tv/wp-content/uploads/2013/06/putty1.png)

Inseriamo l'indirizzo IP dell'istanza ottenuta da instantserver.io, nel nostro caso 54.226.226.82, e selezioniamo nel menu _category_ SSH.

[![putty screenshot](https://dema.tv/wp-content/uploads/2013/06/putty2.png)](https://dema.tv/wp-content/uploads/2013/06/putty2.png)

Qui scegliamo _enable compression_ e il protocollo ssh 2.
Spostiamoci poi alla voce _tunnels_

[![putty screenshot](https://dema.tv/wp-content/uploads/2013/06/putty3.png)](https://dema.tv/wp-content/uploads/2013/06/putty3.png)

Qui dobbiamo inserire il numero di porta tcp nel campo _Source Port_ (nell'esempio 9999, ma potete scegliere un numero _alto_ a vostro piacimento) e nei radio buttons sottostanti, flaggare le opzioni _auto_ e _Dynamic_. Per finire, applichiamo i settaggi cliccando su _add_ e dovremmo ottenere una schermata simile a questa.

[![putty screenshot](https://dema.tv/wp-content/uploads/2013/06/putty4.png)](https://dema.tv/wp-content/uploads/2013/06/putty4.png)

Per finire il setup di PuTTY, ritorniamo su _Session_, diamo un nome tipo _my ssh proxy_ a questa sessione, la salviamo con il pulsante _save_ e clicchiamo su _Open_.

La schermata che ci verrà presentata sarà simile a questa:

[![screenshot ](https://dema.tv/wp-content/uploads/2013/06/first.jpg)](https://dema.tv/wp-content/uploads/2013/06/first.jpg)

Clicchiamo su _Sì_ per inserire la chiave rsa del server nei nostri host conosciuti e, per finire, inseriamo le credenziali — per mantenere l'esempio sopra elencato, la nostra login sarà _ ubuntu_ e la password _utn5bZu7_ — al prompt di PuTTY.

[![shot putty](https://dema.tv/wp-content/uploads/2013/06/shot.jpg)](https://dema.tv/wp-content/uploads/2013/06/shot.jpg)

Da questo momento, saremo collegati alla nostra istanza su EC2 e potremo usare la shell remota, ma allo stesso tempo avremo instaurato un tunnel SOCKSv5 tra il nostro host e quello remoto attraverso la porta 9999. Vediamo quindi come configurare windows per instradare i protocolli http e https verso questa sock.



### Configurare windows per utilizzare SOCKSv5



Dobbiamo ora fare in modo che la nostra navigazione web passi attraverso la sock che abbiamo appena instaurato. 
Per fare questo, andiamo in _pannello di controllo_ -> _opzioni internet_ -> _connessioni_ -> _impostazioni lan_ flaggare _server proxy_ e poi scegliere _ avanzate_.
Nel campo socks inseriamo l'indirizzo di loopback 127.0.0.1, o il valore "localhost" e la porta 9999. 

[![schermata](https://dema.tv/wp-content/uploads/2013/06/putty5.png)](https://dema.tv/wp-content/uploads/2013/06/putty5.png)

Abbiamo finito. Da questo momento, la navigazione web verrà instradata attraverso la sock, aggirando tutti i blocchi di eventuali firewall o filtri web e anonimizzando (parzialmente) la nostra sessione. 

Per evitare di modificare i settaggi di sistema di windows, potremmo utilizzare Firefox, che dispone di un proprio setup per i server proxy, mantenendo quindi separata la navigazione attraverso sock da quella normale. 

Fatemi sapere cosa ne pensate, la discussione è su G+, dal momento che ho disabilitato i commenti sul blog, per un motivo di obsolescenza del medium (magari parliamo anche di questo, perché no).





