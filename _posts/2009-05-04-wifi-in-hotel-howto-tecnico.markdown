---
author: dema
comments: true
date: 2009-05-04 20:46:27+00:00
layout: post
slug: wifi-in-hotel-howto-tecnico
title: 'Wifi in Hotel : howto tecnico '
wordpress_id: 732
categories:
- howto
tags:
- apache
- chilli
- coova-chilli
- freeradius
- hotel
- hotspot
- linux
- mysql
- phpmyprepaid
- router
- wifi
---

Come promesso un paio di settimane fa , ho pronto l'howto secco e come al solito step by step per approntare un sistema di hotspot fai da te per un albergo , una struttura comunitaria , un centro uffici e chi più ne ha più ne metta.

Come al solito è un pippone tecnico , quindi consiglio la lettura solo a chi è veramente interessato

<!-- more -->

[![](http://dema.tv/wp-content/uploads/2009/05/hotspot.jpg)](http://dema.tv/wp-content/uploads/2009/05/hotspot.jpg)

Per una migliore visualizzazione clicca sulla foto

**Requisiti :**



	
  1. una macchina 386 in disuso

	
  2. tre schede di rete

	
  3. due switch da n porte per quanti client e quanti access point intendete collegare

	
  4. n access point a seconda dell'area da coprire

	
  5. matassa di cavo di rete cat5 per cablare dallo switch fino agli access point

	
  6. eventuali moduli POE (non trattati in questo howto)


Per prima cosa procuriamoci il computer che trasformeremo in router , firewall , server radius/mysql e gestore dei vouchers prepagati.

Procuriamoci un disco di installazione di ubuntu server . Questo howto è stato compilato prendendo in esame la 8.10 , ma credo che non ci siano particolari differenze se usiamo l'ultima 9.04.

Installiamo ubuntu server , scegliendo la configurazione LAMP in modo da avere al termine , già funzionanti il server web Apache , mysql e PHP5. E' buona pratica scegliere anche il demone ssh per poter amministrare l'installazione tramite un client di rete. Per installare ssh
_sudo_ a_ptitude install ssh_

**Preparativi post installazione:**

Appena terminata l'installazione occupiamoci delle interfacce di rete che andremo a configurare come segue :

eth0 => WAN
eth1 => rete locale
eth2 => hotspot

Ecco il file _/etc/network/interfaces_

[sourcecode language='cpp']
# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
iface eth0 inet static
address 192.168.1.2
netmask 255.255.255.0
network 192.168.1.0
broadcast 192.168.1.255
gateway 192.168.1.1

auto eth1
iface eth1 inet static
address 192.168.0.1
netmask 255.255.255.0
broadcast 192.168.0.255
network 192.168.0.0
[/sourcecode]

Abbiamo bisogno di un server dhcp dal momento che scollegando il router internet dalla lan i client si ritrovano privi di indirizzo IP .

Installiamo l'helper dns e server dhcp dnsmasq con
_sudo aptitude install dnsmasq
_ed editiamo il file di configurazione _/etc/dnsmasq.conf _come segue :

[sourcecode language='cpp']
server=208.67.222.222 #aggiungere questa riga che indica il server dns esterno a cui appoggiarsi
dhcp-range=eth1,192.168.0.50,192.168.0.150,12h
[/sourcecode]

Editiamo il file /etc/resolv.conf

[sourcecode language='cpp']
nameserver 127.0.0.1
[/sourcecode]

Facciamo ripartire dnsmasq con _
sudo /etc/init.d/dnsmasq restart_
e proviamo a pingare un host esterno ( google?) e diamo temporaneamente accesso ad internet alla rete locale con
_sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE_

A questo punto facciamo partire il demone ssh con
_sudo_ _/etc/init.d/ssh restart _
Colleghiamo lo switch1 alla nostra rete locale e all'interfaccia eth1 e dovremo essere in grado sia di uscire su internet che di collegarci in ssh all'host 192.168.0.1.

Se qualcosa andasse storto fate ripartire tutti i demoni sopra menzionati
_sudo /etc/init.d/networking restart
sudo /etc/init.d/dnsmasq restart
sudo /etc/init.d/ssh restart _
e poi da ultimo
_ sudo_ _iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE_

**Configurazione Apache:**

Spostiamoci ora su un host della rete locale con ip 192.168.0.0/24 e colleghiamoci via ssh all'indirizzo 192.168.0.1 per una migliore gestione del nostro router .

Per prima cosa verifichiamo che Apache ci risponda sulla porta 80 , facendo puntare il nostro browser di fiducia all'indirizzo http://192.168.0.1 .

Per il nostro setup però , avremo bisogno di attivare ssl in Apache.

Entriamo nella directory /etc/apache2/mods-available e verifichiamo l'esistenza di ssl con
_sudo ls | grep ssl_
Se tutto è a posto dovremmo avere ssl.conf e ssl.load come risposta al comando precedente.

Attiviamo i moduli ssl con
_sudo a2enmod ssl_
_sudo_ _/etc/init.d/apache2 force-reload_

Generiamo certificati e chiavi per il server Apache.

[sourcecode language='cpp']
cd /etc/apache2
sudo openssl genrsa -des3 -out server.key 1024
sudo openssl req -new -key server.key -out server.csr[/sourcecode]

Ci verranno richieste la nostra località di lavoro , una challenge password etc etc. Completiamo  e rispondiamo ai form nella maniera che riteniamo più opportuna.

Per finire generiamo ed installiamo il certificato vero e proprio

[sourcecode language='cpp']
sudo openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
sudo cp server.crt /etc/ssl/certs
sudo cp server.key /etc/ssl/private[/sourcecode]

Ricordiamo la password che abbiamo inserito quando abbiamo generato la chiave , ci verrà richiesta per firmare il certificato e al primo riavvio di Apache.

A questo punto dobbiamo completare la configurazione in /etc/apache2/sites-available.

Con il nostro editor di testo preferito diamo un'occhiata al file default-ssl che dovrebbe essere già completamente configurato. Accertimoci che queste voci siano decommentate.

[sourcecode language='cpp']
sudo nano default-ssl
SSLEngine on
SSLOptions +FakeBasicAuth +ExportCertData +StrictRequire
SLCertificateFile    /etc/ssl/certs/server.crt
SSLCertificateKeyFile /etc/ssl/private/server.key[/sourcecode]

A questo punto ci basta solo attivare il default SSL site con
_sudo a2ensite default-ssl_
e far ripartire Apache con
_sudo /etc/init.d/apace2 force-reload_

Verifichiamo il corretto funzionamento , facendo puntare il nostro browser a https://192.168.0.1.

Il browser ci avviserà che il sito non è attendibile , ma forzando l'acquisizione del certificato aggireremo il problema e ai prossimi collegamenti non verremo più disturbati da questi avvisi.

Per fare in modo che lo script di init di Apache non ci richieda sempre la password del certificato dobbiamo rimuovere la crittatura della chiave RSA.

Posizioniamoci in /etc/ssl/private e diamo

[sourcecode language='cpp']
sudo mv server.key server.key.temp
openssl rsa -in server.key.temp -out server.key[/sourcecode]

**Installazione Freeradius, i** **preliminari**:

Sfortunatamente il nostro sistema di hotspot a voucher non funziona con freeradius 2.x , o meglio , il gestore degli accessi a tempo phpmyprepaid non ha ancora rilasciato la versione per le nuove tabelle di freeradius 2.x.

Le moderne Ubuntu dalla 8.10 in poi hanno adottato giustamente la nuova versione di freeradius quindi non potremo installare il demone semplicemente con _
sudo aptitude install freeradius_

Niente paura , preleviamo i sorgenti di freeradius e compiliamoli.

Per prima cosa scarichiamo la distribuzione sorgente all'indirizzo [ftp://ftp.freeradius.org/pub/radius/freeradius-1.1.7.tar.gz](ftp://ftp.freeradius.org/pub/radius/freeradius-1.1.7.tar.gz) ( ho anche approntato un [mirror](http://dema.tv/wp-content/uploads/2009/05/freeradius-1.1.7.tar.gz) nel caso remoto freeradius decidesse di non rendere più disponibile la versione 1.1.7 in quanto non più mantenuta)

Otteniamo i privilegi di superutente con sudo su , posizioniamoci nella nostra directory root con cd e poi diamo
_wget ftp://ftp.freeradius.org/pub/radius/freeradius-1.1.7.tar.gz_

Esplodiamo l'archivio con
_tar zxvf freeradius-1.1.7.tar.gz_

A questo punto dobbiamo compilare i sorgenti a la debian way ecco i passi :

[sourcecode language='cpp']
aptitude install debhelper dpatch autotools-dev libtool libltdl3-dev libpam0g-dev libgdbm-dev libldap2-dev libsasl2-dev libiodbc2-dev libkrb5-dev libperl-dev snmp libsnmp9-dev libsnmp5-dev libsnmp4.2-dev libpq-dev postgresql-dev libssl-dev

aptitude install build-essential linux-headers

cd /root/freeradius-1.1.7

dpkg-buildpackage -rfakeroot[/sourcecode]

Questo processo creerà i pacchetti .deb di cui avremo bisogno per installare freeradius 1.1.7 nel nostro server.

Al termine di dpkg-buildpackage , troveremo in /root i seguenti pacchetti .deb : freeradius_1.1.7-0_i386.deb
freeradius-dialupadmin_1.1.7-0_all.deb
freeradius-iodbc_1.1.7-0_i386.deb
freeradius-krb5_1.1.7-0_i386.deb
freeradius-ldap_1.1.7-0_i386.deb
freeradius-mysql_1.1.7-0_i386.deb
freeradius-postgresql_1.1.7-0_i386.deb

Troppa grazia Sant'Antonio ! A noi servono solo i pacchetti freeradius_1.1.7-0_i386.deb e freeradius-mysql_1.1.7-0_i386.deb.

Installiamoli con _
dpkg -i freeradius_1.1.7-0_i386.deb_
_dpkg -i freeradius-mysql_1.1.7-0_i386.deb_

Avremmo finito con l'installazione preliminare di freeradius , lo riprenderemo in mano più tardi , una volta installato coova-chilli e phpmyprepaid e sistemato i parametri di mysql.

Per evitare che un aggiornamento di Ubuntu ci rimpiazzi la versione 1.1.7 di freeradius dobbiamo bloccare la versione del pacchetto in apt .

Andiamo in /etc/apt e creiamo un file di testo chiamato preferences e copiamo questo contenuto :

[sourcecode language='cpp']
Package: freeradius
Pin:version 1.1.7-0
Pin-Priority: 1001

Package: freeradius-mysql
Pin:version 1.1.7-0
Pin-Priority: 1001[/sourcecode]

In questo modo saremo sicuri che anche se aggiorneremo il nostro server non avremo problemi di rimpiazzamento di freeradius in versione 2.x :)

**Coova-chilli:**

Siamo giunti all'installazione di coova-chilli . Questo demone ci fornirà il captive portal , una pagina web che ci permetterà di inserire le nostre credenziali per il login e autenticarci presso freeradius e in buona sostanza fornirci l'accesso ad internet.

Sempre come superutenti posizioniamoci nella directory /root e scarichiamo il pacchetto precompilato per debian
_wget http://ap.coova.org/chilli/coova-chilli_1.0.13-1_i386.deb_
_dpkg -i coova-chilli_1.0.13-1_i386.deb_

In questo modo abbiamo immediatamente il demone chillispot per l'autentifica pronto per essere lanciato con il comando _
/etc/init.d/chilli start_

Facciamo il tuning del sistema . Posizioniamoci in /etc ed rinominiamo il file di configurazione chilli.conf  in chilli.conf.orig. Sostituiamo completamente il contenuto del file di configurazione originale con il seguente contenuto:

[sourcecode language='cpp']
dns1 192.168.182.1
domain mydomain
dhcpif eth2
radiusserver1 127.0.0.1
radiusserver2 127.0.0.1
radiussecret myradiussecret #vedremo più tardi dove inserire la passwdord di radius
uamallowed www.google.com,gmail.com,mail.google.com,1.gravatar.com,s3.wordpress.com,demablog.wordpress.com,192.168.182.1
uamserver https://192.168.182.1/cgi-bin/hotspotlogin.cgi
uamsecret demahotspot[/sourcecode]

Per ultima cosa scarichiamo lo script cgi che ci permetterà il login tramite pagina web

[sourcecode language='cpp']
cd /usr/lib/cgi-bin
wget http://files.demaitalia.com/radius/hotspotlogin.cgi
chmod +x hotspotlogin.cgi[/sourcecode]

Chilli è a posto ora , lasciamolo da una parte per ora ed andiamo ad interessarci di phpmyprepaid

**Phpmyprepaid**:

Dobbiamo installare uno script php per la gestione delle login del nostro captive portal. Mica vorremo riempire le tabelle di mysql a mano !!!

Procuriamoci l'ultima versione di [phpmyprepaid](http://sourceforge.net/projects/phpmyprepaid/) .

Ecco lo step by step

[sourcecode language='cpp']
sudo su
password:
cd
wget http://dfn.dl.sourceforge.net/sourceforge/phpmyprepaid/phpmyprepaidRC3.tgz
tar xvf phpmyprepaidRC3.tgz
cp phpmyprepaid /var/www/prepaid
chown -R www-data:www-data /var/www/prepaid
[/sourcecode]

A questo punto colleghiamoci con il browser all'indirizzo http://192.168.0.1

Seguiamo passo passo questi screenshots:

[gallery link="file" columns="2" orderby="title"]

Ora giusto un paio di chiarimenti . Nelle schermate qui sopra non ci sono bisogno di spiegazioni . Gli unici campi che possono generare confusione sono :



	
  1. schermata 3  php memory limit:
Ci si puo' presentare un errore (scritta in rosso) dicendo che abbiamo settato il limite di memoria nei nostri settaggi di PHP. Per correggere editiamo il file /etc/php5/apache2/php.ini , cerchiamo il parametro memory_limit e portiamolo a 16M (memory_limit = 16M)

	
  2. schermata 5 parametri radius:
Controlliamo per bene i path che ci vengono proposti di default e cambiamoli se necessario. Una nota speciale per Freeradius secret. Finora non abbiamo toccato nessun file di configurazione di /etc/freeradius e la secret di default di freeradius è testing123. E' una buona idea cambiarla in /etc/freeradius/clients.conf (lo vedremo anche più tardi)


	
  3. schermata 6 User interface administration:
Inseriamo i valori che desideriamo per l'accesso dell'amministratore dell'interfaccia di phpmyprepaid.

	
  4. schermata 7 database configuration:
Quando abbiamo installato Ubuntu Server ed abbiamo scelto la configurazione LAMP ad un certo punto ci è stata richiesta una password di amministratore per il database mysql ricordate ? Ebbene quella è la password da inserire nei campi di questa schermata.
Nel caso non vi ricordiate più la password inserita
_sudo dpkg-reconfigure mysql-server-5.0_


una volta installato e settato il tutto facciamo pulizia con
_sudo_ _rm -R /var/www/prepaid/www/install_

Possiamo già creare i nostri coupons e stamparli . Questo è il risultato

![](http://files.demaitalia.com/radius/Immagine 12.png)

Verifichiamo che le tabelle del database vengano create correttamente

[sourcecode language='cpp']
mysql -uroot -p phpmyprepaid
Enter password:
mysql>select * from radcheck;
+----+----------+-------------------+----+---------------------------------------------+
| id | UserName | Attribute         | op | Value                                       |
+----+----------+-------------------+----+---------------------------------------------+
|  1 | lztudl   | User-Password     | := | wmf                                         |
|  2 | lztudl   | Max-All-Session   | := | 86400                                       |
|  3 | lztudl   | WISPr-Location-ID | := | isocc=IT,cc=39,ac=,network=chiavari/myhotel |
|  4 | lztudl   | Expiration        | := | 5 May 2009 18:11:45                         |
+----+----------+-------------------+----+---------------------------------------------+
4 rows in set (0.00 sec)[/sourcecode]

Evviva , ci siamo quasi :)

**Configurazione Freeradius:**

I file di configurazione della distribuzione di freeradius sfortunatamente non sono così semplici da configurare . Potrei dirvi di cambiare questo e quel  parametro in authorize , instantiate e così via.

Ho preferito rendere disponibili i file di configurazione già pronti , in modo che li possiate scaricare e mettere direttamente nella vostra _/etc/freeradius_.

[sourcecode language='cpp']

cd /etc/freeradius
wget http://files.demaitalia.com/radius/sql.conf
wget http://files.demaitalia.com/radius/radiusd.conf
[/sourcecode]

Editiamo il file /etc/freeradius/clients.conf e cambiamo la default secrect testing123 in una password che andremo ad inserire anche nel valore radiussecret del file chilli.conf

Editiamo il file sql.conf appena scaricato e cambiamo le credenziali di accesso al database

[sourcecode language='cpp']
sql {
 # Database type
 # Current supported are: rlm_sql_mysql, rlm_sql_postgresql,
 # rlm_sql_iodbc, rlm_sql_oracle, rlm_sql_unixodbc, rlm_sql_freetds
 driver = "rlm_sql_mysql"

 # Connect info
 server = "localhost"
 login = root
 password = MYPASS

 # Database table configuration
 radius_db = phpmyprepaid[/sourcecode]

Ultimo tocco editare il file /etc/freeradius/dictionary e inserire la seguente linea
ATTRIBUTE       Max-All-Session                         3000    integer

**Firewalling**:

Il fido shorewall si prenderà cura del firewalling .

[sourcecode language='cpp']
sudo aptitude install shorewall
sudo cp /usr/share/doc/shorewall-common/examples/three-interfaces/* /etc/shorewall
sudo cd /etc/shorewall[/sourcecode]

Editiamo i seguenti files :



	
  1. /etc/shorewall/interfaces  [sourcecode language='cpp']
###############################################################################
#ZONE   INTERFACE       BROADCAST       OPTIONS
net     eth0            detect          dhcp,tcpflags,routefilter,nosmurfs,logmartians
hot     tun0            detect          dhcp
loc     eth1            detect          dhcp
#LAST LINE -- ADD YOUR ENTRIES BEFORE THIS ONE -- DO NOT REMOVE[/sourcecode]

	
  2. /etc/shorewall/masq  [sourcecode language='cpp']
###############################################################################
#INTERFACE              SOURCE          ADDRESS         PROTO   PORT(S) IPSEC   MARK
eth0                    tun0
eth0                    eth1
#LAST LINE -- ADD YOUR ENTRIES ABOVE THIS LINE -- DO NOT REMOVE[/sourcecode]

	
  3. /etc/shorewall/policy  [sourcecode language='cpp']
###############################################################################
#SOURCE         DEST            POLICY          LOG LEVEL       LIMIT:BURST
hot             net             ACCEPT
hot             $FW             ACCEPT
hot             all             REJECT          info
loc             net             ACCEPT
loc             $FW             ACCEPT
loc             all             REJECT          info
$FW             net             ACCEPT
$FW             hot             ACCEPT
$FW             all             REJECT          info
$FW             loc             ACCEPT
net             $FW             ACCEPT
net             hot             DROP            info
net             all             DROP            info
all             all             REJECT          info
#LAST LINE -- ADD YOUR ENTRIES ABOVE THIS LINE -- DO NOT REMOVE[/sourcecode]

	
  4. /etc/shorewall/rules  [sourcecode language='cpp']
#############################################################################################################
#ACTION         SOURCE          DEST            PROTO   DEST    SOURCE          ORIGINAL        RATE            USER/  $
#                                                       PORT    PORT(S)         DEST            LIMIT           GROUP
#
DNS/ACCEPT      $FW             net
SSH/ACCEPT      loc             $FW
Ping/ACCEPT     loc             $FW
Ping/DROP       net             $FW
ACCEPT          $FW             loc             icmp
ACCEPT          $FW             net             icmp
#LAST LINE -- ADD YOUR ENTRIES BEFORE THIS ONE -- DO NOT REMOVE[/sourcecode]

	
  5. /etc/shorewall/zones  [sourcecode language='cpp']
###############################################################################
#ZONE   TYPE    OPTIONS                 IN                      OUT
#                                       OPTIONS                 OPTIONS
fw      firewall
net     ipv4
hot     ipv4
loc     ipv4
#LAST LINE - ADD YOUR ENTRIES ABOVE THIS ONE - DO NOT REMOVE[/sourcecode]

	
  6. Cambiare il parametro STARTUP_ENABLED=Yes in shorewall.conf

	
  7. Editare il file /etc/default/shorewall e impostare il parametro startup=1.


**Restart di tutti i demoni coinvolti : **

/etc/init.d/networking restart
/etc/init.d/dnsmasq restart
/etc/init.d/freeradius restart
/etc/init.d/apache2 force-reload
/etc/init.d/mysql restart
/etc/init.d/chilli stop
/etc/init.d/chilli start
/etc/init.d/shorewall restart

**Collegamento Access Point:**

A questo punto possiamo collegare tanti access point quante porte abbiamo disponibili nello switch2 .

Controlliamo con un notebook ed un netstumbler le aree di copertura radio e piazziamo gli access point nelle aree più consone per una copertura ottimale.

Possiamo usare qualsiasi access point , io per le prove ho usato dei [d-link dwl-g700ap ](http://shopping.kelkoo.it/ctl/do/search?siteSearchQuery=dwl-g700ap&fromform=true)

**Conclusioni:**

Credo  di essere stato sufficientemente chiaro e lineare in questo howto.  Questa guida è stata compilata in quanto non ho trovato in rete nulla che trattasse il tema di hot spot in maniera passo a passo.

Vi invito a provare sul campo questo setup e di segnalarmi problemi , inesatezze e magari suggerire migliorie e aggiornamenti.
