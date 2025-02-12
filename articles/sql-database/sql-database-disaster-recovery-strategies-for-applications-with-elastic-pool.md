---
title: Разработка решений для аварийного восстановления с помощью баз данных SQL Azure | Документация Майкрософт
description: Узнайте, как разработать облачное решение для аварийного восстановления, выбрав правильную модель отработки отказа.
services: sql-database
ms.service: sql-database
ms.subservice: elastic-pools
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: carlrab
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: 6a332ce265a4bb41a9ad3c0c3a29683187a0f0d4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62098411"
---
# <a name="disaster-recovery-strategies-for-applications-using-sql-database-elastic-pools"></a>Стратегии аварийного восстановления для приложений с использованием пула эластичных баз данных SQL

За последние годы мы увидели, что в облачных службах также могут возникать серьезные инциденты с работоспособностью. Базы данных SQL предоставляют несколько возможностей для обеспечения непрерывности бизнес-процессов приложения при возникновении таких инцидентов. [Пулы эластичных баз данных](sql-database-elastic-pool.md) и отдельные базы данных поддерживают одинаковые возможности аварийного восстановления. В этой статье представлены несколько стратегий аварийного восстановления для пулов эластичных БД, в которых используются функции обеспечения непрерывности бизнес-процессов базы данных SQL.

В этой статье используется следующий шаблон канонического приложения независимого поставщика программного обеспечения SaaS:

В современных облачных веб-приложениях на каждого пользователя выделяется одна база данных SQL. Независимый поставщик программного обеспечения имеет широкую клиентскую базу и поэтому использует несколько баз данных, которые называются базами данных клиента. Так как шаблоны использования баз данных клиента достаточно сложно предсказать, чтобы определить стоимость базы данных с учетом длительного периода времени, независимый поставщик программного обеспечения использует пул эластичных БД. Этот пул также позволяет упростить управление производительностью во время пиковой активности пользователей. Помимо баз данных клиента, приложение также использует несколько баз данных для управления профилями пользователей и компонентами безопасности, а также сбора шаблонов использования. Доступность отдельных баз данных клиентов не влияет на работу приложения в целом. Но функционирование приложения значительно зависит от производительности и доступности баз данных управления. Если эти базы данных находятся в автономном режиме, приложение также перейдет в этот режим.

В этой статье рассматриваются стратегии аварийного восстановления, охватывающие разные сценарии — от запускаемых приложений с ограниченным бюджетом до приложений со строгими требованиями к уровню доступности.

> [!NOTE]
> Если вы используете базы данных уровня "Премиум" или "Критически важный для бизнеса" и эластичные пулы, их можно сделать устойчивыми к региональным сбоям, переведя их на избыточную в пределах зоны конфигурацию развертывания. Ознакомьтесь с [базами данных, избыточными между зонами](sql-database-high-availability.md).

## <a name="scenario-1-cost-sensitive-startup"></a>Сценарий 1. Приложения с ограниченным бюджетом

В этом сценарии рассматривается недавно созданная компания с ограниченным бюджетом.  В рамках этого сценария для приложения необходимо упростить процесс развертывания и управления, а также ограничить соглашение об уровне обслуживания для отдельных клиентов. Кроме того, необходимо обеспечить бесперебойную работу приложения.

Чтобы упростить операции, разверните все базы данных клиента в одном эластичном пуле в любом регионе Azure, а в качестве баз данных управления разверните геореплицируемые изолированные базы данных. Для аварийного восстановления баз данных клиента следует использовать геовосстановление. Эта возможность не требует дополнительных затрат. Чтобы обеспечить доступность баз данных управления, их необходимо геореплицировать в другой регион с помощью автоматической отработки отказа (шаг 1). Текущие затраты на настройку аварийного восстановления в этом сценарии равны совокупной стоимости баз данных-получателей. Процесс настройки показан на схеме ниже.

![На рисунке 1](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-1.png)

На схеме ниже показаны шаги восстановления работоспособности приложения после сбоя в основном регионе.

* Группа отработки отказа инициирует автоматическую отработку отказа базы данных управления в регион аварийного восстановления. Приложение автоматически повторно подключается к новой базе данных-источнику. В регионе аварийного восстановления создаются учетные записи и базы данных клиента. Для имеющихся клиентов данные будут временно недоступными.
* Создание пула эластичных БД с такими же настройками, как и в исходном пуле БД (2).
* Использование геовосстановления для создания копий баз данных клиента (3). При необходимости можно запустить отдельные процессы восстановления на основе подключений пользователей или использовать другие схемы расстановки приоритетов приложения.

На этом этапе работа приложения в регионе аварийного восстановления будет восстановлена, но при получении доступа к данным могут происходить задержки.

![На рис. 2](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-2.png)

Если сбой временный, не исключено, что Azure восстановит работоспособность основного региона до завершения всех операций восстановления базы данных в регионе аварийного восстановления. В этом случае проследите за процессом перемещения приложения обратно в основной регион. Этапы этого процесса показаны на схеме ниже.

* Отмена всех ожидающих запросов на геовосстановление.
* Отработка отказа баз данных управления в основном регионе (5). После восстановления работоспособности региона старые базы данных-источники автоматически станут базами данных-получателями. Теперь они снова поменяются ролями.
* Изменение строки подключения приложения для указания основного региона. В основном регионе создаются учетные записи и базы данных клиента. Для некоторых клиентов данные будут временно недоступными.
* Настройка доступа только для чтения для всех баз данных в пуле аварийного восстановления, чтобы предотвратить их изменение в регионе аварийного восстановления (6).
* Удаление или переименование баз данных основного пула, которые были изменены в пуле аварийного восстановления во время процесса восстановления (7).
* Копирование обновленных баз данных из пула аварийного восстановления в основной пул (8).
* Удаление пула аварийного восстановления (9).

На этом этапе приложение включено в основном регионе со всеми доступными базами данных клиента в основном пуле.

![Рис. 3](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-3.png)

Основное **преимущество** этой стратегии — низкий уровень затрат для избыточного хранения данных. Служба баз данных SQL автоматически выполняет резервное копирование, что не влечет перезапись приложения и дополнительные затраты.  Затраты возникают только во время восстановления эластичных баз данных. **Недостаток** этой стратегии заключается в том, что полный процесс восстановления всех баз данных клиента занимает длительный период времени. Это время зависит от общего количества запущенных процессов восстановления в регионе аварийного восстановления и общего размера баз данных клиента. Даже после определения наиболее важных процессов восстановления вы будете конкурировать с другими процессами восстановления, запущенными в том же регионе, так как служба выполняет оценку и регулирование, чтобы уменьшить влияние на имеющиеся базы данных. Кроме того, запустить процесс восстановления баз данных клиента до создания нового пула эластичных БД в регионе аварийного восстановления невозможно.

## <a name="scenario-2-mature-application-with-tiered-service"></a>Сценарий 2. Отлаженное приложение с многоуровневой службой

В этом сценарии рассматривается готовое приложение SaaS с многоуровневыми службами и разными соглашениями об уровне обслуживания для пользователей бесплатной пробной версии и пользователей платной версии. Чтобы удовлетворить потребности пользователей пробной версии, необходимо максимально снизить затраты. Для пользователей пробной версии допускаются простои, но их вероятность необходимо снизить. Пользователям платной версии данные могут понадобиться в любое время. Поэтому необходимо обеспечить им постоянный доступ.

Для выполнения этого сценария отделите пользователей пробной версии от пользователей платной версии и поместите их в разные эластичные пулы. Пользователям пробной версии предоставляется небольшое число eDTU или виртуальных ядер на клиента и соглашение об уровне обслуживания с более низким уровнем услуг, а процесс восстановления занимает больше времени. Пользователи платной версии помещаются в пул с большим числом eDTU или виртуальных ядер на клиента и соглашением об уровне обслуживания с более высоким уровнем услуг. Чтобы максимально сократить время восстановления, базы данных клиентов платной версии географически реплицируются. Процесс настройки показан на схеме ниже.

![Рис. 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-4.png)

Как и в первом сценарии, в качестве баз данных управления следует использовать геореплицируемые изолированные базы данных (1), так как они будут довольно активно использоваться. Это позволит обеспечить предсказуемую производительность новых клиентских подписок, операций обновления профиля и других операций управления. Регион, в котором содержатся базы данных-источники баз данных управления, используется в качестве основного, а регион, в котором находятся базы данных-получатели, — в качестве региона аварийного восстановления.

Базы данных клиентов платной версии имеют активные базы данных в платном пуле основного региона. В регионе аварийного восстановления необходимо подготовить вторичный пул с тем же именем. Каждого клиента необходимо геореплицировать во вторичный пул (2). Это позволит ускорить восстановление всех баз данных клиента с применением отработки отказа.

На следующей схеме показаны шаги восстановления работоспособности приложения после сбоя в основном регионе.

![Рис. 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-5.png)

* Незамедлительная отработка отказа баз данных управления в регионе аварийного восстановления (3).
* Изменение строки подключения приложения для указания региона аварийного восстановления. В регионе аварийного восстановления создаются учетные записи и базы данных клиента. Для имеющихся пользователей пробной версии данные будут временно недоступными.
* Отработка отказа платных баз данных в пул региона аварийного восстановления для немедленного восстановления их доступности (4). Так как отработка отказа сопровождается быстрым изменением уровня метаданных, этот процесс можно оптимизировать, запустив отдельные процессы восстановления по требованию на основе подключений пользователей.
* Если число eDTU или виртуальных ядер дополнительного пула было ниже, чем у основного, так как для дополнительных баз данных eDTU требуются только для обработки журналов изменений при пребывании в роли дополнительных, увеличьте объем пула с учетом рабочей нагрузки всех клиентов (5).
* Создание в регионе аварийного восстановления нового пула эластичных БД с теми же именем и конфигурацией для баз данных пользователей пробной версии (6).
* После создания пула пользователей пробной версии используйте геовосстановление, чтобы выполнить восстановление отдельных баз данных этих пользователей в новый пул (7). При необходимости можно запустить отдельные процессы восстановления на основе подключений пользователей или использовать другие схемы расстановки приоритетов приложения.

На этом этапе работа приложения в регионе аварийного восстановления будет восстановлена. Пользователи платной версии имеют бесперебойный доступ к данным, а пользователи бесплатной пробной версии получают доступ с задержками.

Если Azure восстановит основный регион *после* восстановления приложения в регионе аварийного восстановления, то можно будет использовать приложение в том же регионе или восстановить размещение в основном регионе. Если работоспособность основного региона восстановлена *до* завершения процесса отработки отказа, выполните восстановление размещения. На схеме ниже показаны этапы процесса восстановления размещения.

![Рис. 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-6.png)

* Отмена всех ожидающих запросов на геовосстановление.
* Отработка отказа баз данных управления (8). После восстановления работоспособности региона старая база данных-источник автоматически стала базой данных-получателем. Теперь она снова получит роль базы данных-источника.  
* Отработка отказа для баз данных платных клиентов (9). Аналогичным образом после восстановления работоспособности региона старые базы данных-источники автоматически стали базами данных-получателями. Теперь они снова становятся базами данных-источниками.
* Настройка доступа только для чтения для всех восстановленных баз данных пользователей пробной версии в регионе аварийного восстановления (10).
* Удаление или переименование баз данных в основном пуле пользователей пробной версии, которые были изменены в пуле аварийного восстановления во время процесса восстановления (11).
* Копирование обновленных баз данных из пула аварийного восстановления в основной пул (12).
* Удаление пула аварийного восстановления (13).

> [!NOTE]
> Аварийное восстановление — это асинхронная операция. Чтобы свести к минимуму время восстановления, необходимо выполнить команду отработки отказа баз данных клиента группами по крайней мере из 20 баз данных.

Основное **преимущество** этой стратегии — обеспечение соглашения об уровне обслуживания с наивысшим уровнем обслуживания для пользователей платной версии. Эта стратегия также гарантирует, что новые пробные версии будут доступны сразу же после создания пула аварийного восстановления. **Недостаток** этой стратегии заключается в том, что совокупная стоимость баз данных клиента увеличится с учетом стоимости вторичного пула аварийного восстановления для пользователей платной версии. Кроме того, если размер вторичного пула отличается, у пользователей платной версии до завершения обновления пула в регионе аварийного восстановления возникают проблемы с производительностью.

## <a name="scenario-3-geographically-distributed-application-with-tiered-service"></a>Сценарий 3. Географически распределенное приложение с многоуровневой службой

В этом сценарии рассматривается готовое приложение SaaS с многоуровневыми службами. Для выполнения стратегии необходимо предоставить пользователям платной версии соглашение об уровне обслуживания с наивысшим уровнем услуг и свести к минимуму влияние сбоев, так как даже краткие проблемы с работоспособностью приложения могут стать причиной недовольства пользователей. Очень важно, чтобы пользователи платной версии всегда имели доступ к данным. Плата за использование пробной версии не взимается, и во время пробного периода соглашение об уровне обслуживания не предоставляется.

Для выполнения этого сценария используйте три отдельных эластичных пула. В двух разных регионах подготовьте два пула одинакового размера с большим числом eDTU или виртуальных ядер на базу данных, в которых будут содержаться базы данных платных пользователей. Третий пул, в котором содержатся клиенты платных пользователей, может иметь более низкое число eDTU или виртуальных ядер на базу данных, и его необходимо подготовить в одном из двух регионов.

Чтобы максимально сократить время восстановления после сбоев, базы данных платных пользователей географически реплицируются с половиной баз данных-источников, размещенных в каждом из двух регионов. Точно так же в каждом регионе содержится 50 % вторичных баз данных. Таким образом, если регион вне сети, только половина баз данных платных пользователей будет затронута, и для них будет выполняться отработка отказа. На производительность остальных баз данных это не повлияет. Процесс настройки показан на схеме ниже.

![Рис. 4](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-7.png)

Как и в предыдущих сценариях, в качестве баз данных управления настройте геореплицируемые изолированные базы данных (1), так как они довольно активно используются. Это позволит обеспечить предсказуемую производительность новых клиентских подписок, операций обновления профиля и других операций управления. Регион А используется в качестве основного региона для баз данных управления, а регион Б — в качестве региона для их восстановления.

Базы данных платных пользователей также географически реплицируются, но с разделением баз данных-источников и баз данных-получателей между регионами А и Б (2). Таким образом, в случае сбоя для затронутых баз данных-источников можно выполнить отработку отказа в другой регион и обеспечить их доступность. На производительность другой половины баз данных сбой не повлияет.

На следующей схеме показаны шаги восстановления в случая сбоя в регионе А.

![Рис. 5](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-8.png)

* Незамедлительная отработка отказа баз данных управления в регион Б (3).
* Изменение строки подключения приложения для указания баз данных управления в регионе Б. Измените базы данных управления, чтобы обеспечить перемещение имеющихся баз данных в регион Б и создание там же новых учетных записей и баз данных клиентов. Для имеющихся пользователей пробной версии данные будут временно недоступными.
* Отработка отказа баз данных платных пользователей в пул 2 региона Б для немедленного восстановления их доступности (4). Так как отработка отказа сопровождается быстрым изменением уровня метаданных, этот процесс можно оптимизировать, запустив отдельные процессы восстановления по требованию на основе подключений пользователей.
* Так как теперь в пуле 2 содержатся только базы данных-источники, рабочая нагрузка в нем увеличится и может незамедлительно увеличить число eDTU (5) или виртуальных ядер.
* Создание в регионе Б нового пула эластичных БД с теми же именем и конфигурацией для баз данных пользователей пробной версии (6).
* После создания пула используйте геовосстановление, чтобы выполнить восстановление отдельной базы данных клиента пробной версии (7). При необходимости можно запустить отдельные процессы восстановления на основе подключений пользователей или использовать другие схемы расстановки приоритетов приложения.

> [!NOTE]
> Аварийное восстановление — это асинхронная операция. Чтобы свести к минимуму время восстановления, необходимо выполнить команду отработки отказа баз данных клиента группами по крайней мере из 20 баз данных.

На этом этапе работа приложения в регионе Б будет восстановлена. Пользователи платной версии приложения имеют доступ к их данным, а пользователи бесплатной пробной версии получают доступ с задержками.

После восстановления региона А необходимо принять решение: использовать регион Б для пользователей пробной версии или выполнить восстановление размещения в пул этих пользователей в регионе А. Основным показателем может быть процент баз данных пользователей пробной версии, измененных в процессе восстановления. Независимо от этого решения платных клиентов между двумя пулами необходимо перераспределить. На следующей схеме показан процесс восстановления размещения баз данных клиента пробной версии в регион A.  

![Рис. 6](./media/sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool/diagram-9.png)

* Отмена всех ожидающих запросов на геовосстановление в пуле аварийного восстановления для пользователей пробной версии.
* Отработка отказа базы данных управления (8). После восстановления работоспособности региона старая база данных-источник автоматически стала базой данных-получателем. Теперь она снова получит роль базы данных-источника.  
* Выбор баз данных пользователей платной версии, для которых необходимо выполнить восстановление размещения в пул 1, и запуск отработки отказа для их баз данных-получателей (9). После восстановления работоспособности региона все базы данных в пуле 1 автоматически стали базами данных-получателями. Теперь половина баз данных снова становятся базами данных-источниками.
* Уменьшение размера пула 2 до исходного числа eDTU (10) или виртуальных ядер.
* Настройка доступа только для чтения для всех восстановленных баз данных пользователей пробных версий в регионе Б (11).
* Удаление или переименование баз данных основного пула пользователей пробной версии, которые были изменены в аналогичном пуле аварийного восстановления во время процесса восстановления (12).
* Копирование обновленных баз данных из пула аварийного восстановления в основной пул (13).
* Удаление пула аварийного восстановления (14).

**Преимущества** этой стратегии перечислены ниже.

* Обеспечивает поддержку соглашения об уровне обслуживания с наивысшим уровнем услуг для пользователей платной версии приложения. Это позволяет сохранить работоспособность 50 % баз данных клиентов после сбоя.
* Эта стратегия гарантирует, что новые пробные версии будут доступны сразу же после создания пула аварийного восстановления для пользователей пробной версии.
* Позволяет более эффективно использовать емкость пула, так как в 50 % баз данных-получателей в пуле 1 и 2 снижена активность по сравнению с базами данных-источниками.

**Недостатки** этой стратегии перечислены ниже.

* Операции CRUD в базах данных управления имеют меньшую задержку для пользователей в регионе А, чем для пользователей в регионе Б, так как они выполняются с базами данных-источниками баз данных управления.
* Она требует более сложной структуры базы данных управления. Например, каждая запись клиента включает в себя тег расположения, который необходимо изменить во время отработки отказа и восстановления размещения.  
* У пользователей платной версии приложения до завершения обновления пула в регионе Б будут возникать проблемы с производительностью.

## <a name="summary"></a>Сводка

Эта статья посвящена стратегиям аварийного восстановления баз данных для мультитенантного приложения независимых поставщиков программного обеспечения SaaS. Выбор стратегии зависит от потребностей приложения, включая бизнес-модель и соглашение об уровне обслуживания, которые вы планируете предоставлять своим пользователям, бюджета и т. д. Для каждой рассмотренной стратегии приведены ее основные преимущества и недостатки. Это позволит вам принять обоснованное решение. Вероятно, ваше приложение включает другие компоненты Azure. Изучите руководство по обеспечению непрерывности бизнес-процессов для этих компонентов и выполните восстановление на уровне базы данных. Дополнительные сведения об управлении восстановлением приложений базы данных в Azure см. в статье [Разработка службы высокой доступности с помощью базы данных SQL Azure](sql-database-designing-cloud-solutions-for-disaster-recovery.md).  

## <a name="next-steps"></a>Дальнейшие действия

* Чтобы узнать об автоматически создаваемых резервных копиях базы данных SQL Azure, ознакомьтесь с разделом [Общие сведения об автоматическом резервном копировании базы данных SQL](sql-database-automated-backups.md).
* Сведения об обеспечении непрерывности бизнес-процессов и возможные сценарии описаны в [обзоре непрерывности бизнес-процессов](sql-database-business-continuity.md).
* Чтобы узнать об использовании автоматически создаваемых резервных копий для восстановления, ознакомьтесь с [восстановлением базы данных из резервных копий, инициируемых службой](sql-database-recovery-using-backups.md).
* Чтобы узнать о более быстрых вариантах восстановления, ознакомьтесь со сведениями об [активной георепликации](sql-database-active-geo-replication.md) и [группах автоматической отработки отказа](sql-database-auto-failover-group.md).
* Чтобы узнать об использовании автоматически создаваемых резервных копий для архивации, ознакомьтесь с [копированием базы данных](sql-database-copy.md).
