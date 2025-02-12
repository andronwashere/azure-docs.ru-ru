---
title: Настройка аварийного восстановления для многоуровневого развертывания Dynamics AX с помощью Azure Site Recovery | Документация Майкрософт
description: В этой статье описано, как настроить аварийное восстановление для Dynamics AX с помощью Azure Site Recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: b97bf56c23dfa96acf7cb5af5ac28b4270de117d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61281483"
---
# <a name="set-up-disaster-recovery-for-a-multitier-dynamics-ax-application"></a>Настройка аварийного восстановления для многоуровневого приложения Dynamics AX   




 Dynamics AX является одним из наиболее популярных решений ERP, используемых предприятиями для стандартизации процессов в расположениях, управления ресурсами и упрощения соответствия требованиям. Так как приложение является критически важным для организации, в случае аварии оно должно быть приведено в рабочее состояние в кратчайшие сроки.

Сейчас в Dynamics AX не предоставлены какие-либо готовые функции для аварийного восстановления. Dynamics AX состоит из множества серверных компонентов, таких как сервер Windows Application Object Server, Azure Active Directory, сервер Базы данных SQL Azure, SharePoint Server и Reporting Services. Управление аварийным восстановлением каждого из этих компонентов вручную не только очень затратно, но и может приводить к ошибкам.

В этой статье рассматривается создание решения аварийного восстановления для приложения Dynamics AX с помощью [Azure Site Recovery](site-recovery-overview.md). Кроме того, рассматривается плановая, незапланированная и тестовая отработка отказа с помощью плана восстановления одним щелчком, поддерживаемых конфигураций и необходимых компонентов.



## <a name="prerequisites"></a>Технические условия

Для реализации аварийного восстановления для приложения Dynamics AX с помощью Site Recovery необходимо выполнить следующие предварительные требования:

• Настроить локальное развертывание Dynamics AX.

• Создать хранилище Site Recovery в подписке Azure.

• Если Azure является сайтом аварийного восстановления, запустите средство оценки готовности виртуальной машины Azure на виртуальных машинах. Они должны быть совместимы со службами виртуальных машин Azure и Site Recovery.

## <a name="site-recovery-support"></a>Поддержка Site Recovery

При написании этой статьи использовались виртуальные машины VMware с Dynamics AX 2012 R3 под управлением Windows Server 2012 R2 Enterprise. Так как репликация восстановления сайта не зависит от приложения, описанные здесь рекомендации также подходят для сценариев, представленных ниже.

### <a name="source-and-target"></a>Исходный и целевой объект

**Сценарий** | **На дополнительный сайт** | **В Azure**
--- | --- | ---
**Hyper-V** | Да | Да
**VMware** | Да | Да
**Физический сервер** | Да | Да

## <a name="enable-disaster-recovery-of-the-dynamics-ax-application-by-using-site-recovery"></a>Включение аварийного восстановления приложения Dynamics AX с помощью Site Recovery
### <a name="protect-your-dynamics-ax-application"></a>Защита приложения Dynamics AX
Чтобы можно было включить полные репликацию и восстановление приложения, должен быть защищен каждый компонент Dynamics AX.

### <a name="1-set-up-active-directory-and-dns-replication"></a>1. Настройка репликации Active Directory и DNS

Служба Active Directory должна быть развернута на сайте аварийного восстановления для нормального функционирования приложения Dynamics AX. В зависимости от сложности локальной среды клиента мы рекомендуем два варианта защиты.

**Вариант 1**

Если у пользователя немного приложений и один контроллер домена для всего локального сайта и он планирует выполнить обработку отказа всего сайта, мы рекомендуем использовать Site Recovery для репликации контроллера домена на дополнительный сайт (для сценария репликации как с сайта на сайт, так и с сайта в Azure).

**Вариант 2**

Если у пользователя много приложений, он использует лес Active Directory, а обработке отказа будет подвергнуто несколько приложений одновременно, мы рекомендуем настроить дополнительный контроллер домена на сайте аварийного восстановления (на дополнительном сайте или в Azure).

 Дополнительные сведения см. в статье [Защита Active Directory и DNS с Azure Site Recovery](site-recovery-active-directory.md). Далее в этом документе подразумевается, что контроллер домена доступен на сайте аварийного восстановления.

### <a name="2-set-up-sql-server-replication"></a>2. Настройка репликации SQL Server
Технические рекомендации по решению для защиты на уровне SQL см. в статье [Защита SQL Server с помощью аварийного восстановления SQL Server и Azure Site Recovery](site-recovery-sql.md).

### <a name="3-enable-protection-for-the-dynamics-ax-client-and-application-object-server-vms"></a>3. Включение защиты для клиента Dynamics AX и виртуальных машин Application Object Server
Установите соответствующие конфигурации Site Recovery в зависимости от того, развертываются ли виртуальные машины на узле [Hyper-V](site-recovery-hyper-v-site-to-azure.md) или [VMware](site-recovery-vmware-to-azure.md).

> [!TIP]
> Мы рекомендуем настроить частоту отработки отказа 15 минут.
>

На снимке ниже показано состояние защиты виртуальных машин компонента Dynamics в сценарии защиты "Сайт VMware в Azure".

![Защищенные элементы](./media/site-recovery-dynamics-ax/protecteditems.png)

### <a name="4-configure-networking"></a>4. Настройка сети
**Настройка параметров вычислений и сети для виртуальной машины**

Для виртуальных машин Dynamics AX и Application Object Server настройте параметры сети в Site Recovery таким образом, чтобы сети виртуальных машин подключались к нужной сети аварийного восстановления после отработки отказа. Убедитесь в возможности маршрутизации сети аварийного восстановления для этих уровней до уровня SQL.

Вы можете выбрать виртуальную машину в разделе реплицированных элементов, чтобы настроить параметры сети, как показано на снимке экрана ниже.

* Для серверов AOS выберите правильную группу доступности.

* Если вы используете статический IP-адрес, укажите нужный IP-адрес для виртуальной машины в текстовом поле **Целевой IP-адрес**.

    ![Параметры сети](./media/site-recovery-dynamics-ax/vmpropertiesaos1.png)


### <a name="5-create-a-recovery-plan"></a>5. Создайте план восстановления

Вы можете создать план восстановления в Site Recovery, чтобы автоматизировать процесс отработки отказа. Добавьте в план восстановления уровень приложений и веб-уровень. Распределите их по разным группам, чтобы внешний уровень интерфейса завершал работу до уровня приложений.

1. Выберите хранилище Site Recovery в подписке и щелкните плитку **Планы восстановления**.

2. Выберите **+ План восстановления** и укажите имя.

3. Выберите **источник** и **цель**. Целью может быть Azure или вторичный сайт. Если вы выберите Azure, необходимо указать модель развертывания.

    ![Создать план восстановления](./media/site-recovery-dynamics-ax/recoveryplancreation1.png)

4. Выберите сервер Application Object Server и виртуальные машины клиента в плане восстановления и щелкните значок ✓.

    ![Выбор элементов](./media/site-recovery-dynamics-ax/selectvms.png)

    Пример плана восстановления:

    ![Сведения о плане восстановления](./media/site-recovery-dynamics-ax/recoveryplan.png)

Вы можете настроить план восстановления для приложения Dynamics AX, добавив шаги, как описано ниже. На снимке выше показан полный план восстановления после добавления всех шагов.


* **Действия по отработке отказа для SQL Server.** Сведения о конкретных шагах восстановления для SQL Server см. в статье [Настройка аварийного восстановления для SQL Server](site-recovery-sql.md).

* **Группа отработки отказа 1.** Отработка отказа для виртуальных машин Application Object Server.
Убедитесь, что выбранная точка восстановления максимально близка к точке во времени базы данных, но не предшествует ей.

* **Скрипт.** Добавление подсистемы балансировки нагрузки (только E-A).
Добавьте сценарий (с помощью службы автоматизации Azure) после группы виртуальных машин Application Object Server, чтобы добавить в нее подсистему балансировки нагрузки. Для этого можно использовать сценарий. Дополнительные сведения см. в разделе [Cloud migration and disaster recovery of load balanced multi-tier applications](https://azure.microsoft.com/blog/cloud-migration-and-disaster-recovery-of-load-balanced-multi-tier-applications-using-azure-site-recovery/) (Миграция в облако и аварийное восстановление многоуровневых приложений с балансировкой нагрузки).

* **Группа отработки отказа 2.** Отработка отказа для виртуальных машин клиента Dynamics AX. Выполните отработку отказа виртуальных машин веб-уровня в рамках плана восстановления.


### <a name="perform-a-test-failover"></a>Тестовая отработка отказа

Дополнительные сведения по работе с Active Directory во время тестовой обработки отказа см. в дополнительном руководстве "Решение аварийного восстановления с помощью Active Directory".

Дополнительные сведения по работе с SQL Server во время тестовой отработки отказа см. в статье [Защита SQL Server с помощью аварийного восстановления SQL Server и Azure Site Recovery](site-recovery-sql.md).

1. Перейдите на портал Azure и выберите хранилище Site Recovery.

2. Выберите план восстановления, созданный для Dynamics AX.

3. Выберите **Тестовая отработка отказа**.

4. Выберите виртуальную сеть, чтобы запустить тестовую отработку отказа.

5. Как только будет запущена вторичная среда, вы сможете выполнить проверку.

6. После завершения проверки выберите **Validations complete** (Проверка завершена), и среда тестовой отработки отказа будет удалена.

Дополнительные сведения о выполнении тестовой отработки отказа см. в статье [Тестовая отработка отказа в Azure в Site Recovery](site-recovery-test-failover-to-azure.md).

### <a name="perform-a-failover"></a>Выполнение отработки отказа

1. Перейдите на портал Azure и выберите хранилище Site Recovery.

2. Выберите план восстановления, созданный для Dynamics AX.

3. Щелкните **Отработка отказа** и выберите **Отработка отказа**.

4. Выберите целевую сеть, а затем щелкните значок **✓**, чтобы запустить процесс отработки отказа.

Дополнительные сведения о выполнении отработки отказа см. в разделе [Отработка отказа в Site Recovery](site-recovery-failover.md).

### <a name="perform-a-failback"></a>Восстановление размещения

Рекомендации для среды SQL Server при восстановлении размещения см. в статье [Защита SQL Server с помощью аварийного восстановления SQL Server и Azure Site Recovery](site-recovery-sql.md).

1. Перейдите на портал Azure и выберите хранилище Site Recovery.

2. Выберите план восстановления, созданный для Dynamics AX.

3. Щелкните **Отработка отказа** и выберите **Отработка отказа**.

4. Выберите **Изменить направление**.

5. Настройте соответствующие параметры синхронизации данных и создания виртуальной машины.

6. Чтобы запустить процесс восстановления размещения, щелкните значок **✓**.


Дополнительные сведения о выполнении восстановления размещения см. в статье [Восстановление размещения виртуальных машин VMware и физических серверов на локальном сайте](site-recovery-failback-azure-to-vmware.md).

## <a name="summary"></a>Сводка
С помощью Site Recovery можно создать полностью автоматизированный план аварийного восстановления для приложения Dynamics AX. Отработку отказа можно запустить из любого места в течение нескольких секунд в случае сбоя и восстановить таким образом работоспособность приложения за считанные минуты.

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о защите корпоративных рабочих нагрузок с помощью Azure Site Recovery см. в статье [Какие рабочие нагрузки можно защитить с помощью службы Azure Site Recovery?](site-recovery-workload.md)
