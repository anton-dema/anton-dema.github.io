---
author: dema
comments: true
date: 2014-07-10 12:37:22+00:00
layout: post
slug: inviare-email-shell-gmail
title: 'Inviare email da shell con Gmail '
wordpress_id: 2545
header-img: img/terminale.jpg
---

Pillolina da lame sysadmin.

Per inviare una mail con shell con mailutils, openssl e Gmail:

  1. Creare la directory in home che conterrà i certificati:


    $ mkdir ~/.certs 
    $ certutil -N -d ~/.certs


  2. Importare il certificato di gmail:
    

    $ echo -n | openssl s_client -connect smtp.gmail.com:465 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > ~/.certs/gmail.crt
    $ certutil -A -n "Google Internet Authority" -t "C,," -d ~/.certs -i ~/.certs/gmail.crt



  3. Editare il file /etc/mail.rc ed aggiungere il profilo gmail da invocare con il comando mailx -A.  

Inserire alla fine del file:

    account gmail { 
    set smtp-use-starttls
    set ssl-verify=ignore
    set smtp-auth=login
    set smtp=smtp://smtp.gmail.com:587
    set from="username@gmail.com(Mighty Me)"
    set smtp-auth-user=me@gmail.com
    set smtp-auth-password=myawesomepassword
    set ssl-verify=ignore
    set nss-config-dir=/home/myhome/.certs
    }

  4. Inviare email da script o shell come di consueto, avendo cura di indicare come flag del comando mail -A gmail.
Quindi la riga di comando dovrebbe risultare come segue:


    $ cat test.txt | mailx -A gmail -s "report su test" someone@somemail.com


###### credits [coderwall](https://coderwall.com/p/ez1x2w)
