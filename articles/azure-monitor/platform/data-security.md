---
title: Защита данных Log Analytics | Документация Майкрософт
description: Узнайте, как Log Analytics сохраняет вашу конфиденциальность и защищает данные.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: magoedte
ms.openlocfilehash: dd4efcd2f1d4cbf497ad1fde6936088513cb5fd0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60759950"
---
# <a name="log-analytics-data-security"></a>Защита данных Log Analytics
В этом документе описываются функции Azure Log Analytics, компонента службы Azure Monitor, для дополнения информации о [центре управления безопасностью Azure](../../security/security-microsoft-trust-center.md).  

В этой статье описываются сбор, обработка и защита данных Log Analytics. Вы можете подключиться к веб-службе с помощью агентов, собирать рабочие данные с помощью System Center Operations Manager и получать данные системы диагностики Azure с помощью Log Analytics. 

Служба Log Analytics безопасно управляет облачными данными с использованием следующих методов:

* Разделение данных
* Хранение данных
* Физическая безопасность
* Управление инцидентами
* Соответствие требованиям
* Соответствие сертификатам и стандартам безопасности

Отправляйте свои вопросы, предложения и сообщения о неполадках, связанные с представленными ниже сведениями, включая наши политики защиты, в [службу поддержки Azure](https://azure.microsoft.com/support/options/).

## <a name="sending-data-securely-using-tls-12"></a>Безопасная отправка данных с помощью TLS 1.2 

Чтобы обеспечить безопасность данных, передаваемых в Log Analytics, настоятельно рекомендуем настроить для агента использование протокола TLS как минимум версии 1.2. Более старые версии протоколов TLS/SSL оказались уязвимы. Хотя они все еще используются для обеспечения обратной совместимости, применять их **не рекомендуется**, так как представители отрасли стремятся как можно скорее отказаться от их поддержки. 

[Совет по стандартам безопасности PCI](https://www.pcisecuritystandards.org/) установил [крайний срок (30 июня 2018 года)](https://www.pcisecuritystandards.org/pdfs/PCI_SSC_Migrating_from_SSL_and_Early_TLS_Resource_Guide.pdf) для отказа от старых версий протоколов TLS и SSL и перехода на более безопасные протоколы. Как только Azure прекратит поддержку предыдущих версий, агенты, не поддерживающие протокол TLS версии минимум 1.2, не смогут отправлять данные в Log Analytics. 

Рекомендуем по возможности не настраивать в агенте работу только с протоколом TLS 1.2, так как это может на уровне платформы нарушить работу функций безопасности, которые позволяют автоматически обнаруживать и использовать новые, более безопасные протоколы по мере их появления, например TLS 1.3. 

### <a name="platform-specific-guidance"></a>Поиск руководств в соответствии с платформой

|Платформа или язык | Поддержка | Дополнительные сведения |
| --- | --- | --- |
|Linux | Как правило, дистрибутивы Linux для поддержки протокола TLS 1.2 используют [OpenSSL](https://www.openssl.org).  | Убедитесь, что ваша версия OpenSSL поддерживается, проверив [журнал изменений OpenSSL](https://www.openssl.org/news/changelog.html).|
| Windows 8.0–10 | Поддерживается и включена по умолчанию. | Убедитесь, что вы все еще используете [параметры по умолчанию](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings).  |
| Windows Server 2012–2016 | Поддерживается и включена по умолчанию. | Убедитесь, что вы все еще используете [параметры по умолчанию](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings). |
| Windows 7 с пакетом обновления 1 и Windows Server 2008 R2 с пакетом обновления 1 | Поддерживается, но не включена по умолчанию. | Информацию о том, как ее включить, см. на странице [Transport Layer Security (TLS) registry settings](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings) (Параметры реестра TLS).  |

## <a name="data-segregation"></a>Разделение данных
После того, как ваши данные будут приняты службой Log Analytics, они будут логическим образом отделены для каждого компонента службы. Все данные отмечаются тегами по рабочим областям. Эти теги существуют в течение всего жизненного цикла данных и используются на каждом уровне службы. Данные хранятся в выделенной базе данных в кластере хранения в выбранном регионе.

## <a name="data-retention"></a>Хранение данных
Индексированные данные поиска по журналу хранятся и удерживаются согласно ценовому плану. Дополнительные сведения см. на странице [цен на Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/).

В рамках [соглашения о подписке](https://azure.microsoft.com/support/legal/subscription-agreement/) по его условиям корпорация Майкрософт будет сохранять данные.  Физические диски при этом не затрагиваются.  

В приведенной ниже таблице перечислены доступные решения и предоставляются примеры типов данных, которые они собирают.

| **Решение** | **Типы данных** |
| --- | --- |
| Capacity and Performance |Данные производительности и метаданные. |
| Управление обновлениями |Метаданные и данные о состоянии. |
| Управление журналами; |Пользовательские журналы событий, журналы событий Windows и журналы IIS. |
| Отслеживание изменений |Инвентаризация программного обеспечения, метаданные управляющей программы Linux и службы Windows, метаданные файлов Windows и Linux |
| Оценка SQL и Active Directory. |Данные WMI, данные реестра, данные производительности и результаты динамического управления SQL Server. |

В следующей таблице показаны примеры типов данных:

| **Тип данных** | **Поля** |
| --- | --- |
| Предупреждение |Alert Name, Alert Description, BaseManagedEntityId, Problem ID, IsMonitorAlert, RuleId, ResolutionState, Priority, Severity, Category, Owner, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Параметр Configuration |CustomerID, AgentID, EntityID, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Событие |EventId, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, Number, Category, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Примечание.** Log Analytics собирает данные событий с настраиваемыми полями при их записи в журнал событий Windows. |
| Метаданные |BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IPAddress, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP Address, NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Производительность |ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Состояние |StateChangeEventId, StateId, NewHealthState, OldHealthState, Context, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Физическая безопасность
Персонал корпорации Майкрософт управляет службой Log Analytics, а все действия записываются в журнал и доступны для аудита. Служба Log Analytics управляется в качестве службы Azure и соответствует всем нормативным требованиям Azure и требованиям безопасности. Вы можете просмотреть сведения о физической защите активов Azure на странице 18 [обзора безопасности Microsoft Azure](https://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Права физического доступа к защищенным областям меняются в течение одного рабочего дня для каждого клиента, который перестает нести ответственность за службу Log Analytics, в том числе за передачу данных и завершение операций. Вы также можете ознакомиться с [глобальной физической инфраструктурой, используемой в центрах обработки данных Майкрософт](https://azure.microsoft.com/global-infrastructure/).

## <a name="incident-management"></a>Управление инцидентами
В Log Analytics предусмотрен процесс управления инцидентами, которого придерживаются все службы Майкрософт. Он включает в себя следующие основные этапы:

* Использование модели разделения ответственности: частично ответственность за безопасность лежит на корпорации Майкрософт, а частично — на клиенте.
* Управление инцидентами безопасности Azure:
  * Начало расследования при обнаружении инцидента
  * Дежурный специалист группы реагирования на инциденты оценивает влияние и серьезность инцидента. Исходя из полученных данных, инцидент передается или не передается группе реагирования на инциденты для последующей оценки.
  * Ответственные специалисты диагностируют инцидент, чтобы провести техническое или экспертное расследование, определить стратегии сдерживания, устранения и решения. Если группа безопасности полагает, что к данным клиента был получен незаконный или несанкционированный доступ, в это же время будут предприняты действия для уведомления клиента об инциденте.  
  * Стабилизация и восстановление после инцидента. Группа реагирования на инциденты создает план восстановления, чтобы устранить проблему. Действия по сдерживанию проблемы, в частности помещение затронутых систем под карантин, могут быть предприняты немедленно и выполняться параллельно с диагностикой. Действия по устранению проблем могут также выполняться по окончании периода риска.  
  * Закрытие инцидента и последующий анализ. Группа реагирования на инциденты составляет отчет с подробными сведениями об инциденте для его последующего анализа. Цель этого — избежать повторения происшествия, изменив политики, процедуры и процессы.
* Уведомление клиентов об инцидентах безопасности:
  * Определяется ряд клиентов, которых затронул инцидент. Им предоставляется максимально подробная информация о случившемся.
  * На этом этапе для клиентов создается уведомление, содержащее достаточно подробные сведения для того, чтобы они могли изучить проблему со своей стороны и выполнить свои обязательства перед конечными пользователями, не задерживая процесс уведомления.
  * При необходимости инцидент подтверждается или объявляется.
  * Клиентов уведомляют об инциденте в соответствии с юридическими или договорными обязательствами без какой-либо необоснованной задержки. Клиенты получают уведомления об инцидентах безопасности через своих администраторов любым способом, выбранным корпорацией Майкрософт, включая электронную почту.
* Готовность и обучение команды:
  * Персонал корпорации Майкрософт проходит обязательное обучение в сфере обеспечения безопасности и обнаружения инцидентов, что помогает им выявлять потенциальные проблемы безопасности и сообщать о них.  
  * Операторы, работающие в службе Microsoft Azure, проходят дополнительное обучение, так как они имеют доступ к системам, содержащим данные клиента.
  * Группа Майкрософт по реагированию на инциденты получает обучение в соответствии с выполняемыми ролями.

Если какие-либо данные клиента были утеряны, он получает уведомление в течение одного дня. Но при использовании решения в службе данные не теряются. 

Дополнительные сведения о том, как корпорация Майкрософт реагирует на угрозы безопасности, см. в статье [Microsoft Azure Security Response in the Cloud](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/4/Microsoft%20Azure%20Security%20Response%20in%20the%20cloud.pdf) (Реагирование на нарушения безопасности в облаке Microsoft Azure).

## <a name="compliance"></a>Соответствие требованиям
Программа информационной безопасности и стратегического управления группы разработки и обслуживания программного обеспечения Log Analytics отвечает бизнес-требованиям и придерживается правил и нормативных требований, описанных на страницах, касающихся [центра управления безопасностью Microsoft Azure](https://azure.microsoft.com/support/trust-center/) и [центра соответствия требованиям Майкрософт](https://www.microsoft.com/en-us/trustcenter/compliance/default.aspx). Там также описано, как Log Analytics устанавливает требования безопасности, определяет средства управления безопасностью, управляет рисками и отслеживает их. Мы ежегодно пересматриваем политики, стандарты, процедуры и рекомендации.

Каждый участник команды разработки проходит официальное обучение тому, как обеспечивать безопасность приложений. На внутреннем уровне для разработки программного обеспечения используется система контроля версий. Каждый проект программного обеспечения защищен этой системой.

В корпорации Майкрософт работает команда по обеспечению безопасности и соответствия требованиям, которая контролирует и оценивает все службы Майкрософт. Специалисты по информационной безопасности работают в командах, которые не связаны с командами разработчиков Log Analytics. Чтобы обеспечить безопасность и соответствие требованиям, сотрудники службы безопасности используют собственную управленческую цепочку и проводят независимые оценки продуктов и служб.

Совету директоров корпорации Майкрософт сообщают обо всех программах информационной безопасности в годовом отчете.

Команда разработки и обслуживания Log Analytics активно сотрудничает с юридическим отделом, командой обеспечения соответствия требованиям корпорации Майкрософт и другими отраслевыми партнерами для получения разных сертификатов.

## <a name="certifications-and-attestations"></a>Сертификации и аттестации
Azure Log Analytics соответствует следующим требованиям:

* [ISO/IEC 27001](https://www.iso.org/iso/home/standards/management-standards/iso27001.htm);
* [ISO/IEC 27018:2014](https://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/blog/iso22301/)
* [Стандарт безопасности данных индустрии платежных карт (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI), разработанный Советом по стандартам безопасности индустрии платежных карт;
* [элементы управления организацией служб (SOC) 1 типа 1 и SOC 2 типа 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2);
* требования [HIPAA и HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/hipaa) для компаний, заключивших Соглашение с бизнес-партнерами HIPAA.
* Общие условия проектирования для Windows
* Защищенные информационные системы Майкрософт
* как служба Azure, Log Analytics использует компоненты, которые отвечают нормативным требованиям Azure. Дополнительные сведения см. на странице [центра соответствия требованиям Майкрософт](https://www.microsoft.com/en-us/trustcenter/compliance/default.aspx).

> [!NOTE]
> В некоторых сертификациях и аттестациях служба Log Analytics указывается под прежним названием, *Operational Insights*.
>
>

## <a name="cloud-computing-security-data-flow"></a>Безопасность потока данных для облачных вычислений
На следующей схеме показана архитектура защиты облачных данных в виде потока информации от вашей компании и его защиты по мере перемещения в службу Log Analytics до тех пор, пока вы, наконец, не увидите эти данные на портале Azure. Под схемой приведена дополнительная информация о каждом шаге.

![Иллюстрация сбора и защиты данных в Log Analytics](./media/data-security/log-analytics-data-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. Регистрация в Log Analytics и сбор данных
Чтобы организация отправила данные в Log Analytics, настройте агент Windows или Linux, выполняемый на виртуальных машинах Azure или на физических компьютерах в среде или другом поставщике облака.  При использовании Operations Manager из группы управления можно настроить агент Operations Manager. Пользователям (вам, другим индивидуальным пользователям или группе людей) необходимо создать одну или несколько рабочих областей Log Analytics и зарегистрировать агентов с помощью одной из следующих учетных записей:

* [идентификатор организации;](../../active-directory/fundamentals/sign-up-organization.md)
* [Учетная запись Майкрософт — Outlook, Office Live, MSN.](https://account.microsoft.com/account)

Рабочая область Log Analytics — это место, где данные собираются, вычисляются, анализируются и представляются. Рабочая область в основном используется для секционирования данных, и каждая из них является уникальной. Например, с помощью одной рабочей области можно управлять рабочими данными, а с помощью другой — тестовыми. Рабочие области также помогают администратору управлять доступом пользователей к данным. С каждой рабочей областью может быть связано несколько учетных записей пользователей, и у каждой учетной записи пользователя может быть несколько рабочих областей Log Analytics. Рабочие области создаются на основе региона центра обработки данных.

Группа управления Operations Manager устанавливает подключение к службе Log Analytics. Затем вы настраиваете системы, управляемые агентами в группе управления, которым разрешено собирать и отправлять данные в службу. В зависимости от включенных решений данные с этих решений отправляются непосредственно из сервера управления Operations Manager в службу Log Analytics или непосредственно из агента в службу, что зависит от объема данных, собранных в управляемой агентом системе. Все системы, не отслеживаемые Operations Manager, безопасно подключаются к службе Log Analytics напрямую.

Все данные, передаваемые между подключенными системами и службой Log Analytics, шифруются. Для шифрования используется протокол TLS (HTTPS).  Благодаря соблюдению жизненного цикла разработки защищенных приложений (Майкрософт) служба Log Analytics соответствует последним усовершенствованиям протоколов шифрования.

Каждый тип агента собирает данные для Log Analytics. Тип собираемых данных зависит от типов используемых решений. Сведения о сборе данных см. в статье [Добавление решений Log Analytics из коллекции решений](../../azure-monitor/insights/solutions.md). Кроме того, подробные сведения о сборе доступны для большинства решений. Решением является набор заранее определенных представлений, запросов поиска в журналах, правил сбора данных и логики обработки. Для импорта решения Log Analytics могут использовать только администраторы. После импорта решение перемещается на серверы управления Operations Manager (если они используются), а затем — на любые выбранные агенты. После этого агенты собирают данные.

## <a name="2-send-data-from-agents"></a>2. Отправка данных от агентов
Все агенты регистрируются с помощью ключа регистрации. После этого между агентом и службой Log Analytics устанавливается безопасное подключение с помощью проверки подлинности на основе сертификата и SSL с портом 443. Для создания и обслуживания ключей в Log Analytics используется секретное хранилище. Закрытые ключи меняются каждые 90 дней, хранятся в Azure и управляются с помощью операций Azure согласно строгим рекомендациям соответствия нормативам и требованиям.

В Operations Manager группа управления, зарегистрированная в рабочей области Log Analytics, устанавливает безопасное подключение HTTPS с сервером управления Operations Manager.

Агенты Windows или Linux, работающие на виртуальных машинах Azure, используют ключ к хранилищу данных, доступный в режиме только для чтения, для чтения событий диагностики из таблиц Azure.  

Агенты сообщают группе управления Operations Manager, интегрированной с Log Analytics, поэтому если сервер управления не может связаться со службой по какой-либо причине, собранные данные будут храниться локально во временном кэше на сервере управления.   Они будут пытаться повторно отправить данные каждые две минуты в течение двух часов.  Поведение данных, которые обходят сервер управления и отправляются непосредственно в Log Analytics, совместимо с агентом Windows.  

Кэшированные данные Windows или агента сервера управления защищены хранилищем учетных данных операционной системы. Если в течение 2 часов служба не может обработать данные, агенты помещают их в очередь. Если очередь переполняется, агент начинает удалять данные по типам, начиная с данных производительности. Предел очереди агента — это раздел реестра. При необходимости его можно изменить. Собранные данные сжимаются и отправляются в службу, минуя группу управления Operations Manager базы данных, то есть к ним не применяется никакой дополнительной нагрузки. После отправки собранных данных они удаляются из кэша.

Как описано выше, данные с сервера управления или подключенных напрямую агентов отправляются по протоколу SSL в центры обработки данных Microsoft Azure. Для дополнительной защиты данных можно также использовать ExpressRoute. ExpressRoute — это способ напрямую подключаться к Azure из существующей глобальной сети, например MPLS VPN, предоставленной поставщиком сетевой службы. Дополнительные сведения см. в статье об [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. Служба Log Analytics получает и обрабатывает данные.
Служба Log Analytics обеспечивает надежность источника входящих данных, проверяя сертификаты и целостность данных с помощью проверки подлинности Azure. Необработанные данные затем сохраняются в концентраторе событий Azure в регионе, в котором данные будут в конечном итоге храниться как неактивные. Тип хранящихся данных зависит от типов решений, которые были импортированы и используется для сбора данных. Затем Log Analytics обрабатывает необработанные данные и принимает их в базу данных.

Период хранения собранных данных в базе данных зависит от выбранного тарифного плана. На *бесплатном* уровне собранные данные доступны в течение семи дней. На *платном* уровне собранные данные по умолчанию доступны в течение 31 дня, но этот период можно продлить до 730 дней. Данные хранятся в зашифрованном виде в службе хранилища Azure для обеспечения их конфиденциальности, а также реплицируются в локальном регионе с использованием локально избыточного хранилища (LRS). Последние две недели данных также хранятся в кэше на основе SSD и шифруется этот кэш.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Использование Log Analytics для доступа к данным
Чтобы получить доступ в рабочую область Log Analytics, войдите на портал Azure с помощью учетной записи организации или учетной записи Майкрософт, настроенной ранее. Весь трафик между порталом и Log Analytics в службе отправляется через защищенный канал HTTPS. При использовании портала идентификатор сеанса создается в клиенте пользователя (веб-браузер), а данные хранятся в локальном кэше до завершения сеанса. После завершения сеанса кэш удаляется. Файлы cookie со стороны клиента, не содержащие сведений, по которым можно установить личность, не удаляются автоматически. Файлы cookie сеанса помечены как HTTPOnly и защищены. По истечении предопределенного периода простоя сеанс работы с порталом Azure прерывается.

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения по сбору данных с помощью Log Analytics для виртуальных машин Azure см. в [руководстве по виртуальным машинам Azure](../../azure-monitor/learn/quick-collect-azurevm.md).  

*  Если вы хотите собирать данные с физических или виртуальных компьютеров Windows и Linux в своей среде, ознакомьтесь со статьей [Сбор данных с компьютеров Linux, размещенных в вашем окружении](../../azure-monitor/learn/quick-collect-linux-computer.md) и [Сбор данных с компьютеров Windows, размещенных в вашей среде](../../azure-monitor/learn/quick-collect-windows-computer.md).

