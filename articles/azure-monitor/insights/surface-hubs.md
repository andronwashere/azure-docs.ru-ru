---
title: Мониторинг устройств Surface Hub с помощью Azure Monitor | Документация Майкрософт
description: Решение Surface Hub позволяет отслеживать работоспособность устройств Surface Hub и понимать, как они используются.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: 8b4e56bc-2d4f-4648-a236-16e9e732ebef
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/16/2018
ms.author: magoedte
ms.openlocfilehash: 7e0dbb4c3cd8ae4bb552e7b7f0748f1bde2f51de
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65232783"
---
# <a name="monitor-surface-hubs-with-azure-monitor-to-track-their-health"></a>Мониторинг устройств Surface Hub с помощью Azure Monitor для отслеживания работоспособности

![Символ решения Surface Hub](./media/surface-hubs/surface-hub-symbol.png)

В этой статье описывается использование решения Surface Hub в службе Azure Monitor для мониторинга устройств Microsoft Surface Hub. Это решение позволяет отслеживать работоспособность устройств Surface Hub и понимать, как они используются.

На каждом устройстве Surface Hub установлен агент Microsoft Monitoring Agent. Именно с помощью этого агента вы можете отправлять данные с устройства Surface Hub в рабочую область Log Analytics в Azure Monitor. Файлы журнала считываются с устройств Surface Hub и затем отправляются в Azure Monitor. Проблемы, например отключение серверов от сети, отсутствие синхронизации календаря или невозможность войти в Skype с использованием учетной записи устройства, отображаются в Azure Monitor на панели мониторинга Surface Hub. С помощью данных на панели мониторинга можно определить устройства, которые не работают или столкнулись с другими проблемами, и, возможно, применить исправления обнаруженных проблем.

## <a name="install-and-configure-the-solution"></a>Установка и настройка решения
Для установки и настройки решений используйте указанные ниже данные. Для управления устройствами Surface Hub в Azure Monitor потребуется следующее.

* Уровень [подписки Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/), поддерживающий количество устройств, которые требуется отслеживать. Цены на Log Analytics зависят от количества зарегистрированных устройств и объема обрабатываемых данных. Это необходимо учитывать при планировании развертывания Surface Hub.

Затем следует добавить существующую рабочую область Log Analytics или создать новую. Подробные инструкции по применению обоих методов см. в статье [Создание рабочей области Log Analytics на портале Azure](../learn/quick-create-workspace.md). После настройки рабочей области Log Analytics регистрировать устройства Surface Hub можно двумя способами:

* автоматически с помощью InTune;
* вручную в приложении **Settings** на устройстве Surface Hub.

## <a name="set-up-monitoring"></a>Настройка мониторинга
Вы можете отслеживать работоспособность и активность устройства Surface Hub с помощью Azure Monitor. Устройство Surface Hub можно зарегистрировать с помощью Intune или локально, с помощью **параметров** устройства Surface Hub.

## <a name="connect-surface-hubs-to-azure-monitor-through-intune"></a>Подключение устройств Surface Hub к Azure Monitor с помощью Intune
Вам потребуется идентификатор и ключ для рабочей области Log Analytics, которая будет управлять устройствами Surface Hub. Их можно получить на портале Azure в разделе параметров рабочей области.

Intune — это продукт Microsoft, который позволяет централизованно управлять параметрами конфигурации рабочей области Log Analytics, применяемыми к одному или нескольким устройствам. Чтобы настроить устройства через Intune, выполните следующие действия:

1. Войдите в Intune.
2. Перейдите в раздел **Settings (Параметры)**  > **Connected Sources (Подключенные источники)** .
3. Создайте или отредактируйте политику на основе шаблона Surface Hub.
4. Перейдите к разделу политики "Оперативная аналитика Azure" и добавьте в нее *идентификатор рабочей области* и *ключ рабочей области* Log Analytics.
5. Сохраните политику.
6. Свяжите политику с соответствующей группой устройств.

   ![Политика Intune](./media/surface-hubs/intune.png)

Затем Intune синхронизирует параметры Log Analytics с устройствами в целевой группе, регистрируя их в рабочей области Log Analytics.

## <a name="connect-surface-hubs-to-azure-monitor-using-the-settings-app"></a>Подключение устройств Surface Hub к Azure Monitor с помощью приложения Settings
Вам потребуется идентификатор и ключ для рабочей области Log Analytics, которая будет управлять устройствами Surface Hub. Их можно получить в разделе параметров рабочей области Log Analytics на портале Azure.

Если для управления средой не используется Intune, вы можете зарегистрировать устройства вручную в **параметрах** каждого устройства Surface Hub:

1. На устройстве Surface Hub откройте приложение **Settings**.
2. Введите учетные данные администратора устройства при появлении запроса.
3. Щелкните **This device** (Это устройство), затем в разделе **Monitoring** (Мониторинг) нажмите кнопку **Configure Log Analytics Settings** (Настроить параметры Log Analytics).
4. Выберите **Enable monitoring** (Включить мониторинг).
5. В диалоговом окне параметров Log Analytics введите **идентификатор рабочей области** и **ключ рабочей области**.  
   ![параметры](./media/surface-hubs/settings.png)
6. Нажмите кнопку **ОК**, чтобы завершить настройку.

Появится подтверждение с сообщением о том, удалось ли применить конфигурацию к устройству. Если удалось, появится сообщение о том, что агент успешно подключен к Azure Monitor. Затем устройство начнет отправку данных в Azure Monitor, где вы можете просматривать и обрабатывать эти данные.

## <a name="monitor-surface-hubs"></a>Мониторинг устройств Surface Hub
Мониторинг устройств Surface Hub с помощью Azure Monitor во многом напоминает мониторинг любых других зарегистрированных устройств.

[!INCLUDE [azure-monitor-solutions-overview-page](../../../includes/azure-monitor-solutions-overview-page.md)]

При нажатии на плитку "Surface Hub" отобразится работоспособность вашего устройства.

   ![Панель мониторинга Surface Hub](./media/surface-hubs/surface-hub-dashboard.png)

Можно создавать [оповещения](../platform/alerts-overview.md) на основе существующих или настраиваемых поисковых запросов к журналу. С помощью данных, собираемых службой Azure Monitor с устройств Surface Hub, вы можете выполнять поиск проблем и получать оповещения, которые отправляются при выполнении условий, заданных вами для устройств.

## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения о данных Surface Hub см. в статье [Анализ данных Log Analytics в Azure Monitor](../log-query/log-query-overview.md).
* Создайте [оповещения](../platform/alerts-overview.md), которые будут уведомлять вас о возникновении проблем с устройствами Surface Hub.
