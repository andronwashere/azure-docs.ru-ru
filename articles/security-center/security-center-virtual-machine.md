---
title: Центр безопасности Azure и виртуальные машины Azure | Документация Майкрософт
description: Этот документ содержит сведения о том, как с помощью центра безопасности Azure можно защитить виртуальные машины Azure.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: 5fe5a12c-5d25-430c-9d47-df9438b1d7c5
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/24/2017
ms.author: yurid
ms.openlocfilehash: 527ae9eb59e09885b9b606d74e72817351c31a7f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62121770"
---
# <a name="azure-security-center-and-azure-virtual-machines"></a>Центр безопасности Azure и виртуальные машины Azure
[Центр безопасности Azure](https://azure.microsoft.com/services/security-center/) позволяет предотвращать, обнаруживать угрозы и реагировать на них. Она включает встроенные функции мониторинга безопасности и управления политиками для подписок Azure, помогает выявлять угрозы, которые в противном случае могли бы оказаться незамеченными, и взаимодействует с широким комплексом решений по обеспечению безопасности.

В этой статье показано, как с помощью центра безопасности можно защитить виртуальные машины Azure.

## <a name="why-use-security-center"></a>Для чего используется центр безопасности?
Центр безопасности помогает защитить данные виртуальных машин в Azure, обеспечивая представление параметров безопасности. Защита виртуальных машин с помощью центра безопасности предоставляет следующие возможности:

* Соответствие параметров безопасности операционной системы (ОС) рекомендуемым правилам конфигурации.
* Своевременная установка отсутствующих обновлений системы безопасности и критических обновлений.
* Рекомендации по защите конечных точек.
* Проверка шифрования диска.
* Оценка уязвимостей и их устранение.
* Обнаружение угроз

Помимо защиты виртуальных машин Azure центр безопасности предоставляет средства мониторинга и управления безопасностью облачных служб, служб приложений, виртуальных сетей, а также многие другие возможности. 

> [!NOTE]
> Дополнительные сведения о центре безопасности Azure см. в статье [Введение в центр безопасности Azure](security-center-intro.md).
> 
> 

## <a name="prerequisites"></a>Технические условия
Чтобы начать работу с центром безопасности Azure, необходимо принять во внимание следующее.

* Вам необходимо иметь подписку Microsoft Azure. Дополнительные сведения об уровнях "Бесплатный" и "Стандартный" центра безопасности на странице [цен на центр безопасности](https://azure.microsoft.com/pricing/details/security-center/).
* Дополнительные сведения и рекомендации по планированию использования центра безопасности Azure и работе в нем см. в [этом руководстве](security-center-planning-and-operations-guide.md).
* Сведения о поддержке операционных систем см. в статье [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md). 

## <a name="set-security-policy"></a>Установка политики безопасности
Чтобы центр безопасности Azure мог собирать сведения, необходимые для предоставления рекомендаций и оповещений, которые создаются на основе настроек политики безопасности, нужно включить сбор данных. На рисунке ниже видно, что для параметра **Сбор данных** выбрано значение **Вкл**.

Политика безопасности определяет набор элементов управления, которые рекомендуются для ресурсов в указанной подписке или группе ресурсов. Сбор данных необходимо включить перед включением политики безопасности. Центр безопасности собирает с виртуальных машин данные, оценивает состояние их безопасности на основе этих данных, предоставляет рекомендации по обеспечению безопасности и предупреждает об угрозах. В центре безопасности можно настраивать политики для подписок Azure или групп ресурсов в соответствии с потребностями безопасности вашей компании, типом приложений или конфиденциальностью данных в каждой подписке. 

![Политика безопасности](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

> [!NOTE]
> Дополнительные сведения о каждой доступной **политике предотвращения** см. в статье [Настройка политик безопасности в центре безопасности Azure](tutorial-security-policy.md).
> 
> 

## <a name="manage-security-recommendations"></a>Управление рекомендациями по обеспечению безопасности
Центр безопасности анализирует состояние безопасности ваших ресурсов Azure. Когда Центр безопасности выявляет потенциальные уязвимости системы безопасности, он создает рекомендации. Рекомендации помогают настраивать необходимые элементы управления.

После настройки политики безопасности Центр безопасности анализирует состояние безопасности ресурсов, чтобы выявить потенциальные уязвимости. Рекомендации отображаются в табличном формате, где каждая строка представляет одну конкретную рекомендацию. В таблице ниже приведены некоторые рекомендации для виртуальных машин Azure и их описание. После выбора рекомендации отобразятся сведения о том, как реализовать эту рекомендацию в центре безопасности.

| Рекомендации | Описание |
| --- | --- |
| [Включение сбора данных для подписок](security-center-enable-data-collection.md) |Рекомендует включить в политике безопасности сбор данных для каждой вашей подписки и всех виртуальных машин (ВМ) в ваших подписках. |
| [Исправление уязвимостей ОС](security-center-remediate-os-vulnerabilities.md) |Рекомендует обеспечить соответствие конфигураций ОС рекомендуемым правилам конфигурации, например запретить сохранение паролей. |
| [Применение обновлений системы](security-center-apply-system-updates.md) |Рекомендует выполнить развертывание отсутствующих обновлений системы безопасности и критических обновлений на виртуальных машинах. |
| [Перезагрузка после завершения обновлений системы](security-center-apply-system-updates.md#reboot-after-system-updates) |Рекомендует перезагрузить виртуальную машину, чтобы завершить применение обновлений системы. |
| [Установка Endpoint Protection](security-center-install-endpoint-protection.md) |Рекомендует подготовить антивредоносные программы на виртуальных машинах (только для виртуальных машин Windows). |
| [Включение агента виртуальной машины](security-center-enable-vm-agent.md) |Позволяет узнать, на какие виртуальные машины необходимо установить агент ВМ. На виртуальные машины необходимо установить агент виртуальной машины, чтобы проверить наличие необходимых исправлений, базовые показатели и подготовить антивредоносные программы. Агент виртуальной машины устанавливается по умолчанию на виртуальных машинах, развернутых из Azure Marketplace. В статье [Агент виртуальной машины и расширения. Часть 2](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) содержатся сведения об установке агента ВМ. |
| [Применение шифрования дисков](security-center-apply-disk-encryption.md) |Рекомендует выполнить шифрование дисков виртуальной машины с помощью шифрования дисков Azure (виртуальных машин Windows и Linux). Шифровать рекомендуется как том операционной системы, так и том данных на виртуальной машине. |
| [Оценка уязвимостей не установлена](security-center-vulnerability-assessment-recommendations.md) |Рекомендует установить решение для оценки уязвимостей на виртуальную машину. |
| [Исправление уязвимостей](security-center-vulnerability-assessment-recommendations.md#review-the-recommendation) |Позволяет просматривать уязвимости системы и приложений, обнаруженные решением для оценки уязвимостей, установленным на виртуальной машине. |

> [!NOTE]
> Дополнительные сведения о рекомендациях см. в статье [Управление рекомендациями по безопасности в центре безопасности Azure](security-center-recommendations.md).
> 
> 

## <a name="monitor-security-health"></a>Отслеживание состояния системы безопасности
Когда вы включаете [политики безопасности](tutorial-security-policy.md) для ресурсов в рамках подписки, центр безопасности проверяет состояние защиты ресурсов, чтобы определить потенциальные уязвимости.  Сведения о состоянии безопасности ваших ресурсов, а также возникшие проблемы можно просмотреть в колонке **Работоспособность безопасности ресурсов**. Если щелкнуть **Виртуальные машины** на плитке **Работоспособность безопасности ресурсов**, откроется колонка **Виртуальные машины** с рекомендациями для ваших виртуальных машин. 

![Работоспособность системы безопасности](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Управление оповещениями системы безопасности и реагирование на них
Центр безопасности автоматически собирает, анализирует и объединяет данные журналов, поступающие от ресурсов Azure, сети и подключенных решений партнеров, таких как брандмауэры и решения для защиты конечных точек, для выявления реальных угроз и сокращения ложных срабатываний. Благодаря использованию различных сочетаний [возможностей обнаружения](security-center-detection-capabilities.md) центр безопасности может создавать приоритетные оповещения системы безопасности, позволяющие быстро проанализировать проблему, а также предоставлять рекомендации по устранению атаки.

![Оповещения безопасности](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Выберите оповещение системы безопасности, чтобы получить дополнительные сведения о событиях, вызвавших оповещение, и (при необходимости) действиях, которые следует предпринять для устранения атаки. Оповещения системы безопасности группируются по [типу](security-center-alerts-type.md) и дате.

## <a name="see-also"></a>См. также
Дополнительные сведения о Центре безопасности см. в следующих статьях:

* [Настройка политик безопасности в Центре безопасности Azure](tutorial-security-policy.md) — узнайте, как настроить политики безопасности для подписок и групп ресурсов Azure.
* [Управление оповещениями безопасности в Центре безопасности Azure и реагирование на них](security-center-managing-and-responding-alerts.md) — узнайте, как управлять оповещениями системы безопасности и реагировать на них.
* [Центр безопасности Azure: часто задаваемые вопросы](security-center-faq.md) — часто задаваемые вопросы об использовании этой службы.

