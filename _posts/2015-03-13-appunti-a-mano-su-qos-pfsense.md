---
title: Appunti a mano su QOS per pfSense
author: Dema
layout: post
date: 2015-03-13 09:28:24+00:00
header-img: img/phone.jpg
tags: 
- appunti
- pfsense
---
È da una settimana, circa, che sto combattendo con il traffic shaping di una adsl 15/1, per cercare di ottenere una qualità del servizio omogenea, anche in presenza di massicci upload.        
Se, infatti, anche senza alcun QOS, in presenza di download massivi, la banda si equalizza tra le varie destinazioni, mantenendo inalterato, o quasi, la latenza, le cose cambiano un bel po' se iniziamo a fare upload massiccio.          
La soluzione è, come logico, l'utilizzo delle code gerarchiche di [hfsc](http://en.wikipedia.org/wiki/Hierarchical_fair-service_curve) e la loro disciplina, per quanto rimanga un'arte da druido, grazie alla semplicità della GUI di pfSense, è quasi un gioco da ragazzi.            
Quasi, perché anche nel tuning che ho realizzato, ci sono delle zone d'ombra e il servizio degrada in caso di upload concorrenti e massivi (più di 5 sessioni di upload e i pacchetti SYN vengono ritardati, compromettendo la pronta risposta delle pagine web).                       
Non ho file xml pronti per il download, o meglio ce li ho, ma il vostro setup potrebbe variare, nel nome delle interfacce, quindi sarebbe inservibile. Ho invece la scansione del foglio vergato con la mia scrittura a matita, dove ho pianificato le code.                
Se lo trovate di utilità, potete lasciare un commentino qui sotto, donerete un sorriso al sottoscritto. Ah, se ho scritto delle minchiate siete pregati di farmelo notare, metterò in editing il presente post in men che non si dica.        
Il pdf della scansione potete scaricarlo [qui](http://dmblob.s3.amazonaws.com/appuntipfsense.pdf)