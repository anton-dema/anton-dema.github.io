---
author: dema
comments: true
date: 2009-03-27 10:36:32+00:00
layout: post
slug: disinstallare-safari-4-beta
title: 'Disinstallare Safari 4 Beta '
wordpress_id: 707
categories:
- minihowto
tags:
- mac
- safari
- webkit
---

Stamane mi sono ritrovato a disinstallare Safari 4 beta dal mio Macbook. La disinstallazione su Mac OS di solito è solo questione di un trascinamento nel cestino  dell'applicazione che vogliamo disinstallare e nello svuotamento del cestino stesso.
Il problema è che volendo reinstallare la versione stabile corrente , la 3.2.1 , l'installer si pianta lamentandosi che una versione più recente è già installata nel sistema.

Il problema risiede nel cambiamento del numero di versione delle risorse di Webkit , il motore del browser di Cupertino. Quando viene installato Safari 4 la versione di Webkit viene scritta nel file /System/Library/Frameworks/WebKit.framework/Resources/Info.plist come 5528 e 5528.16

Per ovviare a questo problema è sufficiente aprire l'editor di testo preferito da Terminale e cambiare le occorrenze 5528 e 5528.16 in 5525

[sourcecode language='cpp']
sudo nano -w  /System/Library/Frameworks/WebKit.framework/Resources/Info.plist
password:




        CFBundleDevelopmentRegion
        English
        CFBundleExecutable
        WebKit
        CFBundleGetInfoString
        5525, Copyright 2003-2009 Apple Inc.(valore cambiato)
        CFBundleIdentifier
        com.apple.WebKit
        CFBundleInfoDictionaryVersion
        6.0
        CFBundleName
        WebKit
        CFBundlePackageType
        FMWK
        CFBundleShortVersionString
        5525 (valore cambiato)
        CFBundleVersion
        5525(valore cambiato)

[/sourcecode]

Una volta apportate queste modifiche [safari 3.2.1](http://appldnld.apple.com.edgesuite.net/content.info.apple.com/Safari3/061-5805.20881124.ShIPH/Safari321Leopard.dmg) si installerà senza problemi
