---
title: Общие сведения об использовании OpenShift в Azure | Документация Майкрософт
description: Общие сведения об использовании OpenShift в Azure.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/7/2019
ms.author: haroldw
ms.openlocfilehash: d9e3aa3dae81166ef91f57ea6a95087a952001ed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65550987"
---
# <a name="openshift-in-azure"></a>Использование OpenShift в Azure

OpenShift — это открытая и расширяемая платформа приложений-контейнеров, которая позволяет использовать Docker и Kubernetes на предприятии.  

OpenShift включает Kubernetes для оркестрации контейнеров и управления ими. Здесь добавлены инструменты для разработчиков и операций, позволяющие:

- быстро разрабатывать приложения;
- легко развертывать и масштабировать решения;
- обслуживать жизненный цикл команд и приложений в долгосрочной перспективе.

Существует несколько версий OpenShift.  Из этих версий только два доступны сегодня клиентам самостоятельно развертывать в Azure: Платформа контейнеров OpenShift и OKD (прежнее название — OpenShift Origin).

## <a name="azure-red-hat-openshift"></a>Azure Red Hat OpenShift

Microsoft Azure Red Hat OpenShift — это полностью управляемое предложение OpenShift в Azure. Эта служба управляется и поддерживается совместно корпорацией Майкрософт и Red Hat. Дополнительные сведения см. в разделе [Azure Red Hat OpenShift службы](https://docs.microsoft.com/azure/openshift/) документации.

## <a name="openshift-container-platform"></a>Платформа контейнеров OpenShift

OpenShift Container Platform — это [коммерческая версия](https://www.openshift.com) корпоративного класса, которая разрабатывается и поддерживается компанией Red Hat. Чтобы использовать эту версию, клиенты должен приобрести необходимые права доступа к платформе OpenShift Container Platform. Кроме того, они сами устанавливают и администрируют всю инфраструктуру.

Поскольку клиенты «владел» всей платформы, их можно установить в их локальном центре обработки данных или в общедоступном облаке (Azure).

## <a name="okd"></a>OKD

OKD — это высокоуровневый проект OpenShift с [открытым кодом](https://www.okd.io/), который поддерживается сообществом. OKD можно установить в ОС CentOS или Red Hat Enterprise Linux (RHEL).

## <a name="next-steps"></a>Дальнейшие действия

- [Общие предварительные требования для OpenShift в Azure](./openshift-prerequisites.md)
- [Развертывание платформы OpenShift Container Platform](./openshift-container-platform.md)
- [Развертывание предложения самостоятельным управлением Marketplace платформы контейнеров OpenShift](./openshift-marketplace-self-managed.md)
- [Развертывание OpenShift в Azure Stack](./openshift-azure-stack.md)
- [Задачи, выполняемые после развертывания](./openshift-post-deployment.md)
- [Устранение неполадок с развертыванием OpenShift](./openshift-troubleshooting.md)
