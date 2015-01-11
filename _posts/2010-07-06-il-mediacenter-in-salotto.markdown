---
author: dema
comments: true
date: 2010-07-06 10:23:34+00:00
layout: post
slug: il-mediacenter-in-salotto
title: Il mediacenter in salotto
wordpress_id: 1170
categories:
- howto
tags:
- compat-wireless
- infrared
- linux
- lirc
- mediacenter
- ubuntu
- xbmc
---

[![](http://dema.tv/wp-content/uploads/2010/07/iconografia1.jpg)](http://dema.tv/wp-content/uploads/2010/07/iconografia1.jpg)

Finalmente ho deciso di dotarmi di una postazione di home entertainment in salotto così composta :

Tv lcd full HD 37 pollici LG 37LD428
Decoder sky HD
Asus eeebox  PC EB1501

Se ovviamente lato televisore e decoder c'è veramente poco da smanettare nel piccolo eeebox possiamo mettere le mani con soddisfazione  e creare un media center di tutto rispetto mantenendo ingombro e rumorosità sotto controllo e, cosa da non sottovalutare, senza deturpare l'arredo con "case" catafalchi e grovigli di fili.

L'eeebox EB1501 è stato scelto fra i vari nettop principalmente per tre motivi.
Il primo è che ho un'ottima stima di Asus come produttore e nessuna brutta esperienza con i suoi prodotti, il secondo è la dotazione di serie di un telecomando a raggi infrarossi e un ricevitore IR integrato, indispensabile per un mediacenter e l'ultimo perché è uno dei pochi nettop che incorpora anche il lettore DVD (sempre più raramente, ma a volte può capitare di vederne uno).

Per il resto le caratteristiche sono quelle di un comune nettop: intel atom 330 dual core, 2 Gb Ram, disco da 250 Gb, grafica Nvidia ION, porta Lan e WLAN b/g/n.

Non mi dilungo sulle valutazioni dell'hardware in quanto l'Eeebox EB1501 è ormai in commercio da un pezzo ed è stato già rivoltato come un calzino.

Vediamo come al solito passo passo come procedere all'installazione del software che si appoggerà su Ubuntu 9.10 e XBMC.

<!-- more -->

Ubuntu 9.10? Ma è uscita la 10.4 ormai da tre mesi, perché usare la release vecchia?
Vi rispondo con tre notti insonni, scandaglio totale del forum di forum.xbmc.org per far funzionare a dovere lirc(senza successo), inceppamenti del sistema e problemi al wifi.
Per il computer da salotto volevo che tutto funzionasse al volo senza troppi sbattimenti e la Lucid Lynx non è riuscita a soddisfare questa mia necessità, quindi, Karmic Koala :-)


#### Installazione Ubuntu 9.10, XBMC e Drivers Nvidia


Se non siamo in possesso della vecchia iso possiamo andare a prenderla dagli archivi Ubuntu a [questo indirizzo](http://releases.ubuntu.com/9.10/ubuntu-9.10-desktop-i386.iso).
Colleghiamo la presa hdmi al pannello LCD e facciamo il boot da cd o da pendrive.
L'installazione procede senza nessun intoppo ed alla fine tutte le periferiche verranno riconosciute ad eccezione della scheda grafica Nvidia, della quale verrà proposto un driver "restricted" che **non installeremo.**

Provvederemo infatti ad installare i drivers proprietari Nvidia e le estensioni vdpau per l'offload di alcune porzioni della decodifica dei file MPEG dalla cpu verso la gpu permettendo la visione fluida di filmati a 1080p. Inoltre installeremo il mediacenter XBMC, tramite l'aggiunta della sua repository.

[sourcecode language="bash"]
sudo apt-get install python-software-properties -y
sudo add-apt-repository ppa:team-xbmc
sudo add-apt-repository ppa:nvidia-vdpau/ppa
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 91E7EE5E 318C7509[/sourcecode]

Aggiorniamo ed installiamo XBMC

[sourcecode language="bash"]
apt-get update && sudo apt-get upgrade
apt-get install xbmc xinit x11-xserver-utils -y[/sourcecode]

L'installazione di XBMC sarà un po' lunga per l'elevato numero di pacchetti necessari per soddisfare le dipendenze del nostro mediacenter.

Installiamo ora i drivers Nvidia e Vdpau

[sourcecode language="bash"]
apt-get install nvidia-glx-195 mesa-utils libvdpau1 libvdpau-dev vdpauinfo pkg-config -y[/sourcecode]

Generiamo un xorg.conf per i nostri scopi

[sourcecode language="bash"]
nvidia-xconfig -s --no-logo --force-generate --output-xconfig=/etc/X11/xorg.conf[/sourcecode]

ed editiamolo a mano per una migliore performance della nostra scheda video

[sourcecode language="bash"]
nano /etc/X11/xorg.conf
[/sourcecode]

e aggiungiamo alla sezione "Device" le seguenti righe

[sourcecode language="bash"]
Option "HWCursor" "False"        # Blinking Cursor Fix
Option "DynamicTwinView" "false" # Enable 1080p 24Hz[/sourcecode]

E alla fine del file disabilitiamo composite per una migliore performance

[sourcecode language="bash"]
Section "Extensions"
         Option         "Composite" "Disable" # Disable Composite for better H264 acceleration
EndSection[/sourcecode]

A questo punto possiamo riavviare la macchina o caricare il driver Nvidia

[sourcecode language="bash"]
modprobe nvidia
[/sourcecode]

E far ripartire il server grafico.


#### Audio via HDMI


Dopo il reboot il nostro schermo dovrebbe essere settato alla corretta risoluzione di 1920x1080 a 60Hz.
Dobbiamo ancora sistemare però l'audio via HDMI che di default è azzerato.

Per far questo apriamo la shell e diamo il comando _alsamixer_. Con il cursore ci spostiamo a selezionare i 3 valori  IEC958 e con il tasto "M" provvediamo a levare il mute sul canale.
[![](http://dema.tv/wp-content/uploads/2010/07/alsamixer1.jpg)](http://dema.tv/wp-content/uploads/2010/07/alsamixer1.jpg)


### Drivers wireless


La Ubuntu 9.10 nonostante riconosca il chip AR9285 e carichi i moduli ath9k non riesce a fornire prestazioni accettabili per il link wifi. I driver a corredo offrono un collegamento altamente instabile con disconnessioni ogni 10 secondi quando si effettua un download e non possono in definitiva essere usati.

Per ovviare dobbiamo procurarci gli ultimi driver compat wireless e compilarli per il nostro kernel. Possiamo scaricare l'ultima versione da [linuxwireless.org](http://linuxwireless.org/en/users/Download) oppure usare la [release 27-06-2010](http://www.orbit-lab.org/kernel/compat-wireless-2.6/2010/06/compat-wireless-2010-06-27.tar.bz2) che ho installato sul mio sistema e verificato funzionare perfettamente.

Una volta scaricato il pacchetto compat-wireless-2.6.tar.bz2, dobbiamo scompattarlo e procurarci gli strumenti per compilarlo.

Installiamo i build-essential e gli header del kernel in uso

[sourcecode language="bash"]
apt-get install build-essential
apt-get install linux-headers-`uname -r`[/sourcecode]

Posizioniamoci poi nella directory dove abbiamo scaricato i compat-wireless e diamo i seguenti comandi

[sourcecode language="bash"]
tar xjvf compat-wireless-2.6.tar.bz2

cd compat-wireless-2.6
./scripts/driver-select ath9k
make
make install[/sourcecode]

Alla fine del processo rimpiazzeremo il modulo ath9k dello stock kernel di Ubuntu 9.10 con un nuovo perfettamente funzionante ath9k.  Dopo il reboot.


### Telecomando IR


Per prima cosa procuriamoci i pacchetti di Lirc e i moduli necessari

[sourcecode language="bash"]
apt-get install lirc lirc-modules-source module-assistant
dpkg-reconfigure lirc-modules-source #ridondante? [/sourcecode]

Dovrebbe essere sufficiente per caricare all'avvio sia il demone Lirc che il modulo lirc_it87, ma sfortunatamente senza un ulteriore ritocco, non funzionerà dal momento che il dispositivo ITE8713 viene caricato all'avvio in stato _disabled_.

Per ovviare a questo inconveniente dobbiamo editare lo script di init di lirc inserendo alla linea 173 la seguente stringa

[sourcecode language="bash"]
echo activate > /sys/devices/pnp0/00:09/resources[/sourcecode]

[![](http://dema.tv/wp-content/uploads/2010/07/lirc1.jpg)](http://dema.tv/wp-content/uploads/2010/07/lirc1.jpg)

A questo punto dobbiamo mettere mano ai file di configurazione di lircd in modo che riconosca correttamente il nostro telecomando.

[sourcecode language="bash"]
wget http://nfye.com/EB1501/hardware-german.conf -O /etc/lirc/hardware.conf
wget http://nfye.com/EB1501/lirc.conf -O /etc/modprobe.d/lirc.conf
cp /usr/share/lirc/remotes/mceusb/lircd.conf.mceusb /etc/lirc/lircd.conf
cp /usr/share/xbmc/system/Lircmap.xml ~/.xbmc/userdata[/sourcecode]

Riavviamo la macchina e controlliamo se il telecomando è stato riconosciuto correttamente con questa procedura:
1) lanciamo il comando _irw_
2) premiamo qualche pulsante sul telecomando
3) se tutto è andato bene dovremmo veder apparire del testo nel terminale

Il telecomando lo abbiamo sistemato.


### XBMC


Il programma xbmc.bin può essere lanciato ora e a pieno schermo dovrebbe apparire la schermata di benvenuto

[![](http://dema.tv/wp-content/uploads/2010/07/screenshot0001.jpg)](http://dema.tv/wp-content/uploads/2010/07/screenshot0001.jpg)

Proviamo a navigare con i tasti freccia del telecomando, tutto dovrebbe funzionare senza problemi. Se non riusciamo a comandare l'interfaccia tramite il telecomando qualcosa è andato storto nel setup appena descritto sopra. Possiamo perdere anche tre notti di sonno per capire cosa, vi lascio provare l'ebbrezza, io l'ho già provata :-)

Per ritornare al desktop di gnome si può premere il tasto "" e far girare xbmc in windowed mode e con il terminale cercare di fare del debug o editare i file di configurazione e riprovare. Tenete presente che tramite tastiera si comanda senza problemi, la parte più astiosa è la configurazione del telecomando.

Se vogliamo far partire xbmc all'avvio di ubuntu dobbiamo eseguire i seguenti passi:

1) Settare il login automatico da gdm. Da Gnome scegliere: Sistema->Amministrazione->Schermata di Accesso e scegliere Accedere come $nomeutente automaticamente

[![](http://dema.tv/wp-content/uploads/2010/07/schermata1.jpg)](http://dema.tv/wp-content/uploads/2010/07/schermata1.jpg)

2) Disabilitare la richiesta di password del portachiavi di gnome. Sempre da Gnome scegliere: Applicazione->Accessori->Password e chiavi di cifratura. Selezionare Password:login, cliccare con il tasto destro e scegliere: Cambia password. Immettere la propria password corrente e lasciare vuoti i due campi della nuova password. Scegliere: usa archiviazione non sicura

[![](http://dema.tv/wp-content/uploads/2010/07/login1.jpg)](http://dema.tv/wp-content/uploads/2010/07/login1.jpg)

[![](http://dema.tv/wp-content/uploads/2010/07/non-sicura1.jpg)](http://dema.tv/wp-content/uploads/2010/07/non-sicura1.jpg)

3) Far partire XBMC all'avvio di Gnome. Da Sistema->Preferenze->Applicazioni d'avvio aggiungere xbmc alla lista dei programmi in esecuzione automatica.

[![](http://dema.tv/wp-content/uploads/2010/07/avvio1.jpg)](http://dema.tv/wp-content/uploads/2010/07/avvio1.jpg)

A questo punto se tutto è andato liscio al reboot dovremmo avere direttamente la schermata di XBMC all'avvio del computer ed essere in grado di comandare il mediacenter unicamente con il telecomando IR, compreso lo spegnimento e l'accensione tramite il tasto "power".


### Alto utilizzo della cpu


Grazie alle API Vdpau XBMC è in grado di riprodurre filmati a 1080p senza nessuno sforzo, fluidi e ben definiti, con un impegno di cpu attorno al 5%!

La cosa che però salta subito all'occhio è l'elevato uso di CPU quando XBMC è in idle mode, mostrando unicamente la sua interfaccia. E' un comportamento bizzarro ma che ha la sua spiegazione. Quando non viene invocato nessun player infatti l'interfaccia viene renderizzata a 60 frame al secondo, impegnando di conseguenza la cpu del sistema.

Per ovviare a questo inconveniente è necessario impostare al posto di dim on idle, black on idle, facendo in modo che dopo tre minuti di inattività la gui venga oscurata completamente invece che adombrata.

Sperando che questo post vi possa essere stato d'aiuto, mi pongo come al solito all'ascolto dei vostri commenti per eventuali correzioni, migliorie, insulti etc etc.

Credits:
[Xbmc Wiki (parte 1)](http://wiki.xbmc.org/index.php?title=HOW-TO_preform_a_Miminal_Ubuntu_and_XBMC_install_on_a_Asus_EeeBox_PC_EB1501)
[Xbmc Wiki (parte 2)](http://wiki.xbmc.org/index.php?title=Talk:HOW-TO_preform_a_Miminal_Ubuntu_and_XBMC_install_on_a_Asus_EeeBox_PC_EB1501)
[Xbmc Forum](http://forum.xbmc.org/showthread.php?t=68182)
