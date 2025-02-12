---
title: Часовые пояса управляемый экземпляр базы данных SQL Azure | Документация по Microsoft»
description: Дополнительные сведения о характеристиках часового пояса базы данных управляемого экземпляра SQL Azure
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
manager: craigg
ms.date: 07/05/2019
ms.openlocfilehash: 05ec49c98c5bcfe40346550f5570c03a8fb3f881
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657993"
---
# <a name="time-zones-in-azure-sql-database-managed-instance"></a>Часовые пояса в базе данных управляемого экземпляра SQL Azure

Формате UTC (UTC) — это рекомендуемый часовой пояс для уровня данных облачных решений. База данных управляемый экземпляр SQL Azure также предлагает выбрать часовых поясов для удовлетворения потребностей существующих приложений, которые хранят значения даты и времени и вызова функции даты и времени с неявное контекстом объекта конкретного часового пояса.

Функции T-SQL, например [GETDATE()](https://docs.microsoft.com/sql/t-sql/functions/getdate-transact-sql) или код CLR наблюдать, заданные в экземпляре уровня часовой пояс. Задания агента SQL Server также выполните расписания, согласно часовому поясу экземпляра.

  >[!NOTE]
  > Управляемый экземпляр является единственным развертывания базы данных SQL Azure, который поддерживает параметр часового пояса. Другие варианты развертывания всегда следуйте UTC.
Используйте [AT TIME ZONE](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql) в единый и использование пулов SQL баз данных, если вам нужно интерпретировать сведения о дате и времени в часовом поясе, отличном от UTC.

## <a name="supported-time-zones"></a>Поддерживаемые часовых поясов

Набор поддерживаемых часовых поясов наследуется от базовой операционной системы управляемого экземпляра. Чтобы получить новые определения часового пояса и отражать изменения в существующие постоянно обновляется.

[Летнее время/часовой пояс изменяет политику](https://aka.ms/time) гарантирует точность исторических 2010 вперед.

Список с именами, поддерживаемых часовых поясов предоставляется с помощью [sys.time_zone_info](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql) системное представление.

## <a name="set-a-time-zone"></a>Задать часовой пояс

Во время создания экземпляра только можно задать часовой пояс управляемого экземпляра. Часовой пояс по умолчанию — UTC.

  >[!NOTE]
  > Невозможно изменить часовой пояс существующего управляемого экземпляра.

### <a name="set-the-time-zone-through-the-azure-portal"></a>Задайте часовой пояс на портале Azure

При вводе параметров для нового экземпляра выберите часовой пояс из списка поддерживаемых часовых поясов.
  
![Установка часового пояса во время создания экземпляра](media/sql-database-managed-instance-timezone/01-setting_timezone-during-instance-creation.png)

### <a name="azure-resource-manager-template"></a>Шаблон Azure Resource Manager

Укажите свойство идентификатор часового пояса в вашей [шаблона Resource Manager](https://aka.ms/sql-mi-create-arm-posh) задать часовой пояс во время создания экземпляра.

```json
"properties": {
                "administratorLogin": "[parameters('user')]",
                "administratorLoginPassword": "[parameters('pwd')]",
                "subnetId": "[parameters('subnetId')]",
                "storageSizeInGB": 256,
                "vCores": 8,
                "licenseType": "LicenseIncluded",
                "hardwareFamily": "Gen5",
                "collation": "Serbian_Cyrillic_100_CS_AS",
                "timezoneId": "Central European Standard Time"
            },

```

Список допустимых значений для свойства идентификатор часового пояса — в конце этой статьи.

Если не указан, часовой пояс устанавливается в формате UTC.

## <a name="check-the-time-zone-of-an-instance"></a>Проверить часовой пояс экземпляра

[CURRENT_TIMEZONE](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql) функция возвращает отображаемое имя часового пояса экземпляра.

## <a name="cross-feature-considerations"></a>Вопросы с разными компонентами

### <a name="restore-and-import"></a>Восстановление и импорта

Можно восстановить файл резервной копии или импорта данных в управляемый экземпляр из экземпляра или сервера с параметрами в другом часовом поясе. Убедитесь в том, что необходимо соблюдать осторожность. Анализ поведения приложения и результаты запросов и отчетов, так же, как при передаче данных между двумя экземплярами SQL Server с параметрами в другом часовом поясе.

### <a name="point-in-time-restore"></a>Восстановление до точки во времени

<del>При выполнении восстановления на определенный момент времени для восстановления интерпретируется как время в формате UTC. Этот параметр позволяет избежать неоднозначность из-за летнего времени и его возможные изменения.<del>

 >[!WARNING]
  > Текущее поведение не согласуется с приведенной выше инструкции, и требуется восстановить интерпретируется согласно часовой пояс, где резервные копии баз данных автоматически создаются из управляемого экземпляра источника. Мы работаем над исправление это поведение для интерпретации в заданной точке во времени как время в формате UTC. См. в разделе [известные проблемы](sql-database-managed-instance-timezone.md#known-issues) для получения дополнительных сведений.

### <a name="auto-failover-groups"></a>Группы автоматической отработки отказа

Используя тот же часовой пояс, по экземпляр основного и дополнительного в группу отработки отказа не налагаются, но мы настоятельно рекомендуем.

  >[!WARNING]
  > Настоятельно рекомендуется использовать тот же часовой пояс для основного и дополнительного экземпляра в группу отработки отказа. Из-за некоторых редких случаях сохранение тот же часовой пояс между основным и дополнительным экземплярами не задается. Важно понимать, что в случае отработки отказа вручную или автоматически, дополнительный экземпляр будет сохранять часовой пояс.

## <a name="limitations"></a>Ограничения

- Невозможно изменить часовой пояс для существующего управляемого экземпляра.
- Внешних процессов, запускаемых из заданий агента SQL Server не заметить часового пояса экземпляра.

## <a name="known-issues"></a>Известные проблемы

В определенный момент времени восстановления (PITR) операции, время на восстановление для интерпретируется согласно часового пояса значение в управляемом экземпляре, где резервные копии баз данных автоматически создаются, несмотря на то, что страница портала для PITR предполагает, что время интерпретируется как в формате UTC.

Пример:

Предположим, что этот экземпляр, где Автоматическая архивация из имеет набор часовой пояс Восточное время (UTC-5).
Страница портала для восстановления на момент предполагает, что время, выбранное для восстановления является время в формате UTC:

![PITR с местным временем, с помощью портала](media/sql-database-managed-instance-timezone/02-pitr-with-nonutc-timezone.png)

Тем не менее время на восстановление для фактически интерпретируется как Eastern Standard Time, и в этом примере база данных будет восстановлена до состояния на 9 по восточному стандартному времени, а не время в формате UTC.

Если вы хотите выполнить восстановление на момент времени на определенный момент времени в формате UTC, сначала вычислить эквивалентное время в часовом поясе исходного экземпляра и используйте это время на портале или скрипт PowerShell или интерфейса командной строки.

## <a name="list-of-supported-time-zones"></a>Список поддерживаемых часовых поясов

| **Идентификатор часового пояса** | **Отображаемое имя часового пояса** |
| --- | --- |
| Меридиан (зима) | (UTC – 12:00) Линия перемены дат |
| UTC-11 | (UTC – 11:00) Всемирное координированное время – 11 |
| Алеутские (зима) | (UTC – 10:00) Алеутские острова |
| Гавайское время (зима) | (UTC – 10:00) Гавайи |
| Маркизские (зима) | (UTC – 09:30) Маркизские острова |
| Аляскинское время | (UTC – 09:00) Аляска |
| UTC – 09 | (UTC – 09:00) Всемирное координированное время – 09 |
| Тихоокеанское время (Мексика) | (UTC – 08:00) Нижняя Калифорния |
| UTC-08 | (UTC – 08:00) Всемирное координированное время – 08 |
| По стандартному тихоокеанскому времени | (UTC – 08:00) Тихоокеанское время (США и Канада) |
| Горное время США | (UTC – 07:00) Аризона |
| Горное время Мексики (зима) | (UTC – 07:00) Чиуауа, Ла-Пас, Масатлан |
| Горное время | (UTC – 07:00) Горное время (США и Канада) |
| Центральная Америка (зима) | (UTC – 06:00) Центральная Америка |
| Центральное стандартное время | (UTC – 06:00) Центральное поясное время (США и Канада) |
| Остров Пасхи (зима) | (UTC – 06:00) Остров Пасхи |
| Центральное время Мексики (зима) | (UTC – 06:00) Гвадалахара, Мехико, Монтеррей |
| Центральная Канада (зима) | (UTC – 06:00) Саскачеван |
| SA-Тихоокеанское время | (UTC – 05:00) Богота, Лима, Кито, Риу-Бранку |
| Восточное время Мексики (зима) | (UTC – 05:00) Четумаль |
| Восточное время (зима) | (UTC – 05:00) Восточное поясное время (США и Канада) |
| Гаитянское стандартное время | (UTC – 05:00) Гаити |
| Куба (зима) | (UTC – 05:00) Гавана |
| Восточное время США | (UTC – 05:00) Индиана (восточная) |
| Острова Теркс и Кайкос (зима) | (UTC – 05:00) Острова Теркс и Кайкос |
| Парагвай (зима) | (UTC – 04:00) Асунсьон |
| Атлантическое время (зима) | (UTC – 04:00) Атлантическое время (Канада) |
| Венесуэла (зима) | (UTC – 04:00) Каракас |
| Центральная Бразилия (зима) | (UTC – 04:00) Куяба |
| Южноамериканское западное время (зима) | (UTC – 04:00) Джорджтаун, Ла-Пас, Манаус, Сан-Хуан |
| Тихоокеанское ю-ам | (UTC – 04:00) Сантьяго |
| Ньюфаундленд (зима) | (UTC – 03:30) Ньюфаундленд |
| Токантинс (зима) | (UTC – 03:00) Арагуаяна |
| E. Южная Америка (зима) | (UTC – 03:00) Бразилиа |
| SA Eastern Standard Time | (UTC – 03:00) Кайенна, Форталеза |
| Аргентина (зима) | (UTC – 03:00) Буэнос-Айрес |
| Гренландское время | (UTC – 03:00) Гренландия (Дания) |
| Монтевидео (зима) | (UTC – 03:00) Монтевидео |
| Стандартное время Магелланова Пролива | (UTC – 03:00) Пунта-Аренас |
| Сен-Пьер (зима) | (UTC – 03:00) Сен-Пьер и Микелон |
| Баия (зима) | (UTC – 03:00) Сальвадор |
| UTC-02 | (UTC – 02:00) Всемирное координированное время – 02 |
| Средняя Атлантика (зима) | (UTC – 02:00) Среднеатлантическое время (устаревшее) |
| Азорские о-ва (зима) | (UTC – 01:00) Азорские острова |
| Стандартное время Кабо-Верде | (UTC – 01:00) Острова Кабо-Верде |
| Формат UTC. | (UTC) Всемирное координированное время |
| По Гринвичу (зима) | (UTC + 00:00) Дублин, Эдинбург, Лиссабон, Лондон |
| По Гринвичу (зима) | (UTC + 00:00) Монровия, Рейкьявик |
| ВТ. Европа (зима) | (UTC + 01:00) Амстердам, Берлин, Берн, Рим, Стокгольм, Вена |
| Центральная Европа (зима) | (UTC + 01:00) Белград, Братислава, Будапешт, Любляна, Прага |
| Романское время (зима) | (UTC + 01:00) Брюссель, Копенгаген, Мадрид, Париж |
| Марокко (зима) | (UTC + 01:00) Касабланка |
| Сан-томе (зима) | (UTC + 01:00) Сан-томе |
| Центральная Европа (зима) | (UTC + 01:00) Сараево, Скопье, Варшава, Загреб |
| ВТ. Центральная Африка (зима) | (UTC + 01:00) Западная Центральная Африка |
| Иорданское время | (UTC + 02:00) Амман |
| Греция, ТУРЦИЯ (зима) | (UTC + 02:00) Афины, Бухарест |
| Ближнем Востоке (зима) | (UTC + 02:00) Бейрут |
| Египетское время | (UTC + 02:00) Каир |
| E. Европа (зима) | (UTC + 02:00) Кишинев |
| Сирия (зима) | (UTC + 02:00) Дамаск |
| Западный Берег реки Иордан (зима) | (UTC + 02:00) Газа, Хеврон |
| Южная Африка (зима) | (UTC + 02:00) Хараре, Претория |
| Финляндия (зима) | (UTC + 02:00) Хельсинки, Киев, Рига, София, Таллин, Вильнюс |
| Израиль (зима) | (UTC + 02:00) Иерусалим |
| Калининград (зима) | (UTC + 02:00) Калининград |
| Судан (зима) | (UTC + 02:00) Хартум |
| Ливия (зима) | (UTC + 02:00) Триполи |
| Намибийское время | (UTC + 02:00) Виндхук |
| Арабский (зима) | (UTC + 03:00) Багдад |
| Турция (зима) | (UTC + 03:00) Стамбул |
| Арабская (зима) | (UTC + 03:00) Кувейт, Эр-Рияд |
| Беларусь (зима) | (UTC + 03:00) Минск |
| Русский (зима) | (UTC + 03:00) Москва, Санкт-Петербург |
| E. Африка (зима) | (UTC + 03:00) Найроби |
| Иран (зима) | (UTC + 03:30) Тегеран |
| Арабское время (зима) | (UTC + 04:00) Абу-Даби, Маскат |
| Астраханское стандартное время | (UTC + 04:00) Астрахань, Ульяновск |
| Азербайджанское время | (UTC + 04:00) Баку |
| Россия часовой пояс 3 | (UTC + 04:00) Ижевск, Самара |
| Маврикий (зима) | (UTC + 04:00) Порт-Луи |
| Саратовское стандартное время | (UTC + 04:00) Саратовская |
| Грузинский (зима) | (UTC + 04:00) Тбилиси |
| Волгоград (зима) | (UTC + 04:00) Волгоград |
| Кавказское время | (UTC + 04:00) Ереван |
| Афганистан (зима) | (UTC + 04:30) Кабул |
| Западная Азия (зима) | (UTC + 05:00) Ашхабад, Ташкент |
| Уральское время | (UTC + 05:00) Екатеринбург |
| Пакистан (зима) | (UTC + 05:00) Исламабад, Карачи |
| Индия (зима) | (UTC + 05:30) Ченнаи, Колката, Мумбаи, Нью-Дели |
| Шри-Ланка (зима) | (UTC + 05:30) Шри-Джаявардене-пура-Котте |
| Непальское время | (UTC + 05:45) Катманду |
| Центральная Азия (зима) | (UTC + 06:00) Астана |
| Бангладеш (зима) | (UTC + 06:00) Дакка |
| Омск (зима) | (UTC + 06:00) Омск |
| Мьянмарское время | (UTC + 06:30) Янгон (Рангун) |
| ЮГО-Восточная Азия (зима) | (UTC + 07:00) Бангкок, Ханой, Джакарта |
| Алтайское стандартное время | (UTC + 07:00) Барнаул, Горно-Алтайск |
| ВТ. Монголия (зима) | (UTC + 07:00) Ховд |
| Северная Азия (зима) | (UTC + 07:00) Красноярск |
| N. Центральная Азия (зима) | (UTC + 07:00) Новосибирск |
| Томск (зима) | (UTC + 07:00) Томск |
| Китай (зима) | (UTC + 08:00) Пекин, Чунцин, Гонконг, Урумчи |
| Красноярское время (зима) | (UTC + 08:00) Иркутск |
| Сингапур (зима) | (UTC + 08:00) Куала-Лумпур, Сингапур |
| ВТ. Австралия (зима) | (UTC + 08:00) Перт |
| Тайваньское время | (UTC + 08:00) Тайбэй |
| Улан-Батор (зима) | (UTC + 08:00) Улан-Батор |
| Центрально-Западная W. (зима) | (UTC + 08:45) Юкла |
| Забайкальское стандартное время | (UTC + 09:00) Чита |
| Токийское время | (UTC + 09:00) Осака, Саппоро, Токио |
| КНДР (зима) | (UTC + 09:00) Пхеньян |
| Корея (зима) | (UTC + 09:00) Сеул |
| Якутское время | (UTC + 09:00) Якутск |
| CEN. Австралия (зима) | (UTC + 09:30) Аделаида |
| Центральная Австралия (зима) | (UTC + 09:30) Дарвин |
| E. Австралия (зима) | (UTC + 10:00) Брисбейн |
| Стандартное Восточное время | (UTC + 10:00) Канберра, Мельбурн, Сидней |
| Западная по стандартному тихоокеанскому времени | (UTC + 10:00) Гуам, Порт-Морсби |
| Тасманийское время | (UTC + 10:00) Хобарт |
| Владивосток (зима) | (UTC + 10:00) Владивосток |
| Лорд-Хау (зима) | (UTC + 10:30) Остров Лорд-Хау |
| Бугенвиль (зима) | (UTC + 11:00) Остров Бугенвиль |
| Россия часовой пояс 10 | (UTC + 11:00) Чокурдах |
| Магадан (зима) | (UTC + 11:00) Магадан |
| Норфолк (зима) | (UTC + 11:00) Остров Норфолк |
| Сахалинское стандартное время | (UTC + 11:00) Сахалин |
| Центр по стандартному тихоокеанскому времени | (UTC + 11:00) Соломоновы острова, Новая Каледония |
| Россия часовой пояс 11 | (UTC + 12:00) Анадырь, Петропавловск-Камчатский |
| Новая Зеландия (зима) | (UTC + 12:00) Окленд, Веллингтон |
| UTC + 12 | (UTC + 12:00) Всемирное координированное время + 12 |
| Фиджи (зима) | (UTC + 12:00) Фиджи |
| Камчатка (зима) | (UTC + 12:00) Петропавловск-Камчатский (устаревшее) |
| Чатем (зима) | (UTC + 12:45) Острова Чатем |
| UTC + 13 | (UTC + 13:00) Скоординированное универсальной время + 13 |
| Тонга (зима) | (UTC + 13:00) Нукуалофа |
| Самоа (зима) | (UTC + 13:00) Самоа |
| О-ва лайн (зима) | (UTC + 14:00) Остров Киритимати |

## <a name="see-also"></a>См. также 

- [CURRENT_TIMEZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/functions/current-timezone-transact-sql)
- [AT TIME ZONE (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/at-time-zone-transact-sql)
- [sys.time_zone_info (Transact-SQL)](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-time-zone-info-transact-sql)
