# How to disable videos from all Youtube channels and leave only some (whitelisted) channels

## Principle of work
The idea behind my solution is based on intercepting Youtube traffic (HTML pages) using the Privoxy proxy server and injecting javascript's into them (using a special filter for Privoxy) that do all the work on the client side.

## How to organize a whitelist of Youtube channels

### Preparing
First of all, you must have a working Privoxy proxy server, bootable with your OS, and a self-signed SSL certificate added to the trusted ones.

All network traffic must be routed through a this proxy.

Detailed instructions on how (under Windows) to create and install a certificate, install Privoxy and send traffic through it, as well as how to properly take other necessary actions, are available on my website:

русский: [https://www.balpom.ru/whitelist/](https://www.balpom.ru/whitelist/)

english: [https://www.balpom.ru/en/whitelist/](https://www-balpom-ru.translate.goog/whitelist/?_x_tr_sl=ru&_x_tr_tl=en&_x_tr_hl=en&_x_tr_pto=wapp)

deutsche: [https://www.balpom.ru/de/whitelist/](https://www-balpom-ru.translate.goog/whitelist/?_x_tr_sl=ru&_x_tr_tl=de&_x_tr_hl=de&_x_tr_pto=wapp)

italiano: [https://www.balpom.ru/it/whitelist/](https://www-balpom-ru.translate.goog/whitelist/?_x_tr_sl=ru&_x_tr_tl=it&_x_tr_hl=it&_x_tr_pto=wapp)

español: [https://www.balpom.ru/es/whitelist/](https://www-balpom-ru.translate.goog/whitelist/?_x_tr_sl=ru&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=wapp)

français: [https://www.balpom.ru/fr/whitelist/](https://www-balpom-ru.translate.goog/whitelist/?_x_tr_sl=ru&_x_tr_tl=fr&_x_tr_hl=fr&_x_tr_pto=wapp)

český: [https://www.balpom.ru/cz/whitelist/](https://www-balpom-ru.translate.goog/whitelist/?_x_tr_sl=ru&_x_tr_tl=cs&_x_tr_hl=cs&_x_tr_pto=wapp)


### How to connect youtube.filter to Privoxy.
Repeat: links to the detailed instructions see above. Below I will briefly show the principle itself.

Download *youtube.filter* into Privoxy directory.
Add them into *config.txt* file before *filterfile default.filter* string:
```bash
filterfile youtube.filter
```

Open *user.action* file. At the end of file, we add the following actions:
```bash
{+https-inspection}
.

{+ignore-certificate-errors}
.

{+redirect{s@^https://www.youtu.be@https://www.youtube.com@}}
.youtu.be/

{+redirect{s@^https://youtu.be@https://www.youtube.com@}}
.youtu.be/

{+redirect{s@^https://www.youtube-nocookie.com@https://www.youtube.com@}}
.youtube-nocookie.com/

{+redirect{s@^https://youtube-nocookie.com@https://www.youtube.com@}}
.youtube-nocookie.com/

{ +block{Total ban} +add-header{HTTP/1.0 404 Not Found} }
www.youtube.com/gaming
music.youtube.com
.youtube.com/error
.youtube.com/.*\/haxer-
.ytimg.com/an_webp/
play.google.com
studio.youtube.com
www.youtube.com/ads/
.finditnowonline.com
www.youtube.com/youtubei/.*\/log_event
www.youtube.com/api/stats/qoe
www.youtube.com/api/stats/watchtime
www.youtube.com/api/stats/atr
www.youtube.com/api/stats/playback
www.youtube.com/api/timedtext
www.youtube.com/ptracking
www.youtube.com/pagead/
www.google.ru/pagead/lvz
www.youtube.com/s/desktop/.*\/www-tampering.js
www.youtube.com/sw.js
www.youtube.com/s/player/.*\/miniplayer.js
www.youtube.com/s/desktop/.*\/spf.js
.youtube.com/sw.js
.youtube.com/?feature=ytca
.youtube.com/manifest.webmanifest

{ +filter{youtube-polymer-block}}
www.youtube.com/$

{ +filter{youtube-player-injection}}
www.youtube.com/s/player/.*\/base.js

{ +filter{youtube-enabled-channels} +filter{youtube-common}}
www.youtube.com/$
www.youtube.com/result
www.youtube.com/results?search_query=
www.youtube.com/@
www.youtube.com/watch
www.youtube.com/shorts/
www.youtube.com/feed/
www.youtube.com/channel/
```

### Afterword
Repeat once again: your Privoxy installation MUST have self-signed SSL sertificate, added:
1) In system trusted sertificates.
2) In Privoxy *config.txt* file.

Also, don't forget to clear the browser cache!


