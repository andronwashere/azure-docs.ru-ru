---
title: Переход с Пользовательской службы распознавания речи на службу "Речь"
titleSuffix: Azure Cognitive Services
description: Пользовательская служба распознавания речи теперь входит в состав службы "Речь". Перейдите в службу "Речь", чтобы воспользоваться преимуществами повышенных уровней качества и обновленных компонентов.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/01/2018
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 01b853c59723a8ed79cb32b0ee9c245c9c3ffb3f
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68562765"
---
# <a name="migrate-from-the-custom-speech-service-to-the-speech-service"></a>Переход с Пользовательской службы распознавания речи на службу "Речь"

В этой статьей показано, как выполнить перенос приложений из Пользовательской службы распознавания речи в службу "Речь".

Пользовательская служба распознавания речи теперь входит в состав службы "Речь". Переключитесь на речевые службы, чтобы получить преимущества от последних обновлений качества и компонентов.

## <a name="migration-for-new-customers"></a>Миграция для новых клиентов

Модель ценообразования будет проще, если перейти на почасовую модель для службы "Речь".  

1. Создайте ресурс в тех регионах Azure, в которых доступно приложение. Имя ресурса Azure — **Речь**. Вместо того, чтобы создавать отдельные ресурсы, вы можете использовать один ресурс Azure для следующих служб в том же регионе:

    * Преобразование речи в текст
    * Настройка преобразования речи в текст
    * Преобразование текста в речь
    * Перевод речи

2. Загрузите [пакет SDK службы "Речь"](speech-sdk.md).

3. Следуйте кратким руководствам и примерам пакета SDK, чтобы использовать правильные интерфейсы API. Если вы используете интерфейсы REST API, необходимо также использовать правильные конечные точки и ключи ресурсов.

4. Обновите клиентское приложение для использования речевых служб и API-интерфейсов.

## <a name="migration-for-existing-customers"></a>Миграция для имеющихся клиентов

Перенесите существующие ключи ресурсов в службы распознавания речи на портале речевых служб. Выполните следующие действия.

> [!NOTE]
> Ключи ресурсов можно переносить только в том же регионе.

1. Войдите на портал [cris.ai](https://cris.ai/Home/CustomSpeech) и в правом верхнем меню выберите подписку.

2. Выберите **Migrate selected subscription** (Перенос выбранной подписки).

3. В текстовом поле введите ключ подписки и выберите **Перенос**.

## <a name="next-steps"></a>Следующие шаги

* [Опробуйте речевые службы бесплатно](get-started.md).
* [Сведения об API преобразования речи в текст](./speech-to-text.md)

## <a name="see-also"></a>См. также

* [Общие сведения о службе "Речь"](overview.md)
* [Документация по речевым службам и речевым ПАКЕТам](speech-sdk.md#get-the-sdk)
