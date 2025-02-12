---
title: Электронное сообщение Azure о просроченной задолженности | Документация Майкрософт
description: Описание процедуры оплаты, если имеется задолженность по вашей подписке Azure
services: ''
documentationcenter: ''
author: genlin
manager: jureid
editor: ''
tags: billing
ms.assetid: d0b88c92-fb9d-4d12-931b-c26104ad63e9
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: banders
ms.openlocfilehash: f58969b167b6c0f0a66d46731ad76c1f6e9acc41
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491455"
---
# <a name="resolve-past-due-balance-for-your-azure-subscription"></a>Устранение проблемы с задолженностью по подписке Azure

Если платеж не получен или мы не можем обработать ваш платеж, может получить сообщение электронной почты или оповещение в портале Azure или Центр управления учетной записью.
Если вы являетесь [учетной записи администратора](billing-subscription-transfer.md#whoisaa), невыполненные платежи [портала Azure](https://portal.azure.com). Если вы оплачиваете услугу по выставляемым счетам, отправьте платеж в местоположение, указанное внизу счета.

> [!IMPORTANT]
> * Если у вас несколько подписок, с помощью той же кредитной карте, и они все необработанные, вы платите весь остаток задолженности за один раз.
> * Инструмент оплаты, используемого для невыполненные платежи станет новый активный метод оплаты для всех подписок, которые использовали метод неудачных оплаты.

## <a name="resolve-past-due-balance-in-the-azure-portal"></a>Решение проблемы просроченных сальдо на портале Azure

1. Войдите в [портала Azure](https://portal.azure.com) качестве учетной записи администратора.
1. Поиск **Cost Management + выставление счетов**.
1. На странице "Обзор" вы увидите список своих подписок. Если крайний срок прошел состояние вашей подписки, нажмите кнопку **сопоставления баланс** ссылку.
    ![Снимок экрана, показывающий сопоставления баланс ссылку](./media/billing-azure-subscription-past-due-balance/settle-balance-entry-point.png)
1. Общая задолженность отражает невыполненные платежи по всем службам Майкрософт, с помощью метода невыполненных платежей.
1. Выберите способ оплаты для оплаты баланс. Этот метод оплаты станет активный метод оплаты для всех подписок, в настоящее время с помощью метода невыполненных платежей.
    ![Снимок экрана, показывающий ссылку метод select оплаты](./media/billing-azure-subscription-past-due-balance/settle-balance-screen.png)
1. Если выбранный способ оплаты также имеет невыполненные платежи для служб Майкрософт, это будет отражаться в общий баланс. Необходимо обратить слишком этих невыполненные платежи.
1. Нажмите кнопку **платить**.

## <a name="not-getting-billing-email-notifications"></a>Платежные уведомления не приходят по электронной почте?

Если вы являетесь администратором учетной записи, [проверьте, какой адрес электронной почты используется для уведомлений](billing-how-to-change-azure-account-profile.md). Мы рекомендуем использовать адрес электронной почты, который будет регулярно проверяться. Если адрес электронной почты указан правильно, проверьте папку спама.

## <a name="if-i-forget-to-pay-what-happens"></a>Что случится, если я забуду заплатить?

Служба отменяется, а ресурсы становятся недоступными. Данные Azure будут удалены через 90 дней после прекращения услуги. Дополнительные сведения см. в разделе [What happens to your data if you leave the service](https://go.microsoft.com/fwLink/p/?LinkID=822930&clcid=0x409) (Что происходит с данными, когда вы прекращаете использовать службу).

Если вы знаете, что платеж обработан, но подписка по-прежнему отключена, обратитесь в [службу поддержки Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

## <a name="need-help-contact-us"></a>Требуется помощь? Свяжитесь с нами.

Если у вас есть вопросы или нужна помощь, [создать запрос в службу поддержки](https://go.microsoft.com/fwlink/?linkid=2083458).
