---
title: Общие сведения о Виртуальном рабочем столе Windows (предварительная версия)  — Azure
description: Общие сведения о Виртуальном рабочем столе Windows (предварительная версия)
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 07/17/2019
ms.author: helohr
ms.openlocfilehash: 03105232cb92a65f8c38d6d755910739a3fc4720
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305048"
---
# <a name="what-is-windows-virtual-desktop-preview"></a>Общие сведения о Виртуальном рабочем столе Windows (предварительная версия) 

Предварительная общедоступная версия службы "Виртуальный рабочий стол Windows" — это облачное решение для виртуализации рабочих столов и приложений.

При запуске Виртуального рабочего стола Windows в Azure доступны следующие возможности:

* Настройка развертывания Windows 10 с поддержкой нескольких сеансов со всеми соответствующими возможностями и функцией масштабируемости.
* Виртуализация Office 365 ProPlus с последующей оптимизацией включения поддержки виртуальных сценариев с несколькими пользователями.
* Предоставление виртуальных рабочих столов Windows 7 с бесплатными расширенными обновлениями системы безопасности.
* Перенос существующих служб удаленных рабочих столов, а также рабочих столов и приложений Windows Server на любой компьютер.
* Виртуализация рабочих столов и приложений.
* Управление рабочими столами и приложениями Windows 10, Windows Server и Windows 7 с помощью унифицированных средств управления.

## <a name="introductory-video"></a>Вступительное видео

В следующем видео Скотт Манчестер демонстрирует некоторые из возможностей виртуального рабочего стола Windows:

<br></br><iframe src="https://www.youtube-nocookie.com/embed/30dOLcZ4_9U" width="640" height="320" allowFullScreen="true" frameBorder="0"></iframe>

## <a name="key-capabilities"></a>Ключевые возможности

С помощью Виртуального рабочего стола Windows можно настроить масштабируемую и гибкую среду, используя следующие возможности:

* Создание среды виртуализации рабочих столов со всеми возможностями в подписке Azure без необходимости запускать дополнительные серверы шлюзов.
* Публикация требуемого количества пулов узлов для размещения различных рабочих нагрузок.
* Возможность использования собственных образов для рабочих нагрузок или тестирования из коллекции Azure.
* Минимизация расходов благодаря использованию ресурсов в составе пула с поддержкой нескольких сеансов. Благодаря поддержке нескольких сеансов в Windows 10 Корпоративная, реализуемой исключительно в рамках роли Виртуального рабочего стола Windows и узла сеансов удаленных рабочих столов в Windows Server, можно значительно уменьшить количество виртуальных машин и снизить нагрузку на ОС, предоставляя пользователям те же ресурсы.
* Назначение отдельных ролей владельца через личные рабочие столы (постоянные).

Развертывание и администрирование виртуальных рабочих столов предусматривает следующие задачи:

* Использование интерфейсов PowerShell и REST Виртуального рабочего стола Windows для настройки пулов узлов, создания групп приложений, назначения пользователей и публикации ресурсов.
* Публикация полных классических или отдельных удаленных приложений из одного пула узлов, создание отдельных групп приложений для разных групп пользователей и даже назначение пользователям нескольких групп приложений для минимизации количества используемых образов.
* Использование при управлении средой функции встроенного делегированного доступа для назначения ролей и сбора диагностики с последующим анализом различных пользовательских конфигураций и ошибок.
* Использование новой службы диагностики для устранения неполадок.
* Управление только образами и виртуальными машинами, а не инфраструктурой. Отсутствие необходимости лично управлять ролями удаленного рабочего стола, как при использовании служб удаленных рабочих столов; осуществляется управление только виртуальными машинами в подписке Azure.

Назначение пользователям виртуальных рабочих столов и подключение к ним.

* После назначения пользователи могут запускать любой клиент Виртуального рабочего стола Windows для подключения пользователей к опубликованным рабочим столам и приложениям Windows. Подключение с любого устройства через собственное приложение на устройстве или веб-клиент HTML5 Виртуального рабочего стола Windows.
* Безопасное обратное подключение пользователей к службе для предотвращения работы с открытыми входящими портами.

## <a name="requirements"></a>Требования

Есть несколько вещей, которые необходимо настроить для Виртуального рабочего стола Windows, чтобы успешно подключить пользователей к рабочим столам и приложениям Windows.

Мы планируем добавить поддержку приведенных ниже операционных систем. Поэтому убедитесь, что у вас есть [соответствующие лицензии](https://azure.microsoft.com/pricing/details/virtual-desktop/) для пользователей в соответствии с рабочим столом и приложениями, которые вы планируете развернуть.

|ОС|Требуемая лицензия|
|---|---|
|Windows 10 Корпоративная с поддержкой нескольких сеансов или Windows 10 Корпоративная|Microsoft 365 E3, E5, A3, A5, F1, бизнес<br>Windows E3, E5, A3, A5|
|Windows 7 Корпоративная |Microsoft 365 E3, E5, A3, A5, F1, бизнес<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|Клиентская лицензия служб удаленных рабочих столов с поддержкой программы Software Assurance|

Требования к инфраструктуре для включения поддержки Виртуального рабочего стола Windows следующие:

* Наличие [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/).
* Windows Server Active Directory с синхронизацией с Azure Active Directory. Это можно реализовать с помощью следующих компонентов:
  * Azure AD Connect
  * Доменные службы Azure AD
* Подписка Azure с виртуальной сетью, которая содержит экземпляр Windows Server Active Directory или подключена к нему.
  
Требования к виртуальным машинам Azure, созданным для Виртуального рабочего стола Windows, следующие:

* [Стандартное присоединение к домену](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-comparison) или [гибридное присоединение к AD](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan). Виртуальные машины не могут быть присоединены к Azure AD.
* Использование одного из следующих [поддерживаемых образов ОС](#supported-virtual-machine-os-image).

>[!NOTE]
>Если у вас нет подписки Azure, можно [зарегистрироваться и получить бесплатную пробную версию для использования в течение месяца](https://azure.microsoft.com/free/). Если вы используете бесплатную пробную версию Azure, следует использовать доменные службы Azure AD для синхронизации Windows Server Active Directory с Azure Active Directory.

Виртуальный рабочий стол Windows включает рабочие столы и приложения Windows, предоставляемые пользователям, а также решение для управления, размещаемое как услуга в Azure корпорацией Майкрософт. В период предоставления общедоступной предварительной версии рабочие столы и приложения можно развертывать на виртуальных машинах в любом регионе Azure. При этом решение для управления и данные для этих виртуальных машин будут размещаться в США (регион "Восточная часть США 2"). Это может привести к переносу данных в США при тестировании службы в период предоставления общедоступной предварительной версии. После выхода общедоступной версии мы начнем масштабировать решение для управления и локализацию данных во все регионы Azure.

Для обеспечения оптимальной производительности убедитесь, что сети соответствует следующим требованиям:

* Задержка кругового пути при передаче данных из сети клиента в регион Azure, где были развернуты пулы узлов, должна составлять менее 150 мс.
* Сетевой трафик может передаваться за пределы страны или региона, когда виртуальные машины, на которых размещены рабочие столы и приложения, подключаются к службе управления.
* Чтобы оптимизировать производительность сети, мы рекомендуем, чтобы виртуальные машины узла сеансов совместно размещались в том же регионе Azure, что и служба управления.

## <a name="supported-remote-desktop-clients"></a>Поддерживаемые клиенты удаленного рабочего стола

Виртуальный рабочий стол Windows поддерживают следующие клиенты удаленного рабочего стола:

* [Windows](https://docs.microsoft.com/azure/virtual-desktop/connect-windows-7-and-10)
* [HTML5](https://docs.microsoft.com/azure/virtual-desktop/connect-web)

## <a name="supported-virtual-machine-os-image"></a>Поддерживаемый образ ОС виртуальной машины

Виртуальный рабочий стол Windows поддерживает следующие образы ОС:

* Windows 10 Корпоративная с поддержкой нескольких сеансов
* Windows Server 2016

## <a name="next-steps"></a>Дополнительная информация

Чтобы начать работу, необходимо создать клиент. См. дополнительные сведения о создании клиента.

> [!div class="nextstepaction"]
> [Создание арендатора предварительной версии Виртуального рабочего стола Windows](tenant-setup-azure-active-directory.md)
