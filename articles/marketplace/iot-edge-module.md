---
title: Модули Azure IoT Edge
description: Предложение модуля IoT Edge в Azure Marketplace для издателей приложений и служб.
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, IoT Edge module offer
author: qianw211
manager: pabutler
ms.service: marketplace
ms.topic: article
ms.date: 09/22/2018
ms.author: pabutler
ms.openlocfilehash: 3010b63c7c4c575d915789c19b60710194c79196
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67874675"
---
# <a name="iot-edge-modules"></a>Модули IoT Edge

Платформа [Azure IoT Edge](https://azure.microsoft.com/services/iot-edge/) поддерживается облачными службами Azure.  Эта платформа позволяет пользователям развертывать облачные рабочие нагрузки для непосредственного выполнения на устройствах Интернета вещей.  Модуль IoT Edge может выполнять автономные рабочие нагрузки и совершать анализ данных локально. Этот тип предложения помогает экономить пропускную способность, защищать локальные и конфиденциальные данные и обеспечивает малое время отклика.  Теперь вы можете воспользоваться этими заранее подготовленными рабочими нагрузками. До сих пор было доступно лишь несколько решений от корпорации Майкрософт.  Вам нужно было тратить время и ресурсы на создание своих собственных решений для Интернета вещей.

С вводом [модулей IoT Edge в Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/internet-of-things?page=1) теперь есть единственное расположение, где издатели могут публиковать и продавать свои решения аудитории Интернета вещей. Разработчики решений для Интернета вещей могут в конечном итоге найти и приобрести возможности для ускорения разработки своих решений.  

## <a name="key-benefits-of-iot-edge-modules-in-azure-marketplace"></a>Основные преимущества модулей IoT Edge в Azure Marketplace:

| **Для издателей**    | **Для клиентов (разработчиков решений для Интернета вещей)**  |
| :------------------- | :-------------------|
| Охват миллионов разработчиков, которые хотят создавать и развертывать решения IoT Edge.  | Создание решений IoT Edge с уверенностью в использовании защищенных и протестированных компонентов. |
| Единоразовая публикация и запуск на любом оборудовании IoT Edge, поддерживающем контейнеры. | Сокращение времени до выхода на рынок за счет развертывания модулей IoT Edge корпорации Майкрософт и сторонних разработчиков, предназначенных для конкретных нужд. |
| Монетизация с использованием гибких вариантов выставления счетов: <ul> <li> собственная и бесплатная лицензия. </li> </ul> | Совершение покупок с использованием удобной модели выставления счетов: <ul> <li> собственная и бесплатная лицензия. </li> </ul> |

## <a name="what-is-an-iot-edge-module"></a>Что такое модуль IoT Edge?

Azure IoT Edge позволяет развертывать модули бизнес-логики на пограничных устройствах и управлять этими модулями. Модуль Azure IoT Edge представляет собой наименьшую единицу вычислительных ресурсов, которыми управляет IoT Edge. Эти модули могут содержать службы Майкрософт (например, Azure Stream Analytics), сторонние службы или пользовательский код для конкретного решения. Дополнительные сведения о модулях IoT Edge см. в статье [Общие сведения о модулях IoT Edge Azure](https://docs.microsoft.com/azure/iot-edge/iot-edge-modules).

**Какова разница между типом предложения контейнера и типом предложения модуля IoT Edge?**

Тип предложения IoT Edge — это тип контейнера, работающего на устройстве IoT Edge. Это предложение поставляется с параметрами конфигурации по умолчанию для работы в контексте IoT Edge и может содержать пакет SDK для модуля IoT Edge для интеграции со средой выполнения IoT Edge.

## <a name="publishing-your-iot-edge-module"></a>Публикация модуля IoT Edge

**Выбор правильного онлайн-магазина**

Модули IoT Edge публикуются только в Azure Marketplace. AppSource не поддерживается.  Дополнительные сведения о различиях и целевой аудитории в онлайн-магазинах см. в статье [Выбор варианта публикации](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type).
 
**Варианты выставления счетов**

Marketplace в настоящее время поддерживает **бесплатные** и **собственные лицензии (BYOL)** как варианты выставления счетов для модулей IoT Edge.
 
**Варианты публикации**

Во всех случаях необходимо выбирать для модулей IoT Edge вариант публикации **Транзакция**.  Варианты публикации см. в [этой статье](https://docs.microsoft.com/azure/marketplace/determine-your-listing-type).  

## <a name="eligibility-criteria"></a>Условия для допуска

К предложениям модулей IoT Edge применяются все условия соглашений и политик Microsoft Azure Marketplace.  Кроме того, существуют предварительные и технические требования к модулям IoT Edge.  

**Предварительные требования**

Чтобы опубликовать модуль IoT Edge в Azure Marketplace, вам потребуется выполнить следующие условия:

- Получить доступ к Порталу Cloud Partner. Дополнительные сведения см. в [руководстве по публикации в Azure Marketplace и AppSource](https://docs.microsoft.com/azure/marketplace/marketplace-publishers-guide).
- Разместите ваш модуль IoT Edge в Реестре контейнеров Azure. 
- Подготовьте метаданные модуля IoT Edge, например (не исчерпывающий список): 
    - название;
    - описание (в формате HTML);
    - изображение логотипа (в формате PNG и в одном из фиксированных размеров, включая 40 x 40, 90 x 90, 115 x 115, 255 x 115 пикселей);
    - условия использования и политика конфиденциальности;
    - конфигурация модуля по умолчанию (маршрут, требуемые свойства двойника, createOptions, переменные среды);
    - Документация
    - контактные данные службы поддержки.

**Технические требования**

Основные технические требования к модулю IoT Edge для получения сертификата и публикации в Azure Marketplace подробно описаны в статье [Подготовка технических ресурсов для модуля IoT Edge](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-technical-assets).  

## <a name="documentation-and-resources"></a>Документация и ресурсы

[Создание предложения модуля IoT Edge](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-create-offer) — шаги для публикации нового модуля IoT Edge с помощью Портала Cloud Partner.

## <a name="next-steps"></a>Следующие шаги

Если это еще не сделано:

- Зарегистрируйтесь в партнерской сети [Microsoft Partner Network](https://partner.microsoft.com/membership).
- Создайте [учетную запись Майкрософт](https://account.microsoft.com/account/) (требуется для предложений типа "Транзакция" Microsoft Azure Marketplace; рекомендуется для других).
- Подайте [форму регистрации в Marketplace](https://partner.microsoft.com/dashboard/account/v3/enrollment/introduction/azureisv). Дополнительные сведения см. в статье [Создание учетной записи центра партнеров](https://docs.microsoft.com/azure/marketplace/partner-center-portal/create-account) .

Если вы зарегистрированы и создаете предложение или работаете над существующим,

- [Войдите на Портал Cloud Partner](https://cloudpartner.azure.com/), чтобы создать или завершить предложение.
- Сведения о том, как опубликовать предложение модуля IoT Edge, см. в статье [Общие сведения о публикации предложения модуля IoT Edge](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/iot-edge-module/cpp-offer-process-parts).
