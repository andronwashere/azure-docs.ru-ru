---
author: rothja
ms.service: billing
ms.topic: include
ms.date: 11/09/2018
ms.author: jroth
ms.openlocfilehash: ce64047fd7490106790ea8bb1ad7963d82a87c24
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67185664"
---
| Ресурс | Free | Совмещаемая блокировка | базовая; | Стандартная | Премиум (версия 2) | Isolated </th> |
| --- | --- | --- | --- | --- | --- | --- |
| [Web, mobile или API приложения](https://azure.microsoft.com/services/app-service/) на [план службы приложений Azure](../articles/app-service/overview-hosting-plans.md)<sup>1</sup> |10 |100 |Без ограничений<sup>2</sup> |Без ограничений<sup>2</sup> |Без ограничений<sup>2</sup> |Без ограничений<sup>2</sup>|
| [План обслуживания приложения](../articles/app-service/overview-hosting-plans.md) |10 для каждого региона |10 шт. на одну группу ресурсов |100 шт. на одну группу ресурсов |100 шт. на одну группу ресурсов |100 шт. на одну группу ресурсов |100 шт. на одну группу ресурсов|
| Тип вычислительной операции |Совмещаемая блокировка |Совмещаемая блокировка |Выделенный<sup>3</sup> |Выделенный<sup>3</sup> |Выделенный<sup>3</sup></p> |Выделенный<sup>3</sup>|
| [Горизонтальное масштабирование](../articles/app-service/web-sites-scale.md) (максимальное количество экземпляров) |1 общедоступный |1 общедоступный |3 выделенных<sup>3</sup> |10 выделенных<sup>3</sup> |20 выделенных<sup>3</sup>|100 выделенных<sup>4</sup>|
| Хранилище<sup>5</sup> |1 ГБ<sup>5</sup> |1 ГБ<sup>5</sup> |10 ГБ<sup>5</sup> |50 ГБ<sup>5</sup> |250 ГБ<sup>5</sup></p> |1 ТБ<sup>5</sup>|
| Время ЦП (5 минут)<sup>6</sup> |3 минуты |3 минуты |Без ограничений, оплата по стандартным [тарифам](https://azure.microsoft.com/pricing/details/app-service/)</a> |Без ограничений, оплата по стандартным [тарифам](https://azure.microsoft.com/pricing/details/app-service/)</a> |Без ограничений, оплата по стандартным [тарифам](https://azure.microsoft.com/pricing/details/app-service/)</a> |Без ограничений, оплата по стандартным [тарифам](https://azure.microsoft.com/pricing/details/app-service/)</a>|
| Время ЦП (в день)<sup>6</sup> |60 минут |240 минут |Без ограничений, оплата по стандартным [тарифам](https://azure.microsoft.com/pricing/details/app-service/)</a> |Без ограничений, оплата по стандартным [тарифам](https://azure.microsoft.com/pricing/details/app-service/)</a> |Без ограничений, оплата по стандартным [тарифам](https://azure.microsoft.com/pricing/details/app-service/)</a> |Без ограничений, оплата по стандартным [тарифам](https://azure.microsoft.com/pricing/details/app-service/)</a> |
| Память (1 час) |1024 МБ на один план службы приложений |1024 МБ на одно приложение |Н/Д |Н/Д |Н/Д |Н/Д |
| Пропускная способность |165 MB |Без ограничений, применяются [тарифы на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/) |Без ограничений, применяются [тарифы на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/) |Без ограничений, применяются [тарифы на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/) |Без ограничений, применяются [тарифы на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/) |Без ограничений, применяются [тарифы на передачу данных](https://azure.microsoft.com/pricing/details/data-transfers/) |
| Архитектура приложения |32-битная |32-битная |32- или 64-битная |32- или 64-битная |32- или 64-битная |32- или 64-битная |
| Веб-сокеты на один экземпляр<sup>7</sup> |5 |35 |350 |Не ограничено |Не ограничено |Без ограничений |
| Количество одновременных [подключений к отладчику](../articles/app-service/troubleshoot-dotnet-visual-studio.md) для каждого приложения |1 |1 |1 |5 |5 |5 |
| Сертификаты службы приложений в рамках одной подписки<sup>10</sup>| Не поддерживается | Не поддерживается |10 |10 |10 |10 |
| Количество личных доменов на одно приложение</a> |0 (только поддомен azurewebsites.net)|500 |500 |500 |500 |500 |
| личных доменов [Поддержка SSL](../articles/app-service/app-service-web-tutorial-custom-ssl.md) |Не поддерживается, групповой сертификат для *. azurewebsites.net по умолчанию|Не поддерживается, групповой сертификат для *. azurewebsites.net по умолчанию|Неограниченное количество подключений SSL на основе SNI |Включено неограниченное количество подключений SSL на основе SNI и 1 подключение SSL на основе IP |Включено неограниченное количество подключений SSL на основе SNI и 1 подключение SSL на основе IP | Включено неограниченное количество подключений SSL на основе SNI и 1 подключение SSL на основе IP|
| Интегрированная служба load balancer | |X |X |X |X |X<sup>9</sup> |
| [Всегда включено](../articles/app-service/configure-common.md) | | |X |X |X |X |
| [Плановое резервное копирование](../articles/app-service/manage-backup.md) | | | | Плановое резервное копирование каждые 2 часа более 12 резервных копий в день (ручные + плановые) | Плановая архивация каждый час, более 50 резервных копий в день (ручные + плановые) | Плановая архивация каждый час, более 50 резервных копий в день (ручные + плановые) |
| [Autoscale](../articles/app-service/web-sites-scale.md) | | | |X |X |X |
| [Веб-задания](../articles/app-service/webjobs-create.md)<sup>8</sup> |X |X |X |X |X |X |
| [планировщика Azure](https://azure.microsoft.com/services/scheduler/) | |X |X |X |X |X |
| [Мониторинг конечных точек](../articles/app-service/web-sites-monitor.md) | | |X |X |X |X |
| [Промежуточные слоты](../articles/app-service/deploy-staging-slots.md) | | | |5 |20 |20 |
| Соглашение об уровне обслуживания | |  |99,9 % |99,95 %|99,95 %|99,95 %|  

<sup>1</sup>Если не указано иное, квоты приложений и хранилищ указаны в расчете на один план службы приложений.  
<sup>2</sup>Фактическое количество приложений, которые можно разместить на этих машинах, определяется интенсивностью работы приложений, размером экземпляров машин и степенью использования соответствующих ресурсов.  
<sup>3</sup>Выделенные экземпляры могут иметь различные размеры. Дополнительные сведения см. на странице [цен на службу приложений](https://azure.microsoft.com/pricing/details/app-service/).  
<sup>4</sup>допускается и больше по запросу.  
<sup>5</sup>аксимальный размер хранилища — это общий размер содержимого для всех приложений в одном и том же плане службы приложений.  
<sup>6</sup>Эти ресурсы ограничены физическими ресурсами на выделенных экземплярах (размером экземпляра и количеством экземпляров).  
<sup>7</sup>При масштабировании приложения в два экземпляра на уровне Basic для каждого из двух экземпляров создается 350 параллельных подключений.  
<sup>8</sup>Выполните пользовательские исполняемые файлы или сценарии по требованию, по расписанию или непрерывно в виде фоновых задач в своем экземпляре службы приложений. Для обеспечения непрерывной работы веб-заданий требуется установка опции "Всегда включено". Для обеспечения работы веб-заданий по расписанию на уровнях Free или Standard требуется использование планировщика Azure. Нет предопределенных ограничений на количество веб-заданий, которое может выполняться в одном экземпляре службы приложений. Существуют практические ограничения, которые зависят от того, что пытается сделать код приложения.  
<sup>9</sup>номеров SKU изолированной службы приложений могут быть внутренне балансировки нагрузки (ILB) с помощью Azure Load Balancer, поэтому нет открытого подключения из Интернета. В результате некоторые функции Службы приложений (цен. категория "Изолированный") с ILB следует использовать на компьютерах с прямым доступом к конечной точке сети ILB.  
<sup>10</sup>через службу поддержки запрос на максимальное ограничение в 200 можно увеличить квоту сертификата службы приложений в рамках одной подписки.  