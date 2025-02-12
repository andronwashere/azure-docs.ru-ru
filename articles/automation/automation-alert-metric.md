---
title: Мониторинг модулей runbook службы автоматизации Azure с помощью оповещений метрик
description: В этой статье приведено пошаговое руководство по мониторингу модулей runbook службы автоматизации Azure с помощью метрик.
services: automation
ms.service: automation
author: bobbytreed
ms.author: robreed
ms.date: 11/01/2018
ms.topic: article
manager: carmonm
ms.openlocfilehash: 142fb84624c2b0d3d92868aae5794792ed90b577
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2019
ms.locfileid: "67478010"
---
# <a name="monitoring-runbooks-with-metric-alerts"></a>Мониторинг модулей runbook с помощью оповещений метрик

В этой статье вы узнаете, как создать оповещения на основе состояния выполнения модулей runbook.

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите в Azure (https://portal.azure.com ).

## <a name="create-alert"></a>Создание оповещения

Оповещения позволяют определить условия мониторинга и действие, предпринимаемое при соблюдении этого условия.

На портале Azure перейдите к учетной записи службы автоматизации. В разделе **Мониторинг** выберите **Оповещения**, а затем щелкните **+ Новое правило генерации оповещений**. Область целевого объекта уже определена для вашей учетной записи автоматизации.

### <a name="configure-alert-criteria"></a>Настройка условий оповещения

1. Щелкните **+ Add criteria** (+ Добавить условие). Выберите **метрики** для **типа сигнала** и выберите **Total Jobs** (Всего заданий) в таблице.

2. На странице **Настроить логику сигналов** определяется логика, активирующая оповещение. В архивном графе вам предоставляется два измерения — **Имя Runbook** и **Состояние**. Измерения представляют собой различные свойства для метрики, которые можно использовать для фильтрации результатов. В поле **Имя Runbook** выберите runbook, для которого требуется получать оповещения, или оставьте его пустым, чтобы получать оповещения обо всех модулях runbook. В раскрывающемся списке **Состояние** выберите состояние, которое необходимо отслеживать. Значения имени runbook и состояния, которые содержатся в раскрывающемся списке, предназначены только для заданий, выполненных за последнюю неделю.

   Если вы хотите сообщить о состоянии или runbook, которые не отображаются в раскрывающемся списке, щелкните **\+** рядом с измерением. Это действие открывает диалоговое окно, куда можно ввести другое значение, которое не было передано в ближайшее время для этого измерения. Если введено значение, которое не существует для свойства, оповещение не сработает.

   > [!NOTE]
   > Если не применять имя **RunbookName** измерения, если модули Runbook, не соответствует условиям состояния, вместе с модулями Runbook скрытой системной, вы получите оповещение.

3. В **разделе логики оповещения** определите условие и порог для оповещения. Предварительная версия определенного условия показана ниже.

4. В разделе **Вычисляется на основе** выберите временной период запроса и частоту его выполнения. Например, если для поля **Период** выбрать **Over the last 5 minutes** (За последние 5 минут), а для поля **Частота** — **Every 1 Minute** (Каждую минуту), оповещение будет искать количество модулей runbook, которые соответствовали условиям за последние 5 минут. Этот запрос выполняется каждую минуту, и когда условия оповещения, которые вы определили, отсутствуют в течение 5-минутного периода, оповещение разрешается самостоятельно. По завершении нажмите кнопку **Готово**.

   ![Выбор ресурса для оповещения](./media/automation-alert-activity-log/configure-signal-logic.png)

### <a name="define-alert-details"></a>Определение сведений об оповещении

1. В разделе **2. Определение сведений об оповещении** присвойте для оповещения понятное имя и описание. Задайте **степень серьезности** в соответствии с условием оповещения. Существует пять степеней серьезности в диапазоне от 0 до 5. Оповещения обрабатываются одинаково, независимо от степени серьезности. Степень серьезности можно сопоставить в соответствии с вашей бизнес-логикой.

1. В нижней части раздела есть кнопка, которая позволяет включить правило по завершении. По умолчанию правила включаются при создании. Если выбрать "Нет", можно создать оповещение в состоянии **Отключено**. Чтобы включить оповещение, когда будете готовы, выберите его на странице **Правила** в Azure Monitor и щелкните **Включить**.

### <a name="define-the-action-to-take"></a>Определение предпринимаемого действия

1. В разделе **3. Определение группы действий** щелкните **+ New action group** (+ Новая группа действий). Группа действий содержит действия, которые можно использовать для нескольких оповещений. Они могут включать в себя (но не ограничиваются) уведомления электронной почты, модули Runbook, веб-перехватчики и многое другое. Дополнительные сведения о группах действий см. в статье [Create and manage action groups in the Azure portal](../azure-monitor/platform/action-groups.md) (Создание групп действий и управление ими на портале Azure).

1. В поле **Имя группы действий** введите понятное и краткое имя. Короткое имя используется вместо полного имени группы действий при отправке уведомлений с помощью этой группы.

1. В разделе **Действия** в поле **Тип действия** выберите **Email/SMS/Push/Voice** (Электронная почта, SMS, push-уведомление, голосовой вызов).

1. На странице **Email/SMS/Push/Voice** (Электронная почта, SMS, push-уведомление, голосовой вызов) присвойте имя для действия. Установите флажок **Адрес электронной почты** и введите адрес электронной почты, который нужно использовать.

   ![Настройка группы действий электронной почты](./media/automation-alert-activity-log/add-action-group.png)

1. Нажмите кнопку **ОК** на странице **Email/SMS/Push/Voice** (Электронная почта, SMS, push-уведомление, голосовой вызов), чтобы закрыть ее, и нажмите кнопку **ОК**, чтобы закрыть страницу **Добавить группу действий**. Имя, указанное на этой странице, сохраняется в качестве **имени действия**.

1. Затем нажмите кнопку **Сохранить**. Это действие создает правило, которое оповещает вас, когда модуль runbook завершает работу с определенным состоянием.

> [!NOTE]
> При добавлении адреса электронной почты в группу действий отправляется сообщение электронной почты с уведомлением о добавлении адреса.

## <a name="notification"></a>Уведомление

При соблюдении условий оповещения группа действий выполняет определенное действие. В приведенном здесь примере отправляется сообщение электронной почты. На следующем изображении показан пример сообщения электронной почты, получаемого после активации оповещения:

![Оповещение по электронной почте](./media/automation-alert-activity-log/alert-email.png)

Если метрика больше не превышает определенный порог, оповещение деактивируется и группа действий выполняет определенное действие. Если для типа действия выбрано сообщение, отправляется электронное сообщение со сведениями о разрешении.

## <a name="next-steps"></a>Дальнейшие действия

Перейдите к следующей статье, чтобы узнать о других способах интеграции оповещений в свою учетную запись службы автоматизации.

> [!div class="nextstepaction"]
> [Использование оповещения для активации runbook службы автоматизации Azure](automation-create-alert-triggered-runbook.md)
