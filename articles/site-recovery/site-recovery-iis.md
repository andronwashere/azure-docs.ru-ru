---
title: Настройка аварийного восстановления для многоуровневого веб-приложения на основе IIS с помощью Azure Site Recovery | Документация Майкрософт
description: Узнайте, как реплицировать виртуальные машины веб-фермы IIS с помощью Azure Site Recovery.
author: mayurigupta13
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: mayg
ms.openlocfilehash: 66b9342f1a67c4c9d35fda447a297cc64d048c1e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66480294"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-iis-based-web-application"></a>Настройка аварийного восстановления для многоуровневого веб-приложения на основе IIS

Программные средства являются механизмом обеспечения эффективности бизнес-операций в организации. Разные веб-приложения используются для различных целей. Некоторые приложения, например приложения обработки платежных ведомостей, финансовые приложения и сайты для обслуживания клиентов, очень важны для деятельности организации. Чтобы избежать потери производительности, необходимо постоянно поддерживать их работу. Более того, постоянная доступность этих приложений позволяет сохранить репутацию организации.

Важнейшие веб-приложения обычно настраиваются как многоуровневые приложения с поддержкой Интернета, базы данных и приложений на различных уровнях. Кроме того, что они распределяются по различным уровням, эти приложения могут также использовать несколько серверов на каждом уровне для балансировки трафика. Более того, сопоставления между различными уровнями и на веб-сервере могут основываться на статических IP-адресах. При отработке отказа некоторые из этих сопоставлений потребуется обновить, особенно если на веб-сервере настроено несколько веб-сайтов. Если веб-приложения используют SSL, необходимо обновить привязки сертификатов.

Традиционные методы восстановления, не основанные на репликации, включают в себя архивацию различных файлов конфигурации, параметров реестра, привязок, настраиваемых компонентов (COM или .NET), содержимого и сертификатов. Файлы можно восстановить с помощью выполняемых вручную действий. Традиционные методы восстановления на основе резервного копирования и восстановления файлов вручную весьма трудоемки, могут приводить к ошибкам и не являются масштабируемыми. Например, вы можете забыть об архивации сертификатов. После отработки отказа у вас не останется другого выбора, кроме как приобрести новые сертификаты для сервера.

Хорошее решение аварийного восстановления поддерживает моделирование планов восстановления для сложных архитектур приложений. Вы также должны иметь возможность добавить настраиваемые действия к планам восстановления для обработки сопоставлений приложений между уровнями. В случае аварии сопоставление приложений обеспечивает надежное решение "одним щелчком", позволяющее уменьшить RTO.

В этой статье описывается защита веб-приложения на основе служб IIS с помощью [Azure Site Recovery](site-recovery-overview.md). В ней рассматриваются передовые методы репликации трехуровневого веб-приложения на основе IIS в Azure, выполнение отработки аварийного восстановления и выполнение отработки отказа приложения в Azure.

## <a name="prerequisites"></a>Технические условия

Прежде чем начать, необходимо знать, как выполнять следующие задачи:

* [Репликация виртуальной машины в Azure](vmware-azure-tutorial.md).
* [Проектирование сети для аварийного восстановления](site-recovery-network-design.md).
* [Выполнение тестовой отработки отказа в Azure](site-recovery-test-failover-to-azure.md).
* [Выполнение отработки отказа в Azure](site-recovery-failover.md).
* [Репликация контроллера домена](site-recovery-active-directory.md).
* [Репликация SQL Server](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Модели развертывания
Веб-приложение на основе IIS обычно следует одной из представленных ниже моделей развертывания.

**Модель развертывания 1**

Веб-ферма на основе IIS с маршрутизацией запросов приложений, сервером IIS и SQL Server.

![Схема веб-фермы на основе IIS с тремя уровнями](./media/site-recovery-iis/deployment-pattern1.png)

**Модель развертывания 2**

Веб-ферма на основе IIS с маршрутизацией запросов приложений, сервером IIS, сервером приложений и SQL Server.

![Схема веб-фермы на основе IIS с четырьмя уровнями](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Поддержка Site Recovery

Для примеров этой статьи использовались виртуальные машины VMware с IIS 7.5 под управлением Windows Server 2012 R2 Enterprise. Так как репликация Site Recovery не зависит от приложения, описанные в этой статье рекомендации подходят для представленных ниже сценариев, а также для различных версий IIS.

### <a name="source-and-target"></a>Исходный и целевой объект

Сценарий | На дополнительный сайт | В Azure
--- | --- | ---
Hyper-V. | Да | Да
VMware | Да | Да
Физический сервер | Нет | Да
Таблицы Azure|Нет данных|Да

## <a name="replicate-virtual-machines"></a>Репликация виртуальных машин

Чтобы реплицировать все виртуальные машины веб-фермы IIS в Azure, следуйте рекомендациям статьи [Тестовая отработка отказа в Azure с помощью Site Recovery](site-recovery-test-failover-to-azure.md).

Если используется статический IP-адрес, вы можете указать нужный IP-адрес для виртуальной машины. Чтобы задать IP-адрес, откройте раздел **Compute and Network** (Вычисления и сеть)  >  **TARGET IP** (Целевой IP-адрес).

![Снимок экрана, на котором показано, как задать целевой IP-адрес в области вычислений и сети Site Recovery](./media/site-recovery-active-directory/dns-target-ip.png)

## <a name="create-a-recovery-plan"></a>Создайте план восстановления
План восстановления позволяет воссоздать последовательность различных уровней в многоуровневом приложении во время отработки отказа. Благодаря этому обеспечивается целостность на уровне приложения. При создании плана восстановления для многоуровневого веб-приложения выполните действия, описанные в [этой статье](site-recovery-create-recovery-plans.md).

### <a name="add-virtual-machines-to-failover-groups"></a>Добавление виртуальных машин в группы отработки отказа
Обычное многоуровневое веб-приложение IIS содержит следующие компоненты:
* уровень базы данных с виртуальными машинами SQL;
* веб-уровень, включающий сервер IIS и уровень приложения. 

Добавьте виртуальные машины в разные группы в соответствии с уровнем.

1. Создайте план восстановления. Добавьте виртуальные машины уровня базы данных в группу 1, чтобы они были выключены последними, а начинали работу первыми.
1. Добавьте виртуальные машины уровня приложения в группу 2, чтобы они начинали работу сразу после включения уровня базы данных.
1. Добавьте виртуальные машины веб-уровня в группу 3, чтобы они начинали работу сразу после включения уровня приложения.
1. Добавьте виртуальные машины с балансировкой нагрузки в группу 4, чтобы они начинали работу сразу после включения веб-уровня.

Дополнительные сведения см. в разделе [Настройка плана восстановления](site-recovery-runbook-automation.md#customize-the-recovery-plan).


### <a name="add-a-script-to-the-recovery-plan"></a>Добавление скрипта в план восстановления
Чтобы настроить правильную работу веб-фермы IIS, вам может потребоваться выполнить некоторые операции после отработки отказа виртуальных машин Azure или во время тестовой отработки отказа. Вы можете настроить автоматическое выполнение некоторых операций после отработки отказа. Например, можно обновить записи DNS, изменить привязку сайта или изменить строку подключения, добавив соответствующие скрипты в план восстановления. Сведения о настройке автоматизированных заданий с помощью скрипта см. в статье [Добавление скрипта VMM в план восстановления](site-recovery-how-to-add-vmmscript.md).

#### <a name="dns-update"></a>Обновление DNS
Если в DNS настроено динамическое обновление, виртуальные машины обычно обновляют IP-адрес сразу после запуска. Если вы хотите добавить явное действие для обновления DNS (обновление IP-адреса виртуальных машин), добавьте [скрипт для обновления IP-адреса в DNS](https://aka.ms/asr-dns-update) как завершающее действие после отработки отказа в группах плана восстановления.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Строка подключения в файле web.config приложения
Строка подключения определяет базу данных, с которой обменивается данными веб-сайт. Если строка подключения содержит имя виртуальной машины базы данных, то после отработки отказа никаких действий не требуется. Приложение может автоматически взаимодействовать с базой данных. Кроме того, если IP-адрес виртуальной машины базы данных сохраняется, нет необходимости обновлять строку подключения. 

Если строка подключения ссылается на виртуальную машину базы данных с помощью IP-адреса, ее необходимо обновить после отработки отказа. Например, приведенная ниже строка подключения указывает на базу данных с IP-адресом 127.0.1.2.

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Чтобы обновить строку подключения на веб-уровне, добавьте [скрипт обновления для подключения IIS](https://gallery.technet.microsoft.com/Update-IIS-connection-2579aadc) в группу 3 в плане восстановления.

#### <a name="site-bindings-for-the-application"></a>Привязки сайта для приложения
Каждый сайт содержит сведения о привязке, которые включают в себя тип привязки, IP-адрес, на котором сервер IIS прослушивает запросы к сайту, номер порта и имена узлов сайта. Во время отработки отказа вам необходимо обновить эти привязки, если изменяется связанный с ними IP-адрес.

> [!NOTE]
>
> Если для привязки сайта вы выбрали **Все неназначенные**, обновление этой привязки после отработки отказа не требуется. Кроме того, если после отработки отказа IP-адрес, связанный с сайтом, остается прежним, привязку к сайту обновлять не нужно. (Период хранения IP-адреса зависит от сетевой архитектуры и подсетей, назначенных основному сайту и сайту восстановления. Их обновление может быть нецелесообразным для организации.)

![Снимок экрана, показывающий установку привязки SSL](./media/site-recovery-iis/sslbinding.png)

Если с сайтом связан IP-адрес, обновите все привязки сайта новым IP-адресом. Чтобы изменить привязки сайта, добавьте [скрипт обновления веб-уровня IIS](https://aka.ms/asr-web-tier-update-runbook-classic) в группу 3 в плане восстановления.

#### <a name="update-the-load-balancer-ip-address"></a>Обновление IP-адреса подсистемы балансировки нагрузки
Если у вас есть виртуальная машина с маршрутизацией запросов приложений, чтобы обновить IP-адрес, добавьте [скрипт для отработки отказа при использовании маршрутизации запросов приложений IIS](https://aka.ms/asr-iis-arrtier-failover-script-classic) в группу 4.

#### <a name="ssl-certificate-binding-for-an-https-connection"></a>Привязка SSL-сертификата для подключения HTTPS
С веб-сайтами может быть связан SSL-сертификат, который обеспечивает безопасный обмен данными между веб-сервером и браузером пользователя. Если у веб-сайта есть HTTPS-подключение, а также связанная HTTPS-привязка сайта к IP-адресу сервера IIS с привязкой SSL-сертификата, необходимо добавить новую привязку сайта для сертификата с IP-адресом виртуальной машины IIS после отработки отказа.

SSL-сертификат можно выдать для следующих компонентов:

* Полного доменного имени веб-сайта.
* Имени сервера.
* Группового сертификата для доменного имени.  
* IP-адреса. Если SSL-сертификат сформирован для IP-адреса сервера IIS, другой SSL-сертификат нужно создать для IP-адреса сервера IIS на сайте Azure. Необходимо создать дополнительную привязку SSL для этого сертификата. Таким образом, рекомендуется не использовать SSL-сертификат для IP-адреса. Этот вариант используется не часто, а вскоре он будет объявлен устаревшим в соответствии с изменениями на форуме CA/Browser.

#### <a name="update-the-dependency-between-the-web-tier-and-the-application-tier"></a>Обновление зависимости между веб-уровнем и уровнем приложения
Если у приложения есть зависимость на основе IP-адреса виртуальных машин, вам необходимо обновить эту зависимость после отработки отказа.

## <a name="run-a-test-failover"></a>Запуск тестовой отработки отказа

1. На портале Azure выберите хранилище служб восстановления.
2. Выберите план восстановления, созданный для веб-фермы IIS.
3. Выберите **Тестовая отработка отказа**.
4. Чтобы запустить тестовую отработку отказа, выберите точку восстановления и виртуальную сеть Azure.
5. После запуска вторичной среды можно выполнить проверку.
6. После завершения проверки выберите **Validations complete** (Проверка завершена), чтобы очистить среду тестовой отработки отказа.

Дополнительные сведения см. в статье [Тестовая отработка отказа в Azure с помощью Site Recovery](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Запуск отработки отказа

1. На портале Azure выберите хранилище служб восстановления.
1. Выберите план восстановления, созданный для веб-фермы IIS.
1. Выберите **Отработка отказа**.
1. Чтобы запустить отработку отказа, выберите точку восстановления.

Дополнительные сведения см. в статье [Отработка отказа в Site Recovery](site-recovery-failover.md).

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте больше о [репликации других приложений](site-recovery-workload.md) с помощью Site Recovery.
