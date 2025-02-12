---
title: Преобразование новой ветви сопоставления потока данных Фабрики данных Azure
description: Преобразование новой ветви сопоставления потока данных Фабрики данных Azure
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 9fac78f21f2f128ccb040e176891c33d39bf2820
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61349158"
---
# <a name="azure-data-factory-mapping-data-flow-new-branch-transformation"></a>Преобразование новой ветви сопоставления потока данных Фабрики данных Azure

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Параметры ветви](media/data-flow/menu.png "меню")

Создание ветви будет принимать текущий поток данных в вашем потоке данных и реплицировать его в другой поток. Используйте новую ветвь для выполнения нескольких наборов операций и преобразований в одном и том же потоке данных.

Пример: Ваш поток данных имеет "Преобразование источника" с выбранным набором столбцов и преобразований типов данных. Затем вы помещаете "Производный столбец" сразу после этого источника. В разделе "Производный столбец" вы создаете новое поле, которое объединяет имя и фамилию, чтобы создать новое поле "Полное имя".

Вы можете обработать этот новый поток набором преобразований и приемником в одной строке и использовать команду "Создать ветвь", чтобы создать копию этого потока, где вы можете преобразовать те же данные с другим набором преобразований. Преобразовав эти скопированные данные в отдельную ветвь, вы можете впоследствии передать эти данные в отдельное местоположение.

> [!NOTE]
> "Создать ветвь" будет отображаться как действие в меню "+ Преобразование" только в том случае, если после текущего расположения, в котором вы пытаетесь создать ветвь, есть последующее преобразование. То есть вы не увидите здесь команду "Создать ветвь", пока не добавите еще одно преобразование после выбора.

![Ветвь](media/data-flow/branch2.png "ветвь 2")
