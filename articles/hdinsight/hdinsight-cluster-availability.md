---
title: Мониторинг доступности кластера с помощью журналов Ambari и Azure Monitor
description: Узнайте, как использовать Ambari и журналы Azure Monitor для мониторинга работоспособности кластера и доступности.
keywords: мониторинг, ambari, монитор, log analytics, оповещения, доступность, работоспособность
ms.reviewer: jasonh
author: tylerfox
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: tyfox
ms.openlocfilehash: 195999ba685828042fc958e8aed7e67bad694657
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786558"
---
# <a name="how-to-monitor-cluster-availability-with-ambari-and-azure-monitor-logs"></a>Мониторинг доступности кластера с помощью журналов Ambari и Azure Monitor

Кластеры HDInsight включают как Apache Ambari, который предоставляет сведения о работоспособности в быстро и предварительно установленные предупреждения, а также интеграции журналов Azure Monitor, который предоставляет поддерживает запросы метрик и журналов, а также можете настроить оповещения.

Этом документе показано, как использовать эти средства для мониторинга кластера и рассматриваются некоторые примеры для настройки предупреждения Ambari, мониторинга тарифу узла и создания оповещений Azure Monitor, срабатывает, если пульс не был получен из одного или нескольких узлов в 5 часов.

## <a name="ambari"></a>Ambari

### <a name="dashboard"></a>панель мониторинга

Панели мониторинга Ambari можно получить, щелкнув **Ambari домашней** ссылку в **панели мониторинга кластера** разделе HDInsight колонке обзора на портале Azure, как показано ниже. Кроме того, он доступен, введя следующий URL-адрес в браузере [https://\<clustername\>. azurehdinsight.net](https://clustername.azurehdinsight.net/)

![Представление портала ресурсов HDInsight](media/hdinsight-cluster-availability/portal-overview.png)

Затем вам будет предложено ввести имя пользователя и пароль. Введите учетные данные, выбранный при создании кластера.

Затем, вы будете перенаправлены на панели мониторинга Ambari, которая содержит мини-приложения, в которых показано небольшое число метрик приведу краткий обзор работоспособности кластера HDInsight. Эти мини-приложения Показать метрик, таких как количество динамической DataNodes (рабочих узлов) и JournalNodes (узел zookeeper), время узлах имени (головных узлов), а также метрики отдельных типов кластера, такие как YARN ResourceManager время бесперебойной работы для кластеров Spark и Hadoop.

![Панель мониторинга Ambari](media/hdinsight-cluster-availability/ambari-dashboard.png)

### <a name="hosts--view-individual-node-status"></a>Узлы — Просмотр состояния отдельного узла

Можно также просмотреть сведения о состоянии для отдельных узлов. Нажмите кнопку **узлы** tab, чтобы просмотреть список всех узлов в кластере и просмотреть основную информацию о каждом узле. Зеленый флажок слева от каждого имени узла указывает, что все компоненты не будут на узле. Если компонент не работает на узле, вы увидите красный треугольник предупреждения вместо Зеленый флажок.

![Просмотреть узлы Ambari](media/hdinsight-cluster-availability/ambari-hosts.png)

Вы можете нажать на **имя** узла, чтобы просмотреть более подробные метрики уровня узла для этого конкретного узла. В этом представлении отображаются состояние и доступность каждого компонента.

![Узлы Ambari единое представление узла](media/hdinsight-cluster-availability/ambari-hosts-node.png)

### <a name="ambari-alerts"></a>Предупреждения Ambari

Ambari также предлагает несколько настраиваемых оповещений, которые могут предоставить уведомления об определенных событиях. Когда оповещений, они отображаются в левом верхнем углу Ambari в красный индикатор событий, содержащий список оповещений число оповещений. Щелкнув этот значок отображается список текущих предупреждений.

![Предупреждения Ambari подсчета](media/hdinsight-cluster-availability/ambari-alerts.png)

Чтобы просмотреть список определений предупреждений и их состояние, щелкните **оповещения** вкладке, как показано ниже.

![Просмотр определения предупреждений Ambari](media/hdinsight-cluster-availability/ambari-alerts-definitions.png)

Ambari предоставляет множество предварительно установленные предупреждения, связанные с доступностью, включая:

| Имя оповещения                        | Описание                                                                                                                                                                           |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Сводка по работоспособности DataNode           | Это предупреждение уровня службы активируется в том случае, если неработоспособное DataNodes                                                                                                                |
| Индекс NameNode высокой доступности | Это предупреждение уровня службы активируется в том случае, если Active NameNode или NameNode ждущий режим не запущена.                                                                              |
| Процент JournalNodes доступных    | Это предупреждение активируется в том случае, если число вниз JournalNodes в кластере больше, чем заданное Критическое пороговое значение. Он объединяет результаты JournalNode процесса проверки. |
| Процент DataNodes доступных       | Это предупреждение активируется в том случае, если число вниз DataNodes в кластере больше, чем заданное Критическое пороговое значение. Он объединяет результаты DataNode процесса проверки.       |

Полный список Ambari предупреждения, которое ведет наблюдение за справки, доступность кластера можно найти [здесь](https://docs.microsoft.com/azure/hdinsight/hdinsight-high-availability-linux#ambari-web-ui),

Чтобы просмотреть сведения о предупреждении или изменить условия, нажмите кнопку **имя** оповещения. Воспользоваться **Сводка по работоспособности DataNode** в качестве примера. Вы увидите описание оповещения, а также конкретные условия, которые будут активировать «предупреждение» или «critical» предупреждения и интервал проверки для критерия. Чтобы изменить конфигурацию, нажмите кнопку **изменить** кнопки в правом верхнем углу окно настройки.

![Конфигурация оповещений Ambari](media/hdinsight-cluster-availability/ambari-alert-configuration.png)

Здесь можно изменить описание и, что более важно, проверка интервал и пороговые значения для предупреждений или критических оповещений.

![Представление изменения конфигурации оповещений Ambari](media/hdinsight-cluster-availability/ambari-alert-configuration-edit.png)

В этом примере вы можете создать 2 неработоспособное DataNodes инициируется Критическое оповещение, а также 1 неработоспособное DataNode активировать только предупреждение. Нажмите кнопку **Сохранить** после редактирования.

### <a name="email-notifications"></a>Уведомления по электронной почте

При необходимости также можно настроить уведомления по электронной почте для предупреждения Ambari. Для этого, если работа ведется в **оповещения** , затем щелкните **действия** кнопку в левом верхнем углу, затем **управление уведомления.**

![Действие уведомления управление Ambari](media/hdinsight-cluster-availability/ambari-manage-notifications.png)

Откроется диалоговое окно для управления уведомлений об оповещениях. Нажмите кнопку **+** в нижней части диалогового окна и заполните обязательные поля, чтобы предоставить сведения о сервере, из которого необходимо отправлять сообщения электронной почты Ambari с адресом электронной почты.

> [!TIP]
> Настройка Ambari уведомления по электронной почте может быть хорошим способом получать оповещения в одном месте, при управлении большинства кластеров HDInsight.

## <a name="azure-monitor-logs-integration"></a>Интеграция журналов Azure Monitor

Azure Monitor регистрирует включает данные, создаваемые несколько ресурсов, таких как кластеры HDInsight, чтобы быть собраны и агрегированы в одном месте для достижения унифицированный мониторинг.

Перед началом необходимо будет рабочей области Log Analytics для хранения собранных данных. Если еще не создана, можно следовать приведенным ниже указаниям: [Создание рабочей области Log Analytics](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace).

### <a name="enable-hdinsight-azure-monitor-logs-integration"></a>Включение интеграции журналов HDInsight Azure Monitor

На странице ресурсов кластера HDInsight на портале, щелкните **Operations Management Suite** колонке. Щелкните **включить** и выберите рабочую область Log Analytics из раскрывающегося списка.

![Колонка HDInsight Operations Management Suite](media/hdinsight-cluster-availability/portal-enable-oms.png)

### <a name="query-metrics-and-logs-tables-in-the-logs-blade"></a>Запросы к таблицам метрик и журналов в колонке "журналы"

После включения интеграции журналов Azure Monitor (это может занять несколько минут), перейдите к вашей **рабочей области Log Analytics** ресурс и щелкните **журналы** колонке

![Колонка "журналы" рабочей области log Analytics](media/hdinsight-cluster-availability/portal-logs.png)

**Журналы** колонке содержится ряд примеры запросов, такие как:

| Имя запроса                      | Описание                                                               |
|---------------------------------|---------------------------------------------------------------------------|
| Доступность компьютеров сегодня    | Диаграмма количество компьютеров, отправляющих журналы, каждый час                     |
| Список периодических сигналов                 | Список всех пульса компьютера за последний час                           |
| Последний пакет пульса каждого компьютера | Показать последнего пульса, отправленных каждого компьютера                             |
| Недоступность компьютеров           | Вывод списка всех известных компьютеров, которые не отправляют пакеты пульса за последние 5 часов |
| Частота доступности               | Вычислить частоту доступности каждого подключенного компьютера                |

Например, запустите **тарифу** образец запроса, нажав кнопку **запуска** на основе этого запроса, как показано на снимке экрана выше. Отобразится скорость доступности каждого узла в кластере в процентах. При наличии нескольких кластеров HDInsight для отправки метрик в той же рабочей области Log Analytics, вы увидите, что скорость доступности для всех узлов в этих кластерах отображается.

![Журнал аналитики рабочей области журналы колонку «доступность курс» образец запроса](media/hdinsight-cluster-availability/portal-availability-rate.png)

> [!NOTE] 
> Частота доступности измеряется в течение 24-часового периода, поэтому кластера потребуется выполнить по крайней мере 24 часа, прежде чем вы увидите точные доступности тарифы.

Вы можете Закрепить эту таблицу для общей панели мониторинга, нажав кнопку **ПИН-код** в правом верхнем углу. Если у вас любой доступный для записи Общие панели мониторинга, вы увидите ее создания здесь: [Создание и совместное использование панелей мониторинга на портале Azure](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards#publish-and-share-a-dashboard).

### <a name="azure-monitor-alerts"></a>Оповещения Azure Monitor

Можно также настроить оповещения Azure Monitor, которые будут активировать, когда значение метрики или результаты запроса соответствуют определенным условиям. Например, давайте создадим оповещение, чтобы отправить сообщение электронной почты, когда один или несколько узлов еще не отправили пакеты пульса в 5 часов (т. е. будет считаться недоступной).

Из **журналы** колонке запуска **недоступность компьютеров** образец запроса, нажав кнопку **запуска** на основе этого запроса, как показано ниже.

![Журнал аналитики рабочей области журналы колонку «недоступность компьютеров» образец запроса](media/hdinsight-cluster-availability/portal-unavailable-computers.png)

Если все узлы, этот запрос должен возвращать 0 результатов сейчас. Нажмите кнопку **новое правило генерации оповещений** Чтобы приступить к настройке оповещения для этого запроса.

![Log Analytics рабочей области новое правило генерации оповещений](media/hdinsight-cluster-availability/portal-logs-new-alert-rule.png)

Три компонента на оповещение: *ресурсов* для которого необходимо создать правило (рабочей области Log Analytics в данном случае), *условие* для активации оповещения и *группы действий*  , определяющие, что произойдет при активации оповещения.

Нажмите кнопку **условие title**, как показано ниже, чтобы завершить настройку логики сигнала.

![Условие правила генерации оповещений](media/hdinsight-cluster-availability/portal-condition-title.png)

Откроется **настройка логики сигнала** колонке.

Задайте **логику оповещений** разделе следующим образом:

*На основе: Количество результатов, условие: Выше порогового значения: 0.*

Так как этот запрос возвращает только недоступные узлы как результатов, если количество результатов, когда-либо больше, чем 0, должен срабатывать оповещение.

В **оценено, на основе** задайте **период** и **частота** зависимости от того, как часто требуется проверить для недоступных узлов.

Обратите внимание, что для целей этого предупреждения, вы хотите убедитесь, что **Period = частоты.** Дополнительные сведения о период, частоту и другие параметры оповещений можно найти [здесь](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log#log-search-alert-rule---definition-and-types).

Нажмите кнопку **сделать** когда вы закончите, настройка логики сигнала.

![Правило генерации оповещений настроить логику сигналов](media/hdinsight-cluster-availability/portal-configure-signal-logic.png)

Если у вас еще нет существующей группы действий, щелкните **Create New** под **группы действий** раздел.

![Новая группа действий правила генерации оповещений](media/hdinsight-cluster-availability/portal-create-new-action-group.png)

Откроется **добавить группу действий** колонке. Выберите **имя группы действий**, **короткое имя**, **подписки**, и **группы ресурсов.** В разделе **действия** выберите **имя действия** и выберите **электронной почты/SMS/Push/Voice** как **тип действия.**

> [!NOTE]
> Существует несколько других действий, которые оповещение может активироваться помимо электронной почты/SMS/Push/Voice, такие как функции Azure, приложения логики, веб-перехватчик, ITSM и Runbook службы автоматизации. [Подробнее.](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups#action-specific-information)

Откроется **электронной почты/SMS/Push/Voice** колонке. Выберите **имя** получателя, **проверьте** **электронной почты** и введите адрес электронной почты, к которому будет отправлено оповещение. Нажмите кнопку **ОК** в **электронной почты/SMS/Push/Voice** колонки, а затем в **добавить группу действий** колонку, чтобы завершить настройку вашей группы действий.

![Правило генерации оповещений добавить группу действий](media/hdinsight-cluster-availability/portal-add-action-group.png)

После закрытия этих колонках, вы увидите вашей группы действий, перечисленных в разделе **группы действий** раздел. Наконец, завершите **сведениях о предупреждении** раздел, введя **имя правила оповещения** и **описание** и выбрав **серьезность**.
Нажмите кнопку **Создание правила генерации оповещений** до конца.

![Создание правила генерации оповещений Готово](media/hdinsight-cluster-availability/portal-create-alert-rule-finish.png)

> [!TIP]
> Возможность указывать **серьезность** — это мощный инструмент, который может использоваться при создании нескольких предупреждений. Например можно создать одно предупреждение инициирования предупреждения (уровень серьезности 1), если один головной узел выходит из вниз, а еще одно оповещение, который вызывает критическое (уровень серьезности 0), маловероятно, что оба головных узла из строя.

При выполнении условия для этого предупреждения, предупреждение будет срабатывать, и вы получите сообщение электронной почты с описанием следующим образом:

![Azure Monitor оповещения по электронной почте](media/hdinsight-cluster-availability/alert-email.png)

Можно также просмотреть все оповещения, которые были активированы, сгруппированные по уровню серьезности, перейдя на **оповещения** колонки вашего **рабочей области Log Analytics**.

![Оповещения log Analytics рабочей области](media/hdinsight-cluster-availability/portal-alerts.png)

Щелкнув серьезности группирование (т. е. **1 уровень серьезности,** как показано выше) будет отображаться записи для всех оповещений, уровень серьезности, который сработавшими триггерами как ниже:

![Оповещения log Analytics workspace уровень серьезности 1](media/hdinsight-cluster-availability/portal-alerts-sev-1.png)

## <a name="next-steps"></a>Следующие шаги
- [Доступность и надежность кластеров Apache Hadoop в HDInsight](hdinsight-high-availability-linux.md)
