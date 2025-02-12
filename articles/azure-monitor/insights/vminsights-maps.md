---
title: Просмотр зависимостей приложения с помощью Azure Monitor для виртуальных машин (предварительная версия) | Документация Майкрософт
description: Карта — это функция Azure Monitor для виртуальных машин. Он автоматически обнаруживает компоненты приложений в системах Windows и Linux и сопоставляет взаимодействие между службами. Эта статья содержит сведения о том, как использовать функцию сопоставления в различных сценариях.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/09/2019
ms.author: magoedte
ms.openlocfilehash: f6273e9b6c7ed0c4685479976343497f01201b0b
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67206755"
---
# <a name="use-the-map-feature-of-azure-monitor-for-vms-preview-to-understand-application-components"></a>Использовать функцию карты Azure Monitor для виртуальных машин (Предварительная версия), чтобы понять компоненты приложения
В Azure Monitor для виртуальных машин можно просмотреть компоненты обнаруженных приложений на Windows и Linux виртуальных машинах (ВМ), работающих в Azure или в вашей среде. Можно наблюдать за виртуальные машины одним из двух способов. Просмотреть карту непосредственно из виртуальной Машины или карту из Azure Monitor для просмотра компонентов между группами виртуальных машин. Эта статья поможет вам понять эти два метода просмотра и как использовать функции карты. 

Сведения о настройке Azure Monitor для виртуальных машин, см. в [этой статье](vminsights-enable-overview.md).

## <a name="sign-in-to-azure"></a>Вход в Azure
Войдите на [портале Azure](https://portal.azure.com).

## <a name="introduction-to-the-map-experience"></a>Общие сведения о версии схемы
Прежде чем углубляться в версии схемы, необходимо понимать, как она представляет и визуализирует сведения. При выборе функции карты, непосредственно из виртуальной Машины или Azure Monitor, функция карты предоставляет согласованную работу. Единственная разница в, из Azure Monitor, одной карте будут показаны все элементы многоуровневого приложения или кластера.

Функция карты наглядно представляет зависимости виртуальной Машины путем обнаружения процессов, имеющих: 

- Активных сетевых подключений между серверами.
- Задержка входящего и исходящего подключения.
- Порты, через любой архитектуры подключенных по протоколу TCP через определенного диапазона времени.  
 
Разверните виртуальную Машину, чтобы отобразить сведения о процессе и только те процессы, которые обмениваются данными с виртуальной Машиной. Группа клиентов отображается количество первоначальными клиентами, которые подключаются к виртуальной машине. Группы портов серверов Отображение числа внутренних серверов, к которому подключается виртуальная машина. Раскройте группу порт сервера, чтобы просмотреть подробный список серверов, подключенных через этот порт.  

При выборе виртуальной Машины, **свойства** на правой панели отображаются свойства виртуальной Машины. Свойства включают сведения о системе, о которых сообщает операционная система, свойства виртуальной Машины Azure и кольцевую диаграмму, перечислены обнаруженные подключения. 

![Панель свойств](./media/vminsights-maps/properties-pane-01.png)

В правой части области, выберите **события журнала** для отображения списка данных, который отправлен виртуальной Машины в Azure Monitor. Эти данные доступны для запросов.  Выберите любой тип записи, чтобы открыть **журналы** страницы, где вы увидите результаты для этого типа записи. Вы также увидите предварительно настроенных запрос, который фильтруется по виртуальной Машины.  

![Область событий журнала](./media/vminsights-maps/properties-pane-logs-01.png)

Закрыть **журналы** странице и вернуться к **свойства** области. Выберите **оповещения** для просмотра оповещений, критерии работоспособности виртуальной Машины. Функция карты интегрируется с оповещениями Azure для отображения оповещений для выбранного сервера за выбранный диапазон времени. На сервере отображается значок для текущих оповещений и **оповещений на машине** отображаются в области. 

![На панели оповещений](./media/vminsights-maps/properties-pane-alerts-01.png)

Чтобы сделать функцию карты Отображение важных предупреждений, создайте правило генерации оповещений, которое применяется к конкретному компьютеру:

- Добавьте предложение группировку предупреждений по компьютера (например, **по Computer interval 1 минута**).
- Базовые оповещения на основе метрики.

Дополнительные сведения об оповещениях Azure и создание правил генерации оповещений см. в разделе [единой оповещения в Azure Monitor](../../azure-monitor/platform/alerts-overview.md).

В правом верхнем углу **условных обозначений** на карте описывает символы и ролей. Изучить их подробнее в карту и переместите ее используйте элементы управления масштабом в правом нижнем углу. Можно задать масштаб и карта помещалась в размер страницы.  

## <a name="connection-metrics"></a>Метрики подключения
**Подключений** панели отображаются стандартные метрики для выделенного соединения из виртуальной Машины через TCP-порт. Среди них имеются метрики времени отклика, количества запросов в минуту, пропускной способности трафика и каналов.  

![Диаграмм подключения к сети, в области «соединения»](./media/vminsights-maps/map-group-network-conn-pane-01.png)  

### <a name="failed-connections"></a>Неудачные подключения
На карте показаны неудачных попыток соединения компьютеров и процессов. Пунктирная красная линия указывает, что не удается клиентской системы к процессу или порту. Для систем, использующих агент зависимостей агент сообщает о неудачных попыток подключения. Функция карты контролирует процесс путем наблюдения за сокетами TCP, которым не удалось установить соединение. Этот сбой может быть следствием брандмауэром, неправильными настройками клиента или сервера или недоступным удаленной службы.

![Не удалось подключиться на карте](./media/vminsights-maps/map-group-failed-connection-01.png)

Основные сведения о неудачных попыток соединения может помочь Устранение неполадок, проверить миграцию, анализ безопасности и понять, общая архитектура службы. Неудачные подключения безвредны, иногда, но зачастую они указывают на проблему. Подключения может завершиться ошибкой, например, когда среды отработки отказа внезапно становится недоступным или когда двумя уровнями приложения не могут взаимодействовать друг с другом после миграции в облако.

### <a name="client-groups"></a>Группы клиентов
На карте клиентские группы представляют клиентских компьютеров, которые подключаются к сопоставленной машины. Одной группы клиентов представляет клиентов для отдельного процесса или компьютера.

![Группы клиентов на карте](./media/vminsights-maps/map-group-client-groups-01.png)

Чтобы увидеть отслеживаемых клиентов и IP-адреса из систем в группе клиентов, выберите группу. Содержимое группы отображается ниже.  

![Группы клиентов список IP-адресов на карте](./media/vminsights-maps/map-group-client-group-iplist-01.png)

Если группа включает клиенты отслеживаемых и не отслеживается, можно выбрать соответствующий раздел кольцевой диаграмме группы для фильтрации клиентов.

### <a name="server-port-groups"></a>Группы портов серверов
Группы портов серверов представляют порты на серверах, содержащих входящих подключений из сопоставленного компьютера. Группа содержит порт сервера и число серверов, которые подключены к этому порту. Выберите группу, чтобы просмотреть отдельные серверы и подключения. 

![Порт сервера группы на карте](./media/vminsights-maps/map-group-server-port-groups-01.png)  

Если группа включает серверы отслеживаемых и не отслеживается, можно выбрать соответствующий раздел кольцевой диаграмме группы для фильтрации серверов.

## <a name="view-a-map-from-a-vm"></a>Просмотр карты от виртуальной Машины 

Для доступа к Azure Monitor для виртуальных машин непосредственно из виртуальной Машины:

1. На портале Azure выберите **Виртуальные машины**. 
2. В списке выберите виртуальную Машину. В **мониторинг** выберите **Insights (Предварительная версия)** .  
3. Откройте вкладку **Схема**.

Схеме наглядно представляет зависимости виртуальной Машины, обнаруживая под управлением группы процессов и процессов с активными сетевыми подключениями через определенного диапазона времени.  

По умолчанию на схеме показаны последние 30 минут. Если вы хотите см. в разделе о зависимостях в прошлом, вы можете запрашивать различных временных интервалов до одного часа. Чтобы выполнить запрос, используйте **TimeRange** селектора в левом верхнем углу. Можно запустить запрос, например, во время инцидента или чтобы увидеть состояние до изменения.  

![Общие сведения о непосредственной схеме виртуальной машины](./media/vminsights-maps/map-direct-vm-01.png)

## <a name="view-a-map-from-a-virtual-machine-scale-set"></a>Просмотр карты из масштабируемого набора виртуальных машин

Для доступа к Azure Monitor для виртуальных машин непосредственно из масштабируемого набора виртуальных машин:

1. На портале Azure выберите **масштабируемые наборы виртуальных машин**.
2. В списке выберите виртуальную Машину. Затем в **мониторинг** выберите **Insights (Предварительная версия)** .  
3. Откройте вкладку **Схема**.

Карты визуализирует всех экземпляров в масштабируемом наборе, как узел группы вместе с зависимостей группы. Развернутый узел список экземпляров в масштабируемом наборе. Вы можете просмотреть эти экземпляры 10 одновременно. 

Чтобы загрузить карту для конкретного экземпляра, сначала необходимо выберите этот экземпляр, на карте. Затем выберите **кнопку с многоточием** кнопку (...) справа и выберите **загрузить карту сервера**. На карте, который отображается вы увидите, как группы процессов и процессы с активными сетевыми подключениями через определенного диапазона времени. 

По умолчанию на схеме показаны последние 30 минут. Если вы хотите см. в разделе о зависимостях в прошлом, вы можете запрашивать различных временных интервалов до одного часа. Чтобы выполнить запрос, используйте **TimeRange** селектор. Можно запустить запрос, например, во время инцидента или чтобы увидеть состояние до изменения.

![Общие сведения о непосредственной схеме виртуальной машины](./media/vminsights-maps/map-direct-vmss-01.png)

>[!NOTE]
>Также можно использовать карту для конкретного экземпляра из **экземпляров** представление для масштабируемого набора виртуальных машин. В **параметры** разделе, перейдите к **экземпляров** > **Insights (Предварительная версия)** .

## <a name="view-a-map-from-azure-monitor"></a>Просмотр карты из Azure Monitor
В Azure Monitor функция карты обеспечивает глобальное представление о ваших виртуальных машин и их зависимости. Для доступа к компоненту карты в Azure Monitor:

1. На портале Azure выберите **Монитор**. 
2. В **Insights** выберите **виртуальных машин (Предварительная версия)** .
3. Откройте вкладку **Схема**.

   ![Azure map монитор обзор множества виртуальных машин](./media/vminsights-maps/map-multivm-azure-monitor-01.png)

Выберите рабочую область с помощью **рабочей области** селектор в верхней части страницы. При наличии более одной рабочей области Log Analytics выберите рабочую область, которая включена с помощью решения и с виртуальным машинам, которые он. 

**Группы** селектора возвращает подписки, группы ресурсов, [групп компьютеров](../../azure-monitor/platform/computer-groups.md)и масштабируемые наборы виртуальных машин компьютеров, которые связаны с выбранной рабочей области. Выбор применяется только к функции карты и не применяется к производительности и работоспособности.

По умолчанию на схеме показаны последние 30 минут. Если вы хотите см. в разделе о зависимостях в прошлом, вы можете запрашивать различных временных интервалов до одного часа. Чтобы выполнить запрос, используйте **TimeRange** селектор. Можно запустить запрос, например, во время инцидента или чтобы увидеть состояние до изменения.  

## <a name="next-steps"></a>Дальнейшие действия
- Чтобы узнать, как использовать функцию работоспособности, см. в разделе [работоспособность виртуальной Машины Azure представление](vminsights-health.md). 
- Для выявления узких мест, проверьте производительность и понять общее использование виртуальных машин, см. в разделе [просмотреть состояние производительности для Azure Monitor для виртуальных машин](vminsights-performance.md). 
