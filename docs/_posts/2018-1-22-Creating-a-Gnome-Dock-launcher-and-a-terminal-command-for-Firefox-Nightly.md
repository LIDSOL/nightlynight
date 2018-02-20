---
layout: post
title: Creating a Gnome Dock launcher and a terminal command for Firefox Nightly
---

Pascal Chevrel | January 22, 2018 | [Original post](https://blog.nightly.mozilla.org/2018/01/22/335/)

**About 18 months ago, Wil Clouser wrote a blog post on this very blog titled [Getting Firefox Nightly to stick to Ubuntu’s Unity Dock](https://blog.nightly.mozilla.org/2016/09/19/getting-firefox-nightly-to-stick-to-ubuntus-unity-dock/).**

Fast forward to 2018, Ubuntu announced last year that it is giving up on their Unity desktop and will use Gnome Shell instead. Indeed,  the last Ubuntu 17.10 release uses Gnome Shell by default. That means that the article above is slightly outdated now as its .desktop file was targeting the Unity environment which had its own quirks.

The main annoyance that Firefox Nightly users on Ubuntu noticed after an upgrade to 17.10 was that with every automatic update of the browser (that is to say, everyday) Nightly would restart with a second Nightly icon in the Dock instead of reusing the existing one. This can be solved with a small adjustment to the .desktop file and I thought it might be a good idea to share my own desktop file, here it is:

```
[Desktop Entry]
Version=1.0
Name=Firefox Nightly
Name[fr]=Firefox Nightly
Comment=Browse the World Wide Web
Comment[fr]=Naviguer sur le Web
GenericName=Web Browser
GenericName[fr]=Navigateur Web
Keywords=Internet;WWW;Browser;Web;Explorer
Keywords[fr]=Internet;WWW;Browser;Web;Explorer;Fureteur;Surfer;Navigateur
Type=Application
Exec=/home/pascalc/apps/firefoxnightly/firefox -P MyNightlyProfile %u
Terminal=false
X-MultipleArgs=false
Icon=/home/pascalc/apps/firefoxnightly/browser/chrome/icons/default/default128.png
Categories=GNOME;GTK;Network;WebBrowser;
Actions=ProfileManager;new-window;new-private-window;
MimeType=text/html;text/xml;application/xhtml+xml;application/xml;application/rss+xml;application/rdf+xml;image/gif;image/jpeg;image/png;x-scheme-handler/http;x-scheme-handler/https;x-scheme-handler/ftp;x-scheme-handler/chrome;video/webm;application/x-xpinstall;
StartupNotify=true
StartupWMClass=Nightly

[Desktop Action ProfileManager]
Name=Profile Manager
Name[fr]=Gestionnaire de profil
Exec=/home/pascalc/apps/firefoxnightly/firefox -P

[Desktop Action new-window]
Name=New window
Name[fr]=Nouvelle fenêtre
Exec=/home/pascalc/apps/firefoxnightly/firefox -new-window -P MyNightlyProfile

[Desktop Action new-private-window]
Name=New private window
Name[fr]=Nouvelle fenêtre privée
Exec=/home/pascalc/apps/firefoxnightly/firefox -private-window -P MyNightlyProfile
```

The command in this file that allows grouping all Firefox Nightly windows under one icon is `StartupWMClass=Nightly`. You can also notice that I added localized strings in French for my browser for a better integration in my desktop environment.

As my desktop file above shows, I have installed Nightly in my home and not in opt, I am the only user of my machine, don’t use multiple accounts and I just find it more convenient. If you install Firefox Nightly outside of your home (typically `/opt/firefoxnightly`), don’t forget to give your installation your user rights otherwise you will never get daily updates:

```
sudo chown -R $USER:$USER /opt/firefoxnightly
```

One last thing about that `.desktop` file, the path to the icon recently changed, so if you used Wil’s article to create your launcher and you lost your nice Nightly logo recently, this is because the file is no longer located in `browser/icons/mozicon128.png` but is now in `browser/chrome/icons/default/default128.png`.

And what if you want to launch Nightly from your terminal? The Firefox binary is already associated with the distro-provided Firefox so using `firefox` in the command line would launch the regular Firefox, not Firefox Nightly. The solution I use is to create a bash script in `~/bin/nightly` for Firefox Nightly, make it an executable (`chmod +x ~/bin/nightly`) and call `nightly` from the terminal:

```
#!/bin/bash
#
# Launch Firefox Nigthly with the Nightly profile.
#

$HOME/apps/firefoxnightly/firefox -P MyNightlyProfile $1
```

You can also pass an url as parameter:

```
~ $ nightly https://www.mozilla.org 
```
