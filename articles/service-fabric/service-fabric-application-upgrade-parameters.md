---
title: 'Обновление приложений: параметры обновления | Документация Майкрософт'
description: Описывает параметры, относящиеся к обновлению приложения Service Fabric, включая проверки работоспособности и политики для автоматической отмены обновления.
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: chackdan
editor: ''
ms.assetid: a4170ac6-192e-44a8-b93d-7e39c92a347e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/08/2018
ms.author: subramar
ms.openlocfilehash: 9a93c0993ee45e72b11b023982dfbbe8c6528272
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60614395"
---
# <a name="application-upgrade-parameters"></a>Параметры обновления приложений
В настоящей статье описаны различные параметры, которые применяются во время обновления приложения Azure Service Fabric. Параметры обновления приложения управляют временем ожидания и проверками работоспособности, которые применяются во время обновления и указывают политики, которые должны применяться при сбое обновления. Параметры приложения применяются для обновлений с помощью:
- PowerShell
- Visual Studio
- SFCTL
- [REST](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-startapplicationupgrade)

Обновления приложений инициируются через один из трех режимов обновления, выбираемых пользователем. В каждом из приведенных ниже режимов есть свой собственный набор параметров приложения:
- Monitored;
- Unmonitored Auto;
- Unmonitored Manual.

Соответствующие обязательные и необязательные параметры описаны в следующих разделах.

## <a name="visual-studio-and-powershell-parameters"></a>Параметры Visual Studio и PowerShell

Для обновления приложений Service Fabric с помощью PowerShell используется команда [Start-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade). Режим обновления выбирается путем передачи параметра **Monitored** (Отслеживаемый), **UnmonitoredAuto** (Неотслеживаемый, авто) или **UnmonitoredManual** (Неотслеживаемый, вручную) в [Start-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade).

Параметры обновления приложения Service Fabric Visual Studio устанавливаются в диалоговом окне "Параметры обновления" Visual Studio. Режим обновления Visual Studio выбирается с помощью значений в раскрывающемся списке **Режим обновления**: **Monitored**, **UnmonitoredAuto** или **UnmonitoredManual**. Дополнительные сведения см. в статье [Настройка обновления приложения Service Fabric в Visual Studio](service-fabric-visualstudio-configure-upgrade.md).

### <a name="required-parameters"></a>Необходимые параметры
(PS=PowerShell, VS=Visual Studio)

| Параметр | Применяется к | Описание |
| --- | --- | --- |
ApplicationName |PS| Имя обновляемого приложения. Примеры: fabric:/VisualObjects, fabric:/ClusterMonitor. |
ApplicationTypeVersion|PS|Версия типа приложения, для которой предназначено обновление. |
FailureAction |PS, VS|Допустимые значения этого свойства: **Rollback** (Откат), **Manual** (Вручную) и **Invalid** (Недопустимо). Компенсирующее действие, выполняемое, когда режим обновления *Monitored* обнаруживает нарушения политики мониторинга или политики работоспособности. <br>Значение **Rollback** указывает, что служба обновления автоматически выполняет откат до версии перед обновлением. <br>Значение **Manual** указывает, что обновление будет переключено в режим обновления *UnmonitoredManual*. <br>Значение **Invalid** указывает, что действие при ошибке является недопустимым.|
Monitored |PS|Указывает, что режим обновления является отслеживаемым. Если работоспособность домена обновления и кластера соответствует определенным политикам работоспособности, после того как командлет завершит обновление для домена обновления, Service Fabric начнет обновление следующего домена обновления. Если домен обновления или кластер не соответствует политикам работоспособности, обновление завершается ошибкой и Service Fabric выполняет откат обновления для домена обновления или возвращается в режим обновления вручную в соответствии с заданной политикой. Это рекомендованный режим для обновлений приложения в рабочей среде. |
UpgradeMode | VS | Допустимые значения: **Monitored** (по умолчанию), **UnmonitoredAuto** или **UnmonitoredManual**. Дополнительные сведения см. в параметрах PowerShell для каждого режима в этой статье. |
UnmonitoredAuto | PS | Указывает, что режим обновления автоматический и не отслеживается. После того как Service Fabric обновит домен обновления, начнется обновление следующего домена обновления независимо от состояния работоспособности приложения. Этот режим не рекомендуется для рабочей среды и полезен только во время разработки приложения. |
UnmonitoredManual | PS | Указывает, что режим обновления выполняется пользователем вручную и является отслеживаемым. После того как Service Fabric обновит домен обновления, она ожидает, пока вы не обновите следующий домен обновления с помощью командлета *Resume-ServiceFabricApplicationUpgrade*. |

### <a name="optional-parameters"></a>Необязательные параметры

Параметры оценки работоспособности указывать не обязательно. Если критерии оценки работоспособности не указываются перед началом обновления, Service Fabric использует политики работоспособности приложения, указанные в файле ApplicationManifest.xml экземпляра приложения.

Для просмотра поля полного описания используйте горизонтальную полосу прокрутки в нижней части таблицы.

(PS=PowerShell, VS=Visual Studio)

| Параметр | Применяется к | Описание |
| --- | --- | --- |
| ApplicationParameter |PS, VS| Указывает переопределения параметров приложения.<br>Параметры приложения PowerShell указываются в виде пар имен и значений хэш-таблицы. Например, @{ "VotingData_MinReplicaSetSize" = "3"; "VotingData_PartitionCount" = "1" }.<br>Параметры приложения Visual Studio можно указать в диалоговом окне "Опубликовать приложение Service Fabric" в поле **Файл параметров приложения**.
| Подтверждение |PS| Допустимые значения: **True** и **False**. Запрашивает подтверждение перед выполнением командлета. |
| ConsiderWarningAsError |PS, VS |Допустимые значения: **True** и **False**. Значение по умолчанию — **false**. Считать предупреждения работоспособности приложения ошибками при оценке работоспособности приложения во время обновления. По умолчанию Service Fabric не считает предупреждения работоспособности ошибками (сбоями), чтобы обновление могло продолжиться даже при появлении предупреждений. |
| DefaultServiceTypeHealthPolicy | PS, VS |Указывает политику работоспособности, которую тип службы по умолчанию будет использовать для отслеживаемого обновления, в формате: MaxPercentUnhealthyPartitionsPerService, MaxPercentUnhealthyReplicasPerPartition, MaxPercentUnhealthyServices. Например, "5,10,15" указывает следующие значения: MaxPercentUnhealthyPartitionsPerService = 5, MaxPercentUnhealthyReplicasPerPartition = 10, MaxPercentUnhealthyServices = 15. |
| Force | PS, VS | Допустимые значения: **True** и **False**. Указывает, что процесс обновления пропускает предупреждающее сообщение и выполняет обновление принудительно даже в том случае, если номер версии не изменился. Это полезно для локального тестирования, но не рекомендуется для использования в рабочей среде, так как требует удаления имеющегося развертывания, что приводит к простою и потенциальной потере данных. |
| ForceRestart |PS, VS |Если вы обновите пакет конфигурации или данных, не обновив код службы, то служба будет перезапущена, только если свойству ForceRestart присвоено значение **True**. По завершении обновления структура служб уведомляет службу о том, что доступен новый пакет конфигурации или пакет данных. Ответственность за применение изменений принадлежит службе. При необходимости служба может самостоятельно перезапуститься. |
| HealthCheckRetryTimeoutSec |PS, VS |Продолжительность (в секундах), в течение которой структура службы продолжит выполнение оценки работоспособности перед выводом уведомления о сбое обновления. По умолчанию используется значение "600 секунд". Этот срок запускается после достижения конечного значения *HealthCheckWaitDurationSec*. В течение этого времени ожидания *HealthCheckRetryTimeout* Service Fabric может выполнить несколько проверок работоспособности приложения. Значение по умолчанию — 10 минут, его следует изменить в соответствии с вашим приложением. |
| HealthCheckStableDurationSec |PS, VS |Продолжительность (в секундах) ожидания завершения проверки и заключения о стабильности приложения перед переходом в следующий домен обновления или завершением обновления. Это время ожидания используется для того, чтобы предотвратить возможность пропуска необнаруженных проблем работоспособности сразу после выполнения проверки работоспособности. Значение по умолчанию — 120 секунд, его следует изменить в соответствии с вашим приложением. |
| HealthCheckWaitDurationSec |PS, VS | Время ожидания (в секундах) после завершения обновления на уровне домена обновления перед тем, как Service Fabric выполнит оценку работоспособности приложения. Этот срок можно рассматривать как время, в течение которого приложение должно быть запущено, чтобы оно могло рассматриваться работоспособным. После успешной проверки работоспособности процесс обновления переходит к следующему домену обновления.  При сбое проверки работоспособности Service Fabric будет ожидать в течение периода времени [UpgradeHealthCheckInterval](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-fabric-settings#clustermanager) перед возобновлением попытки выполнить проверку работоспособности снова до истечения срока, заданного свойством *HealthCheckRetryTimeoutSec*. По умолчанию и рекомендуемое значение — "0 секунд". |
| MaxPercentUnhealthyDeployedApplications|PS, VS |По умолчанию и рекомендуемое значение — "0". Укажите максимальное количество развертываемых приложений (см. раздел [Работоспособность](service-fabric-health-introduction.md)), которые могут быть сочтены неработоспособными перед тем, как само приложение будет сочтено неработоспособным и возникнет сбой обновления. Этот параметр определяет работоспособность приложения на узле и помогает обнаружить проблемы во время обновления. Обычно балансировщик нагрузки переносит рабочую нагрузку реплик приложения на другой узел, поэтому приложение может выглядеть работоспособным, позволяя тем самым продолжить обновление. Если в *MaxPercentUnhealthyDeployedApplications* указано точное значение работоспособности, Service Fabric сможет быстро обнаружить проблему с пакетом приложения, что позволит устранить ошибки еще на раннем этапе. |
| MaxPercentUnhealthyServices |PS, VS |Параметр для *DefaultServiceTypeHealthPolicy* и *ServiceTypeHealthPolicyMap*. По умолчанию и рекомендуемое значение — "0". Укажите максимальное количество служб в экземпляре приложения, которое может быть признано неработоспособным до того, как само приложение будет сочтено неработоспособным, что приведет к сбою обновления. |
| MaxPercentUnhealthyPartitionsPerService|PS, VS |Параметр для *DefaultServiceTypeHealthPolicy* и *ServiceTypeHealthPolicyMap*. По умолчанию и рекомендуемое значение — "0". Укажите максимальное количество разделов в службе, которые могут быть признаны неработоспособными, прежде чем служба будет сочтена неработоспособной. |
| MaxPercentUnhealthyReplicasPerPartition|PS, VS |Параметр для *DefaultServiceTypeHealthPolicy* и *ServiceTypeHealthPolicyMap*. По умолчанию и рекомендуемое значение — "0". Укажите максимальное количество реплик в разделе, которые могут быть признаны неработоспособными до того, как весь раздел будет признан неработоспособным. |
| ServiceTypeHealthPolicyMap | PS, VS | Представляет политику работоспособности, используемую для оценки работоспособности служб, относящихся к типу службы. Принимает входные данные хэш-таблицы в следующем формате: @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"} Например: @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" } |
| TimeoutSec | PS, VS | Указывает период времени ожидания в секундах для операции. |
| UpgradeDomainTimeoutSec |PS, VS |Максимальное время (в секундах) для обновления одного домена обновления. По истечении этого времени ожидания обновление остановится, после чего будет предпринято действие, определяемое атрибутом *FailureAction*. Значение по умолчанию — "никогда" (бесконечно). Его рекомендуется изменить в соответствии с вашим приложением. |
| UpgradeReplicaSetCheckTimeoutSec |PS, VS |Измеряется в секундах.<br>**Служба без отслеживания состояния**— при работе в одном домене обновления Service Fabric пытается обеспечить доступность дополнительных экземпляров службы. Если количество целевых экземпляров больше одного, то Service Fabric ожидает, пока не появится несколько экземпляров, до истечения максимального значения времени ожидания. Это время ожидания задается с помощью свойства *UpgradeReplicaSetCheckTimeoutSec*. По истечении времени ожидания Service Fabric продолжает обновление вне зависимости от количества экземпляров службы. Если счетчик целевых экземпляров имеет значение "1", структура службы не ожидает обновления, а сразу переходит к обновлению.<br><br>**Служба с отслеживанием состояния**— в одном домене обновления Service Fabric пытается обеспечить наличие кворума в наборе реплик. Service Fabric ожидает доступности кворума вплоть до достижения максимального значения времени ожидания (указанного в свойстве *UpgradeReplicaSetCheckTimeoutSec*). По истечении времени ожидания Service Fabric продолжает обновление вне зависимости от кворума. Этому параметру присваивается значение never (бесконечно) при накате и 1200 секунд при откате. |
| UpgradeTimeoutSec |PS, VS |Время ожидания (в секундах), применяемое для всего обновления. По истечении этого времени ожидания обновление остановится, после чего будет запущено действие *FailureAction*. Значение по умолчанию — "никогда" (бесконечно). Его рекомендуется изменить в соответствии с вашим приложением. |
| WhatIf | PS | Допустимые значения: **True** и **False**. Показывает, что произойдет при запуске командлета. Командлет не выполняется. |

Критерии *MaxPercentUnhealthyServices*, *MaxPercentUnhealthyPartitionsPerService* и *MaxPercentUnhealthyReplicasPerPartition* можно указать для каждого типа службы в экземпляре приложения. Отдельная установка этих параметров для каждой службы позволяет приложению содержать службы различных типов с разными политиками оценки. Например, значения параметра *MaxPercentUnhealthyPartitionsPerService* для типов службы "шлюз без отслеживания состояния" и "механизм с отслеживанием состояния" могут отличаться для конкретного экземпляра приложения.

## <a name="sfctl-parameters"></a>Параметры SFCTL

Для обновления приложения Service Fabric с помощью интерфейса командной строки Service Fabric используется команда [sfctl application upgrade](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade), а также следующие обязательные и необязательные параметры.

### <a name="required-parameters"></a>Необходимые параметры

| Параметр | Описание |
| --- | --- |
| application-id  |Идентификатор обновляемого приложения. <br> Обычно это полное имя приложения без указания схемы универсального кода ресурса (URI) "fabric:". Начиная с версии 6.0 иерархические имена разделяются знаком \~. Например, если имя приложения — "fabric: / myapp/app1", идентификатором приложения будет "myapp\~app1" в 6.0 + и «myapp/app1», в предыдущих версиях.|
application-version |Версия типа приложения, для которой предназначено обновление.|
parameters  |Список переопределяемых параметров приложения в формате JSON, применяемых при обновлении приложения.|

### <a name="optional-parameters"></a>Необязательные параметры

| Параметр | Описание |
| --- | --- |
default-service-health-policy | Спецификация политики работоспособности в формате [JSON](https://docs.microsoft.com/rest/api/servicefabric/sfclient-model-servicetypehealthpolicy), используемой по умолчанию для оценки работоспособности типа службы. Сопоставление является пустым по умолчанию. |
failure-action | Допустимые значения этого свойства: **Rollback** (Откат), **Manual** (Вручную) и **Invalid** (Недопустимо). Компенсирующее действие, выполняемое, когда режим обновления *Monitored* обнаруживает нарушения политики мониторинга или политики работоспособности. <br>Значение **Rollback** указывает, что служба обновления автоматически выполняет откат до версии перед обновлением. <br>Значение **Manual** указывает, что обновление будет переключено в режим обновления *UnmonitoredManual*. <br>Значение **Invalid** указывает, что действие при ошибке является недопустимым.|
force-restart | Если вы обновите пакет конфигурации или данных, не обновив код службы, то служба будет перезапущена, только если свойству ForceRestart присвоено значение **True**. По завершении обновления структура служб уведомляет службу о том, что доступен новый пакет конфигурации или пакет данных. Ответственность за применение изменений принадлежит службе. При необходимости служба может самостоятельно перезапуститься. |
health-check-retry-timeout | Интервал времени, на протяжении которого повторяются попытки оценить работоспособность неработоспособного приложения или кластера, прежде чем действие *FailureAction* будет выполнено. Сначала он интерпретируется как строка, представляющая длительность ISO 8601. В случае неудачи он интерпретируется как число, представляющее общее количество миллисекунд. Значение по умолчанию: PT0H10M0S. |
health-check-stable-duration | Период времени, в течение которого приложение или кластер должен оставаться работоспособным, прежде чем процесс обновления перейдет к следующему домену обновления. Сначала он интерпретируется как строка, представляющая длительность ISO 8601. В случае неудачи он интерпретируется как число, представляющее общее количество миллисекунд. Значение по умолчанию: PT0H2M0S. |
health-check-wait-duration | Время ожидания после завершения работы домена обновления перед применением политики работоспособности. Сначала он интерпретируется как строка, представляющая длительность ISO 8601. В случае неудачи он интерпретируется как число, представляющее общее количество миллисекунд. Значение по умолчанию: 0.|
max-unhealthy-apps | По умолчанию и рекомендуемое значение — "0". Укажите максимальное количество развертываемых приложений (см. раздел [Работоспособность](service-fabric-health-introduction.md)), которые могут быть сочтены неработоспособными перед тем, как само приложение будет сочтено неработоспособным и возникнет сбой обновления. Этот параметр определяет работоспособность приложения на узле и помогает обнаружить проблемы во время обновления. Обычно балансировщик нагрузки переносит рабочую нагрузку реплик приложения на другой узел, поэтому приложение может выглядеть работоспособным, позволяя тем самым продолжить обновление. Если в *max-unhealthy-apps* указано точное значение работоспособности, Service Fabric сможет быстро обнаружить проблему с пакетом приложения, что позволит устранить ошибки еще на раннем этапе. Представленный в виде числа от 0 до 100. |
mode; | Допустимые значения: **Monitored**, **UpgradeMode**, **UnmonitoredAuto**, **UnmonitoredManual**. Значение по умолчанию: **UnmonitoredAuto**. Описание этих значений приведено в разделе *обязательных параметров* для Visual Studio и PowerShell.|
replica-set-check-timeout |Измеряется в секундах. <br>**Служба без отслеживания состояния**— при работе в одном домене обновления Service Fabric пытается обеспечить доступность дополнительных экземпляров службы. Если количество целевых экземпляров больше одного, то Service Fabric ожидает, пока не появится несколько экземпляров, до истечения максимального значения времени ожидания. Это время ожидания задается с помощью свойства *replica-set-check-timeout*. По истечении времени ожидания Service Fabric продолжает обновление вне зависимости от количества экземпляров службы. Если счетчик целевых экземпляров имеет значение "1", структура службы не ожидает обновления, а сразу переходит к обновлению.<br><br>**Служба с отслеживанием состояния**— в одном домене обновления Service Fabric пытается обеспечить наличие кворума в наборе реплик. Service Fabric ожидает доступности кворума вплоть до достижения максимального значения времени ожидания (указанного в свойстве *replica-set-check-timeout*). По истечении времени ожидания Service Fabric продолжает обновление вне зависимости от кворума. Этому параметру присваивается значение never (бесконечно) при накате и 1200 секунд при откате. |
service-health-policy | Сопоставление политик работоспособности для типов служб и типов служб в формате JSON. Сопоставление является пустым по умолчанию. [Параметр в формате JSON](https://docs.microsoft.com/rest/api/servicefabric/sfclient-model-applicationhealthpolicy#servicetypehealthpolicymap). JSON для части "Значения" содержит **MaxPercentUnhealthyServices**, **MaxPercentUnhealthyPartitionsPerService** и **MaxPercentUnhealthyReplicasPerPartition**. Описание этих параметров приведено в разделе необязательных параметров для Visual Studio и PowerShell.
timeout | Указывает период времени ожидания в секундах для операции. Значение по умолчанию: 60. |
upgrade-domain-timeout | Время выполнения каждого домена обновления, по истечении которого выполняется действие *FailureAction*. Сначала он интерпретируется как строка, представляющая длительность ISO 8601. В случае неудачи он интерпретируется как число, представляющее общее количество миллисекунд. Значение по умолчанию — "никогда" (бесконечно). Его рекомендуется изменить в соответствии с вашим приложением. Значение по умолчанию: P10675199DT02H48M05.4775807S. |
upgrade-timeout | Время выполнения каждого домена обновления, по истечении которого выполняется действие *FailureAction*. Сначала он интерпретируется как строка, представляющая длительность ISO 8601. В случае неудачи он интерпретируется как число, представляющее общее количество миллисекунд. Значение по умолчанию — "никогда" (бесконечно). Его рекомендуется изменить в соответствии с вашим приложением. Значение по умолчанию: P10675199DT02H48M05.4775807S.|
warning-as-error | Допустимые значения: **True** и **False**. Значение по умолчанию — **false**. Можно передать в виде флага. Считать предупреждения работоспособности приложения ошибками при оценке работоспособности приложения во время обновления. По умолчанию Service Fabric не считает предупреждения работоспособности ошибками (сбоями), чтобы обновление могло продолжиться даже при появлении предупреждений. |

## <a name="next-steps"></a>Дальнейшие действия
[Руководство по обновлению приложений Service Fabric с помощью Visual Studio](service-fabric-application-upgrade-tutorial.md) поможет вам выполнить поэтапное обновление приложения с помощью Visual Studio.

[Обновление приложения с помощью PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) поможет вам выполнить обновление приложения с помощью PowerShell.

Статья [Управление приложением Azure Service Fabric с помощью интерфейса командной строки Azure Service Fabric](service-fabric-application-lifecycle-sfctl.md#upgrade-application) поможет вам выполнить обновление приложения с интерфейса командной строки Service Fabric.

[Обновление приложения с помощью подключаемого модуля Eclipse Service Fabric](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

Узнайте, как использовать [сериализацию данных](service-fabric-application-upgrade-data-serialization.md), чтобы обеспечить совместимость обновлений приложения.

[Дополнительные разделы](service-fabric-application-upgrade-advanced.md)содержат сведения о работе с расширенными функциями при обновлении приложения.

Сведения об устранении распространенных проблем при обновлении приложений см. в статье [Устранение неполадок при обновлениях приложений](service-fabric-application-upgrade-troubleshooting.md).
