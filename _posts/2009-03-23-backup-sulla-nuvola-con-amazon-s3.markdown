---
author: dema
comments: true
date: 2009-03-23 10:04:23+00:00
layout: post
slug: backup-sulla-nuvola-con-amazon-s3
title: 'Backup sulla nuvola con Amazon S3 '
wordpress_id: 692
categories:
- howto
tags:
- amazon
- cloud
- s3
- storage
- sync
---

Sabato sono stato al Workcamp di Parma . Abbiamo ragionato di lavoro e web in questi tempi disgraziati. A me è sembrato che fossimo tutti un pochino abbacchiati. Troppe bastonate tra capo e collo negli ultimi tempi ?

Ma questo post secondo le tradizioni a me care vuole essere un mini howto , ispirato da una richiesta di [Iron Mauro](http://www.ariadicrisi.it/) al sottoscritto su un buon servizio di backup online , esigenza scaturita dopo lo speech di [Nicola D'agostino](http://www.nicoladagostino.net/) che ha diffuso un po' di terrore tra i presenti in sala raccontando di disastri legati all'affidamento dei propri dati a servizi web.

Capiamoci , il backup online è pur sempre affidare i propri dati a società terze , ma è  un backup , quindi i dati risiedono anche nei nostri hard disk e si spera che Amazon riesca a rimanere in piedi nella bufera più a lungo della vita delle testine dei nostri hard disk .

Per prima cosa dobbiamo registrarci ai servizi di Amazon a [questo indirizzo](http://aws.amazon.com/s3/) . Dobbiamo inserire le nostre credenziali ed una carta di credito sulla quale verranno addebitati i consumi.

Una volta registrati , riceveremo le chiavi  che dovranno essere inserite nei programmi che sceglieremo per l'accesso al servizio. Non esiste un modo unico per accedere allo storage di S3 . Possiamo usare una miriade di programmi per accedere allo spazio , abbiamo veramente l'imbarazzo della scelta. Fra i programmi che ho provato ci sono : [Cyberduck](http://cyberduck.ch/) per (Mac OS) , [s3Browser](http://s3browser.com/) (win) e [s3fox](https://addons.mozilla.org/en-US/firefox/addon/3247) e [s3://](https://addons.mozilla.org/en-US/firefox/addon/6955) (multipiattaforma plugin per firefox)

Io ho usato uno script ruby per mettere in sync i file di backup che risiedono nel mio server di storage e il bucket che ho deputato a backup in S3.

Installiamo ruby e libopenssl-ruby

[sourcecode language='cpp']
apt-get install ruby libopenssl-ruby[/sourcecode]

Posizioniamoci in /opt e scarichiamo e decomprimiamo il set di script di [Greg](http://developer.amazonwebservices.com/connect/profile.jspa?userID=18616)

[sourcecode language='cpp']
wget http://s3.amazonaws.com/ServEdge_pub/s3sync/s3sync.tar.gz
tar zxvf s3sync.tar.gz[/sourcecode]

Procuriamoci i certificati per ssl . Per prima cosa creiamo una directory certs all'interno di /opt/s3sync

[sourcecode language='cpp']
cd /opt/s3sync/
mkdir certs [/sourcecode]

Installiamo i certificati

[sourcecode language='cpp']
wget http://mirbsd.mirsolutions.de/cvs.cgi/~checkout~/src/etc/ssl.certs.shar
sh ssl.certs.shar [/sourcecode]

A questo punto dobbiamo creare il bucket dentro il quale operare

[sourcecode language='cpp']
export AWS_ACCESS_KEY_ID=yourS3accesskey
export AWS_SECRET_ACCESS_KEY=yourS3secretkey
export SSL_CERT_DIR=/opt/s3sync/certs
/opt/s3sync/s3cmd.rb createbucket mybackup EU [/sourcecode]

Abbiamo creato il bucket mybackup sulla nuvola di Amazon. Ora vediamo di popolarlo con i nostri dati e metterlo in sincronia perfetta con il nostro volume locale.

Posizioniamoci sempre in /opt/s3sync e creiamo due script con il nostro editor preferito :

Upload.sh :

[sourcecode language='cpp']
#!/bin/bash
cd /opt/s3sync/
export AWS_ACCESS_KEY_ID=yourS3accesskey
export AWS_SECRET_ACCESS_KEY=yourS3secretkey
export SSL_CERT_DIR=/opt/s3sync/certs
/opt/s3sync.rb -r -v --ssl --delete /path-directory-to-backup/ mybackup:mydirectory-to-backup/
# Aggiungete linee multiple per ogni directory che vorrete copiare [/sourcecode]

Download.sh :

[sourcecode language='cpp']

#!/bin/bash
cd /opt/s3sync/
export AWS_ACCESS_KEY_ID=yourS3accesskey
export AWS_SECRET_ACCESS_KEY=yourS3secretkey
export SSL_CERT_DIR=/opt/s3sync/certs
/opt/s3sync/s3sync.rb -r -v --ssl --delete mybackup:mydirectory-to-backup/ /path-directory-to-recover
# Aggiungete linee multiple per ogni directory che vorrete recuperare[/sourcecode]

Dobbiamo dare i giusti permessi ai due script appena creati con chmod 700 /opt/s3sync/*.sh

Ci siamo , possiamo ora lanciare lo script di upload con il comando

[sourcecode language='cpp']
/opt/s3sync/Upload.sh[/sourcecode]

Automatizziamo il processo di backup

[sourcecode language='cpp']
crontab -e
0 2 * * *  /opt/s3sync/Upload.sh[/sourcecode]

Abbiamo così creato il nostro bel volume sulla nuvola di Amazon S3 in sincronia perfetta con il nostro hard disk locale. Possiamo inoltre avvalerci degli strumenti di cui ho fatto menzione prima (ad esempio l'ottimo S3fox)  per sfogliare il volume remoto su tutti i nostri computer client , rendere pubblici per il dowload determinati files  o addirittura usare delle signatures temporanee per il dowload di corrispondenti terzi.

Tutto rose e fiori e soavi canti di cherubini quindi ?

No , ecco le dolenti note.

Tempo. Le nostre adsl , anche business, hanno un upload rate medio di circa 200 Kilobit/s  . Questo significa che avremo un rating di circa 1 GigaByte all'ora . Se abbiamo un archivio corposo rischiamo di metterci un mese per trasferire tutto sulla nuvola , a meno di consegnare il nostro hard disk direttamente nelle mani di [Burunotsui San](http://ubuntista.it) e chiedergli di farci la cortesia di copiarli lui quando va da Amazon  :)

Denaro. Il listino prezzi di Amazon non è alla portata di tutti , specialmente di singoli privati . Capiamoci , il servizio è di prim'ordine , ma eseguendo il famoso conto della serva per 10 GigaByte di dati si verrebbe a spendere :

€ 0.76 per il trasferimento
€ 1.38 al mese x 12 mesi = € 16.56
totale = € 17.32 per tranches di 10 GB per un costo annuo  al GB di € 1.73

In più vanno sommate € 0.13 al GigaByte in eventuale download e €0.01 per 1000 PUT , COPY, POST, o LIST richiesta e sempre € 0.01 per 10000 richieste GET 

Comparato con un nas di rete in raid 1 da 1TB abbiamo : 

NAS € 319.00 one shot 

Amazon S3 € 1703.00 annue circa.

E' una comparazione stupida , me ne rendo conto , ma per chi cerca semplicemente un backup amatoriale senza pretese la nuvola è ancora un po' troppo alta in cielo.

Credits per Howto su Linux : [John Eberly ](http://blog.eberly.org/2006/10/09/how-automate-your-backup-to-amazon-s3-using-s3sync/)

