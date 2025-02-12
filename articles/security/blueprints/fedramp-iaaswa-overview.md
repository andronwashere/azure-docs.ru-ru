---
title: Схема безопасности и соответствия требованиям Azure. Веб-приложение IaaS для Федеральной программы аккредитации облачных служб
description: Схема безопасности и соответствия требованиям Azure. Веб-приложение IaaS для Федеральной программы аккредитации облачных служб
services: security
documentationcenter: na
author: jomolesk
manager: barbkess
editor: tomsh
ms.assetid: 8fe47cff-9c24-49e0-aa11-06ff9892a468
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2018
ms.author: jomolesk
ms.openlocfilehash: 1ba5b813843ce2f5d31f337ab4d3d94e521b0e0c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60586140"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-fedramp"></a>Схема безопасности и соответствия требованиям Azure. Веб-приложение IaaS для FedRAMP

## <a name="overview"></a>Обзор

[Федеральная программа аккредитации облачных служб (FedRAMP)](https://www.fedramp.gov) — государственная программа США, которая определяет стандартизированный подход к оценке безопасности, сертификации и постоянному мониторингу облачных продуктов и услуг. В этой статье об автоматизации схемы безопасности и соответствия требованиям Azure приведены инструкции по развертыванию совместимой с FedRAMP среды IaaS, подходящей для создания простого интернет-приложения. Это решение автоматизирует развертывание и конфигурацию ресурсов Azure для общей эталонной архитектуры, демонстрируя, как клиенты могут обеспечить соответствие требованиям к безопасности и соответствию. Оно также служит основой для создания и настройки решений в Azure. В решении реализовано несколько директив из базовых требований FedRAMP уровня High на основе стандартов NIST SP 800-53. Дополнительные сведения о требованиях FedRAMP и этом решении см. в [документации по соответствию требованиям](#compliance-documentation).
> [!NOTE]
> Это решение развертывается в Azure для государственных организаций.

Решение автоматизации безопасности и соответствия требованиям Azure автоматически развертывает эталонную архитектуру веб-приложения IaaS с предварительно настроенными элементами управления безопасностью, чтобы помочь клиентам обеспечить соответствие требованиям FedRAMP. Решение состоит из шаблонов Azure Resource Manager и сценариев PowerShell, позволяющих развернуть и настроить ресурсы.

Эта архитектура призвана служить основой, которую клиенты должны адаптировать для конкретных требований, и не должна использоваться "как есть" в рабочей среде. Развертывания приложения в этой среде без изменений недостаточно для полного соответствия базовым требованиям FedRAMP уровня High. Обратите внимание на следующее:
- Эта архитектура является своего рода эталоном, который помогает клиентам использовать Azure в соответствии с требованиями FedRAMP.
- Клиенты несут ответственность за проведение соответствующих проверок безопасности и соответствия внутренней оценки решения, созданного с использованием этой архитектуры, поскольку требования могут варьироваться в зависимости от специфики реализации каждого клиента.

Краткий обзор того, как работает это решение, содержится в этом [видео](https://aka.ms/fedrampblueprintvideo). В нем объясняется и демонстрируется развертывание решения.

Инструкции по развертыванию см. [здесь](https://aka.ms/fedrampblueprintrepo).

## <a name="architecture-diagram-and-components"></a>Схема и компоненты архитектуры
Это решение позволяет развернуть эталонную архитектуру для веб-приложения IaaS с внутренним сервером SQL Server. Архитектура включает веб-уровень, уровень данных, инфраструктуру Active Directory, шлюз приложений и Load Balancer. Виртуальные машины, развернутые на веб-уровне и уровне данных, настраиваются в группе доступности, а экземпляры SQL Server — в группе доступности AlwaysOn. Это обеспечивает высокий уровень доступности. Виртуальные машины присоединены к доменам, а с помощью политик группы Active Directory обеспечивается безопасность и соответствие конфигурациям на уровне операционной системы. Узел-бастион предоставляет безопасное подключение для администраторов, позволяющее получить доступ к развернутым ресурсам. **Azure рекомендует настроить подключение VPN или Azure ExpressRoute для управления и импорта данных в подсеть эталонной архитектуры.**

![Схема эталонной архитектуры веб-приложения IaaS для FedRAMP](images/fedramp-iaaswa-architecture.png?raw=true "IaaS Web Application for FedRAMP reference architecture diagram")

Это решение использует следующие службы Azure. Подробные сведения об архитектуре развертывания см. в [этом разделе](#deployment-architecture).

- Виртуальные машины Azure
    - (1) узел-бастион (Windows Server 2016 Datacenter);
    - (2) Контроллер домена Active Directory (Windows Server 2016 Datacenter).
    - (2) узел кластера SQL Server (SQL Server 2017 в Windows Server 2016);
    - (2) Веб-службы и службы IIS (Windows Server 2016 Datacenter).
- Группы доступности
    - (1) Контроллеры домена Active Directory.
    - (1) узлы кластера SQL;
    - (1) Веб-службы и службы IIS.
- Виртуальная сеть Azure
    - (1) Виртуальные сети размером в /16.
    - (5) Подсети размером в /24.
    - Параметры DNS настроены на двух контроллерах доменов.
- Azure Load Balancer
- Шлюз приложений Azure
    - (1) Включенный шлюз приложений с брандмауэром веб-приложения:
        - режим брандмауэра — предотвращение;
        - Набор правил: OWASP 3.0
        - прослушиватель — порт 443.
- Хранилище Azure
    - (7) Учетная запись хранения для геоизбыточного хранилища.
- Облако-свидетель Azure.
- Хранилище Служб восстановления
- Хранилище ключей Azure
- Azure Active Directory (Azure AD)
- Azure Resource Manager
- Azure Monitor (журналы)

## <a name="deployment-architecture"></a>Архитектура развертывания

В следующем разделе подробно описываются компоненты разработки и реализации.

**Узел-бастион**. Узел-бастион является единой точкой входа, которая предоставляет защищенное подключение для администраторов, позволяющее получить доступ к развернутым ресурсам. Группа безопасности сети для узла-бастиона разрешает подключения только к TCP-порту 3389 для RDP. В дальнейшем клиенты могут настроить узел-бастион в соответствии с требованиями системы в организации.

### <a name="virtual-network"></a>Виртуальная сеть
Архитектура определяет частную виртуальную сеть с пространством адресов 10.200.0.0/16.

**Группы безопасности сети**. Решение развертывает ресурсы в архитектуре с отдельной веб-подсетью, подсетью базы данных, подсетью Active Directory и подсетью управления внутри виртуальной сети. Подсети логически разделены правилами групп безопасности сети, применяемыми к отдельным подсетям, которые настроены таким образом, чтобы между подсетями проходил только трафик, который необходим для работы функций системы и управления.

См. конфигурацию [групп безопасности сети](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json), развертываемых с этим решением. Клиенты могут настроить группы безопасности сети. Для этого нужно изменить файл, приведенный выше, руководствуясь инструкциями в [этой документации](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg).

Каждая из подсетей имеет выделенную группу безопасности сети (NSG):
- 1 NSG для шлюза приложений (LBNSG);
- 1 NSG для узла-бастиона (MGTNSG);
- 1 NSG для основного и резервного контроллера домена (ADNSG);
- 1 NSG для серверов SQL Server (SQLNSG);
- 1 NSG для веб-уровня (WEBNSG).

**Подсети**. Каждая подсеть связана с соответствующей NSG.

### <a name="data-at-rest"></a>Неактивные данные

Архитектура защищает неактивные данные с помощью нескольких мер шифрования.

**Служба хранилища Azure**. Для удовлетворения требований к шифрованию неактивных данных все учетные записи хранения используют [шифрование службы хранения](https://docs.microsoft.com/azure/storage/common/storage-service-encryption).

**SQL Server**. SQL Server настроен для использования [прозрачного шифрования данных (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption), которое обеспечивает шифрование и расшифровку файлов данных и журналов в реальном времени для защиты неактивных данных. TDE позволяет защитить сохраненные данные от несанкционированного доступа.

Клиенты также могут настроить следующие меры безопасности SQL Server:
-   [Аутентификация и авторизация AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) обеспечивает централизованное управление удостоверениями пользователей базы данных и другими службами Майкрософт.
-   [Аудит базы данных SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) позволяет отслеживать события базы данных и записывать их в журнал аудита в учетной записи хранения Azure.
-   [Правила брандмауэра](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) предотвращают любой доступ к серверам баз данных без надлежащих разрешений. Брандмауэр предоставляет доступ к базам данным на основе исходного IP-адреса каждого запроса.
-   [Обнаружение угроз SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) позволяет немедленно выявлять потенциальные угрозы и реагировать на них. При этом поступают оповещения системы безопасности о подозрительных действиях с базами данных, потенциальных уязвимостях, атаках путем внедрения кода SQL и аномальных шаблонах доступа к базам данных.
-   [Столбцы Always Encrypted](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) гарантируют, что конфиденциальные данные никогда не отобразятся как открытый текст внутри системы баз данных. После включения шифрования данных получить доступ к данным в виде открытого текста смогут только клиентские приложения и серверы приложений, у которых есть доступ к ключам.
-   После развертывания эталонной архитектуры можно выполнить [динамическое маскирование данных в базе данных SQL](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started). Клиентам потребуется настроить параметры динамического маскирования данных в соответствии со своей схемой базы данных.

**Шифрование дисков Azure**. Шифрование дисков Azure применяется для шифрования дисков Windows в виртуальных машинах IaaS. [Шифрование дисков Azure](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) использует возможность BitLocker Windows, чтобы обеспечить шифрование тома для ОС и дисков данных. Решение интегрируется с хранилищем ключей Azure, помогая управлять ключами шифрования дисков.

### <a name="identity-management"></a>Управление удостоверениями

Следующие технологии обеспечивают возможности управления удостоверениями в среде Azure:
- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) — это мультитенантный облачный каталог и служба управления удостоверениями корпорации Майкрософт.
- Аутентификация развернутого пользователем веб-приложения может выполняться с помощью Azure Active Directory. Дополнительные сведения см. в статье [Интеграция приложений с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  
- [Контроль доступа на основе ролей (RBAC) Azure](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) обеспечивает точное управление доступом для Azure. Доступ к подписке ограничен администратором подписки, а доступ к ресурсам можно ограничить с учетом роли пользователя.
- Развернутый экземпляр IaaS Active Directory обеспечивает управление удостоверениями на уровне операционной системы для развернутых виртуальных машин IaaS.

### <a name="security"></a>Безопасность
**Управление секретами**. Для управления ключами и секретами в решении используется [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). Хранилище ключей Azure помогает защитить криптографические ключи и секреты, используемые облачными приложениями и службами. Azure Key Vault обеспечивает управление секретами и ключами шифрования диска виртуальной машины IaaS для этой эталонной архитектуры.

**Управление исправлениями**. Виртуальные машины Windows, развернутые этим решением автоматизации схемы безопасности и соответствия требованиям Azure, по умолчанию настроены для получения автоматических обновлений из службы Центра обновления Windows. В этом решении также развертывается решение службы автоматизации Azure, с помощью которого при необходимости можно обновить развертывания исправлений на серверах Windows.

**Защита от вредоносных программ**. [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) для виртуальных машин Azure предоставляет возможности защиты в реальном времени для обнаружения и удаления вирусов, шпионских и других вредоносных программ. Вы можете настроить оповещения о попытках установки или выполнения вредоносных или нежелательных программ на защищенных виртуальных машинах.

**Шлюз приложений**. Архитектура снижает риск уязвимостей безопасности с помощью шлюза приложений с брандмауэром веб-приложения (WAF) и включенным набором правил OWASP. Доступны следующие дополнительные возможности:

- [сквозное SSL-подключение](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell);
- включение [разгрузки SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal);
- отключение [TLS версий 1.0 и 1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell);
- [брандмауэр веб-приложения](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (режим WAF);
- [режим предотвращения](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) с набором правил OWASP 3.0.

### <a name="business-continuity"></a>Непрерывность бизнес-процессов

**Высокий уровень доступности**. Во время планового или внепланового технического обслуживания доступна как минимум одна виртуальная машина, что соответствует соглашению об уровне обслуживания Azure, гарантирующему доступность в течение 99,95 % времени. Решение позволяет развернуть все виртуальные машины веб-уровня и уровня данных в [группе доступности](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Группа доступности распределяет виртуальные машины между несколькими изолированными аппаратными кластерами, чтобы повысить уровень доступности. Более того, это решение позволяет развернуть виртуальные машины SQL Server в группе доступности в качестве [группы доступности AlwaysOn](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview). Группа доступности Always On предоставляет возможности, обеспечивающие высокий уровень доступности, и возможности аварийного восстановления.

**Хранилище Служб восстановления**. [Хранилище Служб восстановления](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) содержит резервные копии данных и защищает все конфигурации виртуальных машин Azure в этой архитектуре. С помощью хранилища служб восстановления клиенты могут восстановить отдельные файлы и папки виртуальной машины IaaS, а не всю виртуальную машину. Это сокращает время восстановления.

**Облако-свидетель**. [Облако-свидетель](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) — это тип свидетеля кворума отказоустойчивого кластера в Windows Server 2016, использующий Azure в качестве точки разрешения конфликтов. Для облака-свидетеля, как и для других свидетелей кворума, предоставляется возможность голосования и участия в подсчетах кворума, но используется стандартное общедоступное хранилище BLOB-объектов Azure. Это позволяет избежать дополнительных затрат на обслуживание виртуальных машин, размещенных в общедоступном облаке.

### <a name="logging-and-auditing"></a>Ведение журналов и аудит

Журналы Azure Monitor обеспечивает детальную запись системных и пользовательских действий, а также о работоспособности системы. [Журналы Azure Monitor](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) решение собирает и анализирует данные, создаваемые ресурсами в Azure и локальных средах.

- **Журналы действий**.  [Журналы действий](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) предоставляют информацию об операциях, которые выполнялись с ресурсами в подписке. Журналы действий помогают определить инициатора операции, время события и состояние.
- **Журналы диагностики**.  [Журналы диагностики.](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) — это все журналы, создаваемые ресурсами. К этим журналам относятся системные журналы событий Windows, журналы службы хранилища Azure, журналы аудита Key Vault, а также журналы брандмауэра и доступа шлюза приложений.
- **Архивация журналов.**  Все журналы диагностики обеспечивают запись данных в централизованную и зашифрованную учетную запись Azure для архивирования. В соответствии с требованиями конкретной организации пользователь может настроить срок хранения до 730 дней. Эти журналы подключиться к Azure Monitor журналы для обработки, хранения и создания отчетов информационной панели.

Кроме того, в составе этой архитектуры устанавливаются указанные ниже решения мониторинга. Обратите внимание, что клиент отвечает за настройку этих решений в соответствии с мерами безопасности FedRAMP:
-   [Оценка AD](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment). Решение "Проверка работоспособности Active Directory" регулярно оценивает риски и работоспособность серверных сред и предоставляет список рекомендаций, относящихся к развернутой серверной инфраструктуре, в порядке приоритета.
-   [Оценка защиты от вредоносных программ](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware). Решение для защиты от вредоносных программ уведомляет о вредоносных программах, угрозах и состоянии защиты.
-   [Служба автоматизации Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker). Решение "Служба автоматизации" хранит модули runbook, выполняет их и управляет ими.
-   [Безопасность и аудит](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started). Панель мониторинга "Безопасность и аудит" создает высокоуровневое представление для состояния безопасности ресурсов, которое содержит метрики по доменам безопасности, важным проблемам, обнаружениям и исследованию угроз, а также по стандартным запросам безопасности.
-   [Оценка SQL](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment). Решение "Проверка работоспособности SQL" регулярно оценивает риски и работоспособность серверных сред и предоставляет клиентам список рекомендаций, относящихся к развернутой серверной инфраструктуре и перечисленных в порядке приоритета.
-   [Управление обновлениями](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management). Решение "Управление обновлениями" позволяет клиенту управлять обновлениями безопасности операционной системы, включая состояние доступных обновлений, а также процесс установки необходимых обновлений.
-   [Работоспособность агента](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth). Решение "Работоспособность агентов" сообщает количество развернутых агентов, их географическое распределение, а также количество агентов, которые перестают отвечать на запросы, и агентов, которые отправляют операционные данные.
-   [Журналы действий Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity). Решение "Аналитика журнала действий" помогает анализировать журналы действий Azure во всех подписках Azure для клиента.
-   [Отслеживание изменений](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity). Решение "Отслеживание изменений" позволяет клиентам легко определять изменения в среде.

**Azure Monitor.** 
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) помогает пользователям отслеживать производительность, поддерживать безопасность и выявлять тенденции. Решение предоставляет организациям возможность аудита, создания оповещений и архивации данных, включая отслеживание вызовов API в ресурсах Azure клиентов.

## <a name="threat-model"></a>Модель рисков
Схему потока данных для этой эталонной архитектуры можно [скачать](https://aka.ms/fedrampWAdfd) или просмотреть ниже. Используя эту модель, клиенты могут выявить зоны потенциального риска в инфраструктуре системы при внесении изменений.

![Веб-приложение IaaS для модели рисков FedRAMP](images/fedramp-iaaswa-threat-model.png?raw=true "IaaS Web Applicaiton for FedRAMP threat model")

## <a name="compliance-documentation"></a>Документация по соответствию

В документе [Azure Security and Compliance Blueprint – FedRAMP High Customer Responsibility Matrix](https://aka.ms/blueprinthighcrm) (Схема безопасности и соответствия требованиям Azure. Таблица ответственности клиентов для FedRAMP уровня High) перечислены все директивы по обеспечению безопасности базового плана FedRAMP уровня High. В таблице указано, кто отвечает за реализацию каждой директивы: корпорация Майкрософт, клиент либо же они вместе.

В документе [Azure Security and Compliance Blueprint – FedRAMP IaaS Web Application High Control Implementation Matrix](https://aka.ms/blueprintwacim) (Схема безопасности и соответствия требованиям Azure. Таблица реализация директив для веб-приложений IaaS для FedRAMP уровня High) перечислены все директивы обеспечения безопасности базового плана FedRAMP уровня High. В таблице содержатся сведения о том, какие директивы охватывает архитектура веб-приложений IaaS, включая подробные описание методов реализации каждой из них.

## <a name="deploy-the-solution"></a>Развертывание решения

Это решение автоматизации схемы безопасности и соответствия требованиям Azure состоит из файлов конфигурации JSON и сценариев PowerShell, которые обрабатываются службой API Azure Resource Manager для развертывания ресурсов в Azure. Подробные инструкции по развертыванию см. в [этой статье](https://aka.ms/fedrampblueprintrepo).
> [!NOTE]
> Это решение развертывается в Azure для государственных организаций.

#### <a name="quickstart"></a>Краткое руководство
1. Клонируйте или скачайте [этот](https://aka.ms/fedrampblueprintrepo) репозиторий GitHub на локальную рабочую станцию.

2. Запустите сценарий PowerShell для предварительного развертывания: azure-blueprint/predeploy/Orchestration_InitialSetup.ps1.

3. Нажмите кнопку ниже, войдите на портал Azure, введите необходимые параметры шаблона ARM и нажмите кнопку **Приобрести**.

    [![Развертывание в Azure](https://azuredeploy.net/AzureGov.png)](https://portal.azure.us/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Ffedramp-iaas-webapp%2Fmaster%2Fazuredeploy.json)

## <a name="guidance-and-recommendations"></a>Инструкции и рекомендации
### <a name="vpn-and-expressroute"></a>VPN и ExpressRoute
Необходимо настроить защищенный туннель VPN или [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) для защищенного подключения к ресурсам, развертываемым в рамках этой эталонной архитектуры веб-приложений IaaS. Правильно настроив VPN-подключение или ExpressRoute, клиенты могут добавить уровень защиты передаваемых данных.

При реализации защищенного VPN-туннеля с помощью Azure можно создать виртуальное частное подключение между локальной сетью и виртуальной сетью Azure. Это подключение осуществляется через Интернет и позволяет клиентам безопасно передавать данные по "туннелю" внутри зашифрованного канала между сетью клиента и Azure. VPN типа "сеть-сеть" — это безопасная проверенная технология, которая десятилетиями применяется на предприятиях всех размеров. [Режим туннеля IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) используется в этом решении как механизм шифрования.

Так как трафик в VPN-туннеле проходит через Интернет с VPN типа "сеть-сеть", Майкрософт предлагает другой, еще более защищенный вариант подключения. Azure ExpressRoute — это выделенный канал связи в глобальной сети между Azure и локальной средой или поставщиком услуг размещения Exchange. Подключения ExpressRoute не осуществляются через Интернет и обеспечивают повышенный уровень безопасности, надежности и скорости соединения с низким уровнем задержки по сравнению со стандартными подключениями через Интернет. Более того, так как это прямое подключение поставщика телекоммуникационных услуг клиента, данные не передаются через Интернет и поэтому недоступны в нем.

См. [рекомендации](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) по реализации защищенной гибридной сети, расширяющей локальную сеть в Azure.

## <a name="disclaimer"></a>Заявление об отказе

- Этот документ является исключительно информационным. МАЙКРОСОФТ НЕ ПРЕДОСТАВЛЯЕТ НИКАКИХ ГАРАНТИЙ, ЯВНЫХ, КОСВЕННЫХ ИЛИ ПРЕДУСМОТРЕННЫХ ЗАКОНОМ, В ОТНОШЕНИИ ИНФОРМАЦИИ В ЭТОМ ДОКУМЕНТЕ. Данный документ предоставляется "как есть". Сведения и мнения, представленные в данном документе, включая URL-адреса и ссылки на другие веб-сайты, могут быть изменены без предварительного уведомления. Клиенты, читающие этот документ, берут ответственность за его использование на себя.  
- Настоящий документ не предоставляет клиентам юридических прав на интеллектуальную собственность в отношении продуктов или решений корпорации Майкрософт.  
- Клиенты могут скопировать и использовать данный документ для внутренних справочных целей.  
- Выполнение некоторых рекомендаций в этом документе может привести к более интенсивному использованию данных, а также сетевых и вычислительных ресурсов в Azure, а значит и к дополнительным затратам на лицензии или подписки Azure.  
- Эта архитектура призвана служить основой, которую клиенты должны адаптировать для конкретных требований, и не должна использоваться "как есть" в рабочей среде.
- Этот документ разработан в качестве справочного материала и не определяет все способы, с помощью которых клиент может отвечать нормам и стандартам соответствия. Клиентам следует обратиться в юридический отдел своей организации, чтобы получить одобренные варианты пользовательских реализаций.
