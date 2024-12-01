# Практическая работа №4
## Network Threat Hunting
## Чурсинов Герман Сергеевич ББМО-01-23
### 1. Установка
Первым делом скачаем архив
![](https://i.imgur.com/eWhHUGx.png)
Откроем архив в VirtualBox
![](https://i.imgur.com/HZrqXgy.png)
Войдём в заранее настроенную виртуальную машину
![](https://i.imgur.com/gI25zmr.png)
Откроем сайт на localhostе
![](https://i.imgur.com/sQvgLIE.png)
По умолчанию мы видим меню дэшбордов
![](https://i.imgur.com/wHC3XQh.png)

### 2.1 Задание 1
Перейдём в папку с заданиями
![](https://i.imgur.com/0CUSvgE.png)
Запустим задание командой из инструкции
![](https://i.imgur.com/vi96sIB.png)
Выберем задание на вебсайте
![](https://i.imgur.com/tGYXaRu.png)
Задание запущено
![](https://i.imgur.com/63sNCEZ.png)
Передём в меню анализа трафика
![](https://i.imgur.com/7uTmOef.png)
Адрес 1
![](https://i.imgur.com/NgvGnJB.png)
Адрес 2
![](https://i.imgur.com/AM2zpJ3.png)
Адрес 3
![](https://i.imgur.com/k84eNXx.png)
Адрес 4
![](https://i.imgur.com/fyZopLY.png)
Адрес 5 - подозрительный
![](https://i.imgur.com/EAmYkp6.png)
Адрес 6
![](https://i.imgur.com/U4bLUMO.png)
Поиск подозрительного сайта на VirusTotal
![](https://i.imgur.com/xIlMy66.png)
Поскольку первые 5 доменов принадлежат Microsoft и служат для обеспечения работы Windows, добавим их в исключение (safelist)
А этот IP-адрес вызывает подозрение, так как много подключений (ровный график), отсутствие домена и в headere в https запросе нет поля HOST. в User Agent вообще указана Mozila.
![](https://i.imgur.com/DUgv4rP.png)

### 2.2 Задание 2
Аналогичным образом загрузим второе задание
![](https://i.imgur.com/SjjzTDp.png)
И откроем его на сайте
![](https://i.imgur.com/ni8hw2R.png)
Изначально показалось, что задание загружено с ошибкой, так как ничего нет. Однако в разделе DNS указан домен "честноянезло" (honestimnotevil)
![](https://i.imgur.com/4npKT8t.png)
Странные доменные имена адресов второго уровня. Может быть DNS-тунеллированием (CobaltStrike к примеру)
![](https://i.imgur.com/8upJrpt.png)

### 2.3 Задание 3
Также, как предыдущие два задания, загружаем третье:
![](https://i.imgur.com/02qVzXm.png)
Теперь импортируем его
![](https://i.imgur.com/bnIqm7g.png)
Подозрение вызвал домен с подозрительной маскировкой под Skype
![](https://i.imgur.com/jGuGfy9.png)
Обращения идут каждый час в :17 минут, что говорит о DNS-beacon и намекает на CobaltStrike.
![](https://i.imgur.com/ouRvK16.png)
Из виртуалки можно сразу узнать об адресе с помощью следующей встроенной фичи
![](https://i.imgur.com/BY7vlIw.png)
Virustotal показывает, что данный сайт распознан, как вредоносный
![](https://i.imgur.com/esMnIXL.png)
UserAgent также не похож на обычный
![](https://i.imgur.com/I0aRiO1.png)
Вот пример обычного userAgent
![](https://i.imgur.com/IOtGz2i.png)