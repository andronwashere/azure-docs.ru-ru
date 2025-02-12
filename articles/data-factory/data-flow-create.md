---
title: Создание сопоставления потока данных Фабрики данных Azure
description: Как создать Azure фабрики сопоставление данных потока данных
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/12/2019
ms.openlocfilehash: 366ed60534544ebbf889e2f72fe703f9b821f871
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65235653"
---
# <a name="create-azure-data-factory-data-flow"></a>Создание потока данных Фабрики данных Azure

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Сопоставление потоков данных в ADF обеспечивает способ преобразования данных в масштабе без какого-либо кодирования. Вы можете создать задание преобразования данных в конструкторе потока данных, создав серию преобразований. Начните с произвольным количеством исходных преобразований, за которыми следуют шаги преобразования данных. Затем заполните поток данных с помощью приемника, чтобы получить результаты в пункте назначения.

Приступить к работе, создав новую фабрику данных версии 2 на портале Azure. После создания новой фабрики щелкните плитку "Author & Monitor" (Создание и мониторинг), чтобы запустить пользовательский интерфейс Фабрики данных.

![Варианты потока данных](media/data-flow/v2portal.png "создание потока данных")

После входа в пользовательский интерфейс Фабрики данных, вы можете использовать примеры потоков данных. Примеры доступны в коллекции шаблонов ADF. В ADF создайте "Конвейер из шаблона", а в коллекции шаблонов выберите категорию "Поток данных".

![Варианты потока данных](media/data-flow/template.png "создание потока данных")

Появится запрос на ввод учетных данных хранилища BLOB-объектов Azure.

![Варианты потока данных](media/data-flow/template2.png "создание потока данных 2")

[Данные, используемые для этих примеров можно найти здесь](https://github.com/kromerm/adfdataflowdocs/tree/master/sampledata). Загрузите примеры данных и сохраните файлы в своих учетных записях хранилища BLOB-объектов Azure, чтобы вы могли выполнить примеры.

## <a name="create-new-data-flow"></a>Создать новый поток данных

Используйте кнопку Создание ресурса «плюс» в пользовательском Интерфейсе фабрики данных AZURE для создания потоков данных.

![Варианты потока данных](media/data-flow/newresource.png "Новый ресурс")

## <a name="next-steps"></a>Дальнейшие действия

Приступить к созданию преобразования данных с помощью [преобразование исходного](data-flow-source.md).
