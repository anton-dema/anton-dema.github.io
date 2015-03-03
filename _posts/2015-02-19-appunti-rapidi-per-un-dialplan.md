---
title: Appunti rapidi per un dialplan 
author: Dema
layout: post
date: 2015-02-19 09:28:24+00:00
header-img: img/phone.jpg
tags: 
- appunti
- dialplan
---

Era veramente da un bel po' di tempo che non popolavo il mio blog con appunti di utilità - mia, di altri non lo so, ma penso che nel mare magnum di questo internet, non si può proprio mai sapere - ed ora è venuto il momento di farlo. 

Questo è un estratto da una _note to self_ che mi sono fatto su **Evernote** non più tardi di due giorni fa.    
Eccovela, in tutto il suo splendore e nelle sue, immancavoli, imprecisioni (oh, ma l'ho provato e funziona per davvero, solo che magari i veri _asterisk gurus_ si faranno una risata e biasimeranno l'ineleganza di questa soluzione)  

Supponiamo che si voglia uscire su un trunk voip ben definito. 
Dobbiamo prima stabilire una outbound rule del tipo: 

    999|1NXXNXXXXXX
    999|NXXNXXXXXX
    999|.

Che, sostanzialmente, istruisce di digitare 999 per utilizzare il trunk con le combinazioni di cifre elencate (anche se basterebbe la `999|.` per fare un _catch all_ di tutti i numeri, almeno in fase di test)          
Per evitare di digitare ogni volta 999 sul terminale, basta approntare un dial plan così formato, in modo che la macchina esegua per noi il tedioso inserimento delle cifre sulla tastiera: 

    (<:999>*xx|<:999>[49]11S0|<:999>1xxx[2-9]xxxxxxS0|<:999>[2-9]xxxxxxxxxS0|<:999>xxxxxxxxxxxx. | <:999>x.)

Provato, funziona! 

PS: per broker che esigono anche l'inserimento del prefisso internazionale, la regola di outbount sul centralino freepbx diventa: 

    (0039)999|1NXXNXXXXXX
    (0039)999|NXXNXXXXXX
    (0039)999|.

nel caso dell'italia, ovviamente.   
Rimane il problema delle chiamate internazionali, ma lo vedremo più avanti con un dialplan appositamente forgiato 

IMPORTANTE
il default dialplan di un cisco SPA922 (e similari) è: 
    
    (*xx|[3469]11|0|00|[2-9]xxxxxx|1xxx[2-9]xxxxxx|xxxxxxxxxxxx.)   