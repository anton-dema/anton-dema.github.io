---
author: dema
comments: true
date: 2011-09-27 08:18:25+00:00
layout: post
slug: amazon-route-53-non-ci-sono-piu-scuse-per-non-usarlo
title: Amazon Route 53, non ci sono più scuse per non usarlo
wordpress_id: 1407
categories:
- web
tags:
- amazon
- dns
- dyn.com
- route53
---

Amazon ha annunciato la riduzione dei prezzi del suo servizio di DNS [Route 53](http://aws.amazon.com/route53/pricing/).

Dal 1 Ottobre 2011 il servizio costerà 0.50$ al mese per le prime 25 zone e 0.10$ per le zone oltre 25. Il traffico sarà fatturato nella misura di 0.50$ per milione di query per il primo miliardo di query e 0.25$ per le query eccedenti il primo miliardo. E' inutile dire che questa è una notizia straordinaria che mette definitivamente fuori mercato principalmente il maggior fornitore di hosted Dns, ossia [Dyn.com](http://dyn.com/).

Se infatti volessimo avere un servizio paragonabile a quello di Amazon Route 53 da Dyn.com dovremmo rivolgerci al servizio [DynECT Managed DNS](http://dyn.com/dns/dynect-managed-dns-lite/) con costi a partire da 30$/mese per 10 managed zones, ben 25$ più caro di Amazon.

Lo scoglio per l'uso di Amazon Route53 però è la sua difficoltà di setup. In attesa che Amazon integri anche Route53 nella sua management console, per gestire le zone e i record dovremmo affidarci ad un set di perl script e file xml di configurazione.

Dovremmo perché in realtà, grazie alle API di Route53, possiamo usare il meraviglioso [Interstate 53](https://www.interstate53.com/) che ci permette di gestire le nostre zone tramite una facile ed intuitiva interfaccia web. [![Interstate 53 control panel ](http://dema.tv/wp-content/uploads/2011/09/clipboard011.jpg)](http://dema.tv/wp-content/uploads/2011/09/clipboard011.jpg)

Il servizio di Interstate53 è gratuito anche se una donazione a chi ha pensato questo meraviglioso tool è direi quanto meno obbligata.

Rimane ora la questione più importante: perché usare un servizio DNS quando il nostro registrar di solito ne fornisce uno e di solito sempre compreso nel prezzo? La risposta è semplice: affidabilità. Il DNS è il vero cuore della rete.

Senza DNS i siti diventano irraggiungibili;  con un DNS oberato di richieste il nostro sito web rallenta, non riesce ad essere contattato dai nostri visitatori. Un server DNS distribuito e scalabile come quello di Amazon Route53 permette di avere sempre la minor latenza in qualsiasi zona del pianeta e in qualsiasi condizione di traffico.

Insomma una vera e propria infrastuttura da big player internet alla portata di tutte le tasche.
