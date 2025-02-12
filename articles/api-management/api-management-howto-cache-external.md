---
title: Использование внешнего кэша в службе управления API Azure | Документация Майкрософт
description: Узнайте, как настроить и использовать внешний кэш в службе управления API Azure.
services: api-management
documentationcenter: ''
author: mikebudzynski
manager: erikre
editor: ''
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2019
ms.author: apimpm
ms.openlocfilehash: e2362d06fa0ef795122a2d47a7a621b66fdd9470
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65780361"
---
# <a name="use-an-external-azure-cache-for-redis-in-azure-api-management"></a>Использование внешнего кэша Redis для Azure в Управлении API Azure

Помимо использования встроенного кэша, Управление API Azure также позволяет кэшировать ответы во внешнем кэше Redis для Azure.

Использование внешнего кэша позволяет преодолеть некоторые ограничения встроенного кэша. Это особенно полезно, если необходимо:

* избежать периодического очищения кэша при обновлении службы управления API;
* получить больший контроль над конфигурацией кэша;
* кэшировать больше данных, чем позволяет уровень службы управления API;
* использовать кэширование на уровне "Потребление" службы управления API.

Дополнительные сведения о кэшировании см. в статьях [Политики кэширования в службе управления API](api-management-caching-policies.md) и [Пользовательское кэширование в службе управления API Azure](api-management-sample-cache-by-key.md).

![Добавление своего собственного кэша в APIM](media/api-management-howto-cache-external/overview.png)

Из этого руководства вы узнаете, как выполнять такие задачи:

> [!div class="checklist"]
> * Добавление внешнего кэша в службу управления API

## <a name="prerequisites"></a>Технические условия

Для работы с этим руководством вам потребуется следующее:

+ [Создание экземпляра службы управления API Azure](get-started-create-service-instance.md)
+ Понимание принципа [кеширования в службе управления API Azure](api-management-howto-cache.md)

## <a name="create-cache"> </a> Создание кэша Redis для Azure

В этом разделе описано, как создать в Azure кэш Redis для Azure. Если у вас уже есть кэш Redis для Azure в Azure или за пределами, вы можете <a href="#add-external-cache">пропустить</a> этот раздел.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="add-external-cache"> </a>Добавление внешнего кэша

Выполните описанные ниже действия, чтобы добавить внешний кэш Redis для Azure в Управление API Azure.

![Добавление своего собственного кэша в APIM](media/api-management-howto-cache-external/add-external-cache.png)

> [!NOTE]
> **Использовать из** параметр указывает сообщим региональное развертывание управления API с помощью кэша в случае нескольких регионов конфигурации службы управления API. Кэши, заданные как **Default** (По умолчанию), будут переопределены кэшами с региональным значением.
>
> Например, если служба управления API размещена в регионах "Восточная часть США", "Юго-Восточная Азия" и "Западная Европа", и существуют два настроенных кэша, один **Default** (По умолчанию) и второй для региона **Юго-Восточная Азия**, служба управления API в регионе **Юго-Восточная Азия** будет использовать собственный кэш, а два других региона — запись кэша **Default** (По умолчанию).

### <a name="add-an-azure-cache-for-redis-from-the-same-subscription"></a>Добавление кэша Redis для Azure из той же подписки

1. Перейдите к экземпляру службы управления API на портале Azure.
2. Выберите вкладку **External cache** (Внешний кэш) в меню слева.
3. Щелкните **+ Добавить**.
4. В раскрывающемся поле **Cache instance** (Экземпляр кэша) выберите свой кэш.
5. Выберите **по умолчанию** или укажите требуемый регион в **использовать из** Раскрывающееся поле.
6. Выберите команду **Сохранить**.

### <a name="add-an-azure-cache-for-redis-hosted-outside-of-the-current-azure-subscription-or-azure-in-general"></a>Добавление кэша Redis для Azure, размещенного за пределами текущей подписки Azure или Azure

1. Перейдите к экземпляру службы управления API на портале Azure.
2. Выберите вкладку **External cache** (Внешний кэш) в меню слева.
3. Щелкните **+ Добавить**.
4. В раскрывающемся поле **Cache instance** (Экземпляр кэша) выберите **Custom** (Настраиваемый).
5. Выберите **по умолчанию** или укажите требуемый регион в **использовать из** Раскрывающееся поле.
6. В поле **Строка подключения** укажите строку подключения к кэшу Redis для Azure.
7. Выберите команду **Сохранить**.

## <a name="use-the-external-cache"></a>Использование внешнего кэша

Как только внешний кэш в службе управления API Azure настроен, его можно использовать через политики кэширования. Подробные инструкции см. в разделе [Добавление кэширования для повышения производительности в службе управления API Azure](api-management-howto-cache.md).

## <a name="next-steps"> </a>Дальнейшие действия

* Дополнительные сведения о политиках кэширования см. в разделе [Политики кэширования][Caching policies] [справочника по политикам управления API][API Management policy reference].
* Сведения о кэшировании элементов по ключу с помощью выражений политики см. в статье [Пользовательское кэширование в службе управления API Azure](api-management-sample-cache-by-key.md).

[API Management policy reference]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Caching policies]: https://msdn.microsoft.com/library/azure/dn894086.aspx
