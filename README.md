# Дипломный проект
## :pushpin: Содержание:

- [Технологии и инструменты](#computer-технологии-и-инструменты)
- [Список тестов](#bookmark_tabs-список-тестов)
- [Запуск тестов](#running_woman-запуск-тестов)
- [Сборка в Jenkins](#-сборка-в-jenkins)
- [Пример Allure-отчета](#-пример-allure-отчета)
- [Интеграция с Allure TestOps](#-интеграция-с-allure-testops)
- [Интеграция с Jira](#-интеграция-с-jira)
- [Уведомления в Telegram с использованием бота](#-уведомления-в-telegram-с-использованием-бота)
- [Видео-пример запуска теста в Selenoid](#-видео-примера-запуска-теста-в-selenoid)

## :computer: Технологии и инструменты

<p align="center">
<img width="6%" title="IntelliJ IDEA" src="images/logo/Intelij_IDEA.svg">
<img width="6%" title="Java" src="images/logo/Java.svg">
<img width="6%" title="Selenide" src="images/logo/Selenide.svg">
<img width="6%" title="Selenoid" src="images/logo/Selenoid.svg">
<img width="6%" title="Allure Report" src="images/logo/Allure_Report.svg">
<img width="6%" title="Allure TestOps" src="images/logo/Allure_TO.svg">
<img width="6%" title="Jira" src="images/logo/Jira.svg">
<img width="6%" title="Gradle" src="images/logo/Gradle.svg">
<img width="6%" title="JUnit5" src="images/logo/JUnit5.svg">
<img width="6%" title="GitHub" src="images/logo/GitHub.svg">
<img width="6%" title="Jenkins" src="images/logo/Jenkins.svg">
<img width="6%" title="Telegram" src="images/logo/Telegram.svg">
</p>

Автотесты написаны на <code>Java</code> с использованием <code>JUnit 5</code> и <code>Gradle</code>.
Для UI-тестов использована библиотека [Selenide](https://selenide.org/).
Запуск тестов можно осуществлять локально или с помощью [Selenoid](https://aerokube.com/selenoid/).
Также настроена сборка в <code>Jenkins</code>:
* с формированием Allure-отчета,
* с интеграцией с Allure TestOps,
* c интеграцией с Jira,
* c отправкой уведомления с результатами в <code>Telegram</code> после завершения прогона.

## :bookmark_tabs: Список тестов:
### UI тесты для Яндекс Путешествия
* Проверки блока поиска на главной странице:
  * Проверка категорий в блоке поиска;
  * Проверка полей поиска в каждой категории;
* Проверка функции поиска в категории "Авиа":
  * Проверка наличия лучшего предложения (по цене) среди результатов поиска авиабилетов из Москвы в Белград
* Проверки в категории "Ж/д":
  * Проверки отображения фильтра "Скоростные поезда":
    * Проверка наличия фильтра "Скоростные поезда" при поиске ж/д билетов в Санкт-Петербург
    * Проверка отсутствия фильтра "Скоростные поезда" при поиске ж/д билетов не в Санкт-Петербург
  * Проверки функции поиска в категории "Ж/д"
    * Проверка наличия результатов поиска ж/д билетов
    * Проверка наличия города-назначения в заголовке результатов поиска ж/д билетов
## :running_woman: Запуск тестов

Для запуска тестов используем команду:
```bash
gradle clean test {и далее параметры сборки тестов}
```

### Параметры сборки для UI тестов

> <code>REMOTE_URL</code> – адрес удаленного сервера, на котором будут запускаться тесты;
>
> <code>BROWSER</code> – браузер;
>
> <code>BROWSER_VERSION</code> – версия браузера;
>
> <code>BROWSER_SIZE</code> – размер окна браузера.
> 
> <code>BASE_URL</code> – URL главной страницы тестируемого сайта.
>
> <code>ATTACH_VIDEO_URL</code> – базовая часть URL для получения видео из удаленного браузера 

Дефолтные значения параметров для локального запуска тестов задаются в файле <code>local.properties</code>.

Дефолтные значения параметров для запуска тестов на удаленном сервере задаются в файле <code>remote.properties</code>. 
(Для корректного запуска необхожимо передать адрес удаленного сервера)

#### Локальный запуск тестов
```bash
gradle clean test -Denv=local
```

Если необходимо переопределить дефолтные значения параметров (вместо {} подставить требуемое значение):

```bash
gradle clean test 
-Dbrowser={BROWSER}
-DbrowserVersion={BROWSER_VERSION}
-DbrowserSize={BROWSER_SIZE}
-DbaseUrl={BASE_URL}
```

#### Запуск тестов на удаленном сервере
```bash
gradle clean test -Denv=remote -DremoteUrl={REMOTE_URL} -DattachVideoUrl={ATTACH_VIDEO_URL}
```

Если необходимо переопределить дефолтные значения параметров (вместо {} подставить требуемое значение):

```bash
gradle clean test 
-Dbrowser={BROWSER}
-DbrowserVersion={BROWSER_VERSION}
-DbrowserSize={BROWSER_SIZE}
-DremoteUrl={REMOTE_URL}
-DbaseUrl={BASE_URL}
-DattachVideoUrl={ATTACH_VIDEO_URL}
```

## <img width="4%" style="vertical-align:middle" title="Jenkins" src="images/logo/Jenkins.svg"> Сборка в Jenkins

<p align="center">
<img title="Jenkins Build" src="images/screenshots/jenkinsBuild.png">
</p>

1.  Открыть <a target="_blank" href="https://jenkins.autotests.cloud/job/Students/job/18-kiselevanm-Jenkins-diploma/">**сборку в Jenkins**</a>
    * Для корректного открытия ссылки требуется авторизация на <a target="_blank" href="https://jenkins.autotests.cloud/">**jenkins.autotests.cloud**</a>
2. В боковом меню выбрать пункт **"Собрать с параметрами"**;
3. Выбрать параметры из выпадающего списка;
4. Нажать кнопку **"Собрать"**;
5. После прогона тестов в блоке **"История сборок"** появятся ссылки на **Allure-отчет** и на **Allure TestOps**.


## <img width="4%" style="vertical-align:middle" title="Allure Report" src="images/logo/Allure_Report.svg"> Пример Allure-отчета
### Главная страница

<p align="center">
<img title="Allure Overview" src="images/screenshots/allureReportMain.png">
</p>

### Результат выполнения тестов

<p align="center">
<img title="Test Results in Allure" src="images/screenshots/allureReportTests.png">
</p>

## <img width="4%" title="Allure TestOPS" src="images/logo/Allure_TO.svg"> Интеграция с Allure TestOps

## Главный дашборд

<p align="center">
<img title="Allure TestOps Dashboard" src="images/screenshots/allureTestOpsDashboard.png">
</p>

## Результаты выполнения тестов

<p align="center">
<img title="Allure TestOps Results" src="images/screenshots/allureTestOpsResults.png">
</p>

## <img width="4%" title="Jira" src="images/logo/Jira.svg"> Интеграция с Jira

<p align="center">
<img title="Jira" src="images/screenshots/jira.png">
</p>


### <img width="4%" style="vertical-align:middle" title="Telegram" src="images/logo/Telegram.svg"> Уведомления в Telegram с использованием бота

После завершения сборки Telegram-бот, автоматически обрабатывает и отправляет сообщение с отчетом о прогоне.

<p align="center">
<img width="70%" title="Telegram Notifications" src="images/screenshots/notificationExample.png">
</p>

### <img width="4%" style="vertical-align:middle" title="Selenoid" src="images/logo/Selenoid.svg"> Видео примера запуска теста в Selenoid

К каждому тесту в отчете прилагается видео. Одно из таких видео представлено ниже.
<p align="center">
  <img title="Selenoid Video" src="images/video/videoExample.gif">
</p>
