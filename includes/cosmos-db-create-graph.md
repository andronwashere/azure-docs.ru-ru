---
title: включение файла
description: включение файла
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.topic: include
ms.date: 04/13/2018
ms.author: sngun
ms.custom: include file
ms.openlocfilehash: 9800848b24523a0eb5992e64ce1580cc53b0e3a0
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185835"
---
Теперь вы можете использовать обозреватель данных на портале Azure для создания базы данных графов. 

1. Щелкните **Обозреватель данных** > **Новый граф**.

    Справа отобразится область **Добавление графа** (вам может потребоваться прокрутить вправо, чтобы увидеть ее).

    ![Страница добавления графа в обозревателе данных на портале Azure](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer-graph.png)

2. На странице **Добавление графа** введите параметры для нового графа.

    Параметр|Рекомендуемое значение|ОПИСАНИЕ
    ---|---|---
    Идентификатор базы данных|sample-database|Введите имя новой базы данных, например *sample-database*. Имя базы данных может иметь длину от 1 до 255 символов и не может содержать `/ \ # ?` или пробел.
    Идентификатор графа|sample-graph|Введите имя новой коллекции, например *sample-graph*. Для имен графов предусмотрены те же требования к символам, что и для идентификаторов баз данных.
    Емкость хранилища|Фиксированный (10 ГБ)|Оставьте значение по умолчанию — **Fixed (10 GB)** (Фиксированный (10 ГБ)). Это значение представляет емкость хранилища базы данных.
    Пропускная способность|400 ЕЗ|Укажите для пропускной способности 400 единиц запросов в секунду. Чтобы сократить задержку, позже вы можете увеличить масштаб пропускной способности.

3. После заполнения формы нажмите кнопку **ОК**.
