---
title: Операции поставщиков ресурсов Azure Resource Manager | Документация Майкрософт
description: Списки операций, доступных для поставщиков ресурсов Microsoft Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/11/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: ce4765a512b81d13f735a05ad4fba5408284a607
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501413"
---
# <a name="azure-resource-manager-resource-provider-operations"></a>Операции поставщиков ресурсов Azure Resource Manager

В этой статье перечислены операции, доступные в каждом поставщике ресурсов Azure Resource Manager. Эти операции могут использоваться в [настраиваемых ролях](custom-roles.md), чтобы обеспечить детализированное [управление доступом на основе ролей (RBAC)](overview.md) для ресурсов в Azure. Строки операций имеют следующий формат: `{Company}.{ProviderName}/{resourceType}/{action}`

Операции в поставщиках ресурсов всегда развиваются. Чтобы получить список последних операций, используйте команду [Get-AzProviderOperation](/powershell/module/az.resources/get-azprovideroperation) или [az provider operation list](/cli/azure/provider/operation#az-provider-operation-list).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.AAD/domainServices/delete | Удаление службы домена |
> | Action | Microsoft.AAD/domainServices/oucontainer/delete | Удаление контейнера подразделения |
> | Action | Microsoft.AAD/domainServices/oucontainer/read | Чтение контейнеров подразделения |
> | Action | Microsoft.AAD/domainServices/oucontainer/write | Запись в контейнер подразделения |
> | Action | Microsoft.AAD/domainServices/read | Чтение доменных служб |
> | Action | Microsoft.AAD/domainServices/replicaSets/delete | Удаление сайта кластера |
> | Action | Microsoft. AAD/domainServices/Репликасетс/чтение | Чтение сайта кластера |
> | Action | Microsoft. AAD/domainServices/Репликасетс/запись | Запись сайта кластера |
> | Action | Microsoft.AAD/domainServices/write | Запись службы домена |
> | Action | Microsoft.AAD/locations/operationresults/read |  |
> | Action | Microsoft.AAD/Operations/read |  |
> | Action | Microsoft.AAD/register/action | Регистрация службы домена |
> | Action | Microsoft.AAD/unregister/action | Отмена регистрации службы домена |

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | microsoft.aadiam/diagnosticsettings/delete | Удаление параметра диагностики. |
> | Action | microsoft.aadiam/diagnosticsettings/read | Чтение параметра диагностики. |
> | Action | microsoft.aadiam/diagnosticsettings/write | Запись параметра диагностики. |
> | Action | microsoft.aadiam/diagnosticsettingscategories/read | Чтение категорий параметра диагностики. |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Addons/operations/read | Получает поддерживаемые операции RP. |
> | Action | Microsoft.Addons/register/action | Регистрирует указанную подписку в Microsoft.Addons. |
> | Action | Microsoft.Addons/supportProviders/listsupportplaninfo/action | Выводит сведения о текущем плане поддержки для указанной подписки. |
> | Action | Microsoft.Addons/supportProviders/supportPlanTypes/delete | Удаляет указанный план поддержки Canonical. |
> | Action | Microsoft.Addons/supportProviders/supportPlanTypes/read | Получает состояние указанного плана поддержки Canonical. |
> | Action | Microsoft.Addons/supportProviders/supportPlanTypes/write | Добавляет указанный тип плана поддержки Canonical. |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ADHybridHealthService/addsservices/action | Создает лес для клиента. |
> | Action | Microsoft.ADHybridHealthService/addsservices/addomainservicemembers/read | Получает все серверы для указанного имени службы. |
> | Action | Microsoft.ADHybridHealthService/addsservices/alerts/read | Получает сведения об оповещениях для леса, например идентификатор оповещения, дата получения, последнее обнаружение, описание, последнее обновление, уровень и состояние оповещения, а также ссылки на устранение неполадок с оповещением и т. д. |
> | Action | Microsoft.ADHybridHealthService/addsservices/configuration/read | Получает конфигурацию службы для леса. Например, имя леса, функциональный уровень, роль хозяина именования доменов FSMO, роль хозяина схемы FSMO и т. д. |
> | Action | Microsoft.ADHybridHealthService/addsservices/delete | Удаляет службу и ее серверы, а также данные о работоспособности. |
> | Action | Microsoft.ADHybridHealthService/addsservices/dimensions/read | Получает сведения о доменах и сайтах для леса. Например, состояние работоспособности, активные оповещения, разрешенные оповещения, такие свойства, как функциональный уровень домена, лес, хозяин инфраструктуры, PDC, хозяин RID и т. д.  |
> | Action | Microsoft.ADHybridHealthService/addsservices/features/userpreference/read | Получает параметр настройки пользователя для леса.<br>Например, значения MetricCounterName, такие как ldapsuccessfulbinds, ntlmauthentications, kerberosauthentications, addsinsightsagentprivatebytes, ldapsearches.<br>Параметры диаграммы пользовательского интерфейса и т. д. |
> | Action | Microsoft.ADHybridHealthService/addsservices/forestsummary/read | Получает сводку для данного леса, например имя леса, количество доменов в лесу, количество сайтов, сведения о сайтах и т. д. |
> | Action | Microsoft.ADHybridHealthService/addsservices/metricmetadata/read | Получает список поддерживаемых метрик для данной службы.<br>Например, число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы ADSync. |
> | Action | Microsoft.ADHybridHealthService/addsservices/metrics/groups/read | Для данной службы этот API получает информацию о метриках.<br>Например, этот API можно использовать для получения следующей информации: число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы синхронизации. |
> | Action | Microsoft.ADHybridHealthService/addsservices/premiumcheck/read | Этот API возвращает список всех подключенных служб ADDomainServices для клиента категории "Премиум". |
> | Action | Microsoft.ADHybridHealthService/addsservices/read | Получает сведения о службе для указанного имени службы. |
> | Action | Microsoft.ADHybridHealthService/addsservices/replicationdetails/read | Получает сведения о репликации всех серверов для указанного имени службы. |
> | Action | Microsoft.ADHybridHealthService/addsservices/replicationstatus/read | Получает число контроллеров домена и их ошибок репликации, если таковые имеются. |
> | Action | Microsoft.ADHybridHealthService/addsservices/replicationsummary/read | Получает полный список контроллеров домена и сведений о репликации для данного леса. |
> | Action | Microsoft.ADHybridHealthService/addsservices/servicemembers/action | Добавляет экземпляр сервера в службу. |
> | Action | Microsoft.ADHybridHealthService/addsservices/servicemembers/credentials/read | Во время регистрации сервера ADDomainService этот API вызывается, чтобы получить учетные данные для подключения новых серверов. |
> | Action | Microsoft.ADHybridHealthService/addsservices/servicemembers/delete | Удаляет сервер для данной службы и клиента. |
> | Action | Microsoft.ADHybridHealthService/addsservices/write | Создает или обновляет экземпляр ADDomainService для клиента. |
> | Action | Microsoft.ADHybridHealthService/configuration/action | Обновляет конфигурацию клиента. |
> | Action | Microsoft.ADHybridHealthService/configuration/read | Считывает конфигурацию клиента. |
> | Action | Microsoft.ADHybridHealthService/configuration/write | Создает конфигурацию клиента. |
> | Action | Microsoft.ADHybridHealthService/logs/contents/read | Получает содержимое журналов установки и регистрации агента, хранимых в большом двоичном объекте. |
> | Action | Microsoft.ADHybridHealthService/logs/read | Получает журналы установки и регистрации агента для клиента. |
> | Action | Microsoft.ADHybridHealthService/operations/read | Возвращает список операций, поддерживаемых системой. |
> | Action | Microsoft.ADHybridHealthService/register/action | Регистрирует поставщик ресурсов службы работоспособности ADHybrid и позволяет создавать ресурсы службы работоспособности ADHybrid. |
> | Action | Microsoft.ADHybridHealthService/reports/availabledeployments/read | Получает список доступных регионов, используемых командой DevOps для поддержки инцидентов клиентов. |
> | Action | Microsoft.ADHybridHealthService/reports/badpassword/read | Получает список попыток неправильного ввода пароля для всех пользователей в службе федерации Active Directory. |
> | Action | Microsoft.ADHybridHealthService/reports/badpassworduseridipfrequency/read | Получает универсальный код ресурса SAS большого двоичного объекта, содержащий состояние и конечный результат недавно поставленного в очередь задания отчета для определения частоты попыток неправильного ввода имени пользователя или пароля на каждый идентификатор пользователя по каждому IP-адресу в день для данного клиента. |
> | Action | Microsoft.ADHybridHealthService/reports/consentedtodevopstenants/read | Получает список клиентов, получивших согласие DevOps. Обычно используется для поддержки клиентов. |
> | Action | Microsoft.ADHybridHealthService/reports/isdevops/read | Получение значения, указывающего, имеет ли клиент согласие DevOps. |
> | Action | Microsoft.ADHybridHealthService/reports/selectdevopstenant/read | Обновляет идентификатор пользователя (идентификатор объекта) выбранного клиента DevOps. |
> | Action | Microsoft.ADHybridHealthService/reports/selecteddeployment/read | Возвращает выбранное развертывание для данного клиента. |
> | Action | Microsoft.ADHybridHealthService/reports/tenantassigneddeployment/read | Получает расположение хранилища клиента на основе идентификатора клиента. |
> | Action | Microsoft.ADHybridHealthService/reports/updateselecteddeployment/read | Возвращает географическое расположение, из которого будет осуществляться доступ к данных. |
> | Action | Microsoft.ADHybridHealthService/services/action | Обновляет экземпляр службы в клиенте. |
> | Action | Microsoft.ADHybridHealthService/services/alerts/read | Считывает оповещения для службы. |
> | Action | Microsoft.ADHybridHealthService/services/alerts/read | Считывает оповещения для службы. |
> | Action | Microsoft.ADHybridHealthService/services/checkservicefeatureavailibility/read | На основе имени функции проверяет, имеется ли у службы все необходимое для использования этой функции. |
> | Action | Microsoft.ADHybridHealthService/services/delete | Удаляет экземпляр службы из клиента. |
> | Action | Microsoft.ADHybridHealthService/services/exporterrors/read | Возвращает ошибки экспорта для данной службы синхронизации. |
> | Action | Microsoft.ADHybridHealthService/services/exportstatus/read | Возвращает состояние экспорта для данной службы. |
> | Action | Microsoft.ADHybridHealthService/services/feedbacktype/feedback/read | Получает обратную связь об оповещениях для данной службы и сервера. |
> | Action | Microsoft.ADHybridHealthService/services/metricmetadata/read | Получает список поддерживаемых метрик для данной службы.<br>Например, число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы ADSync. |
> | Action | Microsoft.ADHybridHealthService/services/metrics/groups/average/read | Этот API получает среднее значение метрик для данной службы.<br>Например, этот API можно использовать для получения следующей информации: число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы синхронизации. |
> | Action | Microsoft.ADHybridHealthService/services/metrics/groups/read | Для данной службы этот API получает информацию о метриках.<br>Например, этот API можно использовать для получения следующей информации: число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы синхронизации. |
> | Action | Microsoft.ADHybridHealthService/services/metrics/groups/sum/read | Этот API получает агрегированное представление о метриках для данной службы.<br>Например, этот API можно использовать для получения следующей информации: число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы синхронизации. |
> | Action | Microsoft.ADHybridHealthService/services/monitoringconfiguration/write | Добавляет или обновляет конфигурацию мониторинга для службы. |
> | Action | Microsoft.ADHybridHealthService/services/monitoringconfigurations/read | Получает конфигурации мониторинга для данной службы. |
> | Action | Microsoft.ADHybridHealthService/services/monitoringconfigurations/write | Добавляет или обновляет конфигурации мониторинга для службы. |
> | Action | Microsoft.ADHybridHealthService/services/premiumcheck/read | Этот API возвращает список всех подключенных служб для клиента категории "Премиум". |
> | Action | Microsoft.ADHybridHealthService/services/read | Считывает экземпляры службы в клиенте. |
> | Action | Microsoft. Адхибридхеалссервице/Services/Reports/Блобурис/Read | Возвращает все нерискованные URI отчетов IP за последние семь дней. |
> | Action | Microsoft.ADHybridHealthService/services/reports/details/read | Возвращает отчет о первых 50 пользователях, вводивших неправильный пароль, за последние 7 дней. |
> | Action | Microsoft. Адхибридхеалссервице/Services/Reports/Женератеблобури/Action | Создает рискованный IP-отчет и возвращает URI, указывающий на него. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/action | Создает экземпляр службы в клиенте. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/alerts/read | Считывает оповещения для сервера. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/credentials/read | Во время регистрации сервера этот API вызывается, чтобы получить учетные данные для подключения новых серверов. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/datafreshness/read | Для данного сервера этот API возвращает список типов данных, которые передаются серверами, и самое позднее время каждой отправки. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/delete | Удаляет экземпляр службы из службы. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/exportstatus/read | Возвращает сведения об ошибке экспорта данных синхронизации для указанной службы синхронизации. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/metrics/groups/read | Для данной службы этот API получает информацию о метриках.<br>Например, этот API можно использовать для получения следующей информации: число блокировок учетных записей из экстрасети, общее число невыполненных запросов, число невыполненных запросов маркера (прокси), число запросов токенов/с и т. д. для службы ADFS.<br>Количество проверок подлинности NTLM/с, число успешных привязок LDAP/с, время привязки LDAP, количество активных потоков LDAP, число проверок подлинности Kerberos/с, общее количество потоков ATQ и т. д. для службы ADDomain.<br>Задержка запуска профиля, установленные TCP-подключения, частные байты агента получения информации, статистика по экспорту в Azure AD для службы синхронизации. |
> | Action | Microsoft. Адхибридхеалссервице/Services/сервицемемберс/метрики/чтение | Возвращает список соединителей и имена профилей выполнения для заданной службы и члена службы. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/read | Считывает экземпляр сервера в службе. |
> | Action | Microsoft.ADHybridHealthService/services/servicemembers/serviceconfiguration/read | Получает конфигурацию службы для данного клиента. |
> | Action | Microsoft.ADHybridHealthService/services/tenantwhitelisting/read | Получает состояние разрешенных функций для данного клиента. |
> | Action | Microsoft.ADHybridHealthService/services/write | Создает экземпляр службы в клиенте. |
> | Action | Microsoft.ADHybridHealthService/unregister/action | Отменяет регистрацию подписки для поставщика ресурсов службы работоспособности ADHybrid. |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Advisor/configurations/read | Получение конфигураций |
> | Action | Microsoft.Advisor/configurations/write | Создание или обновление конфигурации. |
> | Action | Microsoft.Advisor/generateRecommendations/action | Создает рекомендации. |
> | Action | Microsoft.Advisor/generateRecommendations/read | Возвращает состояние создания рекомендаций. |
> | Action | Microsoft. Advisor/метаданные/чтение | Получить метаданные |
> | Action | Microsoft.Advisor/operations/read | Получение операций для Microsoft Advisor. |
> | Action | Microsoft.Advisor/recommendations/available/action | В Microsoft Advisor доступна новая рекомендация. |
> | Action | Microsoft.Advisor/recommendations/read | Считывает рекомендации. |
> | Action | Microsoft.Advisor/recommendations/suppressions/delete | Удаляет подавления. |
> | Action | Microsoft.Advisor/recommendations/suppressions/read | Возвращает подавления. |
> | Action | Microsoft.Advisor/recommendations/suppressions/write | Создает или обновляет подавления. |
> | Action | Microsoft.Advisor/register/action | Регистрирует подписку для Microsoft Advisor. |
> | Action | Microsoft.Advisor/suppressions/delete | Удаляет подавления. |
> | Action | Microsoft.Advisor/suppressions/read | Возвращает подавления. |
> | Action | Microsoft.Advisor/suppressions/write | Создает или обновляет подавления. |
> | Action | Microsoft.Advisor/unregister/action | Отмена регистрации подписки для Microsoft Advisor. |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.AlertsManagement/actionRules/delete | Удаление правила действия в рамках заданной подписки. |
> | Action | Microsoft.AlertsManagement/actionRules/read | Получение всех правил действий для фильтров входящих данных. |
> | Action | Microsoft.AlertsManagement/actionRules/write | Создание или обновление правила действия в рамках заданной подписки. |
> | Action | Microsoft.AlertsManagement/alerts/changestate/action | Изменение состояния оповещения. |
> | Action | Microsoft.AlertsManagement/alerts/diagnostics/read | Получение всех данных диагностики для оповещения. |
> | Action | Microsoft.AlertsManagement/alerts/history/read | Получение журнала оповещения. |
> | Action | Microsoft.AlertsManagement/alerts/read | Получение всех оповещений для фильтров входа. |
> | Action | Microsoft.AlertsManagement/alertsList/read | Получение всех оповещений для фильтров входящих данных в рамках подписок. |
> | Action | Microsoft. Алертсманажемент/Алертсметадата/Read | Получение метаданных предупреждений для входного параметра. |
> | Action | Microsoft.AlertsManagement/alertsSummary/read | Получение сводки оповещений. |
> | Action | Microsoft.AlertsManagement/alertsSummaryList/read | Получение сводки оповещений для подписок. |
> | Action | Microsoft.AlertsManagement/Operations/read | Чтение предоставленных операций. |
> | Action | Microsoft.AlertsManagement/register/action | Регистрация подписки для Управления оповещениями (Майкрософт). |
> | Action | Microsoft. Алертсманажемент/Смартдетекторалертрулес/Delete | Удаление правила оповещения Smart детектора в заданной подписке |
> | Action | Microsoft. Алертсманажемент/Смартдетекторалертрулес/Read | Получить все правила генерации оповещений смарт-детекторов для входных фильтров |
> | Action | Microsoft. Алертсманажемент/Смартдетекторалертрулес/запись | Создание или обновление правила генерации оповещений Smart детектор в заданной подписке |
> | Action | Microsoft.AlertsManagement/smartGroups/changestate/action | Изменение состояния смарт-группы. |
> | Action | Microsoft.AlertsManagement/smartGroups/history/read | Получение журнала смарт-группы. |
> | Action | Microsoft.AlertsManagement/smartGroups/read | Получение всех смарт-групп для фильтров ввода. |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.AnalysisServices/locations/checkNameAvailability/action | Проверяет, является ли данное имя сервера анализа данных допустимым и неиспользуемым. |
> | Action | Microsoft.AnalysisServices/locations/operationresults/read | Получение сведений о результате указанной операции. |
> | Action | Microsoft.AnalysisServices/locations/operationstatuses/read | Извлечение сведений о состоянии указанной операции. |
> | Action | Microsoft.AnalysisServices/operations/read | Извлечение сведений об операциях. |
> | Action | Microsoft.AnalysisServices/register/action | Регистрация поставщика ресурсов для служб Analysis Services. |
> | Action | Microsoft.AnalysisServices/servers/delete | Удаляет сервер анализа данных. |
> | Action | Microsoft.AnalysisServices/servers/listGatewayStatus/action | Отображение состояния шлюза, связанного с сервером. |
> | Action | Microsoft.AnalysisServices/servers/read | Извлекает информацию об указанном сервере анализа данных. |
> | Action | Microsoft.AnalysisServices/servers/resume/action | Возобновляет работу сервера анализа данных. |
> | Action | Microsoft.AnalysisServices/servers/skus/read | Получение доступных сведений о номере SKU для сервера. |
> | Action | Microsoft.AnalysisServices/servers/suspend/action | Приостанавливает сервер анализа данных. |
> | Action | Microsoft.AnalysisServices/servers/write | Создает или обновляет указанный сервер анализа данных. |
> | Action | Microsoft.AnalysisServices/skus/read | Извлечение сведений о номерах SKU. |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ApiManagement/checkNameAvailability/read | Проверяет, доступно ли указанное имя службы. |
> | Action | Microsoft.ApiManagement/operations/read | Чтение всех операций API для ресурса Microsoft.ApiManagement. |
> | Action | Microsoft.ApiManagement/register/action | Регистрирует подписку для поставщика ресурсов Microsoft.ApiManagement. |
> | Action | Microsoft.ApiManagement/reports/read | Получение отчетов, объединенных по периодам времени, географическим регионам, разработчикам, продуктам, API-интерфейсам, операциям, подпискам и запросам. |
> | Action | Microsoft.ApiManagement/service/apis/delete | Удаляет указанный API экземпляра службы управления API. |
> | Action | Microsoft.ApiManagement/service/apis/diagnostics/delete | Удаляет указанное средство диагностики из API. |
> | Action | Microsoft.ApiManagement/service/apis/diagnostics/read | Список всех средств диагностики API. или возвращает сведения о диагностике для API, заданного его идентификатором. |
> | Action | Microsoft.ApiManagement/service/apis/diagnostics/write | Создает новую диагностику для API или обновляет существующую. или обновляет сведения о диагностике для API, заданного его идентификатором. |
> | Action | Microsoft.ApiManagement/service/apis/issues/attachments/delete | Удаляет указанный комментарий из проблемы. |
> | Action | Microsoft.ApiManagement/service/apis/issues/attachments/read | Список всех вложений для проблемы, связанной с указанным API. или возвращает сведения о вложении проблем для API, заданного его идентификатором. |
> | Action | Microsoft.ApiManagement/service/apis/issues/attachments/write | Создает новое вложение для проблемы в API или обновляет существующее. |
> | Action | Microsoft.ApiManagement/service/apis/issues/comments/delete | Удаляет указанный комментарий из проблемы. |
> | Action | Microsoft.ApiManagement/service/apis/issues/comments/read | Список всех комментариев для проблемы, связанной с указанным API. или возвращает сведения о комментарии к вопросу для API, заданного его идентификатором. |
> | Action | Microsoft.ApiManagement/service/apis/issues/comments/write | Создает новый комментарий для проблемы в API или обновляет существующий. |
> | Action | Microsoft.ApiManagement/service/apis/issues/delete | Удаляет указанную ошибку из API. |
> | Action | Microsoft.ApiManagement/service/apis/issues/read | Список всех проблем, связанных с указанным API. или возвращает сведения о проблемах для API, заданного его идентификатором. |
> | Action | Microsoft.ApiManagement/service/apis/issues/write | Создает новую ошибку для API или обновляет существующую. или обновляет существующую ошибку для API. |
> | Action | Microsoft.ApiManagement/service/apis/operations/delete | Удаляет указанную операцию в API. |
> | Action | Microsoft.ApiManagement/service/apis/operations/policies/delete | Удаляет конфигурацию политики в операции API. |
> | Action | Microsoft.ApiManagement/service/apis/operations/policies/read | Получите список конфигурации политики на уровне операций API. или получите конфигурацию политики на уровне операций API. |
> | Action | Microsoft.ApiManagement/service/apis/operations/policies/write | Создает или обновляет конфигурацию политики для уровня операций API. |
> | Action | Microsoft.ApiManagement/service/apis/operations/policy/delete | Удаление конфигурации политики на уровне операций |
> | Action | Microsoft.ApiManagement/service/apis/operations/policy/read | Получение конфигурации политики на уровне операций |
> | Action | Microsoft.ApiManagement/service/apis/operations/policy/write | Создание конфигурации политики на уровне операций |
> | Action | Microsoft.ApiManagement/service/apis/operations/read | Перечисляет коллекцию операций для указанного API. или возвращает сведения об операции API, указанной в ее идентификаторе. |
> | Action | Microsoft.ApiManagement/service/apis/operations/tags/delete | Отсоедините тег от операции. |
> | Action | Microsoft.ApiManagement/service/apis/operations/tags/read | Список всех тегов, связанных с операцией. или получить тег, связанный с операцией. |
> | Action | Microsoft.ApiManagement/service/apis/operations/tags/write | Назначьте тег операции. |
> | Action | Microsoft.ApiManagement/service/apis/operations/write | Создает новую операцию в API или обновляет существующую. или обновляет сведения об операции в API, указанном с помощью ее идентификатора. |
> | Action | Microsoft.ApiManagement/service/apis/operationsByTags/read | Содержит коллекцию операций, связанных с тегами. |
> | Action | Microsoft.ApiManagement/service/apis/policies/delete | Удаляет конфигурацию политики в API. |
> | Action | Microsoft.ApiManagement/service/apis/policies/read | Получите конфигурацию политики на уровне API. или получите конфигурацию политики на уровне API. |
> | Action | Microsoft.ApiManagement/service/apis/policies/write | Создает или обновляет конфигурацию политики для API. |
> | Action | Microsoft.ApiManagement/service/apis/policy/delete | Удаление конфигурации политики на уровне API |
> | Action | Microsoft.ApiManagement/service/apis/policy/read | Получение конфигурации политики на уровне API |
> | Action | Microsoft.ApiManagement/service/apis/policy/write | Создание конфигурации политики на уровне API |
> | Action | Microsoft.ApiManagement/service/apis/products/read | Список всех продуктов, частью которых является API. |
> | Action | Microsoft.ApiManagement/service/apis/read | Список всех API экземпляра службы управления API. или возвращает сведения об API, заданном по его идентификатору. |
> | Action | Microsoft.ApiManagement/service/apis/releases/delete | Удаляет все выпуски API или удаляет указанный выпуск в API. |
> | Action | Microsoft.ApiManagement/service/apis/releases/read | Выводит список всех выпусков API.<br>Версия API создается при изменении текущей редакции API.<br>Выпуски также используются для отката к предыдущим редакциям.<br>Результаты будут разбиваться на страницы и могут быть ограничены параметрами $top и $skip.<br>или возвращает сведения о выпуске API. |
> | Action | Microsoft.ApiManagement/service/apis/releases/write | Создает новый выпуск для API. или обновляет сведения о выпуске API, заданного его идентификатором. |
> | Action | Microsoft.ApiManagement/service/apis/revisions/delete | Удаление всех редакций API. |
> | Action | Microsoft.ApiManagement/service/apis/revisions/read | Перечисляет все редакции API. |
> | Action | Microsoft.ApiManagement/service/apis/schemas/delete | Удаляет конфигурацию схемы в API. |
> | Action | Microsoft.ApiManagement/service/apis/schemas/read | Получите конфигурацию схемы на уровне API. или получите конфигурацию схемы на уровне API. |
> | Action | Microsoft.ApiManagement/service/apis/schemas/write | Создает или обновляет конфигурацию схемы для API. |
> | Action | Microsoft.ApiManagement/service/apis/tagDescriptions/delete | Удалите описание тега для API. |
> | Action | Microsoft.ApiManagement/service/apis/tagDescriptions/read | Список всех описаний тегов в области API. Модель, похожая на Swagger-Тагдескриптион, определена на уровне API, но тег может быть назначен для операций или получения описания тега в области API. |
> | Action | Microsoft.ApiManagement/service/apis/tagDescriptions/write | Создайте или обновите описание тега в области действия API. |
> | Action | Microsoft.ApiManagement/service/apis/tags/delete | Отсоедините тег от API. |
> | Action | Microsoft.ApiManagement/service/apis/tags/read | Список всех тегов, связанных с API. или получить тег, связанный с API. |
> | Action | Microsoft.ApiManagement/service/apis/tags/write | Назначьте тегу API. |
> | Action | Microsoft.ApiManagement/service/apis/write | Создает новый или обновляет существующий указанный API экземпляра службы управления API. или обновляет указанный API экземпляра службы управления API. |
> | Action | Microsoft.ApiManagement/service/apisByTags/read | Содержит коллекцию интерфейсов API, связанных с тегами. |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/delete | Удаляет конкретный набор версий API. |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/read | Содержит коллекцию наборов версий API в указанном экземпляре службы. или возвращает сведения о наборе версий API, заданного его идентификатором. |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/versions/read | Получение списка сущностей версии. |
> | Action | Microsoft.ApiManagement/service/apiVersionSets/write | Создает или обновляет набор версий API. или обновляет сведения о наборе версий API, заданном идентификатором. |
> | Action | Microsoft.ApiManagement/service/applynetworkconfigurationupdates/action | Обновляет ресурсы Microsoft.ApiManagement, работающие в виртуальной сети, для применения обновленных параметров сети. |
> | Action | Microsoft.ApiManagement/service/authorizationServers/delete | Удаляет конкретный экземпляр сервера авторизации. |
> | Action | Microsoft.ApiManagement/service/authorizationServers/read | Содержит коллекцию серверов авторизации, определенных в экземпляре службы. или возвращает сведения о сервере авторизации, указанном по его идентификатору. |
> | Action | Microsoft.ApiManagement/service/authorizationServers/write | Создает новый сервер авторизации или обновляет существующий. или обновляет сведения о сервере авторизации, указанном с помощью его идентификатора. |
> | Action | Microsoft.ApiManagement/service/backends/delete | Удаляет указанный сервер. |
> | Action | Microsoft.ApiManagement/service/backends/read | Перечисляет коллекцию интерфейсов в указанном экземпляре службы. или возвращает сведения о серверной части, указанной в ее идентификаторе. |
> | Action | Microsoft.ApiManagement/service/backends/reconnect/action | Уведомляет прокси-сервер APIM о создании нового соединения с серверной частью по истечении заданного времени ожидания. Если время ожидания не указано, используется время ожидания, равное 2 минутам. |
> | Action | Microsoft.ApiManagement/service/backends/write | Создает или обновляет серверную часть. или обновляет существующую серверную часть. |
> | Action | Microsoft.ApiManagement/service/backup/action | Архивирует службу управления API в указанный контейнер в предоставленной пользователем учетной записи хранения. |
> | Action | Microsoft. ApiManagement/Service/кэши/удаление | Удаляет конкретный кэш. |
> | Action | Microsoft. ApiManagement/служба/кэш/чтение | Перечисляет коллекцию всех внешних кэшей в указанном экземпляре службы. или возвращает сведения о кэше, заданном его идентификатором. |
> | Action | Microsoft. ApiManagement/служба/кэши/запись | Создает или обновляет внешний кэш для использования в экземпляре управления API. или обновляет сведения о кэше, заданном его идентификатором. |
> | Action | Microsoft.ApiManagement/service/certificates/delete | Удаляет конкретный сертификат. |
> | Action | Microsoft.ApiManagement/service/certificates/read | Перечисляет коллекцию всех сертификатов в указанном экземпляре службы. или возвращает сведения о сертификате, указанном по его идентификатору. |
> | Action | Microsoft.ApiManagement/service/certificates/write | Создает или обновляет сертификат, используемый для проверки подлинности в серверной части. |
> | Action | Microsoft.ApiManagement/service/contentTypes/contentItems/delete | Удаляет указанный элемент содержимого. |
> | Action | Microsoft.ApiManagement/service/contentTypes/contentItems/read | Возвращает список элементов содержимого или сведения об элементе содержимого. |
> | Action | Microsoft.ApiManagement/service/contentTypes/contentItems/write | Создает новый элемент содержимого или обновляет указанный элемент содержимого. |
> | Action | Microsoft.ApiManagement/service/contentTypes/read | Возвращает список типов содержимого. |
> | Action | Microsoft.ApiManagement/service/delete | Удаляет экземпляр службы управления API. |
> | Action | Microsoft.ApiManagement/service/diagnostics/delete | Удаляет указанное средство диагностики. |
> | Action | Microsoft.ApiManagement/service/diagnostics/read | Список всех диагностических сведений об экземпляре службы управления API. или возвращает сведения о диагностике, заданном идентификатором. |
> | Action | Microsoft.ApiManagement/service/diagnostics/write | Создает новую диагностику или обновляет существующую. или обновляет сведения о диагностике, заданные по идентификатору. |
> | Action | Microsoft. ApiManagement/служба/шлюзы/действие | Извлекает конфигурацию шлюза. или обновляет пульс шлюза. |
> | Action | Microsoft. ApiManagement/служба/шлюзы/удаление | Удаляет конкретный шлюз. |
> | Action | Microsoft. ApiManagement/служба/шлюзы/ключи/действие | Получает ключи шлюза. |
> | Action | Microsoft. ApiManagement/служба/шлюзы/чтение | Содержит коллекцию шлюзов, зарегистрированных в экземпляре службы. или возвращает сведения о шлюзе, заданном его идентификатором. |
> | Action | Microsoft. ApiManagement/служба/шлюзы/Реженератепримарикэй/действие | Повторно создает ключ первичного шлюза инвалидатионг все созданные им токены. |
> | Action | Microsoft. ApiManagement/служба/шлюзы/Реженератесекондарикэй/действие | Повторно создает вторичный ключ шлюза инвалидатионг все созданные им токены. |
> | Action | Microsoft. ApiManagement/служба/шлюзы, токен или действие | Возвращает маркер авторизации общего доступа для шлюза. |
> | Action | Microsoft. ApiManagement/служба/шлюзы/запись | Создает или обновляет шлюз, используемый в экземпляре управления API. или обновляет сведения о шлюзе, заданном идентификатором. |
> | Action | Microsoft.ApiManagement/service/getssotoken/action | Возвращает маркер единого входа, который может использоваться для входа на портал прежней версии службы управления API от имени администратора. |
> | Action | Microsoft.ApiManagement/service/groups/delete | Удаляет определенную группу экземпляра службы управления API. |
> | Action | Microsoft.ApiManagement/service/groups/read | Перечисляет коллекцию групп, определенных в экземпляре службы. или возвращает сведения о группе, заданной ее идентификатором. |
> | Action | Microsoft.ApiManagement/service/groups/users/delete | Удалить существующего пользователя из существующей группы. |
> | Action | Microsoft.ApiManagement/service/groups/users/read | Содержит коллекцию сущностей пользователей, связанных с группой. |
> | Action | Microsoft.ApiManagement/service/groups/users/write | Добавляет существующего пользователя в существующий продукт. |
> | Action | Microsoft.ApiManagement/service/groups/write | Создает или обновляет группу. или обновляет сведения о группе, указанной в ее идентификаторе. |
> | Action | Microsoft.ApiManagement/service/identityProviders/delete | Удаляет указанную конфигурацию поставщика удостоверений. |
> | Action | Microsoft.ApiManagement/service/identityProviders/read | Содержит коллекцию поставщиков удостоверений, настроенных в указанном экземпляре службы. или возвращает сведения о конфигурации поставщика удостоверений, настроенного в указанном экземпляре службы. |
> | Action | Microsoft.ApiManagement/service/identityProviders/write | Создает или обновляет конфигурацию IdentityProvider. или обновляет существующую конфигурацию IdentityProvider. |
> | Action | Microsoft. ApiManagement/Service/проблемы/чтение | Перечисляет коллекцию проблем в указанном экземпляре службы. или возвращает сведения о проблемах управления API |
> | Action | Microsoft.ApiManagement/service/locations/networkstatus/read | Получение состояния доступа к сетевым ресурсам, от которых зависит служба в расположении. |
> | Action | Microsoft.ApiManagement/service/loggers/delete | Удаляет указанное средство ведения журнала. |
> | Action | Microsoft.ApiManagement/service/loggers/read | Перечисляет коллекцию средств ведения журнала в указанном экземпляре службы. или возвращает сведения о средстве ведения журнала, заданном его идентификатором. |
> | Action | Microsoft.ApiManagement/service/loggers/write | Создает или обновляет средство ведения журнала. или обновляет существующее средство ведения журнала. |
> | Action | Microsoft.ApiManagement/service/managedeployments/action | Позволяет изменить номер SKU или единицы, а также добавить или удалить региональные развертывания службы управления API. |
> | Action | Microsoft.ApiManagement/service/networkstatus/read | Получение состояния доступа к сетевым ресурсам, от которых зависит служба. |
> | Action | Microsoft.ApiManagement/service/notifications/action | Отправление уведомления указанному пользователю. |
> | Action | Microsoft.ApiManagement/service/notifications/read | Перечисляет коллекцию свойств, определенных в экземпляре службы. или возвращает сведения об уведомлении, заданном его идентификатором. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientEmails/delete | Удаляет сообщение электронной почты из списка уведомлений. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientEmails/read | Возвращает список электронных писем получателей уведомлений, подписанных на уведомление. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientEmails/write | Добавляет адрес электронной почты в список получателей уведомления. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientUsers/delete | Удаляет пользователя управления API из списка уведомлений. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientUsers/read | Получает список пользователей уведомлений, подписанных на уведомление. |
> | Action | Microsoft.ApiManagement/service/notifications/recipientUsers/write | Добавляет пользователя управления API в список получателей уведомления. |
> | Action | Microsoft.ApiManagement/service/notifications/write | Создание или обновление уведомления издателя управления API. |
> | Action | Microsoft.ApiManagement/service/openidConnectProviders/delete | Удаляет конкретного поставщика OpenID Connect Connect экземпляра службы управления API. |
> | Action | Microsoft.ApiManagement/service/openidConnectProviders/read | Список всех поставщиков OpenID Connect Connect. или возвращает конкретного поставщика OpenID Connect Connect. |
> | Action | Microsoft.ApiManagement/service/openidConnectProviders/write | Создает или обновляет поставщик OpenID Connect Connect. или обновляет конкретного поставщика OpenID Connect Connect. |
> | Action | Microsoft.ApiManagement/service/operationresults/read | Возвращает текущее состояние длительной операции. |
> | Action | Microsoft.ApiManagement/service/policies/delete | Удаляет конфигурацию глобальной политики службы управления API. |
> | Action | Microsoft.ApiManagement/service/policies/read | Список всех определений глобальных политик службы управления API. или получите определение глобальной политики службы управления API. |
> | Action | Microsoft.ApiManagement/service/policies/write | Создает или обновляет конфигурацию глобальной политики службы управления API. |
> | Action | Microsoft. ApiManagement/служба/политика/удаление | Удаление конфигурации политики на уровне клиента |
> | Action | Microsoft. ApiManagement/служба/политика/чтение | Получение конфигурации политики на уровне клиента |
> | Action | Microsoft. ApiManagement/служба/политика/запись | Создание конфигурации политики на уровне клиента |
> | Action | Microsoft.ApiManagement/service/policySnippets/read | Список всех фрагментов политик. |
> | Action | Microsoft.ApiManagement/service/portalsettings/read | Получите параметры входа для портала или получите параметры регистрации для портала или получите параметры делегирования для портала. |
> | Action | Microsoft.ApiManagement/service/portalsettings/write | Обновите параметры входа. или создайте или обновите параметры входа. или обновить параметры регистрации или обновить параметры регистрации или изменить параметры делегирования. или создайте или обновите параметры делегирования. |
> | Action | Microsoft.ApiManagement/service/products/apis/delete | Удаляет указанный API из указанного продукта. |
> | Action | Microsoft.ApiManagement/service/products/apis/read | Содержит коллекцию интерфейсов API, связанных с продуктом. |
> | Action | Microsoft.ApiManagement/service/products/apis/write | Добавляет API к указанному продукту. |
> | Action | Microsoft.ApiManagement/service/products/delete | Удаление продукта. |
> | Action | Microsoft.ApiManagement/service/products/groups/delete | Удаляет связь между указанной группой и продуктом. |
> | Action | Microsoft.ApiManagement/service/products/groups/read | Перечисляет коллекцию групп разработчиков, связанных с указанным продуктом. |
> | Action | Microsoft.ApiManagement/service/products/groups/write | Добавляет связь между указанной группой разработчиков и указанным продуктом. |
> | Action | Microsoft.ApiManagement/service/products/policies/delete | Удаляет конфигурацию политики в продукте. |
> | Action | Microsoft.ApiManagement/service/products/policies/read | Получение конфигурации политики на уровне продукта. или получите конфигурацию политики на уровне продукта. |
> | Action | Microsoft.ApiManagement/service/products/policies/write | Создает или обновляет конфигурацию политики для продукта. |
> | Action | Microsoft.ApiManagement/service/products/policy/delete | Удаление конфигурации политики на уровне продукта |
> | Action | Microsoft.ApiManagement/service/products/policy/read | Получение конфигурации политики на уровне продукта |
> | Action | Microsoft.ApiManagement/service/products/policy/write | Создание конфигурации политики на уровне продукта |
> | Action | Microsoft.ApiManagement/service/products/read | Перечисляет коллекцию продуктов в указанном экземпляре службы. или возвращает сведения о продукте, указанном по его идентификатору. |
> | Action | Microsoft.ApiManagement/service/products/subscriptions/read | Перечисляет коллекцию подписок на указанный продукт. |
> | Action | Microsoft.ApiManagement/service/products/tags/delete | Отсоедините тег от продукта. |
> | Action | Microsoft.ApiManagement/service/products/tags/read | Список всех тегов, связанных с продуктом. или получить тег, связанный с продуктом. |
> | Action | Microsoft.ApiManagement/service/products/tags/write | Назначьте тег продукту. |
> | Action | Microsoft.ApiManagement/service/products/write | Создает или обновляет продукт. или обновить существующие сведения о продукте. |
> | Action | Microsoft.ApiManagement/service/productsByTags/read | Содержит коллекцию продуктов, связанных с тегами. |
> | Action | Microsoft.ApiManagement/service/properties/delete | Удаляет конкретное свойство из экземпляра службы управления API. |
> | Action | Microsoft.ApiManagement/service/properties/read | Перечисляет коллекцию свойств, определенных в экземпляре службы. или возвращает сведения о свойстве, заданном его идентификатором. |
> | Action | Microsoft.ApiManagement/service/properties/write | Создает или обновляет свойство. или обновляет конкретное свойство. |
> | Action | Microsoft.ApiManagement/service/quotas/periods/read | Получение значения счетчика квоты за период. |
> | Action | Microsoft.ApiManagement/service/quotas/periods/write | Настройка текущего значения счетчика квоты. |
> | Action | Microsoft.ApiManagement/service/quotas/read | Получение значений квоты. |
> | Action | Microsoft.ApiManagement/service/quotas/write | Настройка текущего значения счетчика квоты. |
> | Action | Microsoft.ApiManagement/service/read | Чтение метаданных для экземпляра службы управления API. |
> | Action | Microsoft. ApiManagement/служба/регионы/чтение | Список всех регионов Azure, в которых существует служба. |
> | Action | Microsoft.ApiManagement/service/reports/read | Получение отчета, объединенного по периодам времени, по географическим регионам или по разработчикам;<br>получение отчета, объединенного по продуктам;<br>получение отчета, объединенного по API-интерфейсам, операциям или по подпискам;<br>получение запросов, передающих данные. |
> | Action | Microsoft.ApiManagement/service/restore/action | Восстановление службы управления API из указанного контейнера в предоставленной пользователем учетной записи хранения. |
> | Action | Microsoft.ApiManagement/service/subscriptions/delete | Удаляет указанную подписку. |
> | Action | Microsoft.ApiManagement/service/subscriptions/read | Список всех подписок экземпляра службы управления API. или возвращает указанную сущность подписки. |
> | Action | Microsoft.ApiManagement/service/subscriptions/regeneratePrimaryKey/action | Повторно создает первичный ключ существующей подписки экземпляра службы управления API. |
> | Action | Microsoft.ApiManagement/service/subscriptions/regenerateSecondaryKey/action | Повторно создает вторичный ключ существующей подписки экземпляра службы управления API. |
> | Action | Microsoft.ApiManagement/service/subscriptions/write | Создает или обновляет подписку указанного пользователя на указанный продукт. или обновляет сведения о подписке, заданной ее идентификатором. |
> | Action | Microsoft.ApiManagement/service/tagResources/read | Содержит коллекцию ресурсов, связанных с тегами. |
> | Action | Microsoft.ApiManagement/service/tags/delete | Удаляет конкретный тег экземпляра службы управления API. |
> | Action | Microsoft.ApiManagement/service/tags/read | Содержит коллекцию тегов, определенных в экземпляре службы. или возвращает сведения о теге, указанном с помощью его идентификатора. |
> | Action | Microsoft.ApiManagement/service/tags/write | Создает тег. или обновляет сведения тега, указанного его идентификатором. |
> | Action | Microsoft.ApiManagement/service/templates/delete | Сброс к шаблону электронного сообщения по умолчанию службы управления API. |
> | Action | Microsoft.ApiManagement/service/templates/read | Получение всех шаблонов сообщений или сведений о шаблоне сообщения службы управления API. |
> | Action | Microsoft.ApiManagement/service/templates/write | Создание или обновление шаблона сообщения службы управления API. |
> | Action | Microsoft.ApiManagement/service/tenant/delete | Удаляет конфигурацию политики для клиента. |
> | Action | Microsoft.ApiManagement/service/tenant/deploy/action | Выполняет задачу развертывания для применения изменений из указанной ветви Git к конфигурации в базе данных. |
> | Action | Microsoft.ApiManagement/service/tenant/operationResults/read | Возвращает список результатов операций или результат определенной операции. |
> | Action | Microsoft.ApiManagement/service/tenant/read | Получите определение глобальной политики службы управления API. или получить сведения о доступе к клиенту |
> | Action | Microsoft.ApiManagement/service/tenant/regeneratePrimaryKey/action | Повторно создает первичный ключ доступа. |
> | Action | Microsoft.ApiManagement/service/tenant/regenerateSecondaryKey/action | Повторно создает вторичный ключ доступа. |
> | Action | Microsoft.ApiManagement/service/tenant/save/action | Создает фиксацию с использованием моментального снимка конфигурации для указанной ветви в репозитории. |
> | Action | Microsoft.ApiManagement/service/tenant/syncState/read | Возвращает состояние последней синхронизации с Git. |
> | Action | Microsoft.ApiManagement/service/tenant/validate/action | Проверяет изменения из указанной ветви Git. |
> | Action | Microsoft.ApiManagement/service/tenant/write | Установка конфигурации политики для клиента или обновление сведений о доступе к клиенту. |
> | Action | Microsoft.ApiManagement/service/updatecertificate/action | Передает SSL-сертификата для службы управления API. |
> | Action | Microsoft.ApiManagement/service/updatehostname/action | Позволяет настроить, обновить или удалить имена личных доменов для службы управления API. |
> | Action | Microsoft.ApiManagement/service/users/action | Регистрация нового пользователя. |
> | Action | Microsoft.ApiManagement/service/users/confirmations/send/action | Отправляет подтверждение. |
> | Action | Microsoft.ApiManagement/service/users/delete | Удаляет конкретного пользователя. |
> | Action | Microsoft.ApiManagement/service/users/generateSsoUrl/action | Извлекает URL-адрес перенаправления, содержащий маркер проверки подлинности для подписания данного пользователя на портал разработчика. |
> | Action | Microsoft.ApiManagement/service/users/groups/read | Список всех групп пользователей. |
> | Action | Microsoft. ApiManagement/Service/Users/identitys/Read | Список всех удостоверений пользователей. |
> | Action | Microsoft.ApiManagement/service/users/keys/read | Получение ключей, связанных с пользователем |
> | Action | Microsoft.ApiManagement/service/users/read | Перечисляет коллекцию зарегистрированных пользователей в указанном экземпляре службы. или возвращает сведения о пользователе, заданном его идентификатором. |
> | Action | Microsoft.ApiManagement/service/users/subscriptions/read | Перечисляет коллекцию подписок указанного пользователя. |
> | Action | Microsoft.ApiManagement/service/users/token/action | Возвращает маркер авторизации общего доступа для пользователя. |
> | Action | Microsoft.ApiManagement/service/users/write | Создает или обновляет пользователя. или обновляет сведения о пользователе, определяемые его идентификатором. |
> | Action | Microsoft.ApiManagement/service/write | Создание экземпляра службы управления API. |
> | Action | Microsoft.ApiManagement/unregister/action | Отменяет регистрацию подписки для поставщика ресурсов Microsoft.ApiManagement. |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Authorization/classicAdministrators/delete | Удаляет администратора из подписки. |
> | Action | Microsoft.Authorization/classicAdministrators/operationstatuses/read | Получает сведения о состоянии операции администратора подписки. |
> | Action | Microsoft.Authorization/classicAdministrators/read | Считывает администраторов подписки. |
> | Action | Microsoft.Authorization/classicAdministrators/write | Добавление или изменение администратора в подписке. |
> | Action | Microsoft.Authorization/denyAssignments/delete | Удаление запрета назначения в указанной области. |
> | Action | Microsoft.Authorization/denyAssignments/read | Получение сведений о запрете назначения. |
> | Action | Microsoft.Authorization/denyAssignments/write | Создание запрета назначения в указанной области. |
> | Action | Microsoft.Authorization/elevateAccess/action | Предоставляет вызывающему права администратора доступа пользователей в области клиента |
> | Action | Microsoft.Authorization/locks/delete | Удаление блокировок в указанной области. |
> | Action | Microsoft.Authorization/locks/read | Получает блокировки в указанной области. |
> | Action | Microsoft.Authorization/locks/write | Добавление блокировок в указанной области. |
> | Action | Microsoft.Authorization/operations/read | Получает список операций |
> | Action | Microsoft.Authorization/permissions/read | Перечисляет все разрешения, которые есть у вызывающего в указанной области. |
> | Action | Microsoft.Authorization/policyAssignments/delete | Удаление назначения политики в указанной области. |
> | Action | Microsoft.Authorization/policyAssignments/read | Получение сведений о назначении политики. |
> | Action | Microsoft.Authorization/policyAssignments/write | Создание назначения политики в указанной области. |
> | Action | Microsoft.Authorization/policyDefinitions/delete | Удаление определения политики. |
> | Action | Microsoft.Authorization/policyDefinitions/read | Получение сведений об определении политики. |
> | Action | Microsoft.Authorization/policyDefinitions/write | Создание пользовательского определения политики. |
> | Action | Microsoft.Authorization/policySetDefinitions/delete | Удаление определения набора политик. |
> | Action | Microsoft.Authorization/policySetDefinitions/read | Получить сведения об определении набора политик. |
> | Action | Microsoft.Authorization/policySetDefinitions/write | Создание пользовательского определения набора политик. |
> | Action | Microsoft.Authorization/providerOperations/read | Получение операций для всех поставщиков, которые могут использоваться в определениях ролей. |
> | Action | Microsoft.Authorization/roleAssignments/delete | Удаление назначения ролей в указанной области. |
> | Action | Microsoft.Authorization/roleAssignments/read | Получение информации о назначении роли. |
> | Action | Microsoft.Authorization/roleAssignments/write. | Создание назначения ролей в указанной области. |
> | Action | Microsoft.Authorization/roleDefinitions/delete | Удаление указанного определения пользовательской роли. |
> | Action | Microsoft.Authorization/roleDefinitions/read | Получение сведений об определении роли. |
> | Action | Microsoft.Authorization/roleDefinitions/write | Создание или изменение определения пользовательской роли с указанными разрешениями и назначаемыми областями. |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Automation/automationAccounts/agentRegistrationInformation/read | Чтение сведений о регистрации Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/agentRegistrationInformation/regenerateKey/action | Запись запроса на повторное создание ключей Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/certificates/delete | Удаляет ресурс сертификата службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/certificates/getCount/action | Считывает число сертификатов. |
> | Action | Microsoft.Automation/automationAccounts/certificates/read | Возвращает ресурс сертификата службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/certificates/write | Создает или обновляет ресурс сертификата службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/read | Чтение содержимого компиляции Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/read | Чтение содержимого компиляции Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/write | Запись содержимого компиляции Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/compilationjobs/write | Запись содержимого компиляции Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/configurations/content/read | Считывает мультимедийное содержимое конфигурации. |
> | Action | Microsoft.Automation/automationAccounts/configurations/delete | Удаление содержимого Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/configurations/getCount/action | Чтение количества содержимого Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/configurations/read | Возвращает содержимое Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/configurations/write | Запись содержимого Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/connections/delete | Удаляет ресурс подключения службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/connections/getCount/action | Считывает число подключений. |
> | Action | Microsoft.Automation/automationAccounts/connections/read | Возвращает ресурс подключения службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/connections/write | Создает или обновляет ресурс подключения службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/connectionTypes/delete | Удаляет ресурс типа подключения службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/connectionTypes/read | Возвращает ресурс типа подключения службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/connectionTypes/write | Создает ресурс типа подключения службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/credentials/delete | Удаляет ресурс учетных данных службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/credentials/getCount/action | Считывает количество учетных данных. |
> | Action | Microsoft.Automation/automationAccounts/credentials/read | Возвращает ресурс учетных данных службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/credentials/write | Создает или обновляет ресурс учетных данных службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/delete | Удаляет учетную запись службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/delete | Удаляет ресурсы гибридной рабочей роли Runbook. |
> | Action | Microsoft.Automation/automationAccounts/hybridRunbookWorkerGroups/read | Считывает ресурсы гибридной рабочей роли Runbook. |
> | Action | Microsoft.Automation/automationAccounts/jobs/output/read | Возвращает выходные данные задания. |
> | Action | Microsoft.Automation/automationAccounts/jobs/read | Возвращает задание службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/jobs/resume/action | Возобновляет выполнение задания службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/jobs/runbookContent/action | Возвращает содержимое runbook службы автоматизации Azure во время выполнения задания. |
> | Action | Microsoft.Automation/automationAccounts/jobs/stop/action | Останавливает задание службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/jobs/streams/read | Возвращает поток задания службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/jobs/streams/read | Возвращает поток задания службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/jobs/suspend/action | Приостанавливает задание службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/jobs/write | Создает задание службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/jobSchedules/delete | Удаляет расписание заданий службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/jobSchedules/read | Возвращает расписание заданий службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/jobSchedules/write | Создает расписание заданий службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/linkedWorkspace/read | Получает рабочую область, связанную с учетной записью службы автоматизации. |
> | Action | Microsoft.Automation/automationAccounts/listKeys/action | Считывает ключи для учетной записи службы автоматизации. |
> | Action | Microsoft.Automation/automationAccounts/modules/activities/read | Получение действий службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/modules/delete | Удаляет модуль PowerShell службы автоматизации Azure |
> | Action | Microsoft.Automation/automationAccounts/modules/getCount/action | Возвращает число модулей PowerShell в рамках учетной записи службы автоматизации |
> | Action | Microsoft.Automation/automationAccounts/modules/read | Получает модуль PowerShell службы автоматизации Azure |
> | Action | Microsoft.Automation/automationAccounts/modules/write | Создает или обновляет модуль PowerShell службы автоматизации Azure |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/delete | Удаление конфигурации узла Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/rawContent/action | Чтение содержимого конфигурации узла Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/read | Чтение конфигурации узла Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/nodeConfigurations/write | Запись конфигурации узла Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/nodecounts/read | Считывает общее число узлов указанного типа. |
> | Action | Microsoft.Automation/automationAccounts/nodes/delete | Удаление узлов Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/nodes/read | Чтение узлов Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/nodes/reports/content/read | Считывает содержимое отчета Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/nodes/reports/read | Чтение отчетов Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/nodes/write | Создает или обновляет узлы Azure Automation DSC. |
> | Action | Microsoft.Automation/automationAccounts/objectDataTypes/fields/read | Получение полей типов службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/python2Packages/delete | Удаляет пакет Python 2 службы автоматизации Azure |
> | Action | Microsoft.Automation/automationAccounts/python2Packages/read | Получает пакет Python 2 службы автоматизации Azure |
> | Action | Microsoft.Automation/automationAccounts/python2Packages/write | Создает или обновляет пакет Python 2 службы автоматизации Azure |
> | Action | Microsoft.Automation/automationAccounts/python3Packages/delete | Удаляет пакет Python 3 службы автоматизации Azure |
> | Action | Microsoft.Automation/automationAccounts/python3Packages/read | Получает пакет Python 3 службы автоматизации Azure |
> | Action | Microsoft.Automation/automationAccounts/python3Packages/write | Создает или обновляет пакет Python 3 службы автоматизации Azure |
> | Action | Microsoft.Automation/automationAccounts/read | Возвращает учетную запись службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/content/read | Возвращает содержимое runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/delete | Удаляет runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/content/write | Создает содержимое черновика runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/operationResults/read | Возвращает результаты операции черновика runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/read | Возвращает черновик runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/read | Возвращает тестовое задание черновика runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/resume/action | Возобновляет выполнение тестового задания черновика runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/stop/action | Останавливает тестовое задание черновика runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/suspend/action | Приостанавливает тестовое задание черновика runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/testJob/write | Создает тестовое задание черновика runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/draft/undoEdit/action | Отменяет правки в черновике runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/getCount/action | Возвращает число модулей runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/publish/action | Публикует черновик runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/read | Возвращает runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/runbooks/write | Создает или обновляет runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/schedules/delete | Удаляет ресурс расписания службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/schedules/getCount/action | Возвращает количество расписаний службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/schedules/read | Возвращает ресурс расписания службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/schedules/write | Создает или обновляет ресурс расписания службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/delete | Удаляет конфигурацию обновления программного обеспечения Azure Automation |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/read | Получает конфигурации обновления программного обеспечения Azure Automation |
> | Action | Microsoft.Automation/automationAccounts/softwareUpdateConfigurations/write | Создает или обновляет конфигурацию обновлений программного обеспечения Azure Automation |
> | Action | Microsoft.Automation/automationAccounts/statistics/read | Получение статистики из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/updateDeploymentMachineRuns/read | Получает компьютер для развертывания обновления службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/updateManagementPatchJob/read | Получает задание исправления управления обновлениями для службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/usages/read | Получение данных использования из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/variables/delete | Удаляет ресурс переменной службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/variables/read | Считывает ресурс переменной службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/variables/write | Создает или обновляет ресурс переменной службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/watchers/delete | Удаляет задание наблюдателя из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/watchers/read | Получает задание наблюдателя из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/watchers/start/action | Запускает задание наблюдателя из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/watchers/stop/action | Останавливает выполнение заданий наблюдателя из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/watchers/streams/read | Получение потока заданий наблюдателя из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/watchers/watcherActions/delete | Удаляет действия заданий наблюдателя из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/watchers/watcherActions/read | Получает действия заданий наблюдателя из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/watchers/watcherActions/write | Создает действия заданий наблюдателя из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/watchers/write | Создает задание наблюдателя из службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/webhooks/action | Создает универсальный код ресурса (URI) для webhook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/webhooks/delete | Удаляет webhook службы автоматизации Azure.  |
> | Action | Microsoft.Automation/automationAccounts/webhooks/read | Считывает runbook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/webhooks/write | Создает или обновляет webhook службы автоматизации Azure. |
> | Action | Microsoft.Automation/automationAccounts/write | Создает или обновляет учетную запись службы автоматизации Azure. |
> | Action | Microsoft.Automation/operations/read | Получает доступные операции для ресурсов службы автоматизации Azure. |
> | Action | Microsoft.Automation/register/action | Регистрирует подписку в службе автоматизации Azure. |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.AzureActiveDirectory/b2cDirectories/delete | Удаление ресурса каталога B2C |
> | Action | Microsoft.AzureActiveDirectory/b2cDirectories/read | Просмотреть ресурс каталога B2C |
> | Action | Microsoft.AzureActiveDirectory/b2cDirectories/write | Создание или обновление ресурса каталога B2C. |
> | Action | Microsoft. AzureActiveDirectory/b2ctenants/Read | Отображает список всех клиентов B2C, членом которых является пользователь. |
> | Action | Microsoft.AzureActiveDirectory/operations/read | Читать все операции API, доступные для поставщика ресурсов Microsoft.AzureActiveDirectory |
> | Action | Microsoft.AzureActiveDirectory/register/action | Зарегистрировать подписку для поставщика ресурсов Microsoft.AzureActiveDirectory |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.AzureStack/Operations/read | Получение свойств операции поставщика ресурсов. |
> | Action | Microsoft.AzureStack/register/action | Регистрация подписки в поставщике ресурсов Microsoft.AzureStack. |
> | Action | Microsoft.AzureStack/registrations/customerSubscriptions/delete | Удаление подписки клиента Azure Stack. |
> | Action | Microsoft.AzureStack/registrations/customerSubscriptions/read | Получение свойств подписки клиента Azure Stack. |
> | Action | Microsoft.AzureStack/registrations/customerSubscriptions/write | Создание или обновление подписки клиента Azure Stack. |
> | Action | Microsoft.AzureStack/registrations/delete | Удаление регистрации в Azure Stack. |
> | Action | Microsoft.AzureStack/registrations/getActivationKey/action | Получение последнего созданного ключа активации Azure Stack. |
> | Action | Microsoft.AzureStack/registrations/products/listDetails/action | Возвращает подробные сведения о продукте из Azure Stack Marketplace. |
> | Action | Microsoft.AzureStack/registrations/products/read | Получает свойства продукта из Azure Stack Marketplace. |
> | Action | Microsoft.AzureStack/registrations/read | Получает свойства регистрации Azure Stack. |
> | Action | Microsoft.AzureStack/registrations/write | Создание или обновление регистрации в Azure Stack. |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Batch/batchAccounts/applications/delete | Удаляет приложение. |
> | Action | Microsoft.Batch/batchAccounts/applications/read | Выводит список приложений или возвращает свойства приложения. |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/activate/action | Активирует пакет приложения. |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/delete | Удаляет пакет приложения. |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/read | Возвращает свойства пакета приложения. |
> | Action | Microsoft.Batch/batchAccounts/applications/versions/write | Создает новый пакет приложения или обновляет существующий. |
> | Action | Microsoft.Batch/batchAccounts/applications/write | Создает новое приложение или обновляет существующее. |
> | Action | Microsoft.Batch/batchAccounts/certificateOperationResults/read | Получение результатов длительных операций с сертификатом в учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/certificates/cancelDelete/action | Отмена неудачного удаления сертификата в учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/certificates/delete | Удаление сертификата из учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/certificates/read | Вывод списка сертификатов в учетной записи пакетной службы или получение свойств сертификата. |
> | Action | Microsoft.Batch/batchAccounts/certificates/write | Создание нового сертификата в учетной записи пакетной службы или обновление существующего сертификата. |
> | Action | Microsoft.Batch/batchAccounts/delete | Удаляет учетную запись пакетной службы. |
> | DataAction | Microsoft. Batch/Батчаккаунтс/задания/Удаление | Удаляет задание из учетной записи пакетной службы. |
> | DataAction | Microsoft. Batch/Батчаккаунтс/задания/чтение | Выводит список заданий в учетной записи пакетной службы или получает свойства задания. |
> | DataAction | Microsoft. Batch/Батчаккаунтс/задания/запись | Создает новое задание в учетной записи пакетной службы или обновляет существующее задание. |
> | DataAction | Microsoft. Batch/Батчаккаунтс/Жобсчедулес/удаление | Удаление расписания задания из учетной записи пакетной службы |
> | DataAction | Microsoft. Batch/Батчаккаунтс/Жобсчедулес/чтение | Выводит список расписаний заданий в учетной записи пакетной службы или получает свойства расписания задания. |
> | DataAction | Microsoft. Batch/Батчаккаунтс/Жобсчедулес/запись | Создает новое расписание заданий в учетной записи пакетной службы или обновляет существующее расписание заданий. |
> | Action | Microsoft.Batch/batchAccounts/listkeys/action | Отображает ключи доступа для учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/operationResults/read | Получение результатов длительных операций в учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/poolOperationResults/read | Получение результатов длительных операций с пулом в учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/pools/delete | Удаление пула из учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/pools/disableAutoscale/action | Отключение автоматического масштабирования в пуле учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/pools/read | Вывод списка пулов в учетной записи пакетной службы или свойств пула. |
> | Action | Microsoft.Batch/batchAccounts/pools/stopResize/action | Остановка текущей операции изменения размера в пуле учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/pools/write | Создание нового пула в учетной записи пакетной службы или обновление существующего пула. |
> | Action | Microsoft.Batch/batchAccounts/read | Выводит список учетных записей пакетной службы или возвращает свойства учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/regeneratekeys/action | Повторно создает ключи доступа для учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/syncAutoStorageKeys/action | Синхронизирует ключи доступа для автоматической учетной записи хранения, настроенной для учетной записи пакетной службы. |
> | Action | Microsoft.Batch/batchAccounts/write | Создает новую учетную запись пакетной службы или обновляет существующую. |
> | Action | Microsoft.Batch/locations/accountOperationResults/read | Получение результатов длительных операций в учетной записи пакетной службы. |
> | Action | Microsoft.Batch/locations/checkNameAvailability/action | Проверка того, является ли имя учетной записи допустимым и неиспользуемым. |
> | Action | Microsoft.Batch/locations/quotas/read | Возвращает квоты пакетной службы для указанной подписки в заданном регионе Azure. |
> | Action | Microsoft.Batch/operations/read | Перечисляет операции, доступные в поставщике ресурсов Microsoft.Batch. |
> | Action | Microsoft.Batch/register/action | Регистрирует подписку для поставщика ресурсов пакетной службы и позволяет создавать учетные записи пакетной службы. |
> | Action | Microsoft.Batch/unregister/action | Отмена регистрации подписки для поставщика ресурсов пакетной службы, которая не позволяет создавать учетные записи пакетной службы. |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Billing/billingAccounts/departments/read | Вывод списка всех отделов, относящихся к учетной записи выставления счетов. |
> | Action | Microsoft.Billing/billingAccounts/enrollmentAccounts/read | Вывод списка всех учетных записей регистрации, относящихся к учетной записи выставления счетов. |
> | Action | Microsoft.Billing/billingAccounts/read | Вывод списка всех учетных записей выставления счетов, к которым пользователь имеет доступ. |
> | Action | Microsoft.Billing/billingPeriods/read | Вывод списка доступных интервалов выставления счетов. |
> | Action | Microsoft.Billing/departments/read | Вывод списка всех отделов, к которым пользователь имеет доступ. |
> | Action | Microsoft.Billing/invoices/read | Выводит список доступных счетов. |
> | Action | Microsoft.Billing/register/action | Регистрация подписки в поставщике ресурсов Microsoft.Billing. |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.BingMaps/mapApis/Delete | Операция удаления |
> | Action | Microsoft.BingMaps/mapApis/listSecrets/action | Вывод списка секретов |
> | Action | Microsoft.BingMaps/mapApis/listSingleSignOnToken/action | Чтение маркера авторизации единого входа для ресурса |
> | Action | Microsoft.BingMaps/mapApis/Read | Операция чтения |
> | Action | Microsoft.BingMaps/mapApis/regenerateKey/action | Повторно создает ключ |
> | Action | Microsoft.BingMaps/mapApis/Write | Операция записи |
> | Action | Microsoft.BingMaps/Operations/read | Описание операции. |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Блокчейн/Блоккчаинмемберс/Delete | Удаляет существующий элемент Блокчейн. |
> | Action | Microsoft. Блокчейн/Блоккчаинмемберс/Листапикэйс/Action | Возвращает или перечисляет существующие ключи API-элементов Блокчейн. |
> | Action | Microsoft. Блокчейн/Блоккчаинмемберс/Read | Возвращает или перечисляет существующие элементы Блокчейн. |
> | DataAction | Microsoft. Блокчейн/Блоккчаинмемберс/Трансактионнодес/Connect/Action | Подключается к узлу транзакции-члена Блокчейн. |
> | Action | Microsoft. Блокчейн/Блоккчаинмемберс/Трансактионнодес/Delete | Удаляет существующий узел транзакции элемента Блокчейн. |
> | Action | Microsoft. Блокчейн/Блоккчаинмемберс/Трансактионнодес/Листапикэйс/действие | Возвращает или перечисляет существующие ключи API узла транзакции Блокчейн-члена. |
> | Action | Microsoft. Блокчейн/Блоккчаинмемберс/Трансактионнодес/Read | Возвращает или перечисляет существующие узлы транзакций с элементами Блокчейн. |
> | Action | Microsoft. Блокчейн/Блоккчаинмемберс/Трансактионнодес/запись | Создает или обновляет узел транзакции элемента Блокчейн. |
> | Action | Microsoft. Блокчейн/Блоккчаинмемберс/запись | Создает или обновляет элемент Блокчейн. |
> | Action | Microsoft. Блокчейн/Locations/Блоккчаинмембероператионресултс/Read | Возвращает результаты операции для элементов Блокчейн. |
> | Action | Microsoft. Блокчейн/Locations/Чеккнамеаваилабилити/Action | Проверяет, является ли имя ресурса допустимым и не используется. |
> | Action | Microsoft. Блокчейн/операции/чтение | Вывод списка всех операций в поставщике ресурсов Microsoft Блокчейн. |
> | Action | Microsoft. Блокчейн/Register/действие | Регистрирует подписку для поставщика ресурсов Блокчейн. |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Blueprint/blueprintAssignments/assignmentOperations/read | Чтение любых артефактов схемы. |
> | Action | Microsoft.Blueprint/blueprintAssignments/delete | Удаление любых артефактов схемы. |
> | Action | Microsoft.Blueprint/blueprintAssignments/read | Чтение любых артефактов схемы. |
> | Action | Microsoft. чертеж/Блуепринтассигнментс/вхоисблуепринт/действие | Получите идентификатор объекта субъекта-службы Azure Blueprints. |
> | Action | Microsoft.Blueprint/blueprintAssignments/write | Создание или изменение любых артефактов схемы. |
> | Action | Microsoft.Blueprint/blueprints/artifacts/delete | Удаление любых артефактов схемы. |
> | Action | Microsoft.Blueprint/blueprints/artifacts/read | Чтение любых артефактов схемы. |
> | Action | Microsoft.Blueprint/blueprints/artifacts/write | Создание или изменение любых артефактов схемы. |
> | Action | Microsoft.Blueprint/blueprints/delete | Удаление любых схем. |
> | Action | Microsoft.Blueprint/blueprints/read | Чтение любых схем. |
> | Action | Microsoft.Blueprint/blueprints/versions/artifacts/read | Чтение любых артефактов схемы. |
> | Action | Microsoft.Blueprint/blueprints/versions/delete | Удаление любых схем. |
> | Action | Microsoft.Blueprint/blueprints/versions/read | Чтение любых схем. |
> | Action | Microsoft.Blueprint/blueprints/versions/write | Создание или изменение любых схем. |
> | Action | Microsoft.Blueprint/blueprints/write | Создание или изменение любых схем. |
> | Action | Microsoft.Blueprint/register/action | Регистрация поставщика ресурсов Azure Blueprints |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.BotService/botServices/channels/delete | Удаление службы бота. |
> | Action | Microsoft.BotService/botServices/channels/read | Чтение службы бота. |
> | Action | Microsoft.BotService/botServices/channels/write | Запись службы бота. |
> | Action | Microsoft.BotService/botServices/connections/delete | Удаление службы бота. |
> | Action | Microsoft.BotService/botServices/connections/read | Чтение службы бота. |
> | Action | Microsoft.BotService/botServices/connections/write | Запись службы бота. |
> | Action | Microsoft.BotService/botServices/delete | Удаление службы бота. |
> | Action | Microsoft.BotService/botServices/read | Чтение службы бота. |
> | Action | Microsoft.BotService/botServices/write | Запись службы бота. |
> | Action | Microsoft.BotService/locations/operationresults/read | Чтение состояния асинхронной операции. |
> | Action | Microsoft.BotService/Operations/read | Чтение операций для всех типов ресурсов. |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Cache/checknameavailability/action | Проверяет, доступно ли название (для использования его для нового кэша Redis). |
> | Action | Microsoft.Cache/locations/operationresults/read | Получает результат выполнения длительной операции, для которой клиенту в прошлом был возвращен заголовок "Расположение" |
> | Action | Microsoft.Cache/operations/read | Список операций, которые поддерживает поставщик "Microsoft.Cache". |
> | Action | Microsoft.Cache/redis/delete | Удалить весь кэш Redis |
> | Action | Microsoft.Cache/redis/export/action | Экспортировать данные Redis в BLOB-объекты хранилища с префиксом в указанном формате |
> | Action | Microsoft.Cache/redis/firewallRules/delete | Удаляет правила брандмауэра для IP-адресов кэша Redis. |
> | Action | Microsoft.Cache/redis/firewallRules/read | Получает правила брандмауэра для IP-адресов кэша Redis. |
> | Action | Microsoft.Cache/redis/firewallRules/write | Изменяет правила брандмауэра для IP-адресов кэша Redis. |
> | Action | Microsoft.Cache/redis/forceReboot/action | Принудительно перезагрузить экземпляр кэша с возможной потерей данных. |
> | Action | Microsoft.Cache/redis/import/action | Импортировать данные в указанном формате из нескольких BLOB-объектов в Redis |
> | Action | Microsoft.Cache/redis/linkedservers/delete | Удалить связанный сервер из кэша Redis |
> | Action | Microsoft.Cache/redis/linkedservers/read | Получить серверы, связанные с кэшем Redis. |
> | Action | Microsoft.Cache/redis/linkedservers/write | Добавить связанный сервер в кэш Redis |
> | Action | Microsoft.Cache/redis/listKeys/action | Просмотр значений ключей доступа к кэшу Redis на портале управления |
> | Action | Microsoft.Cache/redis/listUpgradeNotifications/read | Перечисление последних уведомлений об обновлениях для клиента кэша. |
> | Action | Microsoft.Cache/redis/metricDefinitions/read | Получает доступные метрики для кэша Redis |
> | Action | Microsoft.Cache/redis/patchSchedules/delete | Удаляет расписание установки исправлений для кэша Redis. |
> | Action | Microsoft.Cache/redis/patchSchedules/read | Получает расписание установки исправлений для кэша Redis |
> | Action | Microsoft.Cache/redis/patchSchedules/write | Изменяет расписание установки исправлений для кэша Redis |
> | Action | Microsoft.Cache/redis/read | Просмотр параметров и конфигурации кэша Redis на портале управления |
> | Action | Microsoft.Cache/redis/regenerateKey/action | Изменить значение ключей доступа к кэшу Redis на портале управления |
> | Action | Microsoft.Cache/redis/start/action | Запуск экземпляра кэша. |
> | Action | Microsoft.Cache/redis/stop/action | Остановка экземпляра кэша. |
> | Action | Microsoft.Cache/redis/write | Изменение параметров и конфигурации кэша Redis на портале управления |
> | Action | Microsoft.Cache/register/action | Регистрирует поставщик ресурсов Microsoft.Cache в подписке. |
> | Action | Microsoft.Cache/unregister/action | Отменяет регистрацию поставщика ресурсов Microsoft.Cache в подписке. |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Capacity/appliedreservations/read | Чтение всех резервирований. |
> | Action | Microsoft. Capacity/калкулатиксчанже/действие | Вычисление суммы и цены обмена для новой покупки и возвращение ошибок политики. |
> | Action | Microsoft.Capacity/calculateprice/action | Вычисление цены резервирования. |
> | Action | Microsoft.Capacity/catalogs/read | Чтение каталога резервирования. |
> | Action | Microsoft.Capacity/checkoffers/action | Проверка наличия предложений для подписок. |
> | Action | Microsoft.Capacity/checkscopes/action | Проверка наличия подписок. |
> | Action | Microsoft.Capacity/commercialreservationorders/read | Получение заказов на резервирование, созданных в любом клиенте. |
> | Action | Microsoft. Capacity/Exchange/действие | Обмен любыми резервированиями |
> | Action | Microsoft.Capacity/operations/read | Чтение любой операции. |
> | Action | Microsoft.Capacity/register/action | Регистрация поставщика ресурсов емкости и разрешение создания ресурсов емкости. |
> | Action | Microsoft.Capacity/reservationorders/action | Обновление резервирования. |
> | Action | Microsoft.Capacity/reservationorders/availablescopes/action | Поиск любой доступной области. |
> | Action | Microsoft. Capacity/ресерватионордерс/калкулатерефунд/действие | Вычисление суммы возврата и цены на новую покупку и возвращение ошибок политики. |
> | Action | Microsoft.Capacity/reservationorders/delete | Удаление резервирования. |
> | Action | Microsoft.Capacity/reservationorders/merge/action | Объединение любого резервирования. |
> | Action | Microsoft.Capacity/reservationorders/read | Чтение всех резервирований. |
> | Action | Microsoft.Capacity/reservationorders/reservations/action | Обновление резервирования. |
> | Action | Microsoft.Capacity/reservationorders/reservations/delete | Удаление резервирования. |
> | Action | Microsoft.Capacity/reservationorders/reservations/read | Чтение всех резервирований. |
> | Action | Microsoft.Capacity/reservationorders/reservations/revisions/read | Чтение всех резервирований. |
> | Action | Microsoft.Capacity/reservationorders/reservations/write | Создание резервирования. |
> | Action | Microsoft.Capacity/reservationorders/return/action | Возвращение любого резервирования. |
> | Action | Microsoft.Capacity/reservationorders/split/action | Разделение любого резервирования. |
> | Action | Microsoft.Capacity/reservationorders/swap/action | Переключение любого резервирования. |
> | Action | Microsoft.Capacity/reservationorders/write | Создание резервирования. |
> | Action | Microsoft.Capacity/tenants/register/action | Регистрация любого клиента. |
> | Action | Microsoft.Capacity/unregister/action | Отмена регистрации любого клиента. |
> | Action | Microsoft.Capacity/validatereservationorder/action | Проверка любого резервирования. |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Cdn/CheckNameAvailability/action |  |
> | Action | Microsoft.Cdn/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/edgenodes/delete |  |
> | Action | Microsoft.Cdn/edgenodes/read |  |
> | Action | Microsoft.Cdn/edgenodes/write |  |
> | Action | Microsoft.Cdn/operationresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/DisableCustomHttps/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/EnableCustomHttps/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/customdomainresults/write |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Load/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/delete |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/originresults/write |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Purge/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Start/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/Stop/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/ValidateCustomDomain/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/endpointresults/write |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/GenerateSsoUri/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/GetSupportedOptimizationTypes/action |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/read |  |
> | Action | Microsoft.Cdn/operationresults/profileresults/write |  |
> | Action | Microsoft.Cdn/operationresults/read |  |
> | Action | Microsoft.Cdn/operationresults/write |  |
> | Action | Microsoft.Cdn/operations/read |  |
> | Action | Microsoft.Cdn/profiles/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/profiles/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/CheckResourceUsage/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/DisableCustomHttps/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/EnableCustomHttps/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/read |  |
> | Action | Microsoft.Cdn/profiles/endpoints/customdomains/write |  |
> | Action | Microsoft.Cdn/profiles/endpoints/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Load/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/origins/delete |  |
> | Action | Microsoft.Cdn/profiles/endpoints/origins/read |  |
> | Action | Microsoft.Cdn/profiles/endpoints/origins/write |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Purge/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/read |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Start/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/Stop/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/ValidateCustomDomain/action |  |
> | Action | Microsoft.Cdn/profiles/endpoints/write |  |
> | Action | Microsoft.Cdn/profiles/GenerateSsoUri/action |  |
> | Action | Microsoft.Cdn/profiles/GetSupportedOptimizationTypes/action |  |
> | Action | Microsoft.Cdn/profiles/read |  |
> | Action | Microsoft.Cdn/profiles/write |  |
> | Action | Microsoft.Cdn/register/action | Регистрирует подписку для поставщика ресурсов CDN и разрешает создание профилей CDN. |
> | Action | Microsoft.Cdn/ValidateProbe/action |  |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.CertificateRegistration/certificateOrders/certificates/Delete | Удаляет существующий сертификат. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/certificates/Read | Возвращает список сертификатов. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/certificates/Write | Добавляет новый сертификат или обновляет существующий. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/Delete | Удаляет существующий сертификат службы приложений. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/Read | Возвращает список заказов сертификатов. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/reissue/Action | Повторно отправляет существующий заказ сертификата. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/renew/Action | Обновляет существующий заказ сертификата. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/resendEmail/Action | Повторно отправляет сообщение сертификата. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Повторно отправляет сообщения запроса на другой электронный адрес. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/resendRequestEmails/Action | Извлекает печать сайта для выданного сертификата службы приложений. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/retrieveCertificateActions/Action | Извлекает список действий сертификата. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/retrieveEmailHistory/Action | Извлекает журнал электронных сообщений для сертификата. |
> | Action | Microsoft.CertificateRegistration/certificateOrders/verifyDomainOwnership/Action | Проверить права на владение доменом |
> | Action | Microsoft.CertificateRegistration/certificateOrders/Write | Добавляет новый заказ сертификата или обновляет существующий. |
> | Action | Microsoft.CertificateRegistration/operations/Read | Вывод списка всех операций из регистрации сертификата службы приложений. |
> | Action | Microsoft.CertificateRegistration/provisionGlobalAppServicePrincipalInUserTenant/Action | Подготавливает субъект-службу для субъекта приложения-службы. |
> | Action | Microsoft.CertificateRegistration/register/action | Регистрирует поставщик сертификатов Майкрософт для подписки. |
> | Action | Microsoft.CertificateRegistration/validateCertificateRegistrationInformation/Action | Проверяет объект покупки сертификата без его отправки. |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ClassicCompute/capabilities/read | Показывает возможности |
> | Action | Microsoft.ClassicCompute/checkDomainNameAvailability/action | Проверяет доступность заданного доменного имени. |
> | Action | Microsoft.ClassicCompute/checkDomainNameAvailability/read | Получает сведения о доступности указанного доменного имени. |
> | Action | Microsoft.ClassicCompute/domainNames/active/write | Задает имя активного домена. |
> | Action | Microsoft.ClassicCompute/domainNames/availabilitySets/read | Отображение группы доступности для ресурса. |
> | Action | Microsoft.ClassicCompute/domainNames/capabilities/read | Показывает возможности доменного имени. |
> | Action | Microsoft.ClassicCompute/domainNames/delete | Удаление доменных имен для ресурсов. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/read | Показывает слоты развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/roles/read | Получение роли в слоте развертывания доменного имени |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/roles/roleinstances/read | Получение экземпляра роли для роли в слоте развертывания доменного имени |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/state/read | Получение состояния слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/state/write | Добавление состояния для слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/upgradedomain/read | Получение домена обновления для слота развертывания для доменного имени |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/upgradedomain/write | Обновление домена обновления для слота развертывания для доменного имени |
> | Action | Microsoft.ClassicCompute/domainNames/deploymentslots/write | Создает или обновляет развертывание. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/delete | Удаляет расширения доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/operationStatuses/read | Читает состояние операции для расширений доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/read | Возвращает расширения доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/extensions/write | Добавляет расширения доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/delete | Удаление нового внутреннего распределения нагрузки. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/operationStatuses/read | Читает состояние операции для внутренних подсистем балансировки нагрузки доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/read | Получает внутренние подсистемы балансировки нагрузки. |
> | Action | Microsoft.ClassicCompute/domainNames/internalLoadBalancers/write | Создает новое внутреннее распределение нагрузки. |
> | Action | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/operationStatuses/read | Читает состояние операции для наборов конечных точек с балансировкой нагрузки доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/read | Получение наборов конечных точек с балансировкой нагрузки. |
> | Action | Microsoft.ClassicCompute/domainNames/loadBalancedEndpointSets/write | Добавление наборов конечных точек с балансировкой нагрузки. |
> | Action | Microsoft.ClassicCompute/domainNames/operationstatuses/read | Получение состояния операции доменного имени. |
> | Action | Microsoft.ClassicCompute/domainNames/operationStatuses/read | Читает состояние операции для расширений доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/read | Возвращение доменных имен для ресурсов. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/delete | Удаление использованных сертификатов службы. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/operationStatuses/read | Читает состояние операции для сертификатов службы доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/read | Возвращает использованные сертификаты службы. |
> | Action | Microsoft.ClassicCompute/domainNames/serviceCertificates/write | Добавление или изменение использованных сертификатов службы. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/abortMigration/action | Прерывает миграцию слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/commitMigration/action | Фиксирует миграцию слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/delete | Удаляет заданный слот развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/operationStatuses/read | Читает состояние операции для слотов доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/prepareMigration/action | Подготавливает миграцию слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/read | Показывает слоты развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/delete | Удаляет ссылку на расширение для роли слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/operationStatuses/read | Читает состояние операции для ссылок на расширение ролей слотов доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/read | Возвращает ссылку на расширение для роли слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/extensionReferences/write | Добавляет или изменяет ссылку на расширение для роли слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/metricdefinitions/read | Получение определения метрики роли для доменного имени. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/metrics/read | Получение метрики роли для доменного имени. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/operationstatuses/read | Получение состояния операции для роли слота доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметров диагностики. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/diagnosticSettings/write | Добавление или изменение параметров диагностики. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/providers/Microsoft.Insights/metricDefinitions/read | Получает определения метрик. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/read | Получение роли для слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/downloadremotedesktopconnectionfile/action | Скачивает файл подключения к удаленному рабочему столу для экземпляра роли в роли слота доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/operationStatuses/read | Получает состояние операции для экземпляра роли в роли слота доменных имен. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/read | Получение экземпляра роли. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/rebuild/action | Повторяет сборку экземпляра роли. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/reimage/action | Пересоздает образ экземпляра роли. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/roleInstances/restart/action | Перезапускает экземпляры роли. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/skus/read | Получение номера SKU роли для слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/roles/write | Добавление роли для слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/start/action | Запускает слот развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/state/start/write | Изменяет состояние слота развертывания на «Остановлен». |
> | Action | Microsoft.ClassicCompute/domainNames/slots/state/stop/write | Изменяет состояние слота развертывания на «Запущен». |
> | Action | Microsoft.ClassicCompute/domainNames/slots/stop/action | Приостанавливает слот развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/upgradeDomain/write | Обновление отдельного домена. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/validateMigration/action | Проверяет миграцию слота развертывания. |
> | Action | Microsoft.ClassicCompute/domainNames/slots/write | Создает или обновляет развертывание. |
> | Action | Microsoft.ClassicCompute/domainNames/swap/action | Переключает промежуточный слот на рабочий слот. |
> | Action | Microsoft.ClassicCompute/domainNames/write | Добавление или изменение доменных имен для ресурсов. |
> | Action | Microsoft.ClassicCompute/moveSubscriptionResources/action | Перемещение всех классических ресурсов в другую подписку. |
> | Action | Microsoft.ClassicCompute/operatingSystemFamilies/read | Перечисляет семейства гостевых операционных систем, доступные в Microsoft Azure, а также версии операционных систем в каждом таком семействе. |
> | Action | Microsoft.ClassicCompute/operatingSystems/read | Перечисляет версии гостевых операционных систем, доступные сейчас в Microsoft Azure. |
> | Action | Microsoft.ClassicCompute/operations/read | Получает список операций. |
> | Action | Microsoft.ClassicCompute/operationStatuses/read | Считывает состояние операции для ресурса. |
> | Action | Microsoft.ClassicCompute/quotas/read | Получение квоты для подписки. |
> | Action | Microsoft.ClassicCompute/register/action | Регистрация в классических вычислительных ресурсах |
> | Action | Microsoft.ClassicCompute/resourceTypes/skus/read | Получение списка номеров SKU для поддерживаемых типов ресурсов. |
> | Action | Microsoft.ClassicCompute/validateSubscriptionMoveAvailability/action | Проверка доступности подписки для классического перемещения. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/delete | Удаление сетевой группы безопасности, связанной с виртуальной машиной. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/operationStatuses/read | Читает состояние операций для групп безопасности связанной сети виртуальных машин. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/read | Получение сетевой группы безопасности, связанной с виртуальной машиной. |
> | Action | Microsoft.ClassicCompute/virtualMachines/associatedNetworkSecurityGroups/write | Добавление сетевой группы безопасности, связанной с виртуальной машиной. |
> | Action | Microsoft.ClassicCompute/virtualMachines/asyncOperations/read | Получает возможные асинхронные операции |
> | Action | Microsoft.ClassicCompute/virtualMachines/attachDisk/action | Подключает диск данных к виртуальной машине. |
> | Action | Microsoft.ClassicCompute/virtualMachines/capture/action | Запись виртуальной машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/delete | Удаляет виртуальные машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/detachDisk/action | Отключает диск данных от виртуальной машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/diagnosticsettings/read | Получение параметров диагностики виртуальной машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/disks/read | Извлекает список дисков данных. |
> | Action | Microsoft.ClassicCompute/virtualMachines/downloadRemoteDesktopConnectionFile/action | Скачивает RDP-файл для виртуальной машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/extensions/operationStatuses/read | Читает состояние операции для расширений виртуальных машин. |
> | Action | Microsoft.ClassicCompute/virtualMachines/extensions/read | Получает расширение виртуальной машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/extensions/write | Отправляет расширение виртуальной машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/metricdefinitions/read | Получение определения метрики виртуальной машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/metrics/read | Получает метрики. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/delete | Удаление сетевой группы безопасности, связанной с сетевым интерфейсом. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/operationStatuses/read | Читает состояние операций для групп безопасности связанной сети виртуальных машин. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/read | Получение сетевой группы безопасности, связанной с сетевым интерфейсом. |
> | Action | Microsoft.ClassicCompute/virtualMachines/networkInterfaces/associatedNetworkSecurityGroups/write | Добавление сетевой группы безопасности, связанной с сетевым интерфейсом. |
> | Action | Microsoft.ClassicCompute/virtualMachines/operationStatuses/read | Читает состояние операции для виртуальных машин. |
> | Action | Microsoft.ClassicCompute/virtualMachines/performMaintenance/action | Проводит обслуживание виртуальной машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметров диагностики. |
> | Action | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/diagnosticSettings/write | Добавление или изменение параметров диагностики. |
> | Action | Microsoft.ClassicCompute/virtualMachines/providers/Microsoft.Insights/metricDefinitions/read | Получает определения метрик. |
> | Action | Microsoft.ClassicCompute/virtualMachines/read | Извлекает список виртуальных машин. |
> | Action | Microsoft.ClassicCompute/virtualMachines/redeploy/action | Повторно развертывает виртуальную машину. |
> | Action | Microsoft.ClassicCompute/virtualMachines/restart/action | Перезапускает виртуальные машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/shutdown/action | Завершает работу виртуальной машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/start/action | Запуск виртуальной машины. |
> | Action | Microsoft.ClassicCompute/virtualMachines/stop/action | Останавливает виртуальную машину. |
> | Action | Microsoft.ClassicCompute/virtualMachines/write | Добавление или изменение виртуальных машин. |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/operationstatuses/read | Получение состояния операции для перекрестных подключений ExpressRoute. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/delete | Удаление пиринга перекрестных подключений ExpressRoute. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/operationstatuses/read | Получение состояния операции для пиринга перекрестных подключений ExpressRoute. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/read | Получение пиринга перекрестных подключений ExpressRoute. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/peerings/write | Добавление пиринга перекрестных подключений ExpressRoute. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/read | Получение перекрестных подключений ExpressRoute. |
> | Action | Microsoft.ClassicNetwork/expressroutecrossconnections/write | Добавление перекрестных подключений ExpressRoute. |
> | Action | Microsoft.ClassicNetwork/gatewaySupportedDevices/read | Извлекает список поддерживаемых устройств. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/delete | Удаление сетевой группы безопасности. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/operationStatuses/read | Читает состояние операции для группы безопасности сети. |
> | Action | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/read | Получает параметры диагностики для групп безопасности сети |
> | Action | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметров диагностики для групп безопасности сети. Эта операция осуществляется при задействовании поставщика ресурсов аналитических сведений. |
> | Action | Microsoft.ClassicNetwork/networksecuritygroups/providers/Microsoft.Insights/logDefinitions/read | Получает события для группы безопасности сети |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/read | Получение сетевой группы безопасности. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/delete | Удаление правила безопасности. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/operationStatuses/read | Читает состояние операции для правил безопасности группы безопасности сети. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/read | Получение правила безопасности. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/securityRules/write | Получение и обновление правила безопасности. |
> | Action | Microsoft.ClassicNetwork/networkSecurityGroups/write | Добавление сетевой группы безопасности. |
> | Action | Microsoft.ClassicNetwork/operations/read | Получение классических сетевых операций. |
> | Action | Microsoft.ClassicNetwork/quotas/read | Получение квоты для подписки. |
> | Action | Microsoft.ClassicNetwork/register/action | Регистрация в классической сети |
> | Action | Microsoft.ClassicNetwork/reservedIps/delete | Удаление зарезервированного IP-адреса. |
> | Action | Microsoft.ClassicNetwork/reservedIps/join/action | Присоединение к зарезервированному IP-адресу |
> | Action | Microsoft.ClassicNetwork/reservedIps/link/action | Связь зарезервированного IP-адреса |
> | Action | Microsoft.ClassicNetwork/reservedIps/operationStatuses/read | Читает состояние операции для зарезервированных IP-адресов. |
> | Action | Microsoft.ClassicNetwork/reservedIps/read | Получает зарезервированные IP-адреса |
> | Action | Microsoft.ClassicNetwork/reservedIps/write | Добавление нового зарезервированного IP-адреса |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/abortMigration/action | Прерывает миграцию виртуальной сети |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/capabilities/read | Показывает возможности |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/checkIPAddressAvailability/action | Проверка доступности заданного IP-адреса в виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/commitMigration/action | Фиксирует миграцию виртуальной сети |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/delete | Удаляет виртуальную сеть. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/delete | Отменяет отзыв сертификата клиента. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/read | Чтение отозванных сертификатов клиентов. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRevokedCertificates/write | Отзывает сертификат клиента. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/delete | Удаляет сертификат клиента шлюза виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/download/action | Скачивает сертификат по отпечатку. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/listPackage/action | Перечисляет пакеты сертификатов шлюза виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/read | Поиск корневых сертификатов клиентов. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/clientRootCertificates/write | Передает новый корневой сертификат клиента. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/connect/action | Устанавливает подключение шлюза «сеть-сеть». |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/disconnect/action | Отключает подключение шлюза «сеть-сеть». |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/read | Извлекает список подключений. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/connections/test/action | Проверяет подключение шлюза «сеть-сеть». |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/delete | Удаляет шлюз виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDeviceConfigurationScript/action | Скачивает сценарий конфигурации устройства. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/downloadDiagnostics/action | Скачивает данные диагностики шлюза. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/listCircuitServiceKey/action | Извлекает ключ службы канала. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/listPackage/action | Выводит список для пакета шлюза виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/operationStatuses/read | Читает состояние операции для шлюзов виртуальных сетей. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/packages/read | Получает пакет шлюза виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/read | Получает шлюзы виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/startDiagnostics/action | Запускает диагностику шлюза виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/stopDiagnostics/action | Останавливает диагностику шлюза виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/gateways/write | Добавляет шлюз виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/join/action | Присоединение к виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/operationStatuses/read | Читает состояние операции для виртуальных сетей. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/peer/action | Соединяет виртуальную сеть с другой виртуальной сетью на одноранговой основе. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/prepareMigration/action | Готовит миграцию виртуальной сети |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/read | Получает виртуальную сеть. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/delete | Удаляет удаленный прокси-сервер пиринга виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/read | Получает удаленный прокси-сервер пиринга виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/remoteVirtualNetworkPeeringProxies/write | Добавляет или обновляет удаленный прокси-сервер пиринга виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/delete | Удаление сетевой группы безопасности, связанной с подсетью. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/operationStatuses/read | Считывает состояние операции группы безопасности сети, связанной с подсетью виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/read | Получение сетевой группы безопасности, связанной с подсетью. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/subnets/associatedNetworkSecurityGroups/write | Добавление сетевой группы безопасности, связанной с подсетью. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/validateMigration/action | Проверяет миграцию виртуальной сети |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/virtualNetworkPeerings/read | Получает пиринг виртуальной сети. |
> | Action | Microsoft.ClassicNetwork/virtualNetworks/write | Добавление новой виртуальной сети. |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ClassicStorage/capabilities/read | Показывает возможности |
> | Action | Microsoft.ClassicStorage/checkStorageAccountAvailability/action | Проверяет доступность учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/checkStorageAccountAvailability/read | Получение сведений о доступности учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/disks/read | Возврат диска учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/images/operationstatuses/read | Получает состояние для операции создания образа. |
> | Action | Microsoft.ClassicStorage/images/read | Возвращает образ. |
> | Action | Microsoft.ClassicStorage/operations/read | Получает классические операции хранилища |
> | Action | Microsoft.ClassicStorage/osImages/read | Возвращает образ операционной системы. |
> | Action | Microsoft.ClassicStorage/osPlatformImages/read | Получает образ платформы операционной системы. |
> | Action | Microsoft.ClassicStorage/publicImages/read | Получение образа общедоступной виртуальной машины. |
> | Action | Microsoft.ClassicStorage/quotas/read | Получение квоты для подписки. |
> | Action | Microsoft.ClassicStorage/register/action | Регистрация в классическом хранилище |
> | Action | Microsoft.ClassicStorage/storageAccounts/abortMigration/action | Прерывает миграцию учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/commitMigration/action | Фиксирует миграцию учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/delete | Удаление учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/delete | Удаление указанного диска учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/operationStatuses/read | Считывает состояние операции для ресурса. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/read | Возврат диска учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/disks/write | Добавление диска учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/images/delete | Удаляет заданный образ из учетной записи хранения. (Не рекомендуется к использованию. Следует использовать Microsoft.ClassicStorage/storageAccounts/vmImages.) |
> | Action | Microsoft.ClassicStorage/storageAccounts/images/operationstatuses/read | Возвращает состояние операции создания образа учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/images/read | Возвращает образ в учетной записи хранения. (Не рекомендуется к использованию. Следует использовать Microsoft.ClassicStorage/storageAccounts/vmImages.) |
> | Action | Microsoft.ClassicStorage/storageAccounts/listKeys/action | Выводит список ключей доступа для учетных записей хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/operationStatuses/read | Считывает состояние операции для ресурса. |
> | Action | Microsoft.ClassicStorage/storageAccounts/osImages/delete | Удаляет указанный образ операционной системы учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/osImages/read | Возвращает образ операционной системы учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/osImages/write | Добавляет образ операционной системы указанной учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/prepareMigration/action | Подготавливает миграцию учетной записи хранения. |
> | Action | Microsoft. Классикстораже/storageAccounts/providers/Microsoft. Insights/diagnosticSettings/Read | Получение параметров диагностики. |
> | Action | Microsoft. Классикстораже/storageAccounts/providers/Microsoft. Insights/diagnosticSettings/запись | Добавление или изменение параметров диагностики. |
> | Action | Microsoft. Классикстораже/storageAccounts/providers/Microsoft. Insights/metricDefinitions/Read | Получает определения метрик. |
> | Action | Microsoft.ClassicStorage/storageAccounts/read | Возвращение учетной записи хранения с заданной учетной записью. |
> | Action | Microsoft.ClassicStorage/storageAccounts/regenerateKey/action | Повторно создает ключи доступа для учетных записей хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/read | Получение параметров диагностики. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/diagnosticSettings/write | Добавление или изменение параметров диагностики. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/metricDefinitions/read | Получает определения метрик. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/metrics/read | Получает метрики. |
> | Action | Microsoft.ClassicStorage/storageAccounts/services/read | Получение доступных служб. |
> | Action | Microsoft.ClassicStorage/storageAccounts/validateMigration/action | Проверяет миграцию учетной записи хранения. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/delete | Удаляет указанный образ виртуальной машины. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/operationstatuses/read | Получает состояние операции создания для образа указанной виртуальной машины. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/read | Возвращает образ виртуальной машины. |
> | Action | Microsoft.ClassicStorage/storageAccounts/vmImages/write | Добавляет указанный образ виртуальной машины. |
> | Action | Microsoft.ClassicStorage/storageAccounts/write | Добавляет новую учетную запись хранения. |
> | Action | Microsoft.ClassicStorage/vmImages/read | Перечисляет образы виртуальных машин. |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/анализ/действие | Эта операция извлекает богатый набор визуальных функций на основе содержимого изображения.  |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/ареаофинтерест/Action | Эта операция возвращает ограничивающий прямоугольник вокруг наиболее важной области изображения. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/описание/действие | Эта операция создает описание образа на удобочитаемом языке с помощью полных предложений.<br> Описание основано на коллекции тегов содержимого, которые также возвращаются операцией.<br>Для каждого изображения может быть создано более одного описания.<br> Описания упорядочены по оценкам достоверности.<br>Все описания приведены на английском языке. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/обнаружение/действие | Эта операция выполняет обнаружение объектов для указанного образа.  |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/generatethumbnail/Action | Эта операция создает эскиз изображения с заданной пользователем шириной и высотой.<br> По умолчанию служба анализирует изображение, определяет нужный регион (окупаемость инвестиций) и создает координаты обрезки на основе рентабельности инвестиций.<br> Интеллектуальная обрезка помогает при указании пропорций, отличающемся от типа входного изображения. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/Models/Analyze/Action | Эта операция распознает содержимое в образе, применяя модель для конкретного домена.<br> Список моделей для конкретных доменов, которые поддерживаются API компьютерного зрения, можно получить с помощью запроса GET/Моделс.<br> В настоящее время API предоставляет следующие модели для конкретных доменов: знаменитостей, ориентиры. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/Models/Read | Эта операция возвращает список моделей домена, которые поддерживаются API компьютерного зрения.  В настоящее время API поддерживает следующие модели для конкретных доменов: распознаватель знаменитости, распознаватель ориентиров. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/OCR/Action | Оптическое распознавание символов (OCR) определяет текст в изображении и извлекает распознанные символы в поток символов, пригодный для использования компьютером.    |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/рекогнизетекст/Action | Используйте этот интерфейс для получения результата операции Распознавание текста. При использовании интерфейса Распознавание текста ответ содержит поле "Operation-Location". Поле "Operation-Location" содержит URL-адрес, который необходимо использовать для операции получения результата операции Get Распознавание текста. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/Tag/действие | Эта операция создает список слов или тегов, относящихся к содержимому предоставляемого изображения.<br>API компьютерного зрения может возвращать Теги на основе объектов, живых существ, пейзажей или действий, найденных в образах.<br>В отличие от категорий, теги не упорядочены в соответствии с иерархической системой классификации, но соответствуют содержимому изображения.<br>Теги могут содержать указания, чтобы избежать неоднозначности или предоставить контекст, например тег "целло" может сопровождаться указанием "музыкальное средство".<br>Все теги доступны на английском языке. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Компутервисион/текстоператионс/Read | Этот интерфейс используется для получения результата операции распознавания текста. URL-адрес этого интерфейса должен быть получен из поля <b>"Operation-Location"</b> , возвращенного из интерфейса распознавание текста. |
> | Action | Microsoft.CognitiveServices/accounts/delete | Удаление учетных записей API |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/обнаружение/действие | Обнаруживать человеческих лиц в изображении, возвращать прямоугольники лицом и при необходимости с Фацеидс, ориентирами и атрибутами. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, фацелистс/Delete | Удаляет указанный список лиц. Связанные изображения лицевой стороны в списке лиц также будут удалены. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, фацелистс/персистедфацес/Delete | Удаление грани из списка лиц с помощью указанных Фацелистид и Персиситедфацеид. Изображение, связанное с лицевой стороны, будет удалено. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/фацелистс/персистедфацес/запись | Добавление лица в указанный список лиц, до 1 000 лиц. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, фацелистс/Read | Получение Фацелистид списка лиц, имени, userData и лиц в списке лиц. Список лиц с именами Фацелистид, Name и userData. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, фацелистс/Write | Создайте пустой список лиц с заданными пользователем Фацелистид, именем и необязательным параметром userData. В список лиц, имеющих до 64, разрешены сведения об обновлении списка лиц, включая имя и userData. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, финдсимиларс/действие | FaceId, заданный лицевой стороной запроса, для поиска похожих сторон из faceId массива, списка лиц или большого списка лиц. faceId |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, группа или действие | Разделите потенциальные лица на группы на основе сходства лиц. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, Поиск/действие | "один ко многим" для поиска ближайших совпадений лица, отвечающего конкретным запросам, от группы людей или большой группы лиц. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжефацелистс/Delete | Удаление указанного большого списка лиц. Связанные изображения лицевой стороны в списке больших лиц также будут удалены. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжефацелистс/персистедфацес/Delete | Удаление лица из большого списка с помощью указанных Ларжефацелистид и Персиситедфацеид. Изображение, связанное с лицевой стороны, будет удалено. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжефацелистс/персистедфацес/Read | Получите сохраненное лицо в большом списке лиц, Ларжефацелистид и Персистедфацеид. Список лиц Персистедфацеид и userData в указанном большом списке лиц. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/ларжефацелистс/персистедфацес/запись | Добавление лица в указанный большой список лиц, до 1 000 000 лиц. Обновляет поле userData указанного лица в большом списке лиц по его Персистедфацеид. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжефацелистс/Read | Получите Ларжефацелистид списка больших лиц, имя, userData. Вывод списка больших лиц — сведения о Ларжефацелистид, Name и userData. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжефацелистс, обучение и действие | Отправьте задачу по обучению большого списка лиц. Обучение — это важный этап, который может использовать только обученный большой список лиц. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/ларжефацелистс/Train/Read | Чтобы проверить состояние обучения для большого списка лиц, завершено или по-прежнему. LargeFaceListное обучение — это асинхронная операция |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжефацелистс/Write | Создайте пустой список больших лиц с заданным пользователем Ларжефацелистид, именем и необязательным параметром userData. Обновление сведений о большом списке лиц, включая имя и userData. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжеперсонграупс/Delete | Удаление существующей большой группы людей с указанным Персонграупид. Сохраненные данные в большой группе людей будут удалены. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжеперсонграупс/persons/Action | Создание нового пользователя в указанной большой группе людей. Чтобы добавить лицо для этого человека, позвоните по адресу. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжеперсонграупс/persons/Delete | Удаление существующего пользователя из большой группы людей. Все сохраненные личные данные и изображения лиц в записи пользователя будут удалены. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/ларжеперсонграупс/persons/персистедфацес/Delete | Удаление лица из человека в большой группе людей. Данные о лицах и изображения, связанные с этой записью лица, также будут удалены. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/ларжеперсонграупс/persons/персистедфацес/Read | Получение сведений о лице. Сохраненное лицо Person задается его Ларжеперсонграупид, personId и Персистедфацеид. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/ларжеперсонграупс/persons/персистедфацес/Write | Добавьте изображение лица в крупную группу людей для идентификации или проверки личности. Для работы с изображением поля userData, сохраненного лицом Person. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжеперсонграупс/persons/Read | Получите имя пользователя и userData, а также сохраненное Фацеидс, представляющее зарегистрированное изображение лица. Вывод сведений обо всех лицах в указанной большой группе людей, включая personId, Name, userData и Персистедфацеидс. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжеперсонграупс/persons/Write | Обновление имени или userData пользователя. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжеперсонграупс/Read | Получите сведения о большой группе людей, включая ее имя и userData. Этот API возвращает большой список сведений о группе людей со всеми существующими Ларжепесонграупид, именем и userData группы больших людей. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжеперсонграупс, обучение и действие | Отправка задачи обучения большой группы людей. Обучение — это важный этап, который может использовать только обученная большая группа людей. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/ларжеперсонграупс/Train/Read | Для проверки состояния обучения больших групп людей завершено или по-прежнему. Ларжеперсонграупное обучение — это асинхронная операция |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, ларжеперсонграупс/Write | Создание большой группы людей с заданными пользователем Ларжеперсонграупид, именем и необязательным. Обновление имеющегося имени группы больших людей и userData. Свойства сохраняются без изменений, если они не находятся в тексте запроса. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, персонграупс/Delete | Удаление существующей группы лиц с указанным Персонграупид. Сохраненные данные в этой группе пользователей будут удалены. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, персонграупс/persons/Action | Создание нового пользователя в указанной группе лиц. Чтобы добавить лицо для этого человека, позвоните по адресу. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, персонграупс/persons/Delete | Удаление существующего пользователя из группы людей. Все сохраненные личные данные и изображения лиц в записи пользователя будут удалены. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/персонграупс/persons/персистедфацес/Delete | Удаление лица из человека в группе людей. Данные о лицах и изображения, связанные с этой записью лица, также будут удалены. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/персонграупс/persons/персистедфацес/Read | Получение сведений о лице. Сохраненное лицо Person задается его Персонграупид, personId и Персистедфацеид. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/персонграупс/persons/персистедфацес/Write | Добавление изображения лица к лицу в группу лиц для идентификации или проверки личности лица. Для работы с изображением нескольких обновлений поля userData пользователя, сохраненного лицом. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, персонграупс/persons/Read | Получите имя пользователя и userData, а также сохраненное Фацеидс, представляющее зарегистрированное изображение лица. Вывод сведений обо всех лицах в указанной группе людей, включая personId, Name, userData и Персистедфацеидс, зарегистрированных. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, персонграупс/persons/Write | Обновление имени или userData пользователя. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, персонграупс/Read | Получение имени группы людей и userData. Чтобы получить сведения о лице в этой Персонграуп, используйте список Песонграупид, имя и userData группы людей. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, персонграупс, обучение и действие | Отправка задачи обучения группе Person. Обучение — это важный этап, который может использовать только обученная группа лиц. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face/персонграупс/Train/Read | Чтобы проверить состояние обучения группы лиц завершено или по-прежнему текущее. Персонграупное обучение — это асинхронная операция, активируемая |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, персонграупс/Write | Создайте новую группу Person с указанными Персонграупид, именем и предоставленным пользователем userData. Обновите имя существующей группы людей и userData. Свойства сохраняются без изменений, если они не находятся в тексте запроса. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Face, проверка/действие | Проверьте, принадлежат ли два лица одному человеку или принадлежит ли одна из них пользователю. |
> | Action | Microsoft.CognitiveServices/accounts/listKeys/action | Список ключей |
> | DataAction | Microsoft. CognitiveServices/Accounts/LUIS/Predict/Action | Возвращает прогноз опубликованной конечной точки для данного запроса. |
> | Action | Microsoft.CognitiveServices/accounts/read | Считывание учетных записей API. |
> | Action | Microsoft.CognitiveServices/accounts/regenerateKey/action | Повторное создание ключа |
> | Action | Microsoft.CognitiveServices/accounts/skus/read | Считывание доступных номеров SKU для существующего ресурса. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Текстаналитикс/сущности/действие | API\"возвращает список известных сущностей и общие именованные сущности (пользователь\", \"расположение\", \"организация\" и т. д.) в определенном документе. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Текстаналитикс/фраз/Action | API возвращает список строк с обозначением ключевых тем разговора во входном тексте. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Текстаналитикс/Languages/Action | API возвращает определенный язык и оценку в баллах между 0 и 1. Оценки, близкие к 1, указывают 100 % достоверность, что определенный язык имеет значение true. Поддерживается всего 120 языков. |
> | DataAction | Microsoft. CognitiveServices/Accounts/Текстаналитикс/тональности/Action | API возвращает числовую оценку в диапазоне от 0 до 1.<br>Если оценка приближается к 1, то у текста позитивная тональность, а если к 0, то негативная.<br>Оценка 0,5 указывает на отсутствие тональности (например,<br>Инструкция фактоид). |
> | Action | Microsoft.CognitiveServices/accounts/usages/read | Получение данных об использовании квоты для существующего ресурса. |
> | Action | Microsoft.CognitiveServices/accounts/write | Запись учетных записей API. |
> | Action | Microsoft. CognitiveServices/Чеккдомаинаваилабилити/действие | Чтение доступных номеров SKU для подписки. |
> | Action | Microsoft.CognitiveServices/locations/checkSkuAvailability/action | Чтение доступных номеров SKU для подписки. |
> | Action | Microsoft. CognitiveServices/Locations/Делетевиртуалнетворкорсубнетс/Action | Уведомление от Microsoft. Network об удалении VirtualNetworks или подсетей. |
> | Action | Microsoft.CognitiveServices/Operations/read | Список всех доступных операций |
> | Action | Microsoft.CognitiveServices/register/action | Регистрирует подписку на Cognitive Services |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Commerce/RateCard/read | Возвращает данные предложения, метаданные ресурсов и единиц измерения и тарифы для заданной подписки. |
> | Action | Microsoft.Commerce/UsageAggregates/read | Извлекает данные о потреблении ресурсов Microsoft Azure по подписке. Результат содержит статистические данные об использовании, а также сведения о подписке и ресурсах за определенный диапазон времени. |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Compute/availabilitySets/delete | Удаление группы доступности |
> | Action | Microsoft.Compute/availabilitySets/read | Получение свойств группы доступности |
> | Action | Microsoft.Compute/availabilitySets/vmSizes/read | Получение списка доступных размеров для создания или обновления виртуальной машины в группе доступности |
> | Action | Microsoft.Compute/availabilitySets/write | Создание новой группы доступности или обновление существующей |
> | Action | Microsoft.Compute/disks/beginGetAccess/action | Получить универсальный код ресурса SAS диска для доступа к BLOB-объекту |
> | Action | Microsoft.Compute/disks/delete | Удаляет диск |
> | Action | Microsoft.Compute/disks/endGetAccess/action | Отменить универсальный код ресурса SAS диска |
> | Action | Microsoft.Compute/disks/read | Получить свойства диска |
> | Action | Microsoft.Compute/disks/write | Создает новый диск или обновляет существующий |
> | Action | Microsoft.Compute/galleries/delete | Удаляет коллекцию |
> | Action | Microsoft.Compute/galleries/images/delete | Удаляет образ коллекции |
> | Action | Microsoft.Compute/galleries/images/read | Получает свойства образа коллекции |
> | Action | Microsoft.Compute/galleries/images/versions/delete | Удаляет версию образа коллекции |
> | Action | Microsoft.Compute/galleries/images/versions/read | Получает свойства версии образа коллекции |
> | Action | Microsoft.Compute/galleries/images/versions/write | Создает версию образа коллекции или обновляет существующую |
> | Action | Microsoft.Compute/galleries/images/write | Создает образ коллекции или обновляет существующий |
> | Action | Microsoft.Compute/galleries/read | Получает свойства коллекции |
> | Action | Microsoft.Compute/galleries/write | Создает коллекцию или обновляет существующую |
> | Action | Microsoft. COMPUTE/Хостграупс/удаление | Удаляет группу узлов |
> | Action | Microsoft. COMPUTE/Хостграупс/чтение | Получение свойств группы узлов |
> | Action | Microsoft. COMPUTE/Хостграупс/запись | Создает новую группу узлов или обновляет существующую. |
> | Action | Microsoft. COMPUTE/hosts/Delete | Удаляет узел |
> | Action | Microsoft. COMPUTE/hosts/Read | Получение свойств узла |
> | Action | Microsoft. COMPUTE/hosts/Write | Создает новый узел или обновляет существующий. |
> | Action | Microsoft.Compute/images/delete | Удаляет образ. |
> | Action | Microsoft.Compute/images/read | Получение свойств образа |
> | Action | Microsoft.Compute/images/write | Создает новый образ или обновляет существующий. |
> | Action | Microsoft.Compute/locations/capsOperations/read | Получает состояние асинхронной операции Caps |
> | Action | Microsoft.Compute/locations/diskOperations/read | Получение состояния асинхронной операции диска. |
> | Action | Microsoft.Compute/locations/logAnalytics/getRequestRateByInterval/action | Создайте журналы, чтобы показать общее число запросов по интервалу времени, помогающее при диагностике регулирования. |
> | Action | Microsoft.Compute/locations/logAnalytics/getThrottledRequests/action | Создайте журналы, чтобы показать агрегаты регулируемых запросов, сгруппированные по имени ресурса, имени операции или примененной политике регулирования. |
> | Action | Microsoft.Compute/locations/operations/read | Получение состояния асинхронной операции |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/offers/read | Получить свойства предложения образа платформы |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/read | Получить свойства номера SKU образа платформы |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/offers/skus/versions/read | Получить свойства версии образа платформы |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/types/read | Получить свойства типа VMExtension |
> | Action | Microsoft.Compute/locations/publishers/artifacttypes/types/versions/read | Получить свойства версии VMExtension |
> | Action | Microsoft.Compute/locations/publishers/read | Получить свойства издателя |
> | Action | Microsoft.Compute/locations/runCommands/read | Перечисляет доступные выполняемые команды в расположении |
> | Action | Microsoft.Compute/locations/usages/read | Получение данных об ограничениях служб и текущих показателях использования для вычислительных ресурсов по подписке в расположении |
> | Action | Microsoft.Compute/locations/vmSizes/read | Составление списка доступных размеров виртуальных машин в расположении |
> | Action | Microsoft.Compute/operations/read | Получение списка операций, доступных в поставщике ресурсов Microsoft.Compute |
> | Action | Microsoft. COMPUTE/Проксимитиплацементграупс/удаление | Удаляет группу размещения близкого взаимодействия. |
> | Action | Microsoft. COMPUTE/Проксимитиплацементграупс/чтение | Получение свойств группы размещения близкого взаимодействия. |
> | Action | Microsoft. COMPUTE/Проксимитиплацементграупс/запись | Создает новую группу размещения близкого взаимодействия или обновляет существующую. |
> | Action | Microsoft.Compute/register/action | Регистрация подписки в поставщике ресурсов Microsoft.Compute |
> | Action | Microsoft.Compute/restorePointCollections/delete | Удаляет коллекцию точек восстановления и содержащиеся в ней точки восстановления |
> | Action | Microsoft.Compute/restorePointCollections/read | Получение свойств коллекции точек восстановления |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/delete | Удаляет точку восстановления |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/read | Получение свойств точки восстановления |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/retrieveSasUris/action | Получение свойств точки восстановления наряду с URI BLOB-объектов SAS |
> | Action | Microsoft.Compute/restorePointCollections/restorePoints/write | Создает точку восстановления |
> | Action | Microsoft.Compute/restorePointCollections/write | Создает новую коллекцию точек восстановления или изменяет существующую |
> | Action | Microsoft.Compute/sharedVMImages/delete | Удаляет SharedVMImage |
> | Action | Microsoft.Compute/sharedVMImages/read | Получить свойства SharedVMImage |
> | Action | Microsoft.Compute/sharedVMImages/versions/delete | Удалить SharedVMImageVersion |
> | Action | Microsoft.Compute/sharedVMImages/versions/read | Получить свойства SharedVMImageVersion |
> | Action | Microsoft.Compute/sharedVMImages/versions/replicate/action | Реплицировать SharedVMImageVersion в целевые регионы |
> | Action | Microsoft.Compute/sharedVMImages/versions/write | Создать новый объект SharedVMImageVersion или обновить существующий |
> | Action | Microsoft.Compute/sharedVMImages/write | Создает новый объект SharedVMImage или обновляет существующий |
> | Action | Microsoft.Compute/skus/read | Возвращает список номеров SKU Microsoft.Compute для вашей подписки |
> | Action | Microsoft.Compute/snapshots/beginGetAccess/action | Получение универсального кода ресурса (URI) SAS моментального снимка для доступа к большому двоичному объекту. |
> | Action | Microsoft.Compute/snapshots/delete | Удалить моментальный снимок |
> | Action | Microsoft.Compute/snapshots/endGetAccess/action | Отмена универсального кода ресурса (URI) SAS моментального снимка. |
> | Action | Microsoft.Compute/snapshots/read | Получить свойства моментального снимка |
> | Action | Microsoft.Compute/snapshots/write | Создать новый моментальный снимок или обновить существующий |
> | Action | Microsoft. COMPUTE, отмена регистрации или действие | Отмена регистрации подписки в поставщике ресурсов Microsoft.Compute |
> | Action | Microsoft.Compute/virtualMachines/capture/action | Создание снимка виртуальной машины путем копирования виртуальных жестких дисков, а также создание шаблона, который можно использовать для создания подобных виртуальных машин |
> | Action | Microsoft.Compute/virtualMachines/convertToManagedDisks/action | Преобразует BLOB-диски виртуальной машины в управляемые диски |
> | Action | Microsoft.Compute/virtualMachines/deallocate/action | Отключение питания виртуальной машины и освобождение вычислительных ресурсов |
> | Action | Microsoft.Compute/virtualMachines/delete | Удаление виртуальной машины |
> | Action | Microsoft.Compute/virtualMachines/extensions/delete | Удаление расширения виртуальной машины |
> | Action | Microsoft.Compute/virtualMachines/extensions/read | Получение свойств расширения виртуальной машины |
> | Action | Microsoft.Compute/virtualMachines/extensions/write | Создание нового расширения виртуальной машины или обновление существующего |
> | Action | Microsoft.Compute/virtualMachines/generalize/action | Перевод виртуальной машины в состояние "Универсальная" и подготовка к созданию ее снимка |
> | Action | Microsoft.Compute/virtualMachines/instanceView/read | Получение подробного состояния среды выполнения виртуальной машины и ее ресурсов |
> | DataAction | Microsoft.Compute/virtualMachines/login/action | Вход в систему на виртуальной машине в качестве обычного пользователя |
> | DataAction | Microsoft.Compute/virtualMachines/loginAsAdmin/action | Войдите в систему на виртуальной машине с правами администратора Windows или привилегированного пользователя Linux |
> | Action | Microsoft.Compute/virtualMachines/performMaintenance/action | Обслуживает виртуальную машину. |
> | Action | Microsoft.Compute/virtualMachines/powerOff/action | Завершает работу виртуальной машины. Обратите внимание, что плата за виртуальную машину по-прежнему будет взиматься. |
> | Action | Microsoft.Compute/virtualMachines/read | Получение свойств виртуальной машины |
> | Action | Microsoft.Compute/virtualMachines/redeploy/action | Повторно развертывает виртуальную машину |
> | Action | Microsoft.Compute/virtualMachines/reimage/action | Пересоздает образ виртуальной машины, которая использует разностный диск. |
> | Action | Microsoft.Compute/virtualMachines/restart/action | Перезапуск виртуальной машины |
> | Action | Microsoft.Compute/virtualMachines/runCommand/action | Выполняет предопределенный сценарий в виртуальной машине |
> | Action | Microsoft.Compute/virtualMachines/start/action | Запуск виртуальной машины |
> | Action | Microsoft.Compute/virtualMachines/vmSizes/read | Получение списка доступных размеров виртуальной машины, до которых ее можно обновить |
> | Action | Microsoft.Compute/virtualMachines/write | Создание новой виртуальной машины или обновление существующей |
> | Action | Microsoft.Compute/virtualMachineScaleSets/deallocate/action | Завершение работы вычислительных ресурсов и их освобождение для экземпляров виртуальной машины в масштабируемом наборе виртуальных машин.  |
> | Action | Microsoft.Compute/virtualMachineScaleSets/delete | Удаляет масштабируемый набор виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/delete/action | Удаляет экземпляры масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/extensions/delete | Удаляет расширение масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/extensions/read | Получает свойства расширения масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/extensions/write | Создает расширение масштабируемого набора виртуальных машин или обновляет существующее |
> | Action | Microsoft.Compute/virtualMachineScaleSets/forceRecoveryServiceFabricPlatformUpdateDomainWalk/action | Вручную пройдите по доменам обновления платформы в масштабируемом наборе виртуальных машин Service Fabric, чтобы завершить обновление, зависшее в состоянии ожидания |
> | Action | Microsoft.Compute/virtualMachineScaleSets/instanceView/read | Получает представление экземпляров масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/manualUpgrade/action | Вручную обновляет экземпляры масштабируемого набора виртуальных машин до последней модели |
> | Action | Microsoft.Compute/virtualMachineScaleSets/networkInterfaces/read | Получение свойств всех сетевых интерфейсов масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/osRollingUpgrade/action | Запускает последовательное обновление для перевода всех экземпляров масштабируемых наборов виртуальных машин на последнюю доступную версию ОС образа платформы. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/osUpgradeHistory/read | Получает журнал обновлений ОС для масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/performMaintenance/action | Проводит плановое обслуживание в экземплярах масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/powerOff/action | Отключает питание экземпляров масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/publicIPAddresses/read | Получение свойств всех общедоступных IP-адресов масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/read | Получение свойств масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/redeploy/action | Повторное развертывание экземпляров масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/reimage/action | Повторно создает образы экземпляров масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/reimageAll/action | Пересоздает образы всех дисков (диска ОС и дисков данных) для экземпляров масштабируемого набора виртуальных машин.  |
> | Action | Microsoft.Compute/virtualMachineScaleSets/restart/action | Перезапускает экземпляры масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/cancel/action | Отменяет последовательное обновление масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/rollingUpgrades/read | Получить состояние последнего последовательного обновления масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/scale/action | Проверка того, возможно ли увеличение или уменьшение числа экземпляров масштабируемого набора виртуальных машин до указанного значения |
> | Action | Microsoft.Compute/virtualMachineScaleSets/skus/read | Получение списка действительных номеров SKU для существующего масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/start/action | Запускает экземпляры масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/deallocate/action | Выключение и освобождение вычислительных ресурсов для виртуальной машины в соответствующем наборе масштабирования. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/delete | Удаление определенной виртуальной машины в наборе масштабирования виртуальных машин. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/instanceView/read | Получение представления экземпляров виртуальной машины для набора масштабирования виртуальных машин. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/publicIPAddresses/read | Получение свойства общедоступного IP-адреса, созданного с использованием масштабируемого набора виртуальных машин. Масштабируемый набор виртуальных машин может создать не более одного общедоступного IP-адреса на IP-конфигурацию (частный IP-адрес). |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/ipConfigurations/read | Получение свойств одной или всех IP-конфигураций сетевого интерфейса, созданного с помощью масштабируемого набора виртуальных машин. IP-конфигурации представляют частные IP-адреса. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/networkInterfaces/read | Получение свойств одного сетевого интерфейса или всех для виртуальной машины, созданной с помощью масштабируемого набора виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/performMaintenance/action | Проводит плановое обслуживание экземпляра виртуальной машины в масштабируемом наборе виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/powerOff/action | Отключение экземпляра виртуальной машины в наборе масштабирования виртуальных машин. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read | Получение свойств виртуальной машины в наборе масштабирования виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/redeploy/action | Повторно развертывает экземпляр виртуальной машины в масштабируемом наборе виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimage/action | Повторно создает образ виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/reimageAll/action | Пересоздает образы всех дисков (диска ОС и дисков с данными) для экземпляра виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/restart/action | Перезапуск виртуальной машины в наборе масштабирования виртуальных машин. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/runCommand/action | Выполняет заранее определенный сценарий в экземпляре виртуальной машины в масштабируемом наборе виртуальных машин. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/start/action | Запуск виртуальной машины в наборе масштабирования виртуальных машин. |
> | Action | Microsoft.Compute/virtualMachineScaleSets/virtualMachines/write | Обновляет свойства виртуальной машины в масштабируемом наборе виртуальных машин |
> | Action | Microsoft. COMPUTE/virtualMachineScaleSets/Вмсизес/чтение | Список доступных размеров для создания или обновления виртуальной машины в масштабируемом наборе виртуальных машин |
> | Action | Microsoft.Compute/virtualMachineScaleSets/write | Создает новый масштабируемый набор виртуальных машин или изменяет существующий |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Consumption/balances/read | Получение списка сводки использования за период выставления счетов для группы управления. |
> | Action | Microsoft.Consumption/budgets/delete | Удаление бюджетов подписки или группы управления. |
> | Action | Microsoft.Consumption/budgets/read | Получение списка бюджетов подписки или группы управления. |
> | Action | Microsoft.Consumption/budgets/write | Создание или обновление бюджетов подписки или группы управления. |
> | Action | Microsoft.Consumption/charges/read | Вывод списка расходов. |
> | Action | Microsoft.Consumption/credits/read | Вывод списка кредитов. |
> | Action | Microsoft.Consumption/events/read | Вывод списка событий. |
> | Action | Microsoft.Consumption/forecasts/read | Вывод списка прогнозов. |
> | Action | Microsoft.Consumption/slots/read | Вывод списка слотов. |
> | Action | Microsoft.Consumption/marketplaces/read | Вывод сведений об использовании ресурсов Marketplace для области подписок EA и WebDirect. |
> | Action | Microsoft.Consumption/operationresults/read | Вывод списка результатов операций. |
> | Action | Microsoft.Consumption/operations/read | Вывод списка всех поддерживаемых операций для поставщика ресурсов Microsoft.Consumption. |
> | Action | Microsoft.Consumption/operationstatus/read | Вывод списка состояний операций. |
> | Action | Microsoft.Consumption/pricesheets/read | Вывод расценок для подписки или группы управления. |
> | Action | Microsoft.Consumption/register/action | Регистрация в поставщике используемых ресурсов. |
> | Action | Microsoft.Consumption/reservationDetails/read | Вывод сведений об использовании зарезервированных экземпляров по порядку резервирования или группам управления. Данные относятся к одному экземпляру на уровне дня. |
> | Action | Microsoft.Consumption/reservationRecommendations/read | Вывод одной рекомендации или нескольких общих рекомендаций для зарезервированных экземпляров в подписке. |
> | Action | Microsoft.Consumption/reservationSummaries/read | Вывод сводки использования зарезервированных экземпляров по порядку резервирования или группам управления. Сводка формируется на ежемесячном или ежедневном уровне. |
> | Action | Microsoft.Consumption/reservationTransactions/read | Вывод журнала транзакций для зарезервированных экземпляров по группам управления. |
> | Action | Microsoft.Consumption/tags/read | Вывод списка тегов для EA и подписок. |
> | Action | Microsoft. потребление/клиенты/чтение | Список клиентов |
> | Action | Microsoft.Consumption/tenants/register/action | Регистрация действия для области Microsoft.Consumption клиентом. |
> | Action | Microsoft.Consumption/terms/read | Вывод условий подписки или группы управления. |
> | Action | Microsoft.Consumption/usageDetails/read | Вывод сведений об использовании для области подписок EA и WebDirect. |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ContainerInstance/containerGroups/containers/exec/action | Выполнение в указанном контейнере. |
> | Action | Microsoft.ContainerInstance/containerGroups/containers/logs/read | Получение журналов для конкретного контейнера. |
> | Action | Microsoft.ContainerInstance/containerGroups/delete | Удаление конкретной группы контейнеров. |
> | Action | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметров диагностики для группы контейнеров. |
> | Action | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/diagnosticSettings/write | Создание или изменение параметров диагностики для группы контейнеров. |
> | Action | Microsoft.ContainerInstance/containerGroups/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для группы контейнеров. |
> | Action | Microsoft.ContainerInstance/containerGroups/read | Получение всех групп контейнеров. |
> | Action | Microsoft.ContainerInstance/containerGroups/restart/action | Перезапускает конкретную группу контейнеров. |
> | Action | Microsoft.ContainerInstance/containerGroups/start/action | Запускает конкретную группу контейнеров. |
> | Action | Microsoft.ContainerInstance/containerGroups/stop/action | Останавливает конкретную группу контейнеров. Вычислительные ресурсы будут отозваны, и выставление счетов прекратится. |
> | Action | Microsoft.ContainerInstance/containerGroups/write | Создание или обновление конкретной группы контейнеров. |
> | Action | Microsoft.ContainerInstance/locations/cachedImages/read | Получение кэшированных изображений для подписки в регионе. |
> | Action | Microsoft.ContainerInstance/locations/capabilities/read | Получение возможностей для региона. |
> | Action | Microsoft.ContainerInstance/locations/deleteVirtualNetworkOrSubnets/action | Уведомление Microsoft.ContainerInstance о том, что идет удаление виртуальной сети или подсети. |
> | Action | Microsoft.ContainerInstance/locations/usages/read | Получение данных об использовании для определенного региона. |
> | Action | Microsoft.ContainerInstance/register/action | Регистрирует подписку в поставщике ресурсов экземпляра контейнера и включает возможность создания групп контейнеров. |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ContainerRegistry/checkNameAvailability/read | Проверка возможности использования имени реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/locations/deleteVirtualNetworkOrSubnets/action | Уведомляет Microsoft.ContainerRegistry о том, что удаляется виртуальная сеть или подсеть. |
> | Action | Microsoft.ContainerRegistry/locations/operationResults/read | Получение результата асинхронной операции. |
> | Action | Microsoft.ContainerRegistry/operations/read | Вывод списка всех доступных операций API REST реестра контейнеров Azure. |
> | Action | Microsoft.ContainerRegistry/register/action | Регистрирует подписку для поставщика ресурсов реестра контейнеров и позволяет создавать реестры контейнеров. |
> | Action | Microsoft. ContainerRegistry/реестры/артефакты/удаление | Удаление артефакта в реестре контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/builds/cancel/action | Отменяет существующую сборку. |
> | Action | Microsoft.ContainerRegistry/registries/builds/getLogLink/action | Возвращает ссылку для скачивания журналов сборки. |
> | Action | Microsoft.ContainerRegistry/registries/builds/read | Получает свойства указанной сборки или списка сборок для указанного реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/builds/write | Обновляет сборку для реестра контейнеров с указанными параметрами. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/delete | Удаляет задание сборки из реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/listSourceRepositoryProperties/action | Перечисляет свойства системы управления версиями для задачи сборки. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/read | Получает свойства указанной задачи сборки или списка задач сборок для указанного реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/delete | Удаляет шаг создания из задачи сборки. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/listBuildArguments/action | Перечисляет аргументы сборки для шага сборки, включая аргументы секретов. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/read | Получает свойства указанного шага сборки или списка шагов сборок для указанной задачи сборки. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/steps/write | Создает или обновляет шаг сборки для задачи сборки с указанными параметрами. |
> | Action | Microsoft.ContainerRegistry/registries/buildTasks/write | Создает или обновляет задачу сборки для реестра контейнеров с указанными параметрами. |
> | Action | Microsoft.ContainerRegistry/registries/delete | Удаление реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/eventGridFilters/delete | Удаление фильтра сетки событий из реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/eventGridFilters/read | Получение свойств указанного фильтра сетки событий или списка всех фильтров сетки событий для указанного реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/eventGridFilters/write | Создание или обновление фильтра сетки событий для реестра контейнеров с указанными параметрами. |
> | Action | Microsoft.ContainerRegistry/registries/getBuildSourceUploadUrl/action | Получает расположение для передачи, чтобы пользователь мог передать источник. |
> | Action | Microsoft.ContainerRegistry/registries/importImage/action | Импорт образа в реестр контейнеров с указанными параметрами. |
> | Action | Microsoft.ContainerRegistry/registries/listBuildSourceUploadUrl/action | Получение расположения URL-адреса передачи источника для реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/listCredentials/action | Выводит список учетных данных входа для указанного реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/listPolicies/read | Выводит список политик для указанного реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/listUsages/read | Получение сведений об использовании квот для указанного реестра контейнеров. |
> | Action | Microsoft. ContainerRegistry/реестры/метаданные/чтение | Возвращает метаданные определенного репозитория для реестра контейнеров. |
> | Action | Microsoft. ContainerRegistry/реестры/метаданные/запись | Обновляет метаданные репозитория для реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/operationStatuses/read | Получение состояния асинхронной операции реестра. |
> | Action | Microsoft.ContainerRegistry/registries/pull/read | Извлечение или получение образов из реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/push/write | Передача или запись образов в реестр контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/quarantineRead/read | Извлечение или получение помещенных в карантин образов из реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/quarantineWrite/write | Запись и изменение состояния карантина образов, помещенных в карантин. |
> | Action | Microsoft.ContainerRegistry/registries/queueBuild/action | Создает новую сборку на основе параметров запроса и добавляет ее в очередь сборки. |
> | Action | Microsoft.ContainerRegistry/registries/read | Получение свойств указанного реестра контейнеров или вывод списка всех реестров контейнеров в указанной группе ресурсов или подписке. |
> | Action | Microsoft.ContainerRegistry/registries/regenerateCredential/action | Повторное создание учетных данных входа для указанного реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/replications/delete | Удаление репликации из реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/replications/operationStatuses/read | Получение состояния асинхронной операции репликации. |
> | Action | Microsoft.ContainerRegistry/registries/replications/read | Получение свойств указанной репликации или списка репликаций для указанного реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/replications/write | Создание или обновление репликаций для реестра контейнеров с указанными параметрами. |
> | Action | Microsoft.ContainerRegistry/registries/runs/cancel/action | Отмена существующего выполнения. |
> | Action | Microsoft.ContainerRegistry/registries/runs/listLogSasUrl/action | Получение URL-адреса SAS журнала для выполнения. |
> | Action | Microsoft.ContainerRegistry/registries/runs/read | Получение свойств выполнения по реестру контейнера или списку запусков. |
> | Action | Microsoft.ContainerRegistry/registries/runs/write | Обновление выполнения. |
> | Action | Microsoft.ContainerRegistry/registries/scheduleRun/action | Планирование запуска по реестру контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/sign/write | Передача или извлечение метаданных доверия к содержимому для реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/delete | Удаление задачи для реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/listDetails/action | Список всех сведений о задаче для реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/read | Получение задачи для реестра контейнеров или списка всех задач. |
> | Action | Microsoft.ContainerRegistry/registries/tasks/write | Создание или обновление задачи для реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/updatePolicies/write | Обновляет политики для указанного реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/delete | Удаление веб-перехватчика из реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/getCallbackConfig/action | Получение конфигурации URI службы и пользовательских заголовков для веб-перехватчика. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/listEvents/action | Получение списка последних событий для указанного веб-перехватчика. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/operationStatuses/read | Получение состояния асинхронной операции веб-перехватчика. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/ping/action | Запуск события проверки связи для отправки веб-перехватчику. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/read | Получение свойств указанного веб-перехватчика или списка веб-перехватчиков для указанного реестра контейнеров. |
> | Action | Microsoft.ContainerRegistry/registries/webhooks/write | Создание или обновление веб-перехватчика для реестра контейнеров с указанными параметрами. |
> | Action | Microsoft.ContainerRegistry/registries/write | Создание или обновление реестра контейнеров с указанными параметрами. |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ContainerService/containerServices/delete | Удаляет службу контейнеров |
> | Action | Microsoft.ContainerService/containerServices/read | Получить службу контейнеров |
> | Action | Microsoft.ContainerService/containerServices/write | Создает службу контейнеров или обновляет существующую |
> | Action | Microsoft.ContainerService/locations/operationresults/read | Получает состояние для результата асинхронной операции |
> | Action | Microsoft.ContainerService/locations/operations/read | Получение состояния асинхронной операции |
> | Action | Microsoft.ContainerService/locations/orchestrators/read | Перечисляет поддерживаемые оркестраторы |
> | Action | Microsoft.ContainerService/managedClusters/accessProfiles/listCredential/action | Получить профиль доступа управляемого кластера по имени роли с помощью учетных данных списка |
> | Action | Microsoft.ContainerService/managedClusters/accessProfiles/read | Получить профиль доступа управляемого кластера по имени роли |
> | Action | Microsoft. ContainerService/Манажедклустерс/Ажентпулс/Delete | Удаляет пул агентов. |
> | Action | Microsoft. ContainerService/Манажедклустерс/Ажентпулс/Read | Возвращает Пул агентов |
> | Action | Microsoft. ContainerService/Манажедклустерс/Ажентпулс/запись | Создает новый пул агентов или обновляет существующий. |
> | Action | Microsoft.ContainerService/managedClusters/delete | Удаляет управляемый кластер |
> | Action | Microsoft. ContainerService/Манажедклустерс/детекторы/чтение | Получить средство обнаружения управляемого кластера |
> | Action | Microsoft.ContainerService/managedClusters/listClusterAdminCredential/action | Перечислить учетные данные clusterAdmin управляемого кластера |
> | Action | Microsoft.ContainerService/managedClusters/listClusterUserCredential/action | Перечислить учетные данные clusterUser управляемого кластера |
> | Action | Microsoft.ContainerService/managedClusters/read | Получить управляемый кластер |
> | Action | Microsoft.ContainerService/managedClusters/resetAADProfile/action | Сброс профиля AAD управляемого кластера. |
> | Action | Microsoft.ContainerService/managedClusters/resetServicePrincipalProfile/action | Сброс профиля субъекта-службы управляемого кластера. |
> | Action | Microsoft.ContainerService/managedClusters/upgradeprofiles/read | Возвращает профиль обновления для кластера |
> | Action | Microsoft.ContainerService/managedClusters/write | Создает управляемый кластер или изменяет существущий |
> | Action | Microsoft.ContainerService/openShiftClusters/delete | Удаление кластера с открытым сдвигом |
> | Action | Microsoft.ContainerService/openShiftClusters/read | Получение кластера с открытым сдвигом |
> | Action | Microsoft.ContainerService/openShiftClusters/write | Создание нового кластера Open Shift или обновление имеющегося |
> | Action | Microsoft.ContainerService/openShiftManagedClusters/delete | Удаление управляемого кластера с открытым сдвигом |
> | Action | Microsoft.ContainerService/openShiftManagedClusters/read | Получение управляемого кластера с открытым сдвигом |
> | Action | Microsoft.ContainerService/openShiftManagedClusters/write | Создание нового управляемого кластера Open Shift или обновление имеющегося |
> | Action | Microsoft.ContainerService/operations/read | Выводит список операций, доступных в поставщике ресурсов Microsoft.ContainerService |
> | Action | Microsoft.ContainerService/register/action | Регистрирует подписку в поставщике ресурсов Microsoft.ContainerService |
> | Action | Microsoft.ContainerService/unregister/action | Отменяет регистрацию подписки в поставщике ресурсов Microsoft.ContainerService |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ContentModerator/applications/delete | Операция удаления |
> | Action | Microsoft.ContentModerator/applications/listSecrets/action | Вывод списка секретов |
> | Action | Microsoft.ContentModerator/applications/listSingleSignOnToken/action | Считывает маркеры единого входа. |
> | Action | Microsoft.ContentModerator/applications/read | Операция чтения |
> | Action | Microsoft.ContentModerator/applications/write | Операция записи |
> | Action | Microsoft.ContentModerator/applications/write | Операция записи |
> | Action | Microsoft.ContentModerator/listCommunicationPreference/action | Отображает параметры связи. |
> | Action | Microsoft.ContentModerator/operations/read | Считывает операции. |
> | Action | Microsoft.ContentModerator/updateCommunicationPreference/action | Обновляет параметры связи. |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Костманажемент/Клаудконнекторс/Delete | Удаляет указанный значение cloudconnector. |
> | Action | Microsoft. Костманажемент/Клаудконнекторс/Read | Перечислите Клаудконнекторс для пользователя, прошедшего проверку подлинности. |
> | Action | Microsoft. Костманажемент/Клаудконнекторс/запись | Создание или обновление указанного значение cloudconnector. |
> | Action | Microsoft.CostManagement/dimensions/read | Вывод списка всех поддерживаемых измерений в области. |
> | Action | Microsoft.CostManagement/exports/action | Выполнение указанной операции экспорта. |
> | Action | Microsoft.CostManagement/exports/delete | Удаление указанной операции экспорта. |
> | Action | Microsoft.CostManagement/exports/read | Вывод списка операций экспорта по области. |
> | Action | Microsoft. Костманажемент/EXPORTS/Run/Action | Запустите экспорты. |
> | Action | Microsoft.CostManagement/exports/write | Создание или обновление указанной операции экспорта. |
> | Action | Microsoft. Костманажемент/Екстерналбиллингаккаунтс/Екстерналсубскриптионс/Read | Перечислите Екстерналсубскриптионс в Екстерналбиллингаккаунт для пользователя, прошедшего проверку подлинности. |
> | Action | Microsoft. Костманажемент/Екстерналбиллингаккаунтс/Read | Перечислите Екстерналбиллингаккаунтс для пользователя, прошедшего проверку подлинности. |
> | Action | Microsoft. Костманажемент/Екстерналсубскриптионс/Read | Перечислите Екстерналсубскриптионс для пользователя, прошедшего проверку подлинности. |
> | Action | Microsoft. Костманажемент/Екстерналсубскриптионс/запись | Обновление связанной группы управления Екстерналсубскриптион |
> | Action | Microsoft.CostManagement/query/action | Запрос данных об использовании в области. |
> | Action | Microsoft.CostManagement/query/read | Запрос данных об использовании в области. |
> | Action | Microsoft. Костманажемент/Register/действие | Регистрация действия для области действия Microsoft. Костманажемент по подписке. |
> | Action | Microsoft.CostManagement/reports/action | Планирование создания отчетов о данных об использовании в области. |
> | Action | Microsoft.CostManagement/reports/read | Планирование создания отчетов о данных об использовании в области. |
> | Action | Microsoft. Костманажемент/клиенты/регистрация/действие | Регистрация действия для области действия Microsoft. Костманажемент клиентом. |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.CustomerInsights/hubs/adobemetadata/action | Создание или обновление метаданных Adobe для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/adobemetadata/read | Чтение метаданных Adobe для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/authorizationPolicies/delete | Удаляет политику подписанного URL-адреса Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/authorizationPolicies/read | Считывает политику подписанного URL-адреса Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/authorizationPolicies/regeneratePrimaryKey/action | Повторно создает первичный ключ политики подписанного URL-адреса Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/authorizationPolicies/regenerateSecondaryKey/action | Повторно создает вторичный ключ политики подписанного URL-адреса Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/authorizationPolicies/write | Создает или обновляет политику подписанного URL-адреса Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/activate/action | Активация соединителя Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/activate/action | Активация соединителя Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/delete | Удаляет соединитель Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/getruntimestatus/action | Получение состояния выполнения соединителя Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/mappings/activate/action | Активация сопоставления соединителя Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/mappings/delete | Удаляет сопоставление соединителя Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/mappings/operations/read | Чтение результата операции сопоставления соединителя для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/mappings/read | Считывает сопоставление соединителя Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/mappings/write | Создает или обновляет сопоставление соединителя Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/operations/read | Чтение результата операции соединителя для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/read | Считывает соединитель Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/saveauthinfo/action | Создание или обновление данных аутентификации соединителя Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/update/action | Обновление соединителя Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/connectors/write | Создает или обновляет соединитель Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/crmmetadata/action | Создание или обновление метаданных CRM для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/crmmetadata/read | Чтение метаданных CRM для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/delete | Удаляет концентратор Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/gdpr/delete | Удаление GDPR Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/gdpr/read | Чтение GDPR Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/gdpr/write | Создание или обновление GDPR Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/getbillingcredits/read | Получение данных о кредитах на концентратор Azure Insights. |
> | Action | Microsoft.CustomerInsights/hubs/getbillinghistory/read | Получение журнала выставленных счетов за использование концентратора Azure Insights. |
> | Action | Microsoft.CustomerInsights/hubs/images/delete | Удаление образа Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/images/read | Чтение образа Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/images/write | Создание или обновление образа Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/interactions/delete | Удаление взаимодействий Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/interactions/operations/read | Чтение результата операции взаимодействия для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/interactions/read | Считывает взаимодействие Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/interactions/suggestrelationshiplinks/action | Предложение ссылок связи для взаимодействий Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/interactions/write | Создает или обновляет взаимодействие Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/kpi/delete | Удаляет ключевой показатель эффективности Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/kpi/operations/read | Чтение результата операции получения ключевых показателей эффективности для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/kpi/read | Считывает ключевой показатель эффективности Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/kpi/reprocess/action | Повторная обработка ключевых показателей эффективности Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/kpi/write | Создает или обновляет ключевой показатель эффективности Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/links/delete | Удаление ссылок Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/links/operations/read | Чтение результата операции связей соединителя для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/links/read | Чтение ссылок Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/links/write | Создание или обновление ссылок Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/msemetadata/action | Создание или обновление метаданных MSE для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/msemetadata/read | Чтение метаданных MSE для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/operationresults/read | Получение результатов операции концентратора для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/predictions/delete | Удаление прогнозов Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/predictions/operations/read | Чтение результата операции получения прогнозов для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/predictions/read | Чтение прогнозов Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/predictions/write | Создание или обновление прогнозов Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/predictivematchpolicies/delete | Удаление политик прогнозного сопоставления Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/predictivematchpolicies/operations/read | Чтение результата операции политик прогнозного сопоставления для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/predictivematchpolicies/read | Чтение политик прогнозного сопоставления Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/predictivematchpolicies/write | Создание или обновление политик прогнозного сопоставления Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/profiles/delete | Удаление профиля Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/profiles/operations/read | Чтение результата операции профиля для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/profiles/read | Считывает профиль Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/profiles/write | Записывает профиль Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/read | Получает настройку диагностики для ресурса |
> | Action | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/logDefinitions/read | Возвращает доступные журналы для ресурса. |
> | Action | Microsoft.CustomerInsights/hubs/providers/Microsoft.Insights/metricDefinitions/read | Возвращает доступные метрики для ресурса. |
> | Action | Microsoft.CustomerInsights/hubs/read | Считывает концентратор Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/relationshiplinks/delete | Удаление ссылок связи Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/relationshiplinks/operations/read | Чтение результата операции ссылок связи для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/relationshiplinks/read | Чтение ссылок связи Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/relationshiplinks/write | Создание или обновление ссылок связи Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/relationships/delete | Удаление связей Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/relationships/operations/read | Чтение результата операции связей для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/relationships/read | Чтение связей Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/relationships/write | Создание или обновление связей Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/roleAssignments/delete | Удаляет назначение RBAC для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/roleAssignments/operations/read | Чтение результатов операции назначения RBAC для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/roleAssignments/read | Считывает назначение RBAC для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/roleAssignments/write | Создает или обновляет назначение RBAC для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/roles/read | Чтение ролей RBAC для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/salesforcemetadata/action | Создание или обновление метаданных SalesForce для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/salesforcemetadata/read | Чтение метаданных SalesForce для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/segments/delete | Удаление сегментов Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/segments/dynamic/action | Управление динамическими сегментами Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/segments/read | Чтение сегментов Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/segments/static/action | Управление статическими сегментами Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/segments/write | Создание или обновление сегментов Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/delete | Удаление строк подключения SQL для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/read | Чтение строк подключения SQL для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/sqlconnectionstrings/write | Создание или обновление строк подключения SQL для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/suggesttypeschema/action | Создание предложения схемы типа из примера данных. |
> | Action | Microsoft.CustomerInsights/hubs/tenantmanagement/read | Управление параметрами концентратора Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/views/delete | Удаляет представление приложений Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/views/read | Считывает представление приложений Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/views/write | Создает или обновляет представление приложений Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/widgettypes/read | Чтение типов мини-приложений приложения Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/hubs/write | Создает или обновляет концентратор Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/operations/read | Чтение метаданных API для Azure Customer Insights. |
> | Action | Microsoft.CustomerInsights/register/action | Регистрация подписки для поставщика ресурсов Customer Insights и разрешение создания ресурсов Customer Insights. |
> | Action | Microsoft.CustomerInsights/unregister/action | Отмена регистрации подписки для поставщика ресурсов Customer Insights. |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DataBox/jobs/bookShipmentPickUp/action | Позволяет заказать прием посылки для обратной отправки. |
> | Action | Microsoft.DataBox/jobs/cancel/action | Отменяет выполняемый заказ. |
> | Action | Microsoft.DataBox/jobs/delete | Удаляет заказы. |
> | Action | Microsoft.DataBox/jobs/listCredentials/action | Список незашифрованных учетных данных, связанных с заказом. |
> | Action | Microsoft.DataBox/jobs/read | Выводит список заказов или получает их. |
> | Action | Microsoft.DataBox/jobs/write | Создает или обновляет заказы. |
> | Action | Microsoft.DataBox/locations/availableSkus/action | Этот метод возвращает список доступных номеров SKU. |
> | Action | Microsoft. Датабокс/Locations/Аваилаблескус/Read | Перечисление или получение доступных номеров SKU |
> | Action | Microsoft.DataBox/locations/operationResults/read | Вывод списка или получение результатов операции. |
> | Action | Microsoft.DataBox/locations/validateAddress/action | Проверяет адрес доставки и предлагает альтернативные адреса, если они доступны. |
> | Action | Microsoft. Датабокс/Locations/Валидатеинпутс/Action |  |
> | Action | Microsoft.DataBox/register/action | Регистрация поставщика Microsoft.Databox. |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | Выводит список оповещений или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/alerts/read | Выводит список оповещений или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/delete | Удаляет расписания пропускной способности |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Бандвидссчедулес/Оператионресултс/Read | Выводит или возвращает результат операции. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | Выводит список расписаний пропускной способности или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/read | Выводит список расписаний пропускной способности или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/bandwidthSchedules/write | Создает или обновляет расписания пропускной способности |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/delete | Удаляет устройства Data Box Edge |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/downloadUpdates/action | Скачать обновления на устройство |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Жетекстендединформатион/Action | Возвращает расширенную информацию о ресурсе |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/installUpdates/action | Устанавливает обновления на устройство |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/jobs/read | Выводит список заданий или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/networkSettings/read | Выводит список параметров сети устройства или возвращает их |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Оператионресултс/Read | Выводит или возвращает результат операции. |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Оператионсстатус/Read | Выводит или возвращает состояние операции. |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Orders/Delete | Удаляет заказы. |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Orders/Оператионресултс/Read | Выводит или возвращает результат операции. |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/заказы/чтение | Перечисление или получение заказов |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/заказы/чтение | Перечисление или получение заказов |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/заказы/запись | Создает или обновляет заказы. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Выводит список устройств Data Box Edge или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Выводит список устройств Data Box Edge или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/read | Выводит список устройств Data Box Edge или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/delete | Удаляет роли. |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Roles/Оператионресултс/Read | Выводит или возвращает результат операции. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Выводит список ролей или возвращает их. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/read | Выводит список ролей или возвращает их. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/roles/write | Создает или обновляет роли. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/scanForUpdates/action | Проверяет на наличие обновлений |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/operationResults/read | Выводит или возвращает результат операции. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/securitySettings/update/action | Обновляет параметры безопасности |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/delete | Удаляет общедоступные ресурсы |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/shares/Оператионресултс/Read | Выводит или возвращает результат операции. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | Выводит список общедоступных ресурсов или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/read | Выводит список общедоступных ресурсов или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/refresh/action | Обновить метаданные общих папок данными из облака |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/shares/write | Создает общие папки или обновляет их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/delete | Удаляет данные учетной записи хранения |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Сторажеаккаунткредентиалс/Оператионресултс/Read | Выводит или возвращает результат операции. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | Выводит список данных учетной записи хранения или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/read | Выводит список данных учетной записи хранения или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/storageAccountCredentials/write | Создает данные учетной записи хранения или обновляет их |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Triggers/Delete | Удаляет триггеры. |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Triggers/Оператионресултс/Read | Выводит или возвращает результат операции. |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/триггеры/чтение | Перечисление или получение триггеров |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/триггеры/чтение | Перечисление или получение триггеров |
> | Action | Microsoft. Датабокседже/Датабокседжедевицес/Triggers/Write | Создает или обновляет триггеры. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/updateSummary/read | Выводит сводку обновлений или возвращает ее |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/uploadCertificate/action | Отправьте сертификат для регистрации устройства |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/delete | Удаляет пользователей общих ресурсов |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/operationResults/read | Выводит или возвращает результат операции. |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Выводит список пользователей общих ресурсов или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/read | Выводит список пользователей общих ресурсов или возвращает их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/users/write | Создает пользователей общих ресурсов или обновляет их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Создает устройства Data Box Edge или обновляет их |
> | Action | Microsoft.DataBoxEdge/dataBoxEdgeDevices/write | Создает устройства Data Box Edge или обновляет их |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Databricks/register/action | Регистрация в Databricks. |
> | Action | Microsoft.Databricks/workspaces/delete | Удаляет рабочую область Databricks. |
> | Action | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/read | Задает доступные параметры диагностики для рабочей области Databricks |
> | Action | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/diagnosticSettings/write | Добавление или изменение параметров диагностики. |
> | Action | Microsoft.Databricks/workspaces/providers/Microsoft.Insights/logDefinitions/read | Получает доступные определения журналов для рабочей области Databricks |
> | Action | Microsoft.Databricks/workspaces/read | Получает список рабочих областей Databricks. |
> | Action | Microsoft.Databricks/workspaces/write | Создает рабочую область Databricks. |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DataCatalog/catalogs/delete | Удаление ресурса каталогов для поставщика ресурсов каталога данных. |
> | Action | Microsoft.DataCatalog/catalogs/read | Считывает ресурс каталогов для поставщика ресурсов каталога данных. |
> | Action | Microsoft.DataCatalog/catalogs/write | Ресурс записи каталогов для поставщика ресурсов каталога данных. |
> | Action | Microsoft. catalog/Чеккнамеаваилабилити/чтение | Проверьте доступность имени каталога для поставщика ресурсов каталога данных. |
> | Action | Microsoft. catalog/Каталогизация/удаление | Удаление ресурса каталога данных для поставщика ресурсов каталога. |
> | Action | Microsoft. catalog/Каталогизация/чтение | Чтение ресурса каталога данных для поставщика ресурсов каталога. |
> | Action | Microsoft. catalog/Каталогизация/запись | Запись ресурса каталога данных для поставщика ресурсов каталога. |
> | Action | Microsoft.DataCatalog/operations/read | Считывает все доступные операции в поставщике ресурсов каталога данных. |
> | Action | Microsoft.DataCatalog/register/action | Регистрация подписки для поставщика ресурсов каталога данных |
> | Action | Microsoft.DataCatalog/unregister/action | Отмена регистрации подписки для поставщика ресурсов каталога данных |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DataFactory/checkazuredatafactorynameavailability/read | Проверяет, доступно ли имя фабрики данных для использования. |
> | Action | Microsoft.DataFactory/datafactories/activitywindows/read | Считывает окна действий в фабрике данных с указанными параметрами. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/activities/activitywindows/read | Считывает окна действий для действия конвейера с указанными параметрами. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/activitywindows/read | Считывает окна действий для конвейера с указанными параметрами. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/delete | Удаляет конвейер. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/pause/action | Приостанавливает конвейер. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/read | Считывает конвейер. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/resume/action | Возобновляет работу конвейера. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/update/action | Обновляет конвейер. |
> | Action | Microsoft.DataFactory/datafactories/datapipelines/write | Создает или обновляет конвейер. |
> | Action | Microsoft.DataFactory/datafactories/datasets/activitywindows/read | Считывает окна действий для набора данных с указанными параметрами. |
> | Action | Microsoft.DataFactory/datafactories/datasets/delete | Удаляет набор данных. |
> | Action | Microsoft.DataFactory/datafactories/datasets/read | Считывает набор данных. |
> | Action | Microsoft.DataFactory/datafactories/datasets/sliceruns/read | Считывает выполнение среза данных для заданного набора данных с указанным временем запуска. |
> | Action | Microsoft.DataFactory/datafactories/datasets/slices/read | Возвращает срезы данных за указанный период. |
> | Action | Microsoft.DataFactory/datafactories/datasets/slices/write | Обновляет состояние среза данных. |
> | Action | Microsoft.DataFactory/datafactories/datasets/write | Создает или обновляет набор данных. |
> | Action | Microsoft.DataFactory/datafactories/delete | Удаляет фабрику данных. |
> | Action | Microsoft.DataFactory/datafactories/gateways/connectioninfo/action | Считывает сведения о подключении для шлюза. |
> | Action | Microsoft.DataFactory/datafactories/gateways/delete | Удаляет шлюз. |
> | Action | Microsoft.DataFactory/datafactories/gateways/listauthkeys/action | Выводит список ключей аутентификации для шлюза. |
> | Action | Microsoft.DataFactory/datafactories/gateways/read | Считывает шлюз. |
> | Action | Microsoft.DataFactory/datafactories/gateways/regenerateauthkey/action | Повторно создает ключи аутентификации для шлюза. |
> | Action | Microsoft.DataFactory/datafactories/gateways/write | Создает или обновляет шлюз. |
> | Action | Microsoft.DataFactory/datafactories/linkedServices/delete | Удаляет связанную службу. |
> | Action | Microsoft.DataFactory/datafactories/linkedServices/read | Считывает связанную службу. |
> | Action | Microsoft.DataFactory/datafactories/linkedServices/write | Создает или обновляет связанную службу. |
> | Action | Microsoft.DataFactory/datafactories/read | Считывает фабрику данных. |
> | Action | Microsoft.DataFactory/datafactories/runs/loginfo/read | Считывает универсальный код ресурса SAS контейнера больших двоичных объектов, содержащего журналы. |
> | Action | Microsoft.DataFactory/datafactories/tables/delete | Удаляет набор данных. |
> | Action | Microsoft.DataFactory/datafactories/tables/read | Считывает набор данных. |
> | Action | Microsoft.DataFactory/datafactories/tables/write | Создает или обновляет набор данных. |
> | Action | Microsoft.DataFactory/datafactories/write | Создает или обновляет фабрику данных. |
> | Action | Microsoft.DataFactory/factories/cancelpipelinerun/action | Отменяет запуск конвейера, указанный идентификатором запуска. |
> | Action | Microsoft. креатедатафловдебугсессион/фабрики//действие | Создает сеанс отладки потока данных. |
> | Action | Microsoft. DataFlow/фабрики/поток данных/удаление | Удаляет поток данных. |
> | Action | Microsoft. DataFlow/фабрики/поток данных/чтение | Считывает поток данных. |
> | Action | Microsoft. DataFlow/фабрики/потоки данных/запись | Создание или обновление потока данных |
> | Action | Microsoft.DataFactory/factories/datasets/delete | Удаляет набор данных. |
> | Action | Microsoft.DataFactory/factories/datasets/read | Считывает набор данных. |
> | Action | Microsoft.DataFactory/factories/datasets/write | Создает или обновляет набор данных. |
> | Action | Microsoft.DataFactory/factories/delete | Удаляет фабрику данных. |
> | Action | Microsoft. делетедатафловдебугсессион/фабрики//действие | Удаляет сеанс отладки потока данных. |
> | Action | Microsoft. жетдатапланеакцесс/фабрики//действие | Получает доступ к службе DataPlane ADF. |
> | Action | Microsoft. жетдатапланеакцесс/фабрики//чтение | Считывает данные о доступе к службе DataPlane ADF. |
> | Action | Microsoft. жетфеатуревалуе/фабрики//действие | Получает значение функции управления раскрытием для конкретного расположения. |
> | Action | Microsoft. жетфеатуревалуе/фабрики//чтение | Считывает значение функции управления раскрытием для конкретного расположения. |
> | Action | Microsoft.DataFactory/factories/getGitHubAccessToken/action | Получает маркер доступа GitHub. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/delete | Удаляет среду выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/getconnectioninfo/read | Считывает сведения о подключении к среде выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/getObjectMetadata/action | Получает метаданные среды выполнения интеграции SSIS для указанной среды Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/getstatus/read | Считывает состояние среды выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/linkedIntegrationRuntime/action | Создает ссылку на связанную Integration Runtime для указанной общей среды Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/listauthkeys/read | Выводит список ключей аутентификации для среды выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/monitoringdata/read | Возвращает данные мониторинга для среды выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/delete | Удаляет узел для указанной среды выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/ipAddress/action | Возвращает IP-адрес для указанного узла среды выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/read | Считывает узел для указанной среды Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/nodes/write | Обновляет узел локальной среды выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/read | Считывает среду выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/refreshObjectMetadata/action | Обновляет метаданные среды выполнения интеграции SSIS для указанной среды Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/regenerateauthkey/action | Повторно создает ключи проверки подлинности для указанной среды выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/removelinks/action | Удаляет связанные ссылки Integration Runtime из указанной среды Integration Runtime. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/start/action | Запускает среду выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/stop/action | Останавливает среду выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/synccredentials/action | Синхронизирует учетные данные для указанной среды выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/upgrade/action | Обновляет указанную среду выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/integrationruntimes/write | Создает или обновляет среду выполнения интеграции. |
> | Action | Microsoft.DataFactory/factories/linkedServices/delete | Удаляет связанную службу. |
> | Action | Microsoft.DataFactory/factories/linkedServices/read | Считывает связанную службу. |
> | Action | Microsoft.DataFactory/factories/linkedServices/write | Создает или обновляет связанную службу. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/activityruns/read | Считывает выполнения действий для указанного идентификатора запуска конвейера. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/cancel/action | Отменяет запуск конвейера, указанный идентификатором запуска. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/action | Запрашивает выполнения действий для указанного идентификатора запуска конвейера. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/queryactivityruns/read | Считывает результат выполнения действий запросов для указанного идентификатора запуска конвейера. |
> | Action | Microsoft.DataFactory/factories/pipelineruns/read | Считывает выполнения конвейера. |
> | Action | Microsoft.DataFactory/factories/pipelines/createrun/action | Создает выполнение для конвейера. |
> | Action | Microsoft.DataFactory/factories/pipelines/delete | Удаляет конвейер. |
> | Action | Microsoft.DataFactory/factories/pipelines/pipelineruns/activityruns/progress/read | Возвращает ход выполнения действий. |
> | Action | Microsoft.DataFactory/factories/pipelines/pipelineruns/read | Считывает выполнение конвейера. |
> | Action | Microsoft.DataFactory/factories/pipelines/read | Считывает конвейер. |
> | Action | Microsoft.DataFactory/factories/pipelines/write | Создает или обновляет конвейер. |
> | Action | Microsoft.DataFactory/factories/querydebugpipelineruns/action | Запрашивает данные о выполнении конвейера отладки. |
> | Action | Microsoft.DataFactory/factories/querypipelineruns/action | Запрашивает выполнения конвейера. |
> | Action | Microsoft.DataFactory/factories/querypipelineruns/read | Считывает результат выполнения конвейера запросов. |
> | Action | Microsoft.DataFactory/factories/querytriggerruns/action | Запрашивает выполнения триггеров. |
> | Action | Microsoft.DataFactory/factories/querytriggerruns/read | Считывает результат запусков триггера. |
> | Action | Microsoft.DataFactory/factories/read | Считывает фабрику данных. |
> | Action | Microsoft. стартдатафловдебугсессион/фабрики//действие | Запускает сеанс отладки потока данных. |
> | Action | Microsoft.DataFactory/factories/triggerruns/read | Считывает выполнения триггеров. |
> | Action | Microsoft.DataFactory/factories/triggers/delete | Удаляет триггер. |
> | Action | Microsoft.DataFactory/factories/triggers/read | Считывает триггер. |
> | Action | Microsoft.DataFactory/factories/triggers/start/action | Запускает триггер. |
> | Action | Microsoft.DataFactory/factories/triggers/stop/action | Останавливает триггер. |
> | Action | Microsoft.DataFactory/factories/triggers/triggerruns/read | Считывает выполнения триггеров. |
> | Action | Microsoft.DataFactory/factories/triggers/write | Создает или обновляет триггер. |
> | Action | Microsoft.DataFactory/factories/write | Создает или обновляет фабрику данных. |
> | Action | Microsoft.DataFactory/locations/configureFactoryRepo/action | Настройка репозитория для производства. |
> | Action | Microsoft.DataFactory/locations/getFeatureValue/action | Получает значение функции управления раскрытием для конкретного расположения. |
> | Action | Microsoft.DataFactory/locations/getFeatureValue/read | Считывает значение функции управления раскрытием для конкретного расположения. |
> | Action | Microsoft.DataFactory/operations/read | Считывает все операции в поставщике Фабрики данных Майкрософт. |
> | Action | Microsoft.DataFactory/register/action | Регистрирует подписку для поставщика ресурсов фабрики данных. |
> | Action | Microsoft.DataFactory/unregister/action | Отменяет регистрацию подписки для поставщика ресурсов фабрики данных. |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DataLakeAnalytics/accounts/computePolicies/delete | Удаление политики вычислений. |
> | Action | Microsoft.DataLakeAnalytics/accounts/computePolicies/read | Получение сведений о политике вычислений. |
> | Action | Microsoft.DataLakeAnalytics/accounts/computePolicies/write | Создание или обновление политики вычислений. |
> | Action | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/delete | Отмена связи учетной записи Data Lake Store с учетной записью Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/read | Получение данных о связанной учетной записи Data Lake Store для учетной записи Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/dataLakeStoreAccounts/write | Создание или обновление связанной учетной записи Data Lake Store для учетной записи Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/delete | Удаление учетной записи Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/firewallRules/delete | Удаляет правило брандмауэра. |
> | Action | Microsoft.DataLakeAnalytics/accounts/firewallRules/read | Возвращает сведения о правиле брандмауэра. |
> | Action | Microsoft.DataLakeAnalytics/accounts/firewallRules/write | Создает или обновляет правило брандмауэра. |
> | Action | Microsoft.DataLakeAnalytics/accounts/operationResults/read | Получение результата операции учетной записи Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/read | Получение сведений о существующей учетной записи Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/listSasTokens/action | Получение списка токенов SAS для контейнеров хранилища учетной записи хранения, связанной с учетной записью Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/Containers/read | Получение контейнеров учетной записи хранения, связанной с учетной записью Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/delete | Удаление связи учетной записи хранения с учетной записью Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/read | Получение сведений об учетной записи хранения, связанной с учетной записью Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/storageAccounts/write | Создание или обновление связанной учетной записи хранения для учетной записи Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/accounts/TakeOwnership/action | Предоставление разрешений на отмену заданий, отправленных другими пользователями. |
> | Action | Microsoft.DataLakeAnalytics/accounts/transferAnalyticsUnits/action | Передача значения SystemMaxAnalyticsUnits для учетных записей DataLakeAnalytics. |
> | Action | Microsoft. Data Lake Analytics/Accounts/Виртуалнетворкрулес/Delete | Удаление правила виртуальной сети. |
> | Action | Microsoft. Data Lake Analytics/Accounts/Виртуалнетворкрулес/Read | Получение сведений о правиле виртуальной сети. |
> | Action | Microsoft. Data Lake Analytics/Accounts/Виртуалнетворкрулес/Write | Создайте или обновите правило виртуальной сети. |
> | Action | Microsoft.DataLakeAnalytics/accounts/write | Создание или обновление учетной записи Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/locations/capability/read | Получение сведений о возможностях подписки в отношении использования Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/locations/checkNameAvailability/action | Проверка доступности имени учетной записи Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/locations/operationResults/read | Получение результата операции учетной записи Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/locations/usages/read | Получение сведений об использовании квот подписки, относящихся к Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/operations/read | Получение доступных операций Data Lake Analytics. |
> | Action | Microsoft.DataLakeAnalytics/register/action | Регистрация подписки для Data Lake Analytics. |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DataLakeStore/accounts/delete | Удаление учетной записи Data Lake Store. |
> | Action | Microsoft.DataLakeStore/accounts/enableKeyVault/action | Включение хранилища ключей для учетной записи Data Lake Store. |
> | Action | Microsoft.DataLakeStore/accounts/eventGridFilters/delete | Удаление фильтра EventGrid. |
> | Action | Microsoft.DataLakeStore/accounts/eventGridFilters/read | Получение фильтра EventGrid. |
> | Action | Microsoft.DataLakeStore/accounts/eventGridFilters/write | Создание или обновление фильтра EventGrid. |
> | Action | Microsoft.DataLakeStore/accounts/firewallRules/delete | Удаляет правило брандмауэра. |
> | Action | Microsoft.DataLakeStore/accounts/firewallRules/read | Возвращает сведения о правиле брандмауэра. |
> | Action | Microsoft.DataLakeStore/accounts/firewallRules/write | Создает или обновляет правило брандмауэра. |
> | Action | Microsoft.DataLakeStore/accounts/operationResults/read | Получение результата операции учетной записи Data Lake Store. |
> | Action | Microsoft.DataLakeStore/accounts/read | Получение сведений о существующей учетной записи Data Lake Store. |
> | Action | Microsoft.DataLakeStore/accounts/Superuser/action; | Предоставление прав суперпользователя в хранилище Data Lake Store при предоставлении разрешения Microsoft.Authorization/roleAssignments/write. |
> | Action | Microsoft.DataLakeStore/accounts/trustedIdProviders/delete | Удаляет доверенный поставщик удостоверений. |
> | Action | Microsoft.DataLakeStore/accounts/trustedIdProviders/read | Возвращает сведения о доверенном поставщике удостоверений. |
> | Action | Microsoft.DataLakeStore/accounts/trustedIdProviders/write | Создает или обновляет доверенный поставщик удостоверений. |
> | Action | Microsoft. Data Lake Store/Accounts/Виртуалнетворкрулес/Delete | Удаление правила виртуальной сети. |
> | Action | Microsoft. Data Lake Store/Accounts/Виртуалнетворкрулес/Read | Получение сведений о правиле виртуальной сети. |
> | Action | Microsoft. Data Lake Store/Accounts/Виртуалнетворкрулес/Write | Создайте или обновите правило виртуальной сети. |
> | Action | Microsoft.DataLakeStore/accounts/write | Создание или обновление учетной записи Data Lake Store. |
> | Action | Microsoft.DataLakeStore/locations/capability/read | Получение сведений о возможностях подписки в отношении использования Data Lake Store. |
> | Action | Microsoft.DataLakeStore/locations/checkNameAvailability/action | Проверка доступности имени учетной записи Data Lake Store. |
> | Action | Microsoft.DataLakeStore/locations/operationResults/read | Получение результата операции учетной записи Data Lake Store. |
> | Action | Microsoft.DataLakeStore/locations/usages/read | Получение сведений об использовании квот подписки, относящихся к Data Lake Store. |
> | Action | Microsoft.DataLakeStore/operations/read | Получение доступных операций Data Lake Store. |
> | Action | Microsoft.DataLakeStore/register/action | Регистрация подписки для Data Lake Store. |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DataMigration/locations/operationResults/read | Получить состояние длительной операции, связанной с ответом 202 (принято) |
> | Action | Microsoft.DataMigration/locations/operationStatuses/read | Получить состояние длительной операции, связанной с ответом 202 (принято) |
> | Action | Microsoft.DataMigration/register/action | Регистрирует подписку в поставщике Azure Database Migration Service |
> | Action | Microsoft. Migration/Services/Аддворкер/действие | Добавляет рабочую роль DMS в список доступных рабочих ролей службы. |
> | Action | Microsoft.DataMigration/services/checkStatus/action | Проверьте, что служба развернута и запущена |
> | Action | Microsoft. Migration/Services/Конфигуреворкер/действие | Настраивает рабочую роль DMS для списка доступных рабочих ролей службы. |
> | Action | Microsoft.DataMigration/services/delete | Удаляет ресурс и все его дочерние элементы |
> | Action | Microsoft.DataMigration/services/projects/accessArtifacts/action | Создать URL-адрес для операций GET и PUT с артефактами проекта |
> | Action | Microsoft.DataMigration/services/projects/delete | Удаляет ресурс и все его дочерние элементы |
> | Action | Microsoft.DataMigration/services/projects/files/delete | Удаляет ресурс и все его дочерние элементы |
> | Action | Microsoft.DataMigration/services/projects/files/read | Просмотр сведений о ресурсах |
> | Action | Microsoft.DataMigration/services/projects/files/read/action | Получение URL-адреса для чтения содержимого файла |
> | Action | Microsoft.DataMigration/services/projects/files/readWrite/action | Получение URL-адреса для чтения или записи содержимого файла |
> | Action | Microsoft.DataMigration/services/projects/files/write | Создание или обновление ресурсов и их свойств |
> | Action | Microsoft.DataMigration/services/projects/read | Просмотр сведений о ресурсах |
> | Action | Microsoft.DataMigration/services/projects/tasks/cancel/action | Отменить задачу, если она выполняется |
> | Action | Microsoft.DataMigration/services/projects/tasks/delete | Удаляет ресурс и все его дочерние элементы |
> | Action | Microsoft.DataMigration/services/projects/tasks/read | Просмотр сведений о ресурсах |
> | Action | Microsoft.DataMigration/services/projects/tasks/write | Запуск задач Azure Database Migration Service |
> | Action | Microsoft.DataMigration/services/projects/write | Запуск задач Azure Database Migration Service |
> | Action | Microsoft.DataMigration/services/read | Просмотр сведений о ресурсах |
> | Action | Microsoft. Migration/Services/Ремовеворкер/действие | Удаляет рабочую роль DMS из списка доступных ролей службы. |
> | Action | Microsoft. Migration/Services/Сервицетаскс/отмена/действие | Отменить задачу, если она выполняется |
> | Action | Microsoft. migrations/Services/Сервицетаскс/Delete | Удаляет ресурс и все его дочерние элементы |
> | Action | Microsoft. Migration, Services/Сервицетаскс/Read | Просмотр сведений о ресурсах |
> | Action | Microsoft. Migration, Services/Сервицетаскс/Write | Запуск задач Azure Database Migration Service |
> | Action | Microsoft.DataMigration/services/slots/delete | Удаляет ресурс и все его дочерние элементы |
> | Action | Microsoft.DataMigration/services/slots/read | Просмотр сведений о ресурсах |
> | Action | Microsoft.DataMigration/services/slots/write | Создание или обновление ресурсов и их свойств |
> | Action | Microsoft.DataMigration/services/start/action | Запустить службу DMS, чтобы она снова могла обрабатывать миграции |
> | Action | Microsoft.DataMigration/services/stop/action | Остановить службу DMS, чтобы свести затраты на нее к минимуму |
> | Action | Microsoft. Migration/Services/Упдатеажентконфиг/действие | Обновляет конфигурацию агента DMS с указанными значениями. |
> | Action | Microsoft.DataMigration/services/write | Создание или обновление ресурсов и их свойств |
> | Action | Microsoft.DataMigration/skus/read | Получить список номеров SKU, поддерживаемых ресурсами DMS. |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Дбформариадб/Чеккнамеаваилабилити/действие | Проверка того, доступно ли данное имя сервера для подготовки по всему миру для данной подписки. |
> | Action | Microsoft. Дбформариадб/Locations/Азуреасинкоператион/Read | Возврат результатов операции сервера MariaDB |
> | Action | Microsoft. Дбформариадб/Locations/Оператионресултс/Read | Вернуть результаты операции сервера MariaDB на основе ResourceGroup |
> | Action | Microsoft. Дбформариадб/Locations/Оператионресултс/Read | Возврат результатов операции сервера MariaDB |
> | Action | Microsoft.DBforMariaDB/locations/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Action | Microsoft. Дбформариадб/Locations/СекуритялертполиЦиесазуреасинкоператион/Read | Возвращает список результатов операции обнаружения угроз сервера. |
> | Action | Microsoft.DBforMariaDB/locations/securityAlertPoliciesOperationResults/read | Возвращает список результатов операции обнаружения угроз сервера. |
> | Action | Microsoft. Дбформариадб/операции/чтение | Возвращает список операций MariaDB. |
> | Action | Microsoft.DBforMariaDB/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Action | Microsoft. Дбформариадб/Register/действие | Регистрация поставщика ресурсов MariaDB |
> | Action | Microsoft. Дбформариадб/Servers/Administrators/Delete | Удаляет существующего администратора сервера MariaDB. |
> | Action | Microsoft. Дбформариадб/Servers/Administrators/Read | Возвращает список администраторов сервера MariaDB. |
> | Action | Microsoft. Дбформариадб/Servers/Administrators/Write | Создает или обновляет администратора сервера MariaDB с указанными параметрами. |
> | Action | Microsoft. Дбформариадб/серверы/Advisor/Креатерекоммендедактионсессион/действие | Создание нового сеанса действия рекомендации |
> | Action | Microsoft. Дбформариадб/серверы/помощники/чтение | Возвращает список консультантов. |
> | Action | Microsoft. Дбформариадб/серверы/помощники/чтение | Возврат помощника |
> | Action | Microsoft. Дбформариадб/серверы/Advisor/Рекоммендедактионс/чтение | Возвращает список рекомендуемых действий |
> | Action | Microsoft. Дбформариадб/серверы/Advisor/Рекоммендедактионс/чтение | Возвращает список рекомендуемых действий |
> | Action | Microsoft. Дбформариадб/серверы/Advisor/Рекоммендедактионс/чтение | Возврат рекомендуемого действия |
> | Action | Microsoft.DBforMariaDB/servers/configurations/read | Возвращает список конфигураций для сервера или получает свойства для указанной конфигурации. |
> | Action | Microsoft.DBforMariaDB/servers/configurations/write | Обновляет значение для указанной конфигурации. |
> | Action | Microsoft. Дбформариадб/Servers/databases/Delete | Удаляет существующую базу данных MariaDB. |
> | Action | Microsoft. Дбформариадб/Servers/databases/Read | Возвращает список баз данных MariaDB или свойства для указанной базы данных. |
> | Action | Microsoft. Дбформариадб/серверы/базы данных/запись | Создает базу данных MariaDB с указанными параметрами или обновляет свойства для указанной базы данных. |
> | Action | Microsoft.DBforMariaDB/servers/delete | Удаление существующего сервера. |
> | Action | Microsoft.DBforMariaDB/servers/firewallRules/delete | Удаление существующего правила брандмауэра. |
> | Action | Microsoft.DBforMariaDB/servers/firewallRules/read | Возвращение списка правил брандмауэра для сервера или получение свойств для указанного правила брандмауэра. |
> | Action | Microsoft.DBforMariaDB/servers/firewallRules/write | Создание правила брандмауэра с указанными параметрами или обновление существующего правила. |
> | Action | Microsoft. Дбформариадб/серверы/файлы журнала/чтение | Возврат списка файлов журнала MariaDB. |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для ресурса. |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/logDefinitions/read | Получает доступные журналы для серверов MariaDB. |
> | Action | Microsoft.DBforMariaDB/servers/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Action | Microsoft. Дбформариадб/Servers/Куеритекстс/Action | Возврат текста для списка запросов |
> | Action | Microsoft. Дбформариадб/Servers/Куеритекстс/Action | Возвращает текст запроса. |
> | Action | Microsoft.DBforMariaDB/servers/read | Возвращение списка серверов или получение свойств для указанного сервера. |
> | Action | Microsoft.DBforMariaDB/servers/recoverableServers/read | Возвращает восстанавливаемые данные сервера MariaDB |
> | Action | Microsoft. Дбформариадб/серверы/реплики/чтение | Получение реплик чтения MariaDB сервера |
> | Action | Microsoft. Дбформариадб/Servers/Restart/Action | Перезапускает указанный сервер. |
> | Action | Microsoft.DBforMariaDB/servers/securityAlertPolicies/read | Извлекает сведения о политике обнаружения угроз, настроенной на данном сервере. |
> | Action | Microsoft.DBforMariaDB/servers/securityAlertPolicies/write | Изменение политики обнаружения угроз для заданного сервера. |
> | Action | Microsoft. Дбформариадб/Servers/Топкуеристатистикс/Read | Возвращает список статистики запросов для наиболее ресурсоемких запросов. |
> | Action | Microsoft. Дбформариадб/Servers/Топкуеристатистикс/Read | Возврат статистики запроса |
> | Action | Microsoft.DBforMariaDB/servers/updateConfigurations/action | Обновляет конфигурации для указанного сервера. |
> | Action | Microsoft.DBforMariaDB/servers/virtualNetworkRules/delete | Удаление существующего правила виртуальной сети. |
> | Action | Microsoft.DBforMariaDB/servers/virtualNetworkRules/read | Возвращение списка правил виртуальной сети или свойств указанного правила виртуальной сети. |
> | Action | Microsoft.DBforMariaDB/servers/virtualNetworkRules/write | Создание правила виртуальной сети с указанными параметрами либо обновление свойств или тегов указанного правила виртуальной сети. |
> | Action | Microsoft. Дбформариадб/Servers/Ваитстатистикс/Read | Возвращает статистику ожидания для экземпляра |
> | Action | Microsoft. Дбформариадб/Servers/Ваитстатистикс/Read | Возврат статистики ожидания |
> | Action | Microsoft.DBforMariaDB/servers/write | Создание сервера с указанными параметрами либо обновление свойств или тегов указанного сервера. |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Дбформискл/Чеккнамеаваилабилити/действие | Проверка того, доступно ли данное имя сервера для подготовки по всему миру для данной подписки. |
> | Action | Microsoft. Дбформискл/Locations/Азуреасинкоператион/Read | Возврат результатов операции MySQL Server |
> | Action | Microsoft.DBforMySQL/locations/operationResults/read | Вернуть результаты операции MySQL на основе ResourceGroup |
> | Action | Microsoft.DBforMySQL/locations/operationResults/read | Возврат результатов операции MySQL Server |
> | Action | Microsoft.DBforMySQL/locations/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Action | Microsoft. Дбформискл/Locations/СекуритялертполиЦиесазуреасинкоператион/Read | Возвращает список результатов операции обнаружения угроз сервера. |
> | Action | Microsoft.DBforMySQL/locations/securityAlertPoliciesOperationResults/read | Возвращает список результатов операции обнаружения угроз сервера. |
> | Action | Microsoft. Дбформискл/операции/чтение | Возврат списка операций MySQL. |
> | Action | Microsoft.DBforMySQL/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Action | Microsoft. Дбформискл/Register/действие | Регистрация поставщика ресурсов MySQL |
> | Action | Microsoft. Дбформискл/Servers/Administrators/Delete | Удаляет существующего администратора сервера MySQL. |
> | Action | Microsoft. Дбформискл/Servers/Administrators/Read | Возвращает список администраторов сервера MySQL. |
> | Action | Microsoft. Дбформискл/Servers/Administrators/Write | Создает или обновляет администратора MySQL Server с указанными параметрами. |
> | Action | Microsoft. Дбформискл/серверы/Advisor/Креатерекоммендедактионсессион/действие | Создание нового сеанса действия рекомендации |
> | Action | Microsoft. Дбформискл/серверы/помощники/чтение | Возвращает список консультантов. |
> | Action | Microsoft. Дбформискл/серверы/помощники/чтение | Возврат помощника |
> | Action | Microsoft. Дбформискл/серверы/Advisor/Рекоммендедактионс/чтение | Возвращает список рекомендуемых действий |
> | Action | Microsoft. Дбформискл/серверы/Advisor/Рекоммендедактионс/чтение | Возвращает список рекомендуемых действий |
> | Action | Microsoft. Дбформискл/серверы/Advisor/Рекоммендедактионс/чтение | Возврат рекомендуемого действия |
> | Action | Microsoft.DBforMySQL/servers/configurations/read | Возвращает список конфигураций для сервера или получает свойства для указанной конфигурации. |
> | Action | Microsoft.DBforMySQL/servers/configurations/write | Обновляет значение для указанной конфигурации. |
> | Action | Microsoft. Дбформискл/Servers/databases/Delete | Удаляет существующую базу данных MySQL. |
> | Action | Microsoft. Дбформискл/Servers/databases/Read | Возврат списка баз данных MySQL или получение свойств для указанной базы данных. |
> | Action | Microsoft. Дбформискл/серверы/базы данных/запись | Создает базу данных MySQL с указанными параметрами или обновляет свойства указанной базы данных. |
> | Action | Microsoft.DBforMySQL/servers/delete | Удаление существующего сервера. |
> | Action | Microsoft.DBforMySQL/servers/firewallRules/delete | Удаление существующего правила брандмауэра. |
> | Action | Microsoft.DBforMySQL/servers/firewallRules/read | Возвращение списка правил брандмауэра для сервера или получение свойств для указанного правила брандмауэра. |
> | Action | Microsoft.DBforMySQL/servers/firewallRules/write | Создание правила брандмауэра с указанными параметрами или обновление существующего правила. |
> | Action | Microsoft. Дбформискл/серверы/файлы журнала/чтение | Возврат списка файлов журнала PostgreSQL. |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для ресурса. |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/logDefinitions/read | Получает доступные журналы для серверов MySQL |
> | Action | Microsoft.DBforMySQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Action | Microsoft. Дбформискл/Servers/Куеритекстс/Action | Возврат текста для списка запросов |
> | Action | Microsoft. Дбформискл/Servers/Куеритекстс/Action | Возвращает текст запроса. |
> | Action | Microsoft.DBforMySQL/servers/read | Возвращение списка серверов или получение свойств для указанного сервера. |
> | Action | Microsoft.DBforMySQL/servers/recoverableServers/read | Возвращение восстанавливаемых данных MySQL Server. |
> | Action | Microsoft. Дбформискл/серверы/реплики/чтение | Получение реплик чтения для сервера MySQL |
> | Action | Microsoft. Дбформискл/Servers/Restart/Action | Перезапускает указанный сервер. |
> | Action | Microsoft.DBforMySQL/servers/securityAlertPolicies/read | Извлекает сведения о политике обнаружения угроз, настроенной на данном сервере. |
> | Action | Microsoft.DBforMySQL/servers/securityAlertPolicies/write | Изменение политики обнаружения угроз для заданного сервера. |
> | Action | Microsoft. Дбформискл/Servers/Топкуеристатистикс/Read | Возвращает список статистики запросов для наиболее ресурсоемких запросов. |
> | Action | Microsoft. Дбформискл/Servers/Топкуеристатистикс/Read | Возврат статистики запроса |
> | Action | Microsoft.DBforMySQL/servers/updateConfigurations/action | Обновляет конфигурации для указанного сервера. |
> | Action | Microsoft.DBforMySQL/servers/virtualNetworkRules/delete | Удаление существующего правила виртуальной сети. |
> | Action | Microsoft.DBforMySQL/servers/virtualNetworkRules/read | Возвращение списка правил виртуальной сети или свойств указанного правила виртуальной сети. |
> | Action | Microsoft.DBforMySQL/servers/virtualNetworkRules/write | Создание правила виртуальной сети с указанными параметрами либо обновление свойств или тегов указанного правила виртуальной сети. |
> | Action | Microsoft. Дбформискл/Servers/Ваитстатистикс/Read | Возвращает статистику ожидания для экземпляра |
> | Action | Microsoft. Дбформискл/Servers/Ваитстатистикс/Read | Возврат статистики ожидания |
> | Action | Microsoft.DBforMySQL/servers/write | Создание сервера с указанными параметрами либо обновление свойств или тегов указанного сервера. |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Дбфорпостгрескл/Чеккнамеаваилабилити/действие | Проверка того, доступно ли данное имя сервера для подготовки по всему миру для данной подписки. |
> | Action | Microsoft. Дбфорпостгрескл/Locations/Азуреасинкоператион/Read | Возврат результатов операции сервера PostgreSQL |
> | Action | Microsoft.DBforPostgreSQL/locations/operationResults/read | Вернуть результаты операции сервера PostgreSQL на основе ResourceGroup |
> | Action | Microsoft.DBforPostgreSQL/locations/operationResults/read | Возврат результатов операции сервера PostgreSQL |
> | Action | Microsoft.DBforPostgreSQL/locations/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Action | Microsoft. Дбфорпостгрескл/Locations/Приватиндпоинтконнектионазуреасинкоператион/Read | Возвращает результат операции подключения к частной конечной точке |
> | Action | Microsoft. Дбфорпостгрескл/Locations/Приватиндпоинтконнектионоператионресултс/Read | Возвращает результат операции подключения к частной конечной точке |
> | Action | Microsoft. Дбфорпостгрескл/Locations/Приватиндпоинтконнектионпроксязуреасинкоператион/Read | Возвращает результат для операции прокси-сервера подключения частной конечной точки |
> | Action | Microsoft. Дбфорпостгрескл/Locations/Приватиндпоинтконнектионпроксйоператионресултс/Read | Возвращает результат для операции прокси-сервера подключения частной конечной точки |
> | Action | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesAzureAsyncOperation/read | Возвращает список результатов операции обнаружения угроз сервера. |
> | Action | Microsoft.DBforPostgreSQL/locations/securityAlertPoliciesOperationResults/read | Возвращает список результатов операции обнаружения угроз сервера. |
> | Action | Microsoft.DBforPostgreSQL/operations/read | Возвращает список операций PostgreSQL. |
> | Action | Microsoft.DBforPostgreSQL/performanceTiers/read | Получение списка доступных уровней производительности. |
> | Action | Microsoft. Дбфорпостгрескл/Register/действие | Регистрация поставщика ресурсов PostgreSQL |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Administrators/Delete | Удаляет существующего администратора сервера PostgreSQL. |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Administrators/Read | Возвращает список администраторов сервера PostgreSQL. |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Administrators/Write | Создает или обновляет администратора сервера PostgreSQL с указанными параметрами. |
> | Action | Microsoft.DBforPostgreSQL/servers/advisors/read | Возвращает список консультантов. |
> | Action | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActions/read | Возвращает список рекомендуемых действий |
> | Action | Microsoft.DBforPostgreSQL/servers/advisors/recommendedActionSessions/action | Предоставление рекомендаций |
> | Action | Microsoft.DBforPostgreSQL/servers/configurations/read | Возвращает список конфигураций для сервера или получает свойства для указанной конфигурации. |
> | Action | Microsoft.DBforPostgreSQL/servers/configurations/write | Обновляет значение для указанной конфигурации. |
> | Action | Microsoft. Дбфорпостгрескл/Servers/databases/Delete | Удаляет существующую базу данных PostgreSQL. |
> | Action | Microsoft. Дбфорпостгрескл/Servers/databases/Read | Возвращает список баз данных PostgreSQL или свойства для указанной базы данных. |
> | Action | Microsoft. Дбфорпостгрескл/серверы/базы данных/запись | Создает базу данных PostgreSQL с указанными параметрами или обновляет свойства для указанной базы данных. |
> | Action | Microsoft.DBforPostgreSQL/servers/delete | Удаление существующего сервера. |
> | Action | Microsoft.DBforPostgreSQL/servers/firewallRules/delete | Удаление существующего правила брандмауэра. |
> | Action | Microsoft.DBforPostgreSQL/servers/firewallRules/read | Возвращение списка правил брандмауэра для сервера или получение свойств для указанного правила брандмауэра. |
> | Action | Microsoft.DBforPostgreSQL/servers/firewallRules/write | Создание правила брандмауэра с указанными параметрами или обновление существующего правила. |
> | Action | Microsoft. Дбфорпостгрескл/серверы/файлы журнала/чтение | Возврат списка файлов журнала PostgreSQL. |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Приватиндпоинтконнектионпроксиес/Delete | Удаляет существующий прокси-сервер подключения частной конечной точки. |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Приватиндпоинтконнектионпроксиес/Read | Возвращает список прокси-серверов подключения частной конечной точки или получает свойства для указанного прокси подключения к частной конечной точке. |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Приватиндпоинтконнектионпроксиес/проверка/действие | Проверяет подключение к частной конечной точке создать вызов с NRP стороны |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Приватиндпоинтконнектионпроксиес/Write | Создает прокси-сервер подключения частной конечной точки с указанными параметрами или обновляет свойства или теги для указанного прокси-сервера подключения частной конечной точки. |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Приватиндпоинтконнектионс/Delete | Удаляет существующее подключение к частной конечной точке |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Приватиндпоинтконнектионс/Read | Возвращает список подключений к частным конечным точкам или возвращает свойства для указанного подключения к частной конечной точке. |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Приватиндпоинтконнектионс/Write | Утверждает или отклоняет существующее подключение к частной конечной точке |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для ресурса. |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/logDefinitions/read | Получает доступные журналы серверов Postgres. |
> | Action | Microsoft.DBforPostgreSQL/servers/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Action | Microsoft.DBforPostgreSQL/servers/queryTexts/action | Возвращает текст запроса. |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Куеритекстс/Read | Возврат текста для списка запросов |
> | Action | Microsoft.DBforPostgreSQL/servers/read | Возвращение списка серверов или получение свойств для указанного сервера. |
> | Action | Microsoft.DBforPostgreSQL/servers/recoverableServers/read | Возвращение восстанавливаемых данных PostgreSQL Server. |
> | Action | Microsoft. Дбфорпостгрескл/серверы/реплики/чтение | Получение реплик чтения PostgreSQL сервера |
> | Action | Microsoft. Дбфорпостгрескл/Servers/Restart/Action | Перезапускает указанный сервер. |
> | Action | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/read | Извлекает сведения о политике обнаружения угроз, настроенной на данном сервере. |
> | Action | Microsoft.DBforPostgreSQL/servers/securityAlertPolicies/write | Изменение политики обнаружения угроз для заданного сервера. |
> | Action | Microsoft.DBforPostgreSQL/servers/topQueryStatistics/read | Возвращает список статистики запросов для наиболее ресурсоемких запросов. |
> | Action | Microsoft.DBforPostgreSQL/servers/updateConfigurations/action | Обновляет конфигурации для указанного сервера. |
> | Action | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/delete | Удаление существующего правила виртуальной сети. |
> | Action | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/read | Возвращение списка правил виртуальной сети или свойств указанного правила виртуальной сети. |
> | Action | Microsoft.DBforPostgreSQL/servers/virtualNetworkRules/write | Создание правила виртуальной сети с указанными параметрами либо обновление свойств или тегов указанного правила виртуальной сети. |
> | Action | Microsoft.DBforPostgreSQL/servers/waitStatistics/read | Возвращает статистику ожидания для экземпляра |
> | Action | Microsoft.DBforPostgreSQL/servers/write | Создание сервера с указанными параметрами либо обновление свойств или тегов указанного сервера. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/конфигурации/чтение | Возвращает список конфигураций для сервера или получает свойства для указанной конфигурации. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/конфигурации/запись | Обновляет значение для указанной конфигурации. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/Delete | Удаление существующего сервера. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/Фиреваллрулес/Delete | Удаление существующего правила брандмауэра. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/Фиреваллрулес/Read | Возвращение списка правил брандмауэра для сервера или получение свойств для указанного правила брандмауэра. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/Фиреваллрулес/запись | Создание правила брандмауэра с указанными параметрами или обновление существующего правила. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/providers/Microsoft. Insights/diagnosticSettings/Read | Получение параметра диагностики для ресурса. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/providers/Microsoft. Insights/diagnosticSettings/запись | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.DBforPostgreSQL/serversv2/providers/Microsoft.Insights/logDefinitions/read | Получает доступные журналы серверов Postgres. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/providers/Microsoft. Insights/metricDefinitions/Read | Возвращает типы метрик, доступных для баз данных. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/Read | Возвращение списка серверов или получение свойств для указанного сервера. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/Упдатеконфигуратионс/Action | Обновляет конфигурации для указанного сервера. |
> | Action | Microsoft. Дбфорпостгрескл/serversv2/запись | Создание сервера с указанными параметрами либо обновление свойств или тегов указанного сервера. |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Devices/checkNameAvailability/Action | Проверяет, доступно ли имя Центра Интернета вещей. |
> | Action | Microsoft.Devices/checkProvisioningServiceNameAvailability/Action | Проверяет, доступно ли имя службы подготовки. |
> | Action | Microsoft.Devices/ElasticPools/diagnosticSettings/read | Получает настройку диагностики для ресурса |
> | Action | Microsoft.Devices/ElasticPools/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Delete | Удаляет сертификат. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/generateVerificationCode/Action | Создание кода проверки. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Read | Возвращает сертификат. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/verify/Action | Проверка ресурса сертификата. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/certificates/Write | Создание или обновление сертификата. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/Delete | Удаление ресурса клиента Центра Интернета вещей. |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/read | Получает настройку диагностики для ресурса |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Delete | Удаляет группу потребителей концентратора событий. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Read | Возвращает группы потребителей концентратора событий. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/eventHubEndpoints/consumerGroups/Write | Создает группу потребителей концентратора событий. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/exportDevices/Action | Экспортирует устройства. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/getStats/Read | Получение ресурса статистики клиента Центра Интернета вещей. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/importDevices/Action | Импортирует устройства. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/iotHubKeys/listkeys/Action | Получение ключа клиента Центра Интернета вещей. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/jobs/Read | Возвращает сведения о заданиях, отправленных в заданный Центр Интернета вещей. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/listKeys/Action | Получение ключей клиента Центра Интернета вещей. |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/logDefinitions/read | Получение доступных определений журналов для службы Центра Интернета вещей. |
> | Action | Microsoft.Devices/ElasticPools/IotHubTenants/metricDefinitions/read | Возвращает доступные метрики для службы Центра Интернета вещей. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/quotaMetrics/Read | Возвращает метрики квоты. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/Read | Получение ресурса клиента Центра Интернета вещей. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testall/Action | Тестирует сообщение с помощью всех существующих маршрутов. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/routing/routes/$testnew/Action | Тестирует сообщение с помощью предоставленного тестового маршрута. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/routingEndpointsHealth/Read | Возвращает сведения о работоспособности всех конечных точек маршрутизации для Центра Интернета вещей. |
> | Action | Microsoft.Devices/elasticPools/iotHubTenants/Write | Создание или обновление ресурса клиента Центра Интернета вещей. |
> | Action | Microsoft.Devices/ElasticPools/metricDefinitions/read | Возвращает доступные метрики для службы Центра Интернета вещей. |
> | Action | Microsoft.Devices/iotHubs/certificates/Delete | Удаляет сертификат. |
> | Action | Microsoft.Devices/iotHubs/certificates/generateVerificationCode/Action | Создание кода проверки. |
> | Action | Microsoft.Devices/iotHubs/certificates/Read | Возвращает сертификат. |
> | Action | Microsoft.Devices/iotHubs/certificates/verify/Action | Проверка ресурса сертификата. |
> | Action | Microsoft.Devices/iotHubs/certificates/Write | Создание или обновление сертификата. |
> | Action | Microsoft.Devices/iotHubs/Delete | Удаляет ресурс Центра Интернета вещей. |
> | Action | Microsoft.Devices/IotHubs/diagnosticSettings/read | Получает настройку диагностики для ресурса |
> | Action | Microsoft.Devices/IotHubs/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.Devices/iotHubs/eventGridFilters/Delete | Удаление фильтра сетки событий. |
> | Action | Microsoft.Devices/iotHubs/eventGridFilters/Read | Получение фильтра сетки событий. |
> | Action | Microsoft.Devices/iotHubs/eventGridFilters/Write | Создание фильтра сетки событий или обновление существующего. |
> | Action | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Delete | Удаляет группу потребителей концентратора событий. |
> | Action | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Read | Возвращает группы потребителей концентратора событий. |
> | Action | Microsoft.Devices/iotHubs/eventHubEndpoints/consumerGroups/Write | Создает группу потребителей концентратора событий. |
> | Action | Microsoft.Devices/iotHubs/exportDevices/Action | Экспортирует устройства. |
> | Action | Microsoft.Devices/iotHubs/importDevices/Action | Импортирует устройства. |
> | Action | Microsoft.Devices/iotHubs/iotHubKeys/listkeys/Action | Возвращает ключ Центра Интернета вещей для указанного имени. |
> | Action | Microsoft.Devices/iotHubs/iotHubStats/Read | Возвращает статистику Центра Интернета вещей. |
> | Action | Microsoft.Devices/iotHubs/jobs/Read | Возвращает сведения о заданиях, отправленных в заданный Центр Интернета вещей. |
> | Action | Microsoft.Devices/iotHubs/listkeys/Action | Возвращает все ключи Центра Интернета вещей. |
> | Action | Microsoft.Devices/IotHubs/logDefinitions/read | Получение доступных определений журналов для службы Центра Интернета вещей. |
> | Action | Microsoft.Devices/IotHubs/metricDefinitions/read | Возвращает доступные метрики для службы Центра Интернета вещей. |
> | Action | Microsoft.Devices/iotHubs/operationresults/Read | Получение результата операции (устаревший API). |
> | Action | Microsoft.Devices/iotHubs/quotaMetrics/Read | Возвращает метрики квоты. |
> | Action | Microsoft.Devices/iotHubs/Read | Возвращает ресурсы Центра Интернета вещей. |
> | Action | Microsoft.Devices/iotHubs/routing/$testall/Action | Тестирует сообщение с помощью всех существующих маршрутов. |
> | Action | Microsoft.Devices/iotHubs/routing/$testnew/Action | Тестирует сообщение с помощью предоставленного тестового маршрута. |
> | Action | Microsoft.Devices/iotHubs/routingEndpointsHealth/Read | Возвращает сведения о работоспособности всех конечных точек маршрутизации для Центра Интернета вещей. |
> | Action | Microsoft.Devices/iotHubs/skus/Read | Возвращает допустимые номера SKU Центра Интернета вещей. |
> | Action | Microsoft.Devices/iotHubs/Write | Создает или обновляет ресурс Центра Интернета вещей. |
> | Action | Microsoft.Devices/locations/operationresults/Read | Получает результат операции на основе расположения. |
> | Action | Microsoft.Devices/operationresults/Read | Получить результат операции |
> | Action | Microsoft.Devices/operations/Read | Возвращает все операции поставщика ресурсов. |
> | Action | Microsoft.Devices/provisioningServices/certificates/Delete | Удаляет сертификат. |
> | Action | Microsoft.Devices/provisioningServices/certificates/generateVerificationCode/Action | Создание кода проверки. |
> | Action | Microsoft.Devices/provisioningServices/certificates/Read | Возвращает сертификат. |
> | Action | Microsoft.Devices/provisioningServices/certificates/verify/Action | Проверка ресурса сертификата. |
> | Action | Microsoft.Devices/provisioningServices/certificates/Write | Создание или обновление сертификата. |
> | Action | Microsoft.Devices/provisioningServices/Delete | Удаление ресурса службы политики диагностики (DPS) Центра Интернета вещей. |
> | Action | Microsoft.Devices/provisioningServices/diagnosticSettings/read | Получает настройку диагностики для ресурса |
> | Action | Microsoft.Devices/provisioningServices/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.Devices/provisioningServices/keys/listkeys/Action | Получение ключей DPS Центра Интернета вещей для имени ключа. |
> | Action | Microsoft.Devices/provisioningServices/listkeys/Action | Получение ключей DPS Центра Интернета вещей. |
> | Action | Microsoft.Devices/provisioningServices/logDefinitions/read | Получение доступных определений журналов для службы подготовки. |
> | Action | Microsoft.Devices/provisioningServices/metricDefinitions/read | Получение доступных метрик для службы подготовки. |
> | Action | Microsoft.Devices/provisioningServices/operationresults/Read | Получение результата операции DPS. |
> | Action | Microsoft.Devices/provisioningServices/Read | Получение ресурса DPS Центра Интернета вещей. |
> | Action | Microsoft.Devices/provisioningServices/skus/Read | Получение действительных номеров SKU для DPS Центра Интернета вещей. |
> | Action | Microsoft.Devices/provisioningServices/Write | Создание ресурса DPS для Центра Интернета вещей. |
> | Action | Microsoft.Devices/register/action | Регистрирует подписку в поставщике ресурсов Центра Интернета вещей и позволяет создавать ресурсы Центра Интернета вещей. |
> | Action | Microsoft.Devices/register/action | Регистрирует подписку в поставщике ресурсов Центра Интернета вещей и позволяет создавать ресурсы Центра Интернета вещей. |
> | Action | Microsoft.Devices/usages/Read | Возвращает данные об использовании подписки для данного поставщика. |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DevSpaces/controllers/delete | Удаление контроллера Azure Dev Spaces Controller и служб плоскости данных |
> | Action | Microsoft.DevSpaces/controllers/listConnectionDetails/action | Сведения о подключении списка для инфраструктуры контроллера Azure Dev Spaces |
> | Action | Microsoft.DevSpaces/controllers/read | Чтение свойств контроллера Azure Dev Spaces |
> | Action | Microsoft.DevSpaces/controllers/write | Создание или обновление свойств контроллера Azure Dev Spaces |
> | Action | Microsoft. Девспацес/Locations/Чеккконтаинерхостмаппинг/Action | Проверка существующего сопоставления контроллера для узла контейнера |
> | Action | Microsoft. Девспацес/Locations/оператионресултс/Read | Чтение состояния асинхронной операции |
> | Action | Microsoft.DevSpaces/register/action | Создание поставщика ресурсов Microsoft Dev Spaces с подпиской |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DevTestLab/labCenters/delete | Удаление центров лабораторий. |
> | Action | Microsoft.DevTestLab/labCenters/read | Чтение центров лабораторий. |
> | Action | Microsoft.DevTestLab/labCenters/write | Добавление или изменение центров лабораторий. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/armTemplates/read | Считывает шаблоны Azure Resource Manager. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/artifacts/GenerateArmTemplate/action | Создает шаблон ARM для заданного артефакта, передает необходимые файлы в учетную запись хранения и проверяет созданный артефакт. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/artifacts/read | Считывает артефакты. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/delete | Удаляет источники артефактов. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/read | Считывает источники артефактов. |
> | Action | Microsoft.DevTestLab/labs/artifactSources/write | Добавляет или изменяет источники артефактов. |
> | Action | Microsoft.DevTestLab/labs/ClaimAnyVm/action | Отправляет заявку на доступ к случайной запрашиваемой виртуальной машине в лаборатории. |
> | Action | Microsoft.DevTestLab/labs/costs/read | Считывает затраты. |
> | Action | Microsoft.DevTestLab/labs/costs/write | Добавляет или изменяет затраты. |
> | Action | Microsoft.DevTestLab/labs/CreateEnvironment/action | Создает виртуальные машины в лаборатории. |
> | Action | Microsoft.DevTestLab/labs/customImages/delete | Удаляет пользовательские образы. |
> | Action | Microsoft.DevTestLab/labs/customImages/read | Считывает пользовательские образы. |
> | Action | Microsoft.DevTestLab/labs/customImages/write | Добавляет или изменяет пользовательские образы. |
> | Action | Microsoft.DevTestLab/labs/delete | Удаляет лаборатории. |
> | Action | Microsoft. DevTestLab/Labs/Енсурекуррентусерпрофиле/действие | Убедитесь, что текущий пользователь имеет допустимый профиль в лаборатории. |
> | Action | Microsoft.DevTestLab/labs/ExportResourceUsage/action | Экспортирует данные об использовании ресурсов лаборатории в учетную запись хранения. |
> | Action | Microsoft.DevTestLab/labs/formulas/delete | Удаляет формулы. |
> | Action | Microsoft.DevTestLab/labs/formulas/read | Считывает формулы. |
> | Action | Microsoft.DevTestLab/labs/formulas/write | Добавляет или изменяет формулы. |
> | Action | Microsoft.DevTestLab/labs/galleryImages/read | Считывает образы из коллекции. |
> | Action | Microsoft.DevTestLab/labs/GenerateUploadUri/action | Создает универсальный код ресурса (URI) для передачи настраиваемых образов дисков в лабораторию. |
> | Action | Microsoft. DevTestLab/Labs/Идлешутдовнс/удаление | Удаление бездействующих завершений. |
> | Action | Microsoft. DevTestLab/Labs/Идлешутдовнс/чтение | Чтение бездействующих завершений. |
> | Action | Microsoft. DevTestLab/Labs/Идлешутдовнс/запись | Добавление или изменение бездействующих завершений. |
> | Action | Microsoft.DevTestLab/labs/ImportVirtualMachine/action | Импорт виртуальной машины в другую лабораторию. |
> | Action | Microsoft.DevTestLab/labs/ListVhds/action | Выводит список образов дисков, доступных для создания пользовательского образа. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/delete | Удаляет каналы уведомлений. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/Notify/action | Отправляет уведомление в указанный канал. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/read | Считывает каналы уведомлений. |
> | Action | Microsoft.DevTestLab/labs/notificationChannels/write | Добавляет или изменяет каналы уведомлений. |
> | Action | Microsoft.DevTestLab/labs/policySets/EvaluatePolicies/action | Оценивает политику лаборатории. |
> | Action | Microsoft.DevTestLab/labs/policySets/policies/delete | Удаляет политики. |
> | Action | Microsoft.DevTestLab/labs/policySets/policies/read | Считывает политики. |
> | Action | Microsoft.DevTestLab/labs/policySets/policies/write | Добавляет или изменяет политики. |
> | Action | Microsoft.DevTestLab/labs/policySets/read | Чтение наборов политик. |
> | Action | Microsoft.DevTestLab/labs/read | Считывает лаборатории. |
> | Action | Microsoft.DevTestLab/labs/schedules/delete | Удаляет расписания. |
> | Action | Microsoft.DevTestLab/labs/schedules/Execute/action | Выполняет расписание. |
> | Action | Microsoft.DevTestLab/labs/schedules/ListApplicable/action | Выводит список всех применимых расписаний. |
> | Action | Microsoft.DevTestLab/labs/schedules/read | Считывает расписания. |
> | Action | Microsoft.DevTestLab/labs/schedules/write | Добавляет или изменяет расписания. |
> | Action | Microsoft.DevTestLab/labs/serviceRunners/delete | Удаляет средства выполнения служб. |
> | Action | Microsoft.DevTestLab/labs/serviceRunners/read | Считывает средства выполнения служб. |
> | Action | Microsoft.DevTestLab/labs/serviceRunners/write | Добавляет или изменяет средства выполнения служб. |
> | Action | Microsoft. DevTestLab/Labs/Шаредгаллериес/удаление | Удаление общих галерей. |
> | Action | Microsoft. DevTestLab/Labs/Шаредгаллериес/чтение | Чтение общих галерей. |
> | Action | Microsoft. DevTestLab/Labs/Шаредгаллериес/Шаредимажес/удалить | Удаление общих образов. |
> | Action | Microsoft. DevTestLab/Labs/Шаредгаллериес/Шаредимажес/чтение | Чтение общих образов. |
> | Action | Microsoft. DevTestLab/Labs/Шаредгаллериес/Шаредимажес/запись | Добавление или изменение общих образов. |
> | Action | Microsoft. DevTestLab/Labs/Шаредгаллериес/запись | Добавление или изменение общих галерей. |
> | Action | Microsoft.DevTestLab/labs/users/delete | Удаляет профили пользователей. |
> | Action | Microsoft.DevTestLab/labs/users/disks/Attach/action | Подключает диск к виртуальной машине и создает аренду для него. |
> | Action | Microsoft.DevTestLab/labs/users/disks/delete | Удаляет диски. |
> | Action | Microsoft.DevTestLab/labs/users/disks/Detach/action | Отключает диск от виртуальной машины и прерывает его аренду. |
> | Action | Microsoft.DevTestLab/labs/users/disks/read | Считывает диски. |
> | Action | Microsoft.DevTestLab/labs/users/disks/write | Добавляет или изменяет диски. |
> | Action | Microsoft.DevTestLab/labs/users/environments/delete | Удаляет среды. |
> | Action | Microsoft.DevTestLab/labs/users/environments/read | Считывает среды. |
> | Action | Microsoft.DevTestLab/labs/users/environments/write | Добавляет или изменяет среды. |
> | Action | Microsoft.DevTestLab/labs/users/read | Считывает профили пользователей. |
> | Action | Microsoft.DevTestLab/labs/users/secrets/delete | Удаляет секреты. |
> | Action | Microsoft.DevTestLab/labs/users/secrets/read | Считывает секреты. |
> | Action | Microsoft.DevTestLab/labs/users/secrets/write | Добавляет или изменяет секреты. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/delete | Удаление платформ Service Fabric. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/ListApplicableSchedules/action | Выводит список применимых расписаний запуска и остановки, если они есть. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/read | Чтение платформ Service Fabric. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/delete | Удаляет расписания. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/Execute/action | Выполняет расписание. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/read | Считывает расписания. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/schedules/write | Добавляет или изменяет расписания. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/Start/action | Запуск платформы Service Fabric. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/Stop/action | Остановка платформы Service Fabric. |
> | Action | Microsoft.DevTestLab/labs/users/serviceFabrics/write | Добавление или изменение платформ Service Fabric. |
> | Action | Microsoft.DevTestLab/labs/users/write | Добавляет или изменяет профили пользователей. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/AddDataDisk/action | Подключает новый или существующий диск данных к виртуальной машине. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/ApplyArtifacts/action | Применяет артефакты к виртуальной машине. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Claim/action | Присваивает владение существующей виртуальной машиной. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/delete | Удаляет виртуальные машины. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/DetachDataDisk/action | Отключает указанный диск от виртуальной машины. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/GetRdpFileContents/action | Получает строку, которая представляет содержание RDP-файла для виртуальной машины. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/ListApplicableSchedules/action | Выводит список применимых расписаний запуска и остановки, если они есть. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/read | Считывает виртуальные машины. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Redeploy/action | Повторное развертывание виртуальной машины. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Resize/action | Изменение размера виртуальной машины. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Restart/action | Перезапуск виртуальной машины. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/delete | Удаляет расписания. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/Execute/action | Выполняет расписание. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/read | Считывает расписания. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/schedules/write | Добавляет или изменяет расписания. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Start/action | Запускает виртуальную машину. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/Stop/action | Остановка виртуальной машины |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/TransferDisks/action | Переносит все диски с данными, подключенные к виртуальной машине, к текущему пользователю. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/UnClaim/action | Отказ от владения существующей виртуальной машиной. |
> | Action | Microsoft.DevTestLab/labs/virtualMachines/write | Добавление или изменение виртуальных машин. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/delete | Удаляет виртуальные сети. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/read | Считывает виртуальные сети. |
> | Action | Microsoft.DevTestLab/labs/virtualNetworks/write | Добавляет или изменяет виртуальные сети. |
> | Action | Microsoft.DevTestLab/labs/vmPools/delete | Удаление пулов виртуальных машин. |
> | Action | Microsoft.DevTestLab/labs/vmPools/read | Считывание пулов виртуальных машин. |
> | Action | Microsoft.DevTestLab/labs/vmPools/write | Добавление или изменение пулов виртуальных машин. |
> | Action | Microsoft.DevTestLab/labs/write | Добавляет или изменяет лаборатории. |
> | Action | Microsoft.DevTestLab/locations/operations/read | Считывает операции. |
> | Action | Microsoft.DevTestLab/register/action | Регистрация подписки |
> | Action | Microsoft.DevTestLab/schedules/delete | Удаляет расписания. |
> | Action | Microsoft.DevTestLab/schedules/Execute/action | Выполняет расписание. |
> | Action | Microsoft.DevTestLab/schedules/read | Считывает расписания. |
> | Action | Microsoft.DevTestLab/schedules/Retarget/action | Обновляет идентификатор целевого ресурса расписания. |
> | Action | Microsoft.DevTestLab/schedules/write | Добавляет или изменяет расписания. |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DocumentDB/databaseAccountNames/read | Проверяет доступность имени. |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/коллекции/удаление | Удаляет коллекцию. Применимо только к типам API: "MongoDB". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/коллекции/Оператионресултс/чтение | Чтение состояния асинхронной операции. Применимо только к типам API: "MongoDB". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/коллекции/чтение | Считывает коллекцию или список всех коллекций. Применимо только к типам API: "MongoDB". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/databases/Collections/Settings/Оператионресултс/Read | Чтение состояния асинхронной операции. Применимо только к типам API: "MongoDB". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных, коллекции, параметры/чтение | Считывает пропускную способность коллекции. Применимо только к типам API: "MongoDB". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/коллекции/параметры/запись | Обновите пропускную способность коллекции. Применимо только к типам API: "MongoDB". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/коллекции/запись | Создание или обновление коллекции. Применимо только к типам API: "MongoDB". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных, контейнеры или удаление | Удаление контейнера. Применимо только к типам API: "SQL". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/контейнеры/Оператионресултс/чтение | Чтение состояния асинхронной операции. Применимо только к типам API: "SQL". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных и контейнеры/чтение | Чтение контейнера или перечисление всех контейнеров. Применимо только к типам API: "SQL". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных, контейнеры, параметры, Оператионресултс/чтение | Чтение состояния асинхронной операции. Применимо только к типам API: "SQL". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных, контейнеры, параметры/чтение | Считывает пропускную способность контейнера. Применимо только к типам API: "SQL". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных, контейнеры, параметры/запись | Обновите пропускную способность контейнера. Применимо только к типам API: "SQL". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных и контейнеры/запись | Создание или обновление контейнера. Применимо только к типам API: "SQL". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/удаление | Удаление базы данных. Применимо только к типам API: "SQL", "MongoDB", "гремлин'". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных, графы и удаление | Удаление диаграммы. Применимо только к типам API: "гремлин'". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/графы/Оператионресултс/чтение | Чтение состояния асинхронной операции. Применимо только к типам API: "гремлин'". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/графы/чтение | Чтение диаграммы или составление списка всех диаграмм. Применимо только к типам API: "гремлин'". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных, графы, параметры, Оператионресултс/чтение | Чтение состояния асинхронной операции. Применимо только к типам API: "гремлин'". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных, графы, параметры/чтение | Считывает пропускную способность графа. Применимо только к типам API: "гремлин'". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных, графы, параметры/запись | Обновите пропускную способность графа. Применимо только к типам API: "гремлин'". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/графики/запись | Создание или обновление графа. Применимо только к типам API: "гремлин'". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/databases/Оператионресултс/Read | Чтение состояния асинхронной операции. Применимо только к типам API: "SQL", "MongoDB", "гремлин'". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/чтение | Чтение базы данных или вывод списка всех баз данных. Применимо только к типам API: "SQL", "MongoDB", "гремлин'". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/параметры/Оператионресултс/чтение | Чтение состояния асинхронной операции. Применимо только к типам API: "SQL", "MongoDB", "гремлин'". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/параметры/чтение | Считывает пропускную способность базы данных. Применимо только к типам API: "SQL", "MongoDB", "гремлин'". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/параметры/запись | Обновите пропускную способность базы данных. Применимо только к типам API: "SQL", "MongoDB", "гремлин'". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/базы данных/запись | создавать базу данных; Применимо только к типам API: "SQL", "MongoDB", "гремлин'". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/Delete | Удаление пространства ключей. Применимо только к типам API: "Cassandra". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/Оператионресултс/Read | Чтение состояния асинхронной операции. Применимо только к типам API: "Cassandra". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/Read | Прочтите пространства ключей или перечислите все пространств ключей. Применимо только к типам API: "Cassandra". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/Settings/Оператионресултс/Read | Чтение состояния асинхронной операции. Применимо только к типам API: "Cassandra". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/параметры/чтение | Прочтите пропускную способность пространства ключей. Применимо только к типам API: "Cassandra". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/параметры/запись | Обновление пропускной способности пространства ключей. Применимо только к типам API: "Cassandra". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/Tables/Delete | Удаление таблицы. Применимо только к типам API: "Cassandra". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/Tables/Оператионресултс/Read | Чтение состояния асинхронной операции. Применимо только к типам API: "Cassandra". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/Tables/Read | Считывает таблицу или список всех таблиц. Применимо только к типам API: "Cassandra". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/Tables/Settings/Оператионресултс/Read | Чтение состояния асинхронной операции. Применимо только к типам API: "Cassandra". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/таблицы/параметры/чтение | Чтение пропускной способности таблицы. Применимо только к типам API: "Cassandra". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/таблицы/параметры/запись | Обновите пропускную способность таблицы. Применимо только к типам API: "Cassandra". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/Tables/Write | Создание или обновление таблицы. Применимо только к типам API: "Cassandra". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/пространств ключей/Write | Создайте пространства ключей. Применимо только к типам API: "Cassandra". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/таблицы/удаление | Удаление таблицы. Применимо только к типам API: "Table". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/Tables/Оператионресултс/Read | Чтение состояния асинхронной операции. Применимо только к типам API: "Table". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/таблицы/чтение | Считывает таблицу или список всех таблиц. Применимо только к типам API: "Table". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/Tables/Settings/Оператионресултс/Read | Чтение состояния асинхронной операции. Применимо только к типам API: "Table". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/таблицы/параметры/чтение | Чтение пропускной способности таблицы. Применимо только к типам API: "Table". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/таблицы/параметры/запись | Обновите пропускную способность таблицы. Применимо только к типам API: "Table". Применимо только к типам параметров: "пропускная способность". |
> | Action | Microsoft. DocumentDB/databaseAccounts/API/таблицы/запись | Создание или обновление таблицы. Применимо только к типам API: "Table". |
> | Action | Microsoft.DocumentDB/databaseAccounts/backup/action | Отправка запроса на настройку резервного копирования |
> | Action | Microsoft.DocumentDB/databaseAccounts/changeResourceGroup/action | Изменяет группу ресурсов для учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/metricDefinitions/read | Считывает определения метрик коллекции. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/metrics/read | Считывает метрики коллекции. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitionKeyRangeId/metrics/read | Чтение метрик уровня ключа раздела для учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/metrics/read | Чтение метрик уровня раздела для учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/read | Чтение разделов учетной записи базы данных в коллекции. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/partitions/usages/read | Чтение сведений об использовании уровня раздела для учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/collections/usages/read | Считывает данные об использовании коллекции. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/metricDefinitions/read | Считывает определения метрик базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/metrics/read | Считывает метрики базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/databases/usages/read | Считывает данные об использовании базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/delete | Удаляет учетные записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/failoverPriorityChange/action | Изменяет приоритеты отработки отказа для регионов учетной записи базы данных. Используется для выполнения операции отработки отказа вручную. |
> | Action | Microsoft. DocumentDB/databaseAccounts/Жетбаккупполици/действие | Получение политики резервного копирования учетной записи базы данных |
> | Action | Microsoft.DocumentDB/databaseAccounts/listConnectionStrings/action | Возвращает строки подключения для учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/listKeys/action | Выводит список ключей учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/metricDefinitions/read | Считывает определения метрик учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/metrics/read | Считывает метрики учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/offlineRegion/action | Отключение региона учетной записи базы данных.  |
> | Action | Microsoft.DocumentDB/databaseAccounts/onlineRegion/action | Подключение региона учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/operationResults/read | Чтение состояния асинхронной операции. |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/metrics/read | Чтение метрик задержки. |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/read | Чтение процентилей задержек репликации. |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/sourceRegion/targetRegion/metrics/read | Чтение метрик задержки для определенного источника и целевого региона. |
> | Action | Microsoft.DocumentDB/databaseAccounts/percentile/targetRegion/metrics/read | Чтение метрик задержки для определенного целевого региона. |
> | Action | Microsoft. DocumentDB/databaseAccounts/Приватиндпоинтконнектионпроксиес/Delete | Удаление прокси-сервера подключения частной конечной точки учетной записи базы данных |
> | Action | Microsoft. DocumentDB/databaseAccounts/Приватиндпоинтконнектионпроксиес/Read | Чтение прокси подключения частной конечной точки учетной записи базы данных |
> | Action | Microsoft. DocumentDB/databaseAccounts/Приватиндпоинтконнектионпроксиес/проверка/действие | Проверка прокси-сервера подключения частной конечной точки учетной записи базы данных |
> | Action | Microsoft. DocumentDB/databaseAccounts/Приватиндпоинтконнектионпроксиес/запись | Создание или обновление прокси-сервера подключения к частной конечной точке учетной записи базы данных |
> | Action | Microsoft.DocumentDB/databaseAccounts/read | Считывает учетную запись базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/readonlykeys/action | Считывает ключи только для чтения учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/readonlykeys/read | Считывает ключи только для чтения учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/regenerateKey/action | Выполняет ротацию ключей учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/metrics/read | Чтение метрик региональной коллекции. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitionKeyRangeId/metrics/read | Чтение метрик уровня ключа раздела для учетной записи региональной базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/metrics/read | Чтение метрик уровня раздела для учетной записи региональной базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/databases/collections/partitions/read | Чтение разделов учетной записи региональной базы данных в коллекции. |
> | Action | Microsoft.DocumentDB/databaseAccounts/region/metrics/read | Чтение метрик учетной записи базы данных и региона. |
> | Action | Microsoft.DocumentDB/databaseAccounts/restore/action | Отправка запроса восстановления |
> | Action | Microsoft.DocumentDB/databaseAccounts/usages/read | Считывает данные об использовании учетной записи базы данных. |
> | Action | Microsoft.DocumentDB/databaseAccounts/write | Обновляет учетные записи базы данных. |
> | Action | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/action | Уведомление Microsoft.DocumentDB о том, что идет удаление виртуальной сети или подсети. |
> | Action | Microsoft.DocumentDB/locations/deleteVirtualNetworkOrSubnets/operationResults/read | Чтение состояния асинхронной операции удаления виртуальной сети или подсетей. |
> | Action | Microsoft. DocumentDB/Locations/Оператионсстатус/Read | Чтение состояния асинхронных операций |
> | Action | Microsoft.DocumentDB/operationResults/read | Чтение состояния асинхронной операции. |
> | Action | Microsoft.DocumentDB/operations/read | Чтение операций, доступных для Microsoft DocumentDB.  |
> | Action | Microsoft.DocumentDB/register/action |  Регистрация поставщика ресурсов Microsoft DocumentDB для подписки. |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.DomainRegistration/checkDomainAvailability/Action | Проверяет, доступен ли домен для приобретения. |
> | Action | Microsoft.DomainRegistration/domains/Delete | Удаляет существующий домен. |
> | Action | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Delete | Удаление идентификатора владения. |
> | Action | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Вывод списка идентификаторов владения. |
> | Action | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Read | Получение идентификатора владения. |
> | Action | Microsoft.DomainRegistration/domains/domainownershipidentifiers/Write | Создание или обновление идентификатора. |
> | Action | Microsoft.DomainRegistration/domains/operationresults/Read | Возвращает операцию домена. |
> | Action | Microsoft.DomainRegistration/domains/Read | Возвращает список доменов. |
> | Action | Microsoft.DomainRegistration/domains/Read | Получение домена. |
> | Action | Microsoft.DomainRegistration/domains/renew/Action | Обновление существующего домена. |
> | Action | Microsoft.DomainRegistration/domains/Write | Добавляет новый домен или обновляет существующий. |
> | Action | Microsoft.DomainRegistration/generateSsoRequest/Action | Создает запрос на вход в центр управления доменами. |
> | Action | Microsoft.DomainRegistration/listDomainRecommendations/Action | Извлекает список рекомендуемых доменов на основе ключевых слов. |
> | Action | Microsoft.DomainRegistration/operations/Read | Вывод списка всех операций из регистрации домена службы приложений. |
> | Action | Microsoft.DomainRegistration/register/action | Регистрирует поставщик ресурсов доменов Майкрософт для подписки. |
> | Action | Microsoft.DomainRegistration/topLevelDomains/listAgreements/Action | Вывод списка соглашений. |
> | Action | Microsoft.DomainRegistration/topLevelDomains/Read | Получение доменов верхнего уровня. |
> | Action | Microsoft.DomainRegistration/topLevelDomains/Read | Получение домена верхнего уровня. |
> | Action | Microsoft.DomainRegistration/validateDomainRegistrationInformation/Action | Проверяет объект покупки домена без его отправки. |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.EventGrid/domains/delete | Удаление домена |
> | Action | Microsoft.EventGrid/domains/listKeys/action | Получение списка ключей для домена |
> | Action | Microsoft.EventGrid/domains/providers/Microsoft.Insights/metricDefinitions/read | Получает доступные метрики для доменов |
> | Action | Microsoft.EventGrid/domains/read | Чтение домена |
> | Action | Microsoft.EventGrid/domains/regenerateKey/action | Повторное создание ключа для домена |
> | Action | Microsoft.EventGrid/domains/topics/read | Чтение раздела домена |
> | Action | Microsoft.EventGrid/domains/write | Создание или обновление домена |
> | Action | Microsoft.EventGrid/eventSubscriptions/delete | Удаление подписки |
> | Action | Microsoft.EventGrid/eventSubscriptions/getFullUrl/action | Получение полного URL-адреса подписки на события. |
> | Action | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для подписок на события. |
> | Action | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/diagnosticSettings/write | Получение или обновление параметра диагностики для подписок на события. |
> | Action | Microsoft.EventGrid/eventSubscriptions/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для подписок на события. |
> | Action | Microsoft.EventGrid/eventSubscriptions/read | Чтение подписки |
> | Action | Microsoft.EventGrid/eventSubscriptions/write | Создание или обновление подписки |
> | Action | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для разделов. |
> | Action | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметра диагностики для разделов. |
> | Action | Microsoft.EventGrid/extensionTopics/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для разделов. |
> | Action | Microsoft.EventGrid/extensionTopics/read | Чтение extensionTopic. |
> | Action | Microsoft.EventGrid/locations/eventSubscriptions/read | Создание списка подписок на события в регионе |
> | Action | Microsoft.EventGrid/locations/operationResults/read | Чтение результата региональной операции |
> | Action | Microsoft.EventGrid/locations/operationsStatus/read | Чтение состояния региональной операции |
> | Action | Microsoft.EventGrid/locations/topictypes/eventSubscriptions/read | Создание списка подписок на события в регионе по типу темы |
> | Action | Microsoft.EventGrid/operationResults/read | Чтение результата операции |
> | Action | Microsoft.EventGrid/operations/read | Вывод списка операций EventGrid. |
> | Action | Microsoft.EventGrid/operationsStatus/read | Чтение состояния операции |
> | Action | Microsoft.EventGrid/register/action | Регистрирует подписку для поставщика ресурсов EventGrid. |
> | Action | Microsoft.EventGrid/topics/delete | Удаление раздела |
> | Action | Microsoft.EventGrid/topics/listKeys/action | Получение списка ключей для раздела |
> | Action | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для разделов. |
> | Action | Microsoft.EventGrid/topics/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметра диагностики для разделов. |
> | Action | Microsoft.EventGrid/topics/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для разделов. |
> | Action | Microsoft.EventGrid/topics/read | Чтение раздела. |
> | Action | Microsoft.EventGrid/topics/regenerateKey/action | Повторное создание ключа для раздела |
> | Action | Microsoft.EventGrid/topics/write | Создание или обновление раздела. |
> | Action | Microsoft.EventGrid/topictypes/eventSubscriptions/read | Создание списка подписок на глобальные события по типу темы |
> | Action | Microsoft.EventGrid/topictypes/eventtypes/read | Чтение типов eventtype, поддерживаемых topictype |
> | Action | Microsoft.EventGrid/topictypes/read | Чтение типа раздела (topictype) |
> | Action | Microsoft.EventGrid/unregister/action | Отмена регистрации подписки для поставщика ресурсов EventGrid. |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. EventHub/Аваилаблеклустеррегионс/чтение | Операция чтения для вывода списка доступных предварительно подготовленных кластеров в регионе Azure. |
> | Action | Microsoft.EventHub/checkNameAvailability/action | Проверяет доступность пространства имен в рамках указанной подписки. |
> | Action | Microsoft.EventHub/checkNamespaceAvailability/action | Проверяет доступность пространства имен в рамках указанной подписки. Этот API не рекомендуется. Вместо него используйте CheckNameAvailability. |
> | Action | Microsoft. EventHub/Clusters/Delete | Удаляет существующий ресурс кластера. |
> | Action | Microsoft.EventHub/clusters/namespaces/read | Список идентификаторов ARM для пространств имен в кластере. |
> | Action | Microsoft.EventHub/clusters/operationresults/read | Получение состояния асинхронной операции кластера. |
> | Action | Microsoft.EventHub/clusters/providers/Microsoft.Insights/metricDefinitions/read | Возвращает список описаний ресурсов метрик кластера. |
> | Action | Microsoft.EventHub/clusters/read | Получает описание ресурса кластера |
> | Action | Microsoft.EventHub/clusters/write | Создает или изменяет существующий ресурс кластера. |
> | Action | Microsoft.EventHub/locations/deleteVirtualNetworkOrSubnets/action | Удаляет правила виртуальной сети в поставщике ресурсов EventHub для указанной виртуальной сети |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/action | Обновление правила авторизации для пространства имен. Это нерекомендуемый API. Чтобы обновить правило авторизации для пространства имен, используйте вызов PUT. Эта операция не поддерживается для API версии 2017-04-01. |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/delete | Удаляет правило авторизации для пространства имен. Невозможно удалить правило авторизации пространства имен по умолчанию.  |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/listkeys/action | Получить строку подключения к пространству имен |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/read | Получить список описаний правил авторизации пространства имен. |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/regenerateKeys/action | Повторное создание первичного или вторичного ключа в ресурсе |
> | Action | Microsoft.EventHub/namespaces/authorizationRules/write | Создает правила авторизации уровня пространства имен и обновляет его свойства. Можно обновить права доступа, первичный ключ и вторичный ключ для правил авторизации. |
> | Action | Microsoft.EventHub/namespaces/Delete | Удалить ресурс пространства имен |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Получает ключи правил авторизации для основного пространства имен аварийного восстановления |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/authorizationRules/read | Получить правила авторизации основного пространства имен аварийного восстановления |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/breakPairing/action | Отключает аварийное восстановление и останавливает репликацию изменений из основного пространства имен в дополнительное. |
> | Action | Microsoft.EventHub/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Проверяет доступность псевдонима пространства имен в подписке. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/delete | Удаление конфигурации аварийного восстановления, связанной с пространством имен. Эта операция может быть вызвана только через основное пространство имен. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/failover/action | Вызывает отработку отказа географического аварийного восстановления и перенастраивает псевдоним пространства имен, чтобы он указывал на дополнительное пространство. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/read | Получает конфигурацию аварийного восстановления, связанную с пространством имен. |
> | Action | Microsoft.EventHub/namespaces/disasterRecoveryConfigs/write | Создает или обновляет конфигурацию аварийного восстановления, связанную с пространством имен. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/action | Операция обновления концентратора событий. Эта операция не поддерживается для API версии 2017-04-01. Правила авторизации. Чтобы обновить правило авторизации, используйте вызов PUT. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/delete | Операция удаления правил авторизации концентратора событий. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/listkeys/action | Возвращает строку подключения к концентратору событий. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/read |  Возвращает список правил авторизации концентратора событий. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/regenerateKeys/action | Повторное создание первичного или вторичного ключа в ресурсе |
> | Action | Microsoft.EventHub/namespaces/eventhubs/authorizationRules/write | Создает правила авторизации концентратора событий и обновляет его свойства. Можно обновить права доступа к правилам авторизации. |
> | Action | Microsoft.EventHub/namespaces/eventHubs/consumergroups/Delete | Операция удаления ресурса группы потребителей. |
> | Action | Microsoft.EventHub/namespaces/eventHubs/consumergroups/read | Возвращает список описаний ресурсов группы потребителей. |
> | Action | Microsoft.EventHub/namespaces/eventHubs/consumergroups/write | Создает или обновляет свойства группы потребителей. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/Delete | Операция удаления ресурса концентратора событий. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/read | Возвращает список описаний ресурсов концентратора событий. |
> | Action | Microsoft.EventHub/namespaces/eventhubs/write | Создает или обновляет свойства концентратора событий. |
> | Action | Microsoft.EventHub/namespaces/ipFilterRules/delete | Удалить ресурс IP-фильтра |
> | Action | Microsoft.EventHub/namespaces/ipFilterRules/read | Получить ресурс IP-фильтра |
> | Action | Microsoft.EventHub/namespaces/ipFilterRules/write | Создать ресурс фильтра IP-адресов |
> | DataAction | Microsoft. EventHub/пространства имен/сообщения/получение/действие | Получить сообщения |
> | DataAction | Microsoft. EventHub/пространства имен/сообщения/Отправка/действие | Отправить сообщения |
> | Action | Microsoft.EventHub/namespaces/messagingPlan/read | Получение плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Action | Microsoft.EventHub/namespaces/messagingPlan/write | Обновление плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Action | Microsoft. EventHub/пространства имен, нетворкрулесет/удаление | Удалить ресурс правила виртуальной сети |
> | Action | Microsoft. EventHub/пространства имен/нетворкрулесет/чтение | Получает ресурс Нетворкрулесет |
> | Action | Microsoft. EventHub/пространства имен/нетворкрулесет/запись | Создать ресурс правила виртуальной сети |
> | Action | Microsoft. EventHub/пространства имен, нетворкрулесетс/удаление | Удалить ресурс правила виртуальной сети |
> | Action | Microsoft. EventHub/пространства имен/нетворкрулесетс/чтение | Получает ресурс Нетворкрулесет |
> | Action | Microsoft. EventHub/пространства имен/нетворкрулесетс/запись | Создать ресурс правила виртуальной сети |
> | Action | Microsoft.EventHub/namespaces/operationresults/read | Получение состояния операции пространства имен |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает список описаний параметров диагностики пространства имен. |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Возвращает список описаний параметров диагностики пространства имен. |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/logDefinitions/read | Возвращает список описаний ресурсов журналов пространства имен. |
> | Action | Microsoft.EventHub/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Возвращает список описаний ресурсов метрик пространства имен. |
> | Action | Microsoft.EventHub/namespaces/read | Получить список описаний ресурса пространства имен |
> | Action | Microsoft.EventHub/namespaces/removeAcsNamepsace/action | Удаление пространства имен ACS |
> | Action | Microsoft.EventHub/namespaces/virtualNetworkRules/delete | Удалить ресурс правила виртуальной сети |
> | Action | Microsoft.EventHub/namespaces/virtualNetworkRules/read | Получает ресурс правила виртуальной сети |
> | Action | Microsoft.EventHub/namespaces/virtualNetworkRules/write | Создать ресурс правила виртуальной сети |
> | Action | Microsoft.EventHub/namespaces/write | Создает ресурс пространства имен и обновляет его свойства. К этим свойствам относятся теги и пространство имен. |
> | Action | Microsoft.EventHub/operations/read | Получить операции |
> | Action | Microsoft.EventHub/register/action | Регистрирует подписку для поставщика ресурсов EventHub и разрешает создание ресурсов EventHub |
> | Action | Microsoft.EventHub/sku/read | Получение списка описаний ресурсов номеров SKU. |
> | Action | Microsoft.EventHub/sku/regions/read | Получение списка описаний ресурсов регионов для номеров SKU. |
> | Action | Microsoft.EventHub/unregister/action | Регистрирует поставщика ресурсов EventHub |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Features/features/read | Получает функции подписки. |
> | Action | Microsoft.Features/operations/read | Получает список операций. |
> | Action | Microsoft.Features/providers/features/read | Получает функцию подписки в заданном поставщике ресурсов. |
> | Action | Microsoft.Features/providers/features/register/action | Регистрирует функцию для подписки в заданном поставщике ресурсов. |
> | Action | Microsoft.Features/providers/features/unregister/action | Отменяет регистрацию функции для подписки в заданном поставщике ресурсов. |
> | Action | Microsoft.Features/register/action | Регистрирует компонент подписки. |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Гуестконфигуратион/Гуестконфигуратионассигнментс/Delete | Удаление назначения конфигурации гостя. |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/read | Получение назначения конфигурации гостевой системы. |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/reports/read | Получение отчета о назначении конфигурации гостевой системы. |
> | Action | Microsoft.GuestConfiguration/guestConfigurationAssignments/write | Создание назначения конфигурации гостевой системы. |
> | Action | Microsoft. Гуестконфигуратион/Register/действие | Регистрирует подписку для поставщика ресурсов Microsoft. Гуестконфигуратион. |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.HDInsight/clusters/applications/delete | Удаление приложения для кластера HDInsight |
> | Action | Microsoft.HDInsight/clusters/applications/read | Получение приложения для кластера HDInsight |
> | Action | Microsoft.HDInsight/clusters/applications/write | Создание приложения или его обновление для кластера HDInsight |
> | Action | Microsoft.HDInsight/clusters/changerdpsetting/action | Изменяет параметры RDP для кластера HDInsight. |
> | Action | Microsoft.HDInsight/clusters/configurations/action | Обновляет конфигурацию кластера HDInsight. |
> | Action | Microsoft.HDInsight/clusters/configurations/action | Возвращает конфигурации кластеров HDInsight. |
> | Action | Microsoft.HDInsight/clusters/configurations/read | Возвращает конфигурации кластеров HDInsight. |
> | Action | Microsoft.HDInsight/clusters/delete | Удаляет кластер HDInsight. |
> | Action | Microsoft. HDInsight/Clusters/Жетгатевайсеттингс/действие | Получение параметров шлюза для кластера HDInsight |
> | Action | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/read | Получение параметра диагностики для ресурса кластера HDInsight. |
> | Action | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/diagnosticSettings/write | Создание или обновление параметра диагностики для ресурса кластера HDInsight. |
> | Action | Microsoft.HDInsight/clusters/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для кластера HDInsight. |
> | Action | Microsoft.HDInsight/clusters/read | Возвращает сведения о кластере HDInsight. |
> | Action | Microsoft.HDInsight/clusters/roles/resize/action | Изменяет размер кластера HDInsight. |
> | Action | Microsoft. HDInsight/Clusters/Упдатегатевайсеттингс/действие | Обновление параметров шлюза для кластера HDInsight |
> | Action | Microsoft.HDInsight/clusters/write | Создает или обновляет кластер HDInsight. |
> | Action | Microsoft.HDInsight/locations/capabilities/read | Возвращает возможности подписки. |
> | Action | Microsoft.HDInsight/locations/checkNameAvailability/read | Проверить доступность имени |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ImportExport/jobs/delete | Удаляет существующее задание. |
> | Action | Microsoft.ImportExport/jobs/listBitLockerKeys/action | Возвращает ключи BitLocker для указанного задания. |
> | Action | Microsoft.ImportExport/jobs/read | Возвращает свойства для указанного задания или возвращает список заданий. |
> | Action | Microsoft.ImportExport/jobs/write | Создает задание с указанными параметрами либо обновляет свойства или теги указанного задания. |
> | Action | Microsoft.ImportExport/locations/read | Возвращает свойства для указанного расположения или возвращает список расположений. |
> | Action | Microsoft.ImportExport/register/action | Регистрирует подписку для поставщика ресурсов импорта и экспорта и позволяет создавать задания импорта и экспорта. |

## <a name="microsoftinsights"></a>Microsoft.Insights

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Insights/ActionGroups/Delete | Удаление группы действий |
> | Action | Microsoft.Insights/ActionGroups/Read | Чтение группы действий |
> | Action | Microsoft.Insights/ActionGroups/Write | Создание или изменение группы действий |
> | Action | Microsoft.Insights/ActivityLogAlerts/Activated/Action | Активировано оповещение журнала действий |
> | Action | Microsoft.Insights/ActivityLogAlerts/Delete | Удаление оповещения для журнала действий |
> | Action | Microsoft.Insights/ActivityLogAlerts/Read | Чтение оповещения из журнала действий |
> | Action | Microsoft.Insights/ActivityLogAlerts/Write | Создание или изменение оповещения для журнала действий |
> | Action | Microsoft.Insights/AlertRules/Activated/Action | Активировано классическое оповещение метрики |
> | Action | Microsoft.Insights/AlertRules/Delete | Удаление классического оповещения метрики |
> | Action | Microsoft.Insights/AlertRules/Incidents/Read | Чтение инцидента классического оповещения метрики |
> | Action | Microsoft.Insights/AlertRules/Read | Чтение классического оповещения метрики |
> | Action | Microsoft.Insights/AlertRules/Resolved/Action | Классическое оповещение метрики разрешено |
> | Action | Microsoft.Insights/AlertRules/Throttled/Action | Правило классического оповещения метрики отрегулировано |
> | Action | Microsoft.Insights/AlertRules/Write | Создание или изменение классического оповещения метрики |
> | Action | Microsoft.Insights/AutoscaleSettings/Delete | Удаление параметра автомасштабирования |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Read | Чтение параметра диагностики для ресурсов |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/diagnosticSettings/Write | Создание или изменение параметра для диагностики ресурсов |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/logDefinitions/Read | Чтение определений в журнале |
> | Action | Microsoft.Insights/AutoscaleSettings/providers/Microsoft.Insights/MetricDefinitions/Read | Чтение определений метрик |
> | Action | Microsoft.Insights/AutoscaleSettings/Read | Чтение параметра автомасштабирования |
> | Action | Microsoft.Insights/AutoscaleSettings/Scaledown/Action | Автоматическое уменьшение масштаба запущено |
> | Action | Microsoft.Insights/AutoscaleSettings/ScaledownResult/Action | Автоматическое уменьшение масштаба завершено |
> | Action | Microsoft.Insights/AutoscaleSettings/Scaleup/Action | Автоматическое увеличение масштаба запущено |
> | Action | Microsoft.Insights/AutoscaleSettings/ScaleupResult/Action | Автоматическое увеличение масштаба завершено |
> | Action | Microsoft.Insights/AutoscaleSettings/Write | Создание или изменение параметра автомасштабирования |
> | Action | Microsoft. Insights/базовый/чтение | Чтение базовой метрики (предварительная версия) |
> | Action | Microsoft. Insights/Калкулатебаселине/чтение | Вычисление базового показателя для значений метрик (предварительная версия) |
> | Action | Microsoft.Insights/Components/AnalyticsItems/Delete | Удаляется элемент аналитики Application Insights |
> | Action | Microsoft.Insights/Components/AnalyticsItems/Read | Считывается элемент аналитики Application Insights |
> | Action | Microsoft.Insights/Components/AnalyticsItems/Write | Записывается элемент аналитики Application Insights |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Action | Действие таблицы аналитики Application Insights |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Delete | Удаляется схема таблицы аналитики Application Insights |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Read | Считывается схема таблицы аналитики Application Insights |
> | Action | Microsoft.Insights/Components/AnalyticsTables/Write | Записывается схема таблицы аналитики Application Insights |
> | Action | Microsoft.Insights/Components/Annotations/Delete | Удаляется заметка Application Insights |
> | Action | Microsoft.Insights/Components/Annotations/Read | Считывается заметка Application Insights |
> | Action | Microsoft.Insights/Components/Annotations/Write | Записывается заметка Application Insights |
> | Action | Microsoft.Insights/Components/Api/Read | Считывается API данных компонента Application Insights |
> | Action | Microsoft.Insights/Components/ApiKeys/Action | Создается ключ API Application Insights |
> | Action | Microsoft.Insights/Components/ApiKeys/Delete | Удаляется ключ API Application Insights |
> | Action | Microsoft.Insights/Components/ApiKeys/Read | Считывается ключ API Application Insights |
> | Action | Microsoft.Insights/Components/BillingPlanForComponent/Read | Считывается план выставления счетов для компонента Application Insights |
> | Action | Microsoft.Insights/Components/CurrentBillingFeatures/Read | Считываются текущие функции выставления счетов для компонента Application Insights |
> | Action | Microsoft.Insights/Components/CurrentBillingFeatures/Write | Записываются текущие функции выставления счетов для компонента Application Insights |
> | Action | Microsoft.Insights/Components/DefaultWorkItemConfig/Read | Считывается конфигурация интеграции ALM по умолчанию Application Insights |
> | Action | Microsoft.Insights/Components/Delete | Удаление конфигурации компонента Application Insights |
> | Action | Microsoft.Insights/Components/Events/Read | Получение журналов Application Insights с использованием формата запроса OData |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Action | Действие параметров экспорта Application Insights |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Delete | Удаляются параметры экспорта Application Insights |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Read | Считываются параметры экспорта Application Insights |
> | Action | Microsoft.Insights/Components/ExportConfiguration/Write | Записываются параметры экспорта Application Insights |
> | Action | Microsoft.Insights/Components/ExtendQueries/Read | Считываются расширенные результаты запросов компонента Application Insights |
> | Action | Microsoft.Insights/Components/Favorites/Delete | Удаляется избранное Application Insights |
> | Action | Microsoft.Insights/Components/Favorites/Read | Считывается избранное Application Insights |
> | Action | Microsoft.Insights/Components/Favorites/Write | Записывается избранное Application Insights |
> | Action | Microsoft.Insights/Components/FeatureCapabilities/Read | Считываются функции компонента Application Insights |
> | Action | Microsoft.Insights/Components/GetAvailableBillingFeatures/Read | Считываются доступные функции выставления счетов компонента Application Insights |
> | Action | Microsoft.Insights/Components/GetToken/Read | Считывается токен компонента Application Insights |
> | Action | Microsoft.Insights/Components/MetricDefinitions/Read | Считываются определения метрик компонента Application Insights |
> | Action | Microsoft.Insights/Components/Metrics/Read | Считываются метрики компонента Application Insights |
> | Action | Microsoft.Insights/Components/Move/Action | Перенос компонента Application Insights на другую группу ресурсов или подписку |
> | Action | Microsoft.Insights/Components/MyAnalyticsItems/Delete | Удаляется личный элемент аналитики Application Insights |
> | Action | Microsoft.Insights/Components/MyAnalyticsItems/Read | Считывается личный элемент аналитики Application Insights |
> | Action | Microsoft.Insights/Components/MyAnalyticsItems/Write | Записывается личный элемент аналитики Application Insights |
> | Action | Microsoft.Insights/Components/MyFavorites/Read | Считывается личное избранное Application Insights |
> | Action | Microsoft.Insights/Components/Operations/Read | Получение состояния длительных операций в Application Insights |
> | Action | Microsoft.Insights/Components/PricingPlans/Read | Считывается ценовой план компонента Application Insights |
> | Action | Microsoft.Insights/Components/PricingPlans/Write | Записывается ценовой план компонента Application Insights |
> | Action | Microsoft.Insights/Components/ProactiveDetectionConfigs/Read | Считывается конфигурация упреждающего обнаружения Application Insights |
> | Action | Microsoft.Insights/Components/ProactiveDetectionConfigs/Write | Записывается конфигурация упреждающего обнаружения Application Insights |
> | Action | Microsoft.Insights/Components/providers/Microsoft.Insights/MetricDefinitions/Read | Чтение определений метрик |
> | Action | Microsoft.Insights/Components/Purge/Action | Очистка данных Application Insights |
> | Action | Microsoft.Insights/Components/Query/Read | Выполнение запросов к журналам Application Insights |
> | Action | Microsoft.Insights/Components/QuotaStatus/Read | Считывается состояние квоты компонента Application Insights |
> | Action | Microsoft.Insights/Components/Read | Считывание конфигурации компонента Application Insights |
> | Action | Microsoft.Insights/Components/SyntheticMonitorLocations/Read | Считываются расположения веб-тестов Application Insights |
> | Action | Microsoft.Insights/Components/Webtests/Read | Считывание конфигурации веб-теста |
> | Action | Microsoft.Insights/Components/WorkItemConfigs/Delete | Удаляется конфигурация интеграции ALM Application Insights |
> | Action | Microsoft.Insights/Components/WorkItemConfigs/Read | Считывается конфигурация интеграции ALM Application Insights |
> | Action | Microsoft.Insights/Components/WorkItemConfigs/Write | Записывается конфигурация интеграции ALM Application Insights |
> | Action | Microsoft.Insights/Components/Write | Запись конфигурации компонента Application Insights |
> | Action | Microsoft.Insights/DiagnosticSettings/Delete | Удаление параметра для диагностики ресурсов |
> | Action | Microsoft.Insights/DiagnosticSettings/Read | Чтение параметра диагностики для ресурсов |
> | Action | Microsoft.Insights/DiagnosticSettings/Write | Создание или изменение параметра для диагностики ресурсов |
> | Action | Microsoft.Insights/EventCategories/Read | Чтение доступных категорий событий для журнала действий |
> | Action | Microsoft.Insights/eventtypes/digestevents/Read | Дайджест чтения типов событий управления |
> | Action | Microsoft.Insights/eventtypes/values/Read | Чтение событий из журнала действий |
> | Action | Microsoft.Insights/ExtendedDiagnosticSettings/Delete | Удаление параметра для диагностики журнала сетевых потоков |
> | Action | Microsoft.Insights/ExtendedDiagnosticSettings/Read | Чтение параметра диагностики для журнала сетевых потоков |
> | Action | Microsoft.Insights/ExtendedDiagnosticSettings/Write | Создание или изменение параметра для диагностики журнала сетевых потоков |
> | Action | Microsoft.Insights/ListMigrationDate/Action | Получение даты переноса подписки |
> | Action | Microsoft.Insights/ListMigrationDate/Read | Получение даты переноса подписки |
> | Action | Microsoft.Insights/LogDefinitions/Read | Чтение определений в журнале |
> | Action | Microsoft.Insights/LogProfiles/Delete | Удаление профиля для журнала действий |
> | Action | Microsoft.Insights/LogProfiles/Read | Чтение профиля для журнала действий |
> | Action | Microsoft.Insights/LogProfiles/Write | Создание или изменение профиля для журнала действий |
> | Action | Microsoft.Insights/Logs/ADAssessmentRecommendation/Read | Чтение данных из таблицы ADAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/ADReplicationResult/Read | Чтение данных из таблицы ADReplicationResult |
> | Action | Microsoft.Insights/Logs/ADSecurityAssessmentRecommendation/Read | Чтение данных из таблицы ADSecurityAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/Alert/Read | Чтение данных из таблицы Alert |
> | Action | Microsoft.Insights/Logs/AlertHistory/Read | Чтение данных из таблицы AlertHistory |
> | Action | Microsoft.Insights/Logs/ApplicationInsights/Read | Чтение данных из таблицы ApplicationInsights |
> | Action | Microsoft.Insights/Logs/AzureActivity/Read | Чтение данных из таблицы AzureActivity |
> | Action | Microsoft.Insights/Logs/AzureMetrics/Read | Чтение данных из таблицы AzureMetrics |
> | Action | Microsoft.Insights/Logs/BoundPort/Read | Чтение данных из таблицы BoundPort |
> | Action | Microsoft.Insights/Logs/CommonSecurityLog/Read | Чтение данных из таблицы CommonSecurityLog |
> | Action | Microsoft.Insights/Logs/ComputerGroup/Read | Чтение данных из таблицы ComputerGroup |
> | Action | Microsoft.Insights/Logs/ConfigurationChange/Read | Чтение данных из таблицы ConfigurationChange |
> | Action | Microsoft.Insights/Logs/ConfigurationData/Read | Чтение данных из таблицы ConfigurationData |
> | Action | Microsoft.Insights/Logs/ContainerImageInventory/Read | Чтение данных из таблицы ContainerImageInventory |
> | Action | Microsoft.Insights/Logs/ContainerInventory/Read | Чтение данных из таблицы ContainerInventory |
> | Action | Microsoft.Insights/Logs/ContainerLog/Read | Чтение данных из таблицы ContainerLog |
> | Action | Microsoft.Insights/Logs/ContainerServiceLog/Read | Чтение данных из таблицы ContainerServiceLog |
> | Action | Microsoft.Insights/Logs/DeviceAppCrash/Read | Чтение данных из таблицы DeviceAppCrash |
> | Action | Microsoft.Insights/Logs/DeviceAppLaunch/Read | Чтение данных из таблицы DeviceAppLaunch |
> | Action | Microsoft.Insights/Logs/DeviceCalendar/Read | Чтение данных из таблицы DeviceCalendar |
> | Action | Microsoft.Insights/Logs/DeviceCleanup/Read | Чтение данных из таблицы DeviceCleanup |
> | Action | Microsoft.Insights/Logs/DeviceConnectSession/Read | Чтение данных из таблицы DeviceConnectSession |
> | Action | Microsoft.Insights/Logs/DeviceEtw/Read | Чтение данных из таблицы DeviceEtw |
> | Action | Microsoft.Insights/Logs/DeviceHardwareHealth/Read | Чтение данных из таблицы DeviceHardwareHealth |
> | Action | Microsoft.Insights/Logs/DeviceHealth/Read | Чтение данных из таблицы DeviceHealth |
> | Action | Microsoft.Insights/Logs/DeviceHeartbeat/Read | Чтение данных из таблицы DeviceHeartbeat |
> | Action | Microsoft.Insights/Logs/DeviceSkypeHeartbeat/Read | Чтение данных из таблицы DeviceSkypeHeartbeat |
> | Action | Microsoft.Insights/Logs/DeviceSkypeSignIn/Read | Чтение данных из таблицы DeviceSkypeSignIn |
> | Action | Microsoft.Insights/Logs/DeviceSleepState/Read | Чтение данных из таблицы DeviceSleepState |
> | Action | Microsoft.Insights/Logs/DHAppFailure/Read | Чтение данных из таблицы DHAppFailure |
> | Action | Microsoft.Insights/Logs/DHAppReliability/Read | Чтение данных из таблицы DHAppReliability |
> | Action | Microsoft.Insights/Logs/DHDriverReliability/Read | Чтение данных из таблицы DHDriverReliability |
> | Action | Microsoft.Insights/Logs/DHLogonFailures/Read | Чтение данных из таблицы DHLogonFailures |
> | Action | Microsoft.Insights/Logs/DHLogonMetrics/Read | Чтение данных из таблицы DHLogonMetrics |
> | Action | Microsoft.Insights/Logs/DHOSCrashData/Read | Чтение данных из таблицы DHOSCrashData |
> | Action | Microsoft.Insights/Logs/DHOSReliability/Read | Чтение данных из таблицы DHOSReliability |
> | Action | Microsoft.Insights/Logs/DHWipAppLearning/Read | Чтение данных из таблицы DHWipAppLearning |
> | Action | Microsoft.Insights/Logs/DnsEvents/Read | Чтение данных из таблицы DnsEvents |
> | Action | Microsoft.Insights/Logs/DnsInventory/Read | Чтение данных из таблицы DnsInventory |
> | Action | Microsoft.Insights/Logs/ETWEvent/Read | Чтение данных из таблицы ETWEvent |
> | Action | Microsoft.Insights/Logs/Event/Read | Чтение данных из таблицы Event |
> | Action | Microsoft.Insights/Logs/ExchangeAssessmentRecommendation/Read | Чтение данных из таблицы ExchangeAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/ExchangeOnlineAssessmentRecommendation/Read | Чтение данных из таблицы ExchangeOnlineAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/Heartbeat/Read | Чтение данных из таблицы Heartbeat |
> | Action | Microsoft.Insights/Logs/IISAssessmentRecommendation/Read | Чтение данных из таблицы IISAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/InboundConnection/Read | Чтение данных из таблицы InboundConnection |
> | Action | Microsoft.Insights/Logs/KubeNodeInventory/Read | Чтение данных из таблицы KubeNodeInventory |
> | Action | Microsoft.Insights/Logs/KubePodInventory/Read | Чтение данных из таблицы KubePodInventory |
> | Action | Microsoft.Insights/Logs/LinuxAuditLog/Read | Чтение данных из таблицы LinuxAuditLog |
> | Action | Microsoft.Insights/Logs/MAApplication/Read | Чтение данных из таблицы MAApplication |
> | Action | Microsoft.Insights/Logs/MAApplicationHealth/Read | Чтение данных из таблицы MAApplicationHealth |
> | Action | Microsoft.Insights/Logs/MAApplicationHealthAlternativeVersions/Read | Чтение данных из таблицы MAApplicationHealthAlternativeVersions |
> | Action | Microsoft.Insights/Logs/MAApplicationHealthIssues/Read | Чтение данных из таблицы MAApplicationHealthIssues |
> | Action | Microsoft.Insights/Logs/MAApplicationInstance/Read | Чтение данных из таблицы MAApplicationInstance |
> | Action | Microsoft.Insights/Logs/MAApplicationInstanceReadiness/Read | Чтение данных из таблицы MAApplicationInstanceReadiness |
> | Action | Microsoft.Insights/Logs/MAApplicationReadiness/Read | Чтение данных из таблицы MAApplicationReadiness |
> | Action | Microsoft.Insights/Logs/MADeploymentPlan/Read | Чтение данных из таблицы MADeploymentPlan |
> | Action | Microsoft.Insights/Logs/MADevice/Read | Чтение данных из таблицы MADevice |
> | Action | Microsoft.Insights/Logs/MADevicePnPHealth/Read | Чтение данных из таблицы MADevicePnPHealth |
> | Action | Microsoft.Insights/Logs/MADevicePnPHealthAlternativeVersions/Read | Чтение данных из таблицы MADevicePnPHealthAlternativeVersions |
> | Action | Microsoft.Insights/Logs/MADevicePnPHealthIssues/Read | Чтение данных из таблицы MADevicePnPHealthIssues |
> | Action | Microsoft.Insights/Logs/MADeviceReadiness/Read | Чтение данных из таблицы MADeviceReadiness |
> | Action | Microsoft.Insights/Logs/MADriverInstanceReadiness/Read | Чтение данных из таблицы MADriverInstanceReadiness |
> | Action | Microsoft.Insights/Logs/MADriverReadiness/Read | Чтение данных из таблицы MADriverReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeAddin/Read | Чтение данных из таблицы MAOfficeAddin |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinHealth/Read | Чтение данных из таблицы MAOfficeAddinHealth |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinHealthIssues/Read | Чтение данных из таблицы MAOfficeAddinHealthIssues |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinInstance/Read | Чтение данных из таблицы MAOfficeAddinInstance |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinInstanceReadiness/Read | Чтение данных из таблицы MAOfficeAddinInstanceReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeAddinReadiness/Read | Чтение данных из таблицы MAOfficeAddinReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeApp/Read | Чтение данных из таблицы MAOfficeApp |
> | Action | Microsoft.Insights/Logs/MAOfficeAppHealth/Read | Чтение данных из таблицы MAOfficeAppHealth |
> | Action | Microsoft.Insights/Logs/MAOfficeAppInstance/Read | Чтение данных из таблицы MAOfficeAppInstance |
> | Action | Microsoft.Insights/Logs/MAOfficeAppReadiness/Read | Чтение данных из таблицы MAOfficeAppReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeBuildInfo/Read | Чтение данных из таблицы MAOfficeBuildInfo |
> | Action | Microsoft.Insights/Logs/MAOfficeCurrencyAssessment/Read | Чтение данных из таблицы MAOfficeCurrencyAssessment |
> | Action | Microsoft.Insights/Logs/MAOfficeCurrencyAssessmentDailyCounts/Read | Чтение данных из таблицы MAOfficeCurrencyAssessmentDailyCounts |
> | Action | Microsoft.Insights/Logs/MAOfficeDeploymentStatus/Read | Чтение данных из таблицы MAOfficeDeploymentStatus |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroHealth/Read | Чтение данных из таблицы MAOfficeMacroHealth |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroHealthIssues/Read | Чтение данных из таблицы MAOfficeMacroHealthIssues |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroIssueInstanceReadiness/Read | Чтение данных из таблицы MAOfficeMacroIssueInstanceReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroIssueReadiness/Read | Чтение данных из таблицы MAOfficeMacroIssueReadiness |
> | Action | Microsoft.Insights/Logs/MAOfficeMacroSummary/Read | Чтение данных из таблицы MAOfficeMacroSummary |
> | Action | Microsoft.Insights/Logs/MAOfficeSuite/Read | Чтение данных из таблицы MAOfficeSuite |
> | Action | Microsoft.Insights/Logs/MAOfficeSuiteInstance/Read | Чтение данных из таблицы MAOfficeSuiteInstance |
> | Action | Microsoft.Insights/Logs/MAProposedPilotDevices/Read | Чтение данных из таблицы MAProposedPilotDevices |
> | Action | Microsoft.Insights/Logs/MAWindowsBuildInfo/Read | Чтение данных из таблицы MAWindowsBuildInfo |
> | Action | Microsoft.Insights/Logs/MAWindowsCurrencyAssessment/Read | Чтение данных из таблицы MAWindowsCurrencyAssessment |
> | Action | Microsoft.Insights/Logs/MAWindowsCurrencyAssessmentDailyCounts/Read | Чтение данных из таблицы MAWindowsCurrencyAssessmentDailyCounts |
> | Action | Microsoft.Insights/Logs/MAWindowsDeploymentStatus/Read | Чтение данных из таблицы MAWindowsDeploymentStatus |
> | Action | Microsoft.Insights/Logs/MAWindowsSysReqInstanceReadiness/Read | Чтение данных из таблицы MAWindowsSysReqInstanceReadiness |
> | Action | Microsoft.Insights/Logs/NetworkMonitoring/Read | Чтение данных из таблицы NetworkMonitoring |
> | Action | Microsoft.Insights/Logs/OfficeActivity/Read | Чтение данных из таблицы OfficeActivity |
> | Action | Microsoft.Insights/Logs/Operation/Read | Чтение данных из таблицы Operation |
> | Action | Microsoft.Insights/Logs/OutboundConnection/Read | Чтение данных из таблицы OutboundConnection |
> | Action | Microsoft.Insights/Logs/Perf/Read | Чтение данных из таблицы Perf |
> | Action | Microsoft.Insights/Logs/ProtectionStatus/Read | Чтение данных из таблицы ProtectionStatus |
> | Action | Microsoft.Insights/Logs/Read | Чтение данных из всех журналов |
> | Action | Microsoft.Insights/Logs/ReservedAzureCommonFields/Read | Чтение данных из таблицы ReservedAzureCommonFields |
> | Action | Microsoft.Insights/Logs/ReservedCommonFields/Read | Чтение данных из таблицы ReservedCommonFields |
> | Action | Microsoft.Insights/Logs/SCCMAssessmentRecommendation/Read | Чтение данных из таблицы SCCMAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SCOMAssessmentRecommendation/Read | Чтение данных из таблицы SCOMAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SecurityAlert/Read | Чтение данных из таблицы SecurityAlert |
> | Action | Microsoft.Insights/Logs/SecurityBaseline/Read | Чтение данных из таблицы SecurityBaseline |
> | Action | Microsoft.Insights/Logs/SecurityBaselineSummary/Read | Чтение данных из таблицы SecurityBaselineSummary |
> | Action | Microsoft.Insights/Logs/SecurityDetection/Read | Чтение данных из таблицы SecurityDetection |
> | Action | Microsoft.Insights/Logs/SecurityEvent/Read | Чтение данных из таблицы SecurityEvent |
> | Action | Microsoft.Insights/Logs/ServiceFabricOperationalEvent/Read | Чтение данных из таблицы ServiceFabricOperationalEvent |
> | Action | Microsoft.Insights/Logs/ServiceFabricReliableActorEvent/Read | Чтение данных из таблицы ServiceFabricReliableActorEvent |
> | Action | Microsoft.Insights/Logs/ServiceFabricReliableServiceEvent/Read | Чтение данных из таблицы ServiceFabricReliableServiceEvent |
> | Action | Microsoft.Insights/Logs/SfBAssessmentRecommendation/Read | Чтение данных из таблицы SfBAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SfBOnlineAssessmentRecommendation/Read | Чтение данных из таблицы SfBOnlineAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SharePointOnlineAssessmentRecommendation/Read | Чтение данных из таблицы SharePointOnlineAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SPAssessmentRecommendation/Read | Чтение данных из таблицы SPAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SQLAssessmentRecommendation/Read | Чтение данных из таблицы SQLAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/SQLQueryPerformance/Read | Чтение данных из таблицы SQLQueryPerformance |
> | Action | Microsoft.Insights/Logs/Syslog/Read | Чтение данных из таблицы Syslog |
> | Action | Microsoft.Insights/Logs/SysmonEvent/Read | Чтение данных из таблицы SysmonEvent |
> | Action | Microsoft.Insights/Logs/UAApp/Read | Чтение данных из таблицы UAApp |
> | Action | Microsoft.Insights/Logs/UAComputer/Read | Чтение данных из таблицы UAComputer |
> | Action | Microsoft.Insights/Logs/UAComputerRank/Read | Чтение данных из таблицы UAComputerRank |
> | Action | Microsoft.Insights/Logs/UADriver/Read | Чтение данных из таблицы UADriver |
> | Action | Microsoft.Insights/Logs/UADriverProblemCodes/Read | Чтение данных из таблицы UADriverProblemCodes |
> | Action | Microsoft.Insights/Logs/UAFeedback/Read | Чтение данных из таблицы UAFeedback |
> | Action | Microsoft.Insights/Logs/UAHardwareSecurity/Read | Чтение данных из таблицы UAHardwareSecurity |
> | Action | Microsoft.Insights/Logs/UAIESiteDiscovery/Read | Чтение данных из таблицы UAIESiteDiscovery |
> | Action | Microsoft.Insights/Logs/UAOfficeAddIn/Read | Чтение данных из таблицы UAOfficeAddIn |
> | Action | Microsoft.Insights/Logs/UAProposedActionPlan/Read | Чтение данных из таблицы UAProposedActionPlan |
> | Action | Microsoft.Insights/Logs/UASysReqIssue/Read | Чтение данных из таблицы UASysReqIssue |
> | Action | Microsoft.Insights/Logs/UAUpgradedComputer/Read | Чтение данных из таблицы UAUpgradedComputer |
> | Action | Microsoft.Insights/Logs/Update/Read | Чтение данных из таблицы Update |
> | Action | Microsoft.Insights/Logs/UpdateRunProgress/Read | Чтение данных из таблицы UpdateRunProgress |
> | Action | Microsoft.Insights/Logs/UpdateSummary/Read | Чтение данных из таблицы UpdateSummary |
> | Action | Microsoft.Insights/Logs/Usage/Read | Чтение данных из таблицы Usage |
> | Action | Microsoft.Insights/Logs/W3CIISLog/Read | Чтение данных из таблицы W3CIISLog |
> | Action | Microsoft.Insights/Logs/WaaSDeploymentStatus/Read | Чтение данных из таблицы WaaSDeploymentStatus |
> | Action | Microsoft.Insights/Logs/WaaSInsiderStatus/Read | Чтение данных из таблицы WaaSInsiderStatus |
> | Action | Microsoft.Insights/Logs/WaaSUpdateStatus/Read | Чтение данных из таблицы WaaSUpdateStatus |
> | Action | Microsoft.Insights/Logs/WDAVStatus/Read | Чтение данных из таблицы WDAVStatus |
> | Action | Microsoft.Insights/Logs/WDAVThreat/Read | Чтение данных из таблицы WDAVThreat |
> | Action | Microsoft.Insights/Logs/WindowsClientAssessmentRecommendation/Read | Чтение данных из таблицы WindowsClientAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/WindowsFirewall/Read | Чтение данных из таблицы WindowsFirewall |
> | Action | Microsoft.Insights/Logs/WindowsServerAssessmentRecommendation/Read | Чтение данных из таблицы WindowsServerAssessmentRecommendation |
> | Action | Microsoft.Insights/Logs/WireData/Read | Чтение данных из таблицы WireData |
> | Action | Microsoft.Insights/Logs/WUDOAggregatedStatus/Read | Чтение данных из таблицы WUDOAggregatedStatus |
> | Action | Microsoft.Insights/Logs/WUDOStatus/Read | Чтение данных из таблицы WUDOStatus |
> | Action | Microsoft.Insights/MetricAlerts/Delete | Удаление оповещения метрики |
> | Action | Microsoft.Insights/MetricAlerts/Read | Чтение оповещения метрики |
> | Action | Microsoft.Insights/MetricAlerts/Status/Read | Чтение состояния оповещения для метрики |
> | Action | Microsoft.Insights/MetricAlerts/Write | Создание или изменение оповещения метрики |
> | Action | Microsoft. Insights/Метрикбаселинес/чтение | Чтение базовых показателей метрик |
> | Action | Microsoft.Insights/MetricDefinitions/Microsoft.Insights/Read | Чтение определений метрик |
> | Action | Microsoft.Insights/MetricDefinitions/providers/Microsoft.Insights/Read | Чтение определений метрик |
> | Action | Microsoft.Insights/MetricDefinitions/Read | Чтение определений метрик |
> | Action | Microsoft. Insights/Метрикнамеспацес/чтение | Чтение пространств имен метрик |
> | Action | Microsoft.Insights/Metrics/Action | Действие метрики |
> | Action | Microsoft. Insights/метрики/Microsoft. Insights/Read | Метрик считано |
> | Action | Microsoft.Insights/Metrics/providers/Metrics/Read | Метрик считано |
> | Action | Microsoft.Insights/Metrics/Read | Метрик считано |
> | DataAction | Microsoft.Insights/Metrics/Write | Запись показателей |
> | Action | Microsoft.Insights/MigrateToNewpricingModel/Action | Перенос подписки на новую ценовую модель |
> | Action | Microsoft.Insights/Operations/Read | Операции чтения |
> | Action | Microsoft.Insights/Register/Action | Регистрация поставщика Microsoft Insights |
> | Action | Microsoft.Insights/RollbackToLegacyPricingModel/Action | Откат подписки на старую ценовую модель |
> | Action | Microsoft.Insights/ScheduledQueryRules/Delete | Удаление запланированного правила запроса |
> | Action | Microsoft.Insights/ScheduledQueryRules/Read | Считывание запланированного правила запроса |
> | Action | Microsoft.Insights/ScheduledQueryRules/Write | Запись запланированного правила запроса |
> | Action | Microsoft.Insights/Tenants/Register/Action | Инициализирует поставщик Microsoft Insights |
> | Action | Microsoft.Insights/Unregister/Action | Регистрация поставщика Microsoft Insights |
> | Action | Microsoft.Insights/Webtests/Delete | Удаление конфигурации веб-теста |
> | Action | Microsoft.Insights/Webtests/GetToken/Read | Считывается токен веб-теста |
> | Action | Microsoft.Insights/Webtests/MetricDefinitions/Read | Считываются определения метрик веб-тестов |
> | Action | Microsoft.Insights/Webtests/Metrics/Read | Считываются метрики веб-тестов |
> | Action | Microsoft.Insights/Webtests/Read | Считывание конфигурации веб-теста |
> | Action | Microsoft.Insights/Webtests/Write | Запись конфигурации веб-теста |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Intune/diagnosticsettings/delete | Удаление параметра диагностики. |
> | Action | Microsoft.Intune/diagnosticsettings/read | Чтение параметра диагностики. |
> | Action | Microsoft.Intune/diagnosticsettings/write | Запись параметра диагностики. |
> | Action | Microsoft.Intune/diagnosticsettingscategories/read | Чтение категорий параметра диагностики. |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Иотцентрал/Апптемплатес/действие | Получает все доступные шаблоны приложений в Azure IoT Central |
> | Action | Microsoft.IoTCentral/checkNameAvailability/action | Проверка доступности для имени приложения IoT Central |
> | Action | Microsoft.IoTCentral/checkSubdomainAvailability/action | Проверка доступности для поддомена приложения IoT Central |
> | Action | Microsoft.IoTCentral/IoTApps/delete | Удаление приложений IoT Central |
> | Action | Microsoft.IoTCentral/IoTApps/read | Получение одного приложения IoT Central |
> | Action | Microsoft.IoTCentral/IoTApps/write | Создание или обновлений приложений IoT Central |
> | Action | Microsoft.IoTCentral/operations/read | Получение всех доступных операций в приложениях IoT Central |
> | Action | Microsoft.IoTCentral/register/action | Регистрация подписки для поставщика ресурсов IoT Central Azure |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.IoTSpaces/Graph/delete | Удаляет ресурс Graph Microsoft.IoTSpaces |
> | Action | Microsoft.IoTSpaces/Graph/read | Получает ресурсы Graph Microsoft.IoTSpaces |
> | Action | Microsoft.IoTSpaces/Graph/write | Создает ресурс Graph Microsoft.IoTSpaces |
> | Action | Microsoft.IoTSpaces/register/action | Регистрация подписки для поставщика ресурсов Graph Microsoft.IoTSpaces, чтобы обеспечить создание ресурсов. |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.KeyVault/checkNameAvailability/read | Проверка допустимости и доступности имени хранилища ключей |
> | Action | Microsoft.KeyVault/deletedVaults/read | Показать свойства обратимо удаленных хранилищ ключей |
> | Action | Microsoft.KeyVault/hsmPools/delete | Удаление пула HSM |
> | Action | Microsoft.KeyVault/hsmPools/joinVault/action | Присоединение хранилища ключей к пулу HSM |
> | Action | Microsoft.KeyVault/hsmPools/read | Просмотр свойств пула HSM |
> | Action | Microsoft.KeyVault/hsmPools/write | Создание нового пула HSM или изменение свойств существующего пула HSM |
> | Action | Microsoft.KeyVault/locations/deletedVaults/purge/action | Очистить обратимо удаленное хранилище ключей |
> | Action | Microsoft.KeyVault/locations/deletedVaults/read | Показать свойства обратимо удаленного хранилища ключей |
> | Action | Microsoft.KeyVault/locations/deleteVirtualNetworkOrSubnets/action | Уведомление Microsoft.KeyVault о том, что идет удаление виртуальной сети или подсети |
> | Action | Microsoft.KeyVault/locations/operationResults/read | Проверка результата выполнения длительной операции |
> | Action | Microsoft.KeyVault/operations/read | Получение списка операций, доступных в поставщике ресурсов Microsoft.KeyVault |
> | Action | Microsoft.KeyVault/register/action | Регистрирует подписку |
> | Action | Microsoft.KeyVault/unregister/action | Отменяет регистрацию подписки |
> | Action | Microsoft.KeyVault/vaults/accessPolicies/write | Обновите существующую политику доступа путем слияния или замены, или же добавив новую политику в хранилище. |
> | Action | Microsoft.KeyVault/vaults/delete | Удаление хранилища ключей |
> | Action | Microsoft.KeyVault/vaults/deploy/action | Обеспечение доступа к секретам в хранилище ключей при развертывании ресурсов Azure |
> | Action | Microsoft. KeyVault/хранилища/Евентгридфилтерс/удаление | Уведомляет Microsoft.KeyVault об удалении подписки EventGrid (Сетка событий) для Key Vault |
> | Action | Microsoft. KeyVault/хранилища/Евентгридфилтерс/чтение | Уведомляет Microsoft.KeyVault о просмотре подписки EventGrid (Сетка событий) для Key Vault |
> | Action | Microsoft. KeyVault/Vault/Евентгридфилтерс/запись | Уведомляет Microsoft.KeyVault о создании новой подписки EventGrid (Сетка событий) для Key Vault |
> | Action | Microsoft.KeyVault/vaults/read | Просмотр свойств хранилища ключей |
> | Action | Microsoft.KeyVault/vaults/secrets/read | Просмотр свойств секрета, но не его значения. |
> | Action | Microsoft.KeyVault/vaults/secrets/write | Создайте новый секрет или обновите значение существующего секрета. |
> | Action | Microsoft.KeyVault/vaults/write | Создание хранилища ключей или изменение свойств существующего хранилища ключей |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Kusto/Clusters/активация/действие | Запускает кластер. |
> | Action | Microsoft. Kusto/Clusters/Аттачеддатабасеконфигуратионс/Delete | Удаляет ресурс конфигурации присоединенной базы данных. |
> | Action | Microsoft. Kusto/Clusters/Аттачеддатабасеконфигуратионс/Read | Считывает ресурс конфигурации присоединенной базы данных. |
> | Action | Microsoft. Kusto/Clusters/Аттачеддатабасеконфигуратионс/Write | Записывает ресурс конфигурации присоединенной базы данных. |
> | Action | Microsoft. Kusto/Clusters/Чеккнамеаваилабилити/действие | Проверяет доступность имени кластера. |
> | Action | Microsoft. Kusto/Clusters/databases/АддпринЦипалс/Action | Добавляет участников базы данных. |
> | Action | Microsoft. Kusto/Clusters/databases/Чеккнамеаваилабилити/Action | Проверяет доступность имени для заданного типа. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnections/delete | Удаление ресурса подключений к данным. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnections/read | Чтение ресурса подключений к данным. |
> | Action | Microsoft.Kusto/Clusters/Databases/DataConnections/write | Записывает ресурс подключений к данным. |
> | Action | Microsoft. Kusto/Clusters/databases/Датаконнектионвалидатион/Action | Проверяет подключение к данным базы данных. |
> | Action | Microsoft.Kusto/Clusters/Databases/delete | Удаляет ресурс базы данных. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnections/delete | Удаляет ресурс соединений концентратора событий. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnections/read | Считывает ресурс подключений концентратора событий. |
> | Action | Microsoft.Kusto/Clusters/Databases/EventHubConnections/write | Записывает ресурс соединений концентратора событий. |
> | Action | Microsoft. Kusto/Clusters/databases/Евенсубконнектионвалидатион/Action | Проверяет подключение концентратора событий базы данных. |
> | Action | Microsoft. Kusto/Clusters/databases/ЛистпринЦипалс/Action | Выводит список участников базы данных. |
> | Action | Microsoft.Kusto/Clusters/Databases/read | Читает ресурс базы данных. |
> | Action | Microsoft. Kusto/Clusters/databases/РемовепринЦипалс/Action | Удаляет участников базы данных. |
> | Action | Microsoft.Kusto/Clusters/Databases/write | Записывает ресурс базы данных. |
> | Action | Microsoft. Kusto/Clusters/Deactivate/действие | Останавливает кластер. |
> | Action | Microsoft.Kusto/Clusters/delete | Удаляет кластерный ресурс. |
> | Action | Microsoft.Kusto/Clusters/read | Читает кластерный ресурс. |
> | Action | Microsoft. Kusto/Clusters/SKU/чтение | Считывает ресурс SKU кластера. |
> | Action | Microsoft. Kusto/Clusters/запуск/действие | Запускает кластер. |
> | Action | Microsoft. Kusto/Clusters/завершение/действие | Останавливает кластер. |
> | Action | Microsoft.Kusto/Clusters/write | Записывает кластерный ресурс. |
> | Action | Microsoft. Kusto/Locations/Чеккнамеаваилабилити/Action | Проверяет доступность имени ресурса. |
> | Action | Microsoft.Kusto/locations/operationresults/read | Чтение ресурсов операций |
> | Action | Microsoft.Kusto/Operations/read | Чтение ресурсов операций |
> | Action | Microsoft. Kusto/Register/действие | Регистрирует подписку в поставщике ресурсов Kusto. |
> | Action | Microsoft. Kusto/SKU/чтение | Считывает ресурс номера SKU. |
> | Action | Microsoft. Kusto/отменить регистрацию или действие | Отменяет регистрацию подписки на поставщик ресурсов Kusto. |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.LabServices/labAccounts/CreateLab/action | Создание лаборатории в учетной записи лаборатории. |
> | Action | Microsoft.LabServices/labAccounts/delete | Удаление учетных записей лабораторий. |
> | Action | Microsoft.LabServices/labAccounts/galleryImages/delete | Удаляет образы коллекции. |
> | Action | Microsoft.LabServices/labAccounts/galleryImages/read | Считывает образы из коллекции. |
> | Action | Microsoft.LabServices/labAccounts/galleryImages/write | Добавление или изменение образов из коллекции. |
> | Action | Microsoft.LabServices/labAccounts/GetRegionalAvailability/action | Позволяет получать сведения о доступности по регионам для каждой категории размеров, настроенной в учетной записи лаборатории. |
> | Action | Microsoft. Лабсервицес/Лабаккаунтс/Идлешутдовнс/Delete | Удалите идлешутдовнс. |
> | Action | Microsoft. Лабсервицес/Лабаккаунтс/Идлешутдовнс/Read | Чтение идлешутдовнс. |
> | Action | Microsoft. Лабсервицес/Лабаккаунтс/Идлешутдовнс/запись | Добавьте или измените идлешутдовнс. |
> | Action | Microsoft.LabServices/labAccounts/labs/AddUsers/action | Добавление пользователей в лабораторию |
> | Action | Microsoft.LabServices/labAccounts/labs/delete | Удаляет лаборатории. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/ClaimAny/action | Утверждение случайной среды пользователя в параметрах среды. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/delete | Удаление параметра среды. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Claim/action | Утверждение среды и назначение ее пользователю. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/delete | Удаляет среды. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/read | Считывает среды. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/ResetPassword/action | Сброс пароля пользователя в среде |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Start/action | Запускает среду, запустив все ресурсы в ней. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/Stop/action | Останавливает выполнение среды, остановив все ресурсы в ней. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/environments/write | Добавляет или изменяет среды. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/Publish/action | Подготовка и отзыв необходимых ресурсов для параметра среды на основе текущего состояния параметра лаборатории или среды. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/read | Чтение параметра среды. |
> | Action | Microsoft. Лабсервицес/Лабаккаунтс/Labs/environmentSettings/Ресетпассворд/действие | Сбрасывает пароль на виртуальной машине шаблона. |
> | Action | Microsoft. Лабсервицес/Лабаккаунтс/Labs/environmentSettings/Савеимаже/действие | Сохраняет образ текущего шаблона в общей коллекции в учетной записи лаборатории. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/delete | Удаляет расписания. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/read | Считывает расписания. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/schedules/write | Добавляет или изменяет расписания. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/Start/action | Запускает шаблон, запустив все ресурсы внутри него. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/Stop/action | Останавливает шаблон путем остановки всех ресурсов внутри него. |
> | Action | Microsoft.LabServices/labAccounts/labs/environmentSettings/write | Добавление или изменение параметра среды. |
> | Action | Microsoft.LabServices/labAccounts/labs/read | Считывает лаборатории. |
> | Action | Microsoft.LabServices/labAccounts/labs/Register/action | Регистрация в управляемой лаборатории. |
> | Action | Microsoft.LabServices/labAccounts/labs/SendEmail/action | Отправка электронного письма со ссылкой для регистрации в лаборатории. |
> | Action | Microsoft.LabServices/labAccounts/labs/users/delete | Удаление пользователей. |
> | Action | Microsoft.LabServices/labAccounts/labs/users/read | Чтение пользователей. |
> | Action | Microsoft.LabServices/labAccounts/labs/users/write | Добавление или изменение пользователей. |
> | Action | Microsoft.LabServices/labAccounts/labs/write | Добавляет или изменяет лаборатории. |
> | Action | Microsoft.LabServices/labAccounts/read | Чтение учетных записей лабораторий. |
> | Action | Microsoft.LabServices/labAccounts/sharedGalleries/delete | Удаление общих коллекций. |
> | Action | Microsoft.LabServices/labAccounts/sharedGalleries/read | Чтение общих коллекций. |
> | Action | Microsoft.LabServices/labAccounts/sharedGalleries/write | Добавление или изменение общих коллекций. |
> | Action | Microsoft.LabServices/labAccounts/sharedImages/delete | Удаление общих образов. |
> | Action | Microsoft.LabServices/labAccounts/sharedImages/read | Чтение общих образов. |
> | Action | Microsoft.LabServices/labAccounts/sharedImages/write | Добавление или изменение общих образов. |
> | Action | Microsoft.LabServices/labAccounts/write | Добавление или изменение учетных записей лабораторий. |
> | Action | Microsoft.LabServices/locations/operations/read | Считывает операции. |
> | Action | Microsoft.LabServices/register/action | Регистрация подписки |
> | Action | Microsoft.LabServices/users/GetEnvironment/action | Получает сведения о виртуальных машинах. |
> | Action | Microsoft.LabServices/users/GetOperationBatchStatus/action | Получает состояние пакетной операции. |
> | Action | Microsoft.LabServices/users/GetOperationStatus/action | Получение состояния длительной операции. |
> | Action | Microsoft.LabServices/users/GetPersonalPreferences/action | Получение личных настроек для пользователя |
> | Action | Microsoft. Лабсервицес/Users/Листалленвиронментс/Action | Список всех сред для пользователя |
> | Action | Microsoft.LabServices/users/ListEnvironments/action | Перечисление сред для пользователя. |
> | Action | Microsoft.LabServices/users/ListLabs/action | Перечисление лабораторий для пользователя. |
> | Action | Microsoft.LabServices/users/Register/action | Регистрация пользователя в управляемой лаборатории. |
> | Action | Microsoft.LabServices/users/ResetPassword/action | Сброс пароля пользователя в среде |
> | Action | Microsoft.LabServices/users/StartEnvironment/action | Запускает среду, запустив все ресурсы в ней. |
> | Action | Microsoft.LabServices/users/StopEnvironment/action | Останавливает выполнение среды, остановив все ресурсы в ней. |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Logic/integrationAccounts/agreements/delete | Удаляет соглашение в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/agreements/listContentCallbackUrl/action | Возвращает URL-адрес обратного вызова для содержимого соглашения в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/agreements/read | Считывает соглашение в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/agreements/write | Создает или обновляет соглашение в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/delete | Удаляет сборку в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/listContentCallbackUrl/action | Возвращает URL-адрес обратного вызова для содержимого сборки в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/read | Считывает сборку в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/assemblies/write | Создает или обновляет сборку в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/batchConfigurations/delete | Удаляет конфигурацию пакета в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/batchConfigurations/read | Считывает конфигурацию пакета в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/batchConfigurations/write | Создает или обновляет конфигурацию пакета в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/certificates/delete | Удаляет сертификат в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/certificates/read | Считывает сертификат в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/certificates/write | Создает или обновляет сертификат в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/delete | Удаляет учетную запись интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/join/action | Присоединяется к учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/listCallbackUrl/action | Возвращает URL-адрес обратного вызова для учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/listKeyVaultKeys/action | Возвращает ключи из хранилища ключей. |
> | Action | Microsoft.Logic/integrationAccounts/logTrackingEvents/action | Записывает в журнал события отслеживания в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/maps/delete | Удаляет сопоставление в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/maps/listContentCallbackUrl/action | Возвращает URL-адрес обратного вызова для содержимого сопоставления в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/maps/read | Считывает сопоставление в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/maps/write | Создает или обновляет сопоставление в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/partners/delete | Удаляет партнера в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/partners/listContentCallbackUrl/action | Возвращает URL-адрес обратного вызова для содержимого партнера в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/partners/read | Чтение партнера в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/partners/write | Создает или обновляет партнера в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/providers/Microsoft.Insights/logDefinitions/read | Считывает определения журналов для учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/read | Считывает учетную запись интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/regenerateAccessKey/action | Повторное создание секретов для ключей доступа. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/delete | Удаляет схему в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/listContentCallbackUrl/action | Возвращает URL-адрес обратного вызова для содержимого схемы в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/read | Считывает схему в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/schemas/write | Создает или обновляет схему в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/sessions/delete | Удаляет сеанс в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/sessions/read | Считывает конфигурацию пакета в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/sessions/write | Создает или обновляет сеанс в учетной записи интеграции. |
> | Action | Microsoft.Logic/integrationAccounts/write | Создает или обновляет учетную запись интеграции. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/delete | Удаляет среду службы интеграции. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/join/action | Присоединяет к среде службы интеграции. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/managedApis/apiOperations/read | Считывает операцию управляемого API для среды службы интеграции. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/managedApis/read | Считывает управляемый API для среды службы интеграции. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/operationStatuses/read | Считывает состояния операции среды службы интеграции. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/providers/Microsoft.Insights/metricDefinitions/read | Считывает определение метрики среды службы интеграции. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/read | Читает среду службы интеграции. |
> | Action | Microsoft.Logic/integrationServiceEnvironments/write | Создает или обновляет среду службы интеграции. |
> | Action | Microsoft.Logic/locations/workflows/recommendOperationGroups/action | Получает группы операций, рекомендуемых для рабочего процесса. |
> | Action | Microsoft.Logic/locations/workflows/validate/action | Проверяет рабочий процесс. |
> | Action | Microsoft.Logic/operations/read | Получение операции. |
> | Action | Microsoft.Logic/register/action | Регистрирует поставщик ресурсов Microsoft.Logic для заданной подписки. |
> | Action | Microsoft.Logic/workflows/accessKeys/delete | Удаление ключа доступа. |
> | Action | Microsoft.Logic/workflows/accessKeys/list/action | Список секретов для ключей доступа. |
> | Action | Microsoft.Logic/workflows/accessKeys/read | Считывание ключа доступа. |
> | Action | Microsoft.Logic/workflows/accessKeys/regenerate/action | Повторное создание секретов для ключей доступа. |
> | Action | Microsoft.Logic/workflows/accessKeys/write | Создание или изменение ключа доступа. |
> | Action | Microsoft.Logic/workflows/delete | Удаление рабочего процесса. |
> | Action | Microsoft.Logic/workflows/disable/action | Отключение рабочего процесса. |
> | Action | Microsoft.Logic/workflows/enable/action | Включение рабочего процесса. |
> | Action | Microsoft.Logic/workflows/listCallbackUrl/action | Получает URL-адрес обратного вызова для рабочего процесса. |
> | Action | Microsoft.Logic/workflows/listSwagger/action | Получает определения Swagger рабочего процесса. |
> | Action | Microsoft.Logic/workflows/move/action | Перемещение рабочего процесса от текущих идентификатора подписки, группы ресурсов или имени к другим идентификатору, группе или имени. |
> | Action | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/read | Считывание параметров диагностики рабочих процессов. |
> | Action | Microsoft.Logic/workflows/providers/Microsoft.Insights/diagnosticSettings/write | Создание или изменение параметров диагностики для рабочего процесса. |
> | Action | Microsoft.Logic/workflows/providers/Microsoft.Insights/logDefinitions/read | Считывание определений журналов рабочих процессов. |
> | Action | Microsoft.Logic/workflows/providers/Microsoft.Insights/metricDefinitions/read | Считывание определений метрик рабочих процессов. |
> | Action | Microsoft.Logic/workflows/read | Считывание рабочего процесса. |
> | Action | Microsoft.Logic/workflows/regenerateAccessKey/action | Повторное создание секретов для ключей доступа. |
> | Action | Microsoft.Logic/workflows/run/action | Начало запуска рабочего процесса. |
> | Action | Microsoft.Logic/workflows/runs/actions/listExpressionTraces/action | Получает трассировки выражений действия запуска рабочего процесса. |
> | Action | Microsoft.Logic/workflows/runs/actions/read | Считывание действия запуска рабочего процесса. |
> | Action | Microsoft.Logic/workflows/runs/actions/repetitions/listExpressionTraces/action | Получает трассировки выражений повторов действия для запуска рабочего процесса. |
> | Action | Microsoft.Logic/workflows/runs/actions/repetitions/read | Считывание повтора действия для запуска рабочего процесса. |
> | Action | Microsoft.Logic/workflows/runs/actions/repetitions/requestHistories/read | Считывает журнал запросов действий повторения запуска рабочего процесса. |
> | Action | Microsoft.Logic/workflows/runs/actions/requestHistories/read | Считывает журнал запросов действий запуска рабочего процесса. |
> | Action | Microsoft.Logic/workflows/runs/actions/scoperepetitions/read | Считывание повтора области действия для запуска рабочего процесса. |
> | Action | Microsoft.Logic/workflows/runs/cancel/action | Отмена запуска рабочего процесса. |
> | Action | Microsoft. Logic/рабочие процессы/запуски и удаление | Удаляет выполнение рабочего процесса. |
> | Action | Microsoft.Logic/workflows/runs/operations/read | Считывание состояния для операции запуска рабочего процесса. |
> | Action | Microsoft.Logic/workflows/runs/read | Считывание запуска рабочего процесса. |
> | Action | Microsoft.Logic/workflows/suspend/action | Приостанавливает рабочий процесс. |
> | Action | Microsoft.Logic/workflows/triggers/histories/read | Считывание журналов триггеров. |
> | Action | Microsoft.Logic/workflows/triggers/histories/resubmit/action | Отправляет триггер рабочего процесса еще раз. |
> | Action | Microsoft.Logic/workflows/triggers/listCallbackUrl/action | Получает URL-адрес обратного вызова для триггера. |
> | Action | Microsoft.Logic/workflows/triggers/read | Считывание триггеров. |
> | Action | Microsoft.Logic/workflows/triggers/reset/action | Сбрасывает триггер. |
> | Action | Microsoft.Logic/workflows/triggers/run/action | Выполнение триггера. |
> | Action | Microsoft.Logic/workflows/triggers/setState/action | Задает состояние триггера. |
> | Action | Microsoft.Logic/workflows/validate/action | Проверяет рабочий процесс. |
> | Action | Microsoft.Logic/workflows/versions/read | Считывание версии рабочего процесса. |
> | Action | Microsoft.Logic/workflows/versions/triggers/listCallbackUrl/action | Получает URL-адрес обратного вызова для триггера. |
> | Action | Microsoft.Logic/workflows/write | Создание или изменение рабочего процесса. |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/move/action | Перемещает связь плана предложения машинного обучения. |
> | Action | Microsoft.MachineLearning/commitmentPlans/commitmentAssociations/read | Считывает связь плана предложения машинного обучения. |
> | Action | Microsoft.MachineLearning/commitmentPlans/delete | Удаляет план предложения машинного обучения. |
> | Action | Microsoft.MachineLearning/commitmentPlans/join/action | Присоединяет план предложения машинного обучения. |
> | Action | Microsoft.MachineLearning/commitmentPlans/read | Считывает план предложения машинного обучения. |
> | Action | Microsoft.MachineLearning/commitmentPlans/write | Создает или обновляет план предложения машинного обучения. |
> | Action | Microsoft.MachineLearning/locations/operationresults/read | Получение результата операции машинного обучения. |
> | Action | Microsoft.MachineLearning/locations/operationsstatus/read | Получение состояния текущей операции машинного обучения. |
> | Action | Microsoft.MachineLearning/operations/read | Получение операций машинного обучения. |
> | Action | Microsoft.MachineLearning/register/action | Регистрирует подписку для поставщика ресурсов веб-службы машинного обучения и позволяет создавать веб-службы. |
> | Action | Microsoft.MachineLearning/skus/read | Получение номеров SKU плана предложения машинного обучения. |
> | Action | Microsoft.MachineLearning/webServices/action | Создает свойства региональной веб-службы для поддерживаемых регионов. |
> | Action | Microsoft.MachineLearning/webServices/delete | Удаляет веб-службу машинного обучения. |
> | Action | Microsoft.MachineLearning/webServices/listkeys/read | Получает ключи к веб-службе машинного обучения. |
> | Action | Microsoft.MachineLearning/webServices/read | Считывает веб-службу машинного обучения. |
> | Action | Microsoft.MachineLearning/webServices/write | Создает или обновляет веб-службу машинного обучения. |
> | Action | Microsoft.MachineLearning/Workspaces/delete | Удаляет рабочую область машинного обучения. |
> | Action | Microsoft.MachineLearning/Workspaces/listworkspacekeys/action | Выводит список ключей для рабочей области машинного обучения. |
> | Action | Microsoft.MachineLearning/Workspaces/read | Считывает рабочую область машинного обучения. |
> | Action | Microsoft.MachineLearning/Workspaces/resyncstoragekeys/action | Повторно синхронизирует ключи учетной записи хранения, настроенной для рабочей области машинного обучения. |
> | Action | Microsoft.MachineLearning/Workspaces/write | Создает или обновляет рабочую область машинного обучения. |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Мачинелеарнингсервицес/Locations/компутеоператионсстатус/Read | Получает состояние определенной вычислительной операции. |
> | Action | Microsoft.MachineLearningServices/locations/usages/read | Отчет об использовании для вычислительных ресурсов Машинного обучения Azure в подписке. |
> | Action | Microsoft.MachineLearningServices/locations/vmsizes/read | Возвращает поддерживаемые размеры виртуальных машин. |
> | Action | Microsoft.MachineLearningServices/locations/workspaceOperationsStatus/read | Получает состояние определенной операции рабочей области. |
> | Action | Microsoft.MachineLearningServices/register/action | Регистрирует подписку для поставщика ресурсов служб машинного обучения. |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/delete | Удаляет вычислительные ресурсы в рабочих областях служб машинного обучения. |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/listKeys/action | Выводит список секретов для вычислительных ресурсов в рабочей области служб машинного обучения. |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/listNodes/action | Выводит список узлов для вычислительного ресурса в рабочей области Служб машинного обучения. |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/read | Получает вычислительные ресурсы в рабочих областях служб машинного обучения. |
> | Action | Microsoft.MachineLearningServices/workspaces/computes/write | Создает или обновляет вычислительные ресурсы в рабочих областях служб машинного обучения. |
> | Action | Microsoft.MachineLearningServices/workspaces/delete | Удаляет рабочие области служб машинного обучения. |
> | DataAction | Microsoft. Мачинелеарнингсервицес/рабочие области/эксперименты/запись | Создает или обновляет эксперименты в Службы машинного обучения рабочих областях |
> | Action | Microsoft.MachineLearningServices/workspaces/listKeys/action | Выводит список секретов для рабочей области служб машинного обучения. |
> | Action | Microsoft.MachineLearningServices/workspaces/read | Получает рабочие области служб машинного обучения. |
> | Action | Microsoft.MachineLearningServices/workspaces/write | Создает или обновляет рабочие области служб машинного обучения. |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. ManagedIdentity/удостоверения/чтение | Возвращает существующее удостоверение, назначенное системой |
> | Action | Microsoft.ManagedIdentity/register/action | Регистрирует подписку для поставщика ресурсов управляемых удостоверений. |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/assign/action | Действие RBAC для назначения существующего пользовательского удостоверения для ресурса. |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/delete | Удаление существующего пользовательского удостоверения. |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/read | Получение существующего пользовательского удостоверения. |
> | Action | Microsoft.ManagedIdentity/userAssignedIdentities/write | Создание существующего пользовательского удостоверения или обновление связанных с ним тегов. |

## <a name="microsoftmanagedservices"></a>Microsoft. ManagedServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. ManagedServices/Маркетплацерегистратиондефинитионс/Read | Извлекает список определений регистрации управляемых служб. |
> | Action | Microsoft. ManagedServices/Оператионстатусес/Read | Считывает состояние операции для ресурса. |
> | Action | Microsoft. ManagedServices/Register/действие | Регистрация в управляемых службах. |
> | Action | Microsoft. ManagedServices/Регистратионассигнментс/Delete | Удаляет назначение регистрации управляемых служб. |
> | Action | Microsoft. ManagedServices/Регистратионассигнментс/Read | Извлекает список назначений регистрации управляемых служб. |
> | Action | Microsoft. ManagedServices/Регистратионассигнментс/запись | Добавление или изменение назначения регистрации управляемых служб. |
> | Action | Microsoft. ManagedServices/Регистратиондефинитионс/Delete | Удаляет определение регистрации управляемых служб. |
> | Action | Microsoft. ManagedServices/Регистратиондефинитионс/Read | Извлекает список определений регистрации управляемых служб. |
> | Action | Microsoft. ManagedServices/Регистратиондефинитионс/запись | Добавление или изменение определения регистрации управляемых служб. |
> | Action | Microsoft. ManagedServices/отменить регистрацию или действие | Отмените регистрацию в управляемых службах. |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Management/checkNameAvailability/action | Проверка того, является ли имя указанной группы управления допустимым и уникальным. |
> | Action | Microsoft.Management/getEntities/action | Вывод списка всех сущностей (групп управления, подписок и т. д.) для прошедшего проверку подлинности пользователя. |
> | Action | Microsoft.Management/managementGroups/delete | Удаление группы управления. |
> | Action | Microsoft.Management/managementGroups/read | Вывод списка групп управления для пользователя, прошедшего проверку подлинности. |
> | Action | Microsoft.Management/managementGroups/subscriptions/delete | Отмена связывания подписки с группой управления. |
> | Action | Microsoft.Management/managementGroups/subscriptions/write | Связывание существующей подписки с группой управления. |
> | Action | Microsoft.Management/managementGroups/write | Создание или обновление группы управления. |
> | Action | Microsoft.Management/register/action | Регистрация указанной подписки в Microsoft.Management. |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | DataAction | Microsoft.Maps/accounts/data/read | Предоставляет доступ на чтение данных учетных записей карт. |
> | Action | Microsoft.Maps/accounts/delete | Удаление учетной записи Maps. |
> | Action | Microsoft.Maps/accounts/eventGridFilters/delete | Удаление фильтра сетки событий |
> | Action | Microsoft.Maps/accounts/eventGridFilters/read | Получение фильтра сетки событий |
> | Action | Microsoft.Maps/accounts/eventGridFilters/write | Создание или изменение фильтра сетки событий |
> | Action | Microsoft.Maps/accounts/listKeys/action | Вывод списка ключей учетной записи Maps. |
> | Action | Microsoft.Maps/accounts/read | Получение учетной записи Maps. |
> | Action | Microsoft.Maps/accounts/regenerateKey/action | Создание нового первичного или вторичного ключа учетной записи Maps. |
> | Action | Microsoft.Maps/accounts/write | Создание или обновление учетной записи Maps. |
> | Action | Microsoft. Maps/Operations/Read | Чтение операций поставщика |
> | Action | Microsoft.Maps/register/action | Регистрация поставщика. |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/read | Возвращает соглашение. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/agreements/write | Принимает подписанное соглашение. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/importImage/action | Импортирует изображение в ACR конечного пользователя. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/read | Возвращает конфигурацию. |
> | Action | Microsoft.Marketplace/offerTypes/publishers/offers/plans/configs/write | Сохраняет конфигурацию. |
> | Action | Microsoft.Marketplace/register/action | Регистрирует поставщик ресурсов Microsoft.Marketplace в подписке. |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/delete | Выполнение операции DELETE в ресурсе классической службы разработки. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/listSecrets/action | Получение ключей управления для ресурса классической службы разработки. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/listSingleSignOnToken/action | Получение URL-адреса единого входа для классической службы разработки. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/read | Выполнение операции GET в классической службе разработки. |
> | Action | Microsoft.MarketplaceApps/ClassicDevServices/regenerateKey/action | Создание ключей управления для ресурса классической службы разработки. |
> | Action | Microsoft.MarketplaceApps/Operations/read | Чтение операций для всех типов ресурсов. |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.MarketplaceOrdering/agreements/offers/plans/cancel/action | Отменяет соглашение для заданного элемента Marketplace. |
> | Action | Microsoft.MarketplaceOrdering/agreements/offers/plans/read | Возвращает соглашение для заданного элемента Marketplace. |
> | Action | Microsoft.MarketplaceOrdering/agreements/offers/plans/sign/action | Подписывает соглашение для заданного элемента Marketplace. |
> | Action | Microsoft.MarketplaceOrdering/agreements/read | Возвращение всех соглашений в указанной подписке. |
> | Action | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/read | Получение соглашения для заданного элемента виртуальной машины Marketplace. |
> | Action | Microsoft.MarketplaceOrdering/offertypes/publishers/offers/plans/agreements/write | Подписывание соглашения или его отмена для заданного элемента виртуальной машины Marketplace. |
> | Action | Microsoft. Маркетплацеордеринг/операции/чтение | Вывод списка всех возможных операций в API |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Media/checknameavailability/action | Проверка доступности имени учетной записи служб мультимедиа. |
> | Action | Microsoft.Media/locations/checkNameAvailability/action | Проверка доступности имени учетной записи служб мультимедиа. |
> | Action | Microsoft.Media/mediaservices/accountfilters/delete | Удаление любого фильтра учетных записей. |
> | Action | Microsoft.Media/mediaservices/accountfilters/read | Чтение любого фильтра учетных записей. |
> | Action | Microsoft.Media/mediaservices/accountfilters/write | Создание или обновление фильтра учетных записей. |
> | Action | Microsoft.Media/mediaservices/assets/assetfilters/delete | Удаление любого фильтра ресурсов. |
> | Action | Microsoft.Media/mediaservices/assets/assetfilters/read | Чтение любого фильтра ресурсов. |
> | Action | Microsoft.Media/mediaservices/assets/assetfilters/write | Создание или обновление фильтра ресурсов. |
> | Action | Microsoft.Media/mediaservices/assets/delete | Удаление ресурса. |
> | Action | Microsoft.Media/mediaservices/assets/getEncryptionKey/action | Получение ключа шифрования ресурса. |
> | Action | Microsoft.Media/mediaservices/assets/listContainerSas/action | Вывод списка подписанных URL-адресов контейнера ресурсов. |
> | Action | Microsoft.Media/mediaservices/assets/listStreamingLocators/action | Вывод списка указателей потоковой передачи для ресурса. |
> | Action | Microsoft.Media/mediaservices/assets/read | Чтение ресурсов. |
> | Action | Microsoft.Media/mediaservices/assets/write | Создание или обновление ресурсов. |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/delete | Удаление политики ключей содержимого. |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/getPolicyPropertiesWithSecrets/action | Получение свойств политики с секретами. |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/read | Чтение политики ключей содержимого. |
> | Action | Microsoft.Media/mediaservices/contentKeyPolicies/write | Создание или обновление политики ключей содержимого. |
> | Action | Microsoft.Media/mediaservices/delete | Удаление учетной записи служб мультимедиа. |
> | Action | Microsoft.Media/mediaservices/eventGridFilters/delete | Удаление фильтра сетки событий. |
> | Action | Microsoft.Media/mediaservices/eventGridFilters/read | Чтение фильтра сетки событий. |
> | Action | Microsoft.Media/mediaservices/eventGridFilters/write | Создание или обновление фильтра сетки событий. |
> | Action | Microsoft.Media/mediaservices/liveEventOperations/read | Чтение операции динамического события. |
> | Action | Microsoft.Media/mediaservices/liveEvents/delete | Удаление динамического события. |
> | Action | Microsoft.Media/mediaservices/liveEvents/liveOutputs/delete | Удаление выходных данных динамического события. |
> | Action | Microsoft.Media/mediaservices/liveEvents/liveOutputs/read | Чтение выходных данных динамического события. |
> | Action | Microsoft.Media/mediaservices/liveEvents/liveOutputs/write | Создание или обновление выходных данных динамического события. |
> | Action | Microsoft.Media/mediaservices/liveEvents/read | Чтение динамического события. |
> | Action | Microsoft.Media/mediaservices/liveEvents/reset/action | Сброс операции динамического события. |
> | Action | Microsoft.Media/mediaservices/liveEvents/start/action | Запуск операции динамического события. |
> | Action | Microsoft.Media/mediaservices/liveEvents/stop/action | Остановка выполнения операции динамического события. |
> | Action | Microsoft.Media/mediaservices/liveEvents/write | Создание или обновление динамического события. |
> | Action | Microsoft.Media/mediaservices/liveOutputOperations/read | Чтение операции с выходными данными динамического события. |
> | Action | Microsoft.Media/mediaservices/read | Удаление учетной записи служб мультимедиа. |
> | Action | Microsoft.Media/mediaservices/streamingEndpointOperations/read | Чтение операции конечной точки потоковой передачи. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/delete | Удаление конечной точки потоковой передачи. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/read | Чтение конечной точки потоковой передачи. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/scale/action | Масштабирование операции конечной точки потоковой передачи. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/start/action | Запуск операции конечной точки потоковой передачи. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/stop/action | Остановка выполнения операции конечной точки потоковой передачи. |
> | Action | Microsoft.Media/mediaservices/streamingEndpoints/write | Создание или обновление конечной точки потоковой передачи. |
> | Action | Microsoft.Media/mediaservices/streamingLocators/delete | Удаление потокового указателя. |
> | Action | Microsoft.Media/mediaservices/streamingLocators/listContentKeys/action | Вывод списка ключей содержимого. |
> | Action | Microsoft.Media/mediaservices/streamingLocators/listPaths/action | Вывод списка путей. |
> | Action | Microsoft.Media/mediaservices/streamingLocators/read | Считывание потокового указателя. |
> | Action | Microsoft.Media/mediaservices/streamingLocators/write | Создание или обновление потокового указателя. |
> | Action | Microsoft.Media/mediaservices/streamingPolicies/delete | Удаление политики потоковой передачи. |
> | Action | Microsoft.Media/mediaservices/streamingPolicies/read | Чтение политики потоковой передачи. |
> | Action | Microsoft.Media/mediaservices/streamingPolicies/write | Создание или обновление политики потоковой передачи. |
> | Action | Microsoft.Media/mediaservices/syncStorageKeys/action | Синхронизация ключей хранилища для присоединенной учетной записи хранения Azure. |
> | Action | Microsoft.Media/mediaservices/transforms/delete | Удаление преобразования. |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/cancelJob/action | Отменить задание |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/delete | Удаление задания. |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/read | Считывание задания. |
> | Action | Microsoft.Media/mediaservices/transforms/jobs/write | Создание или обновление задания. |
> | Action | Microsoft.Media/mediaservices/transforms/read | Чтение преобразования. |
> | Action | Microsoft.Media/mediaservices/transforms/write | Создание или обновление преобразования. |
> | Action | Microsoft.Media/mediaservices/write | Создание или обновление учетной записи служб мультимедиа. |
> | Action | Microsoft.Media/operations/read | Возвращает доступные операции. |
> | Action | Microsoft.Media/register/action | Регистрация подписки для поставщика ресурсов служб мультимедиа и разрешение создания учетных записей служб мультимедиа. |
> | Action | Microsoft.Media/unregister/action | Отмена регистрации подписки для поставщика ресурсов Media Services. |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Migrate/ассессментпрожектс/Ассессментоптионс/чтение | Получает параметры оценки, которые можно найти в указанном расположении |
> | Action | Microsoft. Migrate/ассессментпрожектс/оценки/чтение | Составление списков оценок в пределах проекта |
> | Action | Microsoft. Migrate/ассессментпрожектс/удалить | Удаляет проект оценки. |
> | Action | Microsoft. Migrate/ассессментпрожектс/группы/оценки/ассесседмачинес/чтение | Получить свойства компьютера, прошедшего оценку |
> | Action | Microsoft. Migrate/ассессментпрожектс/группы/оценки/удаление | Удаляет оценку |
> | Action | Microsoft. Migrate/ассессментпрожектс/группы/оценки/довнлоадурл/действие | Загружает URL-адрес отчета оценки |
> | Action | Microsoft. Migrate/ассессментпрожектс/группы/оценки/чтение | Получает свойства оценки |
> | Action | Microsoft. Migrate/ассессментпрожектс/группы/оценки/запись | Создает новую оценку или обновляет существующую |
> | Action | Microsoft. Migrate/ассессментпрожектс/группы/удалить | Удаляет группу |
> | Action | Microsoft. Migrate/ассессментпрожектс/группы/чтение | Получить свойства группы |
> | Action | Microsoft. Migrate/ассессментпрожектс/группы/Упдатемачинес/действие | Обновление группы путем добавления или удаления компьютеров |
> | Action | Microsoft. Migrate/ассессментпрожектс/группы/запись | Создает новую группу или обновляет существующую |
> | Action | Microsoft. Migrate/ассессментпрожектс/хипервколлекторс/удаление | Удаляет сборщик HyperV. |
> | Action | Microsoft. Migrate/ассессментпрожектс/хипервколлекторс/чтение | Возвращает свойства сборщика HyperV. |
> | Action | Microsoft. Migrate/ассессментпрожектс/хипервколлекторс/запись | Создает новый сборщик HyperV или обновляет существующий сборщик HyperV. |
> | Action | Microsoft. Migrate/ассессментпрожектс/Machines/Read | Получает свойства компьютера |
> | Action | Microsoft. Migrate/ассессментпрожектс/чтение | Возвращает свойства проекта оценки. |
> | Action | Microsoft. Migrate/ассессментпрожектс/вмвареколлекторс/удаление | Удаляет сборщик VMware. |
> | Action | Microsoft. Migrate/ассессментпрожектс/вмвареколлекторс/чтение | Возвращает свойства сборщика VMware. |
> | Action | Microsoft. Migrate/ассессментпрожектс/вмвареколлекторс/запись | Создает новый сборщик VMware или обновляет существующий сборщик VMware. |
> | Action | Microsoft. Migrate/ассессментпрожектс/запись | Создает новый проект оценки или обновляет существующий проект оценки. |
> | Action | Microsoft.Migrate/locations/assessmentOptions/read | Получает параметры оценки, которые можно найти в указанном расположении |
> | Action | Microsoft.Migrate/locations/checknameavailability/action | Проверка доступности имени ресурса для заданной подписки в заданном расположении |
> | Action | Microsoft. Migrate/мигратепрожектс/Датабасеинстанцес/чтение | Возвращает свойства экземпляра базы данных. |
> | Action | Microsoft. Migrate/мигратепрожектс/databases/Read | Возвращает свойства базы данных. |
> | Action | Microsoft. Migrate/мигратепрожектс/удалить | Удаляет проект миграции. |
> | Action | Microsoft. Migrate/мигратепрожектс/Machines/Read | Получает свойства компьютера |
> | Action | Microsoft. Migrate/мигратепрожектс/Мигративентс/удаление | Удаляет событие миграции. |
> | Action | Microsoft. Migrate/мигратепрожектс/Мигративентс/чтение | Возвращает свойства событий миграции. |
> | Action | Microsoft.Migrate/migrateprojects/read | Возвращает свойства проекта "миграция" |
> | Action | Microsoft. Migrate/мигратепрожектс/Регистертул/действие | Регистрация средства в проекте миграции |
> | Action | Microsoft. Migrate/мигратепрожектс/Solutions/Клеанупдата/Action | Очистка данных решения "миграция проекта" |
> | Action | Microsoft. Migrate/мигратепрожектс/Solutions/Delete | Удаляет решение "миграция проекта" |
> | Action | Microsoft.Migrate/migrateprojects/solutions/getconfig/action | Получает конфигурацию решения проекта миграции. |
> | Action | Microsoft.Migrate/migrateprojects/solutions/read | Возвращает свойства решения "миграция проекта" |
> | Action | Microsoft. Migrate/мигратепрожектс/решения/запись | Создает новое решение миграции проектов или обновляет существующее решение по переносу проекта |
> | Action | Microsoft. Migrate/мигратепрожектс/запись | Создает новый проект миграции или обновляет существующий. |
> | Action | Microsoft.Migrate/Operations/read | Перечисляет операции, доступные в поставщике ресурсов Microsoft.Migrate |
> | Action | Microsoft.Migrate/projects/assessments/read | Составление списков оценок в пределах проекта |
> | Action | Microsoft.Migrate/projects/delete | Удаляет проект |
> | Action | Microsoft.Migrate/projects/groups/assessments/assessedmachines/read | Получить свойства компьютера, прошедшего оценку |
> | Action | Microsoft.Migrate/projects/groups/assessments/delete | Удаляет оценку |
> | Action | Microsoft.Migrate/projects/groups/assessments/downloadurl/action | Загружает URL-адрес отчета оценки |
> | Action | Microsoft.Migrate/projects/groups/assessments/read | Получает свойства оценки |
> | Action | Microsoft.Migrate/projects/groups/assessments/write | Создает новую оценку или обновляет существующую |
> | Action | Microsoft.Migrate/projects/groups/delete | Удаляет группу |
> | Action | Microsoft.Migrate/projects/groups/read | Получить свойства группы |
> | Action | Microsoft.Migrate/projects/groups/write | Создает новую группу или обновляет существующую |
> | Action | Microsoft.Migrate/projects/keys/action | Получает общие ключи проекта |
> | Action | Microsoft.Migrate/projects/machines/read | Получает свойства компьютера |
> | Action | Microsoft.Migrate/projects/read | Получает свойства проекта |
> | Action | Microsoft.Migrate/projects/write | Создает новый проект или обновляет существующий |
> | Action | Microsoft.Migrate/register/action | Регистрация подписки в поставщике ресурсов Microsoft.Migrate |

## <a name="microsoftmixedreality"></a>Microsoft. Микседреалити

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Микседреалити/Register/действие | Регистрирует подписку для поставщика ресурсов Mixed Reality. |
> | DataAction | Microsoft. Микседреалити/Спатиаланчорсаккаунтс/создание/действие | Создание пространственных привязок |
> | DataAction | Microsoft. Микседреалити/Спатиаланчорсаккаунтс/Delete | Удаление пространственных привязок |
> | DataAction | Microsoft. Микседреалити/Спатиаланчорсаккаунтс/обнаружение/чтение | Обнаружение ближайших пространственных привязок |
> | DataAction | Microsoft. Микседреалити/Спатиаланчорсаккаунтс/свойства/чтение | Получение свойств пространственных привязок |
> | Action | Microsoft. Микседреалити/Спатиаланчорсаккаунтс/providers/Microsoft. Insights/diagnosticSettings/Read | Получает параметр диагностики для Microsoft. Микседреалити/Спатиаланчорсаккаунтс |
> | Action | Microsoft. Микседреалити/Спатиаланчорсаккаунтс/providers/Microsoft. Insights/diagnosticSettings/запись | Создает или обновляет параметр диагностики для Microsoft. Микседреалити/Спатиаланчорсаккаунтс |
> | Action | Microsoft. Микседреалити/Спатиаланчорсаккаунтс/providers/Microsoft. Insights/metricDefinitions/Read | Получение доступных метрик для Microsoft. Микседреалити/Спатиаланчорсаккаунтс |
> | DataAction | Microsoft. Микседреалити/Спатиаланчорсаккаунтс/запрос/чтение | Определение пространственных привязок |
> | DataAction | Microsoft. Микседреалити/Спатиаланчорсаккаунтс/субмитдиаг/Read | Отправка диагностических данных для повышения качества службы пространственных привязок Azure |
> | DataAction | Microsoft. Микседреалити/Спатиаланчорсаккаунтс/запись | Обновить свойства пространственных привязок |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.NetApp/locations/operationresults/read | Чтение ресурса результата операции. |
> | Action | Microsoft.NetApp/locations/read | Чтение ресурса проверки доступности. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/delete | Удаление пула ресурсов. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/read | Чтение пула ресурсов. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/delete | Удаление ресурса тома. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/mountTargets/read | Чтение ресурса цели подключения. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/read | Чтение ресурса тома. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/delete | Удаление ресурса моментального снимка. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/read | Чтение ресурса моментального снимка. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/snapshots/write | Запись ресурса моментального снимка. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/volumes/write | Запись ресурса тома. |
> | Action | Microsoft.NetApp/netAppAccounts/capacityPools/write | Запись ресурса пула. |
> | Action | Microsoft.NetApp/netAppAccounts/delete | Удаляет ресурс учетной записи. |
> | Action | Microsoft.NetApp/netAppAccounts/read | Чтение ресурса учетной записи. |
> | Action | Microsoft.NetApp/netAppAccounts/write | Запись ресурса учетной записи. |
> | Action | Microsoft.NetApp/Operations/read | Чтение ресурсов операций. |

## <a name="microsoftnetwork"></a>Microsoft.Network.

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Network/applicationGatewayAvailableRequestHeaders/read | Получение доступных заголовков запроса шлюза приложений. |
> | Action | Microsoft.Network/applicationGatewayAvailableResponseHeaders/read | Получение доступного заголовка ответа шлюза приложений. |
> | Action | Microsoft.Network/applicationGatewayAvailableServerVariables/read | Получение доступных переменных сервера шлюза приложений. |
> | Action | Microsoft.Network/applicationGatewayAvailableSslOptions/predefinedPolicies/read | Предопределенная политика SSL шлюза приложений. |
> | Action | Microsoft.Network/applicationGatewayAvailableSslOptions/read | Доступные параметры SSL шлюза приложений. |
> | Action | Microsoft.Network/applicationGatewayAvailableWafRuleSets/read | Возвращает доступные наборы правил WAF шлюза приложений. |
> | Action | Microsoft.Network/applicationGateways/backendAddressPools/join/action | Присоединяет внутренний пул адресов шлюза приложений. Не оповещать. |
> | Action | Microsoft.Network/applicationGateways/backendhealth/action | Возвращает сведения о работоспособности серверной части шлюза приложений. |
> | Action | Microsoft.Network/applicationGateways/delete | Удаляет шлюз приложений. |
> | Action | Microsoft.Network/applicationGateways/read | Возвращает шлюз приложений. |
> | Action | Microsoft.Network/applicationGateways/start/action | Запускает шлюз приложений. |
> | Action | Microsoft.Network/applicationGateways/stop/action | Останавливает шлюз приложений. |
> | Action | Microsoft.Network/applicationGateways/write | Создает новый шлюз приложений или обновляет существующий. |
> | Action | Microsoft. Network/АппликатионгатевайвебаппликатионфиреваллполиЦиес/удаление | Удаляет политику WAF шлюза приложений. |
> | Action | Microsoft. Network/АппликатионгатевайвебаппликатионфиреваллполиЦиес/чтение | Возвращает политику WAF шлюза приложений. |
> | Action | Microsoft. Network/АппликатионгатевайвебаппликатионфиреваллполиЦиес/запись | Создает политику WAF для шлюза приложений или обновляет политику WAF шлюза приложений. |
> | Action | Microsoft.Network/applicationSecurityGroups/delete | Удаление группы безопасности приложений. |
> | Action | Microsoft.Network/applicationSecurityGroups/joinIpConfiguration/action | Присоединение конфигурации IP-адреса к группам безопасности приложений. Не оповещать. |
> | Action | Microsoft.Network/applicationSecurityGroups/joinNetworkSecurityRule/action | Присоединение правила безопасности к группам безопасности приложений. Не оповещать. |
> | Action | Microsoft.Network/applicationSecurityGroups/listIpConfigurations/action | Вывод списка конфигураций IP-адресов в ApplicationSecurityGroup. |
> | Action | Microsoft.Network/applicationSecurityGroups/read | Получение идентификатора группы безопасности приложений. |
> | Action | Microsoft.Network/applicationSecurityGroups/write | Создание группы безопасности приложений или обновление существующей группы. |
> | Action | Microsoft.Network/azureFirewallFqdnTags/read | Получает теги полного доменного имени службы "Брандмауэр Azure" |
> | Action | Microsoft.Network/azurefirewalls/delete | Удаляет службу "Брандмауэр Azure". |
> | Action | Microsoft.Network/azurefirewalls/read | Получает службу "Брандмауэр Azure". |
> | Action | Microsoft.Network/azurefirewalls/write | Создает или обновляет службу "Брандмауэр Azure". |
> | Action | Microsoft.Network/bastionHosts/delete | Удаление узла-бастиона |
> | Action | Microsoft.Network/bastionHosts/read | Получение узла-бастиона |
> | Action | Microsoft.Network/bastionHosts/write | Создание или обновление узла-бастиона |
> | Action | Microsoft.Network/bgpServiceCommunities/read | Возвращает сообщества службы BGP. |
> | Action | Microsoft.Network/checkFrontDoorNameAvailability/action | Проверка доступности для имени Front Door. |
> | Action | Microsoft.Network/checkTrafficManagerNameAvailability/action | Проверяет доступность относительного DNS-имени диспетчера трафика. |
> | Action | Microsoft.Network/connections/delete | Удаляет подключение шлюза виртуальной сети. |
> | Action | Microsoft.Network/connections/read | Возвращает подключение шлюза виртуальной сети. |
> | Action | Microsoft.Network/connections/revoke/action | Помечает подключение ExpressRoute как отмененное. |
> | Action | Microsoft.Network/connections/sharedkey/action | Получение общего ключа для подключения шлюза виртуальной сети. |
> | Action | Microsoft.Network/connections/sharedKey/read | Возвращает общий ключ для подключения шлюза виртуальной сети. |
> | Action | Microsoft.Network/connections/sharedKey/write | Создает новый общий ключ для подключения шлюза виртуальной сети или обновляет существующий. |
> | Action | Microsoft.Network/connections/vpndeviceconfigurationscript/action | Получение конфигурации VPN-устройства для подключения шлюза виртуальной сети. |
> | Action | Microsoft.Network/connections/write | Создает новое подключение шлюза виртуальной сети или обновляет существующее. |
> | Action | Microsoft.Network/ddosCustomPolicies/delete | Удаление политики, настроенной с учетом DDoS |
> | Action | Microsoft.Network/ddosCustomPolicies/read | Получение определения политики, настроенной с учетом DDoS |
> | Action | Microsoft.Network/ddosCustomPolicies/write | Создание политики, настроенной с учетом DDoS, или обновление такой существующей политики |
> | Action | Microsoft.Network/ddosProtectionPlans/delete | Удаление плана защиты от атак DDoS. |
> | Action | Microsoft.Network/ddosProtectionPlans/join/action | Присоединяет план защиты от атак DDoS. Не оповещать. |
> | Action | Microsoft.Network/ddosProtectionPlans/read | Получение плана защиты от атак DDoS. |
> | Action | Microsoft.Network/ddosProtectionPlans/write | Создание или обновление плана защиты от атак DDoS.  |
> | Action | Microsoft.Network/dnsoperationresults/read | Возвращает результаты операции DNS. |
> | Action | Microsoft.Network/dnsoperationstatuses/read | Возвращает состояние операции DNS.  |
> | Action | Microsoft.Network/dnszones/A/delete | Удаляет из зоны DNS набор записей типа A с заданным именем. |
> | Action | Microsoft.Network/dnszones/A/read | Возвращает набор записей типа A в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft.Network/dnszones/A/write | Создает или обновляет набора записей типа A в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft.Network/dnszones/AAAA/delete | Удаляет из зоны DNS набор записей типа AAAA с заданным именем. |
> | Action | Microsoft.Network/dnszones/AAAA/read | Возвращает набор записей типа AAAA в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft.Network/dnszones/AAAA/write | Создает или обновляет набора записей типа AAAA в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft.Network/dnszones/all/read | Возвращает набор записей DNS разного типа. |
> | Action | Microsoft.Network/dnszones/CAA/delete | Удаление из зоны DNS набора записей типа CAA с заданным именем. |
> | Action | Microsoft.Network/dnszones/CAA/read | Получение набора записей типа CAA в формате JSON. Набор записей содержит срок жизни, теги и ETag. |
> | Action | Microsoft.Network/dnszones/CAA/write | Создание или обновление набора записей типа CAA в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft.Network/dnszones/CNAME/delete | Удаляет из зоны DNS набор записей типа CNAME с заданным именем. |
> | Action | Microsoft.Network/dnszones/CNAME/read | Возвращает набор записей типа CNAME в формате JSON. Набор записей содержит срок жизни, теги и ETag. |
> | Action | Microsoft.Network/dnszones/CNAME/write | Создает или обновляет набора записей типа CNAME в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft.Network/dnszones/delete | Удаляет зону DNS в формате JSON. К свойствам зоны относятся tags, etag, numberOfRecordSets и maxNumberOfRecordSets. |
> | Action | Microsoft.Network/dnszones/MX/delete | Удаляет из зоны DNS набор записей типа MX с заданным именем. |
> | Action | Microsoft.Network/dnszones/MX/read | Возвращает набор записей типа MX в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft.Network/dnszones/MX/write | Создает или обновляет набора записей типа MX в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft.Network/dnszones/NS/delete | Удаляет набор записей DNS типа NS. |
> | Action | Microsoft.Network/dnszones/NS/read | Возвращает набор записей DNS типа NS. |
> | Action | Microsoft.Network/dnszones/NS/write | Создает или обновляет набор записей DNS типа NS. |
> | Action | Microsoft.Network/dnszones/PTR/delete | Удаляет из зоны DNS набор записей типа PTR с заданным именем. |
> | Action | Microsoft.Network/dnszones/PTR/read | Возвращает набор записей типа PTR в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft.Network/dnszones/PTR/write | Создает или обновляет набора записей типа PTR в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft.Network/dnszones/read | Возвращает зону DNS в формате JSON. К свойствам зоны относятся tags, etag, numberOfRecordSets и maxNumberOfRecordSets. Обратите внимание, что эта команда не извлекает наборы записей, содержащиеся в зоне. |
> | Action | Microsoft.Network/dnszones/recordsets/read | Возвращает набор записей DNS разного типа. |
> | Action | Microsoft.Network/dnszones/SOA/read | Возвращает набор записей DNS типа SOA. |
> | Action | Microsoft.Network/dnszones/SOA/write | Создает или обновляет набор записей DNS типа SOA. |
> | Action | Microsoft.Network/dnszones/SRV/delete | Удаляет из зоны DNS набор записей типа SRV с заданным именем. |
> | Action | Microsoft.Network/dnszones/SRV/read | Возвращает набор записей типа SRV в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft.Network/dnszones/SRV/write | Создает или обновляет набор записей типа SRV. |
> | Action | Microsoft.Network/dnszones/TXT/delete | Удаляет из зоны DNS набор записей типа TXT с заданным именем. |
> | Action | Microsoft.Network/dnszones/TXT/read | Возвращает набор записей типа TXT в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft.Network/dnszones/TXT/write | Создает или обновляет набора записей типа TXT в зоне DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft.Network/dnszones/write | Создает или обновляет зону DNS в группе ресурсов.  Используется для обновления тегов в ресурсе зоны DNS. Обратите внимание, что эту команду невозможно использовать для создания или обновления наборов записей в зоне. |
> | Action | Microsoft.Network/expressRouteCircuits/authorizations/delete | Удаляет данные авторизации канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/authorizations/read | Возвращает данные авторизации канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/authorizations/write | Создает новые данные авторизации канала ExpressRoute или обновляет существующие. |
> | Action | Microsoft.Network/expressRouteCircuits/delete | Удаляет канал ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/join/action | Присоединяет канал Express Route. Не оповещать. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/arpTables/action | Возвращает таблицу ARP для пиринга канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/connections/delete | Удаление подключения канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/connections/read | Получение подключения канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/connections/write | Создание нового ресурса подключения канала ExpressRoute или обновление существующего. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/delete | Удаляет пиринг канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/peerConnections/read | Получает пиринговое подключение канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/read | Возвращает пиринг канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/routeTables/action | Возвращает таблицу маршрутов для пиринга канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/routeTablesSummary/action | Возвращает сводку по таблице маршрутов для пиринга канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/stats/read | Возвращает статистику пиринга канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/peerings/write | Создает новый пиринг канала ExpressRoute или обновляет существующий. |
> | Action | Microsoft.Network/expressRouteCircuits/read | Возвращает канал ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/stats/read | Возвращает статистику канала ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCircuits/write | Создает новый канал ExpressRoute или обновляет существующий. |
> | Action | Microsoft.Network/expressRouteCrossConnections/join/action | Присоединяет перекрестное подключение Express Route. Не оповещать. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/arpTables/action | Получение таблицы ARP перекрестного пирингового подключения ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/delete | Удаление перекрестного пирингового подключения ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/read | Получение перекрестного пирингового подключения ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/routeTables/action | Получение таблицы маршрутов перекрестного пирингового подключения ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/routeTableSummary/action | Получение сводки таблицы маршрутов перекрестного пирингового подключения ExpressRoute. |
> | Action | Microsoft.Network/expressRouteCrossConnections/peerings/write | Создание перекрестного пирингового подключения ExpressRoute или обновление существующего. |
> | Action | Microsoft.Network/expressRouteCrossConnections/read | Получение перекрестного подключения ExpressRoute. |
> | Action | Microsoft.Network/expressRouteGateways/expressRouteConnections/delete | Удаляет подключение Express Route. |
> | Action | Microsoft.Network/expressRouteGateways/expressRouteConnections/read | Получает подключение Express Route. |
> | Action | Microsoft.Network/expressRouteGateways/expressRouteConnections/write | Создает подключение Express Route или обновляет имеющееся подключение. |
> | Action | Microsoft.Network/expressRouteGateways/join/action | Присоединяется к шлюзу Express Route. Не оповещать. |
> | Action | Microsoft.Network/expressRouteGateways/read | Получает шлюз Express Route. |
> | Action | Microsoft.Network/expressRoutePorts/delete | Удаляет ExpressRoutePorts |
> | Action | Microsoft.Network/expressRoutePorts/join/action | Соединяет порты Express Route. Не оповещать. |
> | Action | Microsoft.Network/expressRoutePorts/links/read | Получает канал ExpressRoute. |
> | Action | Microsoft.Network/expressRoutePorts/read | Получает ExpressRoutePorts |
> | Action | Microsoft.Network/expressRoutePorts/write | Создает или обновляет ExpressRoutePorts |
> | Action | Microsoft.Network/expressRoutePortsLocations/read | Получает порты расположений ExpressRoute. |
> | Action | Microsoft.Network/expressRouteServiceProviders/read | Возвращает поставщики службы ExpressRoute. |
> | Action | Microsoft. Network/ФиреваллполиЦиес/удаление | Удаляет политику брандмауэра. |
> | Action | Microsoft. Network/ФиреваллполиЦиес/соединение/действие | Присоединяет политику брандмауэра. Не оповещать. |
> | Action | Microsoft. Network/ФиреваллполиЦиес/чтение | Возвращает политику брандмауэра. |
> | Action | Microsoft. Network/ФиреваллполиЦиес/Рулеграупс/удаление | Удаляет группу правил политики брандмауэра. |
> | Action | Microsoft. Network/ФиреваллполиЦиес/Рулеграупс/чтение | Возвращает группу правил политики брандмауэра. |
> | Action | Microsoft. Network/ФиреваллполиЦиес/Рулеграупс/запись | Создает группу правил политики брандмауэра или обновляет существующую группу правил политики брандмауэра. |
> | Action | Microsoft. Network/ФиреваллполиЦиес/запись | Создание политики брандмауэра или обновление существующей политики брандмауэра. |
> | Action | Microsoft.Network/frontDoors/backendPools/delete | Удаление внутреннего пула |
> | Action | Microsoft.Network/frontDoors/backendPools/read | Получение внутреннего пула |
> | Action | Microsoft.Network/frontDoors/backendPools/write | Создание или обновление внутреннего пула. |
> | Action | Microsoft.Network/frontDoors/delete | Удаление Front Door |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/delete | Удаление интерфейсной конечной точки |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/disableHttps/action | Отключение HTTPS для интерфейсной конечной точки. |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/enableHttps/action | Отключение протокола HTTPS для конечной точки интерфейса |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/read | Получение интерфейсной конечной точки |
> | Action | Microsoft.Network/frontDoors/frontendEndpoints/write | Создание или обновление интерфейсной конечной точки |
> | Action | Microsoft.Network/frontDoors/healthProbeSettings/delete | Удаление параметров проверки работоспособности |
> | Action | Microsoft.Network/frontDoors/healthProbeSettings/read | Получение параметров проверки работоспособности |
> | Action | Microsoft.Network/frontDoors/healthProbeSettings/write | Создание или обновление параметров проверки работоспособности |
> | Action | Microsoft.Network/frontDoors/loadBalancingSettings/delete | Создание или обновление параметров балансировки нагрузки |
> | Action | Microsoft.Network/frontDoors/loadBalancingSettings/read | Получение параметров балансировки нагрузки |
> | Action | Microsoft.Network/frontDoors/loadBalancingSettings/write | Создание или обновление параметров балансировки нагрузки |
> | Action | Microsoft.Network/frontDoors/purge/action | Очистка кэшированного содержимого из Front Door |
> | Action | Microsoft.Network/frontDoors/read | Получение Front Door |
> | Action | Microsoft.Network/frontDoors/routingRules/delete | Создание правила маршрутизации |
> | Action | Microsoft.Network/frontDoors/routingRules/read | Получение правила маршрутизации |
> | Action | Microsoft.Network/frontDoors/routingRules/write | Создание или обновление правила маршрутизации |
> | Action | Microsoft.Network/frontDoors/validateCustomDomain/action | Проверка конечной точки интерфейса для Front Door |
> | Action | Microsoft.Network/frontDoors/write | Создание или обновление Front Door |
> | Action | Microsoft. Network/Фронтдурвебаппликатионфиреваллманажедрулесетс/чтение | Получает наборы управляемых правил брандмауэра веб-приложения |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/delete | Удаление политики брандмауэра веб-приложения |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/read | Получение политики брандмауэра веб-приложения |
> | Action | Microsoft.Network/frontDoorWebApplicationFirewallPolicies/write | Создание или обновление политики брандмауэра веб-приложения |
> | Action | Microsoft.Network/loadBalancers/backendAddressPools/join/action | Присоединяет внутренний пул адресов подсистемы балансировки нагрузки. Не оповещать. |
> | Action | Microsoft.Network/loadBalancers/backendAddressPools/read | Возвращает определение внутреннего пула адресов подсистемы балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/delete | Удаляет подсистему балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/frontendIPConfigurations/join/action | Объединяет внешнюю IP-конфигурацию Load Balancer. Не оповещать. |
> | Action | Microsoft.Network/loadBalancers/frontendIPConfigurations/read | Возвращает определение внешней IP-конфигурации подсистемы балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/inboundNatPools/join/action | Присоединяется к пулу входящих подключений NAT подсистемы балансировки нагрузки. Не оповещать. |
> | Action | Microsoft.Network/loadBalancers/inboundNatPools/read | Возвращает определение пула входящих подключений NAT подсистемы балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/delete | Удаляет правило NAT для входящего трафика подсистемы балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/join/action | Присоединяет правило NAT для входящего трафика подсистемы балансировки нагрузки. Не оповещать. |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/read | Возвращает определение правила NAT для входящего трафика подсистемы балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/inboundNatRules/write | Создает новое правило NAT для входящего трафика подсистемы балансировки нагрузки или обновляет существующее. |
> | Action | Microsoft.Network/loadBalancers/loadBalancingRules/read | Возвращает определение правила подсистемы балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/networkInterfaces/read | Возвращает ссылки на все сетевые интерфейсы в подсистеме балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/outboundRules/read | Получает определения исходящего правила подсистемы балансировки нагрузки |
> | Action | Microsoft.Network/loadBalancers/probes/join/action | Позволяет использовать зонды подсистемы балансировки нагрузки. Например, при наличии этого разрешения свойство healthProbe в масштабируемом наборе виртуальных машин может ссылаться на конкретный зонд. Не оповещать. |
> | Action | Microsoft.Network/loadBalancers/probes/read | Возвращает пробу подсистемы балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/read | Возвращает определение подсистемы балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/virtualMachines/read | Возвращает ссылки на все виртуальные машины в подсистеме балансировки нагрузки. |
> | Action | Microsoft.Network/loadBalancers/write | Создает новую подсистему балансировки нагрузки или обновляет существующую. |
> | Action | Microsoft.Network/localnetworkgateways/delete | Удаляет шлюз локальной сети. |
> | Action | Microsoft.Network/localnetworkgateways/read | Возвращает шлюз локальной сети. |
> | Action | Microsoft.Network/localnetworkgateways/write | Создает новый шлюз локальной сети или обновляет существующий. |
> | Action | Microsoft. Network/Locations/Аутоаппроведпривателинксервицес/Read | Получает автоматические утвержденные службы частной связи |
> | Action | Microsoft.Network/locations/availableDelegations/read | Получает доступные делегирования. |
> | Action | Microsoft. Network/Locations/Аваилаблеприватиндпоинттипес/Read | Получение доступных ресурсов частной конечной точки |
> | Action | Microsoft.Network/locations/bareMetalTenants/action | Выделяет или проверяет клиент исходного состояния. |
> | Action | Microsoft.Network/locations/checkAcceleratedNetworkingSupport/action | Проверяет поддержку ускоренной сети. |
> | Action | Microsoft.Network/locations/checkDnsNameAvailability/read | Проверяет, доступна ли метка DNS в указанном расположении. |
> | Action | Microsoft. Network/Locations/Чеккпривателинксервицевисибилити/Action | Проверяет видимость службы закрытых ссылок |
> | Action | Microsoft.Network/locations/operationResults/read | Возвращает результат асинхронной операции POST или DELETE. |
> | Action | Microsoft.Network/locations/operations/read | Возвращает ресурс операции, представляющий состояние асинхронной операции. |
> | Action | Microsoft. Network/Locations/Сервицетагс/Read | Получить Теги службы |
> | Action | Microsoft.Network/locations/supportedVirtualMachineSizes/read | Возвращает поддерживаемые размеры виртуальных машин. |
> | Action | Microsoft.Network/locations/usages/read | Возвращает метрики использования ресурсов. |
> | Action | Microsoft.Network/locations/virtualNetworkAvailableEndpointServices/read | Получение списка доступных служб конечных точек виртуальной сети. |
> | Action | Microsoft.Network/networkIntentPolicies/delete | Удаляет политику намерений сети |
> | Action | Microsoft.Network/networkIntentPolicies/read | Получает описание политики намерений сети |
> | Action | Microsoft.Network/networkIntentPolicies/write | Создает политику намерения сети или обновляет существующую |
> | Action | Microsoft.Network/networkInterfaces/delete | Удаляет сетевой интерфейс. |
> | Action | Microsoft.Network/networkInterfaces/effectiveNetworkSecurityGroups/action | Возвращает группы безопасности сети, настроенные для сетевого интерфейса виртуальной машины. |
> | Action | Microsoft.Network/networkInterfaces/effectiveRouteTable/action | Возвращает таблицу маршрутов, настроенную для сетевого интерфейса виртуальной машины. |
> | Action | Microsoft.Network/networkInterfaces/ipconfigurations/join/action | Объединяет IP-конфигурацию сетевого интерфейса. Не оповещать. |
> | Action | Microsoft.Network/networkInterfaces/ipconfigurations/read | Возвращает определение IP-конфигурации сетевого интерфейса.  |
> | Action | Microsoft.Network/networkInterfaces/join/action | Присоединяет виртуальную машину к сетевому интерфейсу. Не оповещать. |
> | Action | Microsoft.Network/networkInterfaces/loadBalancers/read | Возвращает все подсистемы балансировки нагрузки, в которых используется сетевой интерфейс. |
> | Action | Microsoft.Network/networkInterfaces/read | Возвращает определение сетевого интерфейса.  |
> | Action | Microsoft.Network/networkInterfaces/tapConfigurations/delete | Удаляет конфигурацию TAP сетевого интерфейса. |
> | Action | Microsoft.Network/networkInterfaces/tapConfigurations/read | Получает конфигурацию TAP сетевого интерфейса. |
> | Action | Microsoft.Network/networkInterfaces/tapConfigurations/write | Создает конфигурацию TAP сетевого интерфейса или обновляет имеющуюся конфигурацию TAP сетевого интерфейса. |
> | Action | Microsoft.Network/networkInterfaces/write | Создает новый сетевой интерфейс или обновляет существующий.  |
> | Action | Microsoft.Network/networkProfiles/delete | Удаляет сетевой профиль |
> | Action | Microsoft.Network/networkProfiles/read | Получает сетевой профиль |
> | Action | Microsoft.Network/networkProfiles/removeContainers/action | Удаляет контейнеры |
> | Action | Microsoft.Network/networkProfiles/setContainers/action | Задает контейнеры |
> | Action | Microsoft.Network/networkProfiles/setNetworkInterfaces/action | Задает интерфейсы сетевых контейнеров |
> | Action | Microsoft.Network/networkProfiles/write | Создает или обновляет сетевой профиль |
> | Action | Microsoft.Network/networkSecurityGroups/defaultSecurityRules/read | Возвращает определение правила безопасности по умолчанию. |
> | Action | Microsoft.Network/networkSecurityGroups/delete | Удаляет группу безопасности сети. |
> | Action | Microsoft.Network/networkSecurityGroups/join/action | Присоединяет группу безопасности сети. Не оповещать. |
> | Action | Microsoft.Network/networkSecurityGroups/read | Возвращает определение группы безопасности сети. |
> | Action | Microsoft.Network/networkSecurityGroups/securityRules/delete | Удаляет правило безопасности. |
> | Action | Microsoft.Network/networkSecurityGroups/securityRules/read | Возвращает определение правила безопасности. |
> | Action | Microsoft.Network/networkSecurityGroups/securityRules/write | Создает новое правило безопасности или обновляет существующее. |
> | Action | Microsoft.Network/networkSecurityGroups/write | Создает новую группу безопасности сети или обновляет существующую. |
> | Action | Microsoft.Network/networkWatchers/availableProvidersList/action | Получение всех доступных поставщиков услуг Интернета в заданном регионе Azure. |
> | Action | Microsoft.Network/networkWatchers/azureReachabilityReport/action | Получение оценки относительной задержки для поставщиков услуг Интернета из указанного расположения в регионы Azure. |
> | Action | Microsoft.Network/networkWatchers/configureFlowLog/action | Настраивает ведение журнала потоков для целевого ресурса. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/delete | Удаление монитора подключения. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/query/action | Запрос мониторинга подключения между указанными конечными точками. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/read | Получение сведений о мониторе подключения. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/start/action | Запуск мониторинга подключения между указанными конечными точками. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/stop/action | Остановка или приостановка мониторинга подключения между указанными конечными точками. |
> | Action | Microsoft.Network/networkWatchers/connectionMonitors/write | Создание монитора подключения. |
> | Action | Microsoft.Network/networkWatchers/connectivityCheck/action | Проверка возможности прямого подключения TCP между виртуальной машиной и определенной конечной точкой, при этом используется другая виртуальная машина или произвольный удаленный сервер. |
> | Action | Microsoft.Network/networkWatchers/delete | Удаляет Наблюдатель за сетями. |
> | Action | Microsoft.Network/networkWatchers/ipFlowVerify/action | Проверяет, разрешена ли передача или прием пакета для определенного места назначения. |
> | Action | Microsoft.Network/networkWatchers/lenses/delete | Удаление группы связанных элементов. |
> | Action | Microsoft.Network/networkWatchers/lenses/query/action | Запрос мониторинга трафика в указанной конечной точке. |
> | Action | Microsoft.Network/networkWatchers/lenses/read | Получение сведений о группе связанных элементов. |
> | Action | Microsoft.Network/networkWatchers/lenses/start/action | Запуск мониторинга трафика в указанной конечной точке. |
> | Action | Microsoft.Network/networkWatchers/lenses/stop/action | Остановка или приостановка мониторинга трафика в указанной конечной точке. |
> | Action | Microsoft.Network/networkWatchers/lenses/write | Создание группы связанных элементов. |
> | Action | Microsoft.Network/networkWatchers/networkConfigurationDiagnostic/action | Диагностика конфигурации сети. |
> | Action | Microsoft.Network/networkWatchers/nextHop/action | Для указанного целевого объекта и конечного IP-адреса возвращает тип следующего прыжка и его IP-адрес. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/delete | Удаляет запись пакетов. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/queryStatus/action | Возвращает сведения о свойствах и состоянии ресурса записи пакетов. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/read | Возвращает определение записи пакетов. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/stop/action | Останавливает выполняемый сеанс записи пакетов. |
> | Action | Microsoft.Network/networkWatchers/packetCaptures/write | Создает запись пакетов. |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/delete | Удаляет PingMesh. |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/read | Получает сведения о PingMesh. |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/start/action | Запускает PingMesh между указанными виртуальными машинами. |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/stop/action | Останавливает PingMesh между указанными виртуальными машинами. |
> | Action | Microsoft.Network/networkWatchers/pingMeshes/write | Создает PingMesh. |
> | Action | Microsoft.Network/networkWatchers/queryConnectionMonitors/action | Пакетный запрос мониторинга подключения между указанными конечными точками |
> | Action | Microsoft.Network/networkWatchers/queryFlowLogStatus/action | Возвращает состояние ведения журнала потоков для ресурса. |
> | Action | Microsoft.Network/networkWatchers/queryTroubleshootResult/action | Возвращает результат устранения неполадок предыдущей или текущей операции по устранению неполадок. |
> | Action | Microsoft.Network/networkWatchers/read | Возвращает определение Наблюдателя за сетями. |
> | Action | Microsoft.Network/networkWatchers/securityGroupView/action | Отображает настроенные и действующие правила группы безопасности сети для виртуальной машины. |
> | Action | Microsoft.Network/networkWatchers/topology/action | Возвращает представление уровня сети для ресурсов и их связей в группе ресурсов. |
> | Action | Microsoft.Network/networkWatchers/troubleshoot/action | Начинает устранение неполадок сетевого ресурса в Azure. |
> | Action | Microsoft.Network/networkWatchers/write | Создает новый Наблюдатель за сетями или обновляет существующий. |
> | Action | Microsoft.Network/operations/read | Возвращает доступные операции. |
> | Action | Microsoft.Network/p2sVpnGateways/delete | Удаляет P2SVpnGateway. |
> | Action | Microsoft.Network/p2sVpnGateways/generatevpnprofile/action | Создание профиля Vpn для P2SVpnGateway |
> | Action | Microsoft.Network/p2sVpnGateways/getp2svpnconnectionhealth/action | Получает данные о работоспособности VPN-подключения типа "точка — сеть" для P2SVpnGateway. |
> | Action | Microsoft.Network/p2sVpnGateways/read | Возвращает P2SVpnGateway. |
> | Action | Microsoft.Network/p2sVpnGateways/write | Получает P2SVpnGateway. |
> | Action | Microsoft. Network/Приватеднсоператионресултс/чтение | Возвращает результаты операции Частная зона DNS |
> | Action | Microsoft. Network/Приватеднсоператионстатусес/чтение | Возвращает состояние операции Частная зона DNS |
> | Action | Microsoft. Network/Приватеднсзонес/A/удаление | Удаление набора записей с заданным именем и типа "A" из Частная зона DNS зоны. |
> | Action | Microsoft. Network/Приватеднсзонес/а/Read | Получение набора записей типа A в Частная зона DNSной зоне в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft. Network/Приватеднсзонес/A/запись | Создание или обновление набора записей типа A в Частная зона DNSной зоне. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft. Network/Приватеднсзонес, AAAA/удалить | Удаление набора записей с заданным именем и типа "AAAA" из зоны Частная зона DNS. |
> | Action | Microsoft. Network/Приватеднсзонес/AAAA/чтение | Получите набор записей типа AAAA в Частная зона DNSной зоне в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft. Network/Приватеднсзонес, AAAA/запись | Создание или обновление набора записей типа AAAA в зоне Частная зона DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft. Network/Приватеднсзонес/все/чтение | Возвращает Частная зона DNS наборов записей между типами |
> | Action | Microsoft.Network/privateDnsZones/CNAME/delete | Удаление набора записей с заданным именем и типа "CNAME" из зоны Частная зона DNS. |
> | Action | Microsoft.Network/privateDnsZones/CNAME/read | Получите набор записей типа CNAME в Частная зона DNSной зоне в формате JSON. |
> | Action | Microsoft.Network/privateDnsZones/CNAME/write | Создание или обновление набора записей типа "CNAME" в зоне Частная зона DNS. |
> | Action | Microsoft. Network/Приватеднсзонес/удаление | Удаление зоны Частная зона DNS. |
> | Action | Microsoft. Network/Приватеднсзонес, MX/Delete | Удаление набора записей с заданным именем и типа MX из зоны Частная зона DNS. |
> | Action | Microsoft. Network/Приватеднсзонес, MX/Read | Получите набор записей типа MX в зоне Частная зона DNS в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft. Network/Приватеднсзонес, MX/Write | Создание или обновление набора записей типа MX в зоне Частная зона DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft. Network/Приватеднсзонес, PTR/Delete | Удалить набор записей с заданным именем и типом "PTR" из зоны Частная зона DNS. |
> | Action | Microsoft. Network/Приватеднсзонес/PTR/чтение | Получите набор записей типа PTR в Частная зона DNSной зоне в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft. Network/Приватеднсзонес/PTR/запись | Создание или обновление набора записей типа PTR в зоне Частная зона DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft. Network/Приватеднсзонес/чтение | Получение свойств зоны Частная зона DNS в формате JSON. Обратите внимание, что эта команда не извлекает виртуальные сети, к которым привязана Частная зона DNSная зона, или наборы записей, содержащиеся в зоне. |
> | Action | Microsoft. Network/Приватеднсзонес/наборы записей/чтение | Возвращает Частная зона DNS наборов записей между типами |
> | Action | Microsoft. Network/Приватеднсзонес/SOA/чтение | Получите набор записей типа SOA в зоне Частная зона DNS в формате JSON. |
> | Action | Microsoft. Network/Приватеднсзонес/SOA/запись | Обновление набора записей типа "SOA" в зоне Частная зона DNS. |
> | Action | Microsoft. Network/Приватеднсзонес, SRV/Delete | Удалить набор записей с заданным именем и типом "SRV" из зоны Частная зона DNS. |
> | Action | Microsoft. Network/Приватеднсзонес/SRV/чтение | Получите набор записей типа SRV в Частная зона DNSной зоне в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft. Network/Приватеднсзонес/SRV/запись | Создание или обновление набора записей типа SRV в зоне Частная зона DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft. Network/Приватеднсзонес/TXT/удаление | Удаление набора записей с заданным именем и типа "TXT" из зоны Частная зона DNS. |
> | Action | Microsoft. Network/Приватеднсзонес/TXT/чтение | Получите набор записей типа TXT в зоне Частная зона DNS в формате JSON. Набор записей содержит список записей, а также срок жизни, теги и ETag. |
> | Action | Microsoft. Network/Приватеднсзонес/TXT/запись | Создание или обновление набора записей типа "TXT" в зоне Частная зона DNS. Указанные записи заменят текущие записи в наборе записей. |
> | Action | Microsoft. Network/Приватеднсзонес/Виртуалнетворклинкс/удаление | Удаление ссылки на зону Частная зона DNS в виртуальную сеть. |
> | Action | Microsoft. Network/Приватеднсзонес/Виртуалнетворклинкс/чтение | Ссылка на зону Частная зона DNS в свойствах виртуальной сети в формате JSON. |
> | Action | Microsoft. Network/Приватеднсзонес/Виртуалнетворклинкс/запись | Создайте или обновите ссылку на зону Частная зона DNS к виртуальной сети. |
> | Action | Microsoft. Network/Приватеднсзонес/запись | Создание или обновление зоны Частная зона DNS в группе ресурсов. Обратите внимание, что эта команда не может использоваться для создания или обновления связей виртуальных сетей или наборов записей в зоне. |
> | Action | Microsoft. Network/Приватиндпоинтс/удаление | Удаляет ресурс частной конечной точки. |
> | Action | Microsoft. Network/Приватиндпоинтс/чтение | Получает ресурс частной конечной точки. |
> | Action | Microsoft. Network/Приватиндпоинтс/запись | Создает новую частную конечную точку или обновляет существующую закрытую конечную точку. |
> | Action | Microsoft.Network/privateLinkServices/delete | Удаляет ресурс службы частной связи. |
> | Action | Microsoft. Network/Привателинксервицес/Приватиндпоинтконнектионс/удаление | Удаляет подключение частной конечной точки. |
> | Action | Microsoft. Network/Привателинксервицес/Приватиндпоинтконнектионс/чтение | Возвращает определение подключения к частной конечной точке. |
> | Action | Microsoft. Network/Привателинксервицес/Приватиндпоинтконнектионс/запись | Создает новое подключение к частной конечной точке или обновляет существующее подключение к частной конечной точке. |
> | Action | Microsoft.Network/privateLinkServices/read | Получает ресурс службы частной связи. |
> | Action | Microsoft.Network/privateLinkServices/write | Создает новую службу частной связи или обновляет существующую. |
> | Action | Microsoft.Network/publicIPAddresses/delete | Удаляет общедоступный IP-адрес. |
> | Action | Microsoft.Network/publicIPAddresses/join/action | Присоединяет общедоступный IP-адрес. Не оповещать. |
> | Action | Microsoft.Network/publicIPAddresses/read | Возвращает определение общедоступного IP-адреса. |
> | Action | Microsoft.Network/publicIPAddresses/write | Создает новый общедоступный IP-адрес или обновляет существующий.  |
> | Action | Microsoft.Network/publicIPPrefixes/delete | Удаляет префикс общедоступного IP-адреса. |
> | Action | Microsoft.Network/publicIPPrefixes/join/action | Присоединяется к ПублиЦиппрефикс. Не оповещать. |
> | Action | Microsoft.Network/publicIPPrefixes/read | Возвращает определение префикса общедоступного IP-адреса. |
> | Action | Microsoft.Network/publicIPPrefixes/write | Создает новый префикс общедоступного IP-адреса или обновляет существующий. |
> | Action | Microsoft.Network/register/action | Регистрация подписки |
> | Action | Microsoft.Network/routeFilters/delete | Удаляет определение фильтра маршрутов. |
> | Action | Microsoft.Network/routeFilters/join/action | Присоединяет фильтр маршрутов. Не оповещать. |
> | Action | Microsoft.Network/routeFilters/read | Возвращает определение фильтра маршрутов. |
> | Action | Microsoft.Network/routeFilters/routeFilterRules/delete | Удаляет определение правила фильтра маршрутов. |
> | Action | Microsoft.Network/routeFilters/routeFilterRules/read | Возвращает определение правила фильтра маршрутов. |
> | Action | Microsoft.Network/routeFilters/routeFilterRules/write | Создает новое правило фильтра маршрутов или обновляет существующее. |
> | Action | Microsoft.Network/routeFilters/write | Создает новый фильтр маршрутов или обновляет существующий. |
> | Action | Microsoft.Network/routeTables/delete | Удаляет определение таблицы маршрутов. |
> | Action | Microsoft.Network/routeTables/join/action | Присоединяет таблицу маршрутов. Не оповещать. |
> | Action | Microsoft.Network/routeTables/read | Возвращает определение таблицы маршрутов. |
> | Action | Microsoft.Network/routeTables/routes/delete | Удаляет определение маршрута. |
> | Action | Microsoft.Network/routeTables/routes/read | Возвращает определение маршрута. |
> | Action | Microsoft.Network/routeTables/routes/write | Создает новый маршрут или обновляет существующий. |
> | Action | Microsoft.Network/routeTables/write | Создает новую таблицу маршрутов или обновляет существующую. |
> | Action | Microsoft.Network/securegateways/delete | Удаление защищенного шлюза. |
> | Action | Microsoft.Network/securegateways/read | Получение защищенного шлюза. |
> | Action | Microsoft.Network/securegateways/write | Создание или обновление защищенного шлюза. |
> | Action | Microsoft.Network/serviceEndpointPolicies/delete | Удаление политики конечной точки службы. |
> | Action | Microsoft.Network/serviceEndpointPolicies/join/action | Присоединяет политику конечной точки службы. Не оповещать. |
> | Action | Microsoft.Network/serviceEndpointPolicies/joinSubnet/action | Присоединяет подсеть к политикам конечной точки службы. Не оповещать. |
> | Action | Microsoft.Network/serviceEndpointPolicies/read | Получение описания политики конечной точки службы. |
> | Action | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/delete | Удаление определения политики конечной точки службы. |
> | Action | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/read | Получение описания определения политики конечной точки службы. |
> | Action | Microsoft.Network/serviceEndpointPolicies/serviceEndpointPolicyDefinitions/write | Создание определения политики конечной точки службы или обновление существующего. |
> | Action | Microsoft.Network/serviceEndpointPolicies/write | Создание политики конечной точки службы или обновление существующей. |
> | Action | Microsoft.Network/trafficManagerGeographicHierarchies/read | Возвращает географическую иерархию диспетчера трафика, содержащую регионы, которые могут быть использованы для метода географической маршрутизации трафика. |
> | Action | Microsoft.Network/trafficManagerProfiles/azureEndpoints/delete | Удаление конечной точки Azure из существующего профиля диспетчера трафика. Диспетчер трафика остановит маршрутизацию трафика в удаленную конечную точку Azure. |
> | Action | Microsoft.Network/trafficManagerProfiles/azureEndpoints/read | Получение конечной точки Azure, которая относится к профилю диспетчера трафика, а также свойств этой конечной точки. |
> | Action | Microsoft.Network/trafficManagerProfiles/azureEndpoints/write | Добавление новой конечной точки Azure в существующий профиль диспетчера трафика или обновление свойств существующей конечной точки Azure в этом профиле. |
> | Action | Microsoft.Network/trafficManagerProfiles/delete | Удаляет профиль диспетчера трафика. Будут утрачены все параметры, связанные с профилем диспетчера трафика, и профиль больше не сможет использоваться для маршрутизации трафика. |
> | Action | Microsoft.Network/trafficManagerProfiles/externalEndpoints/delete | Удаление внешней конечной точки из существующего профиля диспетчера трафика. Диспетчер трафика остановит маршрутизацию трафика в удаленную внешнюю конечную точку. |
> | Action | Microsoft.Network/trafficManagerProfiles/externalEndpoints/read | Получение внешней конечной точки, которая относится к профилю диспетчера трафика, а также свойств этой конечной точки. |
> | Action | Microsoft.Network/trafficManagerProfiles/externalEndpoints/write | Добавление новой внешней конечной точки в существующий профиль диспетчера трафика или обновление свойств существующей внешней конечной точки в этом профиле. |
> | Action | Microsoft.Network/trafficManagerProfiles/heatMaps/read | Получение тепловой карты диспетчера трафика для данного профиля диспетчера трафика, которая содержит количество запросов и данные задержек по исходному IP-адресу и расположению. |
> | Action | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/delete | Удаление вложенной конечной точки из существующего профиля диспетчера трафика. Диспетчер трафика остановит маршрутизацию трафика в удаленную вложенную конечную точку. |
> | Action | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/read | Получение вложенной конечной точки, которая относится к профилю диспетчера трафика, а также свойств этой конечной точки. |
> | Action | Microsoft.Network/trafficManagerProfiles/nestedEndpoints/write | Добавление новой вложенной конечной точки в существующий профиль диспетчера трафика или обновление свойств существующей вложенной конечной точки в этом профиле. |
> | Action | Microsoft.Network/trafficManagerProfiles/read | Возвращает конфигурацию профиля диспетчера трафика. Он содержит параметры DNS, параметры маршрутизации трафика, параметры отслеживания конечных точек, а также список конечных точек, маршрутизируемых данным профилем диспетчера трафика. |
> | Action | Microsoft.Network/trafficManagerProfiles/write | Создает профиль диспетчера трафика или изменяет конфигурацию существующего профиля диспетчера трафика.<br>Сюда входит включение или отключение профиля и изменение параметров DNS, параметров маршрутизации трафика или параметров мониторинга конечных точек.<br>Конечные точки, маршрутизируемые с помощью профиля диспетчера трафика, можно добавлять, удалять, включать и отключать. |
> | Action | Microsoft.Network/trafficManagerUserMetricsKeys/delete | Удаление ключа уровня подписки, используемого для сбора метрик пользователя в реальном времени. |
> | Action | Microsoft.Network/trafficManagerUserMetricsKeys/read | Получение ключа уровня подписки, используемого для сбора метрик пользователя в реальном времени. |
> | Action | Microsoft.Network/trafficManagerUserMetricsKeys/write | Создание нового ключа уровня подписки, используемого для сбора метрик пользователя в реальном времени. |
> | Action | Microsoft.Network/unregister/action | Отменяет регистрацию подписки. |
> | Action | Microsoft.Network/virtualHubs/delete | Удаление виртуального концентратора. |
> | Action | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/delete | Удаление подключения к виртуальной сети концентратора. |
> | Action | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/read | Получение подключения к виртуальной сети концентратора. |
> | Action | Microsoft.Network/virtualHubs/hubVirtualNetworkConnections/write | Создание или обновление подключения к виртуальной сети концентратора. |
> | Action | Microsoft.Network/virtualHubs/read | Получение виртуального концентратора. |
> | Action | Microsoft.Network/virtualHubs/write | Создание или обновление виртуального концентратора. |
> | Action | microsoft.network/virtualnetworkgateways/connections/read | Получение подключения к шлюзу виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworkGateways/delete | Удаление шлюза виртуальной сети. |
> | Action | microsoft.network/virtualnetworkgateways/generatevpnclientpackage/action | Создание пакета VPN-клиента для шлюза виртуальной сети. |
> | Action | microsoft.network/virtualnetworkgateways/generatevpnprofile/action | Создание пакета VPN-профиля для шлюза виртуальной сети. |
> | Action | microsoft.network/virtualnetworkgateways/getadvertisedroutes/action | Получение объявленных маршрутов шлюза виртуальной сети. |
> | Action | microsoft.network/virtualnetworkgateways/getbgppeerstatus/action | Получение состояния кэширующего узла BGP шлюза виртуальной сети. |
> | Action | microsoft.network/virtualnetworkgateways/getlearnedroutes/action | Получение обнаруженных маршрутов шлюза виртуальной сети. |
> | Action | microsoft.network/virtualnetworkgateways/getvpnclientconnectionhealth/action | Получение работоспособности подключения VPN-клиента для шлюза виртуальной сети. |
> | Action | microsoft.network/virtualnetworkgateways/getvpnclientipsecparameters/action | Получение параметров IPsec VPN-клиента для клиента P2S шлюза виртуальной сети. |
> | Action | microsoft.network/virtualnetworkgateways/getvpnprofilepackageurl/action | Получение URL-адреса предварительно созданного пакета профиля VPN-клиента. |
> | Action | Microsoft.Network/virtualNetworkGateways/read | Получение шлюза виртуальной сети. |
> | Action | microsoft.network/virtualnetworkgateways/reset/action | Сброс шлюза виртуальной сети. |
> | Action | microsoft.network/virtualnetworkgateways/resetvpnclientsharedkey/action | Сброс общего ключа Vpnclient клиента P2S VirtualNetworkGateway. |
> | Action | microsoft.network/virtualnetworkgateways/setvpnclientipsecparameters/action | Настройка параметров IPsec VPN-клиента для клиента P2S шлюза виртуальной сети. |
> | Action | Microsoft.Network/virtualnetworkgateways/supportedvpndevices/action | Получение списка поддерживаемых VPN-устройств. |
> | Action | Microsoft.Network/virtualNetworkGateways/write | Создание или обновление шлюза виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/BastionHosts/action | Получение ссылок узла-бастиона в виртуальной сети. |
> | Action | Microsoft. Network/virtualNetworks/Бастионхостс/значение по умолчанию/действие | Получение ссылок узла-бастиона в виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/checkIpAddressAvailability/read | Проверяет, доступен ли IP-адрес в указанной виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/delete | Удаляет виртуальную сеть. |
> | Action | Microsoft. Network/virtualNetworks/соединение/действие | Присоединяет виртуальную сеть. Не оповещать. |
> | Action | Microsoft.Network/virtualNetworks/peer/action | Создает пиринг между виртуальными сетями. |
> | Action | Microsoft.Network/virtualNetworks/read | Возвращает определение виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/subnets/delete | Удаляет подсеть виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/subnets/join/action | Присоединяет виртуальную сеть. Не оповещать. |
> | Action | Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action | Присоединение к подсети ресурса, например учетной записи хранения или базы данных SQL. Не оповещать. |
> | Action | Microsoft.Network/virtualNetworks/subnets/prepareNetworkPolicies/action | Подготавливает подсеть, применяя необходимые политики сети. |
> | Action | Microsoft.Network/virtualNetworks/subnets/read | Возвращает определение подсети виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/subnets/virtualMachines/read | Возвращает ссылки на все виртуальные машины в подсети виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/subnets/write | Создает новую подсеть пиринг виртуальной сети или обновляет существующую. |
> | Action | Microsoft.Network/virtualNetworks/usages/read | Получение сведения об использовании IP-адресов для каждой подсети виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/virtualMachines/read | Возвращает ссылки на все виртуальные машины в виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/delete | Удаляет пиринг виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/read | Возвращает определение пиринга виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write | Создает новый пиринг виртуальной сети или обновляет существующий. |
> | Action | Microsoft.Network/virtualNetworks/write | Создает новую виртуальную сеть или обновляет существующую. |
> | Action | Microsoft.Network/virtualNetworkTaps/delete | Удаление устройства перехватчика трафика (TAP) виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworkTaps/join/action | Присоединяется к отводам виртуальной сети. Не оповещать. |
> | Action | Microsoft.Network/virtualNetworkTaps/read | Получение TAP виртуальной сети. |
> | Action | Microsoft.Network/virtualNetworkTaps/write | Создание или обновление TAP виртуальной сети. |
> | Action | Microsoft.Network/virtualWans/delete | Удаление виртуальной глобальной сети. |
> | Action | Microsoft.network/virtualWans/p2sVpnServerConfigurations/delete | Удаляет конфигурацию P2SVpnServerConfiguration виртуальной глобальной сети |
> | Action | Microsoft.Network/virtualWans/p2sVpnServerConfigurations/read | Получает конфигурацию P2SVpnServerConfiguration виртуальной глобальной сети. |
> | Action | Microsoft.network/virtualWans/p2sVpnServerConfigurations/write | Создает конфигурацию P2SVpnServerConfiguration виртуальной глобальной сети или обновляет существующую |
> | Action | Microsoft.Network/virtualWans/read | Получение виртуальной глобальной сети. |
> | Action | Microsoft.Network/virtualwans/supportedSecurityProviders/read | Получает поддерживаемых поставщиков безопасности виртуальной глобальной сети. |
> | Action | Microsoft.Network/virtualWans/virtualHubs/read | Получение всех виртуальных концентраторов, ссылающихся на виртуальную глобальную сеть. |
> | Action | Microsoft.Network/virtualwans/vpnconfiguration/action | Получение конфигурации VPN. |
> | Action | Microsoft.Network/virtualWans/vpnSites/read | Получение всех сетей VPN, ссылающихся на виртуальную глобальную сеть. |
> | Action | Microsoft.Network/virtualWans/write | Создание или обновление виртуальной глобальной сети. |
> | Action | Microsoft.Network/vpnGateways/delete | Удаляет VPN-шлюз. |
> | Action | microsoft.network/vpngateways/listvpnconnectionshealth/action | Получает состояние работоспособности подключения для всех или подмножества подключений на шлюзе VpnGateway |
> | Action | Microsoft.Network/vpnGateways/read | Получение шлюза VPN. |
> | Action | microsoft.network/vpngateways/reset/action | Выполняет сброс VpnGateway |
> | Action | microsoft.network/vpnGateways/vpnConnections/delete | Удаляет VpnConnection. |
> | Action | microsoft.network/vpnGateways/vpnConnections/read | Получение VPN-подключения. |
> | Action | microsoft.network/vpnGateways/vpnConnections/write | Установка VPN-подключения. |
> | Action | Microsoft.Network/vpnGateways/write | Установка VPN-шлюза. |
> | Action | Microsoft.Network/vpnsites/delete | Удаление ресурса сети VPN. |
> | Action | Microsoft.Network/vpnsites/read | Получение ресурса сети VPN. |
> | Action | Microsoft.Network/vpnsites/write | Создание или обновление ресурса сети VPN. |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.NotificationHubs/CheckNamespaceAvailability/action | Проверяет, доступно ли заданное имя ресурса пространства имен в службе концентратора уведомлений или нет. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/action | Получить список описаний правил авторизации пространства имен. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/delete | Удаляет правило авторизации для пространства имен. Невозможно удалить правило авторизации пространства имен по умолчанию.  |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/listkeys/action | Получить строку подключения к пространству имен |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/read | Получить список описаний правил авторизации пространства имен. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/regenerateKeys/action | Повторное создание первичного или вторичного ключа в правиле авторизации пространства имен; укажите ключ, который необходимо повторно создать. |
> | Action | Microsoft.NotificationHubs/Namespaces/authorizationRules/write | Создает правила авторизации уровня пространства имен и обновляет его свойства. Можно обновить права доступа, первичный ключ и вторичный ключ для правил авторизации. |
> | Action | Microsoft.NotificationHubs/Namespaces/CheckNotificationHubAvailability/action | Проверяет, доступно ли заданное имя концентратора уведомлений в пространстве имен. |
> | Action | Microsoft.NotificationHubs/Namespaces/Delete | Удалить ресурс пространства имен |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/action | Получить список правил авторизации концентратора уведомлений |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/delete | Удалить правила авторизации концентратора уведомлений |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/listkeys/action | Получить строку подключения к концентратору уведомлений |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/read | Получить список правил авторизации концентратора уведомлений |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/regenerateKeys/action | Повторное создание первичного или вторичного ключа в правиле авторизации концентратора уведомлений; укажите ключ, который необходимо повторно создать. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/authorizationRules/write | Создает правила авторизации центра уведомлений и обновляет его свойства. Можно обновить права доступа, первичный ключ и вторичный ключ для правил авторизации. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/debugSend/action | Отправить тестовое push-уведомление. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/Delete | Удалить ресурс концентратора уведомлений |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/metricDefinitions/read | Возвращает список описаний ресурсов метрик пространства имен. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/pnsCredentials/action | Возвращает все учетные данные PNS центра уведомлений. Сюда входят учетные данные WNS, MPNS, APNS, GCM и Baidu. |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/read | Получить список описаний ресурса концентратора уведомлений |
> | Action | Microsoft.NotificationHubs/Namespaces/NotificationHubs/write | Создает центр уведомлений и обновляет его свойства. К этим свойствам в основном относятся учетные данные PNS. Правила авторизации и срок жизни. |
> | Action | Microsoft.NotificationHubs/Namespaces/read | Получить список описаний ресурса пространства имен |
> | Action | Microsoft.NotificationHubs/Namespaces/write | Создает ресурс пространства имен и обновляет его свойства. К этим свойствам относятся теги и пространство имен. |
> | Action | Microsoft.NotificationHubs/operationResults/read | Возвращает результаты операции для поставщика концентраторов уведомлений |
> | Action | Microsoft.NotificationHubs/operations/read | Возвращает список поддерживаемых операций для поставщика Центров уведомлений |
> | Action | Microsoft.NotificationHubs/register/action | Регистрирует подписку для поставщика ресурсов центров уведомлений и позволяет создавать пространства имен и центры уведомлений. |
> | Action | Microsoft.NotificationHubs/unregister/action | Отменяет регистрацию подписки для поставщика ресурсов центров уведомлений, позволяющей создавать пространства имен и центры уведомлений. |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.OffAzure/HyperVSites/clusters/read | Получает свойства кластера Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/clusters/write | Создает или обновляет кластер Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/delete | Удаляет сайт Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/hosts/read | Получает свойства узла Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/hosts/write | Создает или обновляет узел Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/jobs/read | Получает свойства заданий Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/machines/read | Получает свойства компьютеров Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/machines/start/action | Запускает компьютеры Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/machines/stop/action | Останавливает компьютеры Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/operationsstatus/read | Получает свойства состояния операции Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/read | Получает свойства сайта Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/refresh/action | Обновляет объекты в пределах сайта Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/runasaccounts/read | Получает свойства учетных записей запуска от имени Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/usage/read | Получает сведения об использовании сайта Hyper-V. |
> | Action | Microsoft.OffAzure/HyperVSites/write | Создает или обновляет сайт Hyper-V. |
> | Action | Microsoft.OffAzure/Operations/read | Чтение предоставленных операций. |
> | Action | Microsoft.OffAzure/register/action | Регистрирует подписку в поставщике ресурсов Microsoft.OffAzure. |
> | Action | Microsoft.OffAzure/VMwareSites/delete | Удаляет сайт VMware. |
> | Action | Microsoft.OffAzure/VMwareSites/jobs/read | Получает свойства заданий VMware. |
> | Action | Microsoft.OffAzure/VMwareSites/machines/read | Получает свойства компьютеров VMware. |
> | Action | Microsoft.OffAzure/VMwareSites/machines/start/action | Запускает компьютеры VMware. |
> | Action | Microsoft.OffAzure/VMwareSites/machines/stop/action | Останавливает компьютеры VMware. |
> | Action | Microsoft.OffAzure/VMwareSites/operationsstatus/read | Получает свойства состояния операции VMware. |
> | Action | Microsoft.OffAzure/VMwareSites/read | Получает свойства сайта VMware. |
> | Action | Microsoft.OffAzure/VMwareSites/refresh/action | Обновляет объекты в пределах сайта VMware. |
> | Action | Microsoft.OffAzure/VMwareSites/runasaccounts/read | Получает свойства учетных записей запуска от имени VMware. |
> | Action | Microsoft.OffAzure/VMwareSites/usage/read | Получает сведения об использовании сайта VMware. |
> | Action | Microsoft.OffAzure/VMwareSites/vcenters/read | Получает свойства VMware vCenter. |
> | Action | Microsoft.OffAzure/VMwareSites/vcenters/write | Создает или обновляет VMware vCenter. |
> | Action | Microsoft.OffAzure/VMwareSites/write | Создает или обновляет сайт VMware. |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.OperationalInsights/linkTargets/read | Выводит список существующих учетных записей, которые не связаны с подпиской Azure. Чтобы связать эту подписку Azure с рабочей областью, укажите идентификатор клиента, возвращаемый этой операцией, в свойстве идентификатора клиента в операции создания рабочей области. |
> | Action | microsoft.operationalinsights/operations/read | Отображение списка всех доступных операций REST API OperationalInsights. |
> | Action | Microsoft. operationalinsights/Register/действие | Рергистерс подписку. |
> | Action | Microsoft.OperationalInsights/register/action | Регистрирует подписку для поставщика ресурсов. |
> | Action | Microsoft. operationalinsights/отменить регистрацию или действие | Отменяет регистрацию подписки. |
> | Action | Microsoft.OperationalInsights/workspaces/analytics/query/action | Поиск с помощью нового механизма. |
> | Action | Microsoft.OperationalInsights/workspaces/analytics/query/schema/read | Получение схемы поиска версии 2. |
> | Action | Microsoft.OperationalInsights/workspaces/api/query/action | Поиск с помощью нового механизма. |
> | Action | Microsoft.OperationalInsights/workspaces/api/query/schema/read | Получение схемы поиска версии 2. |
> | Action | Microsoft.OperationalInsights/workspaces/configurationScopes/delete | Удаляет область конфигурации. |
> | Action | Microsoft.OperationalInsights/workspaces/configurationScopes/read | Возвращает область конфигурации. |
> | Action | Microsoft.OperationalInsights/workspaces/configurationScopes/write | Задает область конфигурации. |
> | Action | Microsoft.OperationalInsights/workspaces/datasources/delete | Удаляет источники данных в рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/datasources/read | Возвращает источники данных в рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/datasources/write | Создает или обновляет источники данных в рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/delete | Удаляет рабочую область. Если при создании рабочая область была связана с существующей рабочей областью, то эта связанная рабочая область не удаляется. |
> | Action | Microsoft.OperationalInsights/workspaces/gateways/delete | Удаляет шлюз, настроенный для рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/generateregistrationcertificate/action | Создает сертификат регистрации для рабочей области. Этот сертификат используется для подключения Microsoft System Center Operations Manager к рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/intelligencepacks/disable/action | Отключает пакет аналитики для заданной рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/intelligencepacks/enable/action | Включает пакет аналитики для заданной рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/intelligencepacks/read | Выводит список всех пакетов аналитики, которые являются видимыми для заданной рабочей области, а также сведения о том, включен или отключен пакет для этой рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/linkedServices/delete | Удаление связанной службы в заданной рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/linkedServices/read | Получение связанных служб в заданной рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/linkedServices/write | Создание или обновление связанных служб в заданной рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/listKeys/action | Получение списка ключей для рабочей области. Эти ключи используются для подключения агентов Microsoft Operational Insights к рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/listKeys/read | Получение списка ключей для рабочей области. Эти ключи используются для подключения агентов Microsoft Operational Insights к рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/managementGroups/read | Возвращает имена и метаданные для групп управления System Center Operations Manager, подключенных к этой рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/metricDefinitions/read | Получение определений метрик в рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/notificationSettings/delete | Удаление параметров уведомления пользователя для рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/notificationSettings/read | Получение параметров уведомления пользователя для рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/notificationSettings/write | Настройка параметров уведомления пользователя для рабочей области. |
> | Action | Microsoft. operationalinsights/рабочие области/операции/чтение | Возвращает состояние операции рабочей области OperationalInsights. |
> | Action | Microsoft.OperationalInsights/workspaces/purge/action | Удаление указанных данных из рабочей области. |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Ааддомаинсервицесаккаунтлогон/чтение | Чтение данных из таблицы Ааддомаинсервицесаккаунтлогон |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Ааддомаинсервицесаккаунтманажемент/чтение | Чтение данных из таблицы Ааддомаинсервицесаккаунтманажемент |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Ааддомаинсервицесдиректорисервицеакцесс/чтение | Чтение данных из таблицы Ааддомаинсервицесдиректорисервицеакцесс |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Ааддомаинсервицеслогонлогофф/чтение | Чтение данных из таблицы Ааддомаинсервицеслогонлогофф |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Ааддомаинсервицесполицичанже/чтение | Чтение данных из таблицы Ааддомаинсервицесполицичанже |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Ааддомаинсервицеспривилежеусе/чтение | Чтение данных из таблицы Ааддомаинсервицеспривилежеусе |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Ааддомаинсервицессистемсекурити/чтение | Чтение данных из таблицы Ааддомаинсервицессистемсекурити |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADAssessmentRecommendation/read | Чтение данных из таблицы ADAssessmentRecommendation |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Адфактивитирун/чтение | Чтение данных из таблицы Адфактивитирун |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Адфпипелинерун/чтение | Чтение данных из таблицы Адфпипелинерун |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Адфтригжеррун/чтение | Чтение данных из таблицы Адфтригжеррун |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADReplicationResult/read | Чтение данных из таблицы ADReplicationResult |
> | Action | Microsoft.OperationalInsights/workspaces/query/ADSecurityAssessmentRecommendation/read | Чтение данных из таблицы ADSecurityAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/Alert/read | Чтение данных из таблицы Alert |
> | Action | Microsoft.OperationalInsights/workspaces/query/AlertHistory/read | Чтение данных из таблицы AlertHistory |
> | Action | Microsoft.OperationalInsights/workspaces/query/AppCenterError/read | Чтение данных из таблицы AppCenterError |
> | Action | Microsoft.OperationalInsights/workspaces/query/ApplicationInsights/read | Чтение данных из таблицы ApplicationInsights |
> | Action | Microsoft.OperationalInsights/workspaces/query/AuditLogs/read | Чтение данных из таблицы AuditLogs |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Аутоскаливалуатионслог/чтение | Чтение данных из таблицы Аутоскаливалуатионслог |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Аутоскалескалеактионслог/чтение | Чтение данных из таблицы Аутоскалескалеактионслог |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Авсклаудтраил/чтение | Чтение данных из таблицы Авсклаудтраил |
> | Action | Microsoft.OperationalInsights/workspaces/query/AzureActivity/read | Чтение данных из таблицы AzureActivity |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Азуреассессментрекоммендатион/чтение | Чтение данных из таблицы Азуреассессментрекоммендатион |
> | Action | Microsoft.OperationalInsights/workspaces/query/AzureMetrics/read | Чтение данных из таблицы AzureMetrics |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Блоккчаинаппликатионлог/чтение | Чтение данных из таблицы Блоккчаинаппликатионлог |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Блоккчаинпроксилог/чтение | Чтение данных из таблицы Блоккчаинпроксилог |
> | Action | Microsoft.OperationalInsights/workspaces/query/BoundPort/read | Чтение данных из таблицы BoundPort |
> | Action | Microsoft.OperationalInsights/workspaces/query/CommonSecurityLog/read | Чтение данных из таблицы CommonSecurityLog |
> | Action | Microsoft.OperationalInsights/workspaces/query/ComputerGroup/read | Чтение данных из таблицы ComputerGroup |
> | Action | Microsoft.OperationalInsights/workspaces/query/ConfigurationChange/read | Чтение данных из таблицы ConfigurationChange |
> | Action | Microsoft.OperationalInsights/workspaces/query/ConfigurationData/read | Чтение данных из таблицы ConfigurationData |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerImageInventory/read | Чтение данных из таблицы ContainerImageInventory |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerInventory/read | Чтение данных из таблицы ContainerInventory |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerLog/read | Чтение данных из таблицы ContainerLog |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Контаинернодеинвентори/чтение | Чтение данных из таблицы Контаинернодеинвентори |
> | Action | Microsoft.OperationalInsights/workspaces/query/ContainerServiceLog/read | Чтение данных из таблицы ContainerServiceLog |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Датабрикксаккаунтс/чтение | Чтение данных из таблицы Датабрикксаккаунтс |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Датабрикксклустерс/чтение | Чтение данных из таблицы Датабрикксклустерс |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Датабрикксдбфс/чтение | Чтение данных из таблицы Датабрикксдбфс |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Датабрикксжобс/чтение | Чтение данных из таблицы Датабрикксжобс |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Датабриккснотебук/чтение | Чтение данных из таблицы Датабриккснотебук |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Датабриккссекретс/чтение | Чтение данных из таблицы Датабриккссекретс |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Датабриккссклпермиссионс/чтение | Чтение данных из таблицы Датабриккссклпермиссионс |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Датабриккссш/чтение | Чтение данных из таблицы Датабриккссш |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Датабриккстаблес/чтение | Чтение данных из таблицы Датабриккстаблес |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Датабрикксворкспаце/чтение | Чтение данных из таблицы Датабрикксворкспаце |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceAppCrash/read | Чтение данных из таблицы DeviceAppCrash |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceAppLaunch/read | Чтение данных из таблицы DeviceAppLaunch |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceCalendar/read | Чтение данных из таблицы DeviceCalendar |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceCleanup/read | Чтение данных из таблицы DeviceCleanup |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceConnectSession/read | Чтение данных из таблицы DeviceConnectSession |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceEtw/read | Чтение данных из таблицы DeviceEtw |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceHardwareHealth/read | Чтение данных из таблицы DeviceHardwareHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceHealth/read | Чтение данных из таблицы DeviceHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceHeartbeat/read | Чтение данных из таблицы DeviceHeartbeat |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeHeartbeat/read | Чтение данных из таблицы DeviceSkypeHeartbeat |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceSkypeSignIn/read | Чтение данных из таблицы DeviceSkypeSignIn |
> | Action | Microsoft.OperationalInsights/workspaces/query/DeviceSleepState/read | Чтение данных из таблицы DeviceSleepState |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHAppFailure/read | Чтение данных из таблицы DHAppFailure |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHAppReliability/read | Чтение данных из таблицы DHAppReliability |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHDriverReliability/read | Чтение данных из таблицы DHDriverReliability |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHLogonFailures/read | Чтение данных из таблицы DHLogonFailures |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHLogonMetrics/read | Чтение данных из таблицы DHLogonMetrics |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHOSCrashData/read | Чтение данных из таблицы DHOSCrashData |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHOSReliability/read | Чтение данных из таблицы DHOSReliability |
> | Action | Microsoft.OperationalInsights/workspaces/query/DHWipAppLearning/read | Чтение данных из таблицы DHWipAppLearning |
> | Action | Microsoft.OperationalInsights/workspaces/query/DnsEvents/read | Чтение данных из таблицы DnsEvents |
> | Action | Microsoft.OperationalInsights/workspaces/query/DnsInventory/read | Чтение данных из таблицы DnsInventory |
> | Action | Microsoft.OperationalInsights/workspaces/query/ETWEvent/read | Чтение данных из таблицы ETWEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/Event/read | Чтение данных из таблицы Event |
> | Action | Microsoft.OperationalInsights/workspaces/query/ExchangeAssessmentRecommendation/read | Чтение данных из таблицы ExchangeAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/ExchangeOnlineAssessmentRecommendation/read | Чтение данных из таблицы ExchangeOnlineAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/Heartbeat/read | Чтение данных из таблицы Heartbeat |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Хунтингбукмарк/чтение | Чтение данных из таблицы Хунтингбукмарк |
> | Action | Microsoft.OperationalInsights/workspaces/query/IISAssessmentRecommendation/read | Чтение данных из таблицы IISAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/InboundConnection/read | Чтение данных из таблицы InboundConnection |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Инсигхтсметрикс/чтение | Чтение данных из таблицы Инсигхтсметрикс |
> | Action | Microsoft.OperationalInsights/workspaces/query/IntuneAuditLogs/read | Чтение данных из таблицы IntuneAuditLogs. |
> | Action | Microsoft.OperationalInsights/workspaces/query/IntuneOperationalLogs/read | Чтение данных из таблицы IntuneOperationalLogs. |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeEvents/read | Чтение данных из таблицы KubeEvents |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeNodeInventory/read | Чтение данных из таблицы KubeNodeInventory |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubePodInventory/read | Чтение данных из таблицы KubePodInventory |
> | Action | Microsoft.OperationalInsights/workspaces/query/KubeServices/read | Чтение данных из таблицы KubeServices |
> | Action | Microsoft.OperationalInsights/workspaces/query/LinuxAuditLog/read | Чтение данных из таблицы LinuxAuditLog |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplication/read | Чтение данных из таблицы MAApplication |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealth/read | Чтение данных из таблицы MAApplicationHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthAlternativeVersions/read | Чтение данных из таблицы MAApplicationHealthAlternativeVersions |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationHealthIssues/read | Чтение данных из таблицы MAApplicationHealthIssues |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstance/read | Чтение данных из таблицы MAApplicationInstance |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationInstanceReadiness/read | Чтение данных из таблицы MAApplicationInstanceReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAApplicationReadiness/read | Чтение данных из таблицы MAApplicationReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeploymentPlan/read | Чтение данных из таблицы MADeploymentPlan |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevice/read | Чтение данных из таблицы MADevice |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeviceNotEnrolled/read | Считывание данных из таблицы MADeviceNotEnrolled |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Мадевиценрт/чтение | Чтение данных из таблицы Мадевиценрт |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealth/read | Чтение данных из таблицы MADevicePnPHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthAlternativeVersions/read | Чтение данных из таблицы MADevicePnPHealthAlternativeVersions |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADevicePnPHealthIssues/read | Чтение данных из таблицы MADevicePnPHealthIssues |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADeviceReadiness/read | Чтение данных из таблицы MADeviceReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADriverInstanceReadiness/read | Чтение данных из таблицы MADriverInstanceReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MADriverReadiness/read | Чтение данных из таблицы MADriverReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddin/read | Чтение данных из таблицы MAOfficeAddin |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinEntityHealth/read | Чтение данных из таблицы Маоффицеаддинентитихеалс |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealth/read | Чтение данных из таблицы MAOfficeAddinHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthEventNRT/read | Чтение данных из таблицы Маоффицеаддинхеалсевентнрт |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinHealthIssues/read | Чтение данных из таблицы MAOfficeAddinHealthIssues |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstance/read | Чтение данных из таблицы MAOfficeAddinInstance |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinInstanceReadiness/read | Чтение данных из таблицы MAOfficeAddinInstanceReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAddinReadiness/read | Чтение данных из таблицы MAOfficeAddinReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeApp/read | Чтение данных из таблицы MAOfficeApp |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Маоффицеаппкрашеснрт/чтение | Чтение данных из таблицы Маоффицеаппкрашеснрт |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppHealth/read | Чтение данных из таблицы MAOfficeAppHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppInstance/read | Чтение данных из таблицы MAOfficeAppInstance |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Маоффицеаппинстанцехеалс/чтение | Чтение данных из таблицы Маоффицеаппинстанцехеалс |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeAppReadiness/read | Чтение данных из таблицы MAOfficeAppReadiness |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Маоффицеаппсессионснрт/чтение | Чтение данных из таблицы Маоффицеаппсессионснрт |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeBuildInfo/read | Чтение данных из таблицы MAOfficeBuildInfo |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessment/read | Чтение данных из таблицы MAOfficeCurrencyAssessment |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeCurrencyAssessmentDailyCounts/read | Чтение данных из таблицы MAOfficeCurrencyAssessmentDailyCounts |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeDeploymentStatus/read | Чтение данных из таблицы MAOfficeDeploymentStatus |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Маоффицедеплойментстатуснрт/чтение | Чтение данных из таблицы Маоффицедеплойментстатуснрт |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Маоффицемакроеррорнрт/чтение | Чтение данных из таблицы Маоффицемакроеррорнрт |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Маоффицемакроглобалхеалс/чтение | Чтение данных из таблицы Маоффицемакроглобалхеалс |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealth/read | Чтение данных из таблицы MAOfficeMacroHealth |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroHealthIssues/read | Чтение данных из таблицы MAOfficeMacroHealthIssues |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueInstanceReadiness/read | Чтение данных из таблицы MAOfficeMacroIssueInstanceReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroIssueReadiness/read | Чтение данных из таблицы MAOfficeMacroIssueReadiness |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeMacroSummary/read | Чтение данных из таблицы MAOfficeMacroSummary |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuite/read | Чтение данных из таблицы MAOfficeSuite |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAOfficeSuiteInstance/read | Чтение данных из таблицы MAOfficeSuiteInstance |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAProposedPilotDevices/read | Чтение данных из таблицы MAProposedPilotDevices |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsBuildInfo/read | Чтение данных из таблицы MAWindowsBuildInfo |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessment/read | Чтение данных из таблицы MAWindowsCurrencyAssessment |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsCurrencyAssessmentDailyCounts/read | Чтение данных из таблицы MAWindowsCurrencyAssessmentDailyCounts |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsDeploymentStatus/read | Чтение данных из таблицы MAWindowsDeploymentStatus |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Мавиндовсдеплойментстатуснрт/чтение | Чтение данных из таблицы Мавиндовсдеплойментстатуснрт |
> | Action | Microsoft.OperationalInsights/workspaces/query/MAWindowsSysReqInstanceReadiness/read | Чтение данных из таблицы MAWindowsSysReqInstanceReadiness |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Микрософтвебаппликатионлог/чтение | Чтение данных из таблицы Микрософтвебаппликатионлог |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Микрософтвебфунктионексекутионлогс/чтение | Чтение данных из таблицы Микрософтвебфунктионексекутионлогс |
> | Action | Microsoft.OperationalInsights/workspaces/query/MicrosoftWebStdOutStdErrLog/read | Чтение данных из таблицы Микрософтвебстдаутстдеррлог |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/MicrosoftWebW3CLog/чтение | Чтение данных из таблицы MicrosoftWebW3CLog |
> | Action | Microsoft.OperationalInsights/workspaces/query/NetworkMonitoring/read | Чтение данных из таблицы NetworkMonitoring |
> | Action | Microsoft.OperationalInsights/workspaces/query/OfficeActivity/read | Чтение данных из таблицы OfficeActivity |
> | Action | Microsoft.OperationalInsights/workspaces/query/Operation/read | Чтение данных из таблицы Operation |
> | Action | Microsoft.OperationalInsights/workspaces/query/OutboundConnection/read | Чтение данных из таблицы OutboundConnection |
> | Action | Microsoft.OperationalInsights/workspaces/query/Perf/read | Чтение данных из таблицы Perf |
> | Action | Microsoft.OperationalInsights/workspaces/query/ProtectionStatus/read | Чтение данных из таблицы ProtectionStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/read | Выполнение запросов к данным в рабочей области |
> | Action | Microsoft.OperationalInsights/workspaces/query/ReservedAzureCommonFields/read | Чтение данных из таблицы ReservedAzureCommonFields |
> | Action | Microsoft.OperationalInsights/workspaces/query/ReservedCommonFields/read | Чтение данных из таблицы ReservedCommonFields |
> | Action | Microsoft.OperationalInsights/workspaces/query/SCCMAssessmentRecommendation/read | Чтение данных из таблицы SCCMAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SCOMAssessmentRecommendation/read | Чтение данных из таблицы SCOMAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityAlert/read | Чтение данных из таблицы SecurityAlert |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityBaseline/read | Чтение данных из таблицы SecurityBaseline |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityBaselineSummary/read | Чтение данных из таблицы SecurityBaselineSummary |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityDetection/read | Чтение данных из таблицы SecurityDetection |
> | Action | Microsoft.OperationalInsights/workspaces/query/SecurityEvent/read | Чтение данных из таблицы SecurityEvent |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Секуритиотравевент/чтение | Чтение данных из таблицы Секуритиотравевент |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Секуритирекоммендатион/чтение | Чтение данных из таблицы Секуритирекоммендатион |
> | Action | Microsoft.OperationalInsights/workspaces/query/ServiceFabricOperationalEvent/read | Чтение данных из таблицы ServiceFabricOperationalEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableActorEvent/read | Чтение данных из таблицы ServiceFabricReliableActorEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/ServiceFabricReliableServiceEvent/read | Чтение данных из таблицы ServiceFabricReliableServiceEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/SfBAssessmentRecommendation/read | Чтение данных из таблицы SfBAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SfBOnlineAssessmentRecommendation/read | Чтение данных из таблицы SfBOnlineAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SharePointOnlineAssessmentRecommendation/read | Чтение данных из таблицы SharePointOnlineAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SigninLogs/read | Чтение данных из таблицы SigninLogs |
> | Action | Microsoft.OperationalInsights/workspaces/query/SPAssessmentRecommendation/read | Чтение данных из таблицы SPAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SQLAssessmentRecommendation/read | Чтение данных из таблицы SQLAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/SQLQueryPerformance/read | Чтение данных из таблицы SQLQueryPerformance |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Склсреатпротектионлогинаудитс/чтение | Чтение данных из таблицы Склсреатпротектионлогинаудитс |
> | Action | Microsoft.OperationalInsights/workspaces/query/SqlVulnerabilityAssessmentResult/read | Чтение данных из таблицы Склвулнерабилитяссессментресулт |
> | Action | Microsoft.OperationalInsights/workspaces/query/Syslog/read | Чтение данных из таблицы Syslog |
> | Action | Microsoft.OperationalInsights/workspaces/query/SysmonEvent/read | Чтение данных из таблицы SysmonEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/Tables.Custom/read | Чтение данных из любого пользовательского журнала |
> | Action | Microsoft. OperationalInsights/рабочие области/запрос/Среатинтеллиженцеиндикатор/чтение | Чтение данных из таблицы Среатинтеллиженцеиндикатор |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAApp/read | Чтение данных из таблицы UAApp |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAComputer/read | Чтение данных из таблицы UAComputer |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAComputerRank/read | Чтение данных из таблицы UAComputerRank |
> | Action | Microsoft.OperationalInsights/workspaces/query/UADriver/read | Чтение данных из таблицы UADriver |
> | Action | Microsoft.OperationalInsights/workspaces/query/UADriverProblemCodes/read | Чтение данных из таблицы UADriverProblemCodes |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAFeedback/read | Чтение данных из таблицы UAFeedback |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAHardwareSecurity/read | Чтение данных из таблицы UAHardwareSecurity |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAIESiteDiscovery/read | Чтение данных из таблицы UAIESiteDiscovery |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAOfficeAddIn/read | Чтение данных из таблицы UAOfficeAddIn |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAProposedActionPlan/read | Чтение данных из таблицы UAProposedActionPlan |
> | Action | Microsoft.OperationalInsights/workspaces/query/UASysReqIssue/read | Чтение данных из таблицы UASysReqIssue |
> | Action | Microsoft.OperationalInsights/workspaces/query/UAUpgradedComputer/read | Чтение данных из таблицы UAUpgradedComputer |
> | Action | Microsoft.OperationalInsights/workspaces/query/Update/read | Чтение данных из таблицы Update |
> | Action | Microsoft.OperationalInsights/workspaces/query/UpdateRunProgress/read | Чтение данных из таблицы UpdateRunProgress |
> | Action | Microsoft.OperationalInsights/workspaces/query/UpdateSummary/read | Чтение данных из таблицы UpdateSummary |
> | Action | Microsoft.OperationalInsights/workspaces/query/Usage/read | Чтение данных из таблицы Usage |
> | Action | Microsoft.OperationalInsights/workspaces/query/VMBoundPort/read | Считывает данные из таблицы VMBoundPort. |
> | Action | Microsoft.OperationalInsights/workspaces/query/VMConnection/read | Считывает данные из таблицы VMConnection. |
> | Action | Microsoft.OperationalInsights/workspaces/query/W3CIISLog/read | Чтение данных из таблицы W3CIISLog |
> | Action | Microsoft.OperationalInsights/workspaces/query/WaaSDeploymentStatus/read | Чтение данных из таблицы WaaSDeploymentStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/WaaSInsiderStatus/read | Чтение данных из таблицы WaaSInsiderStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/WaaSUpdateStatus/read | Чтение данных из таблицы WaaSUpdateStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/WDAVStatus/read | Чтение данных из таблицы WDAVStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/WDAVThreat/read | Чтение данных из таблицы WDAVThreat |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsClientAssessmentRecommendation/read | Чтение данных из таблицы WindowsClientAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsEvent/read | Чтение данных из таблицы WindowsEvent |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsFirewall/read | Чтение данных из таблицы WindowsFirewall |
> | Action | Microsoft.OperationalInsights/workspaces/query/WindowsServerAssessmentRecommendation/read | Чтение данных из таблицы WindowsServerAssessmentRecommendation |
> | Action | Microsoft.OperationalInsights/workspaces/query/WireData/read | Чтение данных из таблицы WireData |
> | Action | Microsoft.OperationalInsights/workspaces/query/WorkloadMonitoringPerf/read | Чтение данных из таблицы WorkloadMonitoringPerf. |
> | Action | Microsoft.OperationalInsights/workspaces/query/WUDOAggregatedStatus/read | Чтение данных из таблицы WUDOAggregatedStatus |
> | Action | Microsoft.OperationalInsights/workspaces/query/WUDOStatus/read | Чтение данных из таблицы WUDOStatus |
> | Action | Microsoft.OperationalInsights/workspaces/read | Возвращает существующую рабочую область. |
> | Action | Microsoft.OperationalInsights/workspaces/regeneratesharedkey/action | Повторно создает общий ключ указанной рабочей области |
> | Action | microsoft.operationalinsights/workspaces/rules/read | Получение всех правил генерации оповещений. |
> | Action | Microsoft.OperationalInsights/workspaces/savedSearches/delete | Удаляет сохраненный поисковый запрос. |
> | Action | Microsoft.OperationalInsights/workspaces/savedSearches/read | Возвращает сохраненный поисковый запрос. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/results/read | Получение сохраненных результатов поиска. Нерекомендуемые |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/delete | Удаление запланированных действий поиска. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/read | Получение запланированных действий поиска. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/actions/write | Создание или обновление запланированных действий поиска. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/delete | Удаление запланированного поиска. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/read | Получение запланированного поиска. |
> | Action | microsoft.operationalinsights/workspaces/savedsearches/schedules/write | Создание или обновление запланированного поиска. |
> | Action | Microsoft.OperationalInsights/workspaces/savedSearches/write | Создает сохраненный поисковый запрос. |
> | Action | Microsoft.OperationalInsights/workspaces/schema/read | Возвращает схему поиска для рабочей области.  Схема поиска содержит предоставленные поля и их типы. |
> | Action | Microsoft.OperationalInsights/workspaces/search/action | Выполняет поисковый запрос. |
> | Action | microsoft.operationalinsights/workspaces/search/read | Получение результатов поиска. Не рекомендуется. |
> | Action | Microsoft.OperationalInsights/workspaces/sharedKeys/action | Извлекает открытые ключи для рабочей области. Эти ключи используются для подключения агентов Microsoft Operational Insights к рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/sharedKeys/read | Извлекает открытые ключи для рабочей области. Эти ключи используются для подключения агентов Microsoft Operational Insights к рабочей области. |
> | Action | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/delete | Удаляет конфигурацию хранилища. Это остановит чтение данных из учетной записи хранения, выполняемое Microsoft Operational Insights. |
> | Action | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/read | Возвращает конфигурацию хранилища. |
> | Action | Microsoft.OperationalInsights/workspaces/storageinsightconfigs/write | Создает конфигурацию учетной записи хранения. Эти конфигурации используются для извлечения данных из расположения в существующей учетной записи хранения. |
> | Action | Microsoft.OperationalInsights/workspaces/upgradetranslationfailures/read | Получение журнала ошибок преобразования обновления поиска для рабочей области |
> | Action | Microsoft.OperationalInsights/workspaces/usages/read | Возвращает данные об использовании для рабочей области, включая объем данных, считываемых рабочей областью. |
> | Action | Microsoft.OperationalInsights/workspaces/write | Создает новую рабочую область или устанавливает связь с существующей рабочей областью с помощью указанного идентификатора клиента, полученного из нее. |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.OperationsManagement/managementAssociations/delete | Удаление существующей связи управления. |
> | Action | Microsoft.OperationsManagement/managementAssociations/read | Получение существующей связи управления. |
> | Action | Microsoft.OperationsManagement/managementAssociations/write | Создание новой связи управления. |
> | Action | Microsoft.OperationsManagement/managementConfigurations/delete | Удаление существующей конфигурации управления. |
> | Action | Microsoft.OperationsManagement/managementConfigurations/read | Получение существующей конфигурации управления. |
> | Action | Microsoft.OperationsManagement/managementConfigurations/write | Создание новой конфигурации управления. |
> | Action | Microsoft.OperationsManagement/register/action | Регистрирует подписку для поставщика ресурсов. |
> | Action | Microsoft.OperationsManagement/solutions/delete | Удаляет существующее решение OMS. |
> | Action | Microsoft.OperationsManagement/solutions/read | Возвращает существующее решение OMS. |
> | Action | Microsoft.OperationsManagement/solutions/write | Создает решение OMS. |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.PolicyInsights/asyncOperationResults/read | Получает результат асинхронной операции. |
> | Action | Microsoft. Полициинсигхтс/операции/чтение | Получение поддерживаемых операций для пространства имен Microsoft. Полициинсигхтс |
> | Action | Microsoft.PolicyInsights/policyEvents/queryResults/action | Запрос сведений о событиях политики. |
> | Action | Microsoft.PolicyInsights/policyEvents/queryResults/read | Запрос сведений о событиях политики. |
> | Action | Microsoft.PolicyInsights/policyStates/queryResults/action | Запрос сведений о состояниях политики. |
> | Action | Microsoft.PolicyInsights/policyStates/queryResults/read | Запрос сведений о состояниях политики. |
> | Action | Microsoft.PolicyInsights/policyStates/summarize/action | Запрос сводки последних состояний политики. |
> | Action | Microsoft.PolicyInsights/policyStates/summarize/read | Запрос сводки последних состояний политики. |
> | Action | Microsoft.PolicyInsights/policyStates/triggerEvaluation/action | Запускает новую оценку соответствия для выбранной области. |
> | Action | Microsoft.PolicyInsights/policyTrackedResources/queryResults/read | Запросите сведения о ресурсах, необходимых политике DeployIfNotExists. |
> | Action | Microsoft.PolicyInsights/register/action | Регистрирует поставщик ресурсов Microsoft Policy Insights и включает на нем действия. |
> | Action | Microsoft.PolicyInsights/remediations/cancel/action | Отмена выполняющихся исправлений политики Майкрософт. |
> | Action | Microsoft.PolicyInsights/remediations/delete | Удаление исправлений политики. |
> | Action | Microsoft.PolicyInsights/remediations/listDeployments/read | Перечисление развертываний, необходимых для исправления политики. |
> | Action | Microsoft.PolicyInsights/remediations/read | Получение исправлений политики. |
> | Action | Microsoft.PolicyInsights/remediations/write | Создание или обновление исправлений политик Майкрософт. |
> | Action | Microsoft. Полициинсигхтс/отменить регистрацию или действие | Отменяет регистрацию поставщика ресурсов Microsoft Policy Insights. |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Portal/consoles/delete | Удаляет экземпляр Cloud Shell. |
> | Action | Microsoft. портал/консоли/чтение | Считывает экземпляр Cloud Shell. |
> | Action | Microsoft.Portal/consoles/write | Создание или обновление экземпляра Cloud Shell. |
> | Action | Microsoft.Portal/dashboards/delete | Удаляет панель мониторинга из подписки. |
> | Action | Microsoft.Portal/dashboards/read | Считывает панели мониторинга для подписки. |
> | Action | Microsoft.Portal/dashboards/write | Добавить или изменить панель мониторинга для подписки. |
> | Action | Microsoft.Portal/register/action | Регистрация на портале. |
> | Action | Microsoft.Portal/usersettings/delete | Удаляет параметры пользователя Cloud Shell. |
> | Action | Microsoft.Portal/usersettings/read | Считывает параметры пользователя Cloud Shell. |
> | Action | Microsoft.Portal/usersettings/write | Создание или обновление параметров пользователя Cloud Shell. |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.PowerBIDedicated/capacities/delete | Удаление выделенной емкости Power BI. |
> | Action | Microsoft.PowerBIDedicated/capacities/read | Извлечение сведений об указанной выделенной емкости Power BI. |
> | Action | Microsoft.PowerBIDedicated/capacities/resume/action | Возобновляет использование емкости. |
> | Action | Microsoft.PowerBIDedicated/capacities/skus/read | Получение доступных сведений о номере SKU для емкости. |
> | Action | Microsoft.PowerBIDedicated/capacities/suspend/action | Приостановка использования емкости. |
> | Action | Microsoft.PowerBIDedicated/capacities/write | Создание или обновление указанной выделенной емкости Power BI. |
> | Action | Microsoft. Повербидедикатед/Locations/Чеккнамеаваилабилити/Action | Проверка того, что имя выделенной емкости Power BI является допустимым и не используется. |
> | Action | Microsoft.PowerBIDedicated/locations/operationresults/read | Получение сведений о результате указанной операции. |
> | Action | Microsoft.PowerBIDedicated/locations/operationstatuses/read | Извлечение сведений о состоянии указанной операции. |
> | Action | Microsoft.PowerBIDedicated/operations/read | Извлечение сведений об операциях. |
> | Action | Microsoft.PowerBIDedicated/register/action | Регистрация выделенного поставщика ресурсов Power BI. |
> | Action | Microsoft.PowerBIDedicated/skus/read | Извлечение сведений о номерах SKU. |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.RecoveryServices/locations/allocatedStamp/read | GetAllocatedStamp — внутренняя операция, используемая службой |
> | Action | Microsoft.RecoveryServices/locations/allocateStamp/action | AllocatedStamp является внутренней операцией, используемой службой. |
> | Action | Microsoft. recoveryservices/Locations/Баккуппревалидатепротектион/Action |  |
> | Action | Microsoft. recoveryservices/Locations/Баккуппротектедитем/Write | Создает элемент, защищенный службой архивации. |
> | Action | Microsoft. recoveryservices/Locations/Баккуппротектедитемс/Read | Возврат списка всех защищенных элементов. |
> | Action | Microsoft. recoveryservices/Locations/Баккупстатус/Action | Проверка состояния архивации хранилищ служб восстановления. |
> | Action | Microsoft. recoveryservices/Locations/Баккупвалидатефеатурес/Action | Проверка компонентов. |
> | Action | Microsoft.RecoveryServices/locations/checkNameAvailability/action | Проверка доступности имени ресурса использует API, позволяющий проверить, доступно ли имя ресурса. |
> | Action | Microsoft.RecoveryServices/locations/operationStatus/read | Возвращает состояние операции для данной операции |
> | Action | Microsoft.RecoveryServices/operations/read | Получение списка операций для поставщика ресурсов. |
> | Action | Microsoft.RecoveryServices/register/action | Регистрация подписки для заданного поставщика ресурсов. |
> | Action | Microsoft. recoveryservices/хранилища/баккупконфиг/чтение | Возвращает конфигурацию хранилища служб восстановления. |
> | Action | Microsoft. recoveryservices/Vault/баккупконфиг/запись | Обновляет конфигурацию хранилища служб восстановления. |
> | Action | Microsoft. recoveryservices/хранилища/Баккупенгинес/чтение | Возвращает все серверы управления архивацией, зарегистрированные в хранилище. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Баккуппротектионинтент/удалить | Удаление цели защиты для резервного копирования. |
> | Action | Microsoft. recoveryservices/хранилища/Баккупфабрикс/Баккуппротектионинтент/чтение | Получение цели защиты для резервного копирования. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Баккуппротектионинтент/запись | Создание цели защиты для резервной копии. |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/operationResults/read | Возвращает состояние операции |
> | Action | Microsoft. recoveryservices/хранилища/Баккупфабрикс/Протектаблеконтаинерс/чтение | Получение всех защищаемых контейнеров. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/удалить | Удаление зарегистрированного контейнера. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/запрос/действие | Запрос рабочих нагрузок в контейнере. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/элементы/чтение | Получение всех элементов в контейнере. |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/operationResults/read | Получение результата операции с контейнером защиты. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/Протектедитемс/резервное копирование/действие | Выполнение архивации для защищенного элемента. |
> | Action | Microsoft. recoveryservices/хранилища/Баккупфабрикс/Протектионконтаинерс/Протектедитемс/Delete | Удаляет защищенный элемент. |
> | Action | microsoft.recoveryservices/Vaults/backupFabrics/protectionContainers/protectedItems/operationResults/read | Получение результата операций с защищенными элементами. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/Протектедитемс/Оператионсстатус/чтение | Возврат состояния для операции с защищенными элементами. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/Протектедитемс/чтение | Возвращает сведения об объекте защищенного элемента |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/Протектедитемс/Рековерипоинтс/Провисионинстантитемрековери/действие | Подготавливает мгновенное восстановление для защищенного элемента. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/Протектедитемс/Рековерипоинтс/чтение | Получение точек восстановления для защищенных элементов. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/Протектедитемс/Рековерипоинтс/Restore/Action | Восстановление точек восстановления для защищенных элементов. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/Протектедитемс/Рековерипоинтс/Ревокеинстантитемрековери/действие | Отменяет мгновенное восстановление для защищенного элемента. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/Протектедитемс/запись | Создает элемент, защищенный службой архивации. |
> | Action | Microsoft. recoveryservices/хранилища/Баккупфабрикс/Протектионконтаинерс/чтение | Возвращает все зарегистрированные контейнеры. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Протектионконтаинерс/запись | Создание зарегистрированного контейнера. |
> | Action | Microsoft. recoveryservices/Vault/Баккупфабрикс/Рефрешконтаинерс/действие | Обновляет список контейнеров. |
> | Action | Microsoft. recoveryservices/Vault/Баккупжобс/отмена/действие | Отменяет задание. |
> | Action | Microsoft. recoveryservices/хранилища/Баккупжобс/Оператионресултс/чтение | Возврат результата для операции задания. |
> | Action | Microsoft. recoveryservices/хранилища/Баккупжобс/чтение | Возвращает все объекты заданий. |
> | Action | Microsoft. recoveryservices/Vault/Баккупжобсекспорт/действие | Экспорт заданий |
> | Action | Microsoft. recoveryservices/хранилища/Баккупоператионресултс/чтение | Возврат результата архивации для хранилища служб восстановления. |
> | Action | Microsoft. recoveryservices/хранилища/Баккупоператионс/чтение | Возвращает состояние операции архивации для хранилища служб восстановления. |
> | Action | Microsoft. recoveryservices/хранилища/БаккупполиЦиес/удаление | Удаляет политику защиты. |
> | Action | Microsoft. recoveryservices/хранилища/БаккупполиЦиес/Оператионресултс/чтение | Получение результатов работы политики. |
> | Action | Microsoft. recoveryservices/Vault/БаккупполиЦиес/операции/чтение | Возвращает состояние операции политики. |
> | Action | Microsoft. recoveryservices/хранилища/БаккупполиЦиес/чтение | Возвращает все политики защиты. |
> | Action | Microsoft. recoveryservices/Vault/БаккупполиЦиес/запись | Создает политику защиты. |
> | Action | Microsoft. recoveryservices/хранилища/Баккуппротектаблеитемс/чтение | Возврат списка всех элементов, доступных для защиты. |
> | Action | Microsoft. recoveryservices/хранилища/Баккуппротектедитемс/чтение | Возврат списка всех защищенных элементов. |
> | Action | Microsoft. recoveryservices/хранилища/Баккуппротектионконтаинерс/чтение | Возвращает все контейнеры, принадлежащие подписке |
> | Action | Microsoft. recoveryservices/хранилища/Баккуппротектионинтентс/чтение | Вывод списка всех целей защиты для резервного копирования. |
> | Action | Microsoft. recoveryservices/Vault/Баккупсекуритипин/действие | Возвращает данные ПИН-кода безопасности для хранилища служб восстановления. |
> | Action | Microsoft. recoveryservices/хранилища/баккупсторажеконфиг/чтение | Возвращает конфигурацию службы хранилища для хранилища служб восстановления. |
> | Action | Microsoft. recoveryservices/Vault/баккупсторажеконфиг/запись | Обновляет конфигурацию службы хранилища для хранилища служб восстановления. |
> | Action | Microsoft. recoveryservices/хранилища/Баккупусажесуммариес/чтение | Возвращает сводки по защищенным элементам и защищенным серверам для служб восстановления. |
> | Action | Microsoft. recoveryservices/Vault/Баккупвалидатеоператион/действие | Проверка операций для защищенного элемента |
> | Action | Microsoft.RecoveryServices/Vaults/certificates/write | Операция "Обновить сертификат ресурсов" обновляет сертификат учетных данных ресурсов или хранилища. |
> | Action | Microsoft.RecoveryServices/Vaults/delete | Операция "Удалить хранилище" удаляет указанный ресурс Azure типа "vault" |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/delete | Операция "Получить расширенные сведения" получает расширенные сведения об объекте, представляющие ресурс Azure типа ?vault? |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/read | Операция "Получить расширенные сведения" получает расширенные сведения об объекте, представляющие ресурс Azure типа ?vault? |
> | Action | Microsoft.RecoveryServices/Vaults/extendedInformation/write | Операция "Получить расширенные сведения" получает расширенные сведения об объекте, представляющие ресурс Azure типа ?vault? |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringAlerts/read | Возвращает оповещения для хранилища служб восстановления. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringAlerts/write | Разрешение оповещения. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/read | Возвращает конфигурацию уведомлений хранилища служб восстановления. |
> | Action | Microsoft.RecoveryServices/Vaults/monitoringConfigurations/write | Настройка уведомлений по электронной почте в хранилище служб восстановления. |
> | Action | Microsoft.RecoveryServices/Vaults/read | Операция "Получить хранилище" получает объект, представляющий ресурс Azure типа "vault" |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/delete | Операцию "Отменить регистрацию контейнера службы" можно использовать для отмены регистрации контейнера службы. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/operationResults/read | Операцию "Получить результаты операции" можно использовать для получения состояния операции и результата асинхронно отправленной операции |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/read | Операцию "Получить контейнеры" можно использовать для регистрации контейнеров в ресурсе. |
> | Action | Microsoft.RecoveryServices/Vaults/registeredIdentities/write | Операцию "Зарегистрировать контейнер службы" можно использовать для регистрации контейнера в службе восстановления. |
> | Action | Microsoft.RecoveryServices/vaults/replicationAlertSettings/read | Считывание параметров оповещений |
> | Action | Microsoft.RecoveryServices/vaults/replicationAlertSettings/write | Создание или обновление параметров оповещений |
> | Action | Microsoft.RecoveryServices/vaults/replicationEvents/read | Считывание события |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/checkConsistency/action | Проверяет согласованность структуры |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/delete | Удаление структуры |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/deployProcessServerImage/action | Развертывает образ сервера обработки. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/migratetoaad/action | Перенос Fabric в AAD |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/read | Считывание структуры |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/reassociateGateway/action | Повторно задает связь шлюза. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/remove/action | Удаляет структуру. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/renewcertificate/action | Обновление сертификата для структуры. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationLogicalNetworks/read | Считывание логических сетей |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/read | Считывание сети |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/delete | Удаление сетевых сопоставлений |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/read | Считывание сетевых сопоставлений |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationNetworks/replicationNetworkMappings/write | Создание или обновление сетевых сопоставлений |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/discoverProtectableItem/action | Обнаруживает защищаемый элемент. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/read | Считывание контейнеров защиты |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/remove/action | Удаляет контейнер защиты. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/delete | Удаление любых элементов миграции. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrate/action | Перенос элемента. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/migrationRecoveryPoints/read | Чтение любых точек восстановления для миграции. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/read | Чтение любых элементов миграции. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrate/action | Тестирование миграции. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/testMigrateCleanup/action | Очистка тестовой миграции. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationMigrationItems/write | Создание или обновление элементов миграции. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectableItems/read | Считывает защищаемые элементы. |
> | Action | Microsoft. RecoveryServices/Vault/Репликатионфабрикс/Репликатионпротектионконтаинерс/Репликатионпротектедитемс/Адддискс/действие | Добавление дисков |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/applyRecoveryPoint/action | Применяет точку восстановления. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/delete | Удаление защищенных элементов |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/failoverCommit/action | Выполняет отработку отказа с фиксацией. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/plannedFailover/action | Плановая отработка отказа |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/read | Считывание защищенных элементов |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/recoveryPoints/read | Считывает точки восстановления для репликации. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/remove/action | Удаляет защищенный элемент. |
> | Action | Microsoft. RecoveryServices/Vault/Репликатионфабрикс/Репликатионпротектионконтаинерс/Репликатионпротектедитемс/Ремоведискс/действие | Удаление дисков |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/repairReplication/action | Исправляет репликацию. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/reProtect/action | Повторно защищает защищенный элемент. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/ResolveHealthErrors/action |  |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/submitFeedback/action | Отправить отзыв |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/targetComputeSizes/read | Считывание целевых объемов вычислительных ресурсов |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailover/action | Тестовая отработка отказа |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/testFailoverCleanup/action | Тестирует очистку отработки отказа. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/unplannedFailover/action | Отработка отказа |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/updateMobilityService/action | Обновить службу мобильности |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectedItems/write | Создание или обновление защищенных элементов |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/delete | Удаление сопоставлений контейнера защиты |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/read | Считывание сопоставлений контейнера защиты |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/remove/action | Удаляет сопоставление контейнера защиты. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/replicationProtectionContainerMappings/write | Создание или обновление сопоставлений контейнера защиты |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/switchprotection/action | Переключает контейнер защиты. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationProtectionContainers/write | Создание или обновление контейнеры защиты |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/delete | Удаление поставщиков службы восстановления |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/read | Считывание поставщиков службы восстановления |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/refreshProvider/action | Обновляет поставщик. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/remove/action | Удаляет поставщик служб восстановления. |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationRecoveryServicesProviders/write | Создание или обновление поставщиков служб восстановления |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/read | Считывание классификации хранилища |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/delete | Удаление сопоставлений классификаций хранилища |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/read | Считывание сопоставлений классификаций хранилища |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationStorageClassifications/replicationStorageClassificationMappings/write | Создание или обновление сопоставлений классификаций хранилища |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/delete | Удаление vCenter |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/read | Чтение vCenter |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/replicationvCenters/write | Создание или обновление vCenter |
> | Action | Microsoft.RecoveryServices/vaults/replicationFabrics/write | Создание или обновление Fabrics |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/cancel/action | Отменить задание |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/read | Считывание задания |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/restart/action | Перезапускает задание. |
> | Action | Microsoft.RecoveryServices/vaults/replicationJobs/resume/action | Возобновляет выполнение задания. |
> | Action | Microsoft.RecoveryServices/vaults/replicationMigrationItems/read | Чтение любых элементов миграции. |
> | Action | Microsoft.RecoveryServices/vaults/replicationNetworkMappings/read | Считывание сетевых сопоставлений |
> | Action | Microsoft.RecoveryServices/vaults/replicationNetworks/read | Считывание сети |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/delete | Удаление политики |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/read | Считывание политики |
> | Action | Microsoft.RecoveryServices/vaults/replicationPolicies/write | Создание или обновление политики |
> | Action | Microsoft.RecoveryServices/vaults/replicationProtectedItems/read | Считывание защищенных элементов |
> | Action | Microsoft.RecoveryServices/vaults/replicationProtectionContainerMappings/read | Считывание сопоставлений контейнера защиты |
> | Action | Microsoft.RecoveryServices/vaults/replicationProtectionContainers/read | Считывание контейнеров защиты |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/delete | Удаление планов восстановления |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/failoverCommit/action | Выполняет отработку отказа с фиксацией для плана восстановления. |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/plannedFailover/action | Выполняет плановую отработку отказа для плана восстановления. |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/read | Считывание планов восстановления |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/reProtect/action | Повторно защищает план восстановления. |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailover/action | Тестирует отработку отказа для плана восстановления. |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/testFailoverCleanup/action | Тестирует очистку отработки отказа для плана восстановления. |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/unplannedFailover/action | Выполняет отработку отказа для плана восстановления. |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryPlans/write | Создание или обновление планов восстановления |
> | Action | Microsoft.RecoveryServices/vaults/replicationRecoveryServicesProviders/read | Считывание поставщиков службы восстановления |
> | Action | Microsoft.RecoveryServices/vaults/replicationStorageClassificationMappings/read | Считывание сопоставлений классификаций хранилища |
> | Action | Microsoft.RecoveryServices/vaults/replicationStorageClassifications/read | Считывание классификации хранилища |
> | Action | Microsoft.RecoveryServices/vaults/replicationSupportedOperatingSystems/read | Чтение любого элемента.  |
> | Action | Microsoft.RecoveryServices/vaults/replicationUsages/read | Чтение данных об использовании хранилищ репликации |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultHealth/read | Считывание всех сведений о работоспособности репликации хранилища |
> | Action | Microsoft.RecoveryServices/vaults/replicationVaultHealth/refresh/action | Обновление сведений о работоспособности хранилища |
> | Action | Microsoft. RecoveryServices/хранилища/Репликатионваултсеттингс/чтение | Чтение любого элемента.  |
> | Action | Microsoft. RecoveryServices/Vault/Репликатионваултсеттингс/запись | Создание или обновление любого  |
> | Action | Microsoft.RecoveryServices/vaults/replicationvCenters/read | Чтение vCenter |
> | Action | Microsoft. recoveryservices/хранилища/использование/чтение | Возвращает подробности об использовании для хранилища служб восстановления. |
> | Action | Microsoft.RecoveryServices/vaults/usages/read | Считывание данных об использовании хранилищ |
> | Action | Microsoft.RecoveryServices/Vaults/vaultTokens/read | Операцию "Токен хранилища" можно использовать, чтобы получить токен хранилища для внутренних операций уровня хранилища. |
> | Action | Microsoft.RecoveryServices/Vaults/write | Операция "Создать хранилище" создает ресурс Azure типа vault. |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Relay/checkNameAvailability/action | Проверяет доступность пространства имен в рамках указанной подписки. |
> | Action | Microsoft.Relay/checkNamespaceAvailability/action | Проверяет доступность пространства имен в рамках указанной подписки. Этот API не рекомендуется. Вместо него используйте CheckNameAvailability. |
> | Action | Microsoft.Relay/namespaces/authorizationRules/action | Обновление правила авторизации для пространства имен. Это нерекомендуемый API. Чтобы обновить правило авторизации для пространства имен, используйте вызов PUT. Эта операция не поддерживается для API версии 2017-04-01. |
> | Action | Microsoft.Relay/namespaces/authorizationRules/delete | Удаляет правило авторизации для пространства имен. Невозможно удалить правило авторизации пространства имен по умолчанию.  |
> | Action | Microsoft.Relay/namespaces/authorizationRules/listkeys/action | Получить строку подключения к пространству имен |
> | Action | Microsoft.Relay/namespaces/authorizationRules/read | Получить список описаний правил авторизации пространства имен. |
> | Action | Microsoft.Relay/namespaces/authorizationRules/regenerateKeys/action | Повторное создание первичного или вторичного ключа в ресурсе |
> | Action | Microsoft.Relay/namespaces/authorizationRules/write | Создает правила авторизации уровня пространства имен и обновляет его свойства. Можно обновить права доступа, первичный ключ и вторичный ключ для правил авторизации. |
> | Action | Microsoft.Relay/namespaces/Delete | Удалить ресурс пространства имен |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Получает ключи правил авторизации для основного пространства имен аварийного восстановления |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/authorizationRules/read | Получить правила авторизации основного пространства имен аварийного восстановления |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/breakPairing/action | Отключает аварийное восстановление и останавливает репликацию изменений из основного пространства имен в дополнительное. |
> | Action | Microsoft.Relay/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Проверяет доступность псевдонима пространства имен в подписке. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/delete | Удаление конфигурации аварийного восстановления, связанной с пространством имен. Эта операция может быть вызвана только через основное пространство имен. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/failover/action | Вызывает отработку отказа географического аварийного восстановления и перенастраивает псевдоним пространства имен, чтобы он указывал на дополнительное пространство. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/read | Получает конфигурацию аварийного восстановления, связанную с пространством имен. |
> | Action | Microsoft.Relay/namespaces/disasterRecoveryConfigs/write | Создает или обновляет конфигурацию аварийного восстановления, связанную с пространством имен. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/action | Операция обновления гибридного подключения. Эта операция не поддерживается для API версии 2017-04-01. Правила авторизации. Чтобы обновить правило авторизации, используйте вызов PUT. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/delete | Операция удаления правил авторизации гибридного подключения. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/listkeys/action | Возвращает строку подключения для гибридного подключения. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/read |  Получение списка правил авторизации гибридного подключения. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/regeneratekeys/action | Повторное создание первичного или вторичного ключа в ресурсе |
> | Action | Microsoft.Relay/namespaces/HybridConnections/authorizationRules/write | Создает правила авторизации гибридного подключения и обновляет его свойства. Можно обновить права доступа к правилам авторизации. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/Delete | Операция удаления ресурса гибридного подключения. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/read | Возвращает список описаний ресурсов гибридного подключения. |
> | Action | Microsoft.Relay/namespaces/HybridConnections/write | Создает или обновляет свойства гибридного подключения. |
> | Action | Microsoft.Relay/namespaces/messagingPlan/read | Получение плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Action | Microsoft.Relay/namespaces/messagingPlan/write | Обновление плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Action | Microsoft.Relay/namespaces/operationresults/read | Получение состояния операции пространства имен |
> | Action | Microsoft.Relay/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Возвращает список описаний ресурсов метрик пространства имен. |
> | Action | Microsoft.Relay/namespaces/read | Получить список описаний ресурса пространства имен |
> | Action | Microsoft.Relay/namespaces/removeAcsNamepsace/action | Удаление пространства имен ACS |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/action | Операция обновления ретранслятора WCF. Эта операция не поддерживается для API версии 2017-04-01. Правила авторизации. Чтобы обновить правило авторизации, используйте вызов PUT. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/delete | Операция удаления правил авторизации ретранслятора WCF. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/listkeys/action | Возвращает строку подключения к ретранслятору WCF. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/read |  Получение списка правил авторизации ретранслятора WCF. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/regeneratekeys/action | Повторное создание первичного или вторичного ключа в ресурсе |
> | Action | Microsoft.Relay/namespaces/WcfRelays/authorizationRules/write | Создает правила авторизации ретранслятора WCF и обновляет его свойства. Можно обновить права доступа к правилам авторизации. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/Delete | Операция удаления ресурса ретранслятора WCF. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/read | Возвращает список описаний ресурсов ретранслятора WCF. |
> | Action | Microsoft.Relay/namespaces/WcfRelays/write | Создает или обновляет свойства ретранслятора WCF. |
> | Action | Microsoft.Relay/namespaces/write | Создает ресурс пространства имен и обновляет его свойства. К этим свойствам относятся теги и пространство имен. |
> | Action | Microsoft.Relay/operations/read | Получить операции |
> | Action | Microsoft.Relay/register/action | Регистрирует подписку для поставщика ресурсов ретранслятора и разрешает создание ресурсов ретранслятора |
> | Action | Microsoft.Relay/unregister/action | Регистрирует подписку для поставщика ресурсов ретранслятора и разрешает создание ресурсов ретранслятора |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ResourceHealth/AvailabilityStatuses/current/read | Получает состояние доступности для указанного ресурса |
> | Action | Microsoft.ResourceHealth/AvailabilityStatuses/read | Получает состояния доступности для всех ресурсов в указанной области |
> | Action | Microsoft.ResourceHealth/events/read | Получение событий работоспособности служб для данной подписки |
> | Action | Microsoft.Resourcehealth/healthevent/action | Обозначает изменение состояния работоспособности для указанного ресурса |
> | Action | Microsoft.Resourcehealth/healthevent/Activated/action | Обозначает изменение состояния работоспособности для указанного ресурса |
> | Action | Microsoft.Resourcehealth/healthevent/InProgress/action | Обозначает изменение состояния работоспособности для указанного ресурса |
> | Action | Microsoft.Resourcehealth/healthevent/Pending/action | Обозначает изменение состояния работоспособности для указанного ресурса |
> | Action | Microsoft.Resourcehealth/healthevent/Resolved/action | Обозначает изменение состояния работоспособности для указанного ресурса |
> | Action | Microsoft.Resourcehealth/healthevent/Updated/action | Обозначает изменение состояния работоспособности для указанного ресурса |
> | Action | Microsoft.ResourceHealth/impactedResources/read | Получение затронутых ресурсов для данной подписки |
> | Action | Microsoft. Ресаурцехеалс/метаданные/чтение | Получение метаданных |
> | Action | Microsoft.ResourceHealth/Operations/read | Получение операций, доступных для ResourceHealth (Майкрософт) |
> | Action | Microsoft.ResourceHealth/register/action | Регистрирует подписку на Microsoft ResourceHealth |
> | Action | Microsoft.ResourceHealth/unregister/action | Отмена регистрации подписки для ResourceHealth (Майкрософт) |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Resources/checkPolicyCompliance/action | Проверка состояния соответствия политикам для указанного ресурса. |
> | Action | Microsoft.Resources/checkResourceName/action | Проверка имени ресурса на допустимость. |
> | Action | Microsoft.Resources/deployments/cancel/action | Отменяет развертывание. |
> | Action | Microsoft.Resources/deployments/delete | Удаляет развертывание. |
> | Action | Microsoft.Resources/deployments/operations/read | Получает или перечисляет операции развертывания. |
> | Action | Microsoft.Resources/deployments/read | Получает или перечисляет развертывания. |
> | Action | Microsoft.Resources/deployments/validate/action | Проверяет развертывание. |
> | Action | Microsoft. Resources/deployments/whatIf/Action | Прогнозирует изменения в развертывании шаблона. |
> | Action | Microsoft.Resources/deployments/write | Создает или обновляет развертывание. |
> | Action | Microsoft.Resources/links/delete | Удаляет ссылку на ресурсы. |
> | Action | Microsoft.Resources/links/read | Получает или перечисляет ссылки на ресурсы. |
> | Action | Microsoft.Resources/links/write | Создает или обновляет ссылку на ресурсы. |
> | Action | Microsoft.Resources/marketplace/purchase/action | Покупка ресурса на Marketplace. |
> | Action | Microsoft.Resources/providers/read | Получить список поставщиков. |
> | Action | Microsoft.Resources/resources/read | Получить список ресурсов на основе фильтров. |
> | Action | Microsoft.Resources/subscriptions/locations/read | Получает список поддерживаемых расположений. |
> | Action | Microsoft.Resources/subscriptions/operationresults/read | Получение результатов операции подписки. |
> | Action | Microsoft.Resources/subscriptions/providers/read | Получает или перечисляет поставщики ресурсов. |
> | Action | Microsoft.Resources/subscriptions/read | Получает список подписок. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/delete | Удаляет группу ресурсов вместе с содержимым. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/operations/read | Получает или перечисляет операции развертывания. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/operationstatuses/read | Получает или перечисляет состояния операций развертывания. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/read | Получает или перечисляет развертывания. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/deployments/write | Создает или обновляет развертывание. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/moveResources/action | Перемещает ресурсы из одной группы в другую. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/read | Получает или перечисляет группы ресурсов. |
> | Action | Microsoft.Resources/subscriptions/resourcegroups/resources/read | Получение ресурсов для группы ресурсов. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/validateMoveResources/action | Проверка перемещения ресурсов из одной группы ресурсов в другую. |
> | Action | Microsoft.Resources/subscriptions/resourceGroups/write | Создает или обновляет группу ресурсов. |
> | Action | Microsoft.Resources/subscriptions/resources/read | Получает ресурсы подписки. |
> | Action | Microsoft.Resources/subscriptions/tagNames/delete | Удаляет тег подписки. |
> | Action | Microsoft.Resources/subscriptions/tagNames/read | Получает или перечисляет теги подписок. |
> | Action | Microsoft.Resources/subscriptions/tagNames/tagValues/delete | Удаляет значение тега подписки. |
> | Action | Microsoft.Resources/subscriptions/tagNames/tagValues/read | Получает или перечисляет значения тега подписки. |
> | Action | Microsoft.Resources/subscriptions/tagNames/tagValues/write | Добавляет значение тега подписки. |
> | Action | Microsoft.Resources/subscriptions/tagNames/write | Добавляет тег подписки. |
> | Action | Microsoft. Resources/Tags/удаление | Удаляет все теги в ресурсе. |
> | Action | Microsoft. Resources/Tags/чтение | Возвращает все теги для ресурса. |
> | Action | Microsoft. Resources/Tags/запись | Обновляет Теги ресурса, заменяя или объединяя существующие теги с новым набором тегов или удаляя существующие теги. |
> | Action | Microsoft.Resources/tenants/read | Получает список клиентов. |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Scheduler/jobcollections/delete | Удаление коллекции заданий. |
> | Action | Microsoft.Scheduler/jobcollections/disable/action | Отключение коллекции заданий. |
> | Action | Microsoft.Scheduler/jobcollections/enable/action | Включение коллекции заданий. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/delete | Удаление задания. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/generateLogicAppDefinition/action | Создание определения для приложения логики на основе задания планировщика. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/jobhistories/read | Получение журнала заданий. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/read | Получение задания. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/run/action | Запускает задание. |
> | Action | Microsoft.Scheduler/jobcollections/jobs/write | Создание или обновление задания. |
> | Action | Microsoft.Scheduler/jobcollections/read | Получить коллекцию заданий |
> | Action | Microsoft.Scheduler/jobcollections/write | Создание или обновление коллекции заданий. |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Search/checkNameAvailability/action | Проверяет доступность имени службы. |
> | Action | Microsoft.Search/operations/read | Выводит список всех доступных операций поставщика Microsoft.Search. |
> | Action | Microsoft.Search/register/action | Регистрирует подписку для поставщика ресурсов службы поиска и позволяет создавать службы поиска. |
> | Action | Microsoft.Search/searchServices/createQueryKey/action | Создает ключ запроса. |
> | Action | Microsoft.Search/searchServices/delete | Удаляет службу поиска. |
> | Action | Microsoft.Search/searchServices/deleteQueryKey/delete | Удаляет ключ запроса. |
> | Action | Microsoft.Search/searchServices/listAdminKeys/action | Считывает ключи администратора. |
> | Action | Microsoft.Search/searchServices/listQueryKeys/read | Возвращает список ключей API запроса для данной службы поиска Azure. |
> | Action | Microsoft.Search/searchServices/read | Считывает службу поиска. |
> | Action | Microsoft.Search/searchServices/regenerateAdminKey/action | Повторно создает ключ администратора. |
> | Action | Microsoft.Search/searchServices/start/action | Запускает службу поиска. |
> | Action | Microsoft.Search/searchServices/stop/action | Останавливает службу поиска. |
> | Action | Microsoft.Search/searchServices/write | Создает или обновляет службу поиска. |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Security/Адаптивенетворкхарденингс/принудительно/действие | Применяет заданные правила усиления защиты трафика, создавая соответствующие правила безопасности для указанных групп безопасности сети. |
> | Action | Microsoft. Security/Адаптивенетворкхарденингс/Read | Получение адаптивных рекомендаций по усилению защиты сети для ресурса, защищенного Azure. |
> | Action | Microsoft.Security/advancedThreatProtectionSettings/read | Получает параметры расширенной защиты от угроз для ресурса. |
> | Action | Microsoft.Security/advancedThreatProtectionSettings/write | Обновляет параметры расширенной защиты от угроз для ресурса. |
> | Action | Microsoft.Security/alerts/read | Получение всех доступных оповещений системы безопасности. |
> | Action | Microsoft.Security/applicationWhitelistings/read | Возвращает список разрешений приложения. |
> | Action | Microsoft.Security/applicationWhitelistings/write | Создает новый список разрешений приложения или обновляет существующий. |
> | Action | Microsoft.Security/complianceResults/read | Получение результатов соответствия для ресурса. |
> | Action | Microsoft.Security/informationProtectionPolicies/read | Получение сведений о политиках защиты для ресурса |
> | Action | Microsoft.Security/informationProtectionPolicies/write | Обновление политик защиты сведений для ресурса |
> | Action | Microsoft.Security/locations/alerts/activate/action | Активация оповещения системы безопасности. |
> | Action | Microsoft.Security/locations/alerts/dismiss/action | Отклонение оповещения системы безопасности. |
> | Action | Microsoft.Security/locations/alerts/read | Получение всех доступных оповещений системы безопасности. |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/delete | Удаляет политики JIT-доступа к сети. |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/initiate/action | Инициирует запрос политики JIT-доступа к сети. |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/read | Возвращает политики JIT-доступа к сети. |
> | Action | Microsoft.Security/locations/jitNetworkAccessPolicies/write | Создает новую политику JIT-доступа к сети или обновляет существующую. |
> | Action | Microsoft.Security/locations/read | Получение расположения данных безопасности. |
> | Action | Microsoft.Security/locations/tasks/activate/action | Активирует рекомендацию по безопасности. |
> | Action | Microsoft.Security/locations/tasks/dismiss/action | Закрывает рекомендацию по безопасности. |
> | Action | Microsoft.Security/locations/tasks/read | Возвращает все доступные рекомендации по безопасности. |
> | Action | Microsoft.Security/locations/tasks/resolve/action | Разрешение рекомендации по безопасности. |
> | Action | Microsoft.Security/locations/tasks/start/action | Запуск рекомендации по безопасности. |
> | Action | Microsoft.Security/policies/read | Возвращает политику безопасности. |
> | Action | Microsoft.Security/policies/write | Обновляет политику безопасности. |
> | Action | Microsoft.Security/pricings/delete | Удаление параметров ценообразования для области. |
> | Action | Microsoft.Security/pricings/read | Получение параметров ценообразования для области. |
> | Action | Microsoft.Security/pricings/write | Обновление параметров ценообразования для области. |
> | Action | Microsoft.Security/register/action | Регистрация подписки в центре безопасности Azure. |
> | Action | Microsoft.Security/securityContacts/delete | Удаление контактного лица по вопросам безопасности. |
> | Action | Microsoft.Security/securityContacts/read | Получение контактного лица по вопросам безопасности. |
> | Action | Microsoft.Security/securityContacts/write | Обновление контактного лица по вопросам безопасности. |
> | Action | Microsoft.Security/securitySolutions/delete | Удаляет решение безопасности. |
> | Action | Microsoft.Security/securitySolutions/read | Возвращает решения безопасности. |
> | Action | Microsoft.Security/securitySolutions/write | Создает новое решение безопасности или обновляет существующее. |
> | Action | Microsoft.Security/securitySolutionsReferenceData/read | Возвращает эталонные данные решений безопасности. |
> | Action | Microsoft.Security/securityStatuses/read | Возвращает состояния работоспособности ресурсов Azure. |
> | Action | Microsoft.Security/securityStatusesSummaries/read | Получение сводок состояния безопасности для области. |
> | Action | Microsoft.Security/settings/read | Получение параметров для области |
> | Action | Microsoft.Security/settings/write | Обновление параметров для области |
> | Action | Microsoft.Security/tasks/read | Возвращает все доступные рекомендации по безопасности. |
> | Action | Microsoft.Security/unregister/action | Отменяет регистрацию подписки в центре безопасности Azure |
> | Action | Microsoft.Security/webApplicationFirewalls/delete | Удаляет брандмауэр веб-приложения. |
> | Action | Microsoft.Security/webApplicationFirewalls/read | Возвращает брандмауэры веб-приложения. |
> | Action | Microsoft.Security/webApplicationFirewalls/write | Создает новый брандмауэр веб-приложения или обновляет существующий. |
> | Action | Microsoft.Security/workspaceSettings/connect/action | Изменение параметров повторного подключения к рабочей области. |
> | Action | Microsoft.Security/workspaceSettings/delete | Удаление параметров рабочей области. |
> | Action | Microsoft.Security/workspaceSettings/read | Получение параметров рабочей области. |
> | Action | Microsoft.Security/workspaceSettings/write | Обновление параметров рабочей области. |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.SecurityGraph/diagnosticsettings/delete | Удаление параметра диагностики. |
> | Action | Microsoft.SecurityGraph/diagnosticsettings/read | Чтение параметра диагностики. |
> | Action | Microsoft.SecurityGraph/diagnosticsettings/write | Запись параметра диагностики. |
> | Action | Microsoft.SecurityGraph/diagnosticsettingscategories/read | Чтение категорий параметра диагностики. |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ServiceBus/checkNameAvailability/action | Проверяет доступность пространства имен в рамках указанной подписки. |
> | Action | Microsoft.ServiceBus/checkNamespaceAvailability/action | Проверяет доступность пространства имен в рамках указанной подписки. Этот API не рекомендуется. Вместо него используйте CheckNameAvailability. |
> | Action | Microsoft.ServiceBus/locations/deleteVirtualNetworkOrSubnets/action | Удаляет правила виртуальной сети в поставщике ресурсов служебной шины для указанной виртуальной сети |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/action | Обновление правила авторизации для пространства имен. Это нерекомендуемый API. Чтобы обновить правило авторизации для пространства имен, используйте вызов PUT. Эта операция не поддерживается для API версии 2017-04-01. |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/delete | Удаляет правило авторизации для пространства имен. Невозможно удалить правило авторизации пространства имен по умолчанию.  |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/listkeys/action | Получить строку подключения к пространству имен |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/read | Получить список описаний правил авторизации пространства имен. |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/regenerateKeys/action | Повторное создание первичного или вторичного ключа в ресурсе |
> | Action | Microsoft.ServiceBus/namespaces/authorizationRules/write | Создает правила авторизации уровня пространства имен и обновляет его свойства. Можно обновить права доступа, первичный ключ и вторичный ключ для правил авторизации. |
> | Action | Microsoft.ServiceBus/namespaces/Delete | Удалить ресурс пространства имен |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/listkeys/action | Получает ключи правил авторизации для основного пространства имен аварийного восстановления |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/authorizationRules/read | Получить правила авторизации основного пространства имен аварийного восстановления |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/breakPairing/action | Отключает аварийное восстановление и останавливает репликацию изменений из основного пространства имен в дополнительное. |
> | Action | Microsoft.ServiceBus/namespaces/disasterrecoveryconfigs/checkNameAvailability/action | Проверяет доступность псевдонима пространства имен в подписке. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/delete | Удаление конфигурации аварийного восстановления, связанной с пространством имен. Эта операция может быть вызвана только через основное пространство имен. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/failover/action | Вызывает отработку отказа географического аварийного восстановления и перенастраивает псевдоним пространства имен, чтобы он указывал на дополнительное пространство. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/read | Получает конфигурацию аварийного восстановления, связанную с пространством имен. |
> | Action | Microsoft.ServiceBus/namespaces/disasterRecoveryConfigs/write | Создает или обновляет конфигурацию аварийного восстановления, связанную с пространством имен. |
> | Action | Microsoft.ServiceBus/namespaces/eventGridFilters/delete | Удаляет фильтр сетки событий, связанный с пространством имен. |
> | Action | Microsoft.ServiceBus/namespaces/eventGridFilters/read | Получает фильтр сетки событий, связанный с пространством имен. |
> | Action | Microsoft.ServiceBus/namespaces/eventGridFilters/write | Создает или обновляет фильтр сетки событий, связанный с пространством имен. |
> | Action | Microsoft.ServiceBus/namespaces/eventhubs/read | Возвращает список описаний ресурсов концентратора событий. |
> | Action | Microsoft.ServiceBus/namespaces/ipFilterRules/delete | Удалить ресурс IP-фильтра |
> | Action | Microsoft.ServiceBus/namespaces/ipFilterRules/read | Получить ресурс IP-фильтра |
> | Action | Microsoft.ServiceBus/namespaces/ipFilterRules/write | Создать ресурс фильтра IP-адресов |
> | DataAction | Microsoft. ServiceBus/пространства имен/сообщения/обзор/действие | Обзор мессжес |
> | DataAction | Microsoft. ServiceBus/Namespaces/messages/отсрочка/действие | Отложить сообщения |
> | DataAction | Microsoft. ServiceBus/пространства имен/сообщения/получение/действие | Получить сообщения |
> | DataAction | Microsoft. ServiceBus/пространства имен/сообщения/расписание/действие | Сообщения расписание |
> | DataAction | Microsoft. ServiceBus/пространства имен/сообщения/Отправка/действие | Отправить сообщения |
> | DataAction | Microsoft. ServiceBus/пространства имен/сообщения/SetState/действие | Задать состояние сеанса |
> | Action | Microsoft.ServiceBus/namespaces/messagingPlan/read | Получение плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Action | Microsoft.ServiceBus/namespaces/messagingPlan/write | Обновление плана обмена сообщениями для пространства имен.<br>Это нерекомендуемый API.<br>Свойства, предоставляемые через ресурс MessagingPlan, перемещены в ресурс (родительского) пространства имен в более поздних версиях API.<br>Эта операция не поддерживается для API версии 2017-04-01. |
> | Action | Microsoft.ServiceBus/namespaces/migrate/action | Операция переноса пространства имен |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/delete | Удаляет конфигурацию миграции. |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/read | Получает конфигурацию миграции, показывающую ее состояние и ожидающие операции репликации |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/revert/action | Отменяет миграцию из пространства имен категории "Стандартный" в пространство категории "Премиум" |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/upgrade/action | Назначает DNS, связанные с пространством имен категории "Стандартный", пространству категории "Премиум". Эта операция завершает миграцию и останавливает синхронизацию ресурсов из одного пространства в другое |
> | Action | Microsoft.ServiceBus/namespaces/migrationConfigurations/write | Создает или обновляет конфигурации миграции. Эта операция начнет синхронизацию ресурсов из пространства имен категории "Стандартный" в пространство категории "Премиум" |
> | Action | Microsoft. ServiceBus/пространства имен/нетворкрулесет/Delete | Удалить ресурс правила виртуальной сети |
> | Action | Microsoft. ServiceBus/пространства имен/нетворкрулесет/чтение | Получает ресурс Нетворкрулесет |
> | Action | Microsoft. ServiceBus/пространства имен/нетворкрулесет/запись | Создать ресурс правила виртуальной сети |
> | Action | Microsoft. ServiceBus/пространства имен/нетворкрулесетс/Delete | Удалить ресурс правила виртуальной сети |
> | Action | Microsoft. ServiceBus/пространства имен/нетворкрулесетс/чтение | Получает ресурс Нетворкрулесет |
> | Action | Microsoft. ServiceBus/пространства имен/нетворкрулесетс/запись | Создать ресурс правила виртуальной сети |
> | Action | Microsoft.ServiceBus/namespaces/operationresults/read | Получение состояния операции пространства имен |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/read | Возвращает список описаний параметров диагностики пространства имен. |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/diagnosticSettings/write | Возвращает список описаний параметров диагностики пространства имен. |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/logDefinitions/read | Возвращает список описаний ресурсов журналов пространства имен. |
> | Action | Microsoft.ServiceBus/namespaces/providers/Microsoft.Insights/metricDefinitions/read | Возвращает список описаний ресурсов метрик пространства имен. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/action | Операция обновления очереди. Эта операция не поддерживается для API версии 2017-04-01. Правила авторизации. Чтобы обновить правило авторизации, используйте вызов PUT. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/delete | Операция удаления правил авторизации очереди. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/listkeys/action | Возвращает строку подключения к очереди. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/read |  Возвращает список правил авторизации очереди. |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/regenerateKeys/action | Повторное создание первичного или вторичного ключа в ресурсе |
> | Action | Microsoft.ServiceBus/namespaces/queues/authorizationRules/write | Создает правила авторизации очереди и обновляет ее свойства. Можно обновить права доступа к правилам авторизации. |
> | Action | Microsoft.ServiceBus/namespaces/queues/Delete | Операция удаления ресурса очереди. |
> | Action | Microsoft.ServiceBus/namespaces/queues/read | Возвращает список описаний ресурсов очереди. |
> | Action | Microsoft.ServiceBus/namespaces/queues/write | Создает или обновляет свойства очереди. |
> | Action | Microsoft.ServiceBus/namespaces/read | Получить список описаний ресурса пространства имен |
> | Action | Microsoft.ServiceBus/namespaces/removeAcsNamepsace/action | Удаление пространства имен ACS |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/action | Операция обновления раздела. Эта операция не поддерживается для API версии 2017-04-01. Правила авторизации. Чтобы обновить правило авторизации, используйте вызов PUT. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/delete | Операция удаления правил авторизации раздела. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/listkeys/action | Возвращает строку подключения к разделу. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/read |  Возвращает список правил авторизации раздела. |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/regenerateKeys/action | Повторное создание первичного или вторичного ключа в ресурсе |
> | Action | Microsoft.ServiceBus/namespaces/topics/authorizationRules/write | Создает правила авторизации раздела и обновляет его свойства. Можно обновить права доступа к правилам авторизации. |
> | Action | Microsoft.ServiceBus/namespaces/topics/Delete | Операция удаления ресурса раздела. |
> | Action | Microsoft.ServiceBus/namespaces/topics/read | Возвращает список описаний ресурсов раздела. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/Delete | Операция удаления ресурса подписки на раздел. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/read | Возвращает список описаний ресурсов подписки на раздел. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/Delete | Операция удаления ресурса правила. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/read | Возвращает список описаний ресурсов правила. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/rules/write | Создает или обновляет свойства правила. |
> | Action | Microsoft.ServiceBus/namespaces/topics/subscriptions/write | Создает или обновляет свойства подписки на раздел. |
> | Action | Microsoft.ServiceBus/namespaces/topics/write | Создает или обновляет свойства раздела. |
> | Action | Microsoft.ServiceBus/namespaces/virtualNetworkRules/delete | Удалить ресурс правила виртуальной сети |
> | Action | Microsoft.ServiceBus/namespaces/virtualNetworkRules/read | Получает ресурс правила виртуальной сети |
> | Action | Microsoft.ServiceBus/namespaces/virtualNetworkRules/write | Создать ресурс правила виртуальной сети |
> | Action | Microsoft.ServiceBus/namespaces/write | Создает ресурс пространства имен и обновляет его свойства. К этим свойствам относятся теги и пространство имен. |
> | Action | Microsoft.ServiceBus/operations/read | Получить операции |
> | Action | Microsoft.ServiceBus/register/action | Регистрирует подписку для поставщика ресурсов служебной шины и разрешает создание ресурсов служебной шины |
> | Action | Microsoft.ServiceBus/sku/read | Получение списка описаний ресурсов номеров SKU. |
> | Action | Microsoft.ServiceBus/sku/regions/read | Получение списка описаний ресурсов регионов для номеров SKU. |
> | Action | Microsoft.ServiceBus/unregister/action | Регистрирует подписку для поставщика ресурсов служебной шины и разрешает создание ресурсов служебной шины |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.ServiceFabric/clusters/applications/delete | Удаление любого приложения |
> | Action | Microsoft.ServiceFabric/clusters/applications/read | Чтение любого приложения |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/delete | Удаление любой службы |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/partitions/read | Чтение любого раздела |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/partitions/replicas/read | Чтение любой реплики |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/read | Чтение любой службы |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/statuses/read | Чтение состояния любой службы |
> | Action | Microsoft.ServiceFabric/clusters/applications/services/write | Создание или изменение любой службы |
> | Action | Microsoft.ServiceFabric/clusters/applications/write | Создание или изменение любого приложения |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/delete | Удаление любого типа приложения |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/read | Считывание любого типа приложения |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/versions/delete | Удаление любой версии типа приложения |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/versions/read | Считывание любой версии типа приложения |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/versions/write | Создание или изменение любой версии типа приложения |
> | Action | Microsoft.ServiceFabric/clusters/applicationTypes/write | Создание или изменение любого типа приложения |
> | Action | Microsoft.ServiceFabric/clusters/delete | Удаление любого кластера |
> | Action | Microsoft.ServiceFabric/clusters/nodes/read | Чтение любого узла |
> | Action | Microsoft.ServiceFabric/clusters/read | Чтение любого кластера |
> | Action | Microsoft.ServiceFabric/clusters/statuses/read | Чтение состояния любого кластера |
> | Action | Microsoft.ServiceFabric/clusters/write | Создание или обновление любого кластера |
> | Action | Microsoft.ServiceFabric/locations/clusterVersions/read | Читать любую версию кластера |
> | Action | Microsoft.ServiceFabric/locations/environments/clusterVersions/read | Считывание любой версии кластера для конкретной среды |
> | Action | Microsoft.ServiceFabric/locations/operationresults/read | Считывание любых результатов операций |
> | Action | Microsoft.ServiceFabric/locations/operations/read | Считывание любых операций по расположению |
> | Action | Microsoft.ServiceFabric/operations/read | Считывание любых доступных операций |
> | Action | Microsoft.ServiceFabric/register/action | Регистрация любого действия |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.SignalRService/locations/checknameavailability/action | Проверяет, доступно ли имя для новой службы SignalR. |
> | Action | Microsoft.SignalRService/locations/operationresults/signalr/read | Запрос состояния асинхронной операции |
> | Action | Microsoft. Сигналрсервице/Locations/Оператионстатусес/идентификатор операции чтения | Запрос состояния асинхронной операции |
> | Action | Microsoft.SignalRService/locations/usages/read | Получение сведений об использовании квот для службы Azure SignalR. |
> | Action | Microsoft.SignalRService/operationresults/read | Запрос состояния асинхронной операции |
> | Action | Microsoft. Сигналрсервице/значение operationstatus/Read | Запрос состояния асинхронной операции |
> | Action | Microsoft.SignalRService/register/action | Регистрирует поставщика ресурсов Microsoft.SignalRService в подписке. |
> | Action | Microsoft.SignalRService/SignalR/delete | Удаление всей службы SignalR. |
> | Action | Microsoft. Сигналрсервице/SignalR/Евентгридфилтерс/Delete | Удаление фильтра сетки событий из SignalR. |
> | Action | Microsoft. Сигналрсервице/SignalR/Евентгридфилтерс/Read | Возвращает свойства указанного фильтра сетки событий или список всех фильтров сетки событий для указанного SignalR. |
> | Action | Microsoft. Сигналрсервице/SignalR/Евентгридфилтерс/запись | Создание или обновление фильтра сетки событий для SignalR с указанными параметрами. |
> | Action | Microsoft.SignalRService/SignalR/listkeys/action | Просмотр значений ключей доступа SignalR на портале управления или с помощью API. |
> | Action | Microsoft.SignalRService/SignalR/read | Просмотр параметров и конфигураций SignalR на портале управления или с помощью API. |
> | Action | Microsoft.SignalRService/SignalR/regeneratekey/action | Изменение значений ключей доступа SignalR на портале управления или с помощью API. |
> | Action | Microsoft.SignalRService/SignalR/restart/action | Перезапуск службы Azure SignalR на портале управления или с помощью API. Эта операция вызовет определенный период простоя. |
> | Action | Microsoft.SignalRService/SignalR/write | Изменяет параметры и конфигурации SignalR на портале управления или с помощью API. |
> | Action | Microsoft.SignalRService/unregister/action | Отменяет регистрацию поставщика ресурсов Microsoft.SignalRService в подписке. |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Solutions/Аппликатиондефинитионс/Аппликатионартифактс/Read | Список артефактов приложения определения приложения. |
> | Action | Microsoft.Solutions/applicationDefinitions/delete | Удаляет определение приложения. |
> | Action | Microsoft.Solutions/applicationDefinitions/read | Извлекает список определений приложений. |
> | Action | Microsoft.Solutions/applicationDefinitions/write | Добавление или изменение определения приложения. |
> | Action | Microsoft. Solutions/Applications/Аппликатионартифактс/Read | Список артефактов приложения. |
> | Action | Microsoft.Solutions/applications/delete | Удаляет приложение. |
> | Action | Microsoft.Solutions/applications/read | Извлекает список приложений. |
> | Action | Microsoft. Solutions/Applications/Рефрешпермиссионс/Action | Обновляет разрешения приложения. |
> | Action | Microsoft. Solutions/Applications/Упдатеакцесс/Action | Обновление доступа к приложению. |
> | Action | Microsoft.Solutions/applications/write | Создает приложение. |
> | Action | Microsoft.Solutions/jitRequests/delete | Удаление запроса JIT |
> | Action | Microsoft.Solutions/jitRequests/read | Получение списка запросов JIT |
> | Action | Microsoft.Solutions/jitRequests/write | Создание запроса JIT |
> | Action | Microsoft.Solutions/locations/operationStatuses/read | Считывает состояние операции для ресурса. |
> | Action | Microsoft.Solutions/register/action | Регистрация в решениях. |
> | Action | Microsoft. Solutions/Unregister/действие | Отменяет регистрацию в решениях. |

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Sql/checkNameAvailability/action | Проверка того, доступно ли данное имя сервера для подготовки по всему миру для данной подписки. |
> | Action | Microsoft.Sql/instancePools/delete | Удаление пула экземпляров. |
> | Action | Microsoft.Sql/instancePools/read | Получение пула экземпляров. |
> | Action | Microsoft. SQL/Инстанцепулс/использование/чтение | Получает сведения об использовании пула экземпляров |
> | Action | Microsoft.Sql/instancePools/write | Создание или обновление пула экземпляров. |
> | Action | Microsoft.Sql/locations/auditingSettingsAzureAsyncOperation/read | Извлечение результата операции настройки расширенной политики аудита больших двоичных объектов на сервере. |
> | Action | Microsoft.Sql/locations/auditingSettingsOperationResults/read | Извлекает результат операции настройки политики аудита больших двоичных объектов на сервере. |
> | Action | Microsoft.Sql/locations/capabilities/read | Получение сведений о возможностях этой подписки в данном расположении. |
> | Action | Microsoft.Sql/locations/databaseAzureAsyncOperation/read | Получение состояния операции базы данных. |
> | Action | Microsoft.Sql/locations/databaseOperationResults/read | Получение состояния операции базы данных. |
> | Action | Microsoft.Sql/locations/deletedServerAsyncOperation/read | Возвращение выполняемых операций на удаленном сервере. |
> | Action | Microsoft.Sql/locations/deletedServerOperationResults/read | Возвращение выполняемых операций на удаленном сервере. |
> | Action | Microsoft.Sql/locations/deletedServers/read | Возвращение списка удаленных серверов или получение свойств для указанного удаленного сервера. |
> | Action | Microsoft.Sql/locations/deletedServers/recover/action | Восстановление удаленного сервера. |
> | Action | Microsoft.Sql/locations/elasticPoolAzureAsyncOperation/read | Возвращение асинхронной операции Azure для эластичного пула. |
> | Action | Microsoft.Sql/locations/elasticPoolOperationResults/read | Возвращение результата операции эластичного пула. |
> | Action | Microsoft. SQL/Locations/Енкриптионпротекторазуреасинкоператион/Read | Возвращает выполняемые операции с предохранителем шифрования прозрачного шифрования данных |
> | Action | Microsoft. SQL/Locations/Енкриптионпротектороператионресултс/Read | Возвращает выполняемые операции с предохранителем шифрования прозрачного шифрования данных |
> | Action | Microsoft.Sql/locations/extendedAuditingSettingsAzureAsyncOperation/read | Извлечение результата операции настройки расширенной политики аудита больших двоичных объектов на сервере. |
> | Action | Microsoft.Sql/locations/extendedAuditingSettingsOperationResults/read | Извлечение результата операции настройки расширенной политики аудита больших двоичных объектов на сервере. |
> | Action | Microsoft.Sql/locations/firewallRulesAzureAsyncOperation/read | Получение состояния операции правила брандмауэра. |
> | Action | Microsoft.Sql/locations/firewallRulesOperationResults/read | Получение состояния операции правила брандмауэра. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/delete | Удаление имеющейся группы отработки отказа экземпляра. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/failover/action | Выполнение плановой отработки отказа в имеющейся группе отработки отказа экземпляра. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/forceFailoverAllowDataLoss/action | Выполнение принудительной отработки отказа в имеющейся группе отработки отказа экземпляра. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/read | Возвращение списка групп отработки отказа экземпляра или свойств для указанной группы экземпляра. |
> | Action | Microsoft.Sql/locations/instanceFailoverGroups/write | Создает группу отработки отказа экземпляра с указанными параметрами или обновляет свойства или теги для указанной группы отработки отказа. |
> | Action | Microsoft.Sql/locations/instancePoolAzureAsyncOperation/read | Возвращает состояние операции пула экземпляров. |
> | Action | Microsoft.Sql/locations/instancePoolOperationResults/read | Возвращает результат для операции с пулом экземпляров |
> | Action | Microsoft.Sql/locations/interfaceEndpointProfileAzureAsyncOperation/read | Возвращение сведений об асинхронной операции Azure для определенной конечной точки интерфейса |
> | Action | Microsoft.Sql/locations/interfaceEndpointProfileOperationResults/read | Возвращение сведений об операции с профилем для определенной конечной точки интерфейса |
> | Action | Microsoft.Sql/locations/jobAgentAzureAsyncOperation/read | Получает состояние операции агента задания. |
> | Action | Microsoft.Sql/locations/jobAgentOperationResults/read | Получает результат операции агента задания. |
> | Action | Microsoft.Sql/locations/longTermRetentionBackups/read | Вывод списка резервных копий на долгосрочном хранении для каждой базы данных на каждом сервере в определенном расположении. |
> | Action | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionBackups/read | Вывод списка резервных копий на долгосрочном хранении для каждой базы данных на сервере. |
> | Action | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/delete | Удаление резервной копии на долгосрочном хранении. |
> | Action | Microsoft.Sql/locations/longTermRetentionServers/longTermRetentionDatabases/longTermRetentionBackups/read | Вывод списка резервных копий на долгосрочном хранении для базы данных. |
> | Action | Microsoft.Sql/locations/managedDatabaseRestoreAzureAsyncOperation/completeRestore/action | Завершение операции восстановления управляемой базы данных. |
> | Action | Microsoft. SQL/Locations/Манажединстанцеенкриптионпротекторазуреасинкоператион/Read | Получает выполняемые операции с прозрачным шифрованием шифрования управляемого шифрования данных |
> | Action | Microsoft. SQL/Locations/Манажединстанцеенкриптионпротектороператионресултс/Read | Получает выполняемые операции с прозрачным шифрованием шифрования управляемого шифрования данных |
> | Action | Microsoft. SQL/Locations/Манажединстанцекэйазуреасинкоператион/Read | Возвращает выполняемые операции с управляемыми ключами экземпляра шифрования прозрачных данных |
> | Action | Microsoft. SQL/Locations/Манажединстанцекэйоператионресултс/Read | Возвращает выполняемые операции с управляемыми ключами экземпляра шифрования прозрачных данных |
> | Action | Microsoft.Sql/locations/managedTransparentDataEncryptionAzureAsyncOperation/read | Возвращение выполняемых операций прозрачного шифрования данных в управляемой базе данных. |
> | Action | Microsoft.Sql/locations/managedTransparentDataEncryptionOperationResults/read | Возвращение выполняемых операций прозрачного шифрования данных в управляемой базе данных. |
> | Action | Microsoft. SQL/Locations/Приватиндпоинтконнектионазуреасинкоператион/Read | Возвращает результат операции подключения к частной конечной точке |
> | Action | Microsoft. SQL/Locations/Приватиндпоинтконнектионоператионресултс/Read | Возвращает результат операции подключения к частной конечной точке |
> | Action | Microsoft. SQL/Locations/Приватиндпоинтконнектионпроксязуреасинкоператион/Read | Возвращает результат для операции прокси-сервера подключения частной конечной точки |
> | Action | Microsoft. SQL/Locations/Приватиндпоинтконнектионпроксйоператионресултс/Read | Возвращает результат для операции прокси-сервера подключения частной конечной точки |
> | Action | Microsoft.Sql/locations/read | Получение расположений, доступных для данной подписки. |
> | Action | Microsoft. SQL/Locations/Серверкэйазуреасинкоператион/Read | Возвращает выполняемые операции с ключами сервера прозрачного шифрования данных |
> | Action | Microsoft. SQL/Locations/Серверкэйоператионресултс/Read | Возвращает выполняемые операции с ключами сервера прозрачного шифрования данных |
> | Action | Microsoft.Sql/locations/syncAgentOperationResults/read | Получение результата операции ресурса агента синхронизации. |
> | Action | Microsoft.Sql/locations/syncDatabaseIds/read | Получение идентификаторов базы данных синхронизации для конкретного региона и подписки. |
> | Action | Microsoft.Sql/locations/syncGroupOperationResults/read | Получение результата операции синхронизации группы. |
> | Action | Microsoft.Sql/locations/syncMemberOperationResults/read | Получение результата операции синхронизации элемента. |
> | Action | Microsoft.Sql/locations/usages/read | Получение коллекции метрик использования для этой подписки в расположении. |
> | Action | Microsoft.Sql/locations/virtualNetworkRulesAzureAsyncOperation/read | Возвращение сведений об асинхронной операции Azure для указанных правил виртуальной сети.  |
> | Action | Microsoft.Sql/locations/virtualNetworkRulesOperationResults/read | Возвращение сведений о правилах операции для указанных правил виртуальной сети.  |
> | Action | Microsoft.Sql/managedInstances/administrators/delete | Удаление существующего администратора управляемого экземпляра. |
> | Action | Microsoft.Sql/managedInstances/administrators/read | Получение списка администраторов управляемого экземпляра. |
> | Action | Microsoft.Sql/managedInstances/administrators/write | Создание или обновление администратора управляемого экземпляра с указанными параметрами. |
> | Action | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/read | Получает политику краткосрочного хранения для управляемой базы данных |
> | Action | Microsoft.Sql/managedInstances/databases/backupShortTermRetentionPolicies/write | Обновляет политику краткосрочного хранения для управляемой базы данных |
> | Action | Microsoft.Sql/managedInstances/databases/currentSensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Action | Microsoft. SQL/Манажединстанцес/databases/Куррентсенситивитилабелс/Write | Метки чувствительности пакетного обновления |
> | Action | Microsoft.Sql/managedInstances/databases/delete | Удаление существующей управляемой базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/read | Получает настройку диагностики для ресурса |
> | Action | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.Sql/managedInstances/databases/providers/Microsoft.Insights/logDefinitions/read | Получение доступных журналов для баз данных управляемых экземпляров |
> | Action | Microsoft.Sql/managedInstances/databases/read | Получение существующей управляемой базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/recommendedSensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Action | Microsoft. SQL/Манажединстанцес/базы данных/схемы/чтение | Получение схемы управляемой базы данных. |
> | Action | Microsoft. SQL/Манажединстанцес/базы данных, схемы, таблицы, столбцы и чтение | Получение столбца управляемой базы данных |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/delete | Удаление метки конфиденциальности данного столбца. |
> | Action | Microsoft. SQL/Манажединстанцес/базы данных/схемы/таблицы/столбцы/Сенситивитилабелс/отключение/действие | Отключить рекомендации по конфиденциальности для данного столбца |
> | Action | Microsoft. SQL/Манажединстанцес/базы данных/схемы/таблицы/столбцы/Сенситивитилабелс/включение/действие | Включить рекомендации по конфиденциальности для данного столбца |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/read | Получение метки конфиденциальности данного столбца. |
> | Action | Microsoft.Sql/managedInstances/databases/schemas/tables/columns/sensitivityLabels/write | Создание или обновление метки конфиденциальности данного столбца. |
> | Action | Microsoft. SQL/Манажединстанцес/базы данных/схемы/таблицы/чтение | Получение таблицы управляемой базы данных |
> | Action | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/read | Получение списка политик обнаружения угроз управляемой базы данных, настроенных для данного сервера |
> | Action | Microsoft.Sql/managedInstances/databases/securityAlertPolicies/write | Изменение политики обнаружения угроз для заданной управляемой базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/securityEvents/read | Получение событий безопасности управляемой базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/sensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/read | Получение сведений о прозрачном шифровании данных в данной управляемой базе данных. |
> | Action | Microsoft.Sql/managedInstances/databases/transparentDataEncryption/write | Изменение прозрачного шифрования данных в данной управляемой базе данных. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/delete | Удаление оценки уязвимости указанной базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/read | Получение политик оценки уязвимости для конкретной базы данных |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/delete | Удаление базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/read | Получение базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/rules/baselines/write | Изменение базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/export/action | Преобразование существующего результата проверки в удобочитаемый формат. Если он уже существует, ничего не происходит. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/initiateScan/action | Выполняет поиск уязвимостей базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/scans/read | Возвращение списка записей сканирования оценки уязвимости базы данных или получение записи для указанного идентификатора сканирования. |
> | Action | Microsoft.Sql/managedInstances/databases/vulnerabilityAssessments/write | Изменение оценки уязвимости указанной базы данных. |
> | Action | Microsoft.Sql/managedInstances/databases/write | Создает новую базу данных или обновляет имеющуюся. |
> | Action | Microsoft.Sql/managedInstances/delete | Удаление существующего управляемого экземпляра. |
> | Action | Microsoft.Sql/managedInstances/encryptionProtector/read | Возвращение списка предохранителей шифрования сервера или получение свойств для указанного предохранителя. |
> | Action | Microsoft.Sql/managedInstances/encryptionProtector/write | Обновление свойств указанного предохранителя шифрования сервера. |
> | Action | Microsoft.Sql/managedInstances/keys/delete | Удаляет имеющийся ключ Управляемого экземпляра SQL Azure. |
> | Action | Microsoft.Sql/managedInstances/keys/read | Возвращает список ключей управляемых экземпляров или получает свойства указанного ключа управляемого экземпляра. |
> | Action | Microsoft.Sql/managedInstances/keys/write | Создает ключ с указанными параметрами или обновляет свойства или теги указанного ключа управляемого экземпляра. |
> | Action | Microsoft.Sql/managedInstances/metricDefinitions/read | Получение определений метрик управляемого экземпляра. |
> | Action | Microsoft.Sql/managedInstances/metrics/read | Получение метрик управляемого экземпляра. |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/read | Получает настройку диагностики для ресурса |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/logDefinitions/read | Получение доступных журналов для управляемых экземпляров |
> | Action | Microsoft.Sql/managedInstances/providers/Microsoft.Insights/metricDefinitions/read | Возвращение типов метрик, доступных для управляемых экземпляров |
> | Action | Microsoft.Sql/managedInstances/read | Возвращение списка управляемых экземпляров или получение свойств указанного управляемого экземпляра. |
> | Action | Microsoft.Sql/managedInstances/recoverableDatabases/read | Возвращает список доступных для восстановления управляемых баз данных. |
> | Action | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/read | Получает политику краткосрочного хранения для удаленной управляемой базы данных |
> | Action | Microsoft.Sql/managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies/write | Обновляет политику краткосрочного хранения для удаленной управляемой базы данных |
> | Action | Microsoft.Sql/managedInstances/restorableDroppedDatabases/read | Возвращает список доступных для восстановления удаленных управляемых баз данных. |
> | Action | Microsoft.Sql/managedInstances/securityAlertPolicies/read | Получение списка политик обнаружения угроз для управляемого сервера, настроенных для данного сервера |
> | Action | Microsoft.Sql/managedInstances/securityAlertPolicies/write | Изменение политики обнаружения угроз на данном управляемом сервере. |
> | Action | Microsoft.Sql/managedInstances/tdeCertificates/action | Создание или обновление сертификата TDE |
> | Action | Microsoft.Sql/managedInstances/vulnerabilityAssessments/delete | Удаление оценки уязвимости для указанного управляемого экземпляра |
> | Action | Microsoft.Sql/managedInstances/vulnerabilityAssessments/read | Получение политик оценки уязвимости для конкретного управляемого экземпляра |
> | Action | Microsoft.Sql/managedInstances/vulnerabilityAssessments/write | Изменение оценки уязвимости для указанного управляемого экземпляра |
> | Action | Microsoft.Sql/managedInstances/write | Создание управляемого экземпляра с указанными параметрами либо обновление свойств или тегов указанного управляемого экземпляра. |
> | Action | Microsoft.Sql/operations/read | Получение доступных операций REST. |
> | Action | Microsoft. SQL/Приватиндпоинтконнектионсаппровал/действие | Определяет, разрешено ли пользователю утверждать подключение частной конечной точки |
> | Action | Microsoft.Sql/register/action | Регистрация подписки для поставщика ресурсов базы данных SQL Azure и разрешение создания баз данных Microsoft SQL. |
> | Action | Microsoft.Sql/servers/administratorOperationResults/read | Возвращение выполняемых операций администраторами серверов. |
> | Action | Microsoft.Sql/servers/administrators/delete | Удаление администратора сервера. |
> | Action | Microsoft.Sql/servers/administrators/read | Извлекает сведения об администраторе сервера. |
> | Action | Microsoft.Sql/servers/administrators/write | Создает или обновляет учетные данные администратора сервера. |
> | Action | Microsoft.Sql/servers/advisors/read | Возвращает список доступных помощников для сервера. |
> | Action | Microsoft.Sql/servers/advisors/recommendedActions/read | Возвращает список рекомендуемых действий указанного помощника для сервера. |
> | Action | Microsoft.Sql/servers/advisors/recommendedActions/write | Применяет рекомендуемое действие к серверу. |
> | Action | Microsoft.Sql/servers/advisors/write | Обновляет состояние автовыполнения Помощника на уровне сервера. |
> | Action | Microsoft.Sql/servers/auditingPolicies/read | Извлекает сведения о политике аудита таблиц по умолчанию, настроенной на заданном сервере. |
> | Action | Microsoft.Sql/servers/auditingPolicies/write | Изменяет политику аудита таблиц по умолчанию для заданного сервера. |
> | Action | Microsoft.Sql/servers/auditingSettings/operationResults/read | Извлекает результат операции настройки политики аудита больших двоичных объектов на сервере. |
> | Action | Microsoft.Sql/servers/auditingSettings/read | Извлекает сведения о политике аудита больших двоичных объектов, настроенной на заданном сервере. |
> | Action | Microsoft.Sql/servers/auditingSettings/write | Изменяет политику аудита больших двоичных объектов для заданного сервера. |
> | Action | Microsoft.Sql/servers/automaticTuning/read | Возвращение параметров автоматической настройки для сервера. |
> | Action | Microsoft.Sql/servers/automaticTuning/write | Обновление параметров автоматической настройки для сервера и возвращение обновленных параметров. |
> | Action | Microsoft.Sql/servers/backupLongTermRetentionVaults/delete | Удаление существующего архивного хранилища резервных копий. |
> | Action | Microsoft.Sql/servers/backupLongTermRetentionVaults/read | С помощью этой операции можно получить хранилище резервных копий долгосрочного хранения. Возвращает сведения о хранилище, зарегистрированном на данном сервере. |
> | Action | Microsoft.Sql/servers/backupLongTermRetentionVaults/write | С помощью этой операции можно зарегистрировать на сервере хранилище резервных копий долгосрочного хранения. |
> | Action | Microsoft.Sql/servers/communicationLinks/delete | Удаление существующего подключения сервера. |
> | Action | Microsoft.Sql/servers/communicationLinks/read | Возвращение списка подключений для указанного сервера. |
> | Action | Microsoft.Sql/servers/communicationLinks/write | Создание или обновление подключения сервера. |
> | Action | Microsoft.Sql/servers/connectionPolicies/read | Возвращение списка политик подключений для указанного сервера. |
> | Action | Microsoft.Sql/servers/connectionPolicies/write | Создание или обновление политики подключения сервера. |
> | Action | Microsoft.Sql/servers/databases/advisors/read | Возвращает список доступных помощников для базы данных. |
> | Action | Microsoft.Sql/servers/databases/advisors/recommendedActions/read | Возвращает список рекомендуемых действий указанного помощника для базы данных. |
> | Action | Microsoft.Sql/servers/databases/advisors/recommendedActions/write | Применяет рекомендуемое действие к базе данных. |
> | Action | Microsoft.Sql/servers/databases/advisors/write | Обновляет состояние автовыполнения Помощника на уровне базы данных. |
> | Action | Microsoft.Sql/servers/databases/auditingPolicies/read | Извлекает сведения о политике аудита таблиц, настроенной для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/auditingPolicies/write | Изменяет политику аудита таблиц для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/auditingSettings/read | Извлекает сведения о политике аудита больших двоичных объектов, настроенной для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/auditingSettings/write | Изменяет политику аудита больших двоичных объектов для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/auditRecords/read | Извлекает записи аудита больших двоичных объектов для базы данных. |
> | Action | Microsoft.Sql/servers/databases/automaticTuning/read | Возвращение параметров автоматической настройки базы данных. |
> | Action | Microsoft.Sql/servers/databases/automaticTuning/write | Обновление параметров автоматической настройки базы данных и возвращение обновленных параметров. |
> | Action | Microsoft.Sql/servers/databases/azureAsyncOperation/read | Получение состояния операции базы данных. |
> | Action | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/read | Возвращение списка политик хранения резервных копий для указанной базы данных. |
> | Action | Microsoft.Sql/servers/databases/backupLongTermRetentionPolicies/write | Создание или обновление политики хранения резервных копий базы данных. |
> | Action | Microsoft. SQL/Servers/databases/БаккупшорттермретентионполиЦиес/Read | Возвращает политику краткосрочного хранения для базы данных. |
> | Action | Microsoft. SQL/Servers/databases/БаккупшорттермретентионполиЦиес/Write | Обновляет политику краткосрочного хранения для базы данных. |
> | Action | Microsoft.Sql/servers/databases/connectionPolicies/read | Извлекает сведения о политике подключений, настроенной для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/connectionPolicies/write | Изменяет политику подключений для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/currentSensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Action | Microsoft. SQL/Servers/databases/Куррентсенситивитилабелс/Write | Метки чувствительности пакетного обновления |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/read | Получение списка политик маскировки данных базы данных. |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/delete | Удаление правила политики маскировки данных для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/read | Извлекает сведения о правиле политики маскирования данных, настроенной для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/rules/write | Изменяет правило политики маскирования данных для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/dataMaskingPolicies/write | Изменяет политику маскирования данных для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/dataWarehouseQueries/dataWarehouseQuerySteps/read | Возвращает сведения о шаге распределенного запроса к хранилищу данных для выбранного идентификатора шага. |
> | Action | Microsoft.Sql/servers/databases/dataWarehouseQueries/read | Возвращает данные запроса на распределение к хранилищу данных для выбранного идентификатора запроса. |
> | Action | Microsoft.Sql/servers/databases/dataWarehouseUserActivities/read | Получение действий пользователя в экземпляре хранилища данных SQL, включая выполняемые и приостановленные запросы. |
> | Action | Microsoft.Sql/servers/databases/delete | Удаление имеющейся базы данных. |
> | Action | Microsoft.Sql/servers/databases/export/action | Экспорт базы данных SQL Azure. |
> | Action | Microsoft.Sql/servers/databases/extendedAuditingSettings/read | Извлечение сведений о расширенной политике аудита больших двоичных объектов, настроенной для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/extendedAuditingSettings/write | Изменение расширенной политики аудита больших двоичных объектов для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/extensions/read | Получение коллекции расширений для базы данных. |
> | Action | Microsoft.Sql/servers/databases/extensions/write | Изменение расширения для конкретной базы данных. |
> | Action | Microsoft. SQL/серверы/базы данных/отработка отказа/действие | Отработка отказа базы данных, инициированная клиентом. |
> | Action | Microsoft.Sql/servers/databases/geoBackupPolicies/read | Получение политик геоизбыточного резервного копирования для конкретной базы данных. |
> | Action | Microsoft.Sql/servers/databases/geoBackupPolicies/write | Создание или обновление политики геоизбыточного резервного копирования базы данных. |
> | Action | Microsoft.Sql/servers/databases/importExportOperationResults/read | Получение выполняемых операций импорта и экспорта. |
> | Action | Microsoft.Sql/servers/databases/maintenanceWindowOptions/read | Получает список доступных окон обслуживания для выбранной базы данных. |
> | Action | Microsoft.Sql/servers/databases/maintenanceWindows/read | Получает параметры окон обслуживания для выбранной базы данных. |
> | Action | Microsoft.Sql/servers/databases/maintenanceWindows/write | Задает параметры окон обслуживания для выбранной базы данных. |
> | Action | Microsoft.Sql/servers/databases/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Action | Microsoft.Sql/servers/databases/metrics/read | Получение метрик для баз данных. |
> | Action | Microsoft.Sql/servers/databases/move/action | Изменение имени существующей базы данных. |
> | Action | Microsoft.Sql/servers/databases/operationResults/read | Получение состояния операции базы данных. |
> | Action | Microsoft.Sql/servers/databases/operations/cancel/action | Отмена приостановленной асинхронной операции базы данных SQL Azure, которая еще не завершена. |
> | Action | Microsoft.Sql/servers/databases/operations/read | Возвращение списка операций, выполняемых в базе данных. |
> | Action | Microsoft.Sql/servers/databases/pause/action | Приостановка работы базы данных хранилища данных SQL Azure. |
> | Action | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/read | Получает настройку диагностики для ресурса |
> | Action | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/logDefinitions/read | Возвращает доступные журналы для баз данных. |
> | Action | Microsoft.Sql/servers/databases/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для баз данных. |
> | Action | Microsoft.Sql/servers/databases/queryStore/queryTexts/read | Возвращение коллекции текстов запросов, которые соответствуют заданным параметрам. |
> | Action | Microsoft.Sql/servers/databases/queryStore/read | Возвращение текущих значений параметров хранилища запросов для базы данных. |
> | Action | Microsoft.Sql/servers/databases/queryStore/write | Обновляет параметр хранилища запросов для базы данных. |
> | Action | Microsoft.Sql/servers/databases/read | Возвращение списка баз данных или возвращение свойств указанной базы данных. |
> | Action | Microsoft.Sql/servers/databases/recommendedSensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/delete | Принудительно удаляет отношение репликации. При этом возможна потеря данных. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/failover/action | Выполнение отработки отказа после синхронизации всех изменений из базы данных-источника, превращая заданную базу данных в базу данных-источник для отношения репликации, а удаленную базу данных-источник — в базу данных-приемник. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/forceFailoverAllowDataLoss/action | Выполнение немедленной отработки отказа с потенциальной потерей данных, превращая заданную базу данных в базу данных-источник для отношения репликации, а удаленную базу данных-источник — в базу данных-приемник. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/read | Возвращает список связей репликации или получает свойства для указанных связей репликации. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/unlink/action | Удаляет отношение репликации в принудительном порядке или после синхронизации с партнером. |
> | Action | Microsoft.Sql/servers/databases/replicationLinks/updateReplicationMode/action | Обновляет режим репликации для ссылки на синхронный или асинхронный режим. |
> | Action | Microsoft.Sql/servers/databases/restorePoints/action | Создает точку восстановления |
> | Action | Microsoft.Sql/servers/databases/restorePoints/delete | Удаляет точку восстановления для базы данных. |
> | Action | Microsoft.Sql/servers/databases/restorePoints/read | Возвращение списка точек восстановления для базы данных. |
> | Action | Microsoft.Sql/servers/databases/resume/action | Возобновление работы базы данных хранилища данных SQL Azure. |
> | Action | Microsoft.Sql/servers/databases/schemas/read | Получение схемы базы данных. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/read | Получение столбца базы данных. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/delete | Удаление метки конфиденциальности данного столбца. |
> | Action | Microsoft. SQL/серверы/базы данных, схемы, таблицы, столбцы, Сенситивитилабелс, отключение и действие | Отключить рекомендации по конфиденциальности для данного столбца |
> | Action | Microsoft. SQL/серверы/базы данных/схемы/таблицы/столбцы/Сенситивитилабелс/включение/действие | Включить рекомендации по конфиденциальности для данного столбца |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/read | Получение метки конфиденциальности данного столбца. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/columns/sensitivityLabels/write | Создание или обновление метки конфиденциальности данного столбца. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/read | Получение таблицы базы данных. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/read | Извлекает список рекомендаций по индексам для базы данных. |
> | Action | Microsoft.Sql/servers/databases/schemas/tables/recommendedIndexes/write | Применяет рекомендацию по индексу. |
> | Action | Microsoft.Sql/servers/databases/securityAlertPolicies/read | Получение списка политик обнаружения угроз базы данных, настроенных для данного сервера |
> | Action | Microsoft.Sql/servers/databases/securityAlertPolicies/write | Изменение политики обнаружения угроз для конкретной базы данных |
> | Action | Microsoft.Sql/servers/databases/securityMetrics/read | Возвращение коллекции метрик безопасности базы данных. |
> | Action | Microsoft.Sql/servers/databases/sensitivityLabels/read | Получение списка меток конфиденциальности для конкретной базы данных. |
> | Action | Microsoft.Sql/servers/databases/serviceTierAdvisors/read | Возвращает предложение по масштабированию базы данных на основе статистики выполнения запросов для повышения производительности или снижения затрат. |
> | Action | Microsoft.Sql/servers/databases/skus/read | Возвращение коллекции доступных номеров SKU для базы данных. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/cancelSync/action | Отмена синхронизации группы синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/delete | Удаление существующей группы синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/hubSchemas/read | Возвращение списка схем баз данных концентратора синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/logs/read | Возвращение списка журналов группы синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/read | Возвращение списка групп синхронизации или свойств для указанной группы. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchema/action | Обновление схемы базы данных концентратора синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/refreshHubSchemaOperationResults/read | Получение результата операции обновления схемы концентратора синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/delete | Удаление существующего элемента синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/read | Возвращение списка элементов синхронизации или свойств для указанного элемента. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchema/action | Обновление схемы элемента синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/refreshSchemaOperationResults/read | Получение результата операции обновления схемы элемента синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/schemas/read | Возвращение списка схем баз данных элемента синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/syncMembers/write | Создание элемента синхронизации с указанными параметрами либо обновление свойств указанного элемента. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/triggerSync/action | Активация синхронизации группы синхронизации. |
> | Action | Microsoft.Sql/servers/databases/syncGroups/write | Создание группы синхронизации с указанными параметрами либо обновление свойств указанной группы. |
> | Action | Microsoft.Sql/servers/databases/topQueries/queryText/action | Возвращает текст Transact-SQL для выбранного идентификатора запроса. |
> | Action | Microsoft.Sql/servers/databases/topQueries/read | Возвращает статистические данные среды выполнения для выбранного запроса в указанный временной период. |
> | Action | Microsoft.Sql/servers/databases/topQueries/statistics/read | Возвращает статистические данные среды выполнения для выбранного запроса в указанный временной период. |
> | Action | Microsoft.Sql/servers/databases/transparentDataEncryption/operationResults/read | Возвращение выполняемых операций прозрачного шифрования данных. |
> | Action | Microsoft.Sql/servers/databases/transparentDataEncryption/read | Извлекает состояние и сведения о функции безопасности "прозрачное шифрование данных" для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/transparentDataEncryption/write | Изменение состояния прозрачного шифрования данных. |
> | Action | Microsoft.Sql/servers/databases/upgradeDataWarehouse/action | Обновление работы базы данных хранилища данных SQL Azure. |
> | Action | Microsoft.Sql/servers/databases/usages/read | Возвращение сведений об использовании Базы данных SQL Azure. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/delete | Удаление оценки уязвимости указанной базы данных. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/read | Получение политик оценки уязвимости для конкретной базы данных |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/delete | Удаление базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/read | Получение базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/rules/baselines/write | Изменение базовых показателей правила оценки уязвимости для указанной базы данных. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/export/action | Преобразование существующего результата проверки в удобочитаемый формат. Если он уже существует, ничего не происходит. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/initiateScan/action | Выполняет поиск уязвимостей базы данных. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/scans/read | Возвращение списка записей сканирования оценки уязвимости базы данных или получение записи для указанного идентификатора сканирования. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessments/write | Изменение оценки уязвимости указанной базы данных. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/action | Выполняет поиск уязвимостей базы данных. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentScans/operationResults/read | Получение результата выполнения сканирования оценки уязвимости базы данных. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/read | Извлечение сведений об оценке уязвимости, настроенной для заданной базы данных. |
> | Action | Microsoft.Sql/servers/databases/vulnerabilityAssessmentSettings/write | Изменение оценки уязвимости указанной базы данных. |
> | Action | Microsoft.Sql/servers/databases/write | Создание базы данных с указанными параметрами либо обновление свойств или тегов для указанной базы данных. |
> | Action | Microsoft.Sql/servers/delete | Удаление существующего сервера. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/delete | Удаление существующих конфигураций аварийного восстановления для указанного сервера. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/failover/action | Отработка отказа конфигурации аварийного восстановления. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/forceFailoverAllowDataLoss/action | Принудительная отработка отказа конфигурации аварийного восстановления. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/read | Получение коллекции конфигураций аварийного восстановления, в которых содержится данный сервер. |
> | Action | Microsoft.Sql/servers/disasterRecoveryConfiguration/write | Изменение конфигурации аварийного восстановления сервера. |
> | Action | Microsoft.Sql/servers/elasticPoolEstimates/read | Возвращает список оценок эластичных пулов, уже созданных на этом сервере. |
> | Action | Microsoft.Sql/servers/elasticPoolEstimates/write | Создает оценку эластичного пула для указанных баз данных. |
> | Action | Microsoft.Sql/servers/elasticPools/advisors/read | Возвращает список доступных помощников для эластичного пула. |
> | Action | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/read | Возвращает список рекомендуемых действий указанного помощника для эластичного пула. |
> | Action | Microsoft.Sql/servers/elasticPools/advisors/recommendedActions/write | Применяет рекомендуемое действие к эластичному пулу. |
> | Action | Microsoft.Sql/servers/elasticPools/advisors/write | Обновляет состояние автовыполнения Помощника на уровне эластичного пула. |
> | Action | Microsoft.Sql/servers/elasticPools/databases/read | Получение списка баз данных в эластичном пуле. |
> | Action | Microsoft.Sql/servers/elasticPools/delete | Удаление имеющегося эластичного пула. |
> | Action | Microsoft.Sql/servers/elasticPools/elasticPoolActivity/read | Извлекает действия и сведения о заданном пуле эластичных баз данных. |
> | Action | Microsoft.Sql/servers/elasticPools/elasticPoolDatabaseActivity/read | Извлекает действия заданной базы данных, входящей в пул эластичных баз данных, а также сведения о ней. |
> | Action | Microsoft. SQL/Servers/Еластикпулс/отработка отказа/действие | Клиент инициировал отработку отказа эластичного пула. |
> | Action | Microsoft.Sql/servers/elasticPools/metricDefinitions/read | Возвращает типы метрик, доступных для пулов эластичных баз данных. |
> | Action | Microsoft.Sql/servers/elasticPools/metrics/read | Возвращение метрик для пулов эластичных баз данных. |
> | Action | Microsoft.Sql/servers/elasticPools/operations/cancel/action | Отмена приостановленной асинхронной операции эластичного пула SQL Azure, которая еще не завершена. |
> | Action | Microsoft.Sql/servers/elasticPools/operations/read | Возвращение списка операций, выполняемых в эластичном пуле. |
> | Action | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/read | Получает настройку диагностики для ресурса |
> | Action | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/diagnosticSettings/write | Создает или обновляет настройку диагностики для ресурса |
> | Action | Microsoft.Sql/servers/elasticPools/providers/Microsoft.Insights/metricDefinitions/read | Возвращает типы метрик, доступных для пулов эластичных баз данных. |
> | Action | Microsoft.Sql/servers/elasticPools/read | Извлечение сведений о пуле эластичных баз данных для заданного сервера. |
> | Action | Microsoft.Sql/servers/elasticPools/skus/read | Возвращение коллекции доступных номеров SKU для пула эластичных баз данных. |
> | Action | Microsoft.Sql/servers/elasticPools/write | Создание эластичного пула или изменение свойств существующего. |
> | Action | Microsoft.Sql/servers/encryptionProtector/read | Возвращение списка предохранителей шифрования сервера или получение свойств для указанного предохранителя. |
> | Action | Microsoft.Sql/servers/encryptionProtector/write | Обновление свойств указанного предохранителя шифрования сервера. |
> | Action | Microsoft.Sql/servers/extendedAuditingSettings/read | Извлечение сведений о расширенной политике аудита больших двоичных объектов, настроенной на заданном сервере. |
> | Action | Microsoft.Sql/servers/extendedAuditingSettings/write | Изменение расширенного аудита больших двоичных объектов для заданного сервера. |
> | Action | Microsoft.Sql/servers/failoverGroups/delete | Удаление существующей группы отработки отказа. |
> | Action | Microsoft.Sql/servers/failoverGroups/failover/action | Выполнение плановой отработки отказа в существующей группе отработки отказа. |
> | Action | Microsoft.Sql/servers/failoverGroups/forceFailoverAllowDataLoss/action | Выполнение принудительной отработки отказа в существующей группе отработки отказа. |
> | Action | Microsoft.Sql/servers/failoverGroups/read | Возвращение списка групп отработки отказа или свойств для указанной группы. |
> | Action | Microsoft.Sql/servers/failoverGroups/write | Создание группы отработки отказа с указанными параметрами либо обновление свойств или тегов указанной группы. |
> | Action | Microsoft.Sql/servers/firewallRules/delete | Удаление существующего правила брандмауэра сервера. |
> | Action | Microsoft.Sql/servers/firewallRules/read | Возвращение списка правил брандмауэра сервера или получение свойства для указанного правила. |
> | Action | Microsoft.Sql/servers/firewallRules/write | Создание правила брандмауэра сервера с указанными параметрами, обновление свойств указанного правила или перезапись всех существующих правил новыми правилами брандмауэра сервера. |
> | Action | Microsoft.Sql/servers/import/action | Создает базу данных на сервере и развертывает схему и данные из пакета DACPAC. |
> | Action | Microsoft.Sql/servers/importExportOperationResults/read | Получение выполняемых операций импорта и экспорта. |
> | Action | Microsoft.Sql/servers/interfaceEndpointProfiles/delete | Удаляет профиль определенной конечной точки интерфейса |
> | Action | Microsoft.Sql/servers/interfaceEndpointProfiles/read | Возвращает свойства для профиля указанной конечной точки интерфейса |
> | Action | Microsoft.Sql/servers/interfaceEndpointProfiles/write | Создает профиль конечной точки интерфейса с указанными параметрами либо обновляет свойства или теги указанной конечной точки интерфейса |
> | Action | Microsoft.Sql/servers/jobAgents/delete | Удаляет агент задания базы данных SQL Azure |
> | Action | Microsoft.Sql/servers/jobAgents/read | Получает агент задания базы данных SQL Azure |
> | Action | Microsoft.Sql/servers/jobAgents/write | Создает или изменяет агент задания базы данных SQL Azure |
> | Action | Microsoft.Sql/servers/keys/delete | Удаление существующего ключа сервера. |
> | Action | Microsoft.Sql/servers/keys/read | Возвращение списка ключей сервера или возвращение свойств указанного ключа сервера. |
> | Action | Microsoft.Sql/servers/keys/write | Создание ключа с указанными параметрами либо обновление свойств или тегов указанного ключа сервера. |
> | Action | Microsoft.Sql/servers/operationResults/read | Возвращение выполняемых операций сервера. |
> | Action | Microsoft. SQL/Servers/Приватиндпоинтконнектионпроксиес/Delete | Удаляет существующий прокси-сервер подключения частной конечной точки. |
> | Action | Microsoft. SQL/Servers/Приватиндпоинтконнектионпроксиес/Read | Возвращает список прокси-серверов подключения частной конечной точки или получает свойства для указанного прокси подключения к частной конечной точке. |
> | Action | Microsoft. SQL/Servers/Приватиндпоинтконнектионпроксиес/проверка/действие | Проверяет подключение к частной конечной точке создать вызов с NRP стороны |
> | Action | Microsoft. SQL/Servers/Приватиндпоинтконнектионпроксиес/Write | Создает прокси-сервер подключения частной конечной точки с указанными параметрами или обновляет свойства или теги для указанного прокси-сервера подключения частной конечной точки. |
> | Action | Microsoft. SQL/Servers/Приватиндпоинтконнектионс/Delete | Удаляет существующее подключение к частной конечной точке |
> | Action | Microsoft. SQL/Servers/Приватиндпоинтконнектионс/Read | Возвращает список подключений к частным конечным точкам или возвращает свойства для указанного подключения к частной конечной точке. |
> | Action | Microsoft. SQL/Servers/Приватиндпоинтконнектионс/Write | Утверждает или отклоняет существующее подключение к частной конечной точке |
> | Action | Microsoft. SQL/Servers/Приватиндпоинтконнектионсаппровал/действие | Определяет, разрешено ли пользователю утверждать подключение частной конечной точки |
> | Action | Microsoft. SQL/Servers/Привателинкресаурцес/Read | Получение ресурсов частной ссылки для соответствующего сервера SQL Server |
> | Action | Microsoft.Sql/servers/providers/Microsoft.Insights/metricDefinitions/read | Возвращение типов метрик, доступных для серверов. |
> | Action | Microsoft.Sql/servers/read | Возвращение списка серверов или получение свойств для указанного сервера. |
> | Action | Microsoft.Sql/servers/recommendedElasticPools/databases/read | Извлекает метрики для рекомендуемых пулов эластичных баз данных для заданного сервера. |
> | Action | Microsoft.Sql/servers/recommendedElasticPools/read | Извлекает рекомендацию для пулов эластичных баз данных, позволяющую снизить затраты или повысить производительность, на основе журнала использования ресурсов. |
> | Action | Microsoft.Sql/servers/recoverableDatabases/read | Эта операция используется для аварийного восстановления динамической базы данных. Она позволяет восстановить базу данных до последней известной корректной точки. Возвращение сведений о последней корректной резервной копии, но без восстановления базы данных. |
> | Action | Microsoft.Sql/servers/replicationLinks/read | Возвращает список связей репликации или получает свойства для указанных связей репликации. |
> | Action | Microsoft.Sql/servers/restorableDroppedDatabases/read | Получение списка баз данных, которые были удалены на заданном сервере, но остались в политике хранения. |
> | Action | Microsoft.Sql/servers/securityAlertPolicies/operationResults/read | Извлечение результатов операции записи политики обнаружения угроз на сервере. |
> | Action | Microsoft.Sql/servers/securityAlertPolicies/read | Получение списка политик обнаружения угроз сервера, настроенных для данного сервера |
> | Action | Microsoft.Sql/servers/securityAlertPolicies/write | Изменение политики обнаружения угроз для заданного сервера. |
> | Action | Microsoft.Sql/servers/serviceObjectives/read | Извлекает список целей уровня обслуживания (также известных как уровни производительности), доступных на данном сервере. |
> | Action | Microsoft.Sql/servers/syncAgents/delete | Удаление существующего агента синхронизации. |
> | Action | Microsoft.Sql/servers/syncAgents/generateKey/action | Создание ключа регистрации агента синхронизации. |
> | Action | Microsoft.Sql/servers/syncAgents/linkedDatabases/read | Получение списка баз данных, связанных с агентом синхронизации. |
> | Action | Microsoft.Sql/servers/syncAgents/read | Возвращение списка агентов синхронизации или свойств для указанного агента. |
> | Action | Microsoft.Sql/servers/syncAgents/write | Создание агента синхронизации с указанными параметрами либо обновление свойств указанного агента. |
> | Action | Microsoft.Sql/servers/tdeCertificates/action | Создание или обновление сертификата TDE |
> | Action | Microsoft.Sql/servers/usages/read | Возвращает квоту DTU сервера и текущие данные об использовании DTU для всех баз данных на сервере. |
> | Action | Microsoft.Sql/servers/virtualNetworkRules/delete | Удаление существующего правила виртуальной сети. |
> | Action | Microsoft.Sql/servers/virtualNetworkRules/read | Возвращение списка правил виртуальной сети или свойств указанного правила виртуальной сети. |
> | Action | Microsoft.Sql/servers/virtualNetworkRules/write | Создание правила виртуальной сети с указанными параметрами либо обновление свойств или тегов указанного правила виртуальной сети. |
> | Action | Microsoft.Sql/servers/vulnerabilityAssessments/delete | Удаление оценки уязвимости указанного сервера |
> | Action | Microsoft.Sql/servers/vulnerabilityAssessments/read | Получение политик оценки уязвимости для конкретного сервера |
> | Action | Microsoft.Sql/servers/vulnerabilityAssessments/write | Изменение оценки уязвимости указанного сервера |
> | Action | Microsoft.Sql/servers/write | Создание сервера с указанными параметрами либо обновление свойств или тегов указанного сервера. |
> | Action | Microsoft.Sql/unregister/action | Отмена регистрации подписки для поставщика ресурсов базы данных SQL Azure с возможностью создания баз данных Microsoft SQL. |
> | Action | Microsoft. SQL/Виртуалклустерс/Delete | Удаляет существующий виртуальный кластер. |
> | Action | Microsoft.Sql/virtualClusters/read | Возвращение списка виртуальных кластеров или свойств указанного виртуального кластера. |
> | Action | Microsoft.Sql/virtualClusters/write | Обновление тегов виртуального кластера. |

## <a name="microsoftstorage"></a>Microsoft.Storage;

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Storage/checknameavailability/read | Проверяет, допустимо ли имя учетной записи и не используется ли оно. |
> | Action | Microsoft. Storage/Locations/чеккнамеаваилабилити/Read | Проверяет, допустимо ли имя учетной записи и не используется ли оно. |
> | Action | Microsoft.Storage/locations/deleteVirtualNetworkOrSubnets/action | Уведомляет Microsoft.Storage о том, что идет удаление виртуальной сети или подсети. |
> | Action | Microsoft.Storage/locations/usages/read | Возвращает предел и текущее значение счетчика использования для ресурсов в указанной подписке |
> | Action | Microsoft.Storage/operations/read | Опрашивает состояние асинхронной операции. |
> | Action | Microsoft.Storage/register/action | Регистрирует подписку в поставщике ресурсов хранилища и включает возможность создания учетных записей хранения. |
> | Action | Microsoft.Storage/skus/read | Перечисляет номера SKU, поддерживаемые Microsoft.Storage. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/add/action | Возвращение результата, полученного при добавлении содержимого большого двоичного объекта. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete | Возвращение результата, полученного при удалении большого двоичного объекта. |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/deleteAutomaticSnapshot/action | Возвращает результат удаления автоматического моментального снимка |
> | DataAction | Microsoft. Storage/storageAccounts/Блобсервицес/Containers/blobs/Filter/действие | Возвращает список больших двоичных объектов в учетной записи с соответствующими фильтру тегами |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read | Возвращение большого двоичного объекта или списка таких объектов. |
> | DataAction | Microsoft. Storage/storageAccounts/Блобсервицес/контейнеры/BLOB-объекты/Теги/чтение | Возвращает результат чтения тегов большого двоичного объекта |
> | DataAction | Microsoft. Storage/storageAccounts/Блобсервицес/Containers/blobs/Tags/запись | Возвращает результат записи тегов большого двоичного объекта |
> | DataAction | Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write | Возвращает результат записи BLOB-объекта |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/clearLegalHold/action | Снять с контейнера BLOB-объектов удержание по юридическим причинам |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/delete | Возвращает результат удаления контейнера |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/delete | Удалить политику неизменяемости контейнера BLOB-объектов |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/extend/action | Расширить политику неизменяемости контейнера BLOB-объектов |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/lock/action | Блокировка политики неизменяемости контейнера BLOB-объектов |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/read | Получить политику неизменяемости контейнера BLOB-объектов |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/immutabilityPolicies/write | Разместить политику неизменяемости контейнера BLOB-объектов |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/lease/action | Возвращает результат аренды контейнера больших двоичных объектов. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/read | Возвращает контейнер. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/read | Возвращает список контейнеров. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/setLegalHold/action | Поставить контейнер BLOB-объектов на удержание по юридическим причинам |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/write | Возвращает результат обновления контейнера больших двоичных объектов. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/containers/write | Возвращает результат размещения контейнера больших двоичных объектов. |
> | Action | Microsoft.Storage/storageAccounts/blobServices/generateUserDelegationKey/action | Возвращает ключ делегирования пользователя для службы BLOB-объектов |
> | Action | Microsoft.Storage/storageAccounts/blobServices/read |  |
> | Action | Microsoft.Storage/storageAccounts/blobServices/read | Возвращает свойства или статистику службы BLOB-объектов |
> | Action | Microsoft.Storage/storageAccounts/blobServices/write | Возвращает результат свойств размещения службы BLOB-объектов |
> | Action | Microsoft.Storage/storageAccounts/delete | Удаляет существующую учетную запись хранения. |
> | Action | Microsoft. Storage, storageAccounts, отработка отказа или действие | Клиент может управлять отработкой отказа в случае проблем доступности |
> | Action | Microsoft. Storage, storageAccounts/Филесервицес/Read | Получить свойства службы файлов |
> | Action | Microsoft.Storage/storageAccounts/listAccountSas/action | Возвращает маркер SAS учетной записи для указанной учетной записи хранения. |
> | Action | Microsoft.Storage/storageAccounts/listkeys/action | Возвращает ключи доступа для указанной учетной записи хранения. |
> | Action | Microsoft.Storage/storageAccounts/listServiceSas/action | Возвращает маркер SAS службы для указанной учетной записи хранения. |
> | Action | Microsoft. Storage, storageAccounts/МанажементполиЦиес/Delete | Удаление политик управления учетными записями хранения |
> | Action | Microsoft. Storage, storageAccounts/МанажементполиЦиес/Read | Получение политик управления учетными записями хранения |
> | Action | Microsoft. Storage/storageAccounts/МанажементполиЦиес/запись | Задание политик управления учетными записями хранения |
> | Action | Microsoft. Storage, storageAccounts/Приватиндпоинтконнектионпроксиес/Delete | Удаление прокси-серверов подключения частной конечной точки |
> | Action | Microsoft. Storage/storageAccounts/Приватиндпоинтконнектионпроксиес/запись | Размещение прокси-серверов подключения частной конечной точки |
> | Action | Microsoft. Storage, storageAccounts/Приватиндпоинтконнектионс/Delete | Удаление подключения к частной конечной точке |
> | Action | Microsoft. Storage, storageAccounts/Приватиндпоинтконнектионс/Read | Получение подключения к частной конечной точке |
> | Action | Microsoft. Storage/storageAccounts/Приватиндпоинтконнектионс/запись | Размещение частного подключения конечной точки |
> | Action | Microsoft. Storage/storageAccounts/Приватиндпоинтконнектионсаппровал/действие | Утверждение подключений к частным конечным точкам |
> | Action | Microsoft. Storage, storageAccounts/Привателинкресаурцес/Read | Получение StorageAccount граупидс |
> | Action | Microsoft.Storage/storageAccounts/queueServices/queues/delete | Возвращает результат удаления очереди |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/add/action | Возвращение результата, полученного при добавлении сообщения. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/delete | Возвращение результата, полученного при удалении сообщения. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/process/action | Возвращение результата, полученного при обработке сообщения. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/read | Возвращение сообщения. |
> | DataAction | Microsoft.Storage/storageAccounts/queueServices/queues/messages/write | Возвращает результат записи сообщения |
> | Action | Microsoft.Storage/storageAccounts/queueServices/queues/read | Возвращает очередь или список очередей. |
> | Action | Microsoft.Storage/storageAccounts/queueServices/queues/write | Возвращает результат записи очереди |
> | Action | Microsoft.Storage/storageAccounts/queueServices/read | Получение свойств служба очередей |
> | Action | Microsoft.Storage/storageAccounts/queueServices/read | Возвращает свойства или статистику службы очередей. |
> | Action | Microsoft.Storage/storageAccounts/queueServices/write | Возвращает результат задания свойств службы очередей |
> | Action | Microsoft.Storage/storageAccounts/read | Возвращает список учетных записей хранения или получает свойства для указанной учетной записи хранения. |
> | Action | Microsoft.Storage/storageAccounts/regeneratekey/action | Повторно создает ключи доступа для указанной учетной записи хранения. |
> | Action | Microsoft. Storage/storageAccounts/Рестореблобранжес/действие | Восстановить диапазоны BLOB-объектов в состояние указанного времени |
> | Action | Microsoft.Storage/storageAccounts/revokeUserDelegationKeys/action | Отзывает все ключи делегирования пользователей для указанной учетной записи хранения. |
> | Action | Microsoft.Storage/storageAccounts/services/diagnosticSettings/write | Создание или обновление параметров диагностики учетной записи хранения. |
> | Action | Microsoft. Storage, storageAccounts/Таблесервицес/Read | Получить свойства службы таблиц |
> | Action | Microsoft.Storage/storageAccounts/write | Создает учетную запись хранения с указанными параметрами или обновляет свойства или теги или добавляет пользовательский домен для указанной учетной записи хранения. |
> | Action | Microsoft.Storage/usages/read | Возвращает предел и текущее значение счетчика использования для ресурсов в указанной подписке |

## <a name="microsoftstoragesync"></a>Microsoft. storagesync

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | microsoft.storagesync/locations/checkNameAvailability/action | Проверяет, является ли имя службы синхронизации хранилища допустимым и неиспользуемым. |
> | Action | microsoft.storagesync/locations/workflows/operations/read | Получение состояния асинхронной операции |
> | Action | Microsoft. storagesync/операции/чтение | Возвращает список поддерживаемых операций. |
> | Action | Microsoft. storagesync/Register/действие | Регистрирует подписку для поставщика синхронизации хранилища. |
> | Action | microsoft.storagesync/storageSyncServices/delete | Удаление служб синхронизации хранилища. |
> | Action | microsoft.storagesync/storageSyncServices/providers/Microsoft.Insights/metricDefinitions/read | Получение доступных метрик для служб синхронизации хранилища. |
> | Action | microsoft.storagesync/storageSyncServices/read | Чтение служб синхронизации хранилища. |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/delete | Удаление любого зарегистрированного сервера. |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/providers/Microsoft.Insights/metricDefinitions/read | Получает доступные метрики для зарегистрированного сервера. |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/read | Чтение зарегистрированного сервера. |
> | Action | microsoft.storagesync/storageSyncServices/registeredServers/write | Создание или обновление любого зарегистрированного сервера. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/delete | Удаление конечных точек облака. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/operationresults/read | Получает состояние асинхронной операции резервного копирования или восстановления |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postbackup/action | Это действие следует вызывать после резервного копирования. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/postrestore/action | Это действие следует вызывать после восстановления. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prebackup/action | Это действие следует вызывать до резервного копирования. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/prerestore/action | Это действие следует вызывать до восстановления. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/read | Чтение конечных точек облака. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/restoreheartbeat/action | Восстановление пакетов пульса. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/cloudEndpoints/write | Создание или обновление конечных точек облака. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/delete | Удаление любой группы синхронизации. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/providers/Microsoft.Insights/metricDefinitions/read | Получает доступные метрики для групп синхронизации. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/read | Чтение всех групп синхронизации. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/delete | Удаление любой конечной точки сервера. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/providers/Microsoft.Insights/metricDefinitions/read | Получает доступные метрики для конечных точек сервера. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/read | Чтение конечных точек сервера. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/recallAction/action | Действие для отзыва файлов на сервере. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/serverEndpoints/write | Создание или обновление конечных точек сервера. |
> | Action | microsoft.storagesync/storageSyncServices/syncGroups/write | Создание или обновление групп синхронизации. |
> | Action | microsoft.storagesync/storageSyncServices/workflows/operationresults/read | Получение состояния асинхронной операции |
> | Action | microsoft.storagesync/storageSyncServices/workflows/operations/read | Получение состояния асинхронной операции |
> | Action | microsoft.storagesync/storageSyncServices/workflows/read | Чтение рабочих процессов |
> | Action | microsoft.storagesync/storageSyncServices/write | Создание или обновление служб синхронизации хранилища. |
> | Action | Microsoft. storagesync/отменить регистрацию или действие | Отменяет регистрацию подписки для поставщика синхронизации хранилища. |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/delete | Удаляет записи контроля доступа. |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/read | Выводит список записей контроля доступа или возвращает их. |
> | Action | Microsoft.StorSimple/managers/accessControlRecords/write | Создает или обновляет записи контроля доступа. |
> | Action | Microsoft.StorSimple/managers/alerts/read | Выводит список оповещений или возвращает их. |
> | Action | Microsoft.StorSimple/managers/backups/read | Возвращает набор архивации или выводит его. |
> | Action | Microsoft.StorSimple/managers/bandwidthSettings/delete | Удаляет существующие параметры пропускной способности (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/bandwidthSettings/operationResults/read | Выводит результаты операции. |
> | Action | Microsoft.StorSimple/managers/bandwidthSettings/read | Выводит список параметров пропускной способности (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/bandwidthSettings/write | Создает или обновляет параметры пропускной способности (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/certificates/write | Создает или обновляет сертификаты. |
> | Action | Microsoft.StorSimple/Managers/certificates/write | Операция "Обновить сертификат ресурсов" обновляет сертификат учетных данных ресурсов или хранилища. |
> | Action | Microsoft.StorSimple/managers/clearAlerts/action | Очищает все оповещения, связанные с диспетчером устройств. |
> | Action | Microsoft.StorSimple/managers/cloudApplianceConfigurations/read | Выводит список конфигураций, поддерживаемых облачным устройством. |
> | Action | Microsoft.StorSimple/managers/configureDevice/action | Настраивает устройство. |
> | Action | Microsoft.StorSimple/managers/delete | Удаляет диспетчеры устройств. |
> | Action | Microsoft.StorSimple/Managers/delete | Операция "Удалить хранилище" удаляет указанный ресурс Azure типа "vault" |
> | Action | Microsoft.StorSimple/managers/devices/alertSettings/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/alertSettings/read | Возвращает параметры оповещения или выводит их список. |
> | Action | Microsoft.StorSimple/managers/devices/alertSettings/write | Создает или обновляет параметры оповещения. |
> | Action | Microsoft.StorSimple/managers/devices/authorizeForServiceEncryptionKeyRollover/action | Разрешает смену ключей шифрования службы на устройствах. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/backup/action | Выполняет ручную архивацию всех томов, защищенных с помощью политики архивации, для создания их резервной копии по запросу. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/delete | Удаляет существующие политики архивации (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/operationResults/read | Выводит результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/read | Выводит список политик архивации (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/delete | Удаляет существующие расписания. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/operationResults/read | Выводит результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/read | Выводит список расписаний. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/schedules/write | Создает или обновляет расписания. |
> | Action | Microsoft.StorSimple/managers/devices/backupPolicies/write | Создает или обновляет политики архивации (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/devices/backups/delete | Удаляет набор архивации. |
> | Action | Microsoft.StorSimple/managers/devices/backups/elements/clone/action | Клонирует общедоступный ресурс или том с помощью элемента архива. |
> | Action | Microsoft.StorSimple/managers/devices/backups/elements/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/backups/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/backups/read | Возвращает набор архивации или выводит его. |
> | Action | Microsoft.StorSimple/managers/devices/backups/restore/action | Восстанавливает все тома из набора архивации. |
> | Action | Microsoft.StorSimple/managers/devices/backupScheduleGroups/delete | Удаляет группы расписаний архивации. |
> | Action | Microsoft.StorSimple/managers/devices/backupScheduleGroups/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/backupScheduleGroups/read | Выводит список групп расписаний архивации или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/backupScheduleGroups/write | Создает или обновляет группы расписаний архивации. |
> | Action | Microsoft.StorSimple/managers/devices/chapSettings/delete | Удаляет параметры CHAP. |
> | Action | Microsoft.StorSimple/managers/devices/chapSettings/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/chapSettings/read | Возвращает параметры CHAP или выводит их список. |
> | Action | Microsoft.StorSimple/managers/devices/chapSettings/write | Создает или обновляет параметры CHAP. |
> | Action | Microsoft.StorSimple/managers/devices/deactivate/action | Деактивирует устройство. |
> | Action | Microsoft.StorSimple/managers/devices/delete | Удаляет устройства. |
> | Action | Microsoft.StorSimple/managers/devices/disks/read | Выводит список дисков или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/download/action | Скачивает обновления для устройства. |
> | Action | Microsoft.StorSimple/managers/devices/failover/action | Выполняет отработку отказа устройства. |
> | Action | Microsoft.StorSimple/managers/devices/failover/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/failoverTargets/read | Выводит или возвращает цели отработки отказа для устройств. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/backup/action | Архивирует файловый сервер. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/delete | Удаляет файловые серверы. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/metrics/read | Выводит список метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/read | Выводит список файловых серверов или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/delete | Удаляет общедоступные ресурсы. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/metrics/read | Выводит список метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/read | Выводит список общедоступных ресурсов или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/shares/write | Создает или обновляет общедоступные ресурсы. |
> | Action | Microsoft.StorSimple/managers/devices/fileservers/write | Создает или обновляет файловые серверы. |
> | Action | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/changeControllerPowerState/action | Изменяет состояние питания контроллера для групп компонентов оборудования. |
> | Action | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/operationResults/read | Выводит результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/hardwareComponentGroups/read | Выводит список групп компонентов оборудования. |
> | Action | Microsoft.StorSimple/managers/devices/install/action | Устанавливает обновления на устройство. |
> | Action | Microsoft.StorSimple/managers/devices/installUpdates/action | Устанавливает обновления на устройства (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/backup/action | Архивирует сервер iSCSI. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/delete | Удаляет серверы iSCSI. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/delete | Удаляет диски. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metrics/read | Выводит список метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/read | Выводит список дисков или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/disks/write | Создает или обновляет диски. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/metrics/read | Выводит список метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/read | Выводит список серверов iSCSI или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/iscsiservers/write | Создает или обновляет серверы iSCSI. |
> | Action | Microsoft.StorSimple/managers/devices/jobs/cancel/action | Отменяет выполнение задания. |
> | Action | Microsoft.StorSimple/managers/devices/jobs/operationResults/read | Выводит результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/jobs/read | Выводит список заданий или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/listFailoverSets/action | Выводит список групп отработки отказа для существующего устройства (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/devices/listFailoverTargets/action | Выводит список целей отработки отказа для устройств (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/devices/metrics/read | Выводит список метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigration/action | Подтверждает успешный перенос и фиксирует его. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/confirmMigrationStatus/read | Выводит состояние подтверждения переноса. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchConfirmMigrationStatus/action | Извлекает состояние подтверждения переноса. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationEstimate/action | Извлекает состояние задания оценки переноса. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/fetchMigrationStatus/action | Извлекает состояние переноса. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/import/action | Импортирует конфигурации источников для переноса. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationEstimate/read | Выводит оценку переноса. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/migrationStatus/read | Выводит состояние переноса. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/operationResults/read | Выводит результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigration/action | Начинает перенос с использованием конфигураций источников. |
> | Action | Microsoft.StorSimple/managers/devices/migrationSourceConfigurations/startMigrationEstimate/action | Запускает задание для оценки длительности процесса переноса. |
> | Action | Microsoft.StorSimple/managers/devices/networkSettings/operationResults/read | Выводит результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/networkSettings/read | Возвращает параметры сети или выводит их список. |
> | Action | Microsoft.StorSimple/managers/devices/networkSettings/write | Создает или обновляет параметры сети. |
> | Action | Microsoft.StorSimple/managers/devices/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/publicEncryptionKey/action | Отображает открытый ключ шифрования для диспетчера устройств. |
> | Action | Microsoft.StorSimple/managers/devices/publishSupportPackage/action | Публикация пакета поддержки для существующего устройства. Пакет поддержки StorSimple — это простой в использовании механизм, который собирает все соответствующие журналы, помогая службе технической поддержки Майкрософт устранять неполадки в работе устройства StorSimple. |
> | Action | Microsoft.StorSimple/managers/devices/read | Выводит список устройств или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/scanForUpdates/action | Выполняет поиск обновлений в устройстве. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/read | Выводит список параметров безопасности. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/syncRemoteManagementCertificate/action | Синхронизирует сертификат удаленного управления для устройства. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/update/action | Обновляет параметры безопасности. |
> | Action | Microsoft.StorSimple/managers/devices/securitySettings/write | Создает или обновляет параметры безопасности. |
> | Action | Microsoft.StorSimple/managers/devices/sendTestAlertEmail/action | Отправляет тестовое сообщение оповещения настроенным получателям электронной почты. |
> | Action | Microsoft.StorSimple/managers/devices/shares/read | Выводит список общедоступных ресурсов или возвращает их. |
> | Action | Microsoft.StorSimple/managers/devices/timeSettings/operationResults/read | Выводит результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/timeSettings/read | Возвращает параметры времени или выводит их список. |
> | Action | Microsoft.StorSimple/managers/devices/timeSettings/write | Создает или обновляет параметры времени. |
> | Action | Microsoft.StorSimple/managers/devices/updates/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/updateSummary/read | Возвращает сводку обновлений или выводит их список. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/delete | Удаляет существующие контейнеры томов (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/metrics/read | Выводит список метрик. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/metricsDefinitions/read | Выводит список определений метрик. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/operationResults/read | Выводит результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/read | Выводит список контейнеров томов (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/delete | Удаляет существующие тома. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metrics/read | Выводит список метрик. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/metricsDefinitions/read | Выводит список определений метрик. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/operationResults/read | Выводит результаты операции. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/read | Выводит список томов. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/volumes/write | Создает или обновляет тома. |
> | Action | Microsoft.StorSimple/managers/devices/volumeContainers/write | Создает или обновляет контейнеры томов (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/devices/volumes/read | Выводит список томов. |
> | Action | Microsoft.StorSimple/managers/devices/write | Создает или обновляет устройства. |
> | Action | Microsoft.StorSimple/managers/encryptionSettings/read | Возвращает параметры шифрования или выводит их список. |
> | Action | Microsoft.StorSimple/managers/extendedInformation/delete | Удаляет расширенные сведения о хранилище. |
> | Action | Microsoft.StorSimple/Managers/extendedInformation/delete | Операция "Получить расширенные сведения" получает расширенные сведения об объекте, представляющие ресурс Azure типа ?vault? |
> | Action | Microsoft.StorSimple/managers/extendedInformation/read | Выводит или получает расширенные сведения о хранилище. |
> | Action | Microsoft.StorSimple/Managers/extendedInformation/read | Операция "Получить расширенные сведения" получает расширенные сведения об объекте, представляющие ресурс Azure типа ?vault? |
> | Action | Microsoft.StorSimple/managers/extendedInformation/write | Создает или обновляет расширенные сведения о хранилище. |
> | Action | Microsoft.StorSimple/Managers/extendedInformation/write | Операция "Получить расширенные сведения" получает расширенные сведения об объекте, представляющие ресурс Azure типа ?vault? |
> | Action | Microsoft.StorSimple/managers/features/read | Выводит список функций. |
> | Action | Microsoft.StorSimple/managers/fileservers/read | Выводит список файловых серверов или возвращает их. |
> | Action | Microsoft.StorSimple/managers/getEncryptionKey/action | Возвращает ключ шифрования для диспетчера устройств. |
> | Action | Microsoft.StorSimple/managers/iscsiservers/read | Выводит список серверов iSCSI или возвращает их. |
> | Action | Microsoft.StorSimple/managers/jobs/read | Выводит список заданий или возвращает их. |
> | Action | Microsoft.StorSimple/managers/listActivationKey/action | Возвращает ключ активации диспетчера устройств StorSimple. |
> | Action | Microsoft.StorSimple/managers/listPublicEncryptionKey/action | Выводит список открытых ключей шифрования для диспетчера устройств StorSimple. |
> | Action | Microsoft.StorSimple/managers/metrics/read | Выводит список метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/metricsDefinitions/read | Выводит список определений метрик или возвращает их. |
> | Action | Microsoft.StorSimple/managers/migrateClassicToResourceManager/action | Переход с классической модели на модель Resource Manager. |
> | Action | Microsoft.StorSimple/managers/migrationSourceConfigurations/read | Вывод списка конфигураций источника переноса (только для серии 8000). |
> | Action | Microsoft.StorSimple/managers/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/provisionCloudAppliance/action | Создает облачное устройство. |
> | Action | Microsoft.StorSimple/managers/read | Выводит список диспетчеров устройств или возвращает их. |
> | Action | Microsoft.StorSimple/Managers/read | Операция "Получить хранилище" получает объект, представляющий ресурс Azure типа "vault" |
> | Action | Microsoft.StorSimple/managers/regenerateActivationKey/action | Повторное создание ключа активации для существующего диспетчера устройств StorSimple. |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/delete | Удаляет данные учетной записи хранения. |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/read | Выводит список данных учетной записи хранения или возвращает их. |
> | Action | Microsoft.StorSimple/managers/storageAccountCredentials/write | Создает или обновляет данные учетной записи хранения. |
> | Action | Microsoft.StorSimple/managers/storageDomains/delete | Удаляет домены хранилища. |
> | Action | Microsoft.StorSimple/managers/storageDomains/operationResults/read | Выводит или возвращает результаты операции. |
> | Action | Microsoft.StorSimple/managers/storageDomains/read | Выводит список доменов хранилища или возвращает их. |
> | Action | Microsoft.StorSimple/managers/storageDomains/write | Создает или обновляет домены хранилища. |
> | Action | Microsoft.StorSimple/managers/write | Создает или обновляет диспетчеры устройств. |
> | Action | Microsoft.StorSimple/Managers/write | Операция "Создать хранилище" создает ресурс Azure типа vault. |
> | Action | Microsoft.StorSimple/operations/read | Выводит или возвращает операции. |
> | Action | Microsoft.StorSimple/register/action | Регистрирует поставщик Microsoft.StorSimple. |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.StreamAnalytics/locations/quotas/Read | Чтение квоты подписки Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/operations/Read | Чтение операций Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/Register/action | Регистрация подписки в поставщике ресурсов Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Delete | Удаляет задание Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Delete | Удаление функции задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/operationresults/Read | Чтение результатов операции функции задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Read | Чтение функции задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/RetrieveDefaultDefinition/action | Получение определения по умолчанию для функции задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Test/action | Тестирование функции задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/functions/Write | Запись функции задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Delete | Удаляет входные данные задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/operationresults/Read | Чтение результатов операции для получения входных данных задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Read | Считывает входные данные задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Sample/action | Пример входных данных задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Test/action | Тестирование входных данных задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/inputs/Write | Записывает входные данные задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/metricdefinitions/Read | Чтение определений метрик. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/operationresults/Read | Чтение результатов операции задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Delete | Удаляет выходные данные задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/operationresults/Read | Чтение результатов операции для получения выходных данных задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Read | Считывает выходные данные задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Test/action | Тестирование выходных данных задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/outputs/Write | Записывает выходные данные задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/read | Считывает параметр диагностики. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/diagnosticSettings/write | Записывает параметр диагностики. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/logDefinitions/read | Возвращает доступные журналы для заданий потоковой передачи. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/providers/Microsoft.Insights/metricDefinitions/read | Возвращает доступные метрики для заданий потоковой передачи. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Read | Считывает задание Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Start/action | Запускает задание Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Stop/action | Останавливает задание Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/transformations/Delete | Удаляет преобразование задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/transformations/Read | Считывает преобразование задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/transformations/Write | Записывает преобразование задания Stream Analytics. |
> | Action | Microsoft.StreamAnalytics/streamingjobs/Write | Записывает задание Stream Analytics. |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft. Subscription/отмена/действие | Отменяет подписку |
> | Action | Microsoft.Subscription/CreateSubscription/action | Создание подписки Azure |
> | Action | Microsoft.Subscription/register/action | Регистрирует подписку в поставщике ресурсов Microsoft.Subscription. |
> | Action | Microsoft. Subscription/Rename/Action | Переименовывает подписку |
> | Action | Microsoft.Subscription/SubscriptionDefinitions/read | Получение определения подписки Azure в группе управления. |
> | Action | Microsoft.Subscription/SubscriptionDefinitions/write | Создание определения подписки Azure. |

## <a name="microsoftsupport"></a>Microsoft.Support

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.Support/register/action | Выполняет регистрацию у поставщика ресурсов поддержки |
> | Action | Microsoft.Support/supportTickets/read | Получает сведения о запросе в службу поддержки (включая состояние, степень серьезности, контактные сведения и сообщения) или получает список запросов в службу поддержки по всем подпискам. |
> | Action | Microsoft.Support/supportTickets/write | Создает или обновляет запрос в службу поддержки. Можно создавать запросы в службу поддержки по техническим проблемам, а также проблемам, связанным с выставлением счетов, квотами или управлением подписками. Вы можете изменить уровень серьезности, контактную информацию и сообщения для существующих запросов в службу поддержки. |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.TimeSeriesInsights/environments/accesspolicies/delete | Удаление политики доступа. |
> | Action | Microsoft.TimeSeriesInsights/environments/accesspolicies/read | Получение свойств политики доступа. |
> | Action | Microsoft.TimeSeriesInsights/environments/accesspolicies/write | Создание новой политики доступа для среды или обновление существующей политики. |
> | Action | Microsoft.TimeSeriesInsights/environments/delete | Удаление среды. |
> | Action | Microsoft.TimeSeriesInsights/environments/eventsources/delete | Удаление источника события. |
> | Action | Microsoft.TimeSeriesInsights/environments/eventsources/read | Получение свойств источника событий. |
> | Action | Microsoft.TimeSeriesInsights/environments/eventsources/write | Создание нового источника событий для среды или обновление существующего. |
> | Action | Microsoft.TimeSeriesInsights/environments/read | Получение свойств среды. |
> | Action | Microsoft.TimeSeriesInsights/environments/referencedatasets/delete | Удаление эталонного набора данных. |
> | Action | Microsoft.TimeSeriesInsights/environments/referencedatasets/read | Получение свойств эталонного набора данных. |
> | Action | Microsoft.TimeSeriesInsights/environments/referencedatasets/write | Создание нового эталонного набора данных для среды или обновление существующего. |
> | Action | Microsoft.TimeSeriesInsights/environments/status/read | Получение состояния среды и связанных с ней операций, таких как получение входящего трафика. |
> | Action | Microsoft.TimeSeriesInsights/environments/write | Создание новой среды или обновление существующей. |
> | Action | Microsoft.TimeSeriesInsights/register/action | Регистрация подписки для поставщика ресурсов службы "Аналитика временных рядов" с возможностью создания сред данной службы. |

## <a name="microsoftvisualstudio"></a>Microsoft.VisualStudio

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.VisualStudio/Account/Delete | Удалить учетную запись |
> | Action | Microsoft.VisualStudio/Account/Extension/Read | Считывает учетную запись или расширение. |
> | Action | Microsoft.VisualStudio/Account/Project/Read | Считывает учетную запись или проект. |
> | Action | Microsoft.VisualStudio/Account/Project/Write | Настраивает учетную запись или проект. |
> | Action | Microsoft.VisualStudio/Account/Read | Чтение учетной записи. |
> | Action | Microsoft.VisualStudio/Account/Write | Настройка учетной записи. |
> | Action | Microsoft.VisualStudio/Extension/Delete | Удаление расширения. |
> | Action | Microsoft.VisualStudio/Extension/Read | Чтение расширения. |
> | Action | Microsoft.VisualStudio/Extension/Write | Установка расширения. |
> | Action | Microsoft.VisualStudio/Project/Delete | Удалить проект |
> | Action | Microsoft.VisualStudio/Project/Read | Чтение проекта. |
> | Action | Microsoft.VisualStudio/Project/Write | Настройка проекта. |
> | Action | Microsoft.VisualStudio/Register/Action | Регистрирует подписку Azure у поставщика Microsoft.VisualStudio. |

## <a name="microsoftweb"></a>microsoft.web

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | microsoft.web/apimanagementaccounts/apiacls/read | Получение списков ACL для учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/apiacls/delete | Удаление списков ACL для API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/apiacls/read | Получение списков ACL для API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/apiacls/write | Обновление списков ACL для API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connectionacls/read | Получение списков ACL для подключения API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/confirmconsentcode/action | Подтверждение кода согласия на подключение для API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/delete | Удаление списков ACL для подключения API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/read | Получение списков ACL для подключения API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/connectionacls/write | Обновление списков ACL для подключения API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/delete | Удаление подключений API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/getconsentlinks/action | Получение ссылок на согласие для подключений API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/listconnectionkeys/action | Получение ключей подключения для подключений API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/listsecrets/action | Получение списка секретов для подключений API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/read | Получение подключений API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/connections/write | Обновление подключений API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/delete | Удаление API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/delete | Удаление локализованных определений API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/read | Получение локализованных определений API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/localizeddefinitions/write | Обновление локализованных определений API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/read | Получение API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/apis/write | Обновление API-интерфейсов учетных записей управления API. |
> | Action | microsoft.web/apimanagementaccounts/connectionacls/read | Получение списков ACL для подключения учетных записей управления API. |
> | Action | microsoft.web/availablestacks/read | Возвращает доступные стеки. |
> | Action | Microsoft.Web/certificates/Delete | Удаляет существующий сертификат. |
> | Action | Microsoft.Web/certificates/Read | Возвращает список сертификатов. |
> | Action | Microsoft.Web/certificates/Write | Добавляет новый сертификат или обновляет существующий. |
> | Action | microsoft.web/checknameavailability/read | Проверяет, доступно ли имя ресурса. |
> | Action | microsoft.web/classicmobileservices/read | Возвращает классические мобильные службы. |
> | Action | Microsoft.Web/connectionGateways/Delete | Удаляет шлюз подключения. |
> | Action | Microsoft.Web/connectionGateways/Join/Action | Присоединяет шлюз подключения. |
> | Action | Microsoft.Web/connectionGateways/ListStatus/Action | Получение состояния шлюза подключения. |
> | Action | Microsoft.Web/connectionGateways/Move/Action | Перемещение шлюза подключения. |
> | Action | Microsoft.Web/connectionGateways/Read | Возвращает список шлюзов подключений. |
> | Action | Microsoft.Web/connectionGateways/Write | Создает или обновляет шлюз для подключения. |
> | Action | microsoft.web/connections/confirmconsentcode/action | Подтверждает код согласия на подключение. |
> | Action | Microsoft.Web/connections/Delete | Удаляет подключение. |
> | Action | Microsoft.Web/connections/Join/Action | Присоединяет подключение. |
> | Action | microsoft.web/connections/listconsentlinks/action | Выводит список ссылок на согласие для подключений. |
> | Action | Microsoft.Web/connections/Move/Action | Перемещение подключения. |
> | Action | Microsoft.Web/connections/Read | Возвращает список подключений. |
> | Action | Microsoft.Web/connections/Write | Создает или обновляет подключение. |
> | Action | Microsoft.Web/customApis/Delete | Удаление пользовательского API. |
> | Action | Microsoft.Web/customApis/extractApiDefinitionFromWsdl/Action | Извлечение определения API из формата WSDL. |
> | Action | Microsoft.Web/customApis/Join/Action | Присоединение пользовательского API. |
> | Action | Microsoft.Web/customApis/listWsdlInterfaces/Action | Получение списка интерфейсов языка WSDL для пользовательского API. |
> | Action | Microsoft.Web/customApis/Move/Action | Перемещение пользовательского API. |
> | Action | Microsoft.Web/customApis/Read | Получение списка пользовательских API. |
> | Action | Microsoft.Web/customApis/Write | Создание или обновление пользовательского API. |
> | Action | Microsoft.Web/deletedSites/Read | Возвращение свойств удаленного веб-приложения. |
> | Action | microsoft.web/deploymentlocations/read | Возвращает расположения развертываний. |
> | Action | Microsoft.Web/geoRegions/Read | Возвращает список географических регионов. |
> | Action | microsoft.web/hostingenvironments/capacities/read | Возвращает емкости сред внешнего размещения. |
> | Action | Microsoft.Web/hostingEnvironments/Delete | Удаляет среду службы приложений. |
> | Action | microsoft.web/hostingenvironments/detectors/read | Возвращение средств обнаружения для сред внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/diagnostics/read | Возвращает данные диагностики сред внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/inboundnetworkdependenciesendpoints/read | Получение конечных точек сети всех входящих зависимостей. |
> | Action | Microsoft. Web/hostingEnvironments/Join/действие | Присоединяет Среда службы приложений |
> | Action | microsoft.web/hostingenvironments/metricdefinitions/read | Возвращает определения метрик сред внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/multirolepools/metricdefinitions/read | Возвращает определения метрик пулов множественных ролей в средах внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/multirolepools/metrics/read | Возвращает метрики пулов множественных ролей в средах внешнего размещения. |
> | Action | Microsoft.Web/hostingEnvironments/multiRolePools/Read | Возвращает свойства интерфейсного пула в среде службы приложений. |
> | Action | microsoft.web/hostingenvironments/multirolepools/skus/read | Возвращает номера SKU пулов множественных ролей в средах внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/multirolepools/usages/read | Возвращает данные об использовании пулов множественных ролей в средах внешнего размещения. |
> | Action | Microsoft.Web/hostingEnvironments/multiRolePools/Write | Создает новый интерфейсный пул в среде службы приложений или обновляет существующий. |
> | Action | microsoft.web/hostingenvironments/operations/read | Возвращает операции сред внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/outboundnetworkdependenciesendpoints/read | Получение конечных точек сети всех исходящих зависимостей. |
> | Action | Microsoft. Web/hostingEnvironments/Приватиндпоинтконнектионсаппровал/действие | Утверждение подключений к частным конечным точкам |
> | Action | Microsoft.Web/hostingEnvironments/Read | Возвращает свойства среды службы приложений. |
> | Action | Microsoft.Web/hostingEnvironments/reboot/Action | Перезагружает все компьютеры в среде службы приложений. |
> | Action | microsoft.web/hostingenvironments/resume/action | Возобновляет работу сред внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/serverfarms/read | Возвращает планы службы приложений в средах внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/sites/read | Возвращает веб-приложения в средах внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/suspend/action | Приостанавливает работу сред внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/usages/read | Возвращает данные об использовании сред внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/workerpools/metricdefinitions/read | Возвращает определения метрик рабочих пулов в средах внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/workerpools/metrics/read | Возвращает метрики рабочих пулов в средах внешнего размещения. |
> | Action | Microsoft.Web/hostingEnvironments/workerPools/Read | Возвращает свойства рабочего пула в среде службы приложений. |
> | Action | microsoft.web/hostingenvironments/workerpools/skus/read | Возвращает номера SKU рабочих пулов в средах внешнего размещения. |
> | Action | microsoft.web/hostingenvironments/workerpools/usages/read | Возвращает данные об использовании рабочих пулов в средах внешнего размещения. |
> | Action | Microsoft.Web/hostingEnvironments/workerPools/Write | Создает новый рабочий пул в среде службы приложений или обновляет существующий. |
> | Action | Microsoft.Web/hostingEnvironments/Write | Создает новую среду службы приложений или обновляет существующую. |
> | Action | microsoft.web/ishostingenvironmentnameavailable/read | Проверяет, доступно ли имя среды внешнего размещения. |
> | Action | microsoft.web/ishostnameavailable/read | Проверяет, доступно ли имя узла. |
> | Action | microsoft.web/isusernameavailable/read | Проверяет, доступно ли имя пользователя. |
> | Action | Microsoft.Web/listSitesAssignedToHostName/Read | Возвращает имена сайтов, назначенные имени узла. |
> | Action | microsoft.web/locations/apioperations/read | Возвращает операции API расположений. |
> | Action | microsoft.web/locations/connectiongatewayinstallations/read | Возвращает данные об установленных шлюзах подключений в расположениях. |
> | Action | microsoft.web/locations/deleteVirtualNetworkOrSubnets/action | Уведомление об удалении виртуальной сети или подсети для расположений. |
> | Action | microsoft.web/locations/extractapidefinitionfromwsdl/action | Извлечение определения API для расположений из синтаксиса WSDL. |
> | Action | microsoft.web/locations/listwsdlinterfaces/action | Получение списка интерфейсов языка WSDL для расположений. |
> | Action | microsoft.web/locations/managedapis/apioperations/read | Получение операций с управляемыми API для расположений. |
> | Action | Microsoft.Web/locations/managedapis/Join/Action | Присоединение управляемого API. |
> | Action | microsoft.web/locations/managedapis/read | Возвращает расположения управляемых интерфейсов API. |
> | Action | microsoft.web/operations/read | Возвращает операции. |
> | Action | microsoft.web/publishingusers/read | Возвращает публикующих пользователей. |
> | Action | microsoft.web/publishingusers/write | Обновляет публикующих пользователей. |
> | Action | Microsoft.Web/recommendations/Read | Возвращает список рекомендаций для подписок. |
> | Action | microsoft.web/register/action | Регистрирует поставщик ресурсов Microsoft.Web для подписки. |
> | Action | microsoft.web/resourcehealthmetadata/read | Получение метаданных работоспособности ресурса. |
> | Action | microsoft.web/serverfarms/capabilities/read | Возвращает возможности планов службы приложений. |
> | Action | Microsoft.Web/serverfarms/Delete | Удаление существующего плана службы приложений |
> | Action | microsoft.web/serverfarms/firstpartyapps/settings/delete | Удаляет параметры основных приложений в планах службы приложений. |
> | Action | microsoft.web/serverfarms/firstpartyapps/settings/read | Возвращает параметры основных приложений в планах службы приложений. |
> | Action | microsoft.web/serverfarms/firstpartyapps/settings/write | Обновляет параметры основных приложений в планах службы приложений. |
> | Action | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/delete | Удаление ретрансляторов в пространствах имен гибридных подключений для плана службы приложений. |
> | Action | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/read | Получение ретрансляторов в пространствах имен гибридных подключений для плана службы приложений. |
> | Action | microsoft.web/serverfarms/hybridconnectionnamespaces/relays/sites/read | Возвращает веб-приложения ретрансляторов в пространствах имен гибридных подключений для плана службы приложений. |
> | Action | microsoft.web/serverfarms/hybridconnectionplanlimits/read | Возвращает ограничения плана гибридных подключений для плана службы приложений. |
> | Action | microsoft.web/serverfarms/hybridconnectionrelays/read | Возвращает ретрансляторы гибридных подключений для планов службы приложений. |
> | Action | microsoft.web/serverfarms/metricdefinitions/read | Возвращает определения метрик планов службы приложений. |
> | Action | microsoft.web/serverfarms/metrics/read | Возвращает метрики планов службы приложений. |
> | Action | microsoft.web/serverfarms/operationresults/read | Возвращает результаты операций планов службы приложений. |
> | Action | Microsoft.Web/serverfarms/Read | Возвращает свойства плана службы приложений. |
> | Action | Microsoft.Web/serverfarms/restartSites/Action | Перезапустите все веб-приложения в плане службы приложений. |
> | Action | microsoft.web/serverfarms/sites/read | Возвращает веб-приложения в планах службы приложений. |
> | Action | microsoft.web/serverfarms/skus/read | Возвращает номера SKU в планах службы приложений. |
> | Action | microsoft.web/serverfarms/usages/read | Возвращает данные об использовании для планов службы приложений. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/gateways/write | Обновляет шлюзы подключений виртуальной сети для планов службы приложений. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/read | Возвращает подключения виртуальной сети для планов службы приложений. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/routes/delete | Удаляет маршруты подключений виртуальной сети для планов службы приложений. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/routes/read | Возвращает маршруты подключений виртуальной сети для планов службы приложений. |
> | Action | microsoft.web/serverfarms/virtualnetworkconnections/routes/write | Обновляет маршруты подключений виртуальной сети для планов службы приложений. |
> | Action | microsoft.web/serverfarms/workers/reboot/action | Перезапускает рабочие роли планов службы приложений. |
> | Action | Microsoft.Web/serverfarms/Write | Создает новый план службы приложений или обновляет существующий. |
> | Action | microsoft.web/sites/analyzecustomhostname/read | Анализирует пользовательское имя узла. |
> | Action | Microsoft.Web/sites/applySlotConfig/Action | Применяет конфигурацию слота веб-приложения из целевого слота к текущему веб-приложению. |
> | Action | Microsoft.Web/sites/backup/Action | Создает резервную копию веб-приложения. |
> | Action | microsoft.web/sites/backup/read | Возвращает резервную копию веб-приложений. |
> | Action | microsoft.web/sites/backup/write | Обновляет резервную копию веб-приложений. |
> | Action | microsoft.web/sites/backups/action | Обнаружение имеющейся резервной копии приложения, которую можно восстановить из большого двоичного объекта в хранилище Azure. |
> | Action | microsoft.web/sites/backups/delete | Удаление резервных копий веб-приложений. |
> | Action | microsoft.web/sites/backups/list/action | Выводит список резервных копий веб-приложений. |
> | Action | Microsoft.Web/sites/backups/Read | Возвращает свойства резервной копии веб-приложения. |
> | Action | microsoft.web/sites/backups/restore/action | Восстанавливает резервные копии веб-приложений. |
> | Action | microsoft.web/sites/backups/write | Обновление резервных копий веб-приложений. |
> | Action | microsoft.web/sites/config/delete | Удаляет конфигурацию веб-приложений. |
> | Action | Microsoft.Web/sites/config/list/Action | Отображает влияющие на безопасность параметры веб-приложения, такие как учетные данные для публикации, параметры приложения и строки подключения. |
> | Action | Microsoft.Web/sites/config/Read | Возвращает параметры конфигурации веб-приложения. |
> | Action | microsoft.web/sites/config/snapshots/read | Получение моментальных снимков конфигурации веб-приложений. |
> | Action | Microsoft.Web/sites/config/Write | Обновляет параметры конфигурации веб-приложения. |
> | Action | microsoft.web/sites/containerlogs/action | Получение сжатых журналов контейнеров для веб-приложений. |
> | Action | microsoft.web/sites/containerlogs/download/action | Скачивание журналов контейнера веб-приложений. |
> | Action | microsoft.web/sites/continuouswebjobs/delete | Удаляет непрерывные веб-задания веб-приложений. |
> | Action | microsoft.web/sites/continuouswebjobs/read | Возвращает непрерывные веб-задания веб-приложений. |
> | Action | microsoft.web/sites/continuouswebjobs/start/action | Запускает непрерывные веб-задания веб-приложений. |
> | Action | microsoft.web/sites/continuouswebjobs/stop/action | Останавливает непрерывные веб-задания веб-приложений. |
> | Action | Microsoft.Web/sites/Delete | Удаление существующего веб-приложения |
> | Action | microsoft.web/sites/deployments/delete | Удаляет развертывания веб-приложений. |
> | Action | microsoft.web/sites/deployments/log/read | Возвращает журнал развертываний веб-приложений. |
> | Action | microsoft.web/sites/deployments/read | Возвращает развертывания веб-приложений. |
> | Action | microsoft.web/sites/deployments/write | Обновляет развертывания веб-приложений. |
> | Action | microsoft.web/sites/detectors/read | Получение средств обнаружения для веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/analyses/execute/Action | Запуск анализа данных диагностики веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/analyses/read | Получение анализа данных диагностики веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/aspnetcore/read | Получение данных диагностики веб-приложений для приложения ASP.NET Core. |
> | Action | microsoft.web/sites/diagnostics/autoheal/read | Получение данных диагностики о функции Autoheal для веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/deployment/read | Получение данных диагностики о развертывании для веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/deployments/read | Получение данных диагностики о развертываниях для веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/detectors/execute/Action | Запуск средства обнаружения диагностики веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/detectors/read | Получение данных средства обнаружения диагностики веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/failedrequestsperuri/read | Получение данных диагностики веб-приложений о неудачных запросах для каждого URI. |
> | Action | microsoft.web/sites/diagnostics/frebanalysis/read | Возвращает данные диагностики об анализе FREB для веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/loganalyzer/read | Получение данных диагностики веб-приложений об анализаторе журналов. |
> | Action | microsoft.web/sites/diagnostics/read | Получение категорий данных диагностики веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/runtimeavailability/read | Возвращает данные диагностики о доступности среды выполнения для веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/servicehealth/read | Возвращает данные диагностики о работоспособности служб для веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/sitecpuanalysis/read | Получение данных диагностики веб-приложений об анализе ЦП для сайта. |
> | Action | microsoft.web/sites/diagnostics/sitecrashes/read | Получение данных диагностики веб-приложений о сбоях сайта. |
> | Action | microsoft.web/sites/diagnostics/sitelatency/read | Получение данных диагностики веб-приложений о задержках сайта. |
> | Action | microsoft.web/sites/diagnostics/sitememoryanalysis/read | Получение данных диагностики веб-приложений об анализе памяти для сайта. |
> | Action | microsoft.web/sites/diagnostics/siterestartsettingupdate/read | Получение данных диагностики веб-приложений об обновлении параметра перезапуска сайта. |
> | Action | microsoft.web/sites/diagnostics/siterestartuserinitiated/read | Получение данных диагностики веб-приложений об инициированном пользователем перезапуске сайта. |
> | Action | microsoft.web/sites/diagnostics/siteswap/read | Получение данных диагностики веб-приложений о переключении сайта. |
> | Action | microsoft.web/sites/diagnostics/threadcount/read | Получение данных диагностики веб-приложений о количестве потоков. |
> | Action | microsoft.web/sites/diagnostics/workeravailability/read | Возвращает данные диагностики о доступности рабочих процессов для веб-приложений. |
> | Action | microsoft.web/sites/diagnostics/workerprocessrecycle/read | Возвращает данные диагностики перезапуска рабочих процессов для веб-приложений. |
> | Action | microsoft.web/sites/domainownershipidentifiers/read | Возвращает идентификаторы владения доменом веб-приложений. |
> | Action | microsoft.web/sites/domainownershipidentifiers/write | Обновляет идентификаторы владения доменом веб-приложений. |
> | Action | microsoft.web/sites/extensions/delete | Удаление расширений сайта веб-приложений. |
> | Action | microsoft.web/sites/extensions/read | Получение расширений сайта веб-приложений. |
> | Action | microsoft.web/sites/extensions/write | Обновление расширений сайта веб-приложений. |
> | Action | microsoft.web/sites/functions/action | Выполняет функции веб-приложений. |
> | Action | microsoft.web/sites/functions/delete | Удаляет функции веб-приложений. |
> | Action | microsoft.web/sites/functions/listsecrets/action | Выводит список секретов для функций веб-приложений. |
> | Action | microsoft.web/sites/functions/masterkey/read | Получение главного ключа функций веб-приложений. |
> | Action | microsoft.web/sites/functions/read | Возвращает функции веб-приложений. |
> | Action | microsoft.web/sites/functions/token/read | Получение токена функций веб-приложений. |
> | Action | microsoft.web/sites/functions/write | Обновляет функции веб-приложений. |
> | Action | microsoft.web/sites/hostnamebindings/delete | Удаляет привязки имен узлов для веб-приложений. |
> | Action | microsoft.web/sites/hostnamebindings/read | Возвращает привязки имен узлов для веб-приложений. |
> | Action | microsoft.web/sites/hostnamebindings/write | Обновляет привязки имен узлов для веб-приложений. |
> | Action | microsoft.web/sites/hostruntime/functions/keys/read | Получение ключей функций среды выполнения узла веб-приложений. |
> | Action | Microsoft.Web/sites/hostruntime/host/_master/read | Получение главного ключа приложения-функции для операций администрирования. |
> | Action | Microsoft.Web/sites/hostruntime/host/action | Выполнение действий среды выполнения приложения-функции, таких как синхронизация триггеров, добавление функций, вызов функций, их удаление и т.д. |
> | Action | microsoft.web/sites/hostruntime/host/read | Получение узла для среды выполнения узла веб-приложений. |
> | Action | microsoft.web/sites/hybridconnection/delete | Удаляет гибридное подключение для веб-приложений. |
> | Action | microsoft.web/sites/hybridconnection/read | Возвращает гибридное подключение для веб-приложений. |
> | Action | microsoft.web/sites/hybridconnection/write | Обновляет гибридное подключение для веб-приложений. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/delete | Удаление ретрансляторов в пространствах имен гибридных подключений для веб-приложений. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/listkeys/action | Получение списка ключей ретрансляторов в пространствах имен гибридных подключений для веб-приложений. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/read | Получение ретрансляторов в пространствах имен гибридных подключений для веб-приложений. |
> | Action | microsoft.web/sites/hybridconnectionnamespaces/relays/write | Обновление ретрансляторов в пространствах имен гибридных подключений для веб-приложений. |
> | Action | microsoft.web/sites/hybridconnectionrelays/read | Возвращает ретрансляторы гибридных подключений для веб-приложений. |
> | Action | microsoft.web/sites/instances/deployments/delete | Удаление развертываний экземпляров веб-приложений. |
> | Action | microsoft.web/sites/instances/deployments/read | Возвращает развертывания экземпляров веб-приложений. |
> | Action | microsoft.web/sites/instances/extensions/log/read | Получение журнала расширений экземпляров веб-приложений. |
> | Action | microsoft.web/sites/instances/extensions/processes/read | Получение процессов расширений экземпляров веб-приложений. |
> | Action | microsoft.web/sites/instances/extensions/read | Получение расширений экземпляров веб-приложений. |
> | Action | microsoft.web/sites/instances/processes/delete | Удаляет процессы экземпляров веб-приложений. |
> | Action | microsoft.web/sites/instances/processes/modules/read | Получение модулей процессов экземпляров веб-приложений. |
> | Action | microsoft.web/sites/instances/processes/read | Возвращает процессы экземпляров веб-приложений. |
> | Action | microsoft.web/sites/instances/processes/threads/read | Возвращение потоков процессов экземпляров веб-приложений. |
> | Action | microsoft.web/sites/instances/read | Возвращает экземпляры веб-приложений. |
> | Action | microsoft.web/sites/listsyncfunctiontriggerstatus/action | Отображает состояние триггера функции синхронизации для веб-приложений. |
> | Action | microsoft.web/sites/metricdefinitions/read | Возвращает определения метрик веб-приложений. |
> | Action | microsoft.web/sites/metrics/read | Возвращает метрики веб-приложений. |
> | Action | microsoft.web/sites/metricsdefinitions/read | Получение определений метрик веб-приложений. |
> | Action | microsoft.web/sites/migratemysql/action | Перенос веб-приложений MySQL. |
> | Action | microsoft.web/sites/migratemysql/read | Получение сведений о переносе веб-приложений MySQL. |
> | Action | microsoft.web/sites/networktrace/action | Трассирует сеть для веб-приложений. |
> | Action | Microsoft. Web/Sites/нетворктрацес/оператионресултс/Read | Возвращает результаты операции трассировки сети для веб-приложений. |
> | Action | microsoft.web/sites/newpassword/action | Задает новый пароль для веб-приложений. |
> | Action | microsoft.web/sites/operationresults/read | Возвращает результаты операций веб-приложений. |
> | Action | microsoft.web/sites/operations/read | Получение операций веб-приложений. |
> | Action | microsoft.web/sites/perfcounters/read | Возвращает счетчики производительности веб-приложений. |
> | Action | microsoft.web/sites/premieraddons/delete | Удаляет надстройки Premier для веб-приложений. |
> | Action | microsoft.web/sites/premieraddons/read | Возвращает надстройки Premier для веб-приложений. |
> | Action | microsoft.web/sites/premieraddons/write | Обновляет надстройки Premier для веб-приложений. |
> | Action | microsoft.web/sites/privateaccess/read | Возвращение данных контроля доступа к частным сайтам и авторизованные виртуальные сети, у которых есть доступ к сайту. |
> | Action | Microsoft. Web/Sites/Приватиндпоинтконнектионсаппровал/Action | Утверждение подключений к частным конечным точкам |
> | Action | Microsoft. Web/Sites/Processes/modules/Read | Возвращает веб-приложения, обрабатывающие модули. |
> | Action | microsoft.web/sites/processes/read | Получение процессов веб-приложений. |
> | Action | Microsoft. Web/Sites/Processes/threads/Read | Возвращает веб-приложения, обрабатывающие потоки. |
> | Action | microsoft.web/sites/publiccertificates/delete | Удаление открытых сертификатов веб-приложений. |
> | Action | microsoft.web/sites/publiccertificates/read | Получение открытых сертификатов веб-приложений. |
> | Action | microsoft.web/sites/publiccertificates/write | Обновление открытых сертификатов веб-приложений. |
> | Action | Microsoft.Web/sites/publish/Action | Публикует веб-приложение. |
> | Action | Microsoft.Web/sites/publishxml/Action | Возвращает XML-файл профиля публикации для веб-приложения. |
> | Action | microsoft.web/sites/publishxml/read | Возвращает XML-файл профиля публикации для веб-приложений. |
> | Action | Microsoft.Web/sites/Read | Возвращает свойства веб-приложения. |
> | Action | microsoft.web/sites/recommendationhistory/read | Возвращает журнал рекомендаций для веб-приложения. |
> | Action | microsoft.web/sites/recommendations/disable/action | Отключает рекомендации для веб-приложений. |
> | Action | Microsoft.Web/sites/recommendations/Read | Возвращает список рекомендаций для веб-приложения. |
> | Action | microsoft.web/sites/recover/action | Восстановление веб-приложений. |
> | Action | Microsoft.Web/sites/resetSlotConfig/Action | Сбрасывает конфигурацию веб-приложения. |
> | Action | microsoft.web/sites/resourcehealthmetadata/read | Получение метаданных работоспособности ресурсов веб-приложений. |
> | Action | Microsoft.Web/sites/restart/Action | Перезапускает веб-приложение. |
> | Action | microsoft.web/sites/restore/read | Возвращает данные о восстановлении веб-приложений. |
> | Action | microsoft.web/sites/restore/write | Восстановление веб-приложений. |
> | Action | microsoft.web/sites/restorefrombackupblob/action | Восстановление веб-приложения из большого двоичного объекта резервных копий. |
> | Action | Microsoft. Web/Sites/ресторефромделетедапп/Action | Восстановление веб-приложений из удаленного приложения. |
> | Action | microsoft.web/sites/restoresnapshot/action | Восстановление моментальных снимков веб-приложений. |
> | Action | microsoft.web/sites/siteextensions/delete | Удаление расширений сайта веб-приложений. |
> | Action | microsoft.web/sites/siteextensions/read | Получение расширений сайта веб-приложений. |
> | Action | microsoft.web/sites/siteextensions/write | Обновление расширений сайта веб-приложений. |
> | Action | microsoft.web/sites/slots/analyzecustomhostname/read | Возвращает результаты анализа пользовательского имени узла для слотов веб-приложений. |
> | Action | Microsoft.Web/sites/slots/applySlotConfig/Action | Применяет конфигурацию слота веб-приложения из целевого слота к текущему слоту. |
> | Action | Microsoft.Web/sites/slots/backup/Action | Создает резервную копию слота веб-приложения. |
> | Action | microsoft.web/sites/slots/backup/read | Получение резервной копии слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/backup/write | Обновляет резервную копию слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/backups/action | Обнаружение резервных копий слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/backups/delete | Удаление резервной копии слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/backups/list/action | Выводит список резервных копий слотов веб-приложений. |
> | Action | Microsoft.Web/sites/slots/backups/Read | Возвращает свойства резервной копии слотов веб-приложения. |
> | Action | microsoft.web/sites/slots/backups/restore/action | Восстанавливает резервные копии слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/config/delete | Удаляет конфигурацию слотов веб-приложений. |
> | Action | Microsoft.Web/sites/slots/config/list/Action | Отображает влияющие на безопасность параметры слота веб-приложения, такие как учетные данные для публикации, параметры приложения и строки подключения. |
> | Action | Microsoft.Web/sites/slots/config/Read | Возвращает параметры конфигурации слота веб-приложения. |
> | Action | Microsoft.Web/sites/slots/config/Write | Обновляет параметры конфигурации слота веб-приложения. |
> | Action | microsoft.web/sites/slots/containerlogs/action | Получение сжатых журналов контейнеров для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/containerlogs/download/action | Скачивание журналов контейнера слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/delete | Удаляет непрерывные веб-задания слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/read | Возвращает непрерывные веб-задания слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/start/action | Запускает непрерывные веб-задания слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/continuouswebjobs/stop/action | Останавливает непрерывные веб-задания слотов веб-приложений. |
> | Action | Microsoft.Web/sites/slots/Delete | Удаляет существующий слот веб-приложения. |
> | Action | microsoft.web/sites/slots/deployments/delete | Удаляет развертывания слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/deployments/log/read | Возвращает журнал развертываний слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/deployments/read | Возвращает развертывания слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/deployments/write | Обновляет развертывания слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/detectors/read | Возвращение средств обнаружения слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/analyses/execute/Action | Запуск анализа данных диагностики слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/analyses/read | Получение анализа данных диагностики слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/aspnetcore/read | Получение данных диагностики слотов для веб-приложений для приложения ASP.NET Core. |
> | Action | microsoft.web/sites/slots/diagnostics/autoheal/read | Получение данных диагностики слотов о функции Autoheal для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/deployment/read | Получение данных диагностики слотов о развертывании для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/deployments/read | Получение данных диагностики слотов о развертываниях для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/detectors/execute/Action | Запуск средства обнаружения данных диагностики слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/detectors/read | Получение сведений средства обнаружения данных диагностики слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/frebanalysis/read | Возвращение данных диагностики об анализе FREB для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/loganalyzer/read | Получение данных диагностики слотов для веб-приложений об анализаторе журналов. |
> | Action | microsoft.web/sites/slots/diagnostics/read | Получение данных диагностики слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/runtimeavailability/read | Получение данных диагностики слотов о доступности среды выполнения для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/servicehealth/read | Получение данных диагностики слотов о работоспособности служб для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/sitecpuanalysis/read | Получение данных диагностики слотов об анализе ЦП сайта для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/sitecrashes/read | Получение данных диагностики слотов о сбоях сайта для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/sitelatency/read | Получение данных диагностики слотов о задержке сайта для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/sitememoryanalysis/read | Получение данных диагностики слотов об анализе памяти сайта для веб-приложений. |
> | Action | microsoft.web/sites/slots/diagnostics/siterestartsettingupdate/read | Получение данных диагностики слотов для веб-приложений об обновлении параметра перезапуска сайта. |
> | Action | microsoft.web/sites/slots/diagnostics/siterestartuserinitiated/read | Получение данных диагностики слотов для веб-приложений об инициированном пользователем перезапуске сайта. |
> | Action | microsoft.web/sites/slots/diagnostics/siteswap/read | Получение данных диагностики слотов для веб-приложений о переключении сайта. |
> | Action | microsoft.web/sites/slots/diagnostics/threadcount/read | Получение данных диагностики слотов для веб-приложений о количестве потоков. |
> | Action | microsoft.web/sites/slots/diagnostics/workeravailability/read | Получение данных диагностики слотов для веб-приложений о доступности рабочих процессов. |
> | Action | microsoft.web/sites/slots/diagnostics/workerprocessrecycle/read | Получение данных диагностики слотов для веб-приложений о перезапуске рабочих процессов. |
> | Action | microsoft.web/sites/slots/domainownershipidentifiers/read | Получение идентификаторов владения доменом слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/functions/listsecrets/action | Вывод списка секретов для функций слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/functions/read | Возвращение функций слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/hostnamebindings/delete | Удаляет привязки имен узлов для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/hostnamebindings/read | Возвращает привязки имен узлов для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/hostnamebindings/write | Обновляет привязки имен узлов для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/hybridconnection/delete | Удаляет гибридное подключение для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/hybridconnection/read | Возвращает гибридное подключение для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/hybridconnection/write | Обновляет гибридное подключение для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/delete | Удаление ретрансляторов в пространствах имен гибридных подключений слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/hybridconnectionnamespaces/relays/write | Обновление ретрансляторов в пространствах имен гибридных подключений слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/hybridconnectionrelays/read | Получение ретрансляторов гибридных подключений слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/instances/deployments/read | Возвращает развертывания экземпляры слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/instances/processes/delete | Удаление процессов экземпляров слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/instances/processes/read | Возвращает процессы экземпляры слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/instances/read | Возвращает экземпляры слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/metricdefinitions/read | Возвращает определения метрик слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/metrics/read | Возвращает метрики слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/migratemysql/read | Получение сведений о переносе слотов для веб-приложений MySQL. |
> | Action | microsoft.web/sites/slots/networktrace/action | Трассировка сети слотов для веб-приложений. |
> | Action | Microsoft. Web/Sites/Slots/нетворктрацес/оператионресултс/Read | Возвращает результаты операции трассировки сети для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/newpassword/action | Задает новый пароль для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/operationresults/read | Возвращает результаты операций слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/operations/read | Получение операций слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/perfcounters/read | Получение счетчиков производительности слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/phplogging/read | Возвращает сведения о ведении журнала PHP для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/premieraddons/delete | Удаляет надстройки Premier для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/premieraddons/read | Возвращает надстройки Premier для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/premieraddons/write | Обновляет надстройки Premier для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/processes/read | Получение процессов слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/publiccertificates/delete | Удаление открытых сертификатов слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/publiccertificates/read | Получение открытых сертификатов слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/publiccertificates/write | Создание или обновление открытых сертификатов слотов для веб-приложений. |
> | Action | Microsoft.Web/sites/slots/publish/Action | Публикует слот веб-приложения. |
> | Action | Microsoft.Web/sites/slots/publishxml/Action | Возвращает XML-файл профиля публикации для слота веб-приложения. |
> | Action | Microsoft.Web/sites/slots/Read | Возвращает свойства слота развертывания веб-приложения. |
> | Action | microsoft.web/sites/slots/recover/action | Восстановление слотов веб-приложений. |
> | Action | Microsoft.Web/sites/slots/resetSlotConfig/Action | Сбрасывает конфигурацию слота веб-приложения. |
> | Action | microsoft.web/sites/slots/resourcehealthmetadata/read | Получение метаданных работоспособности ресурса слотов для веб-приложений. |
> | Action | Microsoft.Web/sites/slots/restart/Action | Перезапускает слот веб-приложения. |
> | Action | microsoft.web/sites/slots/restore/read | Возвращает данные о восстановлении слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/restore/write | Восстановление слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/restorefrombackupblob/action | Восстановление слота веб-приложений из большого двоичного объекта резервных копий. |
> | Action | Microsoft. Web/Sites/Slots/ресторефромделетедапп/Action | Восстановление слотов веб-приложений из удаленного приложения. |
> | Action | microsoft.web/sites/slots/restoresnapshot/action | Восстановление моментальных снимков слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/siteextensions/delete | Удаление расширений сайта слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/siteextensions/read | Получение расширений сайта слотов для веб-приложений. |
> | Action | microsoft.web/sites/slots/siteextensions/write | Обновление расширений сайта слотов для веб-приложений. |
> | Action | Microsoft.Web/sites/slots/slotsdiffs/Action | Возвращает различия конфигураций веб-приложения и слотов. |
> | Action | Microsoft.Web/sites/slots/slotsswap/Action | Переключает слоты развертывания веб-приложения. |
> | Action | microsoft.web/sites/slots/snapshots/read | Получение моментальных снимков слотов для веб-приложений. |
> | Action | Microsoft.Web/sites/slots/sourcecontrols/Delete | Удаляет параметры конфигурации системы управления версиями для слота веб-приложения. |
> | Action | Microsoft.Web/sites/slots/sourcecontrols/Read | Возвращает параметры конфигурации системы управления версиями для слота веб-приложения. |
> | Action | Microsoft.Web/sites/slots/sourcecontrols/Write | Обновляет параметры конфигурации системы управления версиями для слота веб-приложения. |
> | Action | Microsoft.Web/sites/slots/start/Action | Запускает слот веб-приложения. |
> | Action | Microsoft.Web/sites/slots/stop/Action | Останавливает слот веб-приложения. |
> | Action | microsoft.web/sites/slots/sync/action | Синхронизирует слоты веб-приложений. |
> | Action | microsoft.web/sites/slots/triggeredwebjobs/delete | Удаляет активированные веб-задания слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/triggeredwebjobs/read | Возвращает активированные веб-задания слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/triggeredwebjobs/run/action | Выполняет активированные веб-задания слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/usages/read | Возвращает данные об использовании слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/delete | Удаляет подключения виртуальных сетей для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/gateways/write | Обновляет шлюзы подключений виртуальных сетей для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/read | Возвращает подключения виртуальных сетей для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/virtualnetworkconnections/write | Обновляет подключения виртуальных сетей для слотов веб-приложений. |
> | Action | microsoft.web/sites/slots/webjobs/read | Возвращает веб-задания слотов веб-приложений. |
> | Action | Microsoft.Web/sites/slots/Write | Создает новый слот веб-приложения или обновляет существующий. |
> | Action | Microsoft.Web/sites/slotsdiffs/Action | Возвращает различия конфигураций веб-приложения и слотов. |
> | Action | Microsoft.Web/sites/slotsswap/Action | Переключает слоты развертывания веб-приложения. |
> | Action | microsoft.web/sites/snapshots/read | Возвращает моментальные снимки веб-приложений. |
> | Action | Microsoft.Web/sites/sourcecontrols/Delete | Удаляет параметры конфигурации системы управления версиями веб-приложения. |
> | Action | Microsoft.Web/sites/sourcecontrols/Read | Возвращает параметры конфигурации системы управления версиями веб-приложения. |
> | Action | Microsoft.Web/sites/sourcecontrols/Write | Обновляет параметры конфигурации системы управления версиями веб-приложения. |
> | Action | Microsoft.Web/sites/start/Action | Запускает веб-приложение. |
> | Action | Microsoft.Web/sites/stop/Action | Останавливает веб-приложение. |
> | Action | microsoft.web/sites/sync/action | Синхронизирует веб-приложения. |
> | Action | microsoft.web/sites/syncfunctiontriggers/action | Активация функции синхронизации для веб-приложений. |
> | Action | microsoft.web/sites/triggeredwebjobs/delete | Удаляет активированные веб-задания веб-приложений. |
> | Action | microsoft.web/sites/triggeredwebjobs/history/read | Получение истории активированных веб-заданий для веб-приложений. |
> | Action | microsoft.web/sites/triggeredwebjobs/read | Возвращает активированные веб-задания веб-приложений. |
> | Action | microsoft.web/sites/triggeredwebjobs/run/action | Выполняет активированные веб-задания веб-приложений. |
> | Action | microsoft.web/sites/usages/read | Возвращает данные об использовании веб-приложений. |
> | Action | microsoft.web/sites/virtualnetworkconnections/delete | Удаляет подключения виртуальных сетей веб-приложений. |
> | Action | microsoft.web/sites/virtualnetworkconnections/gateways/read | Возвращает шлюзы подключений виртуальных сетей веб-приложений. |
> | Action | microsoft.web/sites/virtualnetworkconnections/gateways/write | Обновляет шлюзы подключений виртуальных сетей веб-приложений. |
> | Action | microsoft.web/sites/virtualnetworkconnections/read | Возвращает подключения виртуальных сетей веб-приложений. |
> | Action | microsoft.web/sites/virtualnetworkconnections/write | Обновляет подключения виртуальных сетей веб-приложений. |
> | Action | microsoft.web/sites/webjobs/read | Возвращает веб-задания веб-приложений. |
> | Action | Microsoft.Web/sites/Write | Создает новое веб-приложение или обновляет существующее. |
> | Action | microsoft.web/skus/read | Возвращает номера SKU. |
> | Action | microsoft.web/sourcecontrols/read | Возвращает систему управления версиями. |
> | Action | microsoft.web/sourcecontrols/write | Обновляет системы управления версиями. |
> | Action | microsoft.web/unregister/action | Отменяет регистрацию поставщика ресурсов Microsoft.Web для подписки. |
> | Action | microsoft.web/validate/action | Выполняет проверку. |
> | Action | microsoft.web/verifyhostingenvironmentvnet/action | Проверка виртуальной сети среды размещения. |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tdCol2BreakAll"]
> | Тип действия | Операция | Описание |
> | --- | --- | --- |
> | Action | Microsoft.WorkloadMonitor/components/read | Возвращает компоненты ресурса. |
> | Action | Microsoft.WorkloadMonitor/componentsSummary/read | Возвращает сводки компонентов. |
> | Action | Microsoft.WorkloadMonitor/monitorInstances/read | Получает экземпляры мониторов ресурса. |
> | Action | Microsoft.WorkloadMonitor/monitorInstancesSummary/read | Получает сводку экземпляров монитора. |
> | Action | Microsoft.WorkloadMonitor/monitors/read | Получает мониторы ресурсов. |
> | Action | Microsoft.WorkloadMonitor/monitors/write | Настойка монитора ресурса. |
> | Action | Microsoft.WorkloadMonitor/notificationSettings/read | Получает параметры уведомлений для ресурса. |
> | Action | Microsoft.WorkloadMonitor/notificationSettings/write | Настройка параметров уведомлений для ресурса. |
> | Action | Microsoft.WorkloadMonitor/operations/read | Получает поддерживаемые операции. |

## <a name="next-steps"></a>Следующие шаги

- [Пользовательские роли для ресурсов Azure](custom-roles.md)
- [Встроенные роли управления доступом на основе ролей в Azure](built-in-roles.md)
