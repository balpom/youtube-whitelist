# Как запретить видео со всех каналов на Youtube и разрешить видео только из каналов из белого списка

## Принцип работы
Идея, положенная в основу моего решения, базируется на перехвате траффика (html страниц) Youtube средствами прокси-сервера Privoxy и инжектированию в них (посредством специального фильтра для privoxy) java-скриптов, делающих всю работу на стороне клиента.

## Как организовать белый список каналов Youtube

### Предварительная подготовка
Прежде всего, у вас должен быть работающий прокси-сервер Privoxy, загружаемый с вашей операционной системой, и самоподписанный SSL-сертификат, добавленный в доверенные.

Весь сетевой трафик должен направляться через этот прокси-сервер.

Подробная инструкция как под Windows создать сертификат, установить Privoxy и направить через него трафик, а также как правильно предпринять прочие необходимые действия и меры, есть у меня на сайте: [https://www.balpom.ru/whitelist/](https://www.balpom.ru/whitelist/)

### Как подключить youtube.filter к Privoxy.
Повторюсь: подробная инструкция как всё правильно настроить есть у меня на сайте: [https://www.balpom.ru/whitelist/](https://www.balpom.ru/whitelist/)

Ниже я коротко покажу лишь сам принцип.

Скачайте *youtube.filter* в директорию с Privoxy.
Добавьте его в файл *config.txt* перед строкой *filterfile default.filter*:
```bash
filterfile youtube.filter
```

Откройте файл *user.action*. В конце файла добавьте следующие действия:
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

### Послесловие
Снова повторюсь: ваша инсталляция Privoxy ДОЛЖНА иметь самоподписанный SSL sertificate, добавленный:
1) В системные доверенные сертификаты.
2) В файл Privoxy *config.txt*.

Также не забудьте очистить кэш браузера!
