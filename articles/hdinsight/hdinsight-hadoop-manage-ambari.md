---
title: Мониторинг и администрирование кластеров Azure HDInsight с помощью веб-интерфейса Ambari
description: Узнайте, как использовать Ambari для наблюдения за кластерами HDInsight на платформе Linux и управления их функционированием. В этом документе рассказывается об использовании веб-интерфейса Ambari, предоставляемого с кластерами HDInsight.
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: hrasheed
ms.openlocfilehash: d0641a1c058db59acd5e9a64b10bb57b334f82bd
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442053"
---
# <a name="manage-hdinsight-clusters-by-using-the-apache-ambari-web-ui"></a>Управление кластерами HDInsight с помощью веб-интерфейса Apache Ambari

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Apache Ambari упрощает управление кластером Apache Hadoop и его мониторинг за счет удобного пользовательского веб-интерфейса и интерфейса REST API. Ambari входит в кластеры HDInsight и используется для мониторинга кластера и внесения изменений в конфигурацию.

В этом документе рассказывается об использовании веб-интерфейса Ambari с кластером HDInsight.

## <a id="whatis"></a>Что такое Apache Ambari?

[Apache Ambari](https://ambari.apache.org) упрощает управление Hadoop, предоставляя простой в использовании веб-интерфейс. Ambari можно использовать для отслеживания кластеров Hadoop, а также для управления ими. Разработчики могут интегрировать эти возможности в своих приложениях с помощью [интерфейсов REST API Ambari](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="connectivity"></a>Подключение

Веб-интерфейс Ambari доступен в кластере HDInsight по адресу `https://CLUSTERNAME.azurehdinsight.net`, где `CLUSTERNAME` — это имя кластера.

> [!IMPORTANT]  
> Подключение к Ambari в HDInsight выполняется по протоколу HTTPS. При появлении запроса на прохождение проверки подлинности используйте имя и пароль учетной записи администратора, указанные при создании кластера.

## <a name="ssh-tunnel-proxy"></a>Туннель SSH (прокси-сервер)

Хотя Ambari для кластера можно использовать непосредственно в Интернете, некоторые ссылки из веб-интерфейса Ambari (например, для JobTracker) не доступны в Интернете. Для доступа к этим службам необходимо создать туннель SSH. Дополнительные сведения см. в статье [Использование туннелирования SSH с для доступа к веб-интерфейсу Ambari, JobHistory, NameNode, Oozie и другим веб-интерфейсам](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="ambari-web-ui"></a>Веб-интерфейс Ambari

> [!WARNING]  
> HDInsight поддерживает не все функции веб-интерфейса Ambari. Дополнительные сведения см. в этой статье в разделе [Неподдерживаемые операции](#unsupported-operations).

При подключении к веб-интерфейсу Ambari вам будет предложено ввести учетные данные для доступа к странице. Укажите имя администратора кластера (по умолчанию это Admin) и пароль, которые были указаны при создании кластера.

При открытии страницы обратите внимание на панель вверху. Она содержит следующую информацию и элементы управления:

![ambari-nav](./media/hdinsight-hadoop-manage-ambari/ambari-nav.png)

|Элемент |Описание |
|---|---|
|Логотип Ambari|Открывает панель мониторинга, которую можно использовать для наблюдения за кластером.|
|Имя кластера # Ops|Отображает количество текущих операций Ambari. Если выбрать имя кластера или **# ops** (К-во операций), отобразится список фоновых операций.|
|число оповещений|Отображает предупреждения или критические предупреждения, если таковые имеются, для кластера.|
|Информационная панель|Отображает панель мониторинга.|
|Службы|Сведения и параметры конфигурации для служб в кластере.|
|Узлы|Сведения и параметры конфигурации для узлов в кластере.|
|Предупреждения|Журнал сведений, предупреждений и критических оповещений.|
|Административная единица|Программный стек и службы, установленные в кластере, сведения об учетной записи службы и безопасность Kerberos.|
|Кнопка "Администратор"|Ambari управление, параметры пользователя и выход.|

## <a name="monitoring"></a>Отслеживание

### <a name="alerts"></a>Предупреждения

В следующем списке содержатся общие состояния оповещений, используемые Ambari.

* **ОК;**
* **Предупреждение;**
* **КРИТИЧЕСКИЙ;**
* **НЕИЗВЕСТНО.**

При любых оповещениях, кроме **ОК**, в поле **# alerts** (К-во оповещений) в верхней части страницы будет отображаться их общее количество. Выберите эту запись, чтобы отобразить предупреждения и их состояние.

Оповещения разбиты на несколько групп по умолчанию, которые можно просмотреть на странице **Оповещения** .

![страница «Оповещения»](./media/hdinsight-hadoop-manage-ambari/alerts.png)

Группами можно управлять с помощью меню **Действия**, а также выбрав **Manage Alert Groups** (Управление группами оповещений).

![диалоговое окно «Управление группами оповещений»](./media/hdinsight-hadoop-manage-ambari/manage-alerts.png)

Вы можете управлять способами оповещения и создавать уведомления об оповещениях в меню **Действия**, выбрав __Manage Alert Notifications__ (Управление уведомлениями об оповещениях). Отображаются все текущие уведомления. Здесь можно также создавать уведомления. Уведомления можно отправлять по **электронной почте** или **протоколу SNMP** при возникновении определенного сочетания оповещения и уровня серьезности. Например, можно отправить сообщение, если для какого-либо оповещения в группе **YARN Default** (YARN по умолчанию) установлено значение **Критическое**.

![диалоговое окно создания оповещения](./media/hdinsight-hadoop-manage-ambari/create-alert-notification.png)

Наконец, выбрав __Manage Alert Settings__ (Управление параметрами оповещения) в меню __Действия__, вы сможете настроить количество появлений оповещения перед отправкой уведомления. Этот параметр можно использовать для предотвращения уведомлений по временным ошибкам.

### <a name="cluster"></a>Кластер

На вкладке **Метрики** панели мониторинга содержится ряд мини-приложений, которые позволяют легко определить состояние кластера с первого взгляда. Если щелкнуть некоторые мини-приложения, например **Загрузка ЦП**, отобразится дополнительная информация.

![панель мониторинга с метриками](./media/hdinsight-hadoop-manage-ambari/metrics.png)

На вкладке **Тепловые карты** метрики отображаются в виде цветных тепловых карт с переходом от зеленого к красному.

![панель мониторинга с тепловыми картами](./media/hdinsight-hadoop-manage-ambari/heatmap.png)

Для получения дополнительных сведений об узлах кластера выберите **Узлы**. Затем выберите конкретный узел, который вас интересует.

![информация об узле](./media/hdinsight-hadoop-manage-ambari/host-details.png)

### <a name="services"></a>Службы

Боковая панель **Службы** на панели мониторинга позволяет получить общее представление о состоянии служб, работающих в кластере. Различные значки обозначают состояния или действия, которые следует предпринять. Например, желтый символ перезапуска отображается, если служба должна быть перезапущена.

![боковая панель «Службы»](./media/hdinsight-hadoop-manage-ambari/service-bar.png)

> [!NOTE]  
> Для разных типов и версий кластера HDInsight отображаются разные службы. Службы, отображаемые здесь, могут отличаться от служб, отображаемых для вашего кластера.

При выборе службы для нее отобразятся более подробные сведения.

![сводные данные о службе](./media/hdinsight-hadoop-manage-ambari/service-details.png)

#### <a name="quick-links"></a>Быстрые ссылки

Для некоторых служб в верхней части страницы отображается ссылка **Быстрые ссылки** . Их можно использовать для доступа к определенным веб-интерфейсам службы, например:

* **Журнал заданий** — журнал заданий MapReduce;
* **Диспетчер ресурсов** — пользовательский интерфейс диспетчера ресурсов YARN;
* **NameNode** — пользовательский интерфейс NameNode распределенной файловой системы Hadoop (HDFS).
* **Веб-интерфейс Oozie** — пользовательский интерфейс Oozie.

При выборе любой из этих ссылок в браузере откроется новая вкладка с выбранной страницей.

> [!NOTE]  
> При выборе элемента **Быстрые ссылки** для службы может возвращаться ошибка о том, что сервер не найден. При возникновении этой ошибки необходимо использовать туннель SSH, если для этой службы выбирается элемент **Быстрые ссылки**. Дополнительные сведения см. в статье [Использование туннелирования SSH для доступа к веб-интерфейсу Ambari, JobHistory, NameNode, Oozie и другим веб-интерфейсам](hdinsight-linux-ambari-ssh-tunnel.md).

## <a name="management"></a>Управление

### <a name="ambari-users-groups-and-permissions"></a>Пользователи, группы и разрешения Ambari

Работа с пользователями, группами и разрешениями поддерживается только в [присоединенных к домену](./domain-joined/hdinsight-security-overview.md) кластерах HDInsight. Сведения об использовании пользовательского интерфейса управления Ambari в присоединенном к домену кластере см. в статье [Введение в присоединенные к домену кластеры HDInsight (предварительная версия)](./domain-joined/hdinsight-security-overview.md).

> [!WARNING]  
> Не изменяйте пароль модуля наблюдения Ambari (hdinsightwatchdog) в кластере HDInsight под управлением Linux. Это не позволит выполнять действия сценария или операции масштабирования в кластере.

### <a name="hosts"></a>Узлы

На странице **Узлы** перечислены все узлы в кластере. Для управления узлами, сделайте следующее.

![страница «Узлы»](./media/hdinsight-hadoop-manage-ambari/hosts.png)

> [!NOTE]  
> В кластерах HDInsight не следует использовать добавление, списание и восстановление списанного узла.

1. Выберите узел, которым вы хотите управлять.

2. С помощью меню **Действия** выберите действие, которое необходимо выполнить.

    |Элемент |Описание |
    |---|---|
    |Запустить все компоненты|Запустите все компоненты на узле.|
    |Завершение всех компонентов|Завершите работу всех компонентов на узле.|
    |Перезапустить все компоненты|Останавливает и запускает все компоненты на узле.|
    |Включить режим обслуживания|Подавляет предупреждения для узла. Этот режим должен быть включен при выполнении действий, которые создают оповещения. Например, остановка и запуск службы.|
    |Отключение режима обслуживания|Возвращает узел для обычного оповещения.|
    |Остановить|Останавливает узел или NodeManagers на узле.|
    |Начать|Запускает node или NodeManagers на узле.|
    |Перезапустить|Останавливает и запускает узел или NodeManagers на узле.|
    |Списать|Удаляет узел из кластера. **Не используйте это действие в кластерах HDInsight.**|
    |Перекомиссия|Добавляет ранее списанный узел в кластер. **Не используйте это действие в кластерах HDInsight.**|

### <a id="service"></a>Службы

На странице **Панель мониторинга** или **Службы** нажмите кнопку **Действия** в нижней части списка служб, чтобы остановить и снова запустить все службы.

![Действия со службой](./media/hdinsight-hadoop-manage-ambari/service-actions.png)

> [!WARNING]  
> Хотя в этом меню и есть пункт **Добавить службу**, не следует использовать его для добавления служб в кластер HDInsight. Для добавления новых служб следует использовать действие сценария во время подготовки кластера. Дополнительные сведения об использовании действий скрипта см. в статье [Настройка кластеров HDInsight под управлением Linux с помощью действия сценария](hdinsight-hadoop-customize-cluster-linux.md).

Хотя с помощью кнопки **Действия** можно перезапустить все службы, зачастую требуется запустить, остановить или перезапустить только одну из них. Чтобы выполнить действия для отдельной службы, сделайте следующее.

1. На странице **Панель мониторинга** или **Службы** выберите службу.

2. В верхней части вкладки **Сводка** нажмите кнопку **Service Actions** (Действия со службой) и выберите действие, которое необходимо выполнить. Это приведет к перезапуску службы на всех узлах.

    ![действие в службе](./media/hdinsight-hadoop-manage-ambari/individual-service-actions.png)

   > [!NOTE]  
   > Перезапуск некоторых служб во время работы кластера может привести к формированию предупреждений. Во избежание этого можно использовать кнопку **Service Actions** (Действия со службой), чтобы включить **режим обслуживания** для службы перед выполнением перезагрузки.

3. После выбора действия значение в поле **# op** (К-во операций) в верхней части страницы увеличится, что будет свидетельствовать о выполнении фоновой операции. Если настроено отображение списка фоновых операций, этот список появится на экране.

   > [!NOTE]  
   > Если для службы включен **режим обслуживания**, не забудьте отключить его. Для этого нажмите кнопку **Service Actions** (Действия со службой) после завершения операции.

Чтобы настроить службу, сделайте следующее:

1. На странице **Панель мониторинга** или **Службы** выберите службу.

2. Выберите вкладку **Конфигурации** . Отобразится текущая конфигурация. Отобразится также список предыдущих конфигураций.

    ![конфигурации](./media/hdinsight-hadoop-manage-ambari/service-configs.png)

3. Используйте отображаемые поля для изменения конфигурации, а затем выберите **Сохранить**. Можно также выбрать предыдущую конфигурацию, а затем выбрать **Сделать текущей** , чтобы выполнить откат к предыдущим настройкам.

## <a name="ambari-views"></a>Представления Ambari

Представления Ambari позволяют разработчикам подключать элементы пользовательского интерфейса к веб-интерфейсу Ambari, используя [Apache Ambari Views Framework](https://cwiki.apache.org/confluence/display/AMBARI/Views). HDInsight предлагает следующие представления с типами кластеров Hadoop:

* Представление Hive: позволяет выполнять запросы Hive прямо из браузера. Можно сохранять запросы, просматривать результаты, сохранять их в хранилище кластера или загружать результаты в локальную систему. Дополнительные сведения об использовании представлений Hive см. в статье [Use Apache Ambari Hive View with Apache Hadoop in HDInsight](hadoop/apache-hadoop-use-hive-ambari-view.md) (Использование представления Hive Apache Ambari с Apache Hadoop в HDInsight).

* Представление Tez: позволяет лучше понять и оптимизировать задания. Можно просмотреть сведения о том, как выполняются задания Tez и какие ресурсы используются.

## <a name="unsupported-operations"></a>Неподдерживаемые операции

Следующие операции Ambari не поддерживаются в HDInsight:

* __Перемещение службы сборщика метрик__. При просмотре сведений в службе сборщика метрик одно из действий, доступных в меню действий службы, — это __Move Metrics collector__ (Переместить сборщик метрик). В HDInsight это действие не поддерживается.

## <a name="next-steps"></a>Следующие шаги

Узнайте, как использовать [Apache Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md) с HDInsight.