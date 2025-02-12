---
title: Поддержка языков — QnA Maker
titleSuffix: Azure Cognitive Services
description: Язык базы знаний влияет на способность службы QnA Maker автоматически извлекать вопросы и ответы из источников, а также на релевантность результатов, предоставляемых службой QnA Maker в ответ на запросы пользователей. Список языков и региональных параметров, а также естественных языков, поддерживаемых QnA Maker для базы знаний. Не следует смешивать языки в одной базе знаний.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 03/21/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: a543b17633b99bea63d72f46ba856a8b4593d16a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67439556"
---
# <a name="language-support-for-qna-maker"></a>Языковая поддержка QnA Maker

Язык базы знаний влияет на способность службы QnA Maker автоматически извлекать вопросы и ответы из [источников](../Concepts/data-sources-supported.md), а также на релевантность результатов, предоставляемых службой QnA Maker в ответ на запросы пользователей.

## <a name="auto-extraction"></a>Автоматическое извлечение
QnA Maker поддерживает извлечение вопросов и ответов из страницы на любом языке. Однако эффективность извлечения намного выше для указанных далее языков, так как QnA Maker использует ключевые слова для определения вопросов.

|Поддерживаемые языки| Языковой стандарт|
|-----|----|
|Английский|en-*|
|Французский|fr-*|
|Итальянский|it-*|
|Немецкий|de-*|
|Испанский|es-*|

## <a name="primary-language-detection"></a>Определение основного языка

Основной язык, используемый для обнаружения имеет значение для ресурса QnA Maker и всех баз знаний, созданные на этот ресурс, при добавлении первого документа или URL-адрес первой базы знаний. Язык не может быть изменен. 

Если пользователь планирует для поддержки нескольких языков, они должны иметь ресурса QnA Maker для каждого языка. Узнайте, как [создать на основе языка базы знаний QnA Maker](../how-to/language-knowledge-base.md).  

Проверьте основной язык, сделав следующее:

1. Войдите на [портале Azure](https://portal.azure.com).  
1. Найдите и выберите ресурс службы поиска Azure, создаваемых как часть ресурса QnA Maker. Имя ресурса службы поиска Azure начнется с тем же именем, что и ресурс QnA Maker и будет иметь тип **службы поиска**. 
1. Из **Обзор** страницу поиска ресурса, выберите **индексы**. 
1. Выберите индекс **testkb**.
1. Выберите **поля** вкладки. 
1. Представление **анализатор** столбец для **вопросы** и **ответов** поля. 


## <a name="query-matching-and-relevance"></a>Сопоставление запросов и релевантность
Служба QnA Maker зависит от [анализаторов языков](https://docs.microsoft.com/rest/api/searchservice/language-support) в службе "Поиск Azure" при предоставлении результатов. Для языков en-* доступны специальные функции повторного ранжирования, позволяющие улучшить релевантность.

Хотя возможности службы "Поиск Azure" работают одинаково для поддерживаемых языков, в QnA Maker предусмотрено дополнительное средство ранжирования, расположенное над результатами поиска Azure. В этой модели средства ранжирования, мы используем некоторые особые функции семантических и на основе word в en-*, которая еще не доступны для других языков. Мы не предоставляют эти возможности, как они являются частью внутренней работы средства ранжирования QnA Maker. 

QnA Maker [автоматически определяет язык этой базы знаний](#primary-language-detection) во время создания и соответствующим образом задает анализатор. Базы знаний можно создавать на указанных ниже языках. 

|Поддерживаемые языки|
|-----|
|Арабский|
|Армянский|
Бенгальский|
|Баскский|
|Болгарский|
|Каталанский|
|Китайский, упрощенное письмо|
|Китайский, традиционное письмо|
|Хорватский|
|Чешский|
|Датский|
|Нидерландский|
|Английский|
|Эстонский|
|Финский|
|Французский|
|Галисийский|
|Немецкий|
|Греческий|
|Гуджарати|
|Иврит|
|Хинди|
|Венгерский|
|Исландский|
|Индонезийский|
|Ирландский|
|Итальянский|
|Японский|
|Каннада|
|Корейский|
|Латышский|
|Литовский|
|Малаялам|
|Малайский|
|Норвежский|
|Польский|
|Португальский|
|Панджаби|
|Румынский|
|Русский|
|Сербский (кириллица)|
|Сербский (латиница)|
|Словацкий|
|Словенский|
|Испанский|
|Шведский|
|Тамильский|
|Телугу|
|Тайский|
|Турецкий|
|Украинский|
|Урду|
|Вьетнамский|
