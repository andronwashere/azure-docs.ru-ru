---
title: Просмотр поставщиков служб и управление ими на портале Azure
description: Клиенты могут использовать страницу "Поставщики услуг" на портале Azure для просмотра сведений о поставщиках услуг, предложениях поставщиков услуг и делегированных ресурсах.
author: JnHs
ms.author: jenhayes
ms.service: lighthouse
ms.date: 07/11/2019
ms.topic: overview
manager: carmonm
ms.openlocfilehash: a45458e7417bba058522fdc0dbc34fee04ad9af8
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67810828"
---
# <a name="view-and-manage-service-providers"></a>Просмотр поставщиков служб и управление ими

Клиенты могут использовать страницу **Поставщики услуг** на [портале Azure](https://portal.azure.com) для просмотра сведений о поставщиках услуг и предложениях поставщиков услуг, делегирования определенных ресурсов с помощью [делегированного управления ресурсами Azure](../concepts/azure-delegated-resource-management.md) и приобретать дополнительные предложения поставщиков услуг. Хотя здесь будет идти речь о поставщиках услуг и клиентах, предприятия, управляющие несколькими арендаторами, могут использовать один и тот же процесс для консолидации своих возможностей управления.

Чтобы получить доступ к странице **Поставщики услуг** на портале Azure, клиент может выбрать **Все услуги**, а затем найти и выбрать страницу **Поставщики услуг**. Страницу также можно найти, введя "поставщики услуг" в поле поиска в верхней части портала Azure.

Помните, что на странице **Поставщики услуг** отображается информация только о поставщиках услуг, которые имеют доступ к подпискам или группам ресурсов клиента через делегированное управление ресурсами Azure. Если клиент работает с дополнительными поставщиками услуг, которые не используют делегированное управление ресурсами Azure для доступа к ресурсам клиента, информация об этих поставщиках услуг здесь не отображается.

> [!NOTE]
> Поставщики услуг могут просматривать информацию о своих клиентах, перейдя в раздел **Мои клиенты** на портале Azure. Дополнительные сведения см. в разделе [Просмотр клиентов и делегированных ресурсов, а также управление ими](view-manage-customers.md)

## <a name="view-service-provider-details"></a>Просмотр сведений о поставщике услуг

Чтобы просмотреть информацию о поставщиках услуг, с которыми работает клиент, можно выбрать **Provider offers** (Предложения поставщиков) в левой части страницы **Поставщики услуг**.

Для каждого предложения поставщика услуг клиент увидит имя поставщика услуг и связанное с ним предложение, а также имя, введенное клиентом в процессе регистрации.

В столбце **Делегирования** клиент видит, сколько подписок и/или групп ресурсов было делегировано поставщику услуг для этого предложения. Поставщик услуг сможет получить доступ к этим подпискам и/или группам ресурсов и управлять ими в соответствии с уровнями доступа, указанными в предложении.

## <a name="delegate-resources"></a>Делегирование ресурсов

Прежде чем поставщик услуг сможет получить доступ к ресурсам клиента и управлять ими, их следует делегировать. Если клиент принял предложение, но еще не делегировал никаких ресурсов, он увидит примечание в верхней части раздела **Provider offers** (Предложения поставщиков). Это позволяет клиенту знать, что ему нужно принять меры, прежде чем поставщик услуг сможет получить доступ к любым ресурсам клиента.

Чтобы делегировать подписки или группы ресурсов:

1. Установите флажок для строки, содержащей поставщика услуг, предложение и имя. Затем вверху экрана выберите **Делегировать ресурсы**.
1. В разделе **Сведения о предложении** на странице **Делегировать ресурсы** просмотрите сведения о поставщике услуг и предложении. Чтобы просмотреть назначения ролей для предложения, выберите **Щелкните, чтобы просмотреть сведения о выбранном предложении**.
1. В разделе **Делегирование** выберите **Делегировать подписки** или **Делегировать группы ресурсов**.
1. Выберите подписки и/или группы ресурсов, которые вы хотите делегировать для этого предложения, а затем выберите команду **Добавить**.
1. Установите флажок внизу страницы, чтобы подтвердить, что вы хотите предоставить этому поставщику услуг доступ к выбранным вами ресурсам, затем выберите команду **Делегировать**.

## <a name="add-or-remove-service-provider-offers"></a>Добавление или удаление предложений поставщиков услуг

Клиент может добавить новое предложение поставщика услуг на странице **Предложения поставщиков**, выбрав команду **Добавить предложение**. Поставщик услуг должен опубликовать предложение для этого клиента. Затем клиент может выбрать это предложение на экране **Частные предложения** и нажать кнопку **Создать**.

Если клиент хочет удалить предложение поставщика услуг, он может выбрать значок корзины в строке для этого предложения. После подтверждения удаления поставщик услуг больше не будет иметь доступа к ресурсам клиента, которые ранее были делегированы для этого предложения.

## <a name="view-delegations"></a>Просмотр делегирований

Делегирование представляет собой назначения ролей, которые предоставляют разрешения поставщику услуг для ресурсов, делегированных клиентом. Чтобы просмотреть эту информацию, выберите **Делегирования** в левой части страницы **Поставщики услуг**.

Фильтры в верхней части страницы позволяют сортировать и группировать сведения о делегировании или фильтровать их по клиентам, предложениям или ключевым словам.

## <a name="next-steps"></a>Дополнительная информация

- Дополнительные сведения о [Azure Lighthouse](../overview.md).
- Узнайте, как поставщики услуг могут [просматривать клиентов и управлять ими](view-manage-customers.md), перейдя на страницу **Мои клиенты** на портале Azure.