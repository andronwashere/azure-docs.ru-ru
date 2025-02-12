---
title: включение файла
description: включение файла
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/05/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: d0accd01926743d64fa4911dfe56806537170c2d
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2019
ms.locfileid: "66271564"
---
[Маршрутизация сообщений](../articles/iot-hub/iot-hub-devguide-messages-d2c.md) позволяет отправлять данные телеметрии с устройств Центра Интернета вещей на встроенные конечные точки, совместимые с центрами событий, или на настраиваемые конечные точки, такие как хранилище больших двоичных объектов, очередь служебной шины, раздел служебной шины и концентраторы событий. Для настройки пользовательской маршрутизации сообщений создайте [запросы маршрутизации](../articles/iot-hub/iot-hub-devguide-routing-query-syntax.md), чтобы настроить маршрут, соответствующий определенному условию. После выполнения настройки входящие данные автоматически направляются на конечные точки Центра Интернета вещей. Если сообщение не соответствует ни одному из определенных запросов маршрутизации, оно направляется к конечной точке по умолчанию.

В этой части руководства описана настройка запросов пользовательской маршрутизации и использование их с Центром Интернета вещей. Сообщения будут направляться от устройств Интернета вещей к одной из нескольких конечных точек, включая хранилище BLOB-объектов и очередь служебной шины. Сообщения в очередь служебной шины будут подхвачены приложением логики и отправлены по электронной почте. Сообщения, для которых не настроена пользовательская маршрутизация, отправляются в конечную точку по умолчанию, а затем принимаются службой Azure Stream Analytics и отображаются в визуализации Power BI.

В рамках первой и второй частей этого руководства вы выполнили приведенные ниже задачи.

**Часть 1. Создание ресурсов, настройка маршрутизации сообщений**
> [!div class="checklist"]
> * Создание ресурсов: Центра Интернета вещей, учетной записи хранения, очереди служебной шины и имитированного устройства. Их можно создать с помощью портала, интерфейса командной строки Azure (Azure CLI), Azure PowerShell или шаблона Azure Resource Manager.
> * Настройка конечных точек и маршрутов сообщений в Центре Интернета вещей для учетной записи хранения и очереди служебной шины.

**Часть 2. Отправка сообщений в концентратор, просмотр результатов маршрутизации**
> [!div class="checklist"]
> * Создание приложения логики, которое запущено и отправляет сообщение по электронной почте, когда сообщение добавляется в очередь служебной шины.
> * Загрузка и запуск приложения, которое имитирует устройство IoT отправки сообщений в концентратор для различных параметров маршрутизации.
> * Создайте визуализацию Power BI для данных, отправляемых в конечную точку по умолчанию.
> * Просмотр результатов ...
> * ... в очереди служебной шины и в сообщениях электронной почты.
> * ... в учетной записи хранения.
> * ... в визуализации Power BI.

## <a name="prerequisites"></a>Предварительные требования

* Для работы с первой частью этого руководства необходимо следующее:
  - У вас должна быть подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

* Для работы со второй частью этого руководства необходимо следующее:
  - Выполнить инструкции, приведенные в первой части руководства, и настроить необходимые ресурсы.
  - Установить [Visual Studio](https://www.visualstudio.com/).
  - Учетная запись Power BI для анализа потока аналитики конечных точек по умолчанию. (Доступна [бесплатная пробная версия Power BI](https://app.powerbi.com/signupredirect?pbi_source=web).)
  - Учетная запись Office 365 для отправки уведомлений по электронной почте.

[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]
