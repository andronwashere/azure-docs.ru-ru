---
title: Схема безопасности и соответствия требованиям Azure. Веб-приложение IaaS для FFIEC
description: Схема безопасности и соответствия требованиям Azure. Веб-приложение IaaS для FFIEC
services: security
author: meladie
ms.assetid: 8577e982-8bc0-4817-a16e-adf2ffefc6f5
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: meladie
ms.openlocfilehash: 10d13e7dd145feff286b8dd58fa1bc657961e8c4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60585800"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-ffiec-financial-services"></a>Схема безопасности и соответствия требованиям Azure. IaaS веб-приложения для финансовых услуг FFIEC

## <a name="overview"></a>Обзор

Эта схема безопасности и соответствия требованиям Azure является руководством по развертыванию среды инфраструктуры как услуги (IaaS), подходящей для сбора, хранения и получения финансовых данных, регулируемых Федеральным советом по надзору за финансовыми учреждениями (Federal Financial Institution Examination Council, FFIEC).

Это руководство по реализации эталонной архитектуры и модель рисков являются основополагающими документами для клиентов, помогающими обеспечить соответствие требованиям FFIEC. В этом решении представлен базовый план, с помощью которого клиенты могут развертывать рабочие нагрузки в Azure в соответствии со стандартами FFIEC. Его не следует использовать "как есть" в рабочей среде, так как нужна дополнительная настройка.

Для соответствия требованиям FFIEC необходимо, чтобы квалифицированные аудиторы сертифицировали решение клиента для рабочей среды. Аудиты проводятся под руководством экспертов из учреждений-членов FFIEC, включая Совет управляющих Федеральной резервной системы(FRB), Федеральную корпорацию по страхованию вкладов (FDIC), Национальное управление кредитных союзов (NCUA), Комиссию пр регулированию деятельности коммерческих банков (OCC) и Бюро по финансовой защите потребителей (CFPB). Эти специалисты подтверждают, что аудиты выполнены экспертами по оценке, сохраняющими независимость от прошедшего аудит учреждения. Клиенты несут ответственность за выполнение надлежащей оценки безопасности и соответствия любого решения, созданного с использованием этой архитектуры, так как требования могут варьироваться в зависимости от специфики реализации каждого клиента.

## <a name="architecture-diagram-and-components"></a>Схема и компоненты архитектуры

Эта схема безопасности и соответствия Azure позволяет развернуть эталонную архитектуру для веб-приложения IaaS с внутренним сервером SQL Server. Архитектура включает веб-уровень, уровень данных, инфраструктуру Active Directory, шлюз приложений и Load Balancer. Виртуальные машины, развернутые на веб-уровне и уровне данных, настраиваются в группе доступности, а экземпляры SQL Server — в группе доступности AlwaysOn. Это обеспечивает высокий уровень доступности. Виртуальные машины присоединены к доменам, а с помощью политик группы Active Directory обеспечивается безопасность и соответствие конфигурациям на уровне операционной системы.

Все решение основывается на службе хранилища Azure, которую пользователи настраивают через портал Azure. Служба хранилища Azure шифрует все данные с помощью шифрования службы хранилища, чтобы гарантировать конфиденциальность неактивных данных. Гео-избыточное хранилище гарантирует, что данные будут реплицированы в дополнительный центр обработки данных, который находится в сотнях километров, и снова сохранены в виде трех копий в этом центре обработки данных, предотвращая неполадки в основном центре обработки данных, которые могут привести к потере данных клиента.

Для повышения уровня безопасности все ресурсы в этом решении управляются как группа ресурсов с помощью Azure Resource Manager. Управление доступом на основе ролей Azure Active Directory используется для управления доступом к развернутым ресурсам, в том числе к ключам в Azure Key Vault. Мониторинг работоспособности системы осуществляется через Azure Monitor. Клиенты могут настроить обе службы мониторинга для ведения журналов и отображения работоспособности системы на единой панели мониторинга с удобной навигацией.

Управляющий узел-бастион предоставляет безопасное подключение для администраторов, позволяющее получить доступ к развернутым ресурсам. **Корпорация Майкрософт рекомендует настроить подключение VPN или ExpressRoute для управления и импорта данных в подсеть эталонной архитектуры.**

![Схема эталонной архитектуры веб-приложения IaaS для FFIEC](images/ffiec-iaaswa-architecture.png "Схема эталонной архитектуры веб-приложения IaaS для FFIEC")

