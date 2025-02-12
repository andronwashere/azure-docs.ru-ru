---
title: Обновление кластера Azure Service Fabric | Документация Майкрософт
description: Узнайте, как обновлять версию или конфигурацию кластера Azure Service Fabric.  В этой статье описывается настройка режима обновления кластера, обновление сертификатов, добавление портов приложений, применение исправлений для ОС и то, чего можно ожидать при выполнении обновления
services: service-fabric
documentationcenter: .net
author: aljo-microsoft
manager: chackdan
editor: ''
ms.assetid: 15190ace-31ed-491f-a54b-b5ff61e718db
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/12/2018
ms.author: aljo
ms.openlocfilehash: 8fa461d8c3a70d4b0d2d9973a840ffc7d1ff6470
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65472766"
---
# <a name="upgrading-and-updating-an-azure-service-fabric-cluster"></a>Обновление кластера Azure Service Fabric

Для любой современной системы разработка с учетом возможности модернизации является неотъемлемой составляющей успеха продукта. Кластер Service Fabric Azure представляет собой ресурс, который принадлежит вам, но частично управляется корпорацией Майкрософт. В этой статье содержатся сведения о компонентах, управляемых автоматически, и о том, что можно настроить самостоятельно.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Управление версиями Service Fabric в кластере

Обязательно следите за тем, чтобы кластер всегда работал под управлением [поддерживаемой версии Service Fabric](service-fabric-versions.md). Когда мы объявляем о выпуске новой версии Service Fabric, для предыдущей версии определяется срок завершения жизненного цикла. Этот срок составляет по меньшей мере 60 дней. О доступности новых выпусков сообщается в блоге группы разработчиков Service Fabric. после чего вы можете использовать их.

За 14 дней до истечения жизненного цикла выпуска, под управлением которого работает ваш кластер, будет создано событие работоспособности, которое переводит кластер в состояние предупреждения. Состояние предупреждения изменится, когда кластер будет обновлен до поддерживаемой версии.

Вы можете настроить для кластера автоматическое обновление Service Fabric по мере выпуска новых версий корпорацией Майкрософт или выбрать поддерживаемую версию, в которой должен работать ваш кластер.  Подробнее [об обновлении версии Service Fabric для кластера](service-fabric-cluster-upgrade-version-azure.md).

## <a name="fabric-upgrade-behavior-during-automatic-upgrades"></a>Поведение обновления Fabric во время автоматического обновления
Майкрософт поддерживает код и конфигурацию системы Service Fabric, которая выполняется в кластере Azure. По мере необходимости мы выполняем автоматические контролируемые обновления программного обеспечения. Эти обновления могут затрагивать код, конфигурацию или и то, и другое. Чтобы обновления не затрагивали приложение или оказывали на него минимальное влияние, обновление выполняется поэтапно.

### <a name="phase-1-an-upgrade-is-performed-by-using-all-cluster-health-policies"></a>Этап 1. Выполнение обновления с помощью всех политик работоспособности кластера
В ходе этого этапа обновления применяются к одному домену обновления за раз, а приложения, которые были запущены в кластере, продолжают работать без простоев. В ходе обновления соблюдаются все политики работоспособности кластера (политики, определяющие состояние работоспособности для узлов и всех приложений, выполняющихся в кластере).

Если требования политик работоспособности кластера не реализуются, выполняется откат обновления. Затем владельцу подписки отправляется сообщение электронной почты. Оно содержит следующую информацию:

* Уведомление о том, что нам пришлось выполнить откат обновления кластера.
* Рекомендуемые действия по решению проблемы, при их наличии.
* Количество дней (n) до выполнения этапа 2.

Мы постараемся выполнить аналогичное обновление еще несколько раз в случае, если причины неудачи связаны с инфраструктурой. Через n дней с момента отправки сообщения мы переходим к этапу 2.

Если политики работоспособности кластера соблюдены, обновление считается успешным и помечается как завершенное. Это может произойти во время первоначального или повторного запусков обновлений на этом этапе. В случае успешного выполнения сообщение с подтверждением не отправляется. Это позволяет избежать отправки чрезмерного количества сообщений. Получение сообщения следует рассматривать как исключение из обычного режима. Мы надеемся, что большинство обновлений кластера будет выполнено без ущерба для доступности вашего приложения.

### <a name="phase-2-an-upgrade-is-performed-by-using-default-health-policies-only"></a>Этап 2. Выполнение обновления только с помощью политик работоспособности по умолчанию
Политики работоспособности на этом этапе задаются таким образом, что ряд приложений, которые были работоспособны в начале обновления, остаются неизменными в течение всего процесса обновления. Как и в ходе этапа 1, на этапе 2 обновления применяются к одному домену обновления за раз, а приложения, которые были запущены в кластере, продолжают работать без простоев. В течение применения обновления действуют политики работоспособности кластера (это сочетание состояния работоспособности узла и всех приложений, выполняющихся в кластере).

Если применяемые политики работоспособности кластера фактически не соблюдаются, выполняется откат обновления. Затем владельцу подписки отправляется сообщение электронной почты. Оно содержит следующую информацию:

* Уведомление о том, что нам пришлось выполнить откат обновления кластера.
* Рекомендуемые действия по решению проблемы, при их наличии.
* Количество дней (n) до выполнения этапа 3.

Мы постараемся выполнить аналогичное обновление еще несколько раз в случае, если причины неудачи связаны с инфраструктурой. За несколько дней до назначенного срока владельцу подписки отправляется сообщение с напоминанием. Через n дней с момента отправки сообщения мы переходим к этапу 3. К сообщениям, которые отправляются на этапе 2, следует отнестись серьезно и выполнить действия по устранению ошибок.

Если политики работоспособности кластера соблюдены, обновление считается успешным и помечается как завершенное. Это может произойти во время первоначального или повторного запусков обновлений на этом этапе. В случае успешного выполнения сообщение с подтверждением не отправляется.

### <a name="phase-3-an-upgrade-is-performed-by-using-aggressive-health-policies"></a>Этап 3. Выполнение обновления с помощью жестких политик работоспособности кластера
Эти политики работоспособности на данном этапе предназначены для выполнения обновления, а не обеспечения работоспособности приложений. Этот этап редко применяется для обновления кластера. Если ваш кластер достигает этого этапа, есть высокая вероятность того, что ваше приложение перейдет в неработоспособное состояние и (или) будет недоступно.

Как и на других двух этапах, обновления на этапе 3 применяются к одному домену обновления за раз.

Если требования политик работоспособности кластера не реализуются, выполняется откат обновления. Мы постараемся выполнить аналогичное обновление еще несколько раз в случае, если причины неудачи связаны с инфраструктурой. После этого кластер закрепляется и больше не будет получать поддержку и обновления.

Сообщение с этими сведениями и корректирующими действиями отправляется владельцу подписки. Мы не ожидаем перехода кластеров в состояние, в котором произошел сбой этапа 3.

Если политики работоспособности кластера соблюдены, обновление считается успешным и помечается как завершенное. Это может произойти во время первоначального или повторного запусков обновлений на этом этапе. В случае успешного выполнения сообщение с подтверждением не отправляется.

## <a name="manage-certificates"></a>Управление сертификатами
Service Fabric использует [сертификаты сервера X.509](service-fabric-cluster-security.md), указываемые при создании кластера, для защиты обмена данными между узлами кластера и проверки подлинности клиентов. Вы можете добавлять, обновлять или удалять сертификаты для кластера и клиента с помощью [портала Azure](https://portal.azure.com), PowerShell или Azure CLI.  Подробнее о [добавлении или удалении сертификатов](service-fabric-cluster-security-update-certs-azure.md).

## <a name="open-application-ports"></a>Открытие портов приложений
Порты приложения можно изменить путем изменения свойств ресурса балансировщика нагрузки, связанных с типом узла. Для этого можно использовать портал Azure, PowerShell или Azure CLI. Дополнительные сведения см. в статье [Открытие портов для кластера Service Fabric](create-load-balancer-rule.md).

## <a name="define-node-properties"></a>Определение свойств узла
Иногда необходимо обеспечить выполнение конкретных рабочих нагрузок только на узлах определенных типов в кластере. Например, для одних рабочих нагрузок могут требоваться графические процессоры или накопители SSD, а для других — нет. Для каждого из типов узлов в кластере можно добавить пользовательские свойства. Ограничения размещения представляют собой операторы, привязанные к отдельным службам и включающие одно или несколько свойств узла. Ограничения размещения определяют, где должны запускаться службы.

Сведения об использовании ограничений на размещение, свойств узла и способах их определения см. [здесь](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints).

## <a name="add-capacity-metrics"></a>Добавление метрик емкости
Для каждого типа узла можно добавить настраиваемые метрики емкости, которые будут использоваться в приложениях для передачи данных о нагрузке. Дополнительные сведения об использовании метрик емкости для отчетах о нагрузке см. в документах по диспетчеру кластерных ресурсов Service Fabric на страницах [Описание кластера Service Fabric](service-fabric-cluster-resource-manager-cluster-description.md) и [Управление потреблением ресурсов и нагрузкой в Service Fabric с помощью метрик](service-fabric-cluster-resource-manager-metrics.md).

## <a name="set-health-policies-for-automatic-upgrades"></a>Настройка политик работоспособности для автоматического обновления
Вы можете указать пользовательские политики работоспособности для обновления Service Fabric. Если для кластера выбран режим автоматического обновления, эти политики применяются на первом этапе автоматического обновления Service Fabric.
Если для кластера указано обновление вручную, эти политики применяются при каждом выборе новой версии, когда запускается обновление Service Fabric в кластере. Если политики не переопределить, будут использоваться значения по умолчанию.

Определить пользовательские политики работоспособности или просмотреть текущие параметры можно в колонке "Обновление Service Fabric", выбрав расширенные параметры обновления. Как это сделать, показано ниже. 

![Управление пользовательскими политиками работоспособности][HealthPolices]

## <a name="customize-fabric-settings-for-your-cluster"></a>Настройка параметров Service Fabric для кластера
В кластере можно настроить множество разных параметров конфигурации, например, уровень надежности кластера и свойства узла. Дополнительные сведения см. в статье [Настройка параметров кластера Service Fabric](service-fabric-cluster-fabric-settings.md).

## <a name="patch-the-os-in-the-cluster-nodes"></a>Применение исправлений для операционной системы на узлах кластера
Приложение для управления исправлениями — это приложение Service Fabric, которое позволяет автоматизировать установку исправлений операционной системы в кластере Service Fabric и избегать простоев. [Приложение для управления исправлениями для Windows](service-fabric-patch-orchestration-application.md) можно развернуть в кластере, чтобы установить исправления контролируемым образом без простоев служб.


## <a name="next-steps"></a>Дальнейшие действия
* Узнайте, как настроить некоторые [параметры Service Fabric для кластера](service-fabric-cluster-fabric-settings.md)
* Ознакомьтесь с концепцией [масштабирования кластера](service-fabric-cluster-scale-up-down.md)
* Узнайте об [обновлениях приложений](service-fabric-application-upgrade.md)

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
