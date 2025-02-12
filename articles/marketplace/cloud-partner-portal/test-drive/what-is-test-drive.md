---
title: Что такое тестовый выпуск? | Azure Marketplace
description: Объяснение возможностей тестового выпуска Marketplace
services: Azure, Marketplace, Cloud Partner Portal,
author: pbutlerm
manager: Ricardo.Villalobos
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 5b23a5e93dcbb9c13243240bda760a16d108d731
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64938441"
---
<a name="what-is-test-drive"></a>Что такое тестовый выпуск?
===================

Использование тестового выпуска — это отличный метод презентовать ваше предложение для потенциальных клиентов, предоставив им возможность \'попробовать его перед покупкой\'. Это поможет повысить вероятность превращения потенциального покупателя в реального и создать высокоэффективные интересы.

После предоставления своих контактных данных клиенты получат доступ к предварительно созданному тестовому выпуску: самостоятельно протестируют пробную версию продукта, ознакомятся с ключевыми функциями и преимуществами на практике.

Тестовый выпуск позволяет потенциальным клиентам самостоятельно ознакомиться с продуктами, а вы можете увеличить число покупателей.

<a name="how-does-a-test-drive-work"></a>Как работает тестовый выпуск?
---------------------------

Потенциальный клиент находит ваше приложение в Marketplace, входит в систему и соглашается с условиями использования. На этом этапе клиент получает предварительно настроенную среду, где он может испытывать ваше предложение в течение фиксированного количества часов, а вы получаете сведения о потенциальном клиенте для дальнейших действий.

![Шаг 1. Отображение предложения в Marketplace](./media/what-is-test-drive/step1.png)

![Шаг 2. Экран входа в Marketplace](./media/what-is-test-drive/step1andahalf.png)

![Шаг 3. Экран соглашения издателя в Marketplace](./media/what-is-test-drive/step2.png)

![Шаг 4. Экран установки тестового выпуска](./media/what-is-test-drive/step3.png)

Ниже приведен пример того, как выглядит предложение, когда ему требуется время для развертывания:

![Развертываемое предложение Marketplace](./media/what-is-test-drive/step4.png)

![Экран готовности развертывания тестового выпуска в Marketplace](./media/what-is-test-drive/step5.png)

![Экран выполненного развертывания тестового выпуска в Marketplace](./media/what-is-test-drive/step6.png)

Независимо от того, насколько сложным является ваше приложение, тестовый выпуск Майкрософт позволяет потенциальным клиентам самостоятельно ознакомиться с продуктом. Сегодня мы предлагаем три различных типа тестовых выпусков в зависимости от типа продукта, сценария и используемой площадки marketplace.

- **[Azure Resource Manager](./azure-resource-manager-test-drive.md)** : Тестовый выпуск Azure Resource Manager — шаблон развертывания, который содержит все ресурсы Azure, составляющие решение, создаваемое издателем. Для этого сценария подходят приложения, использующие только ресурсы Azure.
- **[Приложение логики](./logic-app-test-drive.md)** : Логика приложения тестовый выпуск является шаблон развертывания, который должен охватывать все архитектуры сложных решений. Для всех приложений Dynamics или пользовательских продуктов следует использовать этот тип тестового выпуска.
- **Power BI**. Power BI тестовый выпуск является просто ссылку на панель мониторинга разработанными. Для любого продукта, в котором нужна просто демонстрация интерактивной визуализации Power BI, следует использовать этот тип тестового выпуска.
    Необходимо отправить всего лишь внедренный URL-адрес Power BI.

<a name="what-goes-on-in-the-background"></a>Что происходит в фоновом режиме?
-------------------------------

Служба тестового выпуска постоянно поддерживает и обслуживает ваших клиентов без необходимости ручного вмешательства с вашей стороны. Издателю необходимо администрировать и настраивать параметры тестового выпуска из Портала Cloud Partner, а затем эта настройка будет напрямую доступна для клиентов.

После настройки конфигураций для тестового выпуска он становится управляемым экземпляром, который будет развернут по запросу клиента. После назначения экземпляр тестового выпуска доступен для использования в течение определенного периода времени, по истечении которого он удаляется, освобождая место для другого клиента.

<a name="next-steps"></a>Дальнейшие действия
----------

Теперь когда вы ознакомились со всей информацией о тестовом выпуске, прочитайте об определенном типе тестового выпуска, который хотите опубликовать, чтобы узнать об обязательных полях.

- **[Azure Resource Manager](./azure-resource-manager-test-drive.md)**
- **[Приложение логики](./logic-app-test-drive.md)**

Если у вас есть дополнительные вопросы, вы хотите получить рекомендации по устранению неполадок или сделать ваш тестовый выпуск успешным, перейдите к статье [Тестовый выпуск: рекомендации по маркетингу](./marketing-and-best-practices.md).
