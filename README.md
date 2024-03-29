# Linux Monitoring

Linux bash-скрипты для исследование системы(мониторинг, логи, генерация файлов/папок/логов) на Ubuntu Server 20.04 LTS.

Реализация по кажой части лежит в своей папке.
Каждая скрипт-реализация лежит в соей папке и имеет точку старта в файле main.sh.

##  Навигация

1. [Chapter 1. Мониторинг системы](#Chapter-1-Мониторинг-системы) \
   1.1. [Исследование системы](#11-Исследование-системы) \
   1.2. [Исследование файловой системы](#12-Исследование-файловой-системы)
2. [Chapter 2. Работа с файлами и логами](#Chapter-2-Работа-с-файлами-и-логами) \
   3.1. [Part 1. Генератор файлов](#part-1-Генератор-файлов)  \
   3.2. [Part 2. Засорение файловой системы](#parцt-2-Засорение-файловой-системы)  \
   3.3. [Part 3. Очистка файловой системы](#part-3-Очистка-файловой-системы)  \
   3.4. [Part 4. Генератор логов](#part-4-Генератор-логов)     \
   3.5. [Part 5. Мониторинг](#part-5-Мониторинг) \
   3.6. [Part 6. Свой node_exporter](#part-6-свой-nodeexporter)

## Chapter 1. Мониторинг системы

Данная глава содержит 2 скрипта направленных на получение информации о системе и файлах.

## 1.1 Исследование системы

Скрипт запускается без параметров. Параметры задаются в конфигурационном файле до запуска скрипта.

Параметры обозначают цвета: (1 - white, 2 - red, 3 - green, 4 - blue, 5 – purple, 6 - black)

Данное исследование системы содержит следующие поля:

- **HOSTNAME** = _сетевое имя_  
- **TIMEZONE** = _временная зона в виде: **America/New_York UTC -5**_
- **USER** = _пользователь который запустил скрипт_  
- **OS** = _тип и версия ОС_  
- **DATE** = _текущее время в виде: **12 May 2020 12:24:36**_  
- **UPTIME** = _время работы системы_  
- **UPTIME_SEC** = _время работы системы в секундах_  
- **IP** = _ip-адрес машины_  
- **MASK** = _сетевая маска сетевого интерфейса в виде: **xxx.xxx.xxx.xxx**_  
- **GATEWAY** = _ip шлюза по умолчанию_  
- **RAM_TOTAL** = _размер оперативной памяти в Гб в виде: **3.125 GB**_  
- **RAM_USED** = _размер используемой памяти в Гб_  
- **RAM_FREE** = _размер свободной памяти в Гб_  
- **SPACE_ROOT** = _размер рутового раздела в Mб в виде: **254.25 MB**_  
- **SPACE_ROOT_USED** = _размер занятого пространства рутового раздела в Mб_  
- **SPACE_ROOT_FREE** = _размер свободного пространства рутового раздела в Mб_

После вывода информации о системе выводится использованная цветовая схема.

## 1.2 Исследование файловой системы

Скрипт запускается с одним параметром.  
Параметр - это абсолютный или относительный путь до какой-либо директории и должен заканчиваться знаком '/', например:  
`script.sh /var/log/`

Скрипт выводит следующую информацию о каталоге, указанном в параметре:
- Общее число папок, включая вложенные
- Топ 5 папок с самым большим весом в порядке убывания (путь и размер)
- Общее число файлов
- Число конфигурационных файлов (с расширением .conf), текстовых файлов, исполняемых файлов, логов (файлов с расширением .log), архивов, символических ссылок
- Топ 10 файлов с самым большим весом в порядке убывания (путь, размер и тип)
- Топ 10 исполняемых файлов с самым большим весом в порядке убывания (путь, размер и хеш)
- Время выполнения скрипта

## Chapter 2. Работа с файлами и логами

Глава содержит 6 скриптов с различным функциональном для создания тестовой ситуации в работе с нагрузкой на систему.

## Part 1. Генератор файлов

Скрипт запускается с 6 параметрами. Пример запуска скрипта:
`main.sh /opt/test 4 az 5 az.az 3kb`

- Параметр 1 - абсолютный путь.
- Параметр 2 - количество вложенных папок.
- Параметр 3 - список букв английского алфавита, используемый в названии папок (не более 7 знаков).
- Параметр 4 - количество файлов в каждой созданной папке.
- Параметр 5 - список букв английского алфавита, используемый в имени файла и расширении (не более 7 знаков для имени, не более 3 знаков для расширения).
- Параметр 6 - размер файлов (в килобайтах, но не более 100).

Имена папок и файлов состоят только из букв, указанных в параметрах, и используют каждую из них хотя бы 1 раз в прямом порядке их ввода(от 4 знаков + дата).

При запуске скрипта по указанном в параметре 1 пути, будут созданы папки и файлы в них с соответствующими именами и размером.
Скрипт останавливает работу, если в разделе / останется 1 Гб свободного места.
Параллельно идет запись лог-файла с данными по всем созданным папкам и файлам.

## Part 2. Засорение файловой системы

Скрипт запускается с 3 параметрами. Пример запуска скрипта:
`main.sh az az.az 3Mb`

- Параметр 1 - список букв английского алфавита, используемый в названии папок (не более 7 знаков).
- Параметр 2 - список букв английского алфавита, используемый в имени файла и расширении (не более 7 знаков для имени, не более 3 знаков для расширения).
- Параметр 3 - размер файла (в Мегабайтах, но не более 100).

Имена папок и файлов состоят только из букв, указанных в параметрах, и используют каждую из них хотя бы 1 раз в прямом порядке их ввода(от 5 знаков + дата).

При запуске скрипта, в различных (любых, кроме путей содержащих bin или sbin) местах файловой системы, будут созданы папки с файлами.
Количество вложенных папок - до 100. Количество файлов в каждой папке - случайное число (для каждой папки своё).

Скрипт останавливает работу, если в разделе / останется 1 Гб свободного места.
Параллельно идет запись лог-файла с данными по всем созданным папкам и файлам.

В конце работы скрипта, на экран будет выведено время начало работы скрипта, время окончания и общее время его работы.

## Part 3. Очистка файловой системы

Скрипт запускается с 1 параметром.
Скрипт очищает систему от созданных в Part 2 папок и файлов 3 способами:

- По лог файлу
- По дате и времени создания
- По маске имени (т.е. символы, нижнее подчёркивание и дата).

Способ очистки задается при запуске скрипта, как параметр со значением 1, 2 или 3.
При удалении по дате и времени создания, пользователем вводятся времена начала и конца с точностью до минуты.

## Part 4. Генератор логов

Скрипт генерирует 5 файлов логов nginx в combined формате.
Каждый лог содержит информацию за 1 день, число записей от 100 до 1000.

Для каждой записи случайным образом генерируются:

- IP (любые корректные, т.е. не должно быть ip вида 999.111.777.777)
- Коды ответа (200, 201, 400, 401, 403, 404, 500, 501, 502, 503)
- Методы (GET, POST, PUT, PATCH, DELETE)
- Даты (в рамках заданного дня лога, должны идти по увеличению)
- URL запроса агента
- Агенты (Mozilla, Google Chrome, Opera, Safari, Internet Explorer, Microsoft Edge, Crawler and bot, Library and net tool)

В комментариях в скрипте указано, что означает каждый из использованных кодов ответа.

## Part 5. Мониторинг

Скрипт для разбора логов nginx из Part 4.
Скрипт запускается с 1 параметром, который принимает значение 1, 2, 3 или 4.

В зависимости от значения параметра будет выведены:

- Все записи, отсортированные по коду ответа
- Все уникальные IP, встречающиеся в записях
- Все запросы с ошибками (код ответа - 4хх или 5хх)
- Все уникальные IP, которые встречаются среди ошибочных запросов

Сравнить результат можно с выводом утилиты GoAccess:\
`goaccess  "full path to log files /home/**/src/Part04/*.log" --log-format=COMBINED -o  "path to result html"`

## Part 6. Свой node_exporter

Скрипт собирает информацию по базовым метрикам системы (ЦПУ, оперативная память, жесткий диск (объем)) и формирует html страничку по формату Prometheus,
которую будет отдавать nginx. Страничка постоянно обновляется.

`Для использования скрипта нужно поменять конфигурационный файл Prometheus, чтобы он собирал информацию с созданной странички.`

# Это всё. Спасибо за внимание к этой работе!