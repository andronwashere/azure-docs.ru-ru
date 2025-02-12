---
title: Приложения для рендеринга — пакетная служба Azure
description: Предварительно установленные приложения пакетной службы для рендеринга
services: batch
ms.service: batch
author: laurenhughes
ms.author: lahugh
ms.date: 03/26/2018
ms.topic: conceptual
ms.openlocfilehash: 84b2ca30f1ccd49e18e2f9d42133f8672d3f8ad6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60776175"
---
# <a name="pre-installed-applications-on-rendering-vm-images"></a>Предварительно установленные приложения в образах виртуальных машин для рендеринга

Вы можете использовать в пакетной службе Azure любые приложения для рендеринга Но некоторые распространенные версии уже включены в образы виртуальных машин Azure Marketplace.

Везде, где это возможно, вам будет доступно лицензирование с оплатой по мере использования для всех предустановленных приложений для рендеринга. При создании пула пакетной службы вы можете указать необходимые приложения, и далее поминутно платить за использование виртуальных машин и приложений. Цены на приложения цены указаны на [странице с ценами на пакетную службу Azure](https://azure.microsoft.com/pricing/details/batch/#graphic-rendering).

Некоторые приложения поддерживают только платформу Windows, но большинство из них доступны как в Windows, так и в Linux.

## <a name="applications-on-centos-7-rendering-images"></a>Приложения на CentOS 7 отрисовка изображений

* Autodesk Maya I/O 2017 обновление 5 (версия сборки: 201708032230);
* Autodesk Maya I/O 2018 обновление 2 (версия сборки: 201711281015);
* Autodesk Arnold for Maya 2017 (Arnold версии 5.0.1.1) MtoA-2.0.1.1-2017;
* Autodesk Arnold for Maya 2018 (Arnold версии 5.0.1.4) MtoA-2.1.0.3-2018;
* Chaos Group V-Ray for Maya 2017 (версия 3.60.04);
* Chaos Group V-Ray for Maya 2018 (версия 3.60.04);
* Blender (2.68).

## <a name="applications-on-latest-windows-server-2016-rendering-images"></a>Приложения на последних отрисовка изображений Windows Server 2016

Следующий список относится к Windows Server 2016, версия 1.3.4 отрисовка изображений.

* Autodesk Maya I/O 2017 обновление 5 (версия 17.4.5459);
* Autodesk Maya I/O 2018 обновление 4 (версия 18.4.0.7622);
* Autodesk 3ds Max I/O 2019 обновление 1 (версия 21.2.0.2219);
* Autodesk 3ds Max I/O 2018 обновление 4 (версия 20.4.0.4254);
* Autodesk Arnold for Maya 2017 (Arnold версии 5.2.0.1) MtoA-3.1.0.1-2017;
* Autodesk Arnold for Maya 2018 (Arnold версии 5.2.0.1) MtoA-3.1.0.1-2018;
* Autodesk Arnold for 3ds Max (Arnold версии 5.0.2.4) (версия 1.2.926);
* Chaos Group V-Ray для Maya 2018 г. (версия 3.52.03)
* Chaos Group V-Ray для 3ds Max 2018; (версия 3.60.02)
* Chaos Group V-Ray для Maya 2019 г. (версия 3.52.03)
* Chaos Group V-Ray для 3ds Max 2019 (версия 4.10.01)
* Blender (2.79).

> [!NOTE]
> Chaos Group V-Ray для 3ds Max 2019 (версия 4.10.01) представлены критические изменения V-ray. Чтобы использовать предыдущей версии (версия 3.60.02), используйте Windows Server 2016, версия 1.3.2 отрисовки узлов.

## <a name="applications-on-previous-windows-server-2016-rendering-images"></a>Приложения на предыдущие образы Windows Server 2016 отрисовки

Следующий список относится к Windows Server 2016, версия 1.3.2 отрисовка изображений.

* Autodesk Maya I/O 2017 обновление 5 (версия 17.4.5459);
* Autodesk Maya I/O 2018 обновление 4 (версия 18.4.0.7622);  
* Autodesk 3ds Max I/O 2019 обновление 1 (версия 21.2.0.2219);
* Autodesk 3ds Max I/O 2018 обновление 4 (версия 20.4.0.4254);
* Autodesk Arnold for Maya 2017 (Arnold версии 5.2.0.1) MtoA-3.1.0.1-2017;
* Autodesk Arnold for Maya 2018 (Arnold версии 5.2.0.1) MtoA-3.1.0.1-2018;
* Autodesk Arnold for 3ds Max (Arnold версии 5.0.2.4) (версия 1.2.926);
* Chaos Group V-Ray для Maya 2019 г. (версия 3.52.03)
* Chaos Group V-Ray для 3ds Max 2018; (версия 3.60.02)
* Blender (2.79).

## <a name="next-steps"></a>Дальнейшие действия

Чтобы использовать образы виртуальных машин для рендеринга, их необходимо указывать в конфигурации пула при создании пула. Подробнее см. статью о [возможностях рендеринга в пакетной службе](https://docs.microsoft.com/azure/batch/batch-rendering-functionality#batch-pools).