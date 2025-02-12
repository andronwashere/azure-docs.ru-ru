---
title: Обзор службы Cloudyn в Azure | Документы Майкрософт
description: Cloudyn — это решение по управлению затратами на несколько облачных служб, которое помогает в работе с Azure и другими облачными ресурсами.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 05/14/2019
ms.topic: overview
ms.service: cost-management
manager: benshy
ms.custom: seodec18
ms.openlocfilehash: 8b989e5cf5b66d21c19d58f2f64fbba1927f5d69
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/16/2019
ms.locfileid: "65792803"
---
# <a name="what-is-the-cloudyn-service"></a>Общие сведения о службе Cloudyn

Служба Cloudyn, дочернего подразделения Майкрософт, позволяет отслеживать использование облака и затраты на ресурсы Azure и других поставщиков облачных услуг, включая AWS и Google. Простые для понимания отчеты панели мониторинга помогают с распределением затрат, а также с виртуальными счетами и возвратными платежами. Cloudyn помогает оптимизировать расходы на облако, выявляя недостаточно нагруженные ресурсы, которые затем можно настроить.

См. ознакомительное видео [Общие сведения о службе Cloudyn](https://azure.microsoft.com/resources/videos/azure-cost-management-overview-and-demo).

Служба "Управление затратами Azure" предоставляет аналогичные возможности в Cloudyn. Служба "Управление затратами Azure" является собственным решением Azure по управлению затратами. Она позволяет анализировать затраты, создавать бюджеты и управлять ими, экспортировать данные, а также просматривать рекомендации и реагировать на них с целью экономии средств. Дополнительные сведения см. в статье [Управление затратами Azure](overview-cost-mgt.md).

См. подробнее о выборе соответствующего варианта в [видео об Управлении затратами Azure и Cloudyn](https://www.youtube.com/watch?v=PmwFWwSluh8).

>[!VIDEO https://www.youtube.com/embed/PmwFWwSluh8]

## <a name="cloudyn-features-moving-to-azure-cost-management"></a>Перенос функций Cloudyn в службу "Управление затратами Azure"

Корпорация Майкрософт приобрела службу Cloudyn и переносит ее функции управления затратами с портала Cloudyn в Azure. Чтобы использовать новые функции, войдите на портал Azure и выберите службу [управления затратами и выставлением счетов](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) Azure со списка. По сравнению с Cloudyn привычный интерфейс обеспечивает улучшенную производительность и меньшую задержку данных (примерно восемь часов).

Перенос основных функций, предлагаемых в рамках Соглашения Enterprise, подписки MSDN или подписки с оплатой по мере использования, в службу "Управление затратами Azure" завершено. Перенос подписок CSP в эту службу выполняется.

Если вы имеете категорию предложения, перенос которого еще не завершено, продолжайте использовать портал Cloudyn. Все остальные могут использовать службу "Управления затратами Azure".

| Предложения и функции Microsoft Azure | Рекомендуемая служба управления затратами |
| --- | --- |
| Соглашение Azure Enterprise | [Управление затратами Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure Web Direct (PAYG/MSDN) | [Управление затратами Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure для государственных организаций | [Управление затратами Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Azure CSP | [Cloudyn](https://azure.cloudyn.com); |
| Поддержка анализа затрат в нескольких облаках для AWS (предварительная версия) | [Управление затратами Azure](https://ms.portal.azure.com/#blade/Microsoft_Azure_CostManagement/Menu/overview) |
| Рекомендации AWS | [Cloudyn](https://azure.cloudyn.com); |

Некоторые из приведенных ниже функций доступны в Cloudyn, но теперь все они доступны в службе "Управление затратами Azure".

- API
- Рекомендации по вычислениям Azure
- Рекомендации по резервированию Azure
- сведения о бюджете;
- Анализ затрат
- Экспорт данных в учетную запись хранения Azure
- Низкая задержка
- Соединитель и пакет содержимого Power BI
- Поддержка тегов ресурсов

## <a name="monitor-usage-and-spending"></a>Отслеживание использования и расходов

Отслеживание использования и расходов крайне важно для облачных инфраструктур, так как организации платят за ресурсы, использованные за определенный период времени. В случае превышения пороговых значений, определенных в соглашении, могут быстро возникнуть непредвиденные расходы. Несколько важных факторов могут затруднить ad-hoc-мониторинг. Во-первых, проектирование затрат на основе среднего объема использования предполагает, что потребление остается стабильным в рамках заданного расчетного периода. Во-вторых, когда затраты приближаются к вашему бюджету или превышают его, важно получать упреждающие уведомления, чтобы внести корректировки в свои расходы. Кроме того, поставщики облачных служб могут не предлагать проекцию затрат в зависимости от пороговых значений или сравнительные отчеты по периодам.

С помощью отчетов можно выполнять мониторинг расходов для анализа и отслеживания данных использования ресурсов облака, затрат и тенденций. С помощью отчетов за период времени можно выявлять аномалии, отличающиеся от обычных тенденций. Неэффективность облачного развертывания отображается в отчетах по оптимизации. Неэффективность также можно заметить в отчетах по анализу затрат.

## <a name="manage-costs"></a>Управление затратами

Исторические данные могут быть полезными при управлении затратами, когда анализируются сведения об использовании и затратах за период время для определения тенденций. Тенденции затем используются для прогнозирования будущих затрат. Cloudyn также содержит полезные отчеты с плановыми затратами.

Распределение затрат управляет затратами, анализируя ваши расходы на основе политики расстановки тегов. Для уточнения распределения затрат в пользовательских учетных записях, ресурсах и сущностях можно использовать теги. Диспетчер категорий организует теги для предоставления дополнительных средств управления. Распределение затрат также используется для виртуальных счетов и возвратных платежей, чтобы отобразить потребление ресурсов и сопутствующие расходы. Это позволит повлиять на уровень потребления или выставить счета пользователям клиентов.

Управление доступом помогает управлять затратами, обеспечивая, чтобы пользователи и группы имели доступ только к тем данным службы управления затратами, которые им нужны. Вы можете воспользоваться структурой сущности, управлением пользователями и запланированными отчетами со списками получателей для назначения доступа.

Функция оповещений помогает управлять затратами, автоматически уведомляя о случаях нетипичных или чрезмерных затрат. Оповещения также могут автоматически уведомлять других заинтересованных лиц о рисках, связанных с нетипичными и чрезмерными затратами. Различные отчеты поддерживают оповещения на основе пороговых значений бюджета и затрат. Но предупреждения не поддерживаются для учетных записей и подписок партнера CSP.

## <a name="improve-efficiency"></a>Повышение эффективности

С помощью Cloudyn можно определить оптимальные параметры использования виртуальных машин, а также выявить или удалить бездействующие виртуальные машины и неподключенные диски. С помощью сведений в отчетах по оптимизации размера и отчетах о неэффективности можно создать план по уменьшению размера или удалению бездействующих виртуальных машин. Но отчеты по оптимизации в настоящее время не поддерживаются для учетных записей и подписок партнера CSP.

Если вы подготовили зарезервированные экземпляры AWS, то вы можете повысить эффективность использования этих экземпляров с помощью отчетов по оптимизации, в которых можно просмотреть рекомендации по покупке, внести изменения в резервирование, если оно не используется, и запланировать подготовку.


## <a name="next-steps"></a>Дополнительная информация

Теперь, когда вы знакомы с Cloudyn, следующим шагом является регистрация облачной среды и анализ данных.

- [Регистрация индивидуальной подписки Azure](quick-register-azure-sub.md)
