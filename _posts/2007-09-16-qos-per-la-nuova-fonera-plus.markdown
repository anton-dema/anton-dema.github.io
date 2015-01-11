---
author: dema
comments: true
date: 2007-09-16 11:46:50+00:00
layout: post
slug: qos-per-la-nuova-fonera-plus
title: 'Qos per la nuova Fonera plus '
wordpress_id: 101
categories:
- fon
- fonera
- fonera plus
- l7
- netfilter
- qos
---

![](http://l7-filter.sourceforge.net/l7logo)  La nuova [Fonera plus ](https://shop.fon.com/FonShop/shop/IT/ShopController), introdotta a Luglio da Fon , per rimpiazzare la fonera di vecchia generazione , presenta molte novità sul fronte del firmware .

Nonostante non sia ancora possibile ottenere un **accesso alla console di amministrazione** , ne tramite ssh e neppure tramite porta seriale , **alcune parti del rootfs sono disponibili **e ci rivelano qualche particolare dei nuovi script introdotti.

Qui mi voglio concentrare sulla **gestione del qos** (quality of service , un modo per dare priorità a determinati pacchetti e permettere di riservare maggiore banda a determinati protocolli ) che nella nuova Fonera plus si affida a [L7 filter](http://l7-filter.sourceforge.net/) .

L7 si occupa di marcare i pacchetti a livello di layer data per poi filtrarli con **opportune regole di netfilter**.

Ecco la configurazione di default della fonera plus :
[sourcecode language='cpp']# QoS configuration for OpenWrt
# INTERFACES:
config interface hotspot
option classgroup	"Default"
option enabled		0
option upload    	512
option download		512
option device		tun0

config interface wan
option classgroup  "Default"
option enabled      0
option overhead 	1
option upload       128
option download     1024

# RULES:
config classify
option target       "Bulk"
option ipp2p        "all"
config classify
option target       "Bulk"
option layer7       "edonkey"
config classify
option target       "Bulk"
option layer7       "bittorrent"
config classify
option target       "Priority"
option ports        "22,53"
config classify
option target       "Normal"
option proto        "tcp"
option ports        "20,21,25,80,110,443,993,995"
config classify
option target       "Express"
option ports        "5190"
config default
option target       "Express"
option proto        "udp"
option pktsize      "-500"
config reclassify
option target       "Priority"
option proto        "icmp"
config default
option target       "Bulk"
option portrange    "1024-65535"
config reclassify
option target       "Priority"
option proto        "tcp"
option pktsize      "-128"
option mark         "!Bulk"
option tcpflags     "SYN"
config reclassify
option target       "Priority"
option proto        "tcp"
option pktsize      "-128"
option mark	        "!Bulk"
option tcpflags     "ACK"

# Don't change the stuff below unless you
# really know what it means

config classgroup "Default"
option classes      "Priority Express Normal Bulk"
option default      "Normal"

config class "Priority"
option packetsize  400
option maxsize     400
option avgrate     10
option priority    20
config class "Priority_down"
option packetsize  1000
option avgrate     10

config class "Express"
option packetsize  1000
option maxsize     800
option avgrate     50
option priority    10

config class "Normal"
option packetsize  1500
option packetdelay 100
option avgrate     10
option priority    5
config class "Normal_down"
option avgrate     20

config class "Bulk"
option avgrate     1
option packetdelay 200[/sourcecode]

Tutto molto bello e utile , ma n**on modificabile dai foneros** , ad eccezione della prima parte dove si assegna la banda massima su tun0 , ovvero la wlan pubblica , tramite il panello di controllo della userzone.
