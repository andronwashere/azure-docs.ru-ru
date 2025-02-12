---
title: Новые возможности службы "Миграция Azure" | Документация Майкрософт
description: Обзор службы "Миграция Azure".
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 06/10/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 2c3bc596076f3ec4f9d41f0da819ddd386fee63c
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811048"
---
# <a name="whats-new-in-azure-migrate"></a>Новые возможности в службе "Миграция Azure"

С помощью [службы "Миграция Azure"](migrate-services-overview.md) можно обнаруживать, оценивать и переносить серверы, приложения и данные в облако Microsoft Azure. В этой статье описаны новые функции в службе "Миграция Azure".



## <a name="azure-migrate-new-version"></a>Новая версия службы "Миграция Azure"

Новую версию службы "Миграция Azure" было выпущено в июле 2019 г. 

- **Текущая (новая) версия**: С помощью этой версии службы можно создавать проекты службы "Миграция Azure", обнаруживать локальные компьютеры и выполнять оркестрацию оценок и миграций. 
- **Предыдущая версия**. Теперь пользователям, которые используют предыдущую версии службы "Миграция Azure" (где поддерживалась только оценка локальных виртуальных машин VMware), необходимо использовать текущую версию. В предыдущей версии вы больше не можете создавать новые проекты службы "Миграция Azure" или выполнять новые обнаружения. Вы по-прежнему можете получить доступ к существующим проектам. Перейдите на портал Azure > Все службы, найдите службу "Миграция Azure". В уведомлении службы "Миграция Azure" есть уведомление и ссылка для доступа к старым проектам службы.


## <a name="azure-migrate-features"></a>Миграция Azure. Функции

Новая версия службы "Миграция Azure" предоставляет ряд новых функций.


- **Единая платформа для миграции**: Теперь служба "Миграция Azure" предоставляет единый портал для централизации, управления и отслеживания процесса миграции в Azure с улучшенным потоком развертывания и возможностями портала.
- **Инструменты оценки и миграции**. Служба "Миграция Azure" предоставляет собственные инструменты и интегрируется с другими службами Azure, а также с инструментами независимого поставщика программного обеспечения (ISV). [Дополнительные сведения](migrate-services-overview.md#isv-integration) об интеграции ISV.
- **Оценка службы "Миграция Azure"** . С помощью инструмента оценки сервера службы "Миграция Azure" можно оценивать виртуальные машины VMware и Hyper-V на предмет миграции в Azure. Также можно оценить миграцию, используя другие службы Azure, а также инструменты ISV.
- **Миграция службы "Миграция Azure"** : С помощью средства миграции сервера службы "Миграция Azure" можно перенести локальные виртуальные машины VMware и Hyper-V в Azure, а также физические серверы, другие виртуализированные серверы и виртуальные частные/общедоступные облачные виртуальные машины. Кроме того, вы можете перенести их в Azure с помощью инструментов ISV.
- **Устройство службы "Миграция Azure"** : Служба "Миграция Azure" развертывает упрощенное устройство для обнаружения и оценки локальных виртуальных машин VMware и Hyper-V.
    - Это устройство используется при оценивании и миграции сервера службы "Миграция Azure" без агентов.
    - Метаданные сервера и данные производительности в рамках оценки и миграции можно обнаруживать с помощью непрерывной работы устройства.  
- **Миграция виртуальной машины VMware**:  Миграция сервера службы "Миграция Azure" предоставляет несколько методов для миграции локальных виртуальных машин VMware в Azure.  Миграция без агента с использованием устройства службы "Миграция Azure" и миграция с агентом, при которой используется устройство репликации и развертывается агент в каждой виртуальной машине, которую требуется перенести. [Подробнее](server-migrate-overview.md)
 - **Оценка и миграция базы данных**. С помощью службы "Миграция Azure" можно оценить локальные базы данных для миграции в Azure, используя помощника по миграции баз данных Azure. Базы данных можно перенести с помощью сервера миграции базы данных Azure.
- **Перенос веб-приложений**. Вы можете оценивать веб-приложения, используя общедоступный URL-адрес конечной точки в службе приложений Azure. Для переноса внутренних приложений .NET можно загрузить и запустить помощника по миграции службы приложений. 
- **Data Box**. Импортируйте большие объемы автономных данных в Azure с помощью Azure Data Box в службе "Миграция Azure".


## <a name="next-steps"></a>Дополнительная информация

- [Дополнительные сведения](https://azure.microsoft.com/pricing/details/azure-migrate/) о ценах на службу "Миграция Azure".
- [Просмотрите часто задаваемые вопросы](resources-faq.md) о службе "Миграция Azure".
- Перейдите к другим руководствам, чтобы оценить [виртуальные машины VMware](tutorial-assess-vmware.md) и [виртуальные машины Hyper-V](tutorial-assess-hyper-v.md).
