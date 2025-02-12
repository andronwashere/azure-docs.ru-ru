---
title: Безопасность и конфиденциальность данных в службе "Поиск Azure"
description: Служба "Поиск Azure" совместима с SOC 2, HIPAA и другими сертификатами. Подключение, шифрование данных, проверка подлинности и доступ на основе удостоверений с использованием идентификаторов безопасности пользователей и групп в фильтрах службы "Поиск Azure".
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: f366726f539a817f515a78fbc35bfeaa3b65514e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65024503"
---
# <a name="security-and-data-privacy-in-azure-search"></a>Безопасность и конфиденциальность данных в службе "Поиск Azure"

Служба "Поиск Azure" содержит встроенные комплексные функции безопасности и элементы управления доступом, которые позволяют сохранить конфиденциальность личного содержимого. В этой статье перечислены функции безопасности, встроенные в службу "Поиск Azure", и приводятся сведения о соответствии стандартам.

Архитектура безопасности службы "Поиск Azure" объединяет физическую безопасность, зашифрованную передачу данных, зашифрованное хранилище и соответствие стандартам на уровне платформы. С точки зрения операций служба "Поиск Azure" принимает только запросы, прошедшие проверку подлинности. При необходимости можно добавить элементы управления доступом к содержимому для отдельного пользователя с помощью фильтров безопасности. В этой статье описывается безопасность на каждом уровне, но основное внимание уделяется способам защиты данных и операций в службе поиска Azure.

## <a name="standards-compliance-iso-27001-soc-2-hipaa"></a>Соответствие стандартам: ISO 27001, SOC 2, HIPAA

[Как было объявлено в июне 2018 г.](https://azure.microsoft.com/blog/azure-search-is-now-certified-for-several-levels-of-compliance/), служба "Поиск Azure" сертифицирована на соответствие следующим стандартам:

+ [ISO 27001:2013](https://www.iso.org/isoiec-27001-information-security.html). 
+ [SOC 2 типа 2](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html). Чтобы просмотреть полный отчет, перейдите к [отчету SOC 2 типа II для Azure для государственных организаций](https://servicetrust.microsoft.com/ViewPage/MSComplianceGuide?command=Download&downloadType=Document&downloadId=93292f19-f43e-4c4e-8615-c38ab953cf95&docTab=4ce99610-c9c0-11e7-8c2c-f908a777fa4d_SOC%20%2F%20SSAE%2016%20Reports). 
+ [Акт о передаче и защите данных учреждений здравоохранения (HIPAA)](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act).
+ [GxP (21 CFR, ч. 11)](https://en.wikipedia.org/wiki/Title_21_CFR_Part_11).
+ [HITRUST](https://en.wikipedia.org/wiki/HITRUST).
+ [PCI DSS, уровень 1](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard).
+ [Программа IRAP австралийского правительства (без грифа секретности)](https://asd.gov.au/infosec/irap/certified_clouds.htm).

Соответствие стандартам относится к общедоступным функциям. Функции в предварительной версии будут сертифицированы, когда станут общедоступными. Не следует их использовать в решениях со строгими требованиями к соответствию стандартам. Сведения о сертификатах соответствия см. в документе [Overview of Microsoft Azure compliance](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) (Общие сведения о соответствии требованиям Microsoft Azure) и в [центре управления безопасностью](https://www.microsoft.com/en-us/trustcenter). 

## <a name="encrypted-transmission-and-storage"></a>Зашифрованная передача и хранение

Шифрование действует во всем конвейере индексирования — от соединений, при передачах и до индексированных данных, хранящихся в службе поиска Azure.

| Уровень безопасности | Описание |
|----------------|-------------|
| Шифрование при передаче <br>(HTTPS, SSL, TLS) | Поиск Azure прослушивает через HTTPS-порт 443. Подключения к службам Azure по всей платформе зашифрованы. <br/><br/>Все взаимодействия между клиентом и службой "Поиск Azure" осуществляются по протоколу SSL/TLS 1.2  Обязательно используйте TLS версии 1.2 для SSL-подключений к службе.|
| Шифрование при хранении <br>Ключи, управляемые Майкрософт | Шифрование происходит внутренне в процессе индексирования и не оказывает заметного влияния на время выполнения индексирования или размер индекса. Оно выполняется автоматически во всех процессах индексирования, включая добавочные обновления индекса, который не полностью зашифрован (создан до января 2018 г.).<br><br>На внутреннем уровне шифрование основывается на [шифровании службы хранилища Azure](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) с использованием 256-разрядного [шифрования AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard).<br><br> Шифрование является внутренним для службы поиска Azure, а сертификаты и ключи шифрования внутренне управляются корпорацией Майкрософт и применяются ко всем компонентам. Вы не можете включать или отключать шифрование, подставлять собственные ключи или управлять ими, просматривать параметры шифрования на портале или с помощью программных средств.<br><br>Шифрование при хранении стало общедоступным 24 января 2018 года и применяется ко всем уровням служб, включая общие (бесплатные) службы во всех регионах. Для полного шифрования индексы, созданные до этой даты, необходимо удалить и создать заново, чтобы шифрование вступило в силу. В противном случае будут зашифрованы только новые данные, добавленные после 24 января.|
| Шифрование при хранении <br>Ключами управляет пользователь | Шифрование с управляемыми клиентом ключами **предварительной версии** служб компонент, который не доступен бесплатно. Для платных служб, он доступен только для поиска служб, созданных на или после января 2019 г., с помощью последней предварительной версии api-version (api-version = 2019-05-06-Preview).<br><br>Индексы поиска Azure и карт синонимов можно теперь шифруются при хранении с помощью управляемых ключей пользователя ключей в хранилище ключей Azure. Дополнительные сведения см. в разделе [управлять ключами шифрования в службе поиска Azure](search-security-manage-encryption-keys.md).<br>Эта функция не заменяет значение по умолчанию шифрование при хранении, но вместо этого применяется в дополнение к его.<br>Включение этой функции увеличивают размер индекса и привести к снижению производительности запросов. Исходя из наблюдений до даты, вы можно ожидать увеличение из 30 – 60% время на запрос, несмотря на то, что фактическая производительность будет зависеть от определения индекса и типов запросов. Из-за такого влияния на производительность рекомендуется включать только этой функции для индексов, которые действительно необходимы.

## <a name="azure-wide-user-access-controls"></a>Элементы для управления доступом пользователей в Azure

В Azure есть несколько механизмов безопасности. Следовательно, они автоматически доступны для созданных вами ресурсов Поиска Azure.

+ [Блокировки на уровне подписки или ресурсов для предотвращения удаления](../azure-resource-manager/resource-group-lock-resources.md)
+ [Управление доступом на основе ролей (RBAC) для управления доступом к информации и административным операциям](../role-based-access-control/overview.md)

Все службы Azure поддерживают элементы управления доступом на основе ролей (RBAC) для согласованного задания уровней доступа во всех службах. Например, просматривать конфиденциальные данные, такие как ключ администратора, могут только пользователи с ролью владельца или участника, а состояние службы доступно для обладателей любой роли. RBAC предоставляет роли владельца, участника и читателя. По умолчанию все администраторы службы являются участниками роли владельца.

<a name="service-access-and-authentication"></a>

## <a name="service-access-and-authentication"></a>Доступ к службе и проверка подлинности

Кроме того, что Поиск Azure наследует меры безопасности платформы Azure, он также обеспечивает собственную проверку подлинности на основе ключей. Ключ API является строкой, состоящей из случайно сгенерированных букв и цифр. Тип ключа (администратор или запрос) определяет уровень доступа. Отправка допустимого ключа представляет собой доказательство того, что запрос исходит от доверенной сущности. 

Существует два уровня доступа к службе поиска, включаемые двумя типами ключей.

* Доступ администратора (применяется к любым операциям чтения и записи в отношении службы).
* Доступ по запросу (доступно для операций только для чтения, таких как запросы и коллекции документов индекса)

*Ключи администратора* создаются в процессе подготовки службы. Существует два ключа администратора, обозначенные для упорядочения как *первичный* и *вторичный*, хотя в действительности они являются взаимозаменяемыми. У каждой службы есть два ключа администратора, чтобы вы могли сменять их, не теряя доступа к службе. Вы можете [ключ повторно создать администратора](search-security-api-keys.md#regenerate-admin-keys) периодически по безопасности Azure рекомендации, но нельзя добавить к общему количеству ключей администратора. Существует более двух ключей администратора на одну службу поиска.

*Ключи запросов* создаются по требованию и предназначены для клиентских приложений, выполнять запросы. Вы можете создать до 50 ключей поисковых запросов. В коде приложения укажите URL-адрес поиска и api ключ запроса, чтобы разрешить доступ только для чтения к коллекции документов указанного индекса. Используемые вместе конечная точка, ключ API с доступом только для чтения и целевой индекс определяют область применения и уровень доступа для подключения из клиентского приложения.

Проверка подлинности требуется для каждого запроса, который состоит из обязательного ключа, операции и объекта. Для обеспечения полной безопасности операций службы достаточно двух уровней разрешений (полные или только для чтения) и контекста (например, для операции запроса к индексу). Дополнительные сведения о ключах см. в статье о [создании ключей api и управлении ими](search-security-api-keys.md).

## <a name="index-access"></a>Доступ к индексу

В Поиске Azure какой-либо индекс не является защищаемым объектом. Вместо этого доступ к индексу определяется на уровне службы (доступ на чтение или запись), а также контекстом операции.

В случае с доступом пользователей можно структурировать запросы для подключения с использованием ключа запроса (после чего любой запрос становится доступным только для чтения) и добавить конкретный индекс, используемый приложением. В запросе невозможно объединить индексы или получить доступ к нескольким индексам одновременно, поэтому все запросы по определению направляются к одному индексу. Таким образом, структура самого запроса (ключ и один целевой индекс) определяет границы безопасности.

Доступ администратора и разработчика к индексам недифференцированный: для обоих типов доступа необходимо разрешение на запись для создания, удаления и обновления объектов, управляемых службой. Любой пользователь, имеющий ключ администратора к службе, может прочитать, изменить или удалить любой индекс в той же службе. Решением для защиты от случайного или злонамеренного удаления индексов являются внутренние средства управления исходным кодом для ресурсов кода, которые позволяют отменить нежелательное удаление или изменение индекса. Служба "Поиск Azure" выполняет отработку отказа в кластере для обеспечения доступности, но она не хранит и не выполняет ваш собственный код, используемый для создания или загрузки индексов.

Для мультитенантных решений, границы безопасности которых должны находиться на уровне индексов, такие решения, как правило, содержат средний уровень, на котором клиенты обрабатывают изоляцию индекса. Дополнительные сведения о вариантах использования мультитенантных решений см. в статье [Шаблоны разработки для мультитенантных приложений SaaS и Поиска Azure](search-modeling-multitenant-saas-applications.md).

## <a name="admin-access"></a>Административный доступ

[Доступ на основе ролей (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) определяет, имеется ли доступ к элементам управления через службы и его содержимого. Если вы являетесь владельцем или участником на службу поиска Azure, можно использовать портал или PowerShell **Az.Search** модуль для создания, обновления или удаления объектов в службе. Можно также использовать [API REST управления поиском Azure](https://docs.microsoft.com/rest/api/searchmanagement/search-howto-management-rest-api).

## <a name="user-access"></a>Доступ пользователя

По умолчанию доступ пользователя к индексу определяется ключом доступа в запросе. Большинство разработчиков создают и назначают [*ключи запросов*](search-security-api-keys.md) для поисковых запросов на стороне клиента. Благодаря ключу запроса можно получить доступ на чтение всего содержимого индекса.

Если требуется детальный, индивидуально настраиваемый элемент управления над содержимым, можно компилировать фильтры безопасности в запросах, возвращающих документы, связанные с заданным удостоверением безопасности. Вместо предопределенных ролей и назначений ролей управление доступом на основе удостоверений реализуется как *фильтр*, обрезающий результаты поиска документов и содержимого на основе удостоверений. В следующей таблице описаны два подхода к обрезке результатов поиска несанкционированного содержимого.

| Подход | Описание |
|----------|-------------|
|[Security filters for trimming results in Azure Search](search-security-trimming-for-azure-search.md) (Фильтры безопасности для обрезки результатов Поиска Azure)  | Документирует базовый рабочий процесс для реализации управления доступом на основе удостоверений пользователей. Описывает добавление удостоверений безопасности в индекс, а также объясняет фильтрацию этого поля для усечения результатов запрещенного содержимого. |
|[Security filters for trimming Azure Search results using Active Directory identities](search-security-trimming-for-azure-search-with-aad.md) (Фильтры безопасности для обрезки результатов Поиска Azure с использованием удостоверений Active Directory)  | Эта статья дополняет предыдущую статью, в ней приведены действия для получения удостоверений из Azure Active Directory (AAD), одной из [бесплатных служб](https://azure.microsoft.com/free/) на облачной платформе Azure. |

## <a name="table-permissioned-operations"></a>Таблица. Разрешенные операции

В следующей таблице приведены операции, разрешенные в Поиске Azure, и указано, какие ключи используются для разблокировки доступа к конкретной операции.

| Операция | Разрешения |
|-----------|-------------------------|
| Создание службы | Владелец подписки Azure|
| Масштабирование службы | Ключ администратора, владелец или участник RBAC ресурса  |
| удаление службы; | Ключ администратора, владелец или участник RBAC ресурса |
| Создание, изменение, удаление объектов в службе: <br>индексы и элементы компонентов (включая определения анализатора, профили оценки, параметры CORS), индексаторы, источники данных, синонимы, средства подбора. | Ключ администратора, владелец или участник RBAC ресурса  |
| Запрос индекса | Ключ администратора или запроса (RBAC не применяется) |
| Запрос сведений о системе, например возвращение статистики, количества и списков объектов. | Ключ администратора, RBAC ресурса (владелец, участник или читатель) |
| Управление ключами администратора | Ключ администратора, владелец или участник RBAC ресурса |
| Управление ключами запросов |  Ключ администратора, владелец или участник RBAC ресурса.  |

## <a name="physical-security"></a>Физическая безопасность

Центры обработки данных Майкрософт обеспечивают ведущую в отрасли физическую безопасность и соответствуют широкому спектру стандартов и нормативных требований. Дополнительные сведения см. на странице [глобальных центров обработки данных](https://www.microsoft.com/cloud-platform/global-datacenters) или ознакомьтесь с коротким видеороликом о безопасности центров обработки данных.

> [!VIDEO https://www.youtube.com/embed/r1cyTL8JqRg]


## <a name="see-also"></a>См. также

+ [Создание индекса службы поиска Azure с помощью пакета SDK для .NET](search-create-index-dotnet.md)
+ [Создание индекса службы поиска Azure с помощью REST API](search-create-index-rest-api.md)
+ [Security filters for trimming results in Azure Search](search-security-trimming-for-azure-search.md) (Фильтры безопасности для обрезки результатов Поиска Azure)
+ [Security filters for trimming Azure Search results using Active Directory identities](search-security-trimming-for-azure-search-with-aad.md) (Фильтры безопасности для обрезки результатов Поиска Azure с использованием удостоверений Active Directory)
+ [Фильтры в службе "Поиск Azure"](search-filters.md)