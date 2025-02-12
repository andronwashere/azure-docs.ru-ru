---
title: Оповещения руководство по безопасности для центра безопасности Azure для Интернета вещей Preview | Документация Майкрософт
description: Узнайте о оповещения системы безопасности и рекомендуемые действия по исправлению, использование центра безопасности Azure для службы и возможности Интернета вещей.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: a5c25cba-59a4-488b-abbe-c37ff9b151f9
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: 6168278a51cf5a73fec0387ab61f46e201d158f9
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616946"
---
# <a name="azure-security-center-for-iot-security-alerts"></a>Центр безопасности Azure и оповещения системы безопасности Интернета вещей

> [!IMPORTANT]
> Центр безопасности Azure для Интернета вещей сейчас предоставляется в общедоступной предварительной версии.
> Предварительная версия предоставляется без соглашения об уровне обслуживания. Мы не рекомендуем использовать ее в рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены. Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Центр безопасности Azure (ASC) для Интернета вещей постоянно анализирует решения Интернета вещей с помощью расширенной аналитики и аналитики угроз, чтобы оповещать вас о вредоносных действиях.
Кроме того можно создать настраиваемые оповещения на основе сведений устройства ожидаемого поведения.
Предупреждение выступает в качестве индикатора comprise и следует требуют немедленного изучения и исправить.

В этой статье вы найдете список встроенные оповещения, которые можно запустить на центр Интернета вещей и/или устройств Интернета вещей.
Рядом с встроенные оповещения ASC для Интернета вещей позволяет настроить пользовательские оповещения, в зависимости от ожидаемого центра и/или поведения устройства.
Дополнительные сведения см. в разделе [создать настраиваемые оповещения](quickstart-create-custom-alerts.md).

## <a name="built-in-alerts-for-iot-devices"></a>Встроенные оповещения для устройств Интернета вещей

| severity | Имя                                                   | источника данных | Описание                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|----------|--------------------------------------------------------|-------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Высокий     | Успешно локальное имя входа                                 | Агент       | Обнаружено входа успешно локального устройства                                                                                                                                                                                                                                                                                                                                                                                         |
| Высокий     | Успешного подбора                                  | Агент       | Обнаружено несколько попыток неудачной попытки входа, за которым следует успешного входа. Устройства произошла успешного подбора.                                                                                                                                                                                                                                                                                                              |
| Высокий     | Двоичный командной строки                                    | Агент       | Двоичный файл Linux вызывается или выполнить из командной строки обнаружены. Этот процесс может быть законным действием или может означать, что устройство скомпрометировано.                                                                                                                                                                                                                                                                  |
| Высокий     | Обратный оболочек                                         | Агент       | Анализ данных узла обнаружил использование потенциальных обратной оболочкой. Для получения вновь скомпрометированным компьютером для обратного вызова на компьютер, принадлежащие злоумышленник обычно используются обратной оболочкой атак.                                                                                                                                                                                                                                                                                          |
| Высокий     | Отключение брандмауэра на узле                                       | Агент       | Возможно управление обнаружил брандмауэра-host. Злоумышленники часто отключит брандмауэр так, чтобы вести эксфильтрацию данных на узле.                                                                                                                                                                                                                                                                                                                                    |
| Высокий     | Веб-оболочки                                              | Агент       | Обнаружено оболочку web из возможных способов использования. Злоумышленники обычно отправьте оболочки web скомпрометированным компьютером для целей постоянный доступ в вашей среде или для дальнейшего использования.                                                                                                                                                                                                                                                                              |
| Высокий     | Перенаправление портов                                        | Агент       | Обнаруженные инициирования внешний IP-адрес перенаправления портов.                                                                                                                                                                                                                                                                                                                                                                       |
| Высокий     | Попытка отключить ведение журнала Auditd обнаружена    | Агент       | Система Linux Auditd предоставляет способ отслеживания сведений о системе, связанных с безопасностью. Auditd записывает как можно больше сведений о о событиях, происходящих в системе.  Auditd представляют собой критически важные для критически важных среды, чтобы иметь возможность определить violator политики безопасности и сведения о действиях, которые они выполнили. Отключение ведения журнала auditd можно остановить выполнение скрипта обнаружения нарушений политик безопасности, используемые в системе. |
| Средний   | Недействительная подпись токена SAS                            | Центр Интернета вещей     | Маркер SAS, используемый на устройстве имеет недопустимую подпись. Маркер SAS не соответствует либо первичный или вторичный ключ.                                                                                                                                                                                                                                                                                                                               |
| Средний   | Несоответствие отпечаток сертификата устройства x.509           | Центр Интернета вещей     | Отпечаток сертификата устройства X.509 не соответствует конфигурации                                                                                                                                                                                                                                                                                                                                                                             |
| Средний   | Истек срок действия сертификата X.509                              | Центр Интернета вещей     | Истек срок действия сертификата устройства X.509                                                                                                                                                                                                                                                                                                                                                                                                        |
| Средний   | Не удалось выполнить локальное имя входа                                     | Агент       | Обнаружено неудачной локального попытки входа на устройство.                                                                                                                                                                                                                                                                                                                                                                                     |
| Средний   | Работа средства шифрования монеты                                      | Агент       | Обнаружено выполнение процесса, обычно связанных с интеллектуальным анализом цифровой валюты.                                                                                                                                                                                                                                                                                                                                                        |
| Средний   | Доступ к средствам известных учетных данных                          | Агент       | Обнаруженное использование средство, обычно связанном с злоумышленников, попытка доступа к учетным данным.                                                                                                                                                                                                                                                                                                                                                             |
| Средний   | Образ контейнера miner Crypto монеты                      | Агент       | Контейнер, который запускает изображений известные цифровой валюты miner была обнаружена.                                                                                                                                                                                                                                                                                                                                                                     |
| Средний   | Средство известные направления атаки                                      | Агент       | Обнаружено средство, часто связанные с другими машинами иным образом атаки злоумышленников.                                                                                                                                                                                                                                                                                                                                              |
| Средний   | Доступ к обнаружен файл htaccess                       | Агент       | Анализ данных узла обнаружил возможных манипуляции htaccess файла. Htaccess является файлом настройкой с широкими возможностями, который позволяет выполнять несколько изменений на веб-сервере, на которых выполняется программное обеспечение Apache Web, включая перенаправление основные функциональные возможности, или для более сложные функции, такие как основные парольной защиты. Злоумышленники часто будет изменять файлы htaccess на компьютерах, которые они скомпрометированы для получения постоянного хранения.                    |
| Средний   | Поведение, аналогичное common Linux программы-роботы обнаружено         | Агент       | Выполнение процесса, обычно связанных с общих ботнетов Linux обнаружено.                                                                                                                                                                                                                                                                                                                                                              |
| Средний   | Подозрительный файл скачать затем выполнить действие             | Агент       | Анализ данных узла обнаружила файл загрузить, а затем запустите в одной команде. Это распространенные методики, которую злоумышленники используют для получения вредоносных файлов на компьютерах жертвы.                                                                                                                                                                                                                                                              |
| Средний   | Возможную потерю данных обнаружены                         | Агент       | Анализ данных узла обнаружила условие данных исходящий трафик (потерей данных). Обычно злоумышленники часто вывод данных из компьютеров, которые они скомпрометированы.                                                                                                                                                                                                                                                                                                      |
| Средний   | Пробел после имени файла| Агент | Анализ данных узла указывает выполнение процесса с подозрительным расширением. Подозрительные расширений могут обманом вынудить пользователей думать, файлы можно открыть, и может указывать на наличие вредоносных программ в системе.                                                                                                                                                                                                                   |
| Средний   | Предоставленный управляющую программу Docker с TCP-сокет                    | Агент       | Журналы машины указывают, что управляющая программа Docker (dockerd) предоставляет TCP-сокет. По умолчанию конфигурацию Docker, не использует шифрование или проверку подлинности при включении TCP-сокет. Предоставленный сокета TCP обеспечивает полный доступ к любой пользователь с доступом к нужному порту, управляющей программе Docker.                                                                                                                                                          |
| Средний   | Поведения, схожего с программами-шантажистами Fairware обнаружена       | Агент       | Анализ данных узла обнаружил выполнение команд -rf rm, примененных к подозрительных расположениях. RM -rf рекурсивно удаляет файлы, и он обычно используется в отдельные папки. В настоящее время он используется в расположении, которое способен удалить многие ваши данные. Программы-шантажиста Fairware известно, что выполнение команды rm -rf в этой папке.                                                                                                    |
| Средний   | Возможные черный обнаружено                             | Агент       | Анализ данных узла обнаружен подозрительный файл загружен, и последующем запуске подозрительный файл исходящий от загрузки в вашей подписке. Это действие, ранее была связана с установкой программы.                                                                                                                                                                                                                                                |
| Средний   | Обнаружено разведка локального узла                     | Агент       | Анализ данных узла обнаружил выполнения команды, обычно связанных с разведка bot common Linux.                                                                                                                                                                                                                                                                                                                         |
| Средний   | Переопределение потенциальных общих файлов                   | Агент       | Распространенные исполняемый файл, перезаписаны на устройстве. Злоумышленники обычно перезаписывать общие файлы в так, чтобы скрыть их действий или для получения сохраняемости в среде.                                                                                                                                                                                                                                                                                              |
| Средний   | Загрузка файлов обнаружил из известного вредоносного источника   | Агент       | Анализ данных устройства обнаружила загрузки файла из источника известных вредоносных программ.                                                                                                                                                                                                                                                                                                                                                    |
| Средний   | Привилегированные контейнер обнаружил                          | Агент       | Журналы машин означающий привилегированных контейнера Docker. Привилегированные контейнер имеет полный доступ к ресурсам узла. При компрометации злоумышленник может использовать привилегированных контейнера для получения доступа к хост-компьютере.                                                                                                                                                                                                     |
| Средний   | Поведение, аналогичное обнаружил программ-шантажистов                | Агент       | Анализ данных устройства обнаружил выполнение файлов, имеющих искомому известных программ-шантажистов, который может запрещает пользователям доступ к своей системы или личные файлы и требований ransom оплаты Чтобы получить доступ.                                                                                                                                                                                                           |
| Средний   | Удаление системных журналов обнаружено файлов                  | Агент       | Анализ данных узла обнаружила подозрительное удаление файлов журнала на узле.                                                                                                                                                                                                                                                                                                                                                             |
| Средний   | Несовпадение интерпретатор сценариев и расширение файла | Агент       | Анализ данных узла обнаружено несоответствие интерпретатор сценариев и расширение файла скрипта, указаны во входных данных. Это часто была связана с злоумышленник выполнения скриптов.                                                                                                                                                                                                                                       |
| Средний   | Подозрительные компиляции обнаружены                        | Агент       | Анализ данных узла обнаружил подозрительное компиляции. Злоумышленники часто скомпилирует эксплойтов на компьютере, они скомпрометированы для повышения прав.                                                                                                                                                                                                                                                                                     |
| Средний   | Обнаруженное подозрительное использование команды nohup           | Агент       | Анализ данных узла обнаружила подозрительное использование команды nohup на узле. Злоумышленники наблюдались выполнению nohup команды во временный каталог для их исполняемые файлы в фоновом режиме. Это не обычный, чтобы увидеть эту команду запуска на файлах, расположенных во временном каталоге.                                                                                                                                      |
| Средний   | Обнаруженное подозрительное использование команды "useradd"         | Агент       | Анализ данных узла было обнаружено подозрительное использование команды "useradd" на устройстве                                                                                                                                                                                                                                                                                                                                                      |
| Средний   | Подозрительные IP-адрес связь                    | Агент       | Обнаруженные подозрительные IP-адрес связь, основанных на анализе копии данных устройства.                                                                                                                                                                                                                                                                                                                                                         |
| Низкий      | Истек срок действия маркера SAS                                      | Центр Интернета вещей     | На устройстве использовался просроченных маркеров SAS                                                                                                                                                                                                                                                                                                                                                                                                   |
| Низкий      | Автоматические устройства                                          | Агент       | Устройство не отправило данные телеметрии за последние 72 часа                                                                                                                                                                                                                                                                                                                                                                                 |
| Низкий      | Очистить журнал bash                                   | Агент       | Очистить журнал bash. Злоумышленники могут скрыть свои собственные команды из отображаются в журналах                                                                                                                                                                                                                                                                                                                                                   |
| Низкий      | Не удалось подбора                                      | Агент       | Определены несколько попыток неудачной попытки входа. Попыток и неудачных грубой принудительно атака была сделана на устройстве.                                                                                                                                                                                                                                                                                                                                                  |

