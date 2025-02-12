---
title: Отправлять оповещения о работоспособности служб Azure с помощью веб-перехватчиков OpsGenie
description: Передача в экземпляр OpsGenie персонализированных уведомлений о событиях работоспособности служб.
author: stephbaron
ms.author: stbaron
ms.topic: article
ms.service: service-health
ms.date: 06/10/2019
ms.openlocfilehash: fab99b7093ac3f18f6313273d21905e0a3ed7e5b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67067168"
---
# <a name="send-azure-service-health-alerts-with-opsgenie-using-webhooks"></a>Отправлять оповещения о работоспособности служб Azure с помощью веб-перехватчиков OpsGenie

В этой статье показано, как с помощью веб-перехватчика настроить в OpsGenie оповещения о работоспособности служб Azure. Используя интеграцию работоспособности служб Azure в [OpsGenie](https://www.opsgenie.com/), можно перенаправлять в OpsGenie оповещения о работоспособности служб Azure. OpsGenie может определить, каких лиц следует уведомить, на основе расписаний по вызовам, используя электронную почту, текстовые сообщения (SMS), телефонные звонки, push-уведомления iOS и Android и выполняя эскалацию оповещений до их подтверждения или закрытия.

## <a name="creating-a-service-health-integration-url-in-opsgenie"></a>Создание URL-адреса интеграции работоспособности служб в OpsGenie
1.  Убедитесь, что у вас есть учетная запись [OpsGenie](https://www.opsgenie.com/), и выполните вход в нее.

1.  Перейдите в раздел **Integrations** (Интеграции) в OpsGenie.

    ![Раздел интеграций в OpsGenie](./media/webhook-alerts/opsgenie-integrations-section.png)

1.  Выберите кнопку интеграции **Azure Service Health** (Работоспособность служб Azure).

    ![Кнопка Работоспособность служб Azure (Работоспособность служб Azure) в OpsGenie](./media/webhook-alerts/opsgenie-azureservicehealth-button.png)

1.  Задайте **имя** для оповещения и укажите значение в поле **Assigned to Team** (Назначено команде).

1.  Заполните остальные поля, такие как **Recipients** (Получатели), **Enabled** (Включено) и **Suppress Notifications** (Подавлять уведомления).

1.  Скопируйте и сохраните **URL-адрес интеграции**, который уже должен содержать в конце `apiKey`.

    ![URL-адрес интеграции в OpsGenie](./media/webhook-alerts/opsgenie-integration-url.png)

1.  Выберите **Save Integration**(Сохранить интеграцию).

## <a name="create-an-alert-using-opsgenie-in-the-azure-portal"></a>Создание оповещения с помощью OpsGenie на портале Azure
### <a name="for-a-new-action-group"></a>Для новой группы действий сделайте следующее.
1. Выполните шаги 1–8 из раздела [Создание оповещения для уведомления о работоспособности службы для новой группы действий с помощью портала Azure](../azure-monitor/platform/alerts-activity-log-service-notifications.md).

1. В списке **Действия** определите следующие сведения.

    a. **Тип действия:** *webhook*

    2\. **Подробности:** **URL-адрес интеграции** для OpsGenie, который вы сохранили ранее.

    c. **Имя:** имя, псевдоним или идентификатор веб-перехватчика.

1. Завершив создание оповещения, нажмите кнопку **Сохранить**.

### <a name="for-an-existing-action-group"></a>Для существующей группы действий сделайте следующее.
1. На [портале Azure](https://portal.azure.com/) выберите **Монитор**.

1. В разделе **Параметры** выберите **Группы действий**.

1. Найдите и выберите группу действий, для которой нужно изменить сведения.

1. В список **Действия** добавьте следующие сведения.

    a. **Тип действия:** *webhook*

    2\. **Подробности:** **URL-адрес интеграции** для OpsGenie, который вы сохранили ранее.

    c. **Имя:** имя, псевдоним или идентификатор веб-перехватчика.

1. Завершив изменение группы действий, нажмите кнопку **Сохранить**.

## <a name="testing-your-webhook-integration-via-an-http-post-request"></a>Проверка интеграции с веб-перехватчиком с помощью запроса HTTP POST
1. Создайте полезные данные о работоспособности служб, которые хотите отправить. Пример полезных данных для веб-перехватчика службы работоспособности служб см. в статье [Веб-перехватчики для оповещений журнала действий Azure](../azure-monitor/platform/activity-log-alerts-webhook.md).

1. Создайте запрос HTTP POST следующим образом:

    ```
    POST        https://api.opsgenie.com/v1/json/azureservicehealth?apiKey=<APIKEY>

    HEADERS     Content-Type: application/json

    BODY        <service health payload>
    ```
1. При его отправке должен быть получен ответ `200 OK` с сообщением о состоянии Successful (Успешно).

1. Откройте [OpsGenie](https://www.opsgenie.com/) и убедитесь, что интеграция настроена успешно.

## <a name="next-steps"></a>Дальнейшие действия
- Узнайте, [как настроить уведомления веб-перехватчика для существующих систем управления проблемами](service-health-alert-webhook-guide.md).
- Просмотрите схему веб-перехватчика оповещений журнала действий в статье [Объекты webhook для оповещений журнала действий Azure](../azure-monitor/platform/activity-log-alerts-webhook.md). 
- Дополнительные сведения об уведомлениях о работоспособности службы см. в [этой статье](../azure-monitor/platform/service-notifications.md).
- Дополнительные сведения о группах действий см. в статье [Создание групп действий и управление ими на портале Azure](../azure-monitor/platform/action-groups.md).