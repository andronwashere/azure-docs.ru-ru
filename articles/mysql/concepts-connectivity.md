---
title: Обработка временных ошибок подключения к Базе данных Azure для MySQL | Документация Майкрософт
description: Узнайте, как обрабатывать временные ошибки подключения к Базе данных Azure для MySQL.
keywords: mysql connection,connection string,connectivity issues,transient error,connection error
author: jan-eng
ms.author: janeng
ms.service: mysql
ms.topic: conceptual
ms.date: 11/09/2018
ms.openlocfilehash: 8942223ce233d424e2368e90d2fbac92b1a443f3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60525469"
---
# <a name="handling-of-transient-connectivity-errors-for-azure-database-for-mysql"></a>Обработка временных ошибок подключения к Базе данных Azure для MySQL

В этой статье описывается, как обрабатывать временные ошибки, связанные с подключением к Базе данных Azure для MySQL.

## <a name="transient-errors"></a>Временные ошибки

Временная ошибка, также известная как временный сбой, является ошибкой, которая устраняется автоматически. Чаще всего эти ошибки проявляются в виде сброса соединения с сервером базы данных. При этом невозможно установить новые подключения к серверу. Временные ошибки могут возникать, например, при сбое оборудования или сети. Другой причиной может стать развертывание новой версии службы PaaS. Большинство из этих событий автоматически устраняются системой менее чем за 60 секунд. При проектировании и разработке приложений в облаке рекомендуется учитывать возможность временных ошибок. Нужно учитывать, что они могут произойти в любом компоненте в любое время, и предусмотреть соответствующую логику для таких ситуаций.

## <a name="handling-transient-errors"></a>Обработка временных ошибок

Временные ошибки следует обрабатывать с использованием логики повторных подключений. Нужно учитывать следующие ситуации.

* При попытке открыть соединение возникает ошибка.
* Неактивное подключение сбрасывается на стороне сервера. Попытка выполнить команду завершается сбоем.
* Активное подключение, по которому в настоящее время выполняется команда, сбрасывается.

Ошибки первого и второго типа достаточно просто устранить. Попробуйте открыть подключение еще раз. Когда вам это удастся, система устранит временную ошибку. Базу данных Azure для MySQL можно использовать снова. Мы рекомендуем подождать перед повторной попыткой подключения. Отключитесь, если исходные повторные попытки не дают результата. Таким образом, система может использовать все доступные ресурсы для устранения ошибки. Ниже приведен шаблон для выполнения.

* Подождите 5 секунд перед первой повторной попыткой.
* Для каждой следующей попытки увеличивайте время ожидания экспоненциально до 60 секунд.
* Задайте максимальное число повторных попыток, после чего приложение подтвердит сбой операции.

Когда происходит сбой подключения с активной транзакцией, управлять процессом восстановления становится сложнее. Ниже приведены два возможных варианта. Если транзакция была доступна только для чтения, вы можете безопасно повторно открыть подключение и повторить транзакцию. Однако если транзакция также была доступна и для записи в базу данных, необходимо определить, был ли выполнен откат транзакции или же она была успешно проведена до возникновения временной ошибки. В этом случае вы могли просто не получить подтверждение фиксации с сервера базы данных.

Один из способов сделать это — создать уникальный идентификатор на клиенте, который используется для всех повторных попыток. Вы передаете этот уникальный идентификатор как часть транзакции на сервер и сохраняете его в столбце с уникальным ограничением. Таким образом можно безопасно повторить транзакцию. Это действие будет выполнено успешно, если при предыдущей транзакции был выполнен откат, а созданный клиентом уникальный идентификатор еще не существует в системе. Произойдет сбой с оповещением о дубликате ключа, если уникальный идентификатор был сохранен ранее в результате успешной транзакции.

Если программа взаимодействует с Базой данных Azure для MySQL через стороннее программное обеспечение промежуточного слоя, спросите поставщика, предусмотрена ли в этом программном обеспечении логика повторных попыток в случае временных ошибок.

Не забудьте проверить логику повторных попыток. Например, попробуйте выполнить свой код, увеличивая или уменьшая число вычислительных ресурсов сервера Базы данных Azure для MySQL. Ваше приложение должно без каких-либо проблем справиться с небольшим простоем во время этой операции.

## <a name="next-steps"></a>Дальнейшие действия

* [Узнайте, как устранить проблемы с подключением к Базе данных Azure для MySQL](howto-troubleshoot-common-connection-issues.md)