## <a name="built-in-alerts-for-iot-hub"></a>Встроенные оповещения для центра Интернета вещей

| severity | ИМЯ                                                                         | Описание                                                                                                                                                                                                                                                                                                                            |
|----------|------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Средний   | Новый сертификат, добавленный к центру Интернета вещей                                    |Новый сертификат, добавленный к центру Интернета вещей. Если это добавление не стал авторизованных сторона, это может означать вредоносные действия.                                                                                                                                                                                                    |
| Средний   | Сертификат удален из центра Интернета вещей                                    | Сертификат удален из центра Интернета вещей. Если это действие не было сделано авторизованной стороны, это может означать вредоносные действия.                                                                                                                                                                                                |
| Средний   | После неудачной попытки обнаружил Добавление сертификата в центр Интернета вещей     | Неудачная попытка добавить обнаружил сертификат для центра Интернета вещей. Если это действие не было сделано авторизованной стороны, это может означать вредоносные действия.                                                                                                                                                                     |
| Средний   | После неудачной попытки обнаружено, чтобы удалить сертификат из центра Интернета вещей | Обнаружено Неудачная попытка удалить сертификат из центра Интернета вещей. Если это действие не было сделано авторизованной стороны, это может означать вредоносные действия.                                                                                                                                                                |
| Низкий      | Попытка добавить или изменить параметр диагностики обнаружено центра Интернета вещей    | Попытка добавить или изменить параметры диагностики центра Интернета вещей обнаружено. Параметры диагностики позволяют повторно создать действие след в целях контроля при возникновении инцидента безопасности или несанкционированного доступа к сети. Если это действие не было сделано авторизованной стороны, это может означать вредоносные действия.  |
| Низкий      | Попытка удаления из центра Интернета вещей обнаружен параметр диагностики       | Попытка удалить параметры диагностики обнаружено центра Интернета вещей. Параметр диагностики позволяет повторно создать действие след в целях контроля при возникновении инцидента безопасности или несанкционированного доступа к сети. Если это действие не было сделано авторизованной стороны, это может указывать на вредоносные действия.       |
|

## <a name="see-also"></a>См. также

- [Обзор](overview.md)
- [Доступ к данным безопасности](how-to-security-data-access.md)
- [Исследовать устройство](how-to-investigate-device.md)