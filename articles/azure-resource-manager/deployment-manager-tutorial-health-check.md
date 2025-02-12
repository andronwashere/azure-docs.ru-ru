---
title: Работа с шаблонами Resource Manager в диспетчере развертываний Azure | Документация Майкрософт
description: Использование шаблонов Resource Manager с помощью диспетчера развертывания Azure для развертывания ресурсов Azure.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.date: 05/31/2019
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 0e8a9fefdf92f568001cc3352fe83a85157acf9a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442582"
---
# <a name="tutorial-use-health-check-in-azure-deployment-manager-public-preview"></a>Руководство по использованию проверки работоспособности в диспетчере развертывания Azure (общедоступная предварительная версия)

Узнайте, как интегрировать проверку работоспособности в [диспетчер развертывания Azure](./deployment-manager-overview.md). Это руководство основано на руководстве [Использование диспетчера развертывания Azure с шаблонами Resource Manager (закрытая предварительная версия)](./deployment-manager-tutorial.md). Прежде чем начать работу с этим руководством, необходимо завершить работу с тем руководством.

В шаблоне выпуска, который применяется в руководстве [Использование диспетчера развертывания Azure с шаблонами Resource Manager (закрытая предварительная версия)](./deployment-manager-tutorial.md), вы использовали шаг ожидания. В этом руководстве вы замените шаг ожидания шагом проверки работоспособности.

> [!IMPORTANT]
> Если ваша подписка помечена для раннего выпуска, что позволяет тестировать новые функции Azure, вы можете использовать диспетчер развертывания Azure только для развертывания в регионах, поддерживающих ранний выпуск. 

В рамках этого руководства рассматриваются следующие задачи:

> [!div class="checklist"]
> * Создание симулятора службы проверки работоспособности.
> * Изменение шаблона выпуска.
> * Развертывание топологии.
> * Развертывание выпуска с неработоспособным состоянием.
> * Проверка развертывания выпуска.
> * Развертывание выпуска с работоспособным состоянием.
> * Проверка развертывания выпуска.
> * Очистка ресурсов

Дополнительные ресурсы:

- [Справочник по REST API диспетчера развертывания Azure](https://docs.microsoft.com/rest/api/deploymentmanager/).
- [Пример диспетчера развертывания Azure](https://github.com/Azure-Samples/adm-quickstart).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="prerequisites"></a>Предварительные требования

Для работы с этой статьей необходимо иметь следующее.

* Завершите работу с руководством по [использованию диспетчера развертывания Azure с шаблонами Resource Manager](./deployment-manager-tutorial.md).
* Скачайте [шаблоны и артефакты](https://armtutorials.blob.core.windows.net/admtutorial/ADMTutorial.zip), используемые в этом руководстве.

## <a name="create-a-health-check-service-simulator"></a>Создание симулятора службы проверки работоспособности

В рабочей среде обычно используется один или несколько поставщиков мониторинга. Чтобы максимально упростить интеграцию функций работоспособности, корпорация Майкрософт сотрудничает с некоторыми ведущими компаниями по мониторингу работоспособности служб, чтобы предоставить вам простое решение для копирования и вставки, позволяющее интегрировать проверки работоспособности с вашими развертываниями. Список этих компаний см. в разделе [Поставщики мониторинга работоспособности](./deployment-manager-health-check.md#health-monitoring-providers). В рамках этого руководства вы создадите [функцию Azure](/azure/azure-functions/) для имитации службы мониторинга работоспособности. Эта функция принимает код состояния и возвращает тот же код. В шаблоне диспетчера развертывания Azure используется код состояния, чтобы определить способ выполнения развертывания.

Следующие два файла используются для развертывания функции Azure. Не нужно скачивать эти файлы для работы с этим руководством.

* Шаблон Resource Manager, который находится по адресу [https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json](https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json). Вы развернете этот шаблон, чтобы создать функцию Azure.
* ZIP-файл исходного кода функции Azure, который находится по адресу [https://armtutorials.blob.core.windows.net/admtutorial/ADMHCFunction0417.zip](https://armtutorials.blob.core.windows.net/admtutorial/ADMHCFunction0417.zip). Этот ZIP-файл вызывается шаблоном Resource Manager.

Чтобы развернуть функцию Azure, выберите **Попробовать**, чтобы открыть Azure Cloud Shell, а затем вставьте следующий скрипт в окно оболочки.  Чтобы вставить код, щелкните окно оболочки правой кнопкой мыши, а затем выберите **Вставить**.

> [!IMPORTANT]
> Параметр **projectName** в скрипте PowerShell используется, чтобы создавать имена для служб Azure, которые будут развернуты в этом руководстве. Различные службы Azure имеют различные требования к именам. Чтобы убедиться, что развертывание прошло успешно, выберите имя длиной не более 12 символов, состоящее только со строчных букв и цифр.
> Сохраните копию имени проекта. Во время работы с этим руководством вы используете то же значение projectName.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/deploy_hc_azure_function.json" -projectName $projectName
```

Чтобы проверить функцию Azure, выполните следующее.

1. Откройте [портал Azure](https://portal.azure.com).
1. Откройте группу ресурсов.  Имя по умолчанию — имя проекта с добавлением **rg**.
1. В группе ресурсов выберите службу приложений.  Имя службы приложений по умолчанию — имя проекта с добавлением **webapp**.
1. Разверните **Функции**, а затем выберите **HttpTrigger1**.

    ![Проверка работоспособности функции Azure в диспетчере развертывания Azure](./media/deployment-manager-tutorial-health-check/azure-deployment-manager-hc-function.png)

1. Выберите **&lt;/> Получить URL-адрес функции**.
1. Выберите **Копировать**, чтобы скопировать URL-адрес в буфер обмена.  URL-адрес должен быть следующего вида:

    ```url
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/{healthStatus}?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    ```

    Замените `{healthStatus}` в URL-адресе на код состояния. В рамках этого руководства используйте состояние **unhealthy**, чтобы протестировать сценарий неработоспособности, и состояние **healthy** или состояние **warning**, чтобы протестировать сценарий работоспособности. Создайте два URL-адреса: один с неработоспособным состоянием, а другой — с работоспособным. Примеры приведены ниже.

    ```url
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    https://myhc0417webapp.azurewebsites.net/api/healthStatus/healthy?code=hc4Y1wY4AqsskAkVw6WLAN1A4E6aB0h3MbQ3YJRF3XtXgHvooaG0aw==
    ```

    Чтобы завершить работу с этим руководством, необходимы оба URL-адреса.

1. Чтобы протестировать симулятор мониторинга работоспособности, откройте URL-адреса, созданные на последнем шаге.  Результаты для неработоспособного состояния должны иметь следующий вид:

    ```
    Status: unhealthy
    ```

## <a name="revise-the-rollout-template"></a>Изменение шаблона выпуска

В этом разделе описано, как включить шаг проверки работоспособности в шаблон выпуска. Для завершения работы с этим руководством не нужно создавать собственный файл CreateADMRollout.json. К измененному шаблону выпуска предоставляется общий доступ в учетной записи хранения, которая используется в последующих разделах.

1. Откройте файл **CreateADMRollout.json**. Этот JSON-файл является частью загрузки.  См. раздел [Предварительные требования](#prerequisites).
1. Добавьте еще два параметра:

    ```json
    "healthCheckUrl": {
        "type": "string",
        "metadata": {
            "description": "Specifies the health check URL."
        }
    },
    "healthCheckAuthAPIKey": {
        "type": "string",
        "metadata": {
            "description": "Specifies the health check Azure Function function authorization key."
        }
    }
    ```

1. Замените определение ресурса шага ожидания на определение ресурса шага проверки работоспособности:

    ```json
    {
      "type": "Microsoft.DeploymentManager/steps",
      "apiVersion": "2018-09-01-preview",
      "name": "healthCheckStep",
      "location": "[parameters('azureResourceLocation')]",
      "properties": {
        "stepType": "healthCheck",
        "attributes": {
          "waitDuration": "PT0M",
          "maxElasticDuration": "PT0M",
          "healthyStateDuration": "PT1M",
          "type": "REST",
          "properties": {
            "healthChecks": [
              {
                "name": "appHealth",
                "request": {
                  "method": "GET",
                  "uri": "[parameters('healthCheckUrl')]",
                  "authentication": {
                    "type": "ApiKey",
                    "name": "code",
                    "in": "Query",
                    "value": "[parameters('healthCheckAuthAPIKey')]"
                  }
                },
                "response": {
                  "successStatusCodes": [
                    "200"
                  ],
                  "regex": {
                    "matches": [
                      "Status: healthy",
                      "Status: warning"
                    ],
                    "matchQuantifier": "Any"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

    В зависимости от определения, развертывание продолжается, если состоянием работоспособности — *healthy* или *warning*.

1. Обновите параметр **dependsON** определения выпуска так, чтобы включить новый заданный шаг проверки работоспособности:

    ```json
    "dependsOn": [
        "[resourceId('Microsoft.DeploymentManager/artifactSources', variables('rolloutArtifactSource').name)]",
        "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
    ],
    ```

1. Обновите параметр **stepGroups**, чтобы включить шаг проверки работоспособности. **healthCheckStep** вызывается в **postDeploymentSteps** группы **stepGroup2**. **stepGroup3** и **stepGroup4** развертываются только, если состоянием работоспособности — *healthy* или *warning*.

    ```json
    "stepGroups": [
        {
            "name": "stepGroup1",
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup2",
            "dependsOnStepGroups": ["stepGroup1"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceWUS.name,  variables('serviceTopology').serviceWUS.serviceUnit1.name)]",
            "postDeploymentSteps": [
                {
                    "stepId": "[resourceId('Microsoft.DeploymentManager/steps/', 'healthCheckStep')]"
                }
            ]
        },
        {
            "name": "stepGroup3",
            "dependsOnStepGroups": ["stepGroup2"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit2.name)]",
            "postDeploymentSteps": []
        },
        {
            "name": "stepGroup4",
            "dependsOnStepGroups": ["stepGroup3"],
            "preDeploymentSteps": [],
            "deploymentTargetId": "[resourceId('Microsoft.DeploymentManager/serviceTopologies/services/serviceUnits', variables('serviceTopology').name, variables('serviceTopology').serviceEUS.name,  variables('serviceTopology').serviceEUS.serviceUnit1.name)]",
            "postDeploymentSteps": []
        }
    ]
    ```

    Если вы сравните раздел **stepGroup3** до и после его изменения, вы увидите, что теперь этот раздел зависит от **stepGroup2**.  Это необходимо, если **stepGroup3** и последующие группы шагов зависят от результатов мониторинга работоспособности.

    На следующем снимке экрана показаны измененные области и то, как используется шаг проверки работоспособности:

    ![Шаблон проверки работоспособности диспетчера развертывания Azure](./media/deployment-manager-tutorial-health-check/azure-deployment-manager-hc-rollout-template.png)

## <a name="deploy-the-topology"></a>Развертывание топологии

Чтобы упростить работу с руководством, шаблон топологии и артефакты опубликованы в следующих общих расположениях, поэтому вам не нужно подготавливать собственную копию. Если вы хотите использовать собственную, следуйте указаниям в статье [Руководство. Использование диспетчера развертывания Azure с шаблонами Resource Manager (закрытая предварительная версия)](./deployment-manager-tutorial.md).

* Шаблон топологии: https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplates/CreateADMServiceTopology.json
* Хранилище артефактов: https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore

Чтобы развернуть топологию, выберите **Попробовать** для открытия Cloud Shell, а затем вставьте скрипт PowerShell.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$resourceGroupName = "${projectName}rg"
$artifactLocation = "https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore?st=2019-05-06T03%3A57%3A31Z&se=2020-05-07T03%3A57%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=gOh%2Bkhi693rmdxiZFQ9xbKZMU1kbLJDqXw7EP4TaGlI%3D" | ConvertTo-SecureString -AsPlainText -Force

# Create the service topology
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMServiceTopology.json" `
    -namePrefix $projectName `
    -azureResourceLocation $location `
    -artifactSourceSASLocation $artifactLocation
```

Убедитесь, что топология службы и выделенные ресурсы были успешно созданы с использованием портала Azure:

![Руководство по использованию диспетчера развертывания Azure: развернутые ресурсы шаблона топологии](./media/deployment-manager-tutorial/azure-deployment-manager-tutorial-deployed-topology-resources.png)

Для просмотра ресурсов необходимо выбрать **Показать скрытые типы**.

## <a name="deploy-the-rollout-with-the-unhealthy-status"></a>Развертывание выпуска с неработоспособным состоянием

Чтобы упростить работу с руководством, измененный шаблон выпуска опубликован в следующих общих расположениях, поэтому вам не нужно подготавливать собственную копию. Если вы хотите использовать собственную, следуйте указаниям в статье [Руководство. Использование диспетчера развертывания Azure с шаблонами Resource Manager (закрытая предварительная версия)](./deployment-manager-tutorial.md).

* Шаблон топологии: https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMRollout.json
* Хранилище артефактов: https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore

Используйте URL-адрес неработоспособного состояния, созданный в разделе [Создание симулятора службы проверки работоспособности](#create-a-health-check-service-simulator). Для параметра **managedIdentityID** см. сведения в разделе [Создание управляемого удостоверения, назначаемого пользователем](./deployment-manager-tutorial.md#create-the-user-assigned-managed-identity).

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$managedIdentityID = Read-Host -Prompt "Enter a user-assigned managed identity"
$healthCheckUrl = Read-Host -Prompt "Enter the health check Azure function URL"
$healthCheckAuthAPIKey = $healthCheckUrl.Substring($healthCheckUrl.IndexOf("?code=")+6, $healthCheckUrl.Length-$healthCheckUrl.IndexOf("?code=")-6)
$healthCheckUrl = $healthCheckUrl.Substring(0, $healthCheckUrl.IndexOf("?"))

$resourceGroupName = "${projectName}rg"
$artifactLocation = "https://armtutorials.blob.core.windows.net/admtutorial/ArtifactStore?st=2019-05-06T03%3A57%3A31Z&se=2020-05-07T03%3A57%3A00Z&sp=rl&sv=2018-03-28&sr=c&sig=gOh%2Bkhi693rmdxiZFQ9xbKZMU1kbLJDqXw7EP4TaGlI%3D" | ConvertTo-SecureString -AsPlainText -Force

# Create the rollout
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateUri "https://armtutorials.blob.core.windows.net/admtutorial/ADMTemplatesHC/CreateADMRollout.json" `
    -namePrefix $projectName `
    -azureResourceLocation $location `
    -artifactSourceSASLocation $artifactLocation `
    -managedIdentityID $managedIdentityID `
    -healthCheckUrl $healthCheckUrl `
    -healthCheckAuthAPIKey $healthCheckAuthAPIKey
```

> [!NOTE]
> `New-AzResourceGroupDeployment` выполняет асинхронный вызов. Сообщение об успешном выполнении означает только то, что развертывание начато успешно. Чтобы проверить развертывание, используйте `Get-AZDeploymentManagerRollout`.  Перейдите к следующей процедуре.

Чтобы проверить выполнение развертывания, используйте следующий скрипт PowerShell:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name used earlier in this tutorial"
$resourceGroupName = "${projectName}rg"
$rolloutName = "${projectName}Rollout"

# Get the rollout status
Get-AzDeploymentManagerRollout `
    -ResourceGroupName $resourceGroupName `
    -Name $rolloutName `
    -Verbose
```

В следующем примере выходных данных показано, что развертывание завершилось сбоем из-за неработоспособного состояния:

```output
Service: myhc0417ServiceWUSrg
    TargetLocation: WestUS
    TargetSubscriptionId: <Subscription ID>

    ServiceUnit: myhc0417ServiceWUSWeb
        TargetResourceGroup: myhc0417ServiceWUSrg

        Step: RestHealthCheck/healthCheckStep.PostDeploy
            Status: Failed
            StepGroup: stepGroup2
            Operation Info:
                Start Time: 05/06/2019 17:58:31
                End Time: 05/06/2019 17:58:32
                Total Duration: 00:00:01
                Error:
                    Code: ResourceReportedUnhealthy
                    Message: Health checks failed as the following resources were unhealthy: '05/06/2019 17:58:32 UTC: Health check 'appHealth' failed with the following errors: Response from endpoint 'https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy' does not match the regex pattern(s): 'Status: healthy, Status: warning.'. Response content: "Status: unhealthy"..'.
Get-AzDeploymentManagerRollout :
Service: myhc0417ServiceWUSrg
    ServiceUnit: myhc0417ServiceWUSWeb
        Step: RestHealthCheck/healthCheckStep.PostDeploy
            Status: Failed
            StepGroup: stepGroup2
            Operation Info:
                Start Time: 05/06/2019 17:58:31
                End Time: 05/06/2019 17:58:32
                Total Duration: 00:00:01
                Error:
                    Code: ResourceReportedUnhealthy
                    Message: Health checks failed as the following resources were unhealthy: '05/06/2019 17:58:32 UTC: Health check 'appHealth' failed with the following errors: Response from endpoint 'https://myhc0417webapp.azurewebsites.net/api/healthStatus/unhealthy' does not match the regex pattern(s): 'Status: healthy, Status: warning.'. Response content: "Status: unhealthy"..'.
At line:1 char:1
+ Get-AzDeploymentManagerRollout `
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [Get-AzDeploymentManagerRollout], Exception
+ FullyQualifiedErrorId : RolloutFailed,Microsoft.Azure.Commands.DeploymentManager.Commands.GetRollout


ResourceGroupName       : myhc0417rg
BuildVersion            : 1.0.0.0
ArtifactSourceId        : /subscriptions/<Subscription ID>/resourceGroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/artifactSources/myhc0417ArtifactSourceRollout
TargetServiceTopologyId : /subscriptions/<Subscription ID>/resourceGroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/serviceTopologies/myhc0417ServiceTopology
Status                  : Failed
TotalRetryAttempts      : 0
Identity                : Microsoft.Azure.Commands.DeploymentManager.Models.PSIdentity
OperationInfo           : Microsoft.Azure.Commands.DeploymentManager.Models.PSRolloutOperationInfo
Services                : {myhc0417ServiceWUS, myhc0417ServiceWUSrg}
Name                    : myhc0417Rollout
Type                    : Microsoft.DeploymentManager/rollouts
Location                : centralus
Id                      : /subscriptions/<Subscription ID>/resourcegroups/myhc0417rg/providers/Mi
                          crosoft.DeploymentManager/rollouts/myhc0417Rollout
Tags                    :
```

После завершения развертывания выпуска вы увидите одну дополнительную группу ресурсов, созданную для западной части США.

## <a name="deploy-the-rollout-with-the-healthy-status"></a>Развертывание выпуска с работоспособным состоянием

Повторите действия из этого раздела, чтобы повторно развернуть выпуск с URL-адресом работоспособного состояния.  После завершения развертывания выпуска вы увидите еще одну группу ресурсов, созданную для восточной части США.

## <a name="verify-the-deployment"></a>Проверка развертывания

1. Откройте [портал Azure](https://portal.azure.com).
2. Перейдите к новым веб-приложениям в новых группах ресурсов, созданных в результате развертывания выпуска.
3. Откройте веб-приложение в веб-браузере. Проверьте местоположение и версию в файле index.html.

## <a name="clean-up-resources"></a>Очистка ресурсов

Если ресурсы Azure больше не нужны, их можно удалить. Для этого необходимо удалить группу ресурсов.

1. На портале Azure в меню слева выберите **Группа ресурсов**.
2. Используйте поле **Фильтровать по имени**, чтобы сузить группы ресурсов, созданные в этом руководстве. Их должно быть от 3 до 4:

    * **&lt;namePrefix>rg**: содержит ресурсы диспетчера развертывания.
    * **&lt;namePrefix>ServiceWUSrg**: содержит ресурсы, определенные в ServiceWUS.
    * **&lt;namePrefix>ServiceEUSrg**: содержит ресурсы, определенные в ServiceEUS.
    * Группа ресурсов для определяемого пользователем управляемого удостоверения.
3. Выберите имя группы ресурсов.
4. В главном меню выберите **Удалить группу ресурсов**.
5. Повторите последние два шага для удаления других групп ресурсов, созданных в этом руководстве.

## <a name="next-steps"></a>Дополнительная информация

В этом руководстве вы узнали, как использовать функцию проверки работоспособности диспетчера развертывания Azure. Дополнительные сведения см. в [документации по Azure Resource Manager](/azure/azure-resource-manager/).
