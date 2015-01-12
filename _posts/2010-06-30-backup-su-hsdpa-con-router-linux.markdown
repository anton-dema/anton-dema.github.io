---
author: dema
comments: true
date: 2010-06-30 09:15:31+00:00
layout: post
slug: backup-su-hsdpa-con-router-linux
title: Backup su HSDPA con router Linux
wordpress_id: 1157
categories:
- howto
tags:
- 3g
- adsl
- gateway
- hdsl
- hsdpa
- linux
- router
- script
---

La settimana scorsa [Stefano](http://www.sottoli.it/) ha avuto un down tremendo della HDSL, più di 72 ore, quasi insopportabili per una azienda al giorno d'oggi. Per questo motivo mi ha chiesto di aggiungere al firewall/router che già gestisce le policy di rete, un backup su HSDPA automatico che attivi la connessione su 3G non appena il link ADSL cade.

Ho cercato in rete se esisteva già qualche setup, ma ho trovato solo una guida per zeroshell e alcune discussioni su forum che però non erano pienamente esaustive.

Partendo quindi dal semplice concetto che per effettuare uno switch al volo occorre testare il default gateway e nel caso fallisca attivare il gateway su rete cellulare e il masquerading  attraverso esso, ho buttato giù questo accrocchio immondo in shell script.

Il meccanismo si basa su quattro script chiamati **pingeth1, killpingeth1, pingppp0, killpingppp0.**

Lo script principale è **pingeth1 **


    #/bin/bash
    ping -I eth1 -c3 151.99.29.203 > result.tst;
    ALERT=`grep -c "3 received" result.tst`
    if [ $ALERT = 1 ] ; then
    echo "ok"
    else
    wvdial &
    sleep 10
    /usr/local/bin/killpingeth1
    /sbin/shorewall clear
    /sbin/iptables  -t  nat  -A  POSTROUTING  -o  ppp0 -j  MASQUERADE
    /sbin/route add default gw 10.64.64.64 dev ppp0
    fi
    rm result.tst
    exit 0


Esaminiamo nel dettaglio quanto sopra:

Vengono lanciati 3 ping attraverso eth1 (porta wan collegata al router adsl) verso il nodo p-t-p della connessione (151.99.29.203) e il risultato viene scritto nel file temporaneo result.tst

Viene introdotta la variabile ALERT ottenuta greppando e contando l'occorrenza del termine "3 received" dal file temporaneo result.tst.

Se il valore di ALERT è uguale ad 1, lo script si limita a stampare la scritta OK, se il valore di ALERT è diverso da 1 viene nell'ordine:

1)lanciato wvdial in backgroud
2)inserito un sleep di 10 secondi per permettere a wvdial di alzare correttamente la connessione
3)invocato lo script killpingeth1 (che vedremo più sotto)
4)resettate le regole del firewall in essere
5)inserita una nuova regola di iptables per il masquerading attraverso l'interfaccia ppp0
6)aggiunta la nuova default gateway attraverso l'interfaccia ppp0

Viene infine rimosso il file temporaneo result.tst.

Pingeth1 va eseguito ogni minuto da cron, per cui occorre creare un nuovo cronjob


    crontab -e
    */1 * * * * /usr/local/bin/pingeth1 >/dev/null


Esaminiamo ora lo script **killpingeth1**

    #/bin/bash
    crontab -l >CRON_TEMP
    awk '$0!~/pingeth1/ { print $0 }' CRON_TEMP >CRON_NEW
    echo "*/1 * * * * /root/pingppp0 >/dev/null" >>CRON_NEW
    crontab CRON_NEW
    /etc/init.d/cron restart
    rm CRON_TEMP;
    rm CRON_NEW;
    exit 0

La funzione primaria di killpingeth1 è quella di cambiare lo schedulatore, rimuovendo la programmazione di pingeth1 ed inserendo la nuova programmazione di pingppp0 (che vedremo più sotto).

Invocato unicamente in caso di mancata risposta del gateway primario killpingeth1 esegue i seguenti passaggi:

