# ipregion

Bash-скрипт для определения геолокации вашего IP-адреса с помощью различных сервисов GeoIP и популярных веб-сайтов.  
Это **форк** [оригинального скрипта](https://github.com/vernette/ipregion/), с рядом улучшений и исправлений.

## Использование:
Запуск скрипта **напрямую** с GitHub:
```bash
bash <(wget -qO- https://raw.githubusercontent.com/MacTepYoba/ipregion/main/ipregion.sh)
```

Пример вывода:

![image](https://i.imgur.com/Bn1o644.png)

## Что нового:

По сравнению с оригинальным скриптом, эта версия включает в себя несколько улучшений и исправлений:

- **Улучшено обнаружение IPv6** — ранее в некоторых случаях оно работало ненадежно.   
- **Исправлены зависания** некоторых сервисов.   
- **Скорректировано выравнивание таблицы результатов** для лучшей читаемости.   
- **Разделены проверки YouTube и Google** — иногда они показывают разные регионы.   
- **Правильно определяется регион Google в Китае (CN)**.   
- **Добавлена поддержка дополнительных сервисов**: 2ip.io, Youtube Music, Bing и Amazon Prime.   
- **Обнаружение ASN с помощью базы данных MaxMind** — аналогично [IPQuality](https://github.com/xykt/IPQuality/).  
- **Сервисы CDN удалены из группы по умолчанию** — при необходимости их можно проверить отдельно.  
- **Улучшенный, удобный для восприятия вывод** с расшифровкой названий стран и процентными расчетами.  
- **Названия стран + процентная статистика**: наряду с кодами ISO отображаются названия стран и процентное распределение по сервисам. 
- **Исправлены мелкие ошибки**, например невозможность прервать выполнение скрипта.

## Основные функции

- Проверяет геолокацию вашего IP-адреса с помощью нескольких общедоступных API GeoIP
- Результаты как от «основных» сервисов GeoIP, так и от популярных веб-сервисов (YouTube, Netflix, Twitch и т.д.)
- Поддерживает как IPv4, так и IPv6 (можно тестировать по отдельности)
- Поддержка прокси-сервера SOCKS5 и пользовательского сетевого интерфейса
- Вывод в формате JSON для автоматизации и интеграции
- Легко читаемая таблица с цветовой кодировкой

## Зависимости

- bash
- curl
- jq
- util-linux or bsdmainutils (for `column`)

## Коды стран

Скрипт выводит коды стран в формате ISO 3166-1 альфа-2 (например, RU, US, DE).  
В **человекочитаемом выводе** скрипт также показывает **полное название страны** и **процентное распределение**.

Для ручного поиска кодов можно воспользоваться официальным сайтом ISO: [https://www.iso.org/obp/ui/#search/code/](https://www.iso.org/obp/ui/#search/code/)

### Варианты использования:

```bash
# Показать справочное сообщение
./ipregion.sh --help

# Проверка всех сервисов с настройками по умолчанию
./ipregion.sh

# Проверка только сервисов GeoIP
./ipregion.sh --group primary

# Проверка только сервисов CDN
./ipregion.sh --group cdn

# Проверка только популярных web сервисов
./ipregion.sh --group custom

# Проверка только IPv4
./ipregion.sh --ipv4

# Проверка только IPv6
./ipregion.sh --ipv6

# Использовать SOCKS5 прокси
./ipregion.sh --proxy 127.0.0.1:1080

# Использовать определенный сетевой интерфейс
./ipregion.sh --interface eth1

# Установить пользовательское время ожидания запроса curl в секундах
./ipregion.sh --timeout 20

# Вывести результат в формате JSON
./ipregion.sh --json

# Включить подробное ведение журнала
./ipregion.sh --verbose
```

Можно использовать и комбинировать все параметры, доступные в справочном сообщении (`-h, --help`).

## Options

```
Usage: ipregion.sh [OPTIONS]

IPRegion — determines your IP geolocation using various GeoIP services and popular websites

Options:
  -h, --help           Show this help message and exit
  -v, --verbose        Enable verbose logging
  -j, --json           Output results in JSON format
  -g, --group GROUP    Run only one group: 'primary', 'custom', 'cdn' or 'all' (default: all)
  -t, --timeout SEC    Set curl request timeout in seconds (default: 6)
  -4, --ipv4           Test only IPv4
  -6, --ipv6           Test only IPv6
  -p, --proxy ADDR     Use SOCKS5 proxy (format: host:port)
  -i, --interface IF   Use specified network interface (e.g. eth1)

Examples:
  ipregion.sh                       # Check all services with default settings
  ipregion.sh -g primary            # Check only GeoIP services
  ipregion.sh -g custom             # Check only popular websites
  ipregion.sh -g cdn                # Check only CDN
  ipregion.sh -4                    # Test only IPv4
  ipregion.sh -6                    # Test only IPv6
  ipregion.sh -p 127.0.0.1:1080     # Use SOCKS5 proxy
  ipregion.sh -i eth1               # Use network interface eth1
  ipregion.sh -j                    # Output result as JSON
  ipregion.sh -v                    # Enable verbose logging
```
