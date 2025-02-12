---
title: Просмотр и скачивание счета Microsoft Azure
description: Описание просмотра и загрузки счета Microsoft Azure.
keywords: счет на оплату,скачать счет,счет azure,использование azure
author: bandersmsft
manager: jureid
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: banders
ms.openlocfilehash: 4dc5f88f7a5994e5bcd50d71c86bf1ba35b10734
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68321764"
---
# <a name="view-and-download-your-microsoft-azure-invoice"></a>Просмотр и скачивание счета Microsoft Azure

Для большинства подписок счет можно скачать на [портале Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) или получить по электронной почте. Если вы являетесь клиентом Azure с Соглашением Enterprise (клиент EA), вы не можете скачивать счета своей организации. Счета отправляются тому, кто настроил получение счетов за регистрацию.

Только определенные роли имеют разрешение на просмотр счетов. Например, администратор учетной записи или администратор предприятия. Дополнительные сведения о получении доступа к сведениям о выставлении счетов см. в статье [Управление доступом к выставлению счетов Azure с помощью ролей](billing-manage-access.md).

Если у вас есть [соглашение с клиентами Майкрософт](#check-your-access-to-a-microsoft-customer-agreement), для получения счетов необходимо иметь одну из следующих ролей:

- Владелец профиля выставления счетов
- Участник
- Читатель
- Менеджер по накладным

Дополнительные сведения о ролях выставления счетов для клиентских соглашений Майкрософт см. в разделе [роли и задачи профиля выставления счетов](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

## <a name="noinvoice"></a>Почему вы можете не получить счет

Это может происходить по нескольким причинам:

- Прошло менее 30 дней со дня оформления подписки Azure.

- Вы оплачиваете Azure по окончании расчетного периода. Поэтому счет, возможно, еще не создан. Дождитесь окончания расчетного периода.

- У вас нет разрешения на просмотр счетов. Если у вас есть соглашение с клиентами Майкрософт, вы должны быть владельцем профиля выставления счетов, участником, модулем чтения или менеджером по накладным. Для других подписок вы можете не видеть старые счета, если вы не являетесь администратором учетной записи. Дополнительные сведения о доступе к сведениям о выставлении счетов см. в статье [Управление доступом для выставления счетов Azure с помощью ролей](billing-manage-access.md).

- При наличии бесплатной пробной версии или ежемесячного кредита с подпиской вы получаете счет только при превышении суммы ежемесячного кредита. Если у вас есть соглашение с клиентами Майкрософт, вы всегда получаете счет.

## <a name="download-your-azure-invoices-pdf"></a>Скачивание счетов Azure (PDF)

Для большинства подписок можно загрузить счет из портал Azure. Если у вас есть соглашение с клиентами Майкрософт, см. статью [Загрузка счетов для соглашения клиента Майкрософт](#download-invoices-for-a-microsoft-customer-agreement).

### <a name="download-invoices-for-an-individual-subscription"></a>Скачивание счетов для отдельной подписки

1. Выберите свою подписку на [странице "подписки](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) " в портал Azure в качестве [пользователя с доступом к счетам](billing-manage-access.md).

2. Выберите **Счета**.

    ![Снимок экрана, на котором показан параметр Billing & usage (Счет и данные об использовании)](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. Щелкните **Скачать счет**, чтобы просмотреть счета в формате PDF. Если отображается сообщение **Недоступно**, см. раздел [Почему не отображается счет за последний расчетный период?](#noinvoice)

    ![Снимок экрана, на котором показаны расчетные периоды, параметр для скачивания, данные о ежедневном использовании и общие выплаты за каждый расчетный период](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Кроме того, данные о ежедневном использовании можно просмотреть, щелкнув расчетный период.

Дополнительные сведения о счете см. в статье [Расшифровка счета за использование Microsoft Azure](billing-understand-your-bill.md). Дополнительные сведения об управлении затратами см. в статье [Предотвращение непредвиденных затрат с помощью службы "выставление счетов и управление затратами Azure](billing-getting-started.md)".

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Скачивание счетов для соглашения клиента Майкрософт

Счета формируются для каждого [профиля выставления счетов](billing-mca-overview.md#billing-profiles) в соглашении клиента Майкрософт. Для скачивания счетов из портал Azure необходимо быть владельцем профиля выставления счетов, участником, Читательом или менеджером по накладным.

1. Войдите на [портале Azure](https://portal.azure.com).
1. Выполните поиск по запросу *Управление затратами и выставление счетов*.
1. Выберите профиль выставления счетов. В зависимости от доступа может потребоваться сначала выбрать учетную запись выставления счетов.
1. Выберите **Счета**.
1. В сетке накладной найдите строку счета, которую необходимо загрузить.
1. Нажмите кнопку с многоточием`...`() в конце строки.
    ![Снимок экрана, на котором показано многоточие в конце строки](./media/billing-download-azure-invoice/billingprofile-invoicegrid.png)
1. В контекстном меню загрузки выберите **счет**.

    ![Снимок экрана, показывающий контекстное меню](./media/billing-download-azure-invoice/contextmenu.png)

Если вы не видите счет за последний расчетный период, см [. раздел Почему не отображается счет за последний расчетный период?](#noinvoice)

## <a name="get-your-invoice-in-email-pdf"></a>Получение счета по электронной почте (в формате PDF)

Вы можете согласиться и настроить дополнительных получателей Azure для получения счета по электронной почте. Эта функция может быть недоступна для определенных подписок, например для предложений о планах поддержки, соглашений Enterprise или Azure с открытой лицензией. Если у вас есть соглашение с клиентами корпорации Майкрософт, см. следующий раздел, чтобы [получить счета профиля выставления счетов по электронной почте](#get-your-subscriptions-invoices-in-email).

### <a name="get-your-subscriptions-invoices-in-email"></a>Получение счетов по подписке по электронной почте

1. Выберите свою подписку на [странице подписки](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Выберите все свои подписки. Щелкните **Invoices** (Счета), а затем — **Email my invoice** (Отправить счет по электронной почте).

    ![Снимок экрана, на котором отображается процесс предоставления согласия](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Щелкните **Согласиться** и примите условия.

    ![Снимок экрана, на котором отображается процесс предоставления согласия (шаг 2)](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. После принятия соглашения можно настроить дополнительных получателей. При удалении получателей адреса их электронной почты больше не сохраняются. Если вы передумали, их необходимо прочесть.

    ![Снимок экрана, на котором отображается процесс предоставления согласия (шаг 3)](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Если после выполнения этих действий вы не получили сообщение, то проверьте правильность своего адреса электронной почты в [настройках общения в профиле](https://account.windowsazure.com/profile).

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Отказаться от получения счетов по подписке в сообщении электронной почты

Чтобы отказаться от получения счета по электронной почте, выполните описанные выше действия и нажмите кнопку **отказ от выставления счетов по электронной почте**. После этого удаляются все адреса электронной почты, на которые могли отправляться счета. При необходимости вы можете повторно настроить получателей.

 ![Снимок экрана, на котором отображается процесс отказа от отправки](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Получение счетов по соглашению для клиентов Майкрософт по электронной почте

Если у вас есть соглашение с клиентами корпорации Майкрософт, вы можете принять участие в программе, чтобы получить счет по электронной почте. Все владельцы профилей выставления счетов, участники, читатели и менеджеры по накладным получат счет по электронной почте. Читатели не могут обновить настройку счета по электронной почте.

1. Войдите на [портале Azure](https://portal.azure.com).
1. Выполните поиск по запросу **Управление затратами и выставление счетов**.
1. Выберите профиль выставления счетов. В зависимости от доступа может потребоваться сначала выбрать учетную запись выставления счетов.
1. В разделе **Параметры** выберите **Свойства**.
1. В разделе накладная **по электронной почте**выберите **обновить параметры счета электронной почты**.

    ![Снимок экрана, показывающий свойства накладной электронной почты](./media/billing-download-azure-invoice/billingprofile-email.png)

1. Выберите **согласие**.
1. Нажмите кнопку **Обновить**.

### <a name="opt-out-of-getting-your-microsoft-customer-agreement-invoices-in-email"></a>Отказаться от получения счетов в соглашении клиента Майкрософт по электронной почте

Чтобы отказаться от получения счета по электронной почте, выполните описанные выше действия и нажмите кнопку отказаться. Все владельцы, участники, читатели и менеджеры по накладным отказались от получения счета по электронной почте. Если вы являетесь читателями, вы не сможете изменить настройку счета по электронной почте.



## <a name="check-your-access-to-a-microsoft-customer-agreement"></a>Проверка доступа к соглашению клиента Майкрософт
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами.

Если у вас есть вопросы или вам нужна помощь, [создайте запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о счетах и расходах см. в следующих статьях:

- [Просмотр и скачивание Microsoft Azure использования и расходов](billing-download-azure-daily-usage.md)
- [Расшифровка счета за использование Microsoft Azure](billing-understand-your-bill.md)
- [Общие сведения о терминах в счете Azure](billing-understand-your-invoice.md)
- [Значение терминов в файле с подробными данными об использовании Microsoft Azure](billing-understand-your-usage.md)
- [Просмотр цен на Azure в Организации](billing-ea-pricing.md)

Если у вас есть соглашение с клиентами корпорации Майкрософт, см. следующие сведения:

- [Общие сведения о расходах на счет для профиля выставления счетов](billing-mca-understand-your-bill.md)
- [Сведения о терминах в счете для профиля выставления счетов](billing-mca-understand-your-invoice.md)
- [Общие сведения о файле сведений об использовании и расходах Azure для профиля выставления счетов](billing-mca-understand-your-usage.md)
- [Просмотр и скачивание налоговых документов для профиля выставления счетов](billing-mca-download-tax-document.md)
- [Просмотр цен на Azure в Организации](billing-ea-pricing.md)