1)Elenca il contenuto del crontab e lo scrive in un file temporaneo CRON_TEMP
2)verifica la presenza della stringa pingeth1 sul file temporaneo CRON_TEMP e se presente la elimina scrivendo un nuovo file CRON_NEW
3)appende alla fine del file CRON_NEW la nuova schedula di pingppp0
4)installa il nuovo cronjob
5)rimuove i file temporanei CRON_NEW e CRON_TEMP

Da questo momento la connessione è instradata attraverso ppp0, però con un monitor sempre attivo sull'interfaccia principale, in modo che, se la connessione dovesse essere ripristinata, si ritorni in modo automatico alle condizioni di lavoro predefinite.

Il controllo dell'interfaccia eth1 viene demandato allo script pingppp0 che abbiamo inserito nel nuovo cronjob.

Vediamolo in dettaglio

    #/bin/bash
    /sbin/route del default gw 192.168.0.1 dev eth1
    /sbin/route add default gw 192.168.0.1 dev eth1
    ping -I eth1 -c 3 151.99.29.203 > result.tst;
    ALERT=`grep -c "3 received" result.tst`
    if [ $ALERT = 1 ] ; then
    killall wvdial
    /usr/local/bin/killpingppp0
    /sbin/shorewall restart
    else
    echo "ancora su linea backup"
    /sbin/route del default gw 192.168.0.1 dev eth1
    fi
    rm result.tst
    exit 0

Lo script pingppp0 viene eseguito ogni minuto e compie la seguente routine:

Cancella ed aggiunge la default gateway su eth1 (workaround orrendo, ma senza questo passaggio il ping di controllo non riusciva a passare dall'interfaccia eth1, se qualcuno ha una soluzione può integrare e migliorare questo passaggio)

Esegue i soliti 3 ping verso il nodo p-t-p della connessione (151.99.29.203) e il risultato viene scritto nel file temporaneo result.tst

Introduce la variabile ALERT ottenuta greppando e contando l'occorrenza del termine "3 received" dal file temporaneo result.tst.

Se il valore di ALERT è uguale ad 1 viene nell'ordine:

1)abbattuto il processo wvdial facendo così cadere la connessione 3G
2)invocato lo script killpingppp0 ( che vedremo più sotto)
3)fatto ripartire il firewall

Se il valore di ALERT è diverso da 1 si limita a stampare la stringa "ancora su linea backup" e cancella la default gateway su eth1, rimuovendo come ultimo comando il file temporaneo result.tst.

Rimane da prendere in esame l'ultimo script killpingppp0 che si prende cura di ripristinare il corretto cronjob su pingeth1

Ecco lo script:

    #/bin/bash
    crontab -l >CRON_TEMP
    awk '$0!~/pingppp0/ { print $0 }' CRON_TEMP >CRON_NEW
    echo "*/1 * * * * /root/pingeth1>/dev/null" >>CRON_NEW
    crontab CRON_NEW
    /etc/init.d/cron restart
    rm CRON_TEMP
    rm CRON_NEW
    exit 0

Lo script è del tutto analogo a killpingeth1 e quindi esegue nell'ordine i seguenti passaggi:

1)Elenca il contenuto del crontab e lo scrive in un file temporaneo CRON_TEMP
2)verifica la presenza della stringa pingppp0 sul file temporaneo CRON_TEMP e se presente la elimina scrivendo un nuovo file CRON_NEW
3)appende alla fine del file CRON_NEW la nuova schedula di pingeth1
4)installa il nuovo cronjob
5)rimuove i file temporanei CRON_NEW e CRON_TEMP

Questa catena di script si intesta in un loop che switcha in maniera accettabilmente veloce la connessione tra la linea adsl e 3G, senza nessun intervento manuale da parte dell'amministratore di sistema.

Vista la ruvidità del procedimento non è possibile avere tutte le funzionalità avanzate del router come transparent proxy e vpn, ma credo che il compromesso per essere sempre online anche dopo la caduta del link primario sia accettabile.

I vostri commenti, insulti e migliorie sono come al solito più che apprezzati.
