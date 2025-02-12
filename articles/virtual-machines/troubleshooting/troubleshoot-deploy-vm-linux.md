---
title: Устранение неполадок при развертывании виртуальных машин Linux в Azure | Документация Майкрософт
description: Linux виртуальной машины Устранение неполадок при развертывании в модели развертывания Azure Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 38d53af06312c993a749fa86619e25e60d39fff4
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67710478"
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a>Устранение неполадок при развертывании виртуальных машин Linux в Azure

Для устранения неполадок, связанных с развертыванием виртуальных машин в Azure, ознакомьтесь с разделом [Наиболее важные проблемы](#top-issues), где описаны распространенные ошибки и способы их разрешения.

Если в любой момент при изучении этой статьи вам потребуется дополнительная помощь, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack Overflow](https://azure.microsoft.com/support/forums/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и щелкните **Получить поддержку**.

## <a name="top-issues"></a>Наиболее важные проблемы
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a>Кластер не поддерживает запрошенный размер виртуальной машины
\<properties supportTopicIds="123456789" resourceTags="windows" productPesIds="1234, 5678" />
- Повторите запрос с указанием меньшего размера виртуальной машины.
- Если нельзя изменить размер запрошенной виртуальной машины,
    - остановите все виртуальные машины в группе доступности. Выберите **Группы ресурсов** > имя вашей группы ресурсов > **Ресурсы** > имя вашей группы доступности > **Виртуальные машины** > имя вашей виртуальной машины > **Остановить**.
    - После остановки всех виртуальных машин создайте виртуальную машину необходимого размера.
    - Сначала запустите новую виртуальную машину, а затем выберите каждую из остановленных виртуальных машин и нажмите кнопку "Запустить".


## <a name="the-cluster-does-not-have-free-resources"></a>В кластере нет свободных ресурсов
\<properties supportTopicIds="123456789" resourceTags="windows" productPesIds="1234, 5678" />
- Повторите запрос позже.
- Если новая виртуальная машина должна быть частью другой группы доступности:
    - создайте виртуальную машину в другой группе доступности (в том же регионе);
    - добавьте новую виртуальную машину в ту же виртуальную сеть.

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Как активировать мой ежемесячный кредит для Visual Studio Enterprise (BizSpark)?

Чтобы активировать ежемесячный кредит, см. эту [статью](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="why-can-i-not-install-the-gpu-driver-for-an-ubuntu-nv-vm"></a>Почему не удается установить драйвер графического процессора (GPU) для виртуальной машины Ubuntu NV?

В настоящее время поддержка Linux GPU доступна только на виртуальных машинах Azure серии NC под управлением Ubuntu Server 16.04 LTS. Дополнительные сведения см. в статье [Установка драйверов GPU для виртуальных машин серии N под управлением Linux](../linux/n-series-driver-setup.md).

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a>Для моей виртуальной машины Linux серии N отсутствуют драйверы

Драйверы для виртуальных машин под управлением Linux доступны [здесь](../linux/n-series-driver-setup.md). 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>Не удается найти экземпляр графического процессора (GPU) на виртуальной машине серии N

Чтобы воспользоваться преимуществами возможностей GPU виртуальных машин Azure серии N под управлением Windows Server 2016 или Windows Server 2012 R2, необходимо установить графические драйверы NVIDIA на каждую виртуальную машину после развертывания. Сведения об установке драйверов доступны для [виртуальных машин Windows](../windows/n-series-driver-setup.md) и [виртуальных машин Linux](../linux/n-series-driver-setup.md).

## <a name="is-n-series-vms-available-in-my-region"></a>Доступны ли виртуальные машины серии N в моем регионе?

Доступность можно проверить в [таблице "Доступность продуктов по регионам"](https://azure.microsoft.com/regions/services), а цену можно узнать [здесь](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>При изменении размера виртуальной машины не отображается необходимое семейство размеров виртуальных машин

Когда виртуальная машина запущена, она развертывается на физический сервер. Физические серверы в регионах Azure группируются в кластеры общего физического оборудования. Изменение размера виртуальной машины, для которого требуется переместить виртуальную машину в другой кластер оборудования, отличается в зависимости от того, какая модель развертывания использовалась для развертывания данной виртуальной машины.

- На виртуальных машинах, развернутых с помощью классической модели, необходимо удалить развертывание облачной службы и развернуть ее повторно, выбрав для виртуальных машин размер из другого семейства размеров.

- На виртуальных машинах, развернутых с помощью модели Resource Manager, необходимо остановить все виртуальные машины в группе доступности, прежде чем изменять размер любой виртуальной машины в этой группе доступности.

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>Указанный размер виртуальной машины не поддерживается при развертывании в группе доступности

Выберите размер, который поддерживается в кластере группы доступности. При создании группы доступности рекомендуется выбрать самый большой размер виртуальной машины, который может понадобиться, и использовать его в качестве первого развертывания в этой группе доступности.

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a>Какие дистрибутивы или версии Linux поддерживаются в Azure?

Список для Linux приводится в статье [Linux в Azure — рекомендованные дистрибутивы](../linux/endorsed-distros.md).

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>Можно ли добавить существующую классическую виртуальную машину в группу доступности?

Да. Существующую классическую виртуальную машину можно добавить в новую или существующую группу доступности. Дополнительные сведения см. в разделе [Добавление существующей виртуальной машины к группе доступности](/previous-versions/azure/virtual-machines/windows/classic/configure-availability-classic#addmachine).


## <a name="next-steps"></a>Следующие шаги
Если в любой момент при изучении этой статьи вам потребуется дополнительная помощь, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack Overflow](https://azure.microsoft.com/support/forums/).

Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и щелкните **Получить поддержку**.
