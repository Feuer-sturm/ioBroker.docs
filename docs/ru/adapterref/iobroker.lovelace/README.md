---
translatedFrom: en
translatedWarning: Если вы хотите отредактировать этот документ, удалите поле «translationFrom», в противном случае этот документ будет снова автоматически переведен
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/ru/adapterref/iobroker.lovelace/README.md
title: ioBroker.lovelace
hash: ucysTQCpKxeYf2xX4WyMgGwuO0byAUkW6HEbkxtIZ5Q=
---
![Логотип](../../../en/adapterref/iobroker.lovelace/admin/lovelace.png)

![Количество установок](http://iobroker.live/badges/lovelace-stable.svg)
![НПМ-версия](http://img.shields.io/npm/v/iobroker.lovelace.svg)
![Загрузки](https://img.shields.io/npm/dm/iobroker.lovelace.svg)

# IoBroker.lovelace
![Тестирование и выпуск](https://github.com/ioBroker/iobroker.lovelace/workflows/Test%20and%20Release/badge.svg) [![Статус перевода](https://weblate.iobroker.net/widgets/adapters/-/lovelace/svg-badge.svg)](https://weblate.iobroker.net/engage/adapters/?utm_source=widget)

## Адаптер ловеласа для ioBroker
С помощью этого адаптера вы можете создать визуализацию для ioBroker с пользовательским интерфейсом Home Assistant Lovelace.

[Немецкая документация](docs/de/README.md)

## Объекты экземпляра
В экземплярах папки есть несколько объектов, которые можно использовать для управления пользовательским интерфейсом. Для каждого браузера будет создана новая подпапка со случайным идентификатором. Этот идентификатор хранится в веб-хранилище клиентского браузера. Если вы удалите веб-хранилище, будет создан новый экземпляр. Если вы используете полнофункциональный браузер киоска, убедитесь, что функция `Delete webstorage on reload` **отключена**.

Эта функциональность использует браузер_mod, который устанавливается и обновляется адаптером. Не добавляйте свою версию браузера_mod в качестве пользовательской карты.

## Конфигурация
Существует два способа настройки сущностей:

- авто
- руководство

### Авто
В автоматическом режиме будет применен аналогичный процесс, как для `google home` или `material adapter`.

***Будут обнаружены только объекты и каналы, для которых определены категории `function` и `room`***

Вы можете определить понятные имена, и они будут использоваться в сущностях.

### Руководство
Объекты можно определить вручную в дереве объектов, например sql или histroy. Необходимо указать тип объекта и, при необходимости, имя объекта.
С помощью этого метода можно создавать только простые объекты, такие как input_number, input_text или input_boolean. Он не может иметь более одного состояния или атрибута.

## Панели
### Панель сигнализации
ioBroker пока не поддерживает такое устройство, но его можно смоделировать. Если вы создадите такой скрипт:

```
createState(
    'alarmSimple',
    false,
    false,
    {
        "name": "alarmSimple",
        "role": "alarm",
        "type": "boolean",
        "read": true,
        "write": true,
        "desc": "Arm or disarm with code",
        "def": false,
        "custom": {
            "lovelace.0": {
                "enabled": true,
                "entity": "alarm_control_panel",
                "name": "simulateAlarm" // this is a name how the entity will be called. In this case "alarm_control_panel.simulateAlarm"
            }
        }
    },
    {
        "alarm_code": 1234 // this is a alarm code, that must be entered
    },
    function () {
        // react on changes
        on({id: 'javascript.' + instance + '.alarmSimple', change: 'any'}, function (obj) {
            console.log('Control here the real device: ' + obj.state.val);
        });
    }
);
```

или вы просто используете для этого `lovelace.X.control.alarm (entity_id = alarm_control_panel.defaultAlarm)`.

### Ввод номера
Это можно сделать вручную, если в пользовательском диалоговом окне выбран тип объекта input_number.
Для этого типа требуются значения `min` и `max` в `common`, а также можно добавить дополнительные `step`.
Если вы хотите видеть стрелки вверх и вниз, вам следует установить в пользовательском `mode` значение «номер»:

```
common: {
    custom: {
        "lovelace.0": {
            "enabled": true,
            "entity": "input_number",
            "name": "Shutter" // this is a name how the entity will be called. In this case "alarm_control_panel.simulateAlarm"
            "mode": "number", // default presentation is slider
        }
    }
}
```

### Выбор входа
Это можно сделать вручную, если в пользовательском диалоговом окне выбран тип объекта input_select.
Список опций для выбора должен быть предоставлен в стандартном объекте commom.states:

```
"common": {
    "type": "string",
    "states": {
      "1": "select 1",
      "2": "Select 2",
      "3": "select 3"
    },
    "custom": {
      "lovelace.0": {
        "enabled": true,
        "entity": "input_text",
        "name": "test_input_select"
      }
    }
```

другими словами, в IoB также должен быть выбран вход.

### Таймер
Таймер можно смоделировать с помощью следующего скрипта:

```
createState(
    'timerSimple',
    false,
    false,
    {
        "name": "timerSimple",
        "role": "level.timer",
        "type": "number",
        "read": true,
        "write": true,
        "unit": "sec",
        "desc": "Start/Stop Timer",
        "def": 0,
        "custom": {
            "lovelace.0": {
                "enabled": true,
                "entity": "timer",
                "name": "simulateTimer" // this is a name how the entity will be called. In this case "timer.simulateTimer"
            }
        }
    },
    {
        "alarm_code": 1234 // this is a alarm code, that must be entered
    },
    function () {
        let interval;
        let id = 'javascript.' + instance + '.timerSimple';
        // react on changes
        on({id, change: 'any'}, function (obj) {
            // If command
            if (!obj.state.ack) {
                // If start or pause timer
                if (obj.state.val) {
                    // If pause (the same value was written)
                    if (obj.state.val === obj.oldState.val) {
                        if (interval) {
                            setState(id, state.val, true);
                            clearInterval(interval);
                            interval = null;
                        } else {
                            interval = setInterval(() => {
                                getState(id, (err, state) => {
                                    state.val--;
                                    if (state.val <= 0) {
                                        clearInterval(interval);
                                        interval = null;
                                        state.val = 0;
                                    }
                                    setState(id, state.val, true);
                                });
                            }, 1000);
                        }
                    } else {
                        interval && clearInterval(interval);
                        // update value every second
                        interval = setInterval(() => {
                            getState(id, (err, state) => {
                                state.val--;
                                if (state.val <= 0) {
                                    clearInterval(interval);
                                    interval = null;
                                    state.val = 0;
                                }
                                setState(id, state.val, true);
                            });
                        }, 1000);
                    }
                } else {
                    // stop interval
                    interval && clearInterval(interval);
                    interval = null;
                }
            }
        });
        // test timer. Disable it later
        setTimeout(() => setState(id, 20));
    }
);
```

### Погода
Протестировано с помощью yr и daswetter. Для одного или нескольких из следующих объектов должны быть установлены `Function=Weather` и `Room=Any`, чтобы они были доступны в конфигурации:

- daswetter.0.NextDays.Location_1
- прогноз на 00.год.

Протестировано с драйвером AccuWeather версии 1.1.0 https://github.com/iobroker-community-adapters/ioBroker.accuweather.
Пользовательская карта Lovelace, созданная в поддержку прогноза погоды AccuWeather — https://github.com/algar42/IoB.lovelace.accuweather-card

### Список покупок
Список покупок записывает значения в виде:

```
[
   {name: 'Task 1', id: 1234222, complete: false},
   {name: 'Task 2', id: 1234223, complete: true}
]
```

в состояние `lovelace.X.control.shopping_list`.

### Карта
Объекты должны выглядеть так:

```
createState('location', '39.5681295;2.6432632', false, {
    "name": "location",
    "role": "value.gps",
    "type": "string",
    "read": true,
    "write": false,
    "desc": "Gps Coordinates"
});
```

или это два объекта:

```
createState('location.longitude', 2.6432632, false, {
    "name": "location longitude",
    "role": "value.gps.longitude",
    "type": "number",
    "read": true,
    "write": false,
    "desc": "Gps Coordinates"
});
createState('location.latitude', 39.5681295, false, {
    "name": "location latitude",
    "role": "value.gps.latitude",
    "type": "number",
    "read": true,
    "write": false,
    "desc": "Gps Coordinates"
});
```

### Объект изображения
Вы можете использовать для этого статическое изображение или использовать любое состояние, которое предоставляет URL-адрес в качестве состояния.
Например.:

```
{
  "_id": "daswetter.0.NextDays.Location_1.Day_1.iconURL",
  "type": "state",
  "common": {
    "name": "Weather icon URL",
    "type": "string",
    "role": "weather.icon.forecast.0",
    "read": true,
    "write": false
  },
  "native": {}
}
```

или просто вручную установите тип объекта на `camera` и напишите в него URL.

### Скрыть панель инструментов
Чтобы скрыть панель инструментов, вы можете установить флажок в диалоговом окне конфигурации ioBroker на вкладке Темы.
Чтобы отобразить его, вы можете снова отключить его в диалоговом окне или просто вызвать URL-адрес с параметром `?toolbar=true`.

### Уценка
Вы можете использовать привязки в уценке, как в [iobroker.vis](https://github.com/ioBroker/ioBroker.vis#bindings-of-objects).

Например. Текст `Admin adapter is {a:system.adapter.admin.0.alive;a === true || a === 'true' ? ' ' : 'not '} *alive*.` создаст текст `Admin adapter is alive` на панели уценки.

## Пользовательские карты
### Загрузка пользовательских карточек
Чтобы загрузить пользовательскую карту, напишите следующее:

```iobroker file write PATH_TO_FILE\bignumber-card.js /lovelace.0/cards/```

После перезапуска адаптера Lovelace он автоматически включит все файлы из каталога `cards`.

Следующие пользовательские карты могут быть успешно протестированы:

- карта bignumber: https://github.com/custom-cards/bignumber-card/blob/master/bignumber-card.js
— простой-термостат: https://github.com/nervetattoo/simple-thermostat/releases (берём последний релиз)
- термостат: https://github.com/ciotlosm/custom-lovelace/tree/master/thermostat-card (требуются оба файла .js и .lib.js)

Я нашел эту ссылку https://github.com/jimz011/homeassistant как интересный ресурс для пользовательских карточек.

Часто пользовательские карты хранятся на github в качестве исходников и перед использованием их необходимо скомпилировать.
Вам следует проверить меню `Releases` на github и попытаться найти там скомпилированные файлы.
Например, этот: [https://github.com/kalkih/mini-graph-card/releases](https://github.com/kalkih/mini-graph-card/releases) (найдите файл `mini-graph-card-bundle.js`)

## Собственные изображения
Пользовательские изображения (например, для фона) можно загрузить через тот же диалог конфигурации, что и пользовательские карты. И используйте его так:

`background: center / cover no-repeat url("/cards/background.jpg") fixed`

или

`background: center / cover no-repeat url("/local/custom_ui/background.jpg") fixed`

в файле конфигурации ловеласа. Подробнее о фоне в ловеласе читайте [здесь](https://www.home-assistant.io/lovelace/views/#background).

## Темы
Темы можно определить в диалоговом окне конфигурации ioBroker.
Вставьте что-то вроде:

```
midnight:
  # Main colors
  primary-color: '#5294E2'                                                        # Header
  accent-color: '#E45E65'                                                         # Accent color
  dark-primary-color: 'var(--accent-color)'                                       # Hyperlinks
  light-primary-color: 'var(--accent-color)'                                      # Horizontal line in about

  # Text colors
  primary-text-color: '#FFFFFF'                                                   # Primary text colour, here is referencing dark-primary-color
  text-primary-color: 'var(--primary-text-color)'                                 # Primary text colour
  secondary-text-color: '#5294E2'                                                 # For secondary titles in more info boxes etc.
  disabled-text-color: '#7F848E'                                                  # Disabled text colour
  label-badge-border-color: 'green'                                               # Label badge border, just a reference value

  # Background colors
  primary-background-color: '#383C45'                                             # Settings background
  secondary-background-color: '#383C45'                                           # Main card UI background
  divider-color: 'rgba(0, 0, 0, .12)'                                             # Divider

  # Table rows
  table-row-background-color: '#353840'                                           # Table row
  table-row-alternative-background-color: '#3E424B'                               # Table row alternative

  # Nav Menu
  paper-listbox-color: 'var(--primary-color)'                                     # Navigation menu selection hoover
  paper-listbox-background-color: '#2E333A'                                       # Navigation menu background
  paper-grey-50: 'var(--primary-text-color)'
  paper-grey-200: '#414A59'                                                       # Navigation menu selection

  # Paper card
  paper-card-header-color: 'var(--accent-color)'                                  # Card header text colour
  paper-card-background-color: '#434954'                                          # Card background colour
  paper-dialog-background-color: '#434954'                                        # Card dialog background colour
  paper-item-icon-color: 'var(--primary-text-color)'                              # Icon color
  paper-item-icon-active-color: '#F9C536'                                         # Icon color active
  paper-item-icon_-_color: 'green'
  paper-item-selected_-_background-color: '#434954'                               # Popup item select
  paper-tabs-selection-bar-color: 'green'

  # Labels
  label-badge-red: 'var(--accent-color)'                                          # References the brand colour label badge border
  label-badge-text-color: 'var(--primary-text-color)'                             # Now same as label badge border but that's a matter of taste
  label-badge-background-color: '#2E333A'                                         # Same, but can also be set to transparent here

  # Switches
  paper-toggle-button-checked-button-color: 'var(--accent-color)'
  paper-toggle-button-checked-bar-color: 'var(--accent-color)'
  paper-toggle-button-checked-ink-color: 'var(--accent-color)'
  paper-toggle-button-unchecked-button-color: 'var(--disabled-text-color)'
  paper-toggle-button-unchecked-bar-color: 'var(--disabled-text-color)'
  paper-toggle-button-unchecked-ink-color: 'var(--disabled-text-color)'

  # Sliders
  paper-slider-knob-color: 'var(--accent-color)'
  paper-slider-knob-start-color: 'var(--accent-color)'
  paper-slider-pin-color: 'var(--accent-color)'
  paper-slider-active-color: 'var(--accent-color)'
  paper-slider-container-color: 'linear-gradient(var(--primary-background-color), var(--secondary-background-color)) no-repeat'
  paper-slider-secondary-color: 'var(--secondary-background-color)'
  paper-slider-disabled-active-color: 'var(--disabled-text-color)'
  paper-slider-disabled-secondary-color: 'var(--disabled-text-color)'

  # Google colors
  google-red-500: '#E45E65'
  google-green-500: '#39E949'
```

взято из [здесь](https://community.home-assistant.io/t/midnight-theme/28598/2).

## Иконки
Используйте значки в формате `mdi:NAME`, например «mdi:play-network». Имена можно взять отсюда: https://materialdesignicons.com/

## Уведомления
Вы можете добавлять уведомления с помощью функции `sendTo` или записывая состояние в `lovelace.X.notifications.add`:

```
sendTo('lovelace.0', 'send', {message: 'Message text', title: 'Title'}); // full version
sendTo('lovelace.0', 'send', 'Message text'); // short version
```

или

```
setState('lovelace.0.notifications.add', '{"message": "Message text", "title": "Title"}'); // full version
setState('lovelace.0.notifications.add', 'Message text'); // short version
```

## Голосовое управление
Все команды из веб-интерфейса будут записаны в состояние lovelace.X.conversation с `ack=false`.
Можно написать скрипт, который будет реагировать на запрос и отвечать:

```
on({id: 'lovelace.0.conversation', ack: false, change: 'any'}, obj => {
   console.log('Question: ' + obj.state.val);
   if (obj.state.val.includes('time')) {
      setState('lovelace.0.conversation', new Date().toString(), true); // true is important. It will say, that this is answer.
   } else {
      setState('lovelace.0.conversation', 'Sorry I don\'t know, what do you want', true); // true is important. It will say, that this is answer.
   }
});
```

## Поиск неисправностей
Если вы испортили код YAML и видите пустую страницу, но у вас все еще есть верхнее меню, вы можете включить режим редактирования (если он еще не включен) из меню, а затем снова открыть меню, чтобы получить доступ к «Редактору RAW Yaml», в котором вы увидеть полный код YAML и очистить его.
Если это не помогло, вы можете открыть объект lovelace.*.configuration в raw-редакторе в ioBroker и посмотреть там.
Вы также можете восстановить этот объект из резервной копии. Он содержит полную конфигурацию вашей визуализации.

##Первоисточники ловеласа
Использованные источники находятся здесь https://github.com/ GermanBluefox/home-assistant-polymer.

## Делать
Безопасность должна быть получена от текущего пользователя, а не от default_user.

## Разработка
### Версия
Использованная версия home-assistant-frontend@20230906.1 Версия браузерного мода: 2.3.0

### Как собрать новую версию Лавлейса
Прежде всего, фактический https://github.com/home-assistant/frontend (ветвь разработки) должен быть **вручную** объединен с https://github.com/НемецкийBluefox/home-assistant-polymer.git (* **iob*** ветка!).

Все изменения для ioBroker отмечены комментарием `// IoB`.
На данный момент (20230608.0) были изменены следующие файлы:

- `build-scripts/gulp/app.js` - Добавить новую задачу gulp development-iob
- `build-scripts/bundle.cjs` - отключить сбой при ошибке
- `build-scripts/gulp/webpack.js` - Добавить новую задачу gulp webpack-dev-app
- `src/data/lovelace.ts` - добавить опцию скрытия панели инструментов.
- `src/data/weather.ts` - добавлена поддержка отображения значка погоды по URL-адресу.
- `src/dialogs/more-info/const.ts` — удалить состояние погоды и историю
- `src/dialogs/more-info/ha-more-info-dialog.ts` — удалить кнопку и вкладку настроек объекта.
- `src/dialogs/more-info/ha-more-info-history.ts` - удалить ссылку «показать больше» в истории
- `src/dialogs/more-info/controls/more-info-weather.ts` — добавлена поддержка отображения значка погоды по URL-адресу.
- `src/dialogs/voice-command-dialog/ha-voice-command-dialog.ts` — отключить настройку голосовых помощников
- `src/entrypoints/core.ts` — измененный процесс аутентификации
- `src/layouts/home-assistant-main.ts` — удалить боковую панель приложения.
- `src/panels/lovelace/cards/hui-weather-forecast-card.ts` — добавлена поддержка отображения значка погоды по URL-адресу.
- `src/panels/lovelace/entity-rows/hui-weather-entity-row.ts` - добавлена поддержка отображения значка погоды по URL-адресу с авторизацией.
- `src/panels/lovelace/hui-root.ts` - добавлены уведомления и голосовое управление
- `src/util/documentation-url.ts` — для ссылки на справку iobroker вместо homeassistant.
- `.gitignore` - добавить `.idea` игнорировать
- `.husky/pre-commit` — удалить крючки фиксации git.
- `package.json` - удалить хаски-хук фиксации

+ После этого проверить измененную версию в папке `./build`. Затем.

1. перейдите в каталог ./build.
2. `git clone https://github.com/DeutschBluefox/home-assistant-polymer.git` это форк https://github.com/home-assistant/frontend.git, но некоторые вещи изменены ( см. список файлов ранее).
3. `cd home-assistant-polymer`
4. `git checkout master`
5. «Установка пряжи»
6. `gulp build-app` для выпуска или `gulp development-iob` для отладочной версии. Чтобы собрать веб-сайт после изменений, вы можете вызвать «webpack-dev-app» для более быстрой сборки, но вам все равно придется вызывать «build-app» после того, как версия будет готова к использованию.
7. скопируйте все файлы из `./build/home-assistant-polymer/hass_frontend` в `./hass_frontend` в этом репозитории.
8. Запустите задачу `gulp rename` несколько раз (пока не произойдет никаких изменений).
9. Обновите версию в Readme, а также в константе VERSION сервера.js.

## Changelog

<!--
	PLACEHOLDER for next version:
	### **WORK IN PROGRESS**
-->
### **WORK IN PROGRESS**
* (Garfonso) Update frontent to 2023.06.08.0
* (Garfonso) Use better random numbers

### 3.0.1 (2022-11-03)
* (Garfonso) do not crash if no history instance selected.
* (Garfonso) notifications working again.
* (Garfonso) repaired color temperature handling.

### 3.0.0 (2022-10-28)
* (agross) added: per instance language support
* (Garfonso) entity_id for devices with only one non english name should be ok again.
* (Garfonso) changed: updated frontend to 20221027.0. Needs theme adjustment (add code-editor-background-color) and probably card updates
* (Garfonso) added: browser_mod (2.1.3) is now integrated. Please remove manual installed versions of custom browser_mod card.
* (Garfonso) added: 'instances.refresh' can be used to reload page in connected browsers.
* (Garfonso) removed: lovelace_reload and window_reload states
* (Garfonso) removed: name state, not supported by browser_mod anymore
* (Garfonso) added: Support for toasts with action button (either json or ;-string)
* (Garfonso) added: activity state will show if user is currently using a certain browser
* (Garfonso) added: Support for subfolders in /cards/ for images and stuff custom cards load (please keep cards in root folder).
* (Garfonso) crash if notification was malformed json.
* (Garfonso) some translation stuff
* (Garfonso) crash case when states were updated before websocket was ready
* (Apollon77) Prepare for future js-controller versions
* (bluefox) tried to make html requests relative

### 2.2.0 (2022-06-05)
* (Garfonso) fixed: incorrect warning about duplicate entities on update of manual entity.
* (Garfonso) fixed: input_datetime did not work if time was enabled and did vanish if date and time were enabled.
* (Garfonso) fixed: RGB hex string got broken on not rounded numbers (problem with mushroom ligth card).
* (Garfonso) fixed: state of cover entity if not 0 or 100% (fixes problem with sliter-button-card).
* (Garfonso) fixed: light did not read brightness ACTUAL in dimmer devices.
* (Garfonso) added: support auto entities card and subscription.
* (Garfonso) added: improve support for input_datetime & string states.
* (Garfonso) added: support for browser_mod (i.e. crontrol frontend from iobroker).

### 2.1.4 (2022-01-09)
* (Garfonso) Dependency update

### 2.1.3 (2022-01-07)
* (Garfonso) Fixed: remove backup of old frontend (sorry)

## License

Copyright 2019-2022, bluefox <dogafox@gmail.com>

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.