---
author: dema
comments: true
date: 2008-05-20 07:30:24+00:00
layout: post
slug: aggiornare-eeepc-ad-ubuntu-804
title: 'Aggiornare eeepc ad Ubuntu 8.04 '
wordpress_id: 211
categories:
- eee
- eeepc
- ubuntu
- ubuntu 8.04
- upgrade
tags:
- eee
- eeepc
- ubuntu
- ubuntu 8.04
- upgrade
---

**Premessa **:



	
  1. eeePC con (X)(K)Ubuntu 7.10 installata

	
  2. connessione alla rete via ethernet funzionante


**Procedimento **:



	
  1. Se aggiorneremo alla nuova release di Ubuntu via update-manager con molta probabilità **non ci basterà lo spazio su disco**. Per recuperarlo io suggerisco di sradicare completamente il desktop-environment

	
  2. A seconda del desktop che abbiamo installato ecco i comandi per disinstallare e fare spazio :
**Ubuntu con Gnome**: [sourcecode language='cpp']

sudo  apt-get remove alacarte app-install-data-commercial apport-gtk  apturl at-spi bittorrent bluez-gnome brltty-x11 bug-buddy  capplets-data cli-common compiz compiz-core  compiz-fusion-plugins-extra compiz-fusion-plugins-main compiz-gnome  compiz-plugins consolekit contact-lookup-applet  cupsys-driver-gutenprint dcraw deskbar-applet displayconfig-gtk  diveintopython doc-base docbook-xml ekiga eog espeak espeak-data  evince evolution evolution-common evolution-data-server  evolution-data-server-common evolution-exchange evolution-plugins  evolution-webcal example-content f-spot fast-user-switch-applet  feisty-gdm-themes  file-roller firefox firefox-gnome-support gamin gcalctool  gconf-editor   gconf2 gconf2-common gdebi gdm gedit gedit-common gimp gimp-data  gimp-print gimp-python gksu gnome-about gnome-accessibility-themes   gnome-app-install gnome-applets gnome-applets-data  gnome-btdownload gnome-cards-data gnome-control-center  gnome-desktop-data   gnome-doc-utils gnome-games gnome-games-data gnome-icon-theme  gnome-keyring gnome-keyring-manager gnome-mag gnome-media  gnome-media-common   gnome-menus gnome-mime-data gnome-mount gnome-netstatus-applet  gnome-nettool gnome-orca gnome-panel gnome-panel-data gnome-pilot   gnome-pilot-conduits gnome-power-manager gnome-screensaver  gnome-session gnome-spell gnome-system-monitor gnome-system-tools   gnome-terminal gnome-terminal-data gnome-themes gnome-user-guide  gnome-utils gnome-volume-manager gstreamer0.10-alsa  gstreamer0.10-esd   gstreamer0.10-gnomevfs gstreamer0.10-plugins-base  gstreamer0.10-plugins-base-apps gstreamer0.10-plugins-good  gstreamer0.10-tools   gstreamer0.10-x gthumb gtk2-engines gtk2-engines-pixbuf  gtk2-engines-ubuntulooks gtkhtml3.14 gucharmap guile-1.6-libs  gutsy-wallpapers   hal-cups-utils hal-device-manager human-icon-theme human-theme  hwdb-client-gnome language-selector libaa1 libalut0 libart2.0-cil   libatspi1.0-0 libavahi-glib1 libavc1394-0 libbeagle0 libbonobo2-0  libbonobo2-common libbonoboui2-0 libbonoboui2-common libcaca0   libcairo-perl libcairomm-1.0-1 libcamel1.2-10 libcdio6  libcompizconfig-backend-gconf libcompizconfig0 libcroco3 libcucul0  libdecoration0   libdeskbar-tracker libdjvulibre15 libdv4 libebook1.2-9  libecal1.2-7 libedata-book1.2-2 libedata-cal1.2-6  libedataserver1.2-9   libedataserverui1.2-8 libeel2-2 libeel2-data libegroupwise1.2-13  libenchant1c2a libespeak1 libexchange-storage1.2-3 libgail-common   libgail-gnome-module libgail18 libgamin0 libgconf2-4  libgconf2.0-cil libgda3-3 libgda3-common libgdl-1-0 libgdl-1-common  libgdl-gnome-1-0   libgimp2.0 libgksu1.2-1 libgksu2-0 libgksuui1.0-1 libglade2-0  libglade2.0-cil libglew1.4 libglib-perl libglib2.0-cil  libglibmm-2.4-1c2a   libgmime-2.0-2 libgmime2.2-cil libgnome-desktop-2  libgnome-keyring0 libgnome-mag2 libgnome-media0 libgnome-menu2  libgnome-pilot2   libgnome-speech7 libgnome-vfs2.0-cil libgnome-window-settings1  libgnome2-0 libgnome2-canvas-perl libgnome2-common libgnome2-perl   libgnome2-vfs-perl libgnome2.0-cil libgnomecanvas2-0  libgnomecanvas2-common libgnomecups1.0-1 libgnomekbd-common  libgnomekbd1   libgnomekbdui1 libgnomeprint2.2-0 libgnomeprint2.2-data  libgnomeprintui2.2-0 libgnomeprintui2.2-common libgnomeui-0  libgnomeui-common   libgnomevfs2-0 libgnomevfs2-bin libgnomevfs2-common  libgnomevfs2-extra libgsf-1-114 libgsf-1-common libgtk2-perl  libgtk2.0-bin   libgtk2.0-cil libgtkhtml2-0 libgtkhtml2.0-cil libgtkhtml3.14-19  libgtkhtml3.8-15 libgtkmm-2.4-1c2a libgtksourceview-common   libgtksourceview1.0-0 libgtksourceview2.0-0  libgtksourceview2.0-common libgtkspell0 libgtop2-7 libgtop2-common  libgucharmap6   libguile-ltdl-1 libgutenprintui2-1 libhesiod0 libhsqldb-java  libidl0 libiec61883-0 libkpathsea4 liblaunchpad-integration0  liblircclient0   liblpint-bonobo0 libmetacity0 libmono-cairo1.0-cil  libmono-corlib1.0-cil libmono-corlib2.0-cil libmono-data-tds2.0-cil   libmono-security2.0-cil libmono-sharpzip2.84-cil  libmono-sqlite2.0-cil libmono-system-data2.0-cil  libmono-system-web2.0-cil   libmono-system1.0-cil libmono-system2.0-cil libmono0  libmono2.0-cil libnautilus-burn4 libnautilus-extension1  libndesk-dbus-glib1.0-cil   libndesk-dbus1.0-cil libnet-dbus-perl libnm-glib0 libnotify1  liboil0.3 liboobs-1-3 libopal-2.2 libopenal0a liborbit2  libpam-gnome-keyring   libpanel-applet2-0 libpcrecpp0 libpisock9 libpisync0  libpoppler-glib2 libpt-1.10.0 libpt-plugins-alsa libpt-plugins-v4l  libpt-plugins-v4l2   libpurple0 libqthreads-12 librarian0 librsvg2-2 librsvg2-common  librsvg2.0-cil libscrollkeeper0 libservlet2.4-java libsexy2  libshout3   libsoup2.2-8 libtotem-plparser7 libtracker-gtk0 libtrackerclient0  libvte-common libvte9 libwavpack1 libwmf0.2-7 libwnck-common  libwnck22   libxevie1 libxklavier11 libxml-twig-perl libxml2-utils libxres1  libzephyr3 metacity metacity-common mono-common mono-gac mono-jit   mono-runtime nautilus nautilus-cd-burner nautilus-data  nautilus-sendto network-manager-gnome notification-daemon o3read  onboard   openoffice.org openoffice.org-base openoffice.org-evolution  openoffice.org-gnome openoffice.org-gtk pidgin pidgin-data  pkg-config   python-at-spi python-bittorrent python-cairo python-cups  python-gconf python-gdbm python-glade2 python-gmenu python-gnome2   python-gnome2-desktop python-gnome2-extras python-gnomecanvas  python-gobject python-gst0.10 python-gtk2 python-gtkhtml2   python-launchpad-integration python-notify python-numeric  python-orca-brlapi python-pygtksourceview python-pyorbit python-sexy   python-virtkey python-vte python-xml restricted-manager rhythmbox  rss-glx scim scim-gtk2-immodule scim-modules-socket  scim-modules-table   scim-tables-additional screensaver-default-images scrollkeeper  serpentine sgml-data shared-mime-info software-properties-gtk  sound-juicer   ssh-askpass-gnome synaptic system-config-printer  system-tools-backends tangerine-icon-theme tomboy totem  totem-gstreamer totem-mozilla   tracker tracker-search-tool tsclient ubufox ubuntu-artwork  ubuntu-desktop ubuntu-docs ubuntu-sounds update-manager  update-notifier   usplash-theme-ubuntu vino whois xbitmaps xdg-user-dirs  xdg-user-dirs-gtk xsane xsane-common xscreensaver-data  xscreensaver-gl xsltproc   xterm xvnc4viewer yelp zenity[/sourcecode]

