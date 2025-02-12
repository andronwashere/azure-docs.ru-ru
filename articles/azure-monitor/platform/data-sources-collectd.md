---
title: Сбор данных CollectD в Azure Monitor| Документация Майкрософт
description: CollectD — управляющая программа Linux с открытым исходным кодом, которая периодически собирает данные приложений и системные данные.  В этой статье приведены сведения о сборе данных CollectD в Azure Monitor.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: f1d5bde4-6b86-4b8e-b5c1-3ecbaba76198
ms.service: log-analytics
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/27/2018
ms.author: magoedte
ms.openlocfilehash: 2118f137f2c0d32f891a170c3509bceee7ba13ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60764960"
---
# <a name="collect-data-from-collectd-on-linux-agents-in-azure-monitor"></a>Сбор данных CollectD с помощью агентов Linux в Azure Monitor
[CollectD](https://collectd.org/) — управляющая программа Linux с открытым исходным кодом, которая периодически собирает метрики производительности приложений и системные данные. К примерам таких приложений относятся виртуальная машина Java (JVM), сервер MySQL и Nginx. В этой статье приводятся сведения о сборе данных производительности CollectD в Azure Monitor.

Полный список доступных подключаемых модулей можно найти в [таблице подключаемых модулей](https://collectd.org/wiki/index.php/Table_of_Plugins).

![Обзор CollectD](media/data-sources-collectd/overview.png)

В состав агента Log Analytics для Linux включена следующая конфигурация CollectD, которая перенаправляет данные CollectD в агент Log Analytics для Linux.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Если используется CollectD версии 5.5 или более ранней версии, используйте следующую конфигурацию вместо указанной.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

В конфигурации CollectD для отправки данных производительности агенту Log Analytics для Linux через порт 26000 по умолчанию используется подключаемый модуль `write_http`. 

> [!NOTE]
> При желании номер порта можно изменить.

Агент Log Analytics для Linux также ожидает передачи данных с порта 26000 для сбора метрик CollectD, а затем преобразует эти метрики в метрики схемы Azure Monitor. Ниже приведена конфигурация агента Log Analytics для Linux `collectd.conf`.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>


## <a name="versions-supported"></a>Поддерживаемые версии
- Azure Monitor сейчас поддерживает CollectD версии 4.8 и более поздней.
- Для сбора метрик CollectD необходим агент Log Analytics для Linux версии 1.1.0-217 или более поздней.


## <a name="configuration"></a>Параметр Configuration
Ниже приведены основные шаги по настройке сбора данных CollectD в Azure Monitor.

1. Настройте отправку данных CollectD в агент Log Analytics для Linux с помощью подключаемого модуля write_http.  
2. Настройте агент Log Analytics для Linux для прослушивания данных CollectD на соответствующем порте.
3. Перезапустите CollectD и агент Log Analytics для Linux.

### <a name="configure-collectd-to-forward-data"></a>Настройка пересылки данных в CollectD 

1. Для пересылки данных CollectD агенту Log Analytics для Linux необходимо добавить файл `oms.conf` в каталог конфигурации файлов CollectD. Местоположение этого файла зависит от используемого дистрибутива Linux.

    Если конфигурационные файлы CollectD находятся в папке /etc/collectd.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    Если конфигурационные файлы CollectD находятся в папке /etc/collectd/collectd.conf.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >Если используется CollectD версии 5.5 или более ранней версии, потребуется изменить теги в `oms.conf`, как показано выше.
    >

2. Скопируйте файл collectd.conf в конфигурационный каталог omsagent в соответствующей рабочей области.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Перезапустите CollectD и агент Log Analytics для Linux, выполнив следующие команды.

    sudo service collectd restart  sudo /opt/microsoft/omsagent/bin/service_control restart

## <a name="collectd-metrics-to-azure-monitor-schema-conversion"></a>Метрики CollectD для преобразования в метрики схемы Azure Monitor
Для сохранения знакомой модели между метриками инфраструктуры, уже собранными агентом Log Analytics для Linux, и новыми метриками, собранными CollectD, используется следующая схема сопоставления.

| Поле метрики CollectD | Поле Azure Monitor |
|:--|:--|
| `host` | Computer |
| `plugin` | Нет |
| `plugin_instance` | Имя экземпляра<br>Если **plugin_instance** имеет значение *null*, то InstanceName=" *_Total*" |
| `type` | ObjectName |
| `type_instance` | CounterName<br>Если **type_instance** имеет значение *null*, то CounterName=**blank** |
| `dsnames[]` | CounterName |
| `dstypes` | Нет |
| `values[]` | CounterValue |

## <a name="next-steps"></a>Дальнейшие действия
* Узнайте больше о [запросах журнала](../log-query/log-query-overview.md), которые можно применять для анализа данных, собираемых из источников данных и решений. 
* Используйте [настраиваемые поля](custom-fields.md) для анализа данных из записей системного журнала в отдельных полях.
