---
author: dema
comments: true
date: 2009-02-17 10:27:49+00:00
layout: post
slug: usare-zimbra-con-google-apps
title: 'Usare Zimbra con Google Apps '
wordpress_id: 653
categories:
- howto
- tutorial
tags:
- google
- google apps
- google for domain
- mail
- posta
- smtp
- workgroup
- zimbra
---

Di recente mi sono ritrovato ad implementare un server di collaboration suite per un piccolo studio di architetti.

La scelta è ricaduta sulla versione opensource della suite di collaborazione [Zimbra](http://www.zimbra.com/community/downloads.html).

Inoltre , scartata l'ipotesi di _hostare _la posta su una connessione dsl business con IP fisso , ho deciso di affidare il servizio a Google grazie all' ormai famoso Google Apps conosciuto anche come Google for Domains.

Zimbra però nasce per essere esposto come host di posta pubblico sulla rete e non è concepito per lavorare dietro NAT . Grazie ad alcuni trucchetti possiamo però farlo funzionare egregiamente anche in uno scenario di piccolo gruppo di lavoro dietro firewall.

Vediamo il processo per passi.

<!-- more -->Zimbra dovrà essere  installato in una macchina non pubblicata su internet . Per fare questo dobbiamo ingannare ancora prima di iniziare le routines automatiche dello script di setup che controllano i files host e i record MX del resolver principale. Usando dnsmasq è sufficiente settare i parametri mx-host e address  del file di configurazione.

[sourcecode language='cpp']
address=/yourdomain.com/192.168.0.1
mx-host=mail.yourdomain.com,yourdomain.com,50[/sourcecode]

Per verificare che tutto sia in ordine possiamo controllare con

[sourcecode language='cpp']
dig @localhost yourdomain.com mx
;; ANSWER SECTION:
yourdomain.com.         43200   IN      MX      50 mail.yourdomain.com.
[/sourcecode]

A questo punto si puo' cominciare con l'installazione di Zimbra. Il processo è automatico e senza intoppi.

Una volta terminata l'installazione dobbiamo preoccuparci del file main.cf di postfix che risiede in _/opt/zimbra/postfix/conf/main.cf_. Editiamo con il nostro editor preferito , logghiamoci come utente zimbra (sudo su ; su - zimbra ) ed aggiungiamo le seguenti linee :

[sourcecode language='cpp']
smtp_sender_dependent_authentication = yes
sender_dependent_relayhost_maps = hash:/opt/zimbra/conf/sender_relay
smtp_sasl_auth_enable = yes
smtp_sasl_password_maps = hash:/opt/zimbra/conf/sasl_passwd
smtp_use_tls = yes
smtp_sasl_security_options = noanonymous
relayhost = smtp.gmail.com:587[/sourcecode]

Così facendo , abbiamo istruito il nostro smtp ad usare :



	
  1. un smtp esterno _smtp.gmail.com_ sulla porta 587.

	
  2. un'autentifica diversa a seconda  del mittente _smtp_sender_dependent_authentication = yes_.

	
  3. un file _sender_relay_ dal quale attingere le regole per inviare al corretto smtp a seconda del mittente.

	
  4. una mappatura delle password corrispondenti al mittente _smtp_sasl_password_maps = hash:/opt/zimbra/conf/sasl_passwd_


Dobbiamo ora creare i file di configurazione che abbiamo specificato nel file main.cf di postfix.

Posizioniamoci su _/opt/zimbra/conf_ e sempre come utente zimbra creiamo un nuovo file di configurazione :

[sourcecode language='cpp']
zimbra@postman:~/conf$ vim sender_relay
anton@yourdomain.com      smtp.gmail.com:587
massimo@yourdomain.com    smtp.gmail.com:587
sempronio@yourdomain.com  smtp.gmail.com:587 [/sourcecode]

Creiamo  il file Db per postfix con il comando

[sourcecode language='cpp']
postmap hash:/opt/zimbra/conf/sender_relay[/sourcecode]

Ora dobbiamo creare il file sasl_passwd che dovrà contenere le nostre credenziali . Sempre come utente zimbra diamo :

[sourcecode language='cpp']
vim /opt/zimbra/conf/sasl_passwd
anton@yourdomain.com     anton@yourdomain.com:YOURPASSWD1
massimo@yourdomain.com     massimo@yourdomain.com:YOURPASSWD2
sempronio@yourdomain.com  sempronio@yourdomain.com:YOURPASSWD3[/sourcecode]

Cambiamo i permessi con chmod 640 sasl_passwd e creaiamo il DB per postfix :

[sourcecode language='cpp']
postmap hash:/opt/zimbra/conf/sasl_passwd[/sourcecode]

Possiamo a questo punto dare un refresh a postfix con il comando

[sourcecode language='cpp']
postfix reload[/sourcecode]

e dovremmo essere a posto lato SMTP.

Il nostro environment però è completamente appoggiato a Google Apps , quindi abbiamo bisogno di un meccanismo per recuperare la posta via POP dai server di Google.

Ci appoggeremo a fetchmail lanciato ad intervalli di tempo regolari.

Creiamo il file fetchmail.conf su /opt/zimbra/conf

[sourcecode language='cpp']
poll pop.gmail.com timeout 60 proto POP3
user "anton@yourdomain.com"
pass PASSWD1
is anton@yourdomain.com here
fetchall
ssl[/sourcecode]

Separiamo con una linea vuota ogni entrata per ogni utente di posta remota e cambiamo i permessi del file con _chmod 600 fetchmail.conf_ .

Impostiamo lo schedulatore in modo che controlli la posta ogni 5 minuti

[sourcecode language='cpp']
crontab -e
# m h  dom mon dow   command
*/5  *  * * *  /usr/bin/fetchmail -s -f /opt/zimbra/conf/fetchmail.conf >/dev/null 2>&1[/sourcecode]

Ci siamo . E' ora sufficiente riavviare il server e tutto dovrebbe funzionare.

E' ovvio  che questo accrocchio è possibile in gruppi di lavoro ristretti  in quanto implica una personalizzazione manuale dei files di configurazione e vanifica quindi i benifici della semplice interfaccia di amministrazione di Zimbra che permette di aggiungere e rimuovere liste di utenti con semplicità.

Il setup alternativo che si puo' trovare nel [wiki della comunità zimbra](http://wiki.zimbra.com/index.php?title=Outgoing_SMTP_Authentication) infatti ha come unica soluzione per il relay smtp un sigolo account remoto . Questo setup da un lato molto più immediato però male si adatta con Google Apps in quanto le mail in uscita riportano l'indirizzo del mittente dell'account smtp .

Anche se si puo' facilmente ovviare creando un indirizzo catch all su Google apps del tipo invio_azienda@yourdomain.com ed agire sui campi reply_to del client di posta elettronica ho trovato la soluzione che ho appena descritto decisamente più elegante.
