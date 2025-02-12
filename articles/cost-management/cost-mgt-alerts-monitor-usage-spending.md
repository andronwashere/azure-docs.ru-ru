---
title: Мониторинг потребления и расходов с оповещениями о затратах | Документация Майкрософт
description: В этой статье описывается, как оповещения о затратах помогают отслеживать потребление и расходы в Управлении затратами Azure.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: cost-management
manager: alavital
ms.custom: ''
ms.openlocfilehash: f1bf62596b6edcc6fff6572e431f3a777be93f05
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66002089"
---
# <a name="use-cost-alerts-to-monitor-usage-and-spending"></a>Использование оповещений о затратах для мониторинга потребления и расходов

Эта статья поможет вам понять и использовать оповещения Управления затратами для отслеживания потребления и расходов Azure. Оповещения о затратах создаются автоматически на основе использования ресурсов Azure. Оповещения показывают все активные управления затратами и оповещения о выставлении счетов в одном месте. Когда потребление достигает заданного порогового значения, Управление затратами генерирует оповещения. Существует три типа оповещений о затратах: оповещения о бюджете, кредите и квотах затрат отдела.

## <a name="budget-alerts"></a>Оповещения о бюджете

Оповещения о бюджете уведомляют вас, когда расходы, основанные на использовании или стоимости, достигают или превышают объем, определенный в [состоянии оповещения о бюджете](tutorial-acm-create-budgets.md). Бюджеты Управления затратами создаются с помощью портала Azure или API [потребления Azure](https://docs.microsoft.com/rest/api/consumption).

На портале Azure бюджеты определяются по затратам. С помощью API потребления Azure бюджеты определяются по затратам или уровню потребления. Оповещения о бюджете поддерживают бюджеты как на основе затрат, так и на основе использования. Оповещения о бюджете создаются автоматически при выполнении условий оповещений о бюджете. Вы можете просмотреть все оповещения о затратах на портале Azure. Каждый раз, когда создается оповещение, оно отображается в оповещениях о затратах. Оповещение по электронной почте также отправляется людям в списке получателей оповещений о бюджете.

## <a name="credit-alerts"></a>Оповещения о кредите

Оповещения о кредите уведомляют вас о денежных обязательствах в связи с использованием денег на счете в Azure. Денежные обязательства предназначены для организаций с Соглашениями Enterprise. Оповещения о кредите создаются автоматически при балансе 90 или 100 % денег на вашем счете в Azure. Всякий раз, когда создается оповещение, оно отражается в оповещениях о затратах и в сообщении электронной почты, отправляемом владельцам учетных записей.

## <a name="department-spending-quota-alerts"></a>Оповещения о квотах расходов отдела

Оповещения о квотах расходов отдела уведомляют вас, когда расходы отдела достигают фиксированного порогового значения квоты. Квоты затрат настраиваются на портале EA. Каждый раз, когда достигается пороговое значение, создается сообщение электронной почты для владельцев отделов и отображается в оповещениях о затратах. Например, 50 или 75 % квоты.

## <a name="supported-alert-features-by-offer-categories"></a>Поддерживаемые функции предупреждений по категориям предложения

Поддержка для типов оповещений зависит от типа учетной записи Azure, что у вас есть (Microsoft предлагают). В следующей таблице показаны средства генерации оповещений, которые поддерживаются в различных предложениях Майкрософт. Можно просмотреть полный список предложений Microsoft в [данным Cost Management понять](understand-cost-mgt-data.md).

| Тип оповещения | Соглашение Enterprise | Клиентское соглашение Майкрософт | Web direct или оплаты мере-You-передавать |
|---|---|---|---|
| Бюджет | ✔ | ✔ | ✔ |
| Материалы | ✔ |✘ | ✘ |
| Квота расходов отдела | ✔ | ✘ | ✘ |



## <a name="view-cost-alerts"></a>Просмотр оповещений о затратах

Чтобы просмотреть оповещения затрат, откройте требуемой области в на портале Azure и выберите **бюджеты** в меню. Используйте **область** поглощений переключиться на другую область. Выберите **затрат оповещения** в меню. См. [основные сведения об областях и работе с ними](understand-work-scopes.md).

![Пример изображения оповещений, показанных в Управлении затратами](./media/cost-mgt-alerts-monitor-usage-spending/budget-alerts-fullscreen.png)

Общее количество активных и отклоненных оповещений отображается на странице оповещений о затратах.

Все оповещения показывают тип оповещения. В оповещении о бюджете указана причина, по которой оно было создано, и название бюджета, к которому оно применяется. Каждое предупреждение содержит дату, когда оно было создано, его состояние и область действия (подписка или группа управления), к которой относится предупреждение.

Возможные состояния: **Активно** и **Отклонено**. Состояние "Активно" указывает, что предупреждение все еще актуально. Состояние "Отклонено" означает, что кто-то пометил предупреждение, чтобы установить его как неактуальное.

Выберите оповещение из списка, чтобы просмотреть сведения о нем. Сведения об оповещении показывают дополнительные сведения об оповещении. В оповещениях о бюджете содержится ссылка на бюджет. Если для оповещения о бюджете доступна рекомендация, то отображается ссылка на рекомендацию. Оповещение о бюджете, кредите и квотах затрат отдела содержат ссылку для анализа в анализе затрат, где вы можете изучить затраты для области действия оповещения. В следующем примере показаны расходы отдела со сведениями об оповещении.

![Пример изображения, на котором показаны расходы отдела со сведениями об оповещении](./media/cost-mgt-alerts-monitor-usage-spending/dept-spending-selected-with-credits.png)

При просмотре сведений об отклоненном оповещении вы можете повторно активировать его, если необходимо действие вручную. Пример приведен на следующем рисунке.

![Пример изображения, на котором показано параметры отклонения и повторной активации](./media/cost-mgt-alerts-monitor-usage-spending/Dismiss-reactivate-options.png)

## <a name="see-also"></a>См. также

- Если вы еще не создали бюджет или не настроили условия оповещений для бюджета, выполните действия, описанные в [этом руководстве](tutorial-acm-create-budgets.md).