**Xubuntu con Xfxe4**

[sourcecode language='cpp']

sudo  apt-get remove   abiword abiword-common abiword-plugins  app-install-data-commercial apport-gtk apturl brasero  cupsys-driver-gutenprint dbus-x11   displayconfig-gtk doc-base docbook-xml evince-gtk example-content  feisty-gdm-themes file-roller firefox gamin gcalctool gconf2   gconf2-common gdebi gdm gimp gimp-data gimp-print gksu  gnome-accessibility-themes gnome-app-install gnome-cards-data  gnome-games   gnome-games-data gnome-icon-theme gnome-keyring gnome-media-common  gnome-mime-data gnome-mount gnome-screensaver gnome-system-monitor   gnome-system-tools gnome-themes gnumeric-common gnumeric-gtk  gqview gstreamer0.10-alsa gstreamer0.10-gnomevfs  gstreamer0.10-plugins-base   gstreamer0.10-plugins-good gstreamer0.10-x gtk2-engines  gtk2-engines-murrine gtk2-engines-pixbuf gtk2-engines-ubuntulooks   gtk2-engines-xfce gucharmap guile-1.6-libs gutsy-wallpapers  hal-cups-utils human-icon-theme human-theme language-selector libaa1   libaiksaurus-1.2-0c2a libaiksaurus-1.2-data  libaiksaurusgtk-1.2-0c2a libavahi-glib1 libavc1394-0 libbonobo2-0  libbonobo2-common   libbonoboui2-0 libbonoboui2-common libcaca0 libcairo-perl  libcairomm-1.0-1 libcdio6 libcroco3 libcucul0 libdjvulibre15 libdv4   libenchant1c2a libexo-0.3-0 libgail-common libgail18 libgamin0  libgconf2-4 libgdome2-0 libgdome2-cpp-smart0c2a libgimp2.0  libgksu2-0   libglade2-0 libglib-perl libglib2.0-data libglibmm-2.4-1c2a  libgnome-desktop-2 libgnome-keyring0 libgnome-media0 libgnome-menu2   libgnome2-0 libgnome2-canvas-perl libgnome2-common libgnome2-perl  libgnome2-vfs-perl libgnomecanvas2-0 libgnomecanvas2-common   libgnomecups1.0-1 libgnomekbd-common libgnomekbd1 libgnomekbdui1  libgnomeprint2.2-0 libgnomeprint2.2-data libgnomeprintui2.2-0   libgnomeprintui2.2-common libgnomeui-0 libgnomeui-common  libgnomevfs2-0 libgnomevfs2-common libgoffice-0-common  libgoffice-gtk-0-4   libgsf-1-114 libgsf-1-common libgsf-gnome-1-114 libgtk2-perl  libgtk2.0-bin libgtkhtml2-0 libgtkmathview0c2a libgtkmm-2.4-1c2a   libgtksourceview-common libgtksourceview1.0-0 libgtkspell0  libgtop2-7 libgtop2-common libgucharmap6 libguile-ltdl-1  libgutenprintui2-1   libhesiod0 libidl0 libiec61883-0 libkpathsea4  liblaunchpad-integration0 liblink-grammar4 liblircclient0  libmetacity0 libnautilus-burn4   libnautilus-extension1 libnet-dbus-perl libnm-glib0 libnotify1  liboil0.3 liboobs-1-3 liborbit2 libots0 libpanel-applet2-0  libpcrecpp0   libpoppler-glib2 libpurple0 libqthreads-12 librsvg2-2  librsvg2-common libscrollkeeper0 libsexy2 libshout3 libt1-5 libtagc0   libthunar-vfs-1-2 libtotem-plparser7 libvte-common libvte9  libwavpack1 libwmf0.2-7 libwnck-common libwnck22 libwpd-stream8c2a   libxfce4mcs-client3 libxfce4mcs-manager3 libxfce4util4  libxfcegui4-4 libxklavier11 libxml-twig-perl libxres1 libzephyr3   link-grammar-dictionaries-en metacity-common mousepad  mozilla-thunderbird network-manager-gnome notification-daemon  onboard orage pidgin   pidgin-data python-cairo python-cups python-exo python-gconf  python-gdbm python-glade2 python-gnome2 python-gnome2-desktop   python-gnomecanvas python-gobject python-gst0.10 python-gtk2  python-gtkhtml2 python-launchpad-integration python-notify  python-numeric   python-pyorbit python-sexy python-virtkey python-vte python-xml  restricted-manager scim scim-gtk2-immodule scim-modules-socket   scim-modules-table scim-tables-additional  screensaver-default-images scrollkeeper sgml-data shared-mime-info  software-properties-gtk   synaptic system-config-printer system-tools-backends  tango-icon-theme tango-icon-theme-common thunar  thunar-archive-plugin thunar-data   thunar-media-tags-plugin thunar-volman thunderbird totem  totem-xine ubufox ubuntu-artwork update-manager update-notifier  vim-runtime   xbitmaps xfce4-appfinder xfce4-battery-plugin xfce4-clipman-plugin  xfce4-cpugraph-plugin xfce4-dict-plugin xfce4-fsguard-plugin   xfce4-icon-theme xfce4-mailwatch-plugin xfce4-mcs-manager  xfce4-mcs-plugins xfce4-mixer xfce4-mixer-alsa xfce4-mount-plugin   xfce4-netload-plugin xfce4-notes-plugin xfce4-panel  xfce4-places-plugin xfce4-quicklauncher-plugin  xfce4-screenshooter-plugin   xfce4-session xfce4-smartbookmark-plugin xfce4-systemload-plugin  xfce4-terminal xfce4-utils xfce4-verve-plugin xfce4-weather-plugin   xfce4-xkb-plugin xfdesktop4 xfdesktop4-data xfprint4 xfwm4  xfwm4-themes xscreensaver-data xscreensaver-gl xterm  xubuntu-artwork-usplash   xubuntu-default-settings xubuntu-desktop xubuntu-docs xvnc4viewer  zenity[/sourcecode]

	
  3. Usciamo dalla modalità grafica con CTRL+ALT+F1 e ci logghiamo in console.

	
  4. modifichiamo il file _/etc/apt/sources.list_ cambiando ogni occorrenza di "_gutsy_" con "_hardy_". Io utilizzo l'editor nano e la funzione Cerca+Sostituisci (CTRL+W , CTRL+R)

	
  5. Aggiorniamo la distribuzione con _apt-get update && apt-get dist-upgrade_

	
  6. Una volta terminato il processo riavviamo.

	
  7. Al primo riavvio non avremo un login grafico e problemi con lo splash screen di boot. Ritorniamo alla console con CTRL+ALT+F1 e grazie alla connessione via ethernet preleviamo nuovamente il desktop-environment di nostro gradimento. I comandi per farlo sono _apt-get install ubuntu-desktop_ per Gnome , _apt-get install kubuntu-desktop_ per kde e _apt-get install xubuntu-desktop_ per xfce

	
  8. Entriamo nel nostro nuovo ambiente grafico con _sudo /etc/init.d/gdm restart_

	
  9. Applichiamo una serie di patch per ottimizzare il sistema e far funzionare la scheda wireless
**per Ubuntu Gnome:** [sourcecode language='cpp']`#!/bin/sh
echo ""
echo "*** Ubuntu 8.04 LTS Tweak ***"
echo "***     version 0.0.1     ***"
echo "***      www.x2on.de      ***"
echo ""
echo "thx to http://ubuntu-eee.tuxfamily.org/"
echo "thx to http://code.google.com/p/eee-osd/"
echo ""
echo "** Gnome settings"
echo "* Setting smaller font sizes"
gconftool-2 --set /apps/nautilus/preferences/desktop_font --type string "Sans 8"
gconftool-2 --set /desktop/gnome/interface/document_font_name --type string "Sans 8"
gconftool-2 --set /desktop/gnome/interface/font_name --type string "Sans 8"
gconftool-2 --set /apps/metacity/general/titlebar_font --type string "Sans Bold 8"
gconftool-2 --set /desktop/gnome/interface/monospace_font_name --type string "Monospace 9"
echo "* Smaller toolbars icons only"
gconftool-2 --set /desktop/gnome/interface/toolbar_style --type string "icons"
echo "* Disabling UI sounds"
gconftool-2 --set /desktop/gnome/sound/event_sounds --type bool 0
echo "* Fixing mute key"
gconftool-2 --set /desktop/gnome/sound/default_mixer_tracks --type list --list-type string "[PCM]"
echo "* Fullscreen with -F11"
gconftool-2 --set /apps/metacity/window_keybindings/toggle_fullscreen --type string "F11"
echo "* Setting suspend when closing lid, blank screen"
gconftool-2 --set /apps/gnome-power-manager/actions/sleep_type_battery --type string "suspend"
gconftool-2 --set /apps/gnome-power-manager/actions/sleep_type_ac --type string "suspend"
gconftool-2 --set /apps/gnome-power-manager/buttons/lid_battery --type string "suspend"
gconftool-2 --set /apps/gnome-power-manager/buttons/lid_ac --type string "blank"
gconftool-2 --set /apps/gnome-power-manager/timeout/sleep_computer_ac --type int 0
gconftool-2 --set /apps/gnome-power-manager/timeout/sleep_computer_battery --type int 300
gconftool-2 --set /apps/gnome-power-manager/timeout/sleep_display_ac --type int 300
gconftool-2 --set /apps/gnome-power-manager/timeout/sleep_display_battery --type int 60
echo "* Don't display battery warning"
gconftool-2 --set /apps/gnome-power-manager/notify/low_capacity --type bool 0
echo "* Unconstraining windows to the top of the screen"
gconftool-2 --type bool --set /apps/compiz/plugins/move/allscreens/options/constrain_y 0
echo "Gnome-settings done."
echo "** Installing ACPI modules"
sudo apt-get update
sudo apt-get install -y -f build-essential module-assistant eeepc-acpi-source  --force-yes
sudo m-a a-i eeepc-acpi
sudo cp /etc/modules ~/modules.tmp
sudo chmod 777 ~/modules.tmp
echo "eeepc-acpi" >> ~/modules.tmp
sudo chmod 644 ~/modules.tmp
sudo mv ~/modules.tmp /etc/modules
echo "** Installing WLAN"
wget 'http://snapshots.madwifi.org/special/madwifi-nr-r3366+ar5007.tar.gz'
tar zxvf madwifi-nr-r3366+ar5007.tar.gz
cd madwifi-nr-r3366+ar5007
make clean
make
sudo make install
echo "** Installing OSD"
wget http://eee-osd.googlecode.com/files/eee-osd_2.1-0eeeXubuntu1_i386.deb
sudo dpkg -i eee-osd_2.1-0eeeXubuntu1_i386.deb
echo "** Configuring Sound"
echo "options snd-hda-intel model=3stack-dig" > ~/snd-hda-intel.tmp
sudo mv ~/snd-hda-intel.tmp /etc/modprobe.d/snd-hda-intel
echo "Done! Please reboot now"[/sourcecode]

**Per Xubuntu Xfce**

[sourcecode language='cpp']`#!/bin/sh
echo ""
echo "*** Ubuntu 8.04 LTS Tweak ***"
echo "***     version 0.0.1     ***"
echo "***      www.x2on.de      ***"
echo ""
echo "thx to http://ubuntu-eee.tuxfamily.org/"
echo "thx to http://code.google.com/p/eee-osd/"
echo ""
echo "** Installing ACPI modules"
sudo apt-get update
sudo apt-get install -y -f build-essential module-assistant eeepc-acpi-source  --force-yes
sudo m-a a-i eeepc-acpi
sudo cp /etc/modules ~/modules.tmp
sudo chmod 777 ~/modules.tmp
echo "eeepc-acpi" >> ~/modules.tmp
sudo chmod 644 ~/modules.tmp
sudo mv ~/modules.tmp /etc/modules
echo "** Installing WLAN"
wget 'http://snapshots.madwifi.org/special/madwifi-nr-r3366+ar5007.tar.gz'
tar zxvf madwifi-nr-r3366+ar5007.tar.gz
cd madwifi-nr-r3366+ar5007
make clean
make
sudo make install
echo "** Installing OSD"
wget http://eee-osd.googlecode.com/files/eee … 1_i386.deb
sudo dpkg -i eee-osd_2.1-0eeeXubuntu1_i386.deb
echo "** Configuring Sound"
echo "options snd-hda-intel model=3stack-dig" > ~/snd-hda-intel.tmp
sudo mv ~/snd-hda-intel.tmp /etc/modprobe.d/snd-hda-intel
echo "Done! Please reboot now"[/sourcecode]

	
  10. Copiamo il contenuto di uno dei due script qui sopra e salviamo in un file di nome tweakubuntu.sh . Rendiamo eseguibile con chmod +x tweakubuntu.sh e eseguiamo lo script con ./tewakubuntu.sh . Lo script chiederà la password quando ci sarà necessità di operare in modalità sudoers.

	
  11. Ancora un reboot

	
  12. Fatto , abbiamo aggiornato perfettamente a 8.10 , senza intoppi e problemi.


Io personalmente ho perso  ,come accennato sopra , lo splash screen di boot , ma non mi sono fatto eccessivi problemi , in quanto a me **piace vedere l'output di boot a schermo** , e per questo scopo ho modificato le righe di _/boot/grub/menu.lst_ levando _splash _e aggiungendo _vga=normal_ e clocksource=hpet.

Credits: [](http://www.eeepc.it/aggiornare-eeexubuntu-710-alla-804/)

[howto di eeepc.it](http://www.eeepc.it/aggiornare-eeexubuntu-710-alla-804/)

[howto di nerdlogger.com](http://www.nerdlogger.com/2008/05/asus-eeepc-installing-ubuntu-804.html)
