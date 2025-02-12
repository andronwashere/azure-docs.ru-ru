---
title: Методики безопасного развертывания в регионах с помощью диспетчера развертывания Azure
description: В этой статье описывается, как развернуть службу в нескольких регионах с помощью диспетчера развертывания Azure. Кроме того, здесь приведены методики безопасного развертывания, которые позволяют проверить стабильность развертывания во все регионы, прежде чем выполнять этот процесс.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/31/2019
ms.author: tomfitz
ms.custom: seodec18
ms.openlocfilehash: 6a25444f0207ec5eceb029c5d31d222a31813e22
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67066820"
---
# <a name="enable-safe-deployment-practices-with-azure-deployment-manager-public-preview"></a>Включить методах безопасного развертывания с помощью диспетчера развертывания Azure (Предварительная версия)

Чтобы развернуть службу в нескольких регионах и обеспечить ее правильную работу в них, можно использовать диспетчер развертывания Azure, чтобы координировать поэтапное развертывание службы. Как и для любого развертывания Azure, ресурсы для службы определяются в [шаблонах Resource Manager](resource-group-authoring-templates.md). После создания шаблонов используйте диспетчер развертывания, чтобы описать топологию службы и ее развертывание.

Диспетчер развертывания — это компонент Resource Manager. Он расширяет возможности во время развертывания. Используйте диспетчер развертывания, если вам нужно развернуть сложную службу в нескольких регионах. Помещая выпуск службы на промежуточное хранение и обработку, можно обнаружить потенциальные проблемы, прежде чем он будет развернут во всех регионах. Если вам не нужны дополнительные меры предосторожности поэтапного развертывания, используйте стандартные [варианты развертывания](resource-group-template-deploy-portal.md) с помощью Resource Manager. Диспетчер развертывания легко интегрируется со всеми имеющимися сторонними инструментами, которые поддерживают развертывания Resource Manager, такими как предложения для непрерывной интеграции и непрерывной доставки (CI/CD).

