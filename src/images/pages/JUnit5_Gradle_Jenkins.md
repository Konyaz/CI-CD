# JUnit5 - Gradle - Jenkins
### Запуск определенных тестов из единого репозитория с помощью тегов Juni5 и их запуск в Jenkins

## Настройка Gradle

Открываем **build.gradle**

В блоке test пишем

```bash
test {

           String itags = System.getProperty("includeTags") ?

                   System.getProperty("includeTags") : 'regression'

           String etags = System.getProperty("excludeTags") ?

                   System.getProperty("excludeTags") : 'acceptance'

           useJUnitPlatform {

               includeTags itags

               excludeTags etags

           }

       }

   }

}
```

![Gradle](/src/images/screenshots/junit5_gradle_jenkins/build_gradle1.png)

Обновляем Gradle project

![Gradle](/src/images/screenshots/junit5_gradle_jenkins/build_gradle2.png)

## Добавление тегов в Авто тест

Перед тест классом ставим тег 

```bash
@Tag("********")
```

![JUnit5](/src/images/screenshots/junit5_gradle_jenkins/junit5.1.png)

## Проверка работы Тега
Открываем консоль и пишем команду

```bash
gradle clean test -DincludeTags='********'
```

Тест должен запуститься.

## Запуск в Jenkins

Настраиваем сборку - указываем репозиторий и т.д.

Настраиваем параметры Invoke Gradle script:

В блоке **Сборка** выбираем **Invoke Gradle script**

Invoke Gradle **Gradle Version** - gradle 6.8.3

**Tasks** - 

```bash 
clean test -DincludeTags='********' 
```

![Jenkins](/src/images/screenshots/junit5_gradle_jenkins/Jenkins1.png)

## Проверка работы Джобы

Запускаем Jenkins Джобу проверяем что были запущены только те тесты в которых был указан соответствующий Тег


