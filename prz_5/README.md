# Практическая работа №5
## Threat Hunting
## Чурсинов Герман Сергеевич ББМО-01-23

### 1. Создайте внутреннюю сеть виртуальных машин (VM) или контейнеров
В рамках данной практической работы были взяты виртуальные машины из [практики №3](https://github.com/l-Il/LOGI/tree/main/prz_3). 
![](https://i.imgur.com/GQ2Yg7e.png)
![](https://i.imgur.com/mnsAcCD.png)

### 2. Установите и настройте систему мониторинга
Из [практики №3](https://github.com/l-Il/LOGI/tree/main/prz_3) у меня осталась SIEM-система Wazuh. Её я и буду использовать.
![](https://i.imgur.com/ZcI8lqk.png)

### 3. Разверните уязвимые сервисы/сборки ОС в составе стенда
В качестве стенда я использовал:

Ubuntu с сервером Wazuh (IP-адрес `192.168.1.86`)
![](https://i.imgur.com/6v2qsd7.png)
Debian с агентом Wazuh (IP-адрес `192.168.1.83`)
![](https://i.imgur.com/aGQLjWj.png)
Kali Linux для проведения мероприятий по оценке защищенности (IP-адрес `192.168.1.75`)
![](https://i.imgur.com/inCNTDG.png)

Уязвимым сервисом можно считать базовый развернутый apache web-сервер, без каких-либо доп.настроек
![](https://i.imgur.com/BHb3jJ7.png)
![](https://i.imgur.com/KWbt2QB.png)

### 4. Создайте профили источников данных
![](https://i.imgur.com/c0FR3uz.png)
![](https://i.imgur.com/hMasOrU.png)
![](https://i.imgur.com/AKGoRdN.png)


### 5. Создайте правила обнаружения
Правило для suricata выглядит следующим образом: `alert http any any -> any 80 (msg:"Test rule to check made by Chursinov German BBMO-01-23"; content:"test"; nocase; http_uri; sid:1000001; rev:1;)`
Оно улавливает любой пакет с содержимым "text" для простой проверки работоспособности
Любой пакет по протоколу http, с любого адреса и любого порта на любой адрес по 80 порту, ищет в содержимом "text", на регистр не обращает внимание, sid и rev это дефолтная информация, например rev - ревизия правила, при обновлении меняется на 2, 3, и т.д.

![](https://i.imgur.com/dOJCDLQ.png)
![](https://i.imgur.com/d7uUa5j.png)
Так выглядит само правило
![](https://i.imgur.com/dThSVlx.png)
Смотрим логи Suricata
![](https://i.imgur.com/tX8bvjQ.png)
Дошло и до Wazuh
![](https://i.imgur.com/DCRvSPt.png)


### 6. Настройте инструменты для мониторинга и обнаружения угроз безопасности, а также анализа данных
Пункты 3-6 были объединены, а картинки-скриншоты моего хода действий перемешаны
![](https://i.imgur.com/f2iF17J.png)
Создал YARA правило
![](https://i.imgur.com/X5OuJ1V.png)


### 7. Запустите сканеры уязвимостей
![](https://i.imgur.com/RloLPkD.png)
Воспользовался сканером nikto
![](https://i.imgur.com/D9meNuV.png)
Нашёл уязвимость 2003 года
![](https://i.imgur.com/zZxENC9.png)
Wazuh на графике уязвимостей показывает эту уязвимость
![](https://i.imgur.com/X5OuJ1V.png)
![](https://i.imgur.com/1JkOZlU.png)
![](https://i.imgur.com/jUzpfwI.png)
![](https://i.imgur.com/DV1Sqm3.png)
![](https://i.imgur.com/fuD2Cnm.png)
![](https://i.imgur.com/xtubDXf.png)
![](https://i.imgur.com/ADbmmlg.png)
В процессе перезагрузки Wazuh-агента я столкнулся с проблемой, которая решилась (два раза был написан блок `<ossec_config>`)
![](https://i.imgur.com/hs3xIwv.png)

### 8. Создайте «локальные» угрозы безопасности
Для эмуляции атаки буду использовать скрипт с [руководства по настройке YARA-правил](https://documentation.wazuh.com/current/proof-of-concept-guide/detect-malware-yara-integration.html)
![](https://i.imgur.com/uFEPnPM.png)
![](https://i.imgur.com/glycAZF.png)
В процессе выполнения из-за ошибок стало понятно, что данный скрипт надо запускать не на ВМ с Kali, а на машине с агентом.
![](https://i.imgur.com/MIOKs9c.png)
![](https://i.imgur.com/ib1B3DD.png)
![](https://i.imgur.com/UGYwYkY.png)


### 9. Анализируйте потенциальные угрозы
В рамках данной практической работы мы развернули полноценную SIEM, добавили в неё модули, такие как IDS/IPS для анализа трафика. Это позволяет нам использовать абсолютно любые правила для выявления вредоносного трафика в нашей сети, можно использовать недавно опубликованные (заопенсоршенные) правила от PT https://rules.ptsecurity.com/

Также мы настроили анализатор YARA-правил, с помощью которого можно детектировать вредоносные файлы и т.д. Это позволяет нам также использовать любые правила, будь то найденные в Интернете, или написанные самими. 