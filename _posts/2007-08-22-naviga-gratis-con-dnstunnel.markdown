---
author: dema
comments: true
date: 2007-08-22 04:53:14+00:00
layout: post
slug: naviga-gratis-con-dnstunnel
title: Naviga gratis con dnstunnel
wordpress_id: 91
categories:
- dnstunnel
- export
- fon
- fonera
- ssh
- udp 53
---

Il titolo è abbastanza interessante in effetti .

Oggi durante il quotidiano chiacchiericcio su [twitter](http://twitter.com/abeggi/statuses/217664762) , il vate [Andrea Beggi](http://andreabeggi.net) :) se n'è uscito con la possibilità di **bypassare l'autenticazione** dei captive portal di moltissimi hotspot a pagamento , sfruttando il fatto che in quasi tutti **è permesso il traffico attraverso la porta 53** .

Ebbene tutto ciò è fattibile , ma ci vuole un manico decisamente non comune. Per chi si vuole cimentare , è tutto spiegato [qui](http://dnstunnel.de/) .

C'è anche un simpatico video dimostrativo che può essere scaricato [qui](http://dema.tv/wp-content/uploads/2007/08/dns_tunneling_example.avi).

Ho controllato inoltre se sul versante [Fon](http://fon.com) si incontra questa possibilità di **navigare a sbafo**.

Ebbene sì anche su fon è possibile , in virtù di questi due settaggi .

Il primo in **chillispot**


<blockquote>

>     
>     uamanydns
> 
> 
</blockquote>




    
    Il secondo nelle regole predefinite di iptables della fonera




<blockquote>

>     
>     iptables -A NET_ACCESS -p udp --dport 53 -j ACCEPT
> 
> 

>     
>     iptables -A NET_ACCESS -p tcp --dport 53 -j ACCEPT
> 
> 
</blockquote>


Per poter inibire la navigazione tramite tunnel dns , sarebbe sufficiente levare il parametro _uamanydns_ dal chilli.conf
