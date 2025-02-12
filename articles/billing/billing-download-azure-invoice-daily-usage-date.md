---
title: Скачивание счета на оплату и данных о ежедневном использовании в Azure | Документация Майкрософт
description: Описывается, как скачать или просмотреть счет на оплату и данные о ежедневном использовании в Azure.
keywords: счет на оплату,скачать счет,счет azure,использование azure
services: billing
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: 16b2eaef74a7aa0e3e28bfcbb6dbd9da568db6cf
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491406"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>Скачивание или просмотр счета на оплату и данных о ежедневном использовании в Azure

Для большинства подписок счет можно скачать на [портале Azure](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) или получить по электронной почте. Если вы являетесь клиентом Azure с Соглашением Enterprise (клиент EA), вы не можете скачивать счета своей организации. Счета отправляются тому, кто настроил получение счетов за регистрацию.

Если вы являетесь клиентом EA, или имеете [соглашении клиента Microsoft](#check-access-to-a-microsoft-customer-agreement), можно скачать сведения об использовании, в [портала Azure](https://portal.azure.com/). Другие подписки, см. в статье [центре управления учетной записью](https://account.azure.com/Subscriptions) скачать сведения об использовании.

Только определенные роли нет разрешения на получение выставления счетов счетах и статистику использования, например учетной записи администратора или администратора предприятия. Дополнительные сведения о доступе к сведениям о выставлении счетов см. в статье [Управление доступом для выставления счетов Azure с помощью ролей](billing-manage-access.md).

Если у вас есть соглашение Microsoft клиента, необходимо быть профиля выставления счетов владелец, участник или Читатель, или выставляется счет manager для просмотра сведений о выставлении счетов и использовании. Дополнительные сведения о ролях выставления счетов для соглашений клиентов Майкрософт, см. в разделе [выставления счетов профиля роли и задачи](billing-understand-mca-roles.md#billing-profile-roles-and-tasks).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-your-azure-invoices-pdf"></a>Скачивание счетов Azure (в формате PDF)

Для большинства подписок счет можно скачать с портала Azure. Если у вас есть соглашение Microsoft клиента, см. в разделе загрузки счета-фактуры для профиля выставления счетов.

### <a name="download-invoices-for-an-individual-subscription"></a>Скачивать счета для отдельной подписки

1. Выберите свою подписку [страницу "подписки"](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) на портале Azure как [пользователя с доступом к счетам](billing-manage-access.md).

2. Выберите **Счета**.

    ![Снимок экрана, на котором показан параметр Billing & usage (Счет и данные об использовании)](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. Щелкните **Скачать счет**, чтобы просмотреть счета в формате PDF. Если отображается сообщение **Недоступно**, см. раздел [Почему не отображается счет за последний расчетный период?](#noinvoice)

    ![Снимок экрана, на котором показаны расчетные периоды, параметр для скачивания, данные о ежедневном использовании и общие выплаты за каждый расчетный период](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. Кроме того, данные о ежедневном использовании можно просмотреть, щелкнув расчетный период.

Дополнительные сведения о счете см. в статье [Расшифровка счета за использование Microsoft Azure](billing-understand-your-bill.md). Справочные сведения об управлении затратами см. в статье [Предотвращение непредвиденных расходов с помощью функции выставления счетов и управления затратами в Azure](billing-getting-started.md).

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Скачивать счета клиента соглашения Майкрософт

Счета будут сгенерированы для каждой [профиль выставления счетов](billing-mca-overview.md#billing-profiles) в соглашении клиента Microsoft. Необходимо быть профиля выставления счетов владелец, участник, читатель, или выставляется счет manager для скачивания счетов на портале Azure.

1. Поиск **Cost Management + выставление счетов**.
2. Выберите профиль выставления счетов.
3. Выберите **Счета**.
4. В сетке счета найдите строку счета-фактуры, который вы хотите загрузить.
5. Нажмите кнопку с многоточием (`...`) в конце строки.
6. Выберите в контекстном меню загрузки **счета**.

Если вы не видите счет за последний период выставления счетов, см. в разделе **дополнительную информацию**. <!-- Fix this -->
### <a name="noinvoice"></a>Почему не отображается счет за последний расчетный период?

Это может происходить по нескольким причинам:

- Прошло менее 30 дней со дня оформления подписки Azure.

- Счет еще не создан. Дождитесь окончания расчетного периода.

- У вас нет разрешения на просмотр счетов. Если у вас есть соглашение Microsoft клиента, необходимо быть профиль выставления счетов владелец, участник или Читатель, или выставляется счет manager. Для других подписок могут не отображаться старые счета, если вы не являетесь администратором учетной записи. Дополнительные сведения о доступе к сведениям о выставлении счетов см. в статье [Управление доступом для выставления счетов Azure с помощью ролей](billing-manage-access.md).

- При наличии бесплатной пробной версии или ежемесячный кредит с вашей подпиской, не использованный полностью, вы не получите счет, если у вас есть соглашение клиента Microsoft.

## <a name="get-your-invoice-in-email-pdf"></a>Получение счета по электронной почте (в формате PDF)

Вы можете согласиться и настроить дополнительных получателей Azure для получения счета по электронной почте. Эта функция может быть недоступна для определенных подписок, например для предложений о планах поддержки, соглашений Enterprise или Azure с открытой лицензией. Если у вас есть соглашения с Майкрософт, см. в разделе Get, профиль выставления счетов по электронной почте.

### <a name="get-your-subscriptions-invoices-in-email"></a>Получение счетов вашей подписки по электронной почте

1. Выберите свою подписку на [странице подписок](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Выберите все свои подписки. Щелкните **Invoices** (Счета), а затем — **Email my invoice** (Отправить счет по электронной почте).

    ![Снимок экрана, на котором отображается процесс предоставления согласия](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. Щелкните **Согласиться** и примите условия.

    ![Снимок экрана, на котором отображается процесс предоставления согласия (шаг 2)](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. Приняв условия соглашения, можно настроить дополнительных получателей. При удалении получателей адреса их электронной почты больше не сохраняются. Если вы передумаете, их необходимо добавить повторно.

    ![Снимок экрана, на котором отображается процесс предоставления согласия (шаг 3)](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

Если после выполнения этих действий вы не получили сообщение, то проверьте правильность своего адреса электронной почты в [настройках общения в профиле](https://account.windowsazure.com/profile).

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>Отказаться от получения счетов вашей подписки по электронной почте

Можно отказаться от получения счета по электронной почте, следуя инструкциям выше и выберите команду **отказаться от счета по электронной почте**. После этого удаляются все адреса электронной почты, на которые могли отправляться счета. Если вы выбрали в можно перенастроить получателей.

 ![Снимок экрана, на котором отображается процесс отказа от отправки](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>Получение счетов клиента соглашение по электронной почте

Если у вас есть соглашение Microsoft Customer, можно согласиться на получение счета по электронной почте. Все выставления счетов владельцы профиль, участники, читатели и счета диспетчеры будет получение счета по электронной почте. Читатели не удалось обновить предпочтения счета по электронной почте.

1. Поиск **Cost Management + выставление счетов**.
1. Выберите профиль выставления счетов.
1. В разделе **Параметры** выберите **Свойства**.
1. В разделе **счет по электронной почте**выберите **предпочтения счета по электронной почте обновления**.
1. Выберите **согласиться**.
1. Нажмите кнопку **Обновить**.

### <a name="opt-out-of-getting-your-billing-profile-invoices-in-email"></a>Отказаться от получения выставления счетов профиля в сообщении электронной почты

Можно отказаться от получения счета по электронной почте, следуя инструкциям выше и выберите команду **отказаться**. Все владельцы, участники, читатели и счета диспетчеры будете исключены из получение счета по электронной почте, слишком. Если средство чтения, нельзя изменить приоритет счета по электронной почте.

## <a name="download-usage"></a>Скачивание сведений об использовании

 Для большинства подписок файл со сведениями о ежедневном использовании можно найти в [Центре управления учетной записью Azure](https://account.azure.com/Subscriptions). Если вы являетесь клиентом EA или соглашение Microsoft Customer, его можно скачать использования в [портала Azure](https://portal.azure.com/). <!-- TO DO: update PayG experience to Ibiza once it ships-->

### <a name="download-usage-from-the-account-center-csv"></a>Скачивание данных об использовании в Центре управления учетной записью (в формате CSV)

1. Войдите в [Центр управления учетной записью Azure](https://account.windowsazure.com/subscriptions) в качестве администратора учетной записи.

2. Выберите подписку, для которой необходимо получить информацию о счетах и статистику использования.

3. Щелкните **ЖУРНАЛ ВЫСТАВЛЕННЫХ СЧЕТОВ**.

    ![Снимок экрана, на котором показан вариант "Журнал выставленных счетов"](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Вы увидите выписки за шесть последних расчетных периодов, а также сведения о текущем периоде, счет за который еще не выставлен.

    ![Снимок экрана, на котором показаны расчетный период, параметры для скачивания счета, данные о ежедневном использовании и общие выплаты за каждый расчетный период](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Щелкните **Просмотреть текущую выписку** , чтобы просмотреть сумму к оплате по состоянию на момент создания выписки. Эти сведения обновляются только один раз в день, поэтому в ней могут отображаться не все данные об использовании. Ежемесячный счет может отличаться от предварительного.

    ![Снимок экрана, на котором показан параметр "Просмотреть текущую выписку"](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Снимок экрана, на котором показан расчет текущих выплат](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Выберите **Скачать сведения об использовании** и сохраните CSV-файл с данными о ежедневном использовании. Если вы видите две доступные версии, скачайте вторую.

    ![Снимок экрана, на котором показан параметр "Скачать сведения об использовании"](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Только администратор учетной записи имеет доступ к Центру управления учетной записью Azure. Другие администраторы по выставлению счетов, например владелец, могут получить сведения об использовании с помощью [API выставления счетов](billing-usage-rate-card-overview.md).

Дополнительные сведения о данных о ежедневном использовании см. в статье [Расшифровка счета за использование Microsoft Azure](billing-understand-your-bill.md). Справочные сведения об управлении затратами см. в статье [Предотвращение непредвиденных расходов с помощью функции выставления счетов и управления затратами в Azure](billing-getting-started.md).

### <a name="download-usage-for-ea-customers"></a>Скачивание сведений об использовании для клиентов EA

Возможность просматривать и скачивать данные об использовании в качестве клиента EA, необходимо быть администратором предприятия, владелец учетной записи или администратор отдела с представлением передачу включена политика.

1. Войдите на [портале Azure](https://portal.azure.com).
1. Поиск *Cost Management + выставление счетов*.

    ![Снимок экрана, показывающий поиск на портале Azure](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. Выберите **Использование+расходы**.
1. Возле месяца, сведения о котором необходимо скачать, выберите **Скачать**.

### <a name="download-usage-for-your-microsoft-customer-agreement"></a>Скачать сведения об использовании для соглашение с клиентом Microsoft

Возможность просматривать и скачивать данные об использовании для профиля выставления счетов, необходимо быть профиля выставления счетов владелец, участник или Читатель, или выставляется счет manager.

#### <a name="download-usage-for-billed-charges"></a>Скачать сведения об использовании для начисления оплаты

1. Поиск **Cost Management + выставление счетов**.
2. Выберите профиль выставления счетов.
3. Выберите **Счета**.
4. В сетке счета найдите строку счета-фактуры, соответствующее использование, который вы хотите загрузить.
5. Нажмите кнопку с многоточием (`...`) в конце строки.
6. Выберите в контекстном меню загрузки **Azure об использовании и затратах**.

#### <a name="download-usage-for-open-charges"></a>Скачать сведения об использовании для оплаты по открытым

Также можно скачать сведения об использовании с начала месяца для текущего расчетного периода, это означает, что еще не был выставлен накладные расходы.

1. Поиск **Cost Management + выставление счетов**.
2. Выберите профиль выставления счетов.
3. В **Обзор** колонке щелкните **скачать Azure об использовании и затратах**.

## <a name="check-access-to-a-microsoft-customer-agreement"></a>Проверьте доступ к соглашению клиента Microsoft
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами

Если у вас есть вопросы или нужна помощь, [создать запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о счете и расходов, см. в разделе:

- [Расшифровка счета за использование Microsoft Azure](billing-understand-your-bill.md)
- [Значение терминов в счете Azure](billing-understand-your-invoice.md)
- [Значение терминов в файле с подробными данными об использовании Microsoft Azure](billing-understand-your-usage.md)
- [Просмотреть цены на Azure вашей организации](billing-ea-pricing.md)

Если у вас есть соглашения клиентов Майкрософт, см. в разделе:

- [Основная информация о расходах на счете для профиля выставления счетов](billing-mca-understand-your-bill.md)
- [Значение терминов в счете для профиля выставления счетов](billing-mca-understand-your-invoice.md)
- [Понять Azure файл об использовании и затратах для профиль выставления счетов](billing-mca-understand-your-usage.md)
- [Просмотр и Загрузка налоговых документов для профиля выставления счетов](billing-mca-download-tax-document.md)
- [Просмотреть цены на Azure вашей организации](billing-ea-pricing.md)
