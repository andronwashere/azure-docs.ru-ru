---
title: Управление учетными записями разработчиков с помощью групп в службе управления API Azure | Документация Майкрософт
description: Сведения об управлении учетными записями разработчика с помощью групп в службе управления API Azure
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2018
ms.author: apimpm
ms.openlocfilehash: 5392cf5463dd0b11d1ce53856c8e4e2e788892b0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60658466"
---
# <a name="how-to-create-and-use-groups-to-manage-developer-accounts-in-azure-api-management"></a>Как создавать и использовать группы для управления учетными записями разработчика в службе управления Azure API

В службе управления API группы используются для управления видимостью продуктов для разработчиков. Продукты сначала делаются видимыми для групп, а затем разработчики в таких группах могут просматривать и подписываться на продукты, которые связаны с группами. 

Служба управления API включает несколько неизменяемых системных групп.

* **Администраторы** — члены этой группы являются администраторами подписок Azure. Администраторы управляют экземплярами службы управления API, созданием API, операциями и продуктами, которые используются разработчиками.
* **Разработчики** — в эту группу входят пользователи портала разработчиков, прошедшие проверку подлинности. Разработчики — это клиенты, которые создают приложения с использованием API. Разработчикам предоставляется доступ к порталу разработчика, и они могут создавать приложения, вызывающие операции этого API.
* **Гости** — пользователи портала разработчиков, не прошедшие проверку подлинности; например, в эту группу попадают возможные клиенты, посетившие портал разработчиков экземпляра управления API. Им может быть предоставлен доступ только для чтения, позволяющий просматривать API, но не вызывать их.

Помимо системных групп администраторы могут создавать пользовательские группы или [использовать внешние группы в связанных с ними клиентах Azure Active Directory][leverage external groups in associated Azure Active Directory tenants]. Чтобы обеспечить разработчикам видимость и предоставить доступ к продуктам API, пользовательские и внешние группы можно использовать вместе с системными группами. Например, можно создать одну пользовательскую группу для разработчиков, связанных с конкретной партнерской организацией, и предоставить им доступ к интерфейсам API в продукте, содержащем только соответствующие интерфейсы API. Пользователь может входить в несколько групп.

В этом руководстве показано, как администраторы экземпляра службы управления API могут добавлять новые группы и связывать их с продуктами и разработчиками.

Помимо создания групп и управления ими на портале издателя, это можно делать с помощью сущности [Group](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity) REST API интерфейса API управления.

[!INCLUDE [premium-dev-standard-basic.md](../../includes/api-management-availability-premium-dev-standard-basic.md)]

## <a name="prerequisites"></a>Технические условия

Выполните задачи в этой статье. [Создание экземпляра службы управления API Azure](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-group"> </a>Создание группы

В этом разделе показано, как добавить новую группу к учетной записи службы управления API.

1. В левой части экрана выберите вкладку **Группы**.
2. Щелкните **+Добавить**.
3. Введите уникальное имя для группы и описание (необязательно).
4. Нажмите кнопку **Создать**.

    ![Добавление новой группы](./media/api-management-howto-create-groups/groups001.png)

После создания группа будет добавлена в список **Группы**. <br/>Чтобы изменить **имя** или **описание** группы, щелкните имя группы, а затем — **Параметры**.<br/>Чтобы удалить группу, щелкните имя группы и нажмите кнопку **Удалить**.

Теперь, когда группа создана, ее можно связать с продуктами и разработчиками.

## <a name="associate-group-product"> </a>Связывание группы с продуктом

1. Слева выберите вкладку **Продукты**.
2. Щелкните имя нужного продукта.
3. Щелкните **Контроль доступа**.
4. Щелкните **+ Добавить группу**.

    ![Связывание группы с продуктом](./media/api-management-howto-create-groups/groups002.png)
5. Выберите группу, которую нужно добавить.

    ![Связывание группы с продуктом](./media/api-management-howto-create-groups/groups003.png)

    Чтобы удалить группу из продукта, нажмите кнопку **Удалить**.

    ![Удаление группы](./media/api-management-howto-create-groups/groups004.png)

После установления связи между продуктом и группой разработчики в данной группе могут просматривать продукт и подписаться на него.

> [!NOTE]
> Дополнительную информацию о добавлении групп Azure Active Directory см. в статье [Авторизация учетных записей разработчиков с помощью Azure Active Directory в управлении API Azure](api-management-howto-aad.md).

## <a name="associate-group-developer"> </a>Связывание групп с разработчиками

В этом разделе показано, как связать группы с их элементами.

1. В левой части экрана выберите вкладку **Группы**.
2. Выберите **Элементы**.

    ![Добавление элементов](./media/api-management-howto-create-groups/groups005.png)
3. Нажмите кнопку **+ Добавить** и выберите элемент.

    ![Добавление элементов](./media/api-management-howto-create-groups/groups006.png)
4. Нажмите кнопку **Выбрать**.

После добавления связи между разработчиком и группой ее можно просмотреть на вкладке **Пользователи** .

## <a name="next-steps"> </a>Дальнейшие действия

* После добавления  в группу разработчик может просматривать связанные с данной группой продукты и подписываться на них. Дополнительные сведения см. в статье [Создание и публикация продукта в службе управления API Azure][How create and publish a product in Azure API Management].
* Помимо создания групп и управления ими на портале издателя, это можно делать с помощью сущности [Group](https://docs.microsoft.com/rest/api/apimanagement/apimanagementrest/azure-api-management-rest-api-group-entity) REST API интерфейса API управления.

[Create a group]: #create-group
[Associate a group with a product]: #associate-group-product
[Associate groups with developers]: #associate-group-developer
[Next steps]: #next-steps

[How create and publish a product in Azure API Management]: api-management-howto-add-products.md

[Get started with Azure API Management]: get-started-create-service-instance.md
[Create an API Management service instance]: get-started-create-service-instance.md
[leverage external groups in associated Azure Active Directory tenants]: api-management-howto-aad.md
