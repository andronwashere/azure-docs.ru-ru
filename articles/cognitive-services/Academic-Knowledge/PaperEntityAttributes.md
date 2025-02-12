---
title: Атрибуты сущности Academic Knowledge API
titlesuffix: Azure Cognitive Services
description: Изучите атрибуты, которые вы можете использовать с сущностью документа в Academic Knowledge API.
services: cognitive-services
author: alch-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: academic-knowledge
ms.topic: conceptual
ms.date: 03/31/2017
ms.author: alch
ms.openlocfilehash: 92844b5faf691b67617c9f3424a1322aa05429bb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64875732"
---
# <a name="paper-entity"></a>Сущность документа

<sub> *Атрибуты, приведенные ниже, относятся к сущности документа. (Ty = "0") </sub>


Name    |Описание                                        |type       | Операции
------- | ------------------------------------------------- | --------- | ----------------------------
Идентификатор      |Идентификатор сущности                                          |Int64      |Равно
Ti      |Название документа                                        |String     |Равно,<br/>StartsWith;
L       |Код языка бумаги, разделенных точкой "\@\@\@"          |String     |Равно
Да       |Год создания документа                                         |Int32      |Равно,<br/>IsBetween
D       |Дата создания документа                                         |Дата       |Равно,<br/>IsBetween
CC      |Показатель цитируемости                                     |Int32      |нет  
ECC     |Расчетный показатель цитируемости                           |Int32      |нет
AA.AuN  |Имя автора                                        |String     |Равно,<br/>StartsWith;
AA.AuId |Идентификатор автора                                          |Int64      |Равно
AA.AfN  |Имя принадлежности автора                            |String     |Равно,<br/>StartsWith;
AA.AfId |Идентификатор принадлежности автора                              |Int64      |Равно
AA.S    |Количество документов, заказанных автором                         |Int32      |Равно
F.FN    |Имя области исследований                                |String     |Равно,<br/>StartsWith;
F.FId   |Идентификатор области исследований                                  |Int64      |Равно
J.JN    |Название журнала                                       |String     |Равно,<br/>StartsWith;
J.JId   |Идентификатор журнала                                         |Int64      |Равно
C.CN    |Имя серии конференций                             |String     |Равно,<br/>StartsWith;
C.CId   |Идентификатор серии конференций                               |Int64      |Равно
RId     |Идентификатор указанных документов                              |Int64[]    |Равно
W       |Слова из названия документа и аннотации                |String[]   |Равно
E       |Расширенные метаданные (см. таблицу ниже)                |String     |нет  
        


## <a name="extended-metadata-attributes"></a>Расширенные атрибуты метаданных ##

Name    | Описание               
--------|---------------------------    
DN      | Отображаемое название документа 
S       | Источники — список веб-источников документа, отсортированных по статическому рангу
S.Ty    | Тип источника (1:HTML, 2:Текст, 3:PDF, 4:DOC, 5:PPT, 6:XLS, 7:PS)
S.U     | URL-адрес источника
VFN     | Полное имя места проведения — полное название журнала или конференции
VSN     | Сокращенное имя места проведения — сокращенное название журнала или конференции
V       | Номер — номер журнала
BV      | Название журнала
BT      | 
PB      | Сокращения журнала
I       | Выпуск — выпуск журнала
FP      | Первая страница — первая страница в документе
LP      | Последняя страница — последняя страница в документе
DOI     | Идентификатор цифрового объекта
CC      | Содержимое цитаты — набор указанных идентификаторов документа и связанного содержимого в нем (например, [{123:["коричневые лисицы известны тем, что они прыгают, как указано в документе 123", "ленивые собаки являются историческим неправильным выражением, как показано в документе 123"]})
IA      | Инвертированная аннотация
IA.IndexLength| Число элементов в индексе (количество слов в аннотации)
IA.InvertedIndex| Список слов в аннотации и их позиции в оригинальной аннотации (например, [{"это":[0, 15, 30]}, {"коричневая":[1]}, {"лисица":[2]}])
