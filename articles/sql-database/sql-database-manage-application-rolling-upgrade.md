---
title: Последовательные обновления приложений (База данных SQL Azure) | Документация Майкрософт
description: Вы можете узнать, как использовать георепликацию базы данных SQL Azure для поддержки оперативных обновлений облачного приложения.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 02/13/2019
ms.openlocfilehash: 47fd6c1e2bb342bc1a31fb16a45a5ebc749dca69
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60702679"
---
# <a name="manage-rolling-upgrades-of-cloud-applications-by-using-sql-database-active-geo-replication"></a>Управление последовательными обновлениями для облачных приложений с помощью активной георепликации базы данных SQL

Узнайте, как использовать [активную георепликацию](sql-database-auto-failover-group.md) в Базе данных SQL Azure для последовательных обновлений облачного приложения. Так как операции обновления нарушают работу, это должно учитываться при планировании и проектировании непрерывности бизнес-процессов. В этой статье мы рассмотрим два разных метода для оркестрации процесса обновления, а также преимущества и недостатки каждого из них. В рамках этой статьи мы используем приложение, состоящее из веб-сайта, подключенного к отдельной базе данных в качестве уровня данных. Наша цель заключается в обновлении версии 1 (V1) приложения до версии 2 (V2) без значительного влияния на взаимодействие с пользователем.

При оценке вариантов обновления учитывайте следующие факторы:

* Влияние на доступность приложения во время обновления, например, как долго функциональные возможности приложения могут быть ограничены или понижены.
* Возможность отката в случае сбоя обновления.
* Уязвимость приложения к не связанному с ним разрушительному сбою во время обновления.
* Общие денежные затраты. Сюда входят затраты на дополнительную избыточность баз данных и добавочные затраты на временные компоненты, используемые в процессе обновления.

## <a name="upgrade-applications-that-rely-on-database-backups-for-disaster-recovery"></a>Обновление приложений, полагающихся на резервные копии базы данных в случае аварийного восстановления

Если приложение полагается на создаваемые автоматически резервные копии базы данных и использует геовосстановление для аварийного восстановления, оно развертывается в одном регионе Azure. Чтобы свести к минимуму влияние на работу пользователей, создайте промежуточную среду в этом регионе со всеми компонентами приложения, участвующими в обновлении. На первой схеме показана рабочая среда до начала процесса обновления. Конечная точка `contoso.azurewebsites.net` представляет рабочую среду веб-приложения. Чтобы обеспечить возможность отката обновления, сначала создайте промежуточную среду с полностью синхронизированной копией базы данных. Чтобы создать промежуточную среду для обновления, выполните следующие действия.

1. Создайте базу данных-получатель в том же регионе Azure. Осуществляйте ее мониторинг, чтобы определить завершение процесса заполнения (1).
2. Создайте новую среду для веб-приложения и назовите ее Staging. Она будет зарегистрирована в Azure DNS с URL-адресом `contoso-staging.azurewebsites.net` (2).

> [!NOTE]
> Эти подготовительные меры не затронут рабочую среду, поэтому она сможет работать в режиме полного доступа.

![Настройка георепликации базы данных SQL для облачного аварийного восстановления.](media/sql-database-manage-application-rolling-upgrade/option1-1.png)

По завершении этих действий приложение будет готово к фактическому обновлению. Шаги, входящие в процесс обновления, показаны на следующей схеме.

1. Настройте базу данных-источник в режиме только для чтения (3). Такой режим гарантирует, что рабочая среда веб-приложения (V1) останется доступной только для чтения во время обновления. Это предотвратит расхождение данных между экземплярами базы данных V1 и V2.
2. Отключите базу данных-получатель, используя режим планового завершения (4). При этом создается полностью синхронизированная независимая копия базы данных-источника. Эта база данных будет обновлена.
3. Переключите базу данных-получатель в режим чтения и записи, а затем запустите сценарий обновления (5).

![Настройка георепликации базы данных SQL для облачного аварийного восстановления.](media/sql-database-manage-application-rolling-upgrade/option1-2.png)

Если обновление завершится успешно, то все будет готово к переключению пользователей на обновленную копию приложения, которая станет рабочей средой. Для такого переключения нужно выполнить несколько дополнительных действий, как показано на следующей схеме.

1. Активируйте операцию переключения между рабочей и промежуточной средами веб-приложения (6). При этом переключаются URL-адреса этих двух сред. Теперь `contoso.azurewebsites.net` указывает на версию V2 веб-сайта и базы данных (в рабочей среде). 
2. Если вам больше не требуется версия V1, которая стала промежуточной копией после замены, промежуточную среду можно списать (7).

![Настройка георепликации базы данных SQL для облачного аварийного восстановления.](media/sql-database-manage-application-rolling-upgrade/option1-3.png)

Если обновление не удалось выполнить (например, из-за ошибки в сценарии обновления), то промежуточную среду следует считать несогласованной. Чтобы выполнить откат приложения до состояния перед обновлением, верните приложение в рабочей среде в режим полного доступа. На следующей схеме показаны действия для отката обновления.

1. Переключите копию базы данных в режим чтения и записи (8). Будет восстановлена полная функциональность версии V1 рабочей копии.
2. Выполните анализ первопричин и спишите промежуточную среду (9).

На этом этапе приложение полностью работоспособно, и можно повторить действия по обновлению.

> [!NOTE]
> Откат не требует изменений DNS, так как операция переключения еще не выполнялась.

![Настройка георепликации базы данных SQL для облачного аварийного восстановления.](media/sql-database-manage-application-rolling-upgrade/option1-4.png)

Ключевым преимуществом этого варианта является возможность обновить приложение в одном регионе с помощью набора простых шагов. Денежная стоимость обновления сравнительно мала. 

Главный недостаток заключается в том, что в случае разрушительного сбоя во время обновления для восстановления до состояния перед обновлением потребуется повторно развернуть приложение в другом регионе и восстановить базу данных из резервной копии с помощью геовосстановления. Этот процесс приведет к значительному простою.

## <a name="upgrade-applications-that-rely-on-database-geo-replication-for-disaster-recovery"></a>Обновление приложений, полагающихся на геовосстановление базы данных в случае аварийного восстановления

Если приложение использует активную георепликацию или группы автоматической отработки отказа для обеспечения непрерывности бизнеса, то оно развертывается по крайней мере в двух разных регионах. В основном регионе размещается база данных-источник, а в резервном регионе размещается база данных-получатель, доступная только для чтения. Помимо факторов, упомянутых в начале этой статьи, процесс обновления также должен гарантировать следующее:

* Приложение остается защищенным от разрушительного сбоя на протяжении всего обновления.
* Геоизбыточные компоненты приложения обновляются параллельно с активными компонентами.

Для достижения этих целей, помимо сред веб-приложений, вы воспользуетесь преимуществами диспетчера трафика Azure, применив профиль отработки отказа с одной активной и одной резервной конечными точками. На следующей схеме показана рабочая среда до начала процесса обновления. Веб-сайты `contoso-1.azurewebsites.net` и `contoso-dr.azurewebsites.net` представляют рабочую среду приложения с полной географической избыточностью. Рабочая среда состоит из следующих компонентов:

* рабочая среда веб-приложения `contoso-1.azurewebsites.net` в основном регионе (1);
* база данных-источник в основном регионе (2);
* резервный экземпляр веб-приложения в резервном регионе (3);
* геореплицируемая база данных-получатель в резервном регионе (4);
* профиль производительности диспетчера трафика Azure с конечной точкой `contoso-1.azurewebsites.net` в сети и конечной точкой `contoso-dr.azurewebsites.net` вне сети.

Чтобы обеспечить возможность отката обновления, нужно создать промежуточную среду с полностью синхронизированной копией приложения. Так как необходимо убедиться, что приложение можно быстро восстановить в случае разрушительного сбоя во время обновления, промежуточная среда также должна быть геоизбыточной. Чтобы создать промежуточную среду для обновления, выполните следующие действия:

1. Разверните промежуточную среду веб-приложения в основном регионе (6).
2. Создайте базу данных-получатель в основном регионе Azure (7). Настройте промежуточную среду веб-приложения для подключения к ней. 
3. Создайте еще одну геоизбыточную базу данных-получатель в резервном регионе путем репликации базы данных-получателя в основной регион. Этот метод называется *цепной георепликацией* (8).
4. Разверните промежуточную среду экземпляра веб-приложения в резервном регионе (9) и настройте его для подключения к геореплицируемой базе данных-получателю, созданной на шаге (8).

> [!NOTE]
> Эти подготовительные действия не затронут приложение в рабочей среде. Оно останется полностью функциональным в режиме чтения и записи.

![Настройка георепликации базы данных SQL для облачного аварийного восстановления.](media/sql-database-manage-application-rolling-upgrade/option2-1.png)

По завершении подготовительных действий промежуточная среда будет готова к обновлению. Шаги обновления показаны на следующей схеме.

1. Переключите базу данных-источник в рабочей среде в режим только для чтения (10). Такой режим гарантирует, что рабочая база данных (V1) не будет изменена во время обновления. Это предотвратит расхождение данных между экземплярами базы данных V1 и V2.

