### Jenkins - Gitlab
### Настройка интеграции Jenkins с Gitlab 
### Настройка триггера Джобы в Jenkins по Push из Gitlab

## Установка плагинов в Jenkins

Переходим в Jenkins скачиваем плагины: GitLab Plugin, GitLab Authentication plugin, Gitlab API Plugin.

![Jenkins](/src/images/screenshots/jenkins1.png)
![Jenkins](/src/images/screenshots/jenkins2.png)

## Создаём Access Tokens в Gitlab
Открываем GitLab переходим в Preferences => Access Tokens

Указываем Token name

Выбираем дату окончания годности токена - Expiration date

В Select scopes ставим галочку напротив api

Нажимаем Create personal access token

![Gitlab](/src/images/screenshots/gitlab1.png)

На появившейся странице **сохраняем сгенерированный токен**  Your new personal access token

![Gitlab](/src/images/screenshots/gitlab2.png)

## Создаём Credentials в Jenkins

Открываем Jenkins переходим в настройки открываем Manage Credentials

![Jenkins](/src/images/screenshots/jenkins3.png)

Далее переходим  **Stores scoped to Jenkins => Domains => (global) => Add Credentials**

![Jenkins](/src/images/screenshots/jenkins4.png)
![Jenkins](/src/images/screenshots/jenkins5.png)

в **Kind** выбираем -  **GitLab API token**

в **Scope - Global (Jenkins, nodes, items, all child items, etc)**

в **API token** - Вставляем наш токен получен в гитлабе

в **Description** - пишем описание

Нажимаем Ок

![Jenkins](/src/images/screenshots/jenkins6.png)

## Проверка создания Credentials
**Manage Credentials => Stores scoped to Jenkins => Domains => (global) => Add Credentials**

В **Global credentials (unrestricted)** должен быть наш Credentials

![Jenkins](/src/images/screenshots/jenkins7.png)

## Конфигурация системы в Jenkins

Переходим в настройки заходим в Конфигурация системы

![Jenkins](/src/images/screenshots/jenkins7.1.png)

Находим секцию **Gitlab**

Заполняем **Connection name , Gitlab host URL, Credentials**

![Jenkins](/src/images/screenshots/jenkins8.png)

Нажимаем применить и сохранить

## Создаём Джобу в Jenkins

Нажимаем Создать Item Создать задачу со свободной конфигурацией

![Jenkins](/src/images/screenshots/jenkins9.png)

Вводим имя itema

Нажимаем Создать задачу со свободной конфигурацией

Нажимаем Ok

![Jenkins](/src/images/screenshots/jenkins10.png)

## Конфигурируем сборку, получаем Webhook и Secret token

**Управление исходным кодом - Git**

**Repository URL** - указываем репозиторий с авто тестами

**Credentials** - указываем Gitlab токен

**Branch Specifier** - */название ветки

**Триггеры сборки**

Cтавим чекбокс напротив - Build when a change is pushed to GitLab. GitLab webhook URL: http://************ (сохраняем адрес вэбхука)

Cтавим чекбокс напротив - Push Events

Нажимаем Расширенные 

![Jenkins](/src/images/screenshots/jenkins11.png)

Secret token

Нажимаем Generate

**Сохраняем полученный токен.**

![Jenkins](/src/images/screenshots/jenkins11.1.png)

**Сборка**

Нажимаем Добавить шаг сборки 

![Jenkins](/src/images/screenshots/jenkins12.png)

**Gradle Version** - выбираем версию Gradle

**Tasks** - clean test

![Jenkins](/src/images/screenshots/jenkins13.png)

**После Сборочные операции**

Нажимаем Добавить шаг после сборки 

![Jenkins](/src/images/screenshots/jenkins14.png)

Выбираем Allure Report

![Jenkins](/src/images/screenshots/jenkins14.1.png)

**Results: Path - build/allure-results**

Нажимаем Применить и Сохранить 

![Jenkins](/src/images/screenshots/jenkins14.2.png)

## Предоставляем доступ Gitlab к локальному репозиторию

Заходим в **Admin Area** (Доступ есть только у админа)

**Settings => Network => Outbound reguests**

Ставим чекбоксы напротив **Allow requests to the local network from web hooks and services**

Ставим чекбоксы напротив **Allow requests to the local network from  system hooks**

В **Local IP addresses and domains names that hooks and services may access** прописываем адрес расположения Jenkins xxx.xxx.x.xxx

![Gitlab](/src/images/screenshots/gitlab3.png)

Если не предоставить доступ, то будет ошибка

**Url is blocked: requests to the local network are not allowed**

![Gitlab](/src/images/screenshots/gitlab4.png)

## Подключаем Webhook в Gitlab

Переходим в гитлаб

**Settings => Webhooks**

**URL** - вставляем адрес вэбхука из Jenkins

**Secret token** - вставляем Jenkins Secret token

**Trigger** - ставим галочку напротив Push events

Нажимаем **add webhook**

Проверяем что в **Project Hooks появился наш Webhook**

![Gitlab](/src/images/screenshots/gitlab5.png)

## Проверка автоматический запуск сборки после Push

Заходим в IDE открываем репозиторий с автотестами, коммитим изменение, делаем Push

Переходим в Jenkins нажимаем Dashboard переходим в Джобу

![Jenkins](/src/images/screenshots/jenkins15.png)

При правильном выполнении всех действий Сборка должна запуститься автоматически с указаниями старта из Gitlab и именем пользователя того кто осуществил Push

![Jenkins](/src/images/screenshots/jenkins16.png)
