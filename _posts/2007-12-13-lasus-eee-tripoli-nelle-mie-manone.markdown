---
author: dema
comments: true
date: 2007-12-13 16:44:45+00:00
layout: post
slug: lasus-eee-tripoli-nelle-mie-manone
title: 'L''asus eee (Tripoli) nelle mie manone '
wordpress_id: 152
categories:
- asus eee
- asus tripoli
- fon
- linux
- mini laptop
- ubuntu
tags:
- asus eee
- asus tripoli
- fon
- howto
- linux
- mini laptop
- ubuntu
---

[![asus eee and me](http://dema.tv/wp-content/uploads/2007/12/2106139720_fe657db742.jpg)](http://www.flickr.com/photos/dema/2106139720/)

Domenica mi è arrivato direttamente da Guangzhou , via [Alessandro ](http://www.magodiwoz.com/), il nuovo **oggetto del desiderio** dei geek di mezzo mondo .<!-- more -->


##### Aspetto esterno


[![asus eee and me](http://dema.tv/wp-content/uploads/2007/12/2106140082_77c4d67b66_m.jpg)](http://www.flickr.com/photos/dema/2106140082/)
L'Asus Tripoli ha un** sex-appeal decisamente forte**. E' minuscolo , leggerissimo , non passa assolutamente inosservato. Appena ti fermi in un luogo pubblico ad usarlo , attira subito una schiera di curiosi . La mia prima sensazione a riguardo del case esterno in plastica perlata , della griglia plasticata microforata a lato dello schermo , della tastiera e del resto del case esterno è quella di **avere a che fare con un computer giocattolo**.  Lo schermo da 7 pollici , permette una risoluzione di 800x480 , una discreta qualità dei colori e una nitidezza veramente notevole. La tastiera è molto piccola e sono frequenti errori di digitazione , ma direi che è assolutamente usabile , a patto di farci un po' l'abitudine. Il trackpad anche esso in versione mini è dotato di scrolling verticale . Molto utile , vista la dimensione dello schermo , è la possibilità di attivare nel trackpad anche lo scrolling orizzontale.

Insomma da lontano sembra il **computer della Chicco** , una volta messe le mani sopra ti lascia un po' interdetto , ma il bello comincia appena lo accendi.


#####  Il sistema operativo


[![Asus Tripoli](http://dema.tv/wp-content/uploads/2007/12/2100446808_d17f762467_m.jpg)](http://www.flickr.com/photos/dema/2100446808/)
Il nuovo Asus Tripoli viene preconfigurato con una versione ottimizzata di Xandros OS , un fork di Debian . Vengono tagliati alcuni pacchetti non necessari per risparmiare il più possibile lo spazio su disco che è di 4 Gb in stato solido e soprattutto viene usato un desktop utente con grosse icone che corrispondono ad altrettanti applicativi. Molti di questi applicativi non sono altro che scorciatoie web per i servizi online di Google , come Gmail e Google Docs. L'asuseeeOS mi è arrivato in Cinese , ed è rimasto installato per non più di 30 secondi. Volevo installare un vero sistema operativo e la scelta è ricaduta ovviamente su Ubuntu Gutsy.

Ho usato un cdrom usb che avevo dentro un cassetto dimenticato , avendo cura di cambiare la sequenza di boot nel bios. Qualora non foste dotati di un cdrom usb , potete fare il boot da usb stick , seguendo [questo tutorial](http://www.pendrivelinux.com/2007/09/28/usb-ubuntu-710-gutsy-gibbon-install/) veramente chiaro.

L'installazione di ubuntu funziona come una qualsiasi altra installazione. Dobbiamo solo prendere alcune precauzioni perchè la nostra installazione prosegua in maniera tranquilla.

**Passo 1:** Appena visualizzato il desktop , appare un popup che dice che la nostra batteria potrebbe essere malfunzionante . Ignoriamolo e chiudiamolo.

**Passo 2:** Installando dal live cd di ubuntu , vista la dimensione esigua dello schermo e vista l'attivazione di default di compiz , potremmo avere dei problemi con le dialog windows che non mostreranno i pulsanti di comando , ne permetteranno di essere trascinate verso l'alto per scoprire i comandi. Per permettere il trascinamento mediante Alt+click sinistro dobbiamo cambiare un parametro di compiz dal gconf-editor. La chiave da cercare è _apps/compiz/plugins/move/allscreens/options/__constrain_y _che è da deflaggare. Alternativamente , possiamo usare un cd di installazione di ubuntu alternate senza sessione live che possiamo recuperare [qui](http://na.mirror.garr.it/mirrors/ubuntu-releases/gutsy/ubuntu-7.10-alternate-i386.iso).

**Passo 3: **A questo punto possiamo cliccare sull'icona del desktop install. Scegliamo Italiano come localizzazione , e Us English come layout di tastiera.

**Passo 4: **La partizione del disco. L'esiguo spazio disco non ci permette di usare una partizione per lo swap , quindi scegliamo di usare tutto il disco per un'unica partizione di root . Scegliamo il metodo manuale e distruggiamo le partizioni esistenti in cui è gia installato xandros eee. Ricreiamo una nuova partizione primaria da 4 Gb e come filesystem usiamo a scelta ext2 o , meglio , jfs  per un supporto al journaling. Ignoriamo l'avvertimento della mancanza  di una partizione di swap e proseguiamo.

**Passo 5:** L'installer comincerà a fare il suo sporco lavoro . Ci metterà parecchio , quindi intrattenetevi in qualche modo , che sò , riguardatevi  la vostra puntata preferita di IT-crowd , la mia è definitivamente [questa ](http://sharetv.org/shows/the_it_crowd_uk/episodes/2x01).

**Passo 6:** Ad un certo punto del processo di installazione , verranno cercate le repository esterne per lo scaricamento degli aggiornamenti . Se avete collegato il laptop ad una presa di rete e avete un accesso internet potete accettare e installarli , altrimente procedete oltre . Finite l'installazione , spegnete il sistema e scollegate il cdrom usb o la stick che avete usato per l'installazione.

**Passo 7:**Una volta portato a termine l'installazione e dato reboot  , ci troviamo davanti al classico desktop marrone di default di ubuntu 7.10 , ma abbiamo bisogno di fare parecchi aggiustamenti . Primo fra tutti , ignoriamo i due popup che ci dicono che la nostra batteria è rotta e che stiamo usando dei restricted drivers . Poi ripetiamo quanto già fatto al Passo 2 , ossia la possibilità di muovere le finestre di dialogo. Minimizziamo gli accessi in scrittura al disco , aggiungedo le seguenti righe al file fstab:

[sourcecode language='cpp']
# /dev/sda1
UUID=d2f0be8e-a4ce-49c5-b6be-0fd3a8b4a5ce   /          jfs	defaults,errors=remount-ro,noatime        0       2
tmpfs     /var/log       tmpfs     defaults,noatime        0 0
tmpfs     /tmp              tmpfs     defaults,noatime        0 0
tmpfs     /var/tmp       tmpfs     defaults,noatime        0 0[/sourcecode]
Azzeriamo anche l'uso del file di swap , che non abbiamo creato , aggiungendo le seguenti linee al file _/etc/sysctl.com_
[sourcecode language='cpp']

vm.swappiness=0[/sourcecode]
Editiamo il file _/etc/apt/sources.list_ e mettimo un cancelletto davanti alla riga deb cdrom e se abbiamo installato senza connessione alla rete , togliamo sempre il cancelletto a tutte le repository esterne di ubuntu.

**Passo 8**: Cominciamo a sistemare le cose che non funzionano . A questo punto una connessione alla rete è necessaria . Usate un cavo ethernet per collegarvi al vostro router adsl . Scarichiamo tutti gli aggiornamenti di sicurezza e attendiamo anche qui un bel po'.E' tempo ora di far parlare la nostra lingua a Ubuntu . Andiamo in _amministrazione -> supporto lingue_ e installiano il pieno supporto all'italiano.

Ultimo tweak , configuriamo il desktop di ubuntu per adeguarsi al nostro mini schermo. Andiamo in _Sistema->Preferenze-Aspetto_ e su tipi di carattere selezioniamo un font di dimensione 9 e un tema più gradevole di quello di default.

**Passo 9**: Cominciamo a lavorare a quello che non funziona . **Iniziamo con la scheda wireless** . Dobbiamo prendere i driver patchati di madwifi . Ecco il percorso completo :
[sourcecode language='cpp']
sudo apt-get install build-essential
wget 'http://madwifi.org/attachment/ticket/1679/madwifi-ng-0933.ar2425.20071130.i386.patch?format=raw'
wget http://snapshots.madwifi.org/madwifi-ng/madwifi-ng-r2756-20071018.tar.gz
tar zxvf madwifi-ng-r2756-20071018.tar.gz
cd madwifi-ng-r2756-20071018
patch -p0 < ../madwifi-ng-0933.ar2425.20071130.i386.patch?format=raw
make clean
make
sudo make install
reboot[/sourcecode]
Da questo momento la vostra scheda wireless funzionerà con gli **ottimi driver di madwifi **. A breve mi cimenterò anche nella costruzione di un custom router Fon con il Tripolino e le meravigliose potenzialità di Atheros.Continuiamo con i **fix sulla scheda audio** per far funzionare il microfono incorporato .Aggiungiamo al file _/etc/modprobe.d/alsa-base_ la seguente stinga alla fine del file
[sourcecode language='cpp']

options snd-hda-intel model=3stack-dig[/sourcecode]
Aggiustiamo alcuni settings per permettere **un corretto suspend/resume del sistema**.Editiamo il file_ /etc/default/acpi-support_ , cambiando SAVE_VBE_STATE in false
[sourcecode language='cpp']

# Should we save and restore state using the VESA BIOS Extensions?

SAVE_VBE_STATE=false[/sourcecode]



Abilitiamo SAVE_VIDEO_PCI_STATE=true

[sourcecode language='cpp']

# Save and restore video state?

SAVE_VIDEO_PCI_STATE=true[/sourcecode]



Abilitiamo DPMS

[sourcecode language='cpp']
# Should we switch the screen off with DPMS on suspend?
USE_DPMS=true[/sourcecode]
Infine inseriamo alcuni workaround per permetter il corretto **spegnimento **del nostro laptop.Editiamo il file _/etc/init.d/halt_ e inseriamo all'inizio dello scrip la seguente riga :
[sourcecode language='cpp']

rmmod snd-hda-intel[/sourcecode]
Io per ultimissima cosa sono un pignolo e voglio vedere cosa succede durante il processo di boot , quindi ho modificato la seguente linea al file _/boot/grub/menu.lst_
[sourcecode language='cpp']

/vmlinuz-2.6.22-14-generic root=UUID=1532e751-35a4-4ef3-9073-7986d33f74c8 ro vga=normal[/sourcecode]
Dovremmo aver finito con il tweak , diamo un reboot e dovrebbe funzionare tutto come si deve.


##### I pacchetti software


[![Schermata](http://dema.tv/wp-content/uploads/2007/12/2108133889_03c4821ab0.jpg)](http://www.flickr.com/photos/dema/2108133889/)
Lasciando tutto di default , il sistema ubuntu usa  2.4 Gb di spazio sul disco , lasciando 1.4 Gb liberi . Si deve quindi ricorrere a memorie esterne per il trasporto dei dati , ma non è una grande limitazione , visto che le pennine usb ormai si trovano anche dentro le patatine. ( modo di dire molto in voga negli anni 80).

Abbiamo quindi tutto a portata di mano , e se ci dovesse venire in mente qualche applicativo , è come al solito a portata di _apt-get install_.

Io non sono un amante di skype , ma ho installato la versione 2.0 beta  che incorpora la funzione video. E devo dire che funziona veramente bene.

**Impressioni d'uso**

Dopo solo un paio di giorni sono **praticamente addicted** . Ho una mobilità totale e un perfetto tool di scanning per reti wireless , grazie a kismet (condito con la solita salsa lamer chi vuol capire capisca) . Posso fare l'upload delle foto su flickr al volo , inserendo la scheda sd nell'apposito lettore , ascoltare la mia musica via lastFM tramite il plugin di Rhythmbox e in buona sostanza fare quasi tutto quello che mi permette di fare un altro laptop o desktop linux che sia. Una nota dolente purtroppo è **la durata dalle batterie , che reggono solo per un'ora e quaranta **di uso continuato. Non ho potuto provare con il SO originale , in quanto subito sradicato , che forse ha qualche accorgimento per risparmiare energia.

Il portatile è costato **3000 Yuan , al cambio circa 260 euro** , e devo dire che per il prezzo fa anche troppo.

Spero che abbiate trovato queste informazioni utili , e attendo la commercializzazione in italia per vedere come reagirà il mercato a questa novità.

Per stendere questo post ho utilizzato il [wiki di eeeuser.com ](http://wiki.eeeuser.com/ubuntu?s=ubuntu)e un utilissimo post di [Paul McGuinnes](http://www.internet-tools.co.uk/blog/index.php/2007/11/26/installing-ubuntu-710-gutsey-gibbon-on-my-asus-eee-pc/)