Azure Deployment Manager находится в предварительной версии. Помогите нам улучшить функцию, предоставляя [отзывы](https://aka.ms/admfeedback).

Чтобы использовать диспетчер развертывания, необходимо создать четыре файла:

* шаблон топологии;
* шаблон развертывания;
* файл параметров для топологии;
* файл параметров для развертывания.

Сначала развертывается шаблон топологии, а затем — шаблон развертывания.

Дополнительные ресурсы:

- [Справочник по REST API Azure Deployment Manager](https://docs.microsoft.com/rest/api/deploymentmanager/).
- [Учебник. Использование диспетчера развертывания Azure с шаблонами Resource Manager (закрытая предварительная версия)](./deployment-manager-tutorial.md).
- [Учебник. Use health check in Azure Deployment Manager](./deployment-manager-tutorial-health-check.md) (Учебник по использованию проверки работоспособности в диспетчере развертывания Azure).
- [Пример диспетчера развертывания Azure](https://github.com/Azure-Samples/adm-quickstart).

## <a name="identity-and-access"></a>Удостоверение и доступ

Диспетчер развертывания использует [управляемое удостоверение, назначаемое пользователем](../active-directory/managed-identities-azure-resources/overview.md), для выполнения операций развертывания. Это удостоверение создается перед началом развертывания. Ему должен быть предоставлен доступ к подписке, в которую развертывается служба, а также достаточные разрешения для выполнения развертывания. Сведения о действиях, разрешаемых с помощью ролей, см. в разделе [Встроенные роли для ресурсов Azure](../role-based-access-control/built-in-roles.md).

Удостоверение должны находиться в одном расположении с развертыванием.

## <a name="topology-template"></a>Шаблон топологии

Шаблон топологии описывает ресурсы Azure для создания службы и расположение их развертывания. На следующем рисунке показана топология для примера службы.

![Иерархия: от топологии службы к службам и модулям службы](./media/deployment-manager-overview/service-topology.png)

В шаблоне топологии указаны следующие ресурсы.

* Источник артефактов указывает место хранения шаблонов Resource Manager и параметров.
* Топология службы указывает источник артефактов.
  * Службы указывают расположение и идентификатор подписки Azure.
    * Модули службы определяют группу ресурсов, режим развертывания и путь к файлам шаблонов и параметров.

Чтобы понять, что происходит на каждом уровне, поможет выяснить, какие значения вы указываете.

![Значения для каждого уровня](./media/deployment-manager-overview/topology-values.png)

### <a name="artifact-source-for-templates"></a>Источник артефактов для шаблонов

В шаблоне топологии вы создаете источник артефактов, в котором расположены файлы шаблонов и параметров. Источник артефактов позволяет извлечь эти файлы для развертывания. Вы увидите другой источник артефактов для двоичных файлов далее в этой статье.

В следующем примере показан общий формат источника артефактов.

```json
{
    "type": "Microsoft.DeploymentManager/artifactSources",
    "name": "<artifact-source-name>",
    "location": "<artifact-source-location>",
    "apiVersion": "2018-09-01-preview",
    "properties": {
        "sourceType": "AzureStorage",
        "artifactRoot": "<root-folder-for-templates>",
        "authentication": {
            "type": "SAS",
            "properties": {
                "sasUri": "<SAS-URI-for-storage-container>"
            }
        }
    }
}
```

Дополнительные сведения см. в [справочнике по шаблону artifactSources](/azure/templates/Microsoft.DeploymentManager/artifactSources).

### <a name="service-topology"></a>Топология службы:

В следующем примере показан общий формат источника топологии службы. Следует указать идентификатор ресурса источника артефактов, который содержит файлы шаблонов и файл параметров. Топология службы включает в себя все ресурсы службы. Чтобы обеспечить доступность источника артефактов, топология службы зависит от него.

```json
{
    "type": "Microsoft.DeploymentManager/serviceTopologies",
    "name": "<topology-name>",
    "location": "<topology-location>",
    "apiVersion": "2018-09-01-preview",
    "properties": {
        "artifactSourceId": "<resource-ID-artifact-source>"
    },
    "dependsOn": [
        "<artifact-source>"
    ],
    "resources": [
        {
            "type": "services",
            ...
        }
    ]
}
```

Дополнительные сведения см. в [справочнике по шаблону serviceTopologies](/azure/templates/Microsoft.DeploymentManager/serviceTopologies).

### <a name="services"></a>Службы

В следующем примере показан общий формат ресурса служб. В каждой службе следует указать расположение и идентификатор подписки Azure, используемой для развертывания службы. Чтобы выполнить развертывание в нескольких регионах, нужно определить службу для каждого региона. Служба зависит от топологии службы.

```json
{
    "type": "services",
    "name": "<service-name>",
    "location": "<service-location>",
    "apiVersion": "2018-09-01-preview",
    "dependsOn": [
        "<service-topology>"
    ],
    "properties": {
        "targetSubscriptionId": "<subscription-ID>",
        "targetLocation": "<location-of-deployed-service>"
    },
    "resources": [
        {
            "type": "serviceUnits",
            ...
        }
    ]
}
```

Дополнительные сведения см. в [справочнике по шаблону services](/azure/templates/Microsoft.DeploymentManager/serviceTopologies/services).

### <a name="service-units"></a>Единицы измерения службы

В следующем примере показан общий формат ресурса модулей службы. В каждом модуле службы следует указать группу ресурсов, [режим развертывания](deployment-modes.md) и путь к файлу шаблона и файлу параметров. Если указать относительный путь к файлам шаблона и параметров, полный путь составляется на основе корневого каталога в источнике артефактов. Можно указать абсолютный путь к файлам шаблона и параметров, но вы не сможете легко управлять версиями выпусков. Модуль службы зависит от службы.

```json
{
    "type": "serviceUnits",
    "name": "<service-unit-name>",
    "location": "<service-unit-location>",
    "apiVersion": "2018-09-01-preview",
    "dependsOn": [
        "<service>"
    ],
    "tags": {
        "serviceType": "Service West US Web App"
    },
    "properties": {
        "targetResourceGroup": "<resource-group-name>",
        "deploymentMode": "Incremental",
        "artifacts": {
            "templateArtifactSourceRelativePath": "<relative-path-to-template>",
            "parametersArtifactSourceRelativePath": "<relative-path-to-parameter-file>"
        }
    }
}
```

Каждый шаблон должен включать в себя связанные ресурсы, которые вы хотите развернуть за один шаг. Например, модуль службы может определять шаблон, который развертывает все ресурсы для внешнего интерфейса службы.

Дополнительные сведения см. в [справочнике по шаблону serviceUnits](/azure/templates/Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits).

## <a name="rollout-template"></a>Шаблон развертывания

Шаблон развертывания описывает действия, выполняемые при развертывании службы. Следует указать используемую топологию службы и определить порядок развертывания модулей службы. В шаблоне указывается источник артефактов для хранения двоичных файлов, необходимых для развертывания. В шаблоне развертывания следует определить следующую иерархию.

* Источник артефактов.
* Шаг
* Развертывание:
  * группы шагов;
    * операции развертывания.

На рисунке ниже показана иерархия шаблона развертывания.

![Иерархия: от развертывания к отдельным шагам](./media/deployment-manager-overview/Rollout.png)

Каждое развертывание может включать в себя несколько групп шагов. Каждая группа шагов содержит одну операцию развертывания, которая указывает на модуль службы в топологии службы.

### <a name="artifact-source-for-binaries"></a>Источник артефактов для двоичных файлов

В шаблоне развертывания следует создать источник артефактов для двоичных файлов, которые необходимо развернуть в службе. Он аналогичен [источнику артефактов для шаблонов](#artifact-source-for-templates), за исключением того, что содержит сценарии, веб-страницы, скомпилированный код или другие файлы, необходимые для вашей службы.

### <a name="steps"></a>Действия

Вы можете определить шаг, выполняемый до или после операции развертывания. В настоящее время только `wait` шаг и шаг «healthCheck» доступны.

Шаге wait приостанавливает развертывания, после чего его можно продолжить. Это позволяет проверить, что правильно ли работает служба, прежде чем развертывать следующий модуль службы. В следующем примере показан общий формат шага wait.

```json
{
    "apiVersion": "2018-09-01-preview",
    "type": "Microsoft.DeploymentManager/steps",
    "name": "waitStep",
        "location": "<step-location>",
    "properties": {
        "stepType": "wait",
        "attributes": {
          "duration": "PT1M"
        }
    }
},
```

Свойство duration использует [стандарт ISO 8601](https://en.wikipedia.org/wiki/ISO_8601#Durations). Предыдущий пример указывает длительность ожидания в 1 минуту.

Дополнительные сведения о на этапе проверки работоспособности, см. в разделе [вводят работоспособности интеграции развертывания для диспетчера развертывания Azure](./deployment-manager-health-check.md) и [руководства: Use health check in Azure Deployment Manager](./deployment-manager-tutorial-health-check.md) (Учебник по использованию проверки работоспособности в диспетчере развертывания Azure).

Дополнительные сведения см. в [справочнике по шаблону steps](/azure/templates/Microsoft.DeploymentManager/steps).

### <a name="rollouts"></a>Выпуски

Чтобы обеспечить доступность источника артефактов, развертывание зависит от него. Развертывание определяет группы шагов для каждого модуля службы, который развертывается. Можно определить действия, выполняемые до или после развертывания. Например, можно приостановить развертывание после развертывания модуля службы. Можно определить порядок групп шагов.

Объект identity указывает [управляемое удостоверение, назначаемое пользователем](#identity-and-access), которое используется для выполнения действий по развертыванию.

В следующем примере показан общий формат развертывания.

```json
{
    "type": "Microsoft.DeploymentManager/rollouts",
    "name": "<rollout-name>",
    "location": "<rollout-location>",
    "apiVersion": "2018-09-01-preview",
    "Identity": {
        "type": "userAssigned",
        "identityIds": [
            "<managed-identity-ID>"
        ]
    },
    "dependsOn": [
        "<artifact-source>"
    ],
    "properties": {
        "buildVersion": "1.0.0.0",
        "artifactSourceId": "<artifact-source-ID>",
        "targetServiceTopologyId": "<service-topology-ID>",
        "stepGroups": [
            {
                "name": "stepGroup1",
                "dependsOnStepGroups": ["<step-group-name>"],
                "preDeploymentSteps": ["<step-ID>"],
                "deploymentTargetId":
                    "<service-unit-ID>",
                "postDeploymentSteps": ["<step-ID>"]
            },
            ...
        ]
    }
}
```

Дополнительные сведения см. в [справочнике по шаблону rollouts](/azure/templates/Microsoft.DeploymentManager/rollouts).

## <a name="parameter-file"></a>Файл параметров

Следует создать два файла параметров. Один файл параметров используется при развертывании топологии службы, а другой — при развертывании ее ресурсов. Некоторые значения в обоих файлах параметров должны быть одинаковыми.

## <a name="containerroot-variable"></a>Переменная containerRoot

При развертывании с управлением версиями путь к артефактам изменяется с выпуском каждой новой версии. Сначала, при развертывании, это может быть путь `https://<base-uri-blob-container>/binaries/1.0.0.0`. Во второй раз это может быть путь `https://<base-uri-blob-container>/binaries/1.0.0.1`. Диспетчер развертывания упрощает получение правильного корневого пути для текущего развертывания с помощью переменной `$containerRoot`. Это значение изменяется для каждой версии, оно неизвестно до выполнения развертывания.

Используйте переменную `$containerRoot` в файле параметров для шаблона, чтобы развертывать ресурсы Azure. Во время развертывания эта переменная заменяется фактическими значениями из развертывания.

Например, во время развертывания создается источник артефактов для двоичных артефактов.

```json
{
    "type": "Microsoft.DeploymentManager/artifactSources",
    "name": "[variables('rolloutArtifactSource').name]",
    "location": "[parameters('azureResourceLocation')]",
    "apiVersion": "2018-09-01-preview",
    "properties": {
        "sourceType": "AzureStorage",
        "artifactRoot": "[parameters('binaryArtifactRoot')]",
        "authentication" :
        {
            "type": "SAS",
            "properties": {
                "sasUri": "[parameters('artifactSourceSASLocation')]"
            }
        }
    }
},
```

Обратите внимание на свойства `artifactRoot` и `sasUri`. Для корня артефактов можно задать значение, например `binaries/1.0.0.0`. Универсальный код ресурса (URI) SAS — это универсальный код ресурса (URI) контейнера хранилища с маркером SAS для доступа. Диспетчер развертывания автоматически составляет значение переменной `$containerRoot`. Он объединяет эти значения в формате `<container>/<artifactRoot>`.

Файл шаблона и файл параметров должны содержать правильный путь для получения двоичных файлов с управлением версиями. Например, для развертывания файлов веб-приложения создайте приведенный ниже файл параметров с переменной $containerRoot. Необходимо использовать две обратные косые черты (`\\`) в пути, так как первым указывается escape-символ.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "deployPackageUri": {
            "value": "$containerRoot\\helloWorldWebAppWUS.zip"
        }
    }
}
```

Затем используйте этот параметр в шаблоне.

```json
{
    "name": "MSDeploy",
    "type": "extensions",
    "location": "[parameters('location')]",
    "apiVersion": "2015-08-01",
    "dependsOn": [
        "[concat('Microsoft.Web/sites/', parameters('WebAppName'))]"
    ],
    "tags": {
        "displayName": "WebAppMSDeploy"
    },
    "properties": {
        "packageUri": "[parameters('deployPackageURI')]"
    }
}
```

Для управления развертываниями с управлением версиями создаются новые папки, пути к которым передаются в качестве корня во время развертывания. Этот путь передается в шаблон, который развертывает ресурсы.

## <a name="next-steps"></a>Дальнейшие действия

В этой статье вы узнали о диспетчере развертывания. Перейдите к следующей статье, чтобы узнать, как выполнить развертывание с помощью диспетчера развертывания.

> [!div class="nextstepaction"]
> [Руководство. Использование диспетчера развертывания Azure с шаблонами Resource Manager (закрытая предварительная версия)](./deployment-manager-tutorial.md)