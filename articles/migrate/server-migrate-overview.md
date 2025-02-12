---
title: Выберите вариант миграции VMware с миграцией сервера миграции Azure | Документация Майкрософт
description: Предоставляет обзор возможностей переноса виртуальных машин VMware в Azure с помощью миграции сервера миграции Azure
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/09/2019
ms.author: raynew
ms.openlocfilehash: f8bfbe26dc4c6ddbcf622f91938ba060de00b2ec
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811574"
---
# <a name="select-a-vmware-migration-option"></a>Выберите вариант миграции VMware

Можно перенести виртуальные машины VMware в Azure с помощью средства миграции сервера миграции Azure. Это средство предлагает несколько вариантов для миграции виртуальной Машины VMware:

- Миграция с помощью агента репликации. Перенос виртуальных машин не нужно ничего устанавливать на них.
- Миграция с агентом репликации. Установите агент на виртуальной Машине для репликации.


Несмотря на то, что с точки зрения развертывания проще без агента репликации, в настоящее время он имеет ряд ограничений.

## <a name="agentless-migration-limitations"></a>Ограничения миграции без агента

Ниже приведены ограничения.

- **Одновременные репликации**: Более 50 виртуальных машин могут одновременно реплицированы из vCenter Server.<br/> При наличии более 50 виртуальных машин для миграции, создайте несколько пакетов виртуальных машин.<br/> Более репликации за один раз повлияет на производительность.
- **Диски виртуальной Машины**: Необходимо выполнить миграцию виртуальной Машины должно быть 60 или менее дисков.
- **Операционные системы для виртуальной Машины**: Как правило любой Windows Server или операционной системы Linux "Миграция Azure" можно перенести, но может потребоваться изменения в виртуальных машинах, чтобы их можно запустить в Azure. Служба "Миграция Azure" вносит изменения автоматически для этих операционных систем:
    - Red Hat Enterprise Linux 6.5 +, 7.0 и более поздних
    - CentOS 7.0 и более 6.5 +
    - SUSE Linux Enterprise Server 12 SP1+
    - Ubuntu 14.04LTS, 16.04LTS, 18.04LTS
    - Debian дефектах 7,8.
    - Для других операционных систем необходимо внести изменения вручную, прежде чем выполнять миграцию. [Перенести руководстве](tutorial-migrate-vmware.md) объясняет, как это сделать.
- **Загрузки Linux**: Если на выделенный раздел/Boot, он должен находиться на диске операционной системы и не быть распределены между несколькими дисками.<br/> Если каталог/Boot является частью секции корневой (/), затем «/» раздел должен находиться на диске операционной системы и охватывает другие диски.
- **Загрузка UEFI**: Виртуальные машины с загрузкой UEFI не поддерживаются для миграции.
- **Шифрование дисков и томов (с помощью BitLocker, cryptfs)** : Виртуальные машины с помощью зашифрованных дисков или томов не поддерживаются для миграции.
- **RDM/транзитные диски**: Если виртуальные машины имеют RDM или транзитных дисков, эти диски не будут реплицироваться в Azure
- **NFS**: Тома NFS, подключенного в качестве тома на виртуальных машинах не будут реплицироваться.
- **Целевое хранилище**: Только перенос виртуальных машин VMware с виртуальными машинами Azure с управляемыми дисками (стандартная HDD, SSD уровня "премиум").



## <a name="deployment-steps-comparison"></a>Сравнение шаги развертывания

Ознакомьтесь с ограничениями, понимание действий, необходимых для развертывания каждого решения помогут вам выбрать оптимальный вариант.

**Задача** | **Сведения** |**Без агента** | **На основе агента**
--- | --- | --- | ---
**Подготовка виртуальных машин и серверы VMware к миграции** | Настройте ряд параметров на серверы VMware и виртуальные машины. | Обязательно для заполнения | Обязательно для заполнения
**Добавление средства миграции сервера** | Средство миграции сервера миграции Azure добавьте в проект "Миграция Azure". | Обязательно для заполнения | Обязательно для заполнения
**Развертывание устройства "Миграция Azure"** | Настройка простой модуль на виртуальной Машине VMware для обнаружения виртуальных Машин и оценки. | Обязательно для заполнения | Необязательно.
**Установка службы Mobility service на виртуальных машинах** | Установка службы Mobility service на каждой виртуальной Машины, которые нужно реплицировать | Не требуется | Обязательно для заполнения
**Развернуть модуль репликации миграция сервера миграции Azure** | Настройка устройства на виртуальной Машине VMware для обнаружения виртуальных машин и создание связи между службы мобильности, запущенной на виртуальных машинах и миграция сервера миграции Azure | Не требуется | Обязательно для заполнения
**Репликация виртуальных машин**. Включение репликации виртуальных Машин. | Настройка параметров репликации и выберите виртуальные машины для репликации | Обязательно для заполнения | Обязательно для заполнения
**Запуск тестовой миграции** | Запуск тестовой миграции, чтобы убедиться в том, что все работает должным образом. | Обязательно для заполнения | Обязательно для заполнения
**Выполнение полной миграции** | Миграция виртуальных машин. | Обязательно для заполнения | Обязательно для заполнения




## <a name="next-steps"></a>Следующие шаги

[Перенос виртуальных машин VMware](tutorial-migrate-vmware.md) с помощью миграцию без агента.



