---
title: Журналы Azure Service Fabric. мониторинг производительности с помощью Azure Monitor | Документация Майкрософт
description: Сведения о том, как настраивать агент Log Analytics для наблюдения за контейнерами и счетчиками производительности для кластеров Azure Service Fabric.
services: service-fabric
documentationcenter: .net
author: srrengar
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/16/2018
ms.author: srrengar
ms.openlocfilehash: 819f6ee4ab079361279a567bceeb74c33fe14186
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60952385"
---
# <a name="performance-monitoring-with-azure-monitor-logs"></a>Мониторинг производительности с помощью журналов Azure Monitor

В этой статье описаны шаги, позволяющие добавить агент Log Analytics как расширение масштабируемого набора виртуальных машин в кластер и подключить его к существующей рабочей области Azure Log Analytics. Это активирует сбор данных диагностики о контейнерах и приложениях, а также мониторинг производительности. Если вы добавите агент в качестве расширения ресурса масштабируемого набора виртуальных машин, Azure Resource Manager гарантирует его установку на каждом узле, даже при масштабировании кластера.

> [!NOTE]
> В этой статье предполагается наличие настроенной рабочей области Azure Log Analytics. Если этого не сделать, перейдите к [настроить журналы Azure Monitor](service-fabric-diagnostics-oms-setup.md)

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="add-the-agent-extension-via-azure-cli"></a>Добавление расширения агента через Azure CLI

Добавить агент Log Analytics в кластер лучше всего через API масштабируемого набора виртуальных машин, доступные в Azure CLI. Если Azure CLI еще не настроен, перейдите на портал Azure и откройте экземпляр [Cloud Shell](../cloud-shell/overview.md) или [установите Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

1. Как только придет запрос Cloud Shell, убедитесь, что вы работаете в той же подписке, что и ресурс. Проверьте это с помощью `az account show` и убедитесь, что значение "name" соответствует подписке кластера.

2. На портале перейдите к группе ресурсов, где находится рабочая область Log Analytics. Щелкните ресурс log analytics (тип ресурса будет рабочей области Log Analytics). Перейдя на страницу обзора ресурсов, щелкните **Дополнительные настройки** в разделе "Параметры" в меню слева.

    ![Страница свойств log analytics](media/service-fabric-diagnostics-oms-agent/oms-advanced-settings.png)

3. Щелкните **Серверы с Windows**, если используется кластер Windows, и **Серверы с Linux**, если вы создаете кластер Linux. На этой странице отображаются ваши `workspace ID` и `workspace key` (отображается на портале как первичный ключ). Они необходимы для выполнения следующего шага.

4. Выполните команду, чтобы установить агент Log Analytics в кластере с помощью API `vmss extension set` в Cloud Shell:

    Для кластера Windows:

    ```sh
    az vmss extension set --name MicrosoftMonitoringAgent --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Для кластера Linux:

    ```sh
    az vmss extension set --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType> --settings "{'workspaceId':'<Log AnalyticsworkspaceId>'}" --protected-settings "{'workspaceKey':'<Log AnalyticsworkspaceKey>'}"
    ```

    Ниже приведен пример агента Log Analytics, добавляемого в кластер Windows.

    ![Команда CLI агента Log Analytics](media/service-fabric-diagnostics-oms-agent/cli-command.png)

5. Добавление агента на узлы займет меньше 15 минут. Вы можете проверить, что агенты были добавлены, с помощью API `az vmss extension list`:

    ```sh
    az vmss extension list --resource-group <nameOfResourceGroup> --vmss-name <nameOfNodeType>
    ```

## <a name="add-the-agent-via-the-resource-manager-template"></a>Добавление агента через шаблон Resource Manager

Примеры шаблонов Resource Manager, при помощи которых развертывается рабочую область Azure Log Analytics и добавляется агент на каждый из узлов, доступны для [Windows](https://github.com/Azure-Samples/service-fabric-cluster-templates/tree/master/5-VM-Windows-OMS-UnSecure) и [Linux](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux).

Вы можете скачать и изменить такой шаблон для развертывания кластера с учетом ваших требований.

## <a name="view-performance-counters"></a>Просмотр счетчиков производительности

После добавления агента Log Analytics перейдите на портал Log Analytics, чтобы выбрать счетчики производительности, данные которых необходимо собирать.

1. На портале Azure перейдите в группу ресурсов, в которой вы создали решение "Аналитика Service Fabric". Выберите **ServiceFabric\<имя_рабочей_области_Log_Analytics\>** .

2. Щелкните **Log Analytics**.

3. Нажмите кнопку **Дополнительные параметры**.

4. Нажмите кнопку **Данные**, затем нажмите кнопку **Счетчики производительности Windows или Linux**. Существует список счетчиков по умолчанию, которые можно выбрать. Вы также можете задать интервал коллекции. Можно также выбрать [дополнительные счетчики производительности](service-fabric-diagnostics-event-generation-perf.md), чтобы они выполняли сбор данных. Правильный формат см. в [этой статье](https://msdn.microsoft.com/library/windows/desktop/aa373193(v=vs.85).aspx).

5. Нажмите кнопку **Сохранить**, затем нажмите кнопку **ОК**.

6. Закройте колонку "Дополнительные параметры".

7. Под заголовком General (Общие) щелкните **Сводка рабочей области**.

8. Вы увидите плитки в форме графа для каждого включенного решения, включая одну для Service Fabric. Щелкните граф **Service Fabric**, чтобы продолжить решение службы Fabric Analytics.

9. Вы увидите несколько плиток с графами на рабочем канале и надежными событиями служб. Графическое представление данных, поступающих от выбранных счетчиков, будет отображаться в метриках узла.

10. Для просмотра дополнительных сведений щелкните граф метрики контейнера. Вы можете запрашивать данные счетчиков производительности аналогично событиям кластера и задавать фильтры по узлам, именам счетчиков производительности и значениям с помощью языка запросов Kusto.

![Запрос счетчика производительности Log Analytics](media/service-fabric-diagnostics-event-analysis-oms/oms_node_metrics_table.PNG)

## <a name="next-steps"></a>Дальнейшие действия

* Включите сбор соответствующих [счетчиков производительности](service-fabric-diagnostics-event-generation-perf.md). Чтобы настроить агент Log Analytics для сбора данных определенных счетчиков производительности, ознакомьтесь с разделом [Настройка источников данных](../azure-monitor/platform/agent-data-sources.md#configuring-data-sources).
* Настройка журналов Azure Monitor для настройки [автоматические оповещения](../log-analytics/log-analytics-alerts.md) для помогают выполнять обнаружение и диагностика
* В качестве альтернативы можно собирать счетчики производительности с помощью [расширения Диагностики Azure и отправлять их в Application Insight](service-fabric-diagnostics-event-aggregation-wad.md#add-the-application-insights-sink-to-the-resource-manager-template).
