---
title: включение файла
description: включение файла
services: virtual-machines
author: vermagit
ms.service: virtual-machines
ms.topic: include
ms.date: 05/29/2018
ms.author: azcspmt;jonbeck;cynthn;danlep;amverma
ms.custom: include file
ms.openlocfilehash: 88d9491bae1fcc6acb7ab631514aa4f6244f593d
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538015"
---
## <a name="deployment-considerations"></a>Рекомендации по развертыванию
* **Подписка Azure.** Чтобы развернуть большое число экземпляров для ресурсоемких вычислений, рекомендуем подписку с оплатой по мере использования или другие варианты покупки. Если вы используете [бесплатную учетную запись Azure](https://azure.microsoft.com/free/), вам доступно ограниченное количество вычислительных ядер Azure.

* **Цены и доступность.** Виртуальные машины предлагаются только в ценовой категории уровня "Стандартный". Проверьте [доступность продуктов по регионам](https://azure.microsoft.com/global-infrastructure/services/) , чтобы узнать, в каких регионах Azure их можно использовать. 
* **Квота ядер**. Вам может потребоваться увеличить стандартную квоту на число ядер в подписке Azure. Кроме того, количество ядер, которые можно развернуть для некоторых семейств размеров виртуальных машин (включая серию H), может быть ограничено условиями вашей подписки. Чтобы увеличить квоту, [отправьте запрос в службу поддержки](../articles/azure-supportability/how-to-create-azure-support-request.md). Это бесплатная услуга. (Ограничения по умолчанию могут быть разными в зависимости от категории подписки).
  
  > [!NOTE]
  > Если вам нужны ресурсы в очень большом объеме, обратитесь в службу поддержки Azure. Квоты Azure — это ограничения по кредитам, а не гарантированная емкость. Вне зависимости от квоты с вас будет взиматься плата только за используемые ядра.
  > 
  > 
* **Виртуальная сеть** — [виртуальная сеть](https://azure.microsoft.com/documentation/services/virtual-network/) Azure не требуется для использования ресурсоемких экземпляров. Но для нескольких развертываний вам потребуется по крайней мере облачная виртуальная сеть Azure или подключение типа "сеть — сеть", если нужен доступ к локальным ресурсам. При необходимости создайте виртуальную сеть, чтобы развернуть экземпляры. Добавление виртуальных машин для ресурсоемких вычислений в виртуальную сеть в территориальной группе не поддерживается.
* **Изменение размера.** Из-за специализированного оборудования изменить можно только размер экземпляров для ресурсоемких вычислений одного семейства размеров (серии H или серии A для ресурсоемких вычислений). Например, можно изменить только размер виртуальной машины серии H (один размер из серии H на другой размер из этой же серии). Кроме того, не поддерживается изменение размера экземпляра, не предназначенного для ресурсоемких вычислений, на размер экземпляра, предназначенного для таких вычислений.  

## <a name="rdma-capable-instances"></a>Экземпляры с поддержкой RDMA
Подмножество экземпляров вычислений (A8, A9, H16r, H16mr, HB и HC) компонентов сетевого интерфейса для подключений удаленного памяти (RDMA). Выбранные размеры серии N, назначенный символом «r», например в конфигурациях с NC24rs (NC24rs_v2 и NC24rs_v3) также являются с поддержкой RDMA. Этот интерфейс является дополнением к стандартному сетевому интерфейсу Azure, который доступен для виртуальных машин других размеров. 
  
Этот интерфейс обеспечивает связь экземпляров с поддержкой RDMA сети InfiniBand (IB), рабочая тарифам EDR HB, гибридного подключения, FDR скорости для виртуальных машин H16r, H16mr и серии N с поддержкой RDMA и QDR для виртуальных машин A8 и A9. Эти возможности RDMA позволяют увеличить масштабируемость и производительность определенных приложений с интерфейсом MPI. Дополнительные сведения о скорости см. в разделе сведений в таблицах на этой странице.

> [!NOTE]
> В Azure IP через IB поддерживается только для виртуальных машин (SR-IOV для InfiniBand, в настоящее время HB и HC) с поддержкой SR-IOV. RDMA через IB поддерживается для всех экземпляров с поддержкой RDMA.
>

