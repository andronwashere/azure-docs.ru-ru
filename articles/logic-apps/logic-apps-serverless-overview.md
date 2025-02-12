---
title: Обзор бессерверных приложений Azure | Документация Майкрософт
description: Сведения о создании эффективных решений в облаке без хлопот с инфраструктурой
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: jeffhollan
ms.author: jehollan
ms.reviewer: klam, estfan, LADocs
ms.custom: vs-azure
ms.topic: article
ms.date: 03/30/2017
ms.openlocfilehash: 9cd1946d8fa670764bcc95b82298d07cc70417bd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60684114"
---
# <a name="overview-azure-serverless-with-azure-logic-apps-and-azure-functions"></a>Рассматриваемые действия: Бессерверных компонентов Azure с Azure Logic Apps и функции Azure

[Независимые (бессерверные)](https://azure.microsoft.com/solutions/serverless/) приложения обеспечивают такие преимущества, как увеличение скорости разработки, сокращение объема кода, простота и масштабируемость.  В этой статье рассматриваются различные атрибуты бессерверных решений и предложения Azure, независимые от сервера.

## <a name="what-is-serverless"></a>Что значит "без сервера"?

Бессерверность означает отсутствие не серверов, а необходимости беспокоиться о них.  Традиционная разработка приложений по большей части связана с решением задач масштабирования, размещения и мониторинга решений, которые бы соответствовали требованиям приложения.  Бессерверные решения подразумевают, что все эти задачи решаются поставщиком.  Кроме того, бессерверные приложения оплачиваются в рамках плана, основанного на потреблении.  Если приложение не используется, плата не начисляется.  Эти функции позволяют разработчикам сосредоточиться исключительно на бизнес-логике решения.

Базовые службы Azure, в которых реализован принцип независимости от серверов, — [Функции Azure](https://azure.microsoft.com/services/functions/) и [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/).  Оба эти решения позволяют разработчикам создавать надежные облачные приложения с помощью минимального объема кода.

## <a name="what-are-azure-functions"></a>Что такое функции Azure?

Функции Azure — это решение для быстрого запуска фрагментов кода (функций) в облаке. Вы можете быстро написать код для решения своих задач, не беспокоясь о наличии приложения или инфраструктуры для его выполнения. Функции делают разработку более продуктивной. Кроме того, вы можете использовать предпочтительный язык программирования, например C#, F#, Node.js, Python или PHP. Оплата взимается только при выполнении кода, а масштабирование в Azure выполняется при необходимости.

Чтобы приступить к работе с Функциями Azure сразу, [создайте первую функцию Azure](../azure-functions/functions-create-first-azure-function.md). Дополнительные технические сведения о Функциях см. в статье [Справочник разработчика по функциям Azure](../azure-functions/functions-reference.md).

## <a name="what-are-azure-logic-apps"></a>Что такое Azure Logic Apps?

Служба Azure Logic Apps позволяет упростить и реализовать масштабируемые рабочие процессы и сценарии интеграции в облаке. Это визуальный конструктор для моделирования и автоматизации процессов в виде ряда операций, которые называются рабочим процессом.  Для облачных и локальных служб предусмотрено [много соединителей](../connectors/apis-list.md) для быстрого подключения бессерверного приложения к другим API.  Приложение логики запускается при срабатывании триггера (например, When an account is added to Dynamics CRM (При добавлении учетной записи в Dynamics CRM)), после чего могут выполняться разные комбинации действий, преобразования и условная логика.  Служба Logic Apps удобна при оркестрации различных функций Azure в процессе, особенно в том случае, если процесс требует взаимодействия с внешней системой или API.

Чтобы приступить к работе с Logic Apps, начните с [создания первого приложения логики](quickstart-create-first-logic-app-workflow.md).  Дополнительные технические сведения о Logic Apps см. в [справочнике разработчика](logic-apps-workflow-actions-triggers.md).

## <a name="how-can-i-build-and-deploy-serverless-applications-in-azure"></a>Как создавать и развертывать бессерверные приложения в Azure?

Azure предоставляет широкий набор средств для разработки, развертывания бессерверных приложений и управления ими.  Приложения можно создавать непосредственно на портале Azure или с помощью [инструментов из Visual Studio](logic-apps-serverless-get-started-vs.md).  После разработки приложение можно [развернуть мгновенно](logic-apps-create-deploy-template.md).  Azure также обеспечивает мониторинг бессерверных приложений.  Мониторинг может осуществляться из портала Azure, через API или SDK или с помощью интегрированных инструментов для журналов Azure Monitor и Application Insights.

## <a name="next-steps"></a>Дальнейшие действия

* [Создание первого бессерверного приложения в Visual Studio](logic-apps-serverless-get-started-vs.md)
* [Создание панели мониторинга Customer Insights с помощью бессерверного решения](logic-apps-scenario-social-serverless.md)
* [Создание шаблона развертывания для приложения логики](logic-apps-create-deploy-template.md)
