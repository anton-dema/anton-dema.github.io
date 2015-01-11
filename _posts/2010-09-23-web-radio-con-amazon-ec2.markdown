---
author: dema
comments: true
date: 2010-09-23 08:59:04+00:00
layout: post
slug: web-radio-con-amazon-ec2
title: 'Web radio con Amazon EC2 '
wordpress_id: 1221
categories:
- howto
tags:
- amazon
- darkice
- ebs
- ec2
- fedora
- icecast
- linux
- oddcast
- putty
- radio
- streaming
---

Come forse già sapete, [Amazon ha lanciato le nuove micro istanze](http://compl.in/aZtVWS) da 0.02 US$ l'ora che permettono di avere un server virtuale  con 633 MB di Ram e 1 VCPU. Questo ci permette di poter fare delle sperimentazioni senza troppi patemi d'animo.

Quello che segue è un piccolo esperimento per un server webradio [Icecast](http://compl.in/dkByAK) da usare in qualsiasi momento per una diretta radio estemporanea con la possibilità di collegare un discreto numero di ascoltatori contemporaneamente.

Infatti ogni istanza EC2 Amazon dovrebbe poter contare su 250Mbps di banda upstream, il che permette un totale di quasi 1900 ascoltatori a 128Kbps con un costo indicativo di circa 0.15 us$ per Gb. Per un'ora di trasmissione con 2000 ascoltatori dovremmo spendere all'incirca 15 euro. Ci dovremmo stare nelle spese :-)

Andiamo a vedere nel dettaglio come lanciare un'istanza micro di fedora core 8.

<!-- more -->

Servendoci della pratica console web di AWS lanciamo il wizard per lo start delle istanze.

[![Scelta dell'istanza](http://dema.tv/wp-content/uploads/2010/09/launch11.jpg)](http://dema.tv/wp-content/uploads/2010/09/launch11.jpg)

Scegliamo una Basic fedora Core 8 a 32bit che dispone di un EBS da 15GB che ci può quindi permettere di lanciare una micro istanza da 0.02 $ l'ora.

Nella menù Instance Type scegliamo "Micro (t1.micro)" e selezioniamo Continue

[![](http://dema.tv/wp-content/uploads/2010/09/micro21.jpg)](http://dema.tv/wp-content/uploads/2010/09/micro21.jpg)

Lasciamo i parametri di default nella schermata successiva e scegliamo Continue

[![](http://dema.tv/wp-content/uploads/2010/09/default11.jpg)](http://dema.tv/wp-content/uploads/2010/09/default11.jpg)

A questo punto possiamo assegnare un tag alla istanza che stiamo per lanciare, per una migliore gestione della nostra infrastruttura EC2. Possiamo lasciare vuoto il campo se non desideriamo taggare la macchina virtuale che stiamo per lanciare.

[![](http://dema.tv/wp-content/uploads/2010/09/tag11.jpg)](http://dema.tv/wp-content/uploads/2010/09/tag11.jpg)

Dobbiamo creare ora una private key da usare successivamente per amministrare via ssh la nostra istanza remota. Potremmo usare una chiave già registrata, qualora avessimo già lanciato altre istanze EC2. Nel caso non ne avessimo, la creazione del keypair è molto semplice, basta un clic su "Create and download your key pair"

[![](http://dema.tv/wp-content/uploads/2010/09/key11.jpg)](http://dema.tv/wp-content/uploads/2010/09/key11.jpg)

Verrà avviato il download del file musicserver.pem che conserveremo e vedremo poi come usare in abbinamento a [Putty](http://the.earth.li/~sgtatham/putty/latest/x86/putty-0.60-installer.exe). La prossima schermata è relativa al settaggio del firewall.

[![](http://dema.tv/wp-content/uploads/2010/09/firewall21.jpg)](http://dema.tv/wp-content/uploads/2010/09/firewall21.jpg)

Come possiamo vedere esiste già un gruppo di default che comprende ovviamente l'apertura della porta 22 tcp per permettere le connessioni in ingresso per l'amministrazione. Possiamo scegliere il default group, successivamente dalla console di amministrazione di EC2 potremo aprire le porte che ci interessano per il nostro servizio.

Siamo arrivati, alla prossima schermata ci verrà fornito un riepilogo delle impostazioni e potremo, dopo aver controllato i parametri e verificati,  lanciare la nostra istanza EC2.

[![](http://dema.tv/wp-content/uploads/2010/09/finalize11.jpg)](http://dema.tv/wp-content/uploads/2010/09/finalize11.jpg)

Il boot della nostra istanza necessiterà di circa un paio di minuti (anche meno, sono stato largo) durante i quali possiamo nel frattempo preparare l'accesso via ssh. La chiave privata musicserver.pem, che abbiamo in precedenza scaricato, dovrà essere convertita tramite puttygen per usarla con putty.

Lanciamo PuttyGen ed scegliamo load, selezionando il file musicserver.pem.

[![](http://dema.tv/wp-content/uploads/2010/09/selectcert11.jpg)](http://dema.tv/wp-content/uploads/2010/09/selectcert11.jpg)

Il file importato verrà convertito in un file ppk che salveremo in una directory di nostra scelta e che useremo per autenticarci presso la nostra istanza remota.

[![](http://dema.tv/wp-content/uploads/2010/09/saveprivatekey11.jpg)](http://dema.tv/wp-content/uploads/2010/09/saveprivatekey11.jpg)

Nel frattempo, la nostra istanza dovrebbe essere già attivata e la prima cosa che dobbiamo fare è scoprire quale nome di DNS pubblico ha ottenuto.

Dal pannello di controllo di EC2 scegliamo "instances" e osserviamo i dati della nostra istanza.

[![](http://dema.tv/wp-content/uploads/2010/09/overview11.jpg)](http://dema.tv/wp-content/uploads/2010/09/overview11.jpg)

[](http://dema.tv/wp-content/uploads/2010/09/overview2.jpg)Di questa schermata riassuntiva ci interessa il valore di Public DNS, al quale dovremo far puntare in nostro client putty per collegarci via ssh alla console remota.

Lanciamo Putty e inseriamo nel campo "host name" il dns pubblico ottenuto dalla schermata precedente .

[![](http://dema.tv/wp-content/uploads/2010/09/sshtoinstance11.jpg)](http://dema.tv/wp-content/uploads/2010/09/sshtoinstance11.jpg)

Prima di connetterci dal menù ad albero di sinistra espandiamo la voce SSH e scegliamo Auth. Dalla finestra di dialogo risultante scegliamo browse e selezioniamo la nostra chiave musicserver.ppk salvata in precedenza.

[![](http://dema.tv/wp-content/uploads/2010/09/browseprivateppk11.jpg)](http://dema.tv/wp-content/uploads/2010/09/browseprivateppk11.jpg)

Siamo pronti per collegarci. Ecco la remote console ssh.

[![](http://dema.tv/wp-content/uploads/2010/09/root11.jpg)](http://dema.tv/wp-content/uploads/2010/09/root11.jpg)

Installiamo il server icecast2 e le sue dipendenze con yum install icecast

[![](http://dema.tv/wp-content/uploads/2010/09/installicecast11.jpg)](http://dema.tv/wp-content/uploads/2010/09/installicecast11.jpg)

E configuriamo i pochi parametri necessari del file /etc/icecast.xml
Sezione "limits"


    
    
    <limits>
            <clients>2000</clients>
            <sources>2</sources>
            <threadpool>5</threadpool>
            <queue-size>524288</queue-size>
            <client-timeout>30</client-timeout>
            <header-timeout>15</header-timeout>
            <source-timeout>10</source-timeout>
            <!-- same as burst-on-connect, but this allows for being more
                 specific on how much to burst. Most people won't need to
                 change from the default 64k. Applies to all mountpoints  -->
            <burst-size>65535</burst-size>
        </limits>



Il parametro che ci interessa è clients, settiamolo al valore massimo teorico che la banda fornita da Amazon ci può consentire, ossia 2000.

Sezione "authentication"



    
    <authentication>
            <!-- Sources log in with username 'source' -->
            <source-password>passsorgente</source-password>
            <!-- Relays log in username 'relay' -->
            <relay-password>passrelay</relay-password>
    
            <!-- Admin logs in with the username given below -->
            <admin-user>admin</admin-user>
            <admin-password>adminpassword</admin-password>
        </authentication>



Inseriamo le password per la sorgente: [Oddcast](http://compl.in/doPZTR) per Windows, sia standalone che plugin per [Winamp](http://www.winamp.com/), o [Darkice](http://compl.in/aKNX1o) per Linux o [MPD](http://compl.in/a1bAyE) o qualsiasi altro source client vogliate usare e la password eventuale per il relay presso altri server di streaming, nel caso la vostra trasmissione diventasse mainstream :-)

Infine username e  password di amministratore del server icecast per amministrare  via interfaccia web  le operazioni del server.

Tutto il resto possiamo lasciarlo di default, salvo intervenire in caso di problemi con del fine tuning per ottimizzare la trasmissione.

Una volta configurato il file icecast.xml con un /etc/init.d/icecast restart siamo quasi pronti per iniziare la nostra prima trasmissione.

Dobbiamo infatti aprire la porta 8000 sul firewall per permettere l'ingresso dello stream sorgente dal nmostro client e l'uscita dello streaming verso i nostri acoltatori.

Dalla console web di amazon EC2 scegliamo "security groups" ed apriamo tramite il pannello di controllo la porta 8000.
[![](http://dema.tv/wp-content/uploads/2010/09/security11.jpg)](http://dema.tv/wp-content/uploads/2010/09/security11.jpg)

Abbiamo terminato. Non ci resta che settare i parametri di collegamento del nostro source client verso il dns pubblico della nostra istanza EC2 e cominciare a trasmettere il nostro stream radio.

Con il browser possiamo aprire la URL del pannello d'amministrazione web.

[![](http://dema.tv/wp-content/uploads/2010/09/webpanel11.jpg)](http://dema.tv/wp-content/uploads/2010/09/webpanel11.jpg)

Con un click destro su "click to listen" e accorciando la URL con uno qualsiasi dei servizi di shortner disponibili (il mio preferito è bit.ly) potremo postare il link su Twitter o Facebook o sul nostro blog e cominciare la nostra trasmissione.

Il bello di questa soluzione è che le micro istanze sono basate su EBS e quindi i settaggi rimarranno scritti anche quando decideremo di fermare l'istanza stessa. Per questa istanza di Fedora è previsto un EBS da 15Gb che costa 2.25 us$ al mese e ci permetterà di non ripetere il setup ogni volta che lanceremo il server di streaming. Inoltre dovremo conteggiare 0.02 us$ all'ora per il tempo di run del server e la banda consumata nell'ordine di 0.15 us$ al Gb oltre il primo Gb trasferito.

L'importante è scegliere "stop instance" invece che "terminate instance", comando che termina completamente l'istanza, compreso il volume EBS.

Happy web radio con Amazon EC2 :-)
