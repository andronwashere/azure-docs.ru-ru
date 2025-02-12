---
title: Создание правил события и управление ими в приложении Azure IoT Central | Документация Майкрософт
description: Правила события Azure IoT Central позволяют отслеживать устройства практически в реальном времени и автоматически вызывать действия (например, отправлять сообщение электронной почты) при срабатывании правила.
author: ankitscribbles
ms.author: ankitgup
ms.date: 06/09/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: peterpr
ms.openlocfilehash: 4754e6b571845d286ef22014f87b86fae2f6633d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67053018"
---
# <a name="create-an-event-rule-and-set-up-notifications-in-your-azure-iot-central-application"></a>Создание правила события и настройка уведомлений в приложении Azure IoT Central

*Эта статья предназначена для операторов, разработчиков и администраторов.*

Вы можете использовать Azure IoT Central для удаленного мониторинга подключенных устройств. Правила Azure IoT Central позволяют вам отслеживать устройства практически в реальном времени и автоматически вызывать действия, например, отправлять сообщение электронной почты или активировать Microsoft Flow. Всего в несколько щелчков вы можете определить условие для отслеживания данных устройства и настроить соответствующее действие. В этой статье объясняется, как создать правила для отслеживания событий, отправляемых устройством.

Устройства могут использовать измерение событий для отправки важных или информационных событий. Правило события активируется, когда с устройства передается выбранное событие

## <a name="create-an-event-rule"></a>Создание правила события

Чтобы создать правило события, шаблон устройства должен иметь по крайней мере одно определенное измерение события. В этом примере используется устройство охлаждаемого торгового автомата, сообщающее о событии сбоя в работе мотора вентилятора. Правило отслеживает передаваемые устройством данные события и отправляет сообщение при каждой передаче события.

1. С помощью **шаблонов устройств** страницы, перейдите к шаблон устройства, для которой вы добавляете для правила.

1. Если вы еще не создали правило, появится следующий экран:

    ![Правила отсутствуют](media/howto-create-event-rules/rules_landing_page1.png)

1. На **правила** выберите **+ новое правило** типы правил, можно создать.

1. Выберите **событий** плитку, чтобы создать правило мониторинга событие.

    ![Типы правил](media/howto-create-event-rules/rule_types1.png)

1. Введите имя, определяющее правило в этом шаблоне устройства.

1. Чтобы сразу же включить правила для всех устройств, созданные по этому шаблону, переключите **включить правило для всех устройств этого шаблона**.

    ![Сведения о правиле](media/howto-create-event-rules/rule_detail1.png)

    Правило автоматически применяется ко всем устройствам в шаблоне устройств.

### <a name="configure-the-rule-conditions"></a>Настройка условий правила

Условие определяет критерии, которые отслеживаются этим правилом.

1. Выберите **+** рядом с полем **Условия** для добавления нового условия.

1. Из раскрывающегося списка выберите измерение события, которое требуется отслеживать. В этом примере выбрано событие **Fan Motor Error** (Ошибка мотора вентилятора).

   ![Условие](media/howto-create-event-rules/condition_filled_out1.png)

1. При желании вы также можете установить для параметра **Агрегирование** значение **Счетчик** и предоставить соответствующий порог.

   - Без агрегирования правило запускается для каждой точки данных события, которая соответствует условию. Например, при настройке правила условие в условие запуска **Motor ошибка вентилятор** событием, а затем правило инициирует почти сразу же, когда устройство сообщает это событие.
   - Если счетчик используется как агрегированная функция, вам необходимо предоставить **порог** и **окно времени агрегирования**, в которым необходимо оценить условие. В этом случае вычисляется количество событий и правило срабатывает, только в том случае, если количество объединенные событий соответствует пороговое значение.

     Например, если вы хотите оповещать о наличии более трех событий устройства в течение 5 минут, выберите событие и установите агрегатную функцию "счетчик", оператор "больше чем" и "порог" равный 3. Установите для периода времени агрегирования значение "5 минут". Правило срабатывает, когда устройство отправляет более трех событий в течение 5 минут. Частота оценки правила такая же, как и **период времени агрегирования**. Это означает, что в этом примере правило оценивается один раз каждые 5 минут.

     ![Добавление условия события](media/howto-create-event-rules/aggregate_condition_filled_out1.png)

     >[!NOTE]
     >В поле **Условие** можно добавить несколько измерений события. Если задано несколько условий, для срабатывания правила необходимо, чтобы были соблюдены все условия. Каждое условие получает неявным образом объединить с предложением «AND». При использовании агрегирования каждое измерение должно быть агрегировано.

### <a name="configure-actions"></a>Настройка действий

В этом разделе показано, как настроить действия, предпринимаемые при запуске правила. Действия вызываются в том случае, если соблюдаются все условия, указанные в правиле.

1. Выберите **+** рядом с разделом **Действия**. Здесь отобразится список доступных действий.

    ![Добавление действия](media/howto-create-event-rules/add_action1.png)

1. Выберите действие **Электронное письмо**, введите допустимый адрес электронной почты в поле **Кому** и укажите примечание, отображаемое в тексте сообщения при срабатывании правила.

    > [!NOTE]
    > Письма отправляются только пользователям, которые были добавлены в приложение и входили в систему минимум один раз. Узнайте больше об [управлении пользователями](howto-administer.md) в Azure IoT Central.

   ![Настройка действия](media/howto-create-event-rules/configure_action1.png)

1. Чтобы сохранить правило, нажмите кнопку **Сохранить**. Правило станет активным в течение нескольких минут и начнет выполнять мониторинг событий, отправляемых в приложение. Если условие, указанное в правиле, соответствует требованиям, правило активирует настроенное действие отправки сообщения электронной почты.

Вы можете добавить другие действия в правило, например действия Microsoft Flow и веб-перехватчики. Для каждого правила можно добавить до 5 действий.

- [действие Microsoft Flow](howto-add-microsoft-flow.md) для запуска рабочего процесса в Microsoft Flow при срабатывании правила; 
- [действие веб-перехватчика](howto-create-webhooks.md) для уведомления других служб при срабатывании правила.

## <a name="parameterize-the-rule"></a>Параметризация правила

Действия также можно настроить, используя **свойство устройства** в качестве параметра. Если адрес электронной почты хранится в виде свойства устройства, его можно использовать при определении адреса **Кому**.

## <a name="delete-a-rule"></a>Удаление правила

Если вам больше не нужно правило, удалите его. Для этого откройте правило и щелкните **Удалить**. Удаление правила приводит к его удалению из шаблона устройства и всех связанных устройств.

## <a name="enable-or-disable-a-rule-for-a-device-template"></a>Включение или отключение правила для шаблона устройства

Перейдите к устройству и выберите правило, которое требуется включить или отключить. Переведите переключатель **Enable rule for all devices of this template** (Включить правило для всех устройств в шаблоне), чтобы включить или отключить правило для всех устройств, связанных с определенным шаблоном.

## <a name="enable-or-disable-a-rule-for-a-device"></a>Включение или отключение правила для устройства

Перейдите к устройству и выберите правило, которое требуется включить или отключить. Переведите переключатель **Enable rule for this device** (Включить правило для этого устройства), чтобы включить или отключить правило для этого устройства.

## <a name="next-steps"></a>Дальнейшие действия

Вы узнали, как создавать правила в приложении Azure IoT Central, а значит вы готовы к следующему шагу:

- [Добавление действия Microsoft Flow в правилах](howto-add-microsoft-flow.md)
- [Добавление действия веб-перехватчика в правилах](howto-create-webhooks.md)
- [Сгруппировать несколько действий для запуска из одного или нескольких правил](howto-use-action-groups.md)
- [Управление устройствами](howto-manage-devices.md)