```sql
-- Set the production database to read-only mode
ALTER DATABASE <Prod_DB>
SET (ALLOW_CONNECTIONS = NO)
```

2. Закрыть географической репликации, отключение вторичной реплике (11). При этом создается независимая, но полностью синхронизированная копия рабочей базы данных-источника. Эта база данных будет обновлена. В следующем примере используется Transact-SQL, но [PowerShell](/powershell/module/az.sql/remove-azsqldatabasesecondary?view=azps-1.5.0) также доступна. 

```sql
-- Disconnect the secondary, terminating geo-replication
ALTER DATABASE <Prod_DB>
REMOVE SECONDARY ON SERVER <Partner-Server>
```

3. Запустите сценарий обновления `contoso-1-staging.azurewebsites.net`, `contoso-dr-staging.azurewebsites.net` и промежуточной базы данных-источника (12). Изменения в базе данных автоматически реплицируются в промежуточную базу данных-получатель.

![Настройка георепликации базы данных SQL для облачного аварийного восстановления.](media/sql-database-manage-application-rolling-upgrade/option2-2.png)

После успешного обновления все будет готово к переключению пользователей на версию V2 приложения. Соответствующие шаги показаны на следующей схеме.

1. Активируйте операцию переключения между рабочей и промежуточной средами веб-приложения в основном регионе (13) и резервном регионе (14). Версия V2 приложения теперь станет рабочей средой с избыточной копией в резервном регионе.
2. Если версия V1 приложения больше не требуется (15 и 16), можно списать промежуточную среду.

![Настройка георепликации базы данных SQL для облачного аварийного восстановления.](media/sql-database-manage-application-rolling-upgrade/option2-3.png)

Если обновление не удастся выполнить (например, из-за ошибки в сценарии обновления), промежуточную среду следует считать несогласованной. Чтобы выполнить откат приложения до состояния перед обновлением, верните версию V1 приложения в рабочую среду. Необходимые действия указаны на приведенной ниже схеме.

1. Переключите копию базы данных-источника в рабочей среде в режим чтения и записи (17). Будет восстановлена полная функциональность версии V1 в рабочей среде.
2. Выполните анализ первопричин и исправьте или удалите промежуточную среду (18 и 19).

На этом этапе приложение полностью работоспособно, и можно повторить действия по обновлению.

> [!NOTE]
> Откат не требует изменений DNS, так как операция переключения еще не выполнялась.

![Настройка георепликации базы данных SQL для облачного аварийного восстановления.](media/sql-database-manage-application-rolling-upgrade/option2-4.png)

Ключевым преимуществом этого варианта является возможность параллельно обновить приложение и его геоизбыточную копию, не нарушая непрерывность бизнес-процессов во время обновления.

Главный недостаток заключается в двукратной избыточности каждого компонента приложения, что влечет за собой повышенную стоимость. Кроме того, этот вариант подразумевает более сложный рабочий процесс.

## <a name="summary"></a>Сводка

Два описанных здесь метода обновления отличаются по сложности и стоимости, но они оба позволяют минимизировать период, в течение которого пользователь сможет выполнять только операции чтения. Это время напрямую зависит от длительности выполнения сценария обновления. Оно не зависит от размера базы данных, выбранного уровня служб, конфигурации веб-сайта и других факторов, которыми нелегко управлять. Подготовительные действия отделены от действий по обновлению и не влияют на рабочее приложение. Эффективность сценария обновления — ключевой фактор, определяющий взаимодействие с пользователем во время обновления. Поэтому для улучшения всей процедуры рекомендуется сосредоточиться на повышении эффективности сценария обновления.

## <a name="next-steps"></a>Дальнейшие действия

* Сведения об обеспечении непрерывности бизнес-процессов и возможные сценарии описаны в [обзоре непрерывности бизнес-процессов](sql-database-business-continuity.md).
* Сведения об активной георепликации базы данных SQL Azure см. в статье [Создание доступных для чтения баз данных-получателей с помощью активной георепликации](sql-database-active-geo-replication.md).
* Сведения о группах автоматической отработки отказа Базы данных SQL Azure см. в статье [Использование групп автоматической отработки отказа для включения прозрачной и согласованной отработки отказа в нескольких базах данных](sql-database-auto-failover-group.md).
* Сведения о промежуточных средах в Службе приложений Azure см. в статье [Настройка промежуточных сред в Службе приложений Azure](../app-service/deploy-staging-slots.md).
* Сведения о профилях диспетчера трафика Azure см. в статье [Управление профилем диспетчера трафика Azure](../traffic-manager/traffic-manager-manage-profiles.md).
