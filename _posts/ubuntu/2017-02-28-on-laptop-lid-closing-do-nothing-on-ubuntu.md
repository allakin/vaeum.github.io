---
title: 'Как в Ubuntu, при закрытии крышки, не переводить компьютер ни в какие режимы'
layout: post
categories: Linux
tags: Ubuntu Vim Terminal
description: >-
  Бывают случаи, что надо выключить действия при закрытии крышки ноутбука. В
  Ubuntu это можно сделать несколькими способами
published: true
---

В операционной системе Ubuntu есть функция для перевода ноутбука/нетбука в
Спящий/Ждущий режимы, и она включена по умолчанию. Если вам не нужно,чтобы, после
закрытия крышки, ноутбук "засыпал", то можно решить эту проблему несколькими
способами, в зависимости от того, какая у вас стоит версия этой системы.

Есть несколько версий - десктопная и серверная. На серверной работает только
1 способ - через терминал. А на десктопной работают оба способа, т.к. есть
графический интерфейс.

И так начнем с десктопной, на ней это делается быстрее всего. Для начала откроем
"Параметры системы", потом "Питание" и в выпадающем меню "Переводить в ждущий
режим при бездействии через", выбираем "Не переводить в ждущий режим".

![Выбираем "Не переводить в ждущий режим"](/images/post/on-laptop-lid-closing-do-nothing-on-ubuntu-image1.png)

Это же можно сделать и через терминал, этот способ подходит как для десктопной
версии, так и для серверной. Всего лишь надо изменить настройку в файле **logind.conf**,
который находится в папке **/etc/systemd/**. Нужно выполнить команду от имени
администратора, т.е. с "sudo", с помощью своего редактора, у меня это Vim.

```bash
sudo vim /etc/systemd/logind.conf
```

После открытия файла в редакторе, надо найти следующюю строчку

```
#HandleLidSwitch=suspend
```

И заменить ее на эту строку

```
HandleLidSwitch=ignore
```
Должно получиться вот так:

![Конечный результат](/images/post/on-laptop-lid-closing-do-nothing-on-ubuntu-image2.png)

Отличия второй строки от первой - в отсутсвии знака `#` впереди и вместо слова `suspend`,
стоит слово `ignore`.

После замены, нужно сохранить этот файл. В VIM для этого есть команда `:wq`.

А так же нам надо остановить сервис **Login Manager**.

```
sudo service systemd-logind stop
```

И после запускаем этот сервис

```
sudo service systemd-logind start
```


И для уверености можно перезагрузить сам ноутбук, командой

```
sudo reboot
```

В этой короткой статье мы отключили переход в спящий режим операционной системы
Ubuntu, эта возможность нам может понадобиться, например, при запуске своего сервера,
и нам не нужно, чтобы он выключался, когда закроется крышка.

Оставайтесь с нами, и читайте наш блог.
