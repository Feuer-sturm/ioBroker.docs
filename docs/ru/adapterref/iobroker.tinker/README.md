---
translatedFrom: en
translatedWarning: Если вы хотите отредактировать этот документ, удалите поле «translationFrom», в противном случае этот документ будет снова автоматически переведен
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/ru/adapterref/iobroker.tinker/README.md
title: ioBroker.tinker
hash: AUPXL9BxNG45UFrTKkEbjPt6irP0fpPFrXNNC5pO9lk=
---
![Логотип](../../../en/adapterref/iobroker.tinker/admin/tinker.png)

![версия NPM](http://img.shields.io/npm/v/iobroker.tinker.svg)
![Загрузки](https://img.shields.io/npm/dm/iobroker.tinker.svg)
![Количество установок (последние)](http://iobroker.live/badges/tinker-installed.svg)
![Количество установок (стабильно)](http://iobroker.live/badges/tinker-stable.svg)
![Известные уязвимости](https://snyk.io/test/github/simatec/ioBroker.tinker/badge.svg)
![Лицензия](https://img.shields.io/github/license/simatec/ioBroker.tinker?style=flat)
![Пожертвовать](https://img.shields.io/badge/paypal-donate%20|%20spenden-blue.svg)
![](https://img.shields.io/static/v1?label=Sponsor&message=%E2%9D%A4&logo=GitHub&color=%23fe8e86)

# IoBroker.tinker
===================

![Тестируйте и выпускайте](https://github.com/simatec/ioBroker.tinker/workflows/Test%20and%20Release/badge.svg)

Адаптер Tinker Board Monitor — это модифицированная версия адаптера Raspberry PI Monitor и адаптера OrangePi Monitor для ioBroker.

**Если вам это нравится, рассмотрите пожертвование:**

[![PayPal] (https://www.paypalobjects.com/en_US/DK/i/btn/btn_donateCC_LG.gif)](https://paypal.me/mk1676)

### Важная информация
протестированное оборудование: Asus Tinker Board

### Следующие объекты доступны после выбора:
## *ПРОЦЕССОР*
- частота_процессора
- загрузка1
- груз5
- нагрузка15

## *Объем памяти*
- память_доступна
- memory_free
- memory_total

## *Сеть (eth0)*
- net_received
- net_send

## *SD Card*
- sdcard_root_total
- sdcard_root_used

## *Менять*
- swap_total
- swap_used

## *Температура*
- соц_темп

## *Время работы*
- время безотказной работы

## *Беспроводная сеть*
- wifi_received
- wifi_send

## Конфигурация
На странице конфигурации вы можете выбрать следующие модули:

- ПРОЦЕССОР
- Объем памяти
- Сеть
- SD Card
- Менять
- Температура
- Время безотказной работы
- беспроводная сеть

## Changelog
<!-- ### __WORK IN PROGRESS__ -->
### 1.2.0 (2023-03-18)
* (simatec) Dependencies updated
* (simatec) test and release updated
* (simatec) Repo updated

### 1.1.1 (2021-11-18)
* (simatec) Dependencies updated
* (simatec) test and release updated

### 1.1.0 (2020-04-08)
* (simatec) delete sync-exec
* (simatec) Rewritten code on child_process
* (simatec) code cleaned

### 1.0.0 (2020-04-07)
* (simatec) Release 1.0.0

### 0.1.3 (2019-03-14)
* (simatec) Ready for latest

### 0.1.1 (2019-01-08)
* Fix for new iobroker Installer

### 0.1.0 (2018-07-03)
* First Beta

### 0.0.1 (2018-07-03)
* initial Version

## License

The MIT License (MIT)

Copyright (c) 2018 - 2023 simatec

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.