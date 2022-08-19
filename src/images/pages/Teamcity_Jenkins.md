# Teamcity - Jenkins
### Настройка триггера Авто тестов в Jenkins из Teamcity

## Создание Сборки

Открываем рабочий проект в Teamcity.

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity1.png)


Переходим в подменю проекта где находится Deploy.

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity2.png)

Нажимаем **Edit project…**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity3.png)

Нажимаем **Create build configuration**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity4.png)

Нажимаем **Manually.**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity5.png)

Настраиваем параметры :

**Name:** CI/CD Jenkins AutoTests (Dependencies "Deploy")

**Build configuration ID:** Заполняется автоматически

Нажимаем **Create.**

## Настраиваем Dependencies

Переходим в созданную сборку.

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity6.png)

Переходим в **Dependencies.**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity7.png)

Нажимаем **Add new snapshot dependency.**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity8.png)

Настраиваем зависимость  нашей сборки от Deploy нажимаем **Save.**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity9.png)

## Настраиваем Triggers

Переходим в **Triggers** нажимаем **Add new trigger.**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity10.png)

Выбираем **Finish Build Trigger.**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity11.png)

Настраиваем параметры:

**Build configuration:** Выставляем зависимость от Deploy

**Trigger after successful build only:** Ставим галочку 

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity12.png)

Нажимаем **Save.**

## Настраиваем Build Steps

Переходим в **Build Steps** нажимаем **Add build step.**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity13.png)

Выбираем **Command Line.**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity14.png)

Настраиваем параметры:

**Step name:**  Run build Jenkins

**Run:** Custom script

**Custom script:**


```bash
crumb=$(wget -q --auth-no-challenge --user xxxxx --password xxxxx --output-document - 
'http://192.xxx.x.xxx:xxx/crumbIssuer/api/xml?xpath=concat(//crumbRequestField,":",//crumb)') 
curl -X POST -H "$crumb" --user xxxxx:xxxxxxxxxxxxxxxxxxxxxx 
http://xxx.xxx.x.xxx:xxxxx/job/xxxxxxxxx/build
```

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity15.png)

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity16.png)

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity17.png)

Нажимаем **Save.**

## Проверка работы триггера

Переходим в рабочий проект.

Напротив Deploy нажимаем **Run.**

![Teamcity](/src/images/screenshots/teamcity_jenkins/Teamcity18.png)

После завершения сборки в Teamcity переходим в Jenkins.

Открываем Джобу проекта.

Проверяем запуск сборки.

![Jenkins](/src/images/screenshots/teamcity_jenkins/Jenkins1a.png)