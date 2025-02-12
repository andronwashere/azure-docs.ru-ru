---
title: Вопросы и ответы по доступности приложений и служб для облачных служб Microsoft Azure | Документы Майкрософт
description: В этой статье приведены часто задаваемые вопросы по доступности приложений и служб для облачных служб Microsoft Azure.
services: cloud-services
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: top-support-issue
ms.assetid: 84985660-2cfd-483a-8378-50eef6a0151d
ms.service: cloud-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: fb4b5dde63d8c7c75419d3202d9848cd6fde8b8a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60337550"
---
# <a name="application-and-service-availability-issues-for-azure-cloud-services-frequently-asked-questions-faqs"></a>Приложения и проблемы доступности службы для облачных служб Azure: Часто задаваемые вопросы (FAQ)

В этой статье приведены часто задаваемые вопросы по доступности приложений и служб для [облачных служб Microsoft Azure](https://azure.microsoft.com/services/cloud-services). Сведения о размерах приводятся в статье [Размеры для облачных служб](cloud-services-sizes-specs.md) .

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="my-role-got-recycled-was-there-any-update-rolled-out-for-my-cloud-service"></a>Моя роль была возобновлена. Было ли развернуто какое-либо обновление для моей облачной службы?
Примерно раз в месяц корпорация Майкрософт выпускает новую версию гостевой ОС для виртуальных машин PaaS Windows Azure. Гостевая ОС — только одно из таких обновлений в конвейере. На выпуск может влиять множество факторов. Кроме того, Azure работает на сотнях тысяч компьютеров. Это означает, что невозможно предсказать точную дату и время перезапуска ваших ролей. Корпорация Майкрософт будет размещать на RSS-канале обновлений гостевой ОС последние сведения, которыми мы располагаем, но указанные временные рамки следует воспринимать только в качестве приблизительных. Мы осознаем, что это вызывает неудобства для клиентов, и работаем над планом по ограничению перезапусков или уточнению их времени.

Полные сведения о последних обновлениях гостевой ОС см. в статье [Выпуски гостевой ОС Azure и таблица совместимости пакетов SDK](cloud-services-guestos-update-matrix.md).

Полезные сведения о перезапусках и ссылки на дополнительную техническую информацию по обновлениям гостевой и базовой ОС см. в записи блога MSDN [Role Instance Restarts Due to OS Upgrades](https://blogs.msdn.com/b/kwill/archive/2012/09/19/role-instance-restarts-due-to-os-upgrades.aspx) (Перезапуски экземпляров ролей из-за обновлений ОС).

## <a name="why-does-the-first-request-to-my-cloud-service-after-the-service-has-been-idle-for-some-time-take-longer-than-usual"></a>Почему первый запрос в облачную службу после простоя службы в течение некоторого времени занимает больше времени, чем обычно?
Когда веб-сервер получает первый запрос, он сначала перекомпилирует код, а затем обрабатывает запрос. Вот почему первый запрос занимает больше времени, чем другие. По умолчанию пул приложений завершает работу в случае бездействия пользователя. Пул приложений также будет перезапускаться по умолчанию каждые 1,740 минут (29 часов).

Пулы приложений служб IIS могут периодически перезапускаться во избежание нестабильных состояний, которые могут привести к аварийному завершению приложений, зависаниям или утечкам памяти.

Следующие документы помогут вам понять и решить эту проблему.
* [Исправление медленной начальной загрузки для служб IIS](https://stackoverflow.com/questions/13386471/fixing-slow-initial-load-for-iis)
* [Очень медленный первый запрос веб-приложения IIS 7.5 после перезапуска пула приложений](https://stackoverflow.com/questions/13917205/iis-7-5-web-application-first-request-after-app-pool-recycle-very-slow)

Если вы хотите изменить поведение по умолчанию служб IIS, необходимо будет использовать задачи при запуске, так как если вручную применить изменения к экземплярам веб-роли, в итоге изменения будут потеряны.

Дополнительные сведения см. в статье [Как настроить и выполнить задачи при запуске для облачной службы](cloud-services-startup-tasks.md).