Это решение использует следующие службы Azure. Подробные сведения об архитектуре развертывания см. в [этом разделе](#deployment-architecture).

- Группы доступности
    - (1) Контроллеры домена Active Directory.
    - (1) узлы кластера SQL;
    - (1) Веб-службы и службы IIS.
- Azure Active Directory
- Шлюз приложений Azure
    - (1) Брандмауэр веб-приложения
        - режим брандмауэра — предотвращение;
        - Набор правил: OWASP 3.0
        - Порт прослушивания: 443
- Хранилище ключей Azure
- Azure Load Balancer
- Azure Monitor (журналы)
- Azure Resource Manager
- Центр безопасности Azure
- Хранилище Azure
    - (7) Учетная запись хранения для геоизбыточного хранилища.
- Виртуальные машины Azure
    - (1) управляющий узел или узел бастион (Windows Server 2016 Datacenter);
    - (2) Контроллер домена Active Directory (Windows Server 2016 Datacenter).
    - (2) узел кластера SQL Server (SQL Server 2017 в Windows Server 2016);
    - (2) Веб-службы и службы IIS (Windows Server 2016 Datacenter).
- Виртуальная сеть Azure
    - (1) Сеть /16
    - (5) Сети /24
    - (5) Группы безопасности сети
- Облако-свидетель.
- Хранилище служб восстановления

## <a name="deployment-architecture"></a>Архитектура развертывания

В следующем разделе подробно описываются компоненты развертывания и реализации.

**Узел-бастион**. Узел-бастион является единой точкой входа, которая позволяет пользователям получать доступ к ресурсам, развернутым в этой среде. Узел-бастион предоставляет безопасное подключение к развернутым ресурсам, разрешая только удаленный трафик с общедоступных IP-адресов из надежного списка. Для разрешения трафика по протоколу удаленного рабочего стола (RDP) источник трафика должен быть определен в группе безопасности сети.

Это решение создает виртуальную машину в виде присоединенного к домену узла-бастиона со следующими настройками:
-   [расширение защиты от вредоносных программ](https://docs.microsoft.com/azure/security/azure-security-antimalware);
-   [Расширение системы диагностики Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [шифрование дисков Azure](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) с помощью хранилища ключей Azure Key Vault;
-   [политика автоматического завершения работы](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) для уменьшения степени использования ресурсов виртуальной машины во время простоя.
-   функция [Credential Guard в Защитнике Windows](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) должна быть включена, чтобы учетные данные и другие секретные сведения обрабатывались в защищенной среде, изолированной от работающей операционной системы.

### <a name="virtual-network"></a>Виртуальная сеть

Архитектура определяет частную виртуальную сеть с пространством адресов 10.200.0.0/16.

**Группы безопасности сети**. Решение развертывает ресурсы в архитектуре с отдельной веб-подсетью, подсетью базы данных, подсетью Active Directory и подсетью управления внутри виртуальной сети. Подсети логически разделены правилами групп безопасности сети, применяемыми к отдельным подсетям, которые настроены таким образом, чтобы между подсетями проходил только трафик, который необходим для работы функций системы и управления.

См. конфигурацию [групп безопасности сети](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json), развертываемых с этим решением. Организации могут настроить группы безопасности сети, изменив файл, приведенный выше, с использованием [этой документации](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) в качестве руководства.

Каждая из подсетей имеет выделенную группу безопасности сети:
- 1 группа безопасности сети для шлюза приложений (LBNSG);
- 1 группа безопасности сети для узла-бастиона (MGTNSG);
- 1 группа безопасности сети для основного и резервного контроллеров домена (ADNSG);
- 1 группа безопасности сети для серверов SQL Server и облака-свидетеля (SQLNSG);
- 1 группа безопасности сети для веб-уровня (WEBNSG).

### <a name="data-in-transit"></a>Передаваемые данные
Azure по умолчанию шифрует весь обмен данными в центрах обработки данных в обоих направлениях. Кроме того, все транзакции, направляемые в службу хранилища Azure через портал Azure, проходят по протоколу HTTPS.

### <a name="data-at-rest"></a>Неактивные данные

Архитектура защищает неактивные данные, используя шифрование, аудит базы данных и другие меры.

**Служба хранилища Azure**. Для выполнения требований к шифрованию неактивных данных во всей [службе хранилища Azure](https://azure.microsoft.com/services/storage/) используется [шифрование службы хранилища](https://docs.microsoft.com/azure/storage/storage-service-encryption). Это помогает защищать и охранять данные в рамках корпоративных обязательств по безопасности и соответствия требованиям, определенным в FFIEC.

**Шифрование дисков Azure**. Служба [Шифрование дисков Azure](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) использует компонент BitLocker в Windows, чтобы обеспечить шифрование томов для дисков данных. Решение интегрируется с Azure Key Vault, что помогает управлять ключами шифрования дисков.

**База данных SQL Azure**. В экземпляре Базы данных Azure SQL используются следующие меры безопасности базы данных:

- [Использование Active Directory для аутентификации и авторизации](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) обеспечивает централизованное управление удостоверениями для пользователей баз данных и других служб Майкрософт.
- [Аудит базы данных SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) позволяет отслеживать события базы данных и записывать их в журнал аудита в учетной записи хранения Azure.
- В Базе данных SQL Azure настроено [прозрачное шифрование данных](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), которое обеспечивает шифрование и расшифровку базы данных, связанных резервных копий и файлов журналов транзакций в реальном времени для защиты неактивных данных. Прозрачное шифрование данных позволяет защитить сохраненные данные от несанкционированного доступа.
- [Правила брандмауэра](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) предотвращают любой доступ к серверам баз данных без надлежащих разрешений. Брандмауэр предоставляет доступ к базам данным на основе исходного IP-адреса каждого запроса.
- [Обнаружение угроз SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) позволяет немедленно выявлять потенциальные угрозы и реагировать на них. При этом поступают оповещения системы безопасности о подозрительных действиях с базами данных, потенциальных уязвимостях, атаках путем внедрения кода SQL и аномальных шаблонах доступа к базам данных.
- [Зашифрованные столбцы](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) гарантируют, что конфиденциальные данные никогда не отобразятся как открытый текст внутри системы баз данных. После включения шифрования данных получить доступ к данным в виде открытого текста смогут только клиентские приложения и серверы приложений, у которых есть доступ к ключам.
- [Динамическое маскирование данных Базы данных SQL](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) ограничивает возможность раскрытия конфиденциальных данных, маскируя их для непривилегированных пользователей или приложений. Динамическое маскирование данных может автоматически обнаруживать потенциально конфиденциальные данные и предлагать применение соответствующих масок. Это помогает определить и сократить доступ к данным таким образом, чтобы они не извлекались из базы данных с помощью несанкционированного доступа. Клиенты несут ответственность за настройку параметров динамического маскирования данных в соответствии со своей схемой базы данных.

### <a name="identity-management"></a>Управление удостоверениями

Следующие технологии обеспечивают возможности управления доступом к данным в среде Azure.

- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) — это мультитенантный облачный каталог и служба управления удостоверениями корпорации Майкрософт. Все пользователи этого решения создаются в Azure Active Directory, включая пользователей, обращающихся к Базе данных SQL Azure.
- Аутентификация приложения выполняется с использованием Azure Active Directory. Дополнительные сведения см. в статье [Интеграция приложений с Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Кроме того, при шифровании столбцов базы данных используется Azure Active Directory для проверки подлинности приложения в Базе данных Azure SQL. Дополнительные сведения см. в статье о [защите конфиденциальных данных в Базе данных SQL Azure](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- [Управление доступом на основе ролей Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) позволяет администраторам определять детальные разрешения доступа, предоставляя его ровно в той мере, которая необходима пользователям для выполнения работы. Вместо предоставления каждому пользователю неограниченных разрешений на ресурсы Azure администраторы могут разрешить только определенные действия для доступа к данным. Доступ к подписке есть только у администратора подписки.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) позволяет клиентам свести к минимуму число пользователей, имеющих доступ к определенной информации. Администраторы могут использовать Azure Active Directory Privileged Identity Management для обнаружения, ограничения и отслеживания привилегированных удостоверений, а также их доступа к ресурсам. Эту функцию можно также задействовать для получения административного JIT-доступа по требованию, когда это необходимо.
- [Защита идентификации Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) выявляет потенциальные уязвимости, затрагивающие удостоверения организации, настраивает автоматическое реагирование на обнаруженные подозрительные действия, связанные с удостоверениями организации, и исследует подозрительные инциденты для принятия соответствующих мер по их устранению.


### <a name="security"></a>Безопасность

**Управление секретами**. Для управления ключами и секретами в решении используется [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). Хранилище ключей Azure помогает защитить криптографические ключи и секреты, используемые облачными приложениями и службами. Следующие возможности Azure Key Vault помогают клиентам защитить эти данные и получить доступ к ним:

- Политики расширенного доступа настраиваются по мере необходимости.
- Политики доступа к Key Vault определены с минимальными необходимыми разрешениями на использование ключей и секретов.
- Все ключи и секреты в Key Vault имеют срок действия.
- Все ключи в Key Vault защищены специализированными аппаратными модулями безопасности. Тип ключа — 2048-разрядный ключ RSA, защищенный с помощью HSM.
- Всем пользователям или идентификаторам предоставляются минимальные необходимые разрешения с помощью управления доступом на основе ролей.
- Для журналов диагностики хранилища ключей включен срок хранения не менее 365 дней.
- Допустимые операции шифрования для ключей доступны только тем пользователям, которым они нужны.
- Решение интегрируется с Azure Key Vault, чтобы обеспечить управление секретами и ключами шифрования диска виртуальной машины IaaS.

**Управление исправлениями**. Виртуальные машины Windows, развернутые как часть этой эталонной архитектуры, настраиваются по умолчанию для получения автоматических обновлений из службы Центра обновления Windows. Это решение также включает службу [автоматизации Azure](https://docs.microsoft.com/azure/automation/automation-intro), с помощью которой можно создавать обновленные развертывания для исправления виртуальных машин при необходимости.

**Защита от вредоносных программ**. [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) для виртуальных машин Azure предоставляет возможности защиты в реальном времени для обнаружения и удаления вирусов, шпионских и других вредоносных программ. Вы можете настроить оповещения о попытках установки или выполнения вредоносных или нежелательных программ на защищенных виртуальных машинах.

**Центр безопасности Azure**. С помощью [Центра безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-intro) клиенты могут централизованно применять политики безопасности и управлять ими в рабочих нагрузках, ограничивать подверженность системы к угрозам, а также выявлять атаки и реагировать на них. Кроме того, центр безопасности Azure обращается к существующей конфигурации служб Azure и предоставляет конфигурацию и рекомендации по службам для повышения уровня безопасности и защиты данных.

В центре безопасности Azure используются различные возможности обнаружения, чтобы оповещать клиентов о потенциальных атаках на их среды. Эти оповещения содержат важные сведения о причине их активации, ресурсах, на которые нацелена атака, и ее источнике. Центр безопасности Azure имеет ряд предопределенных [оповещений безопасности](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), которые активируются при возникновении угрозы или подозрительной активности. [Настраиваемые правила генерации оповещений](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) в центре безопасности Azure позволяют клиентам определять новые оповещения безопасности на основе данных, собранных из их среды.

Центр безопасности Azure предоставляет приоритетные оповещения безопасности и инциденты, упрощая процедуры обнаружения и устранения потенциальных проблем безопасности. Для каждой обнаруженной угрозы создается [отчет об исследовании угроз](https://docs.microsoft.com/azure/security-center/security-center-threat-report). Это позволяет командам реагирования на инциденты расследовать и устранять угрозы.

**Шлюз приложений Azure**. Архитектура снижает риск уязвимостей безопасности с помощью шлюза приложений Azure с настроенным брандмауэром веб-приложения и включенным набором правил OWASP. Доступны следующие дополнительные возможности:

- [сквозное SSL-подключение](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell);
- включение [разгрузки SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal);
- отключение [TLS версий 1.0 и 1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell);
- [брандмауэр веб-приложения](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (режим предотвращения);
- [режим предотвращения](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) с набором правил OWASP 3.0.
- включение [ведения журналов диагностики](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics);
- [пользовательские пробы работоспособности](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal);
- [Центр безопасности Azure](https://azure.microsoft.com/services/security-center) и [Помощник Azure](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) обеспечивают дополнительную защиту и уведомления. Центр безопасности Azure также предоставляет систему репутации.

### <a name="business-continuity"></a>Непрерывность бизнес-процессов

**Высокий уровень доступности**. Решение развертывает все виртуальные машины в группе доступности. Дополнительные сведения см. в [этом руководстве](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Группа доступности распределяет виртуальные машины между несколькими изолированными аппаратными кластерами, чтобы повысить уровень доступности. Во время планового или внепланового технического обслуживания доступна как минимум одна виртуальная машина, что соответствует соглашению об уровне обслуживания Azure, гарантирующему доступность в течение 99,95 % времени.

**Хранилище служб восстановления**. [Хранилище Служб восстановления](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) содержит резервные копии данных и защищает все конфигурации виртуальных машин Azure в этой архитектуре. С помощью хранилища служб восстановления клиенты могут восстановить отдельные файлы и папки виртуальной машины IaaS, а не всю виртуальную машину. Это сокращает время восстановления.

**Облако-свидетель**. [Облако-свидетель](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) — это тип свидетеля кворума отказоустойчивого кластера в Windows Server 2016, использующий Azure в качестве точки разрешения конфликтов. Для облака-свидетеля, как и для других свидетелей кворума, предоставляется возможность голосования и участия в подсчетах кворума, но используется стандартное общедоступное хранилище BLOB-объектов Azure. Это позволяет избежать дополнительных затрат на обслуживание виртуальных машин, размещенных в общедоступном облаке.

### <a name="logging-and-auditing"></a>Ведение журналов и аудит

Службы Azure обеспечивают детальную запись системных и пользовательских действий, а также сведений о работоспособности системы.
- **Журналы действий**. [Журналы действий](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) предоставляют информацию об операциях, которые выполнялись с ресурсами в подписке. Журналы действий помогают определить инициатора операции, время события и состояние.
- **Журналы диагностики**. [Журналы диагностики](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) включают все журналы, создаваемые ресурсами. К этим журналам относятся системные журналы событий Windows, журналы службы хранилища Azure, журналы аудита Key Vault, а также журналы брандмауэра и доступа к шлюзу приложений. Все журналы диагностики обеспечивают запись данных в централизованную и зашифрованную учетную запись Azure для архивирования. В соответствии с требованиями конкретной организации пользователь может настроить срок хранения до 730 дней.

**Журналы Azure Monitor**. Эти журналы объединяются в [журналы Azure Monitor](https://azure.microsoft.com/services/log-analytics/) для обработки, хранения и создания отчетов информационной панели. Собранные данные объединяются в отдельные таблицы для каждого типа данных в рабочих областях Log Analytics, что позволяет анализировать все данные независимо от первичного источника. Кроме того центр безопасности Azure интегрируется с журналами Azure Monitor, позволяя клиентам использовать запросов Kusto получить доступ к данным событий безопасности и использовать ее в сочетании с данными из других служб.

Следующие Azure [решения для мониторинга](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) включены как часть этой архитектуры:
-   [Оценка Active Directory](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment). Решение "Проверка работоспособности Active Directory" регулярно оценивает риски и работоспособность серверных сред и предоставляет список рекомендаций, относящихся к развернутой серверной инфраструктуре, в порядке приоритета.
- [Оценка SQL](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment). Решение "Проверка работоспособности SQL" регулярно оценивает риски и работоспособность серверных сред и предоставляет клиентам список рекомендаций, относящихся к развернутой серверной инфраструктуре и перечисленных в порядке приоритета.
- [Работоспособность агентов](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth). Решение "Работоспособность агентов" сообщает количество развернутых агентов, их географическое распределение, а также количество агентов, которые перестают отвечать на запросы, и агентов, которые отправляют операционные данные.
-   [Аналитика журнала действий](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity). Решение Azure Activity Log Analytics помогает анализировать журналы действий Azure во всех подписках Azure для клиента.

**Служба автоматизации Azure**. [Служба автоматизации Azure](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) обеспечивает хранение и выполнение модулей runbook, а также управление ими. В этом решении модули runbook помогают собирать журналы из Базы данных SQL Azure. Решение [Отслеживание изменений](https://docs.microsoft.com/azure/automation/automation-change-tracking) службы автоматизации позволяет клиентам легко определять изменения в среде.

**Azure Monitor**. [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) помогает пользователям отслеживать производительность, поддерживать безопасность и выявлять тенденции, предоставляя организациям возможность аудита, создания оповещений и архивации данных, включая отслеживание вызовов API в ресурсах Azure.

## <a name="threat-model"></a>Модель рисков

Схему потока данных для этой эталонной архитектуры можно [скачать](https://aka.ms/ffiec-iaaswa-tm) или просмотреть ниже. Используя эту модель, клиенты могут выявить зоны потенциального риска в инфраструктуре системы при внесении изменений.

![Веб-приложение IaaS для модели угроз FFIEC](images/ffiec-iaaswa-threat-model.png "Веб-приложение IaaS для модели угроз FFIEC")

## <a name="compliance-documentation"></a>Документация по соответствию

В документе [Схема безопасности и соответствия требованиям Azure. Таблица ответственности клиента согласно FFIEC](https://aka.ms/ffiec-crm) указаны все нормативы FFIEC по безопасности. В этой таблице указано, кто отвечает за реализацию каждого норматива: корпорация Майкрософт, клиент или все вместе.

В документе [Схема безопасности и соответствия требованиям Azure. Таблица реализации веб-приложения IaaS в соответствии с FFIEC](https://aka.ms/ffiec-iaaswa-cim) содержатся сведения о том, какие требования FFIEC отражены в архитектуре веб-приложения IaaS, включая подробные описания того, как реализация соответствует требованиям каждого норматива.

## <a name="guidance-and-recommendations"></a>Инструкции и рекомендации

### <a name="vpn-and-expressroute"></a>VPN и ExpressRoute

Необходимо настроить защищенный туннель VPN или [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) для защищенного подключения к ресурсам, развертываемым в рамках этой эталонной архитектуры веб-приложений IaaS. Правильно настроив VPN-подключение или ExpressRoute, клиенты могут добавить уровень защиты передаваемых данных.

При реализации защищенного VPN-туннеля с помощью Azure можно создать виртуальное частное подключение между локальной сетью и виртуальной сетью Azure. Это подключение осуществляется через Интернет и позволяет клиентам безопасно передавать данные по &quot;туннелю&quot; внутри зашифрованного канала между сетью клиента и Azure. VPN типа "сеть — сеть" представляет собой безопасную проверенную технологию, которая десятилетиями применяется на предприятиях всех размеров. [Режим туннеля IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) используется в этом решении как механизм шифрования.

Так как трафик в VPN-туннеле проходит через Интернет с VPN типа "сеть-сеть", Майкрософт предлагает другой, еще более защищенный вариант подключения. Azure ExpressRoute — это выделенный канал связи в глобальной сети между Azure и локальной средой или поставщиком услуг размещения Exchange. Подключения ExpressRoute не осуществляются через Интернет и обеспечивают повышенный уровень безопасности, надежности и быстродействия подключений с низким уровнем задержки по сравнению со стандартными подключениями через Интернет. Более того, так как это прямое подключение поставщика телекоммуникационных услуг клиента, данные не передаются через Интернет и поэтому недоступны в нем.

См. [рекомендации](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) по реализации защищенной гибридной сети, расширяющей локальную сеть в Azure.

## <a name="disclaimer"></a>Заявление об отказе

- Этот документ является исключительно информационным. МАЙКРОСОФТ НЕ ПРЕДОСТАВЛЯЕТ НИКАКИХ ГАРАНТИЙ, ЯВНЫХ, КОСВЕННЫХ ИЛИ ПРЕДУСМОТРЕННЫХ ЗАКОНОМ, В ОТНОШЕНИИ ИНФОРМАЦИИ В ЭТОМ ДОКУМЕНТЕ. Данный документ предоставляется "как есть". Сведения и мнения, представленные в данном документе, включая URL-адреса и ссылки на другие веб-сайты, могут быть изменены без предварительного уведомления. Клиенты, читающие этот документ, берут ответственность за его использование на себя.
- Настоящий документ не предоставляет клиентам юридических прав на интеллектуальную собственность в отношении продуктов или решений корпорации Майкрософт.
- Клиенты могут скопировать и использовать данный документ для внутренних справочных целей.
- Выполнение некоторых рекомендаций в этом документе может привести к более интенсивному использованию данных, а также сетевых и вычислительных ресурсов в Azure, а значит и к дополнительным затратам на лицензии или подписки Azure.
- Эта архитектура призвана служить основой, которую клиенты должны адаптировать для конкретных требований, и не должна использоваться "как есть" в рабочей среде.
- Этот документ разработан в качестве справочного материала и не определяет все способы, с помощью которых клиент может отвечать нормам и стандартам соответствия. Клиентам следует обратиться в юридический отдел своей организации, чтобы получить одобренные варианты пользовательских реализаций.
