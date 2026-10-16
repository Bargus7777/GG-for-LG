# GG for LG

Неофициальный клиент [GoodGame.ru](https://goodgame.ru/) для телевизоров LG с webOS. Позволяет смотреть прямые трансляции с телевизора без браузера.

> Проект не связан с GoodGame или LG. Работоспособность зависит от доступности потоков GoodGame и поддержки HLS на конкретном телевизоре.

## Возможности

- Каталог онлайн-трансляций, раздел по играм, случайный стрим и локальное избранное.
- Управление пультом LG и указателем Magic Remote.
- Автоматическое восстановление воспроизведения при остановке или сетевой ошибке; обновление потока после возвращения из другого приложения.
- Журнал критических событий: **«О программе» → «Открыть журнал событий»**. Хранятся последние 60 записей на самом ТВ.

## Установка

1. Скачайте IPK из [последнего релиза](https://github.com/Bargus7777/GG-for-LG/releases/latest).
2. На телевизоре установите приложение **Developer Mode** и включите **Dev Mode Status** и **Key Server**. Компьютер и телевизор должны быть в одной сети.
3. Установите IPK через [webOS Dev Manager](https://github.com/webosbrew/dev-manager-desktop/releases) либо через [webOS CLI](https://www.npmjs.com/package/@webos-tools/cli):

   ```powershell
   npm.cmd install -g @webos-tools/cli
   ares-install.cmd --device lg_tv "C:\путь\к\ggforlg_2.2.3.ipk"
   ```

   Перед установкой через CLI добавьте телевизор в `ares-setup-device.cmd` и получите ключ командой `ares-novacom.cmd --device lg_tv --getkey`; имя `lg_tv` в примере замените своим. [Официальная инструкция LG](https://webostv.developer.lge.com/develop/getting-started/developer-mode-app).

> Приложения, установленные через Developer Mode, могут удалиться после отключения этого режима.

## Если возникают проблемы

- **ТВ не подключается:** проверьте IP-адрес, порт 9922, режим разработчика и Key Server.
- **Поток не запускается:** откройте встроенный журнал через «О программе» и проверьте тип HLS-ошибки и HTTP-код, если он есть. `manifestLoadError` и `fragLoadError` сами по себе не позволяют установить причину: это может быть сеть, CDN, срок действия ссылки или ошибка воспроизведения.
- **Ошибка `Unknown DH group` в CLI:** в установленном webOS CLI файл `lib/base/novacom.js` может предлагать первым устаревший SSH-алгоритм `diffie-hellman-group1-sha1`. Для телевизора с поддержкой ECDH переместите `ecdh-sha2-nistp256` на первое место списка `this.target.algorithms.kex`.

## Для разработчиков

Приложение написано на HTML/CSS/JavaScript: основной интерфейс и логика находятся в `index.html`, метаданные — в `appinfo.json`, библиотека HLS — в `hls.min.js`.

Для сборки IPK установите [webOS CLI](https://www.npmjs.com/package/@webos-tools/cli) и запускайте `ares-package.cmd` для каталога с `appinfo.json`, `index.html`, `app.js`, `hls.min.js` и изображениями. Готовый пакет для установки публикуется во вкладке **Releases**, а не среди исходных файлов репозитория.
