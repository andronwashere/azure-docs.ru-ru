---
title: Сопоставление преобразование строки Alter потока данных фабрики данных Azure
description: Обновление целевой базы данных с помощью Azure фабрики сопоставление данных потока Alter строки преобразования данных
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: f0ac5bb36079983b10e4d86cc776bd4e5ee6817d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65520146"
---
# <a name="azure-data-factory-alter-row-transformation"></a>Преобразование строки Alter фабрики данных Azure

Используйте преобразование строки Alter для задания политик вставки, удаления, обновления и вставки-обновления в строках. Можно добавить один ко многим условия как выражения. Каждый из этих условий может привести к (или строки) строки вставлены, обновлено, удалено или upsert. Строки ALTER может создавать действия DDL и DML в базе данных.

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

![Изменить параметры строки](media/data-flow/alter-row1.png "Alter Settings строки")

> [!NOTE]
> Преобразования строк ALTER будет работать только на приемники базы данных в потоке данных. Действия, которые назначены строк (insert, update, delete, upsert) не будет выполняться во время сеансов отладки. Необходимо добавить задачу выполнения потока данных конвейера и использовать конвейер debug или триггеры для применения политик строки alter на таблицах базы данных.

## <a name="view-policies"></a>Просмотр политик

Переключиться в режим потока отладки данных, а затем просмотрите результаты политик строки alter в области предварительного просмотра данных. Выполнение в режиме потока отладки данных строку alter не даст DDL и DML действия, выполняемые на целевом сервере. Чтобы получить эти действия, которые происходят, выполнение потока данных внутри потока данных, выполните действия в конвейере.

![Изменить политики строки](media/data-flow/alter-row3.png "Alter политики строки")

Это позволит вам для проверки и просмотреть состояние каждой строки, в зависимости от условия. Что представляет значок для каждой операции вставки, обновления, удаления и вставки-обновления действие, которое будет выполняться в потоке данных, указывающее действие, которое будет иметь место при выполнении потока данных в конвейере.

## <a name="sink-settings"></a>Параметры приемника

Необходимо иметь базу данных типа для строки Alter для работы приемника. В приемнике параметры необходимо задать каждого действия, чтобы был разрешен.

![Изменять строку в качестве приемника](media/data-flow/alter-row2.png "Alter приемника строки")

По умолчанию в поток данных в ADF с приемниками базы данных задается для вставки строк. Если вы хотите разрешить обновления, операция Upsert и удаления также, необходимо также проверить этих полей в приемник, который необходимо разрешить действия.

> [!NOTE]
> Если вашей операции вставки, обновления или операции Upsert изменения схемы целевой таблицы в приемнике, произойдет сбой потока данных. Чтобы изменения схемы целевой базы данных, необходимо выбрать параметр «Повторное создание таблицы» в приемнике. Это будет удалите и повторно с новым определением схемы таблицы.

## <a name="next-steps"></a>Дальнейшие действия

После преобразования строк Alter, может потребоваться [приемника данных в целевое хранилище данных](data-flow-sink.md).
