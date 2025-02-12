---
title: С помощью общего образа коллекции в службы лабораторий Azure | Документация Майкрософт
description: Сведения о настройке учетной записи лаборатории для использования общего образа коллекции, чтобы пользователь может поделиться изображение с другими, и другой пользователь может использовать образ для создания шаблона виртуальной Машины в лаборатории.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: 8d8b6fffe197d4180b091518dcd1615d0e0b9d19
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65412852"
---
# <a name="use-a-shared-image-gallery-in-azure-lab-services"></a>С помощью общего образа коллекции в службы лабораторий Azure
В этой статье показано, как администратор преподавателей и лабораторий можно сохранить образ шаблона виртуальной машины для нее для повторного использования другими пользователями. Эти образы сохраняются в Azure [общего образа коллекции](../../virtual-machines/windows/shared-image-galleries.md). В качестве первого шага администратор лаборатории присоединяет существующей коллекцией общего образа к учетной записи лаборатории. После присоединения коллекции общих образов лабораторий, созданных в учетной записи лаборатории можно сохранить образы коллекции общих образов. Другие преподавателей можно выбрать образ из коллекции общего образа, чтобы создать шаблон для своих классов. 

## <a name="prerequisites"></a>Технические условия
- Создать галерею общего образа с помощью [Azure PowerShell](../../virtual-machines/windows/shared-images.md) или [Azure CLI](../../virtual-machines/linux/shared-images.md).
- Общего образа коллекции были присоединены к учетной записи лаборатории. Пошаговые инструкции см. в разделе [для присоединения или отсоединения коллекции образов общих](how-to-attach-detach-shared-image-gallery.md).


## <a name="save-an-image-to-the-shared-image-gallery"></a>Сохранить изображение в галерее общего образа
После присоединения коллекции общего образа преподаватель можно сохранить или передача образа в общей коллекции, таким образом, он может быть использован другими преподавателей. Инструкции для загрузки изображения в коллекцию общих образов см. в разделе [коллекции образов общий обзор](../../virtual-machines/windows/shared-images.md). 

> [!NOTE]
> В данный момент лабораторные занятия в аудитории пользовательский интерфейс (UI) не поддерживает сохранение образа виртуальной лаборатории в коллекцию общих образов. 

## <a name="use-an-image-from-the-shared-image-gallery"></a>Использование образа из коллекции общего образа
Преподаватель/профессором можно выбрать пользовательский образ доступен в коллекции образов, общих для шаблона во время создания новой лабораторной.

![Использовать образ виртуальной машины из коллекции](../media/how-to-use-shared-image-gallery/use-shared-image.png)

## <a name="next-steps"></a>Дальнейшие действия
Дополнительные сведения о коллекциях общего образа см. в разделе [общего образа коллекции](../../virtual-machines/windows/shared-image-galleries.md).
