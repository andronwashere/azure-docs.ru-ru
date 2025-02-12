---
title: Часто задаваемые вопросы — Personality Chat
titlesuffix: Azure Cognitive Services
description: Часто задаваемые вопросы по индивидуальному чату
services: cognitive-services
author: noellelacharite
manager: nitinme
ms.service: cognitive-services
ms.subservice: personality-chat
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: nolachar
comment: As a bot developer, I want my bot to be able to handle small talk in a consistent tone so that my bot appears more complete and conversational.
ms.openlocfilehash: 88dfbb2a547b44de1bb98ca536c7841570a12168
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60829979"
---
# <a name="frequently-asked-questions"></a>Часто задаваемые вопросы

## <a name="why-doesnt-this-answer-every-question-i-ask-it-like-other-chat-bots"></a>Почему чат-бот не отвечает на каждый мой вопрос, как другие боты?

Служба индивидуального чата в проектах расширяет возможности чат-бота с помощью непринужденных разговоров на общие темы, которые раскрывают индивидуальность и обеспечивают более полноценное взаимодействие с пользователем. Она не предназначена для длинных бесед на темы, которые не относятся к основной функции бота. Хотя бот может отвечать на все реплики, в бета-версии он ограничен общими темами для непринужденных разговоров.

## <a name="how-can-i-customize-the-personality-to-suit-my-brand"></a>Как выполнить индивидуальную настройку в соответствии с моей торговой маркой?

Выберите самую подходящую личность из доступных по умолчанию. Сейчас вы можете взять за основу редакторскую библиотеку и внести изменения в ответы, чтобы они более полно соответствовали вашему фирменному стилю. В будущем можно будет отправить пример набора фраз для выбранной личности и найти самую подходящую ее версию по идентификатору личности. Также существуют способы повторного обучения и настройки модели.

## <a name="is-this-service-powering-existing-intelligent-agents-such-aszo"></a>Поддерживает ли эта служба имеющиеся интеллектуальные агенты, например Zo?

В службах, поддерживающих Zo, Cortana и индивидуальный чат в проектах, используются некоторые похожие методики, но их стеки отличаются. Эта служба создана с учетом результатов обучения, полученных при взаимодействии с Zo и Cortana.

## <a name="can-this-service-lead-to-bad-customer-experiences"></a>Может ли эта служба формировать отрицательные впечатления у пользователей?

Для обеспечения более широких возможностей служба индивидуального чата может генерировать ответы, не входящие в редакторский набор данных, пытаясь интерпретировать все данные, которые вводит пользователь. Поэтому существует вероятность того, что ответ не будет казаться уместным в том или ином контексте. Для предотвращения нежелательных ответов внедрен ряд элементов управления на основе знаний, полученных от интеллектуальных агентов типа Zo. По умолчанию служба индивидуального чата в проектах настроена так, чтобы отвечать исключительно на распознанные намерения пользователя. Возможно, вы захотите проверить, подходит ли вам служба индивидуального чата в проектах. Если вы можете указать на аспекты, по которым требуется дополнительное обучение, мы будем признательны вам за отзыв. При использовании этой службы для клиентов в будущем рекомендуем внедрить анонимное ведение журнала. Это поможет определять проблемы при динамическом взаимодействии с пользователями.
