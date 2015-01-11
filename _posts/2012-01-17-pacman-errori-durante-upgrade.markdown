---
author: dema
comments: true
date: 2012-01-17 13:58:51+00:00
layout: post
slug: pacman-errori-durante-upgrade
title: ' Pacman errori durante upgrade'
wordpress_id: 1450
categories:
- linux
tags:
- arch
- archlinux
- gpg
- pacman
---

Pacman, il meraviglioso gestore di pacchetti di ArchLinux - anche se poi non gestisce pacchetti, ma più correttamente, metapacchetti - è giunto alla versione 4.0.1-4. Per chi proviene dalla 3.5, come me, si è trovato una bella sorpresa dopo aver lanciato il consueto pacman -Syu: una volta scaricati i pacchetti da aggiornare, non c'è modo di installarli. Ci si trova infatti davanti ad un messaggio poco confortante:

    
    error: failed to commit transaction (invalid or corrupted package)
    Errors occurred, no packages were upgraded.


Niente paura. Una breve ricerca sul forum di Arch e il rimedio è presto trovato.

E' necessario ricreare una chiave gpg per pacman con il comando pacman-key --init ed aggiungere la seguente linea a /etc/pacman.conf

    
    SigLevel = Optional TrustAll


Se durante la creazione della chiave gpg il sistema si lamentasse per una non sufficiente entopia del sistema potete provare a muovere/trascinare una finestra, se siete in ambito X, o se state amministrando un sistema senza server grafico, magari via ssh, potete procedere come segue: Installate rng-tools via yaourt (non avrete i problemi di pacman come sopra) lanciate come root il seguente comando:

    
    rngd -f -r /dev/urandom


e di seguito - in un altro terminale -

    
    pacman-key --init.
