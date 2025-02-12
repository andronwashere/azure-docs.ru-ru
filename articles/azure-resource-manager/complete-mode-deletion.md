---
title: Удаление ресурсов в полном режиме в Azure Resource Manager по типу ресурса
description: Здесь показано, как происходит удаление ресурсов в полном режиме в шаблонах Azure Resource Manager по типу ресурса.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 04/24/2019
ms.author: tomfitz
ms.openlocfilehash: 5ac442c0ae1e397fd1e8b58fdbcf61eb8712046c
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302845"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Удаление ресурсов Azure для развертываний в полном режиме
В этой статье описывается, как процесс удаления будет обрабатываться различными типами, когда он не происходит в шаблоне, развернутом в завершенном режиме.

Типы ресурсов, помеченные как `Yes`, будут удалены, если их тип отсутствует в шаблоне, развернутом в завершенном режиме. 

Типы ресурсов, помеченные как `No`, не будут удалены автоматически, если они отсутствуют в шаблоне, однако они будут удалены после удаления родительского ресурса. Чтобы получить полное описание поведения см. статью [Режимы развертывания в Azure Resource Manager](deployment-modes.md).


## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | DomainServices | Да | 
> | DomainServices/oucontainer | Нет | 

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | diagnosticSettings | Нет | 
> | diagnosticSettingsCategories | Нет | 

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | supportProviders | Нет | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | aadsupportcases | Нет | 
> | addsservices | Нет | 
> | agents | Нет | 
> | anonymousapiusers | Нет | 
> | настройка | Нет | 
> | logs | Нет | 
> | reports | Нет | 
> | services; | Нет | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | конфигурации | Нет | 
> | generateRecommendations | Нет | 
> | к просмотру фильмов | Нет | 
> | suppressions | Нет | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | actionRules | Нет | 
> | оповещения | Нет | 
> | alertsList | Нет | 
> | alertsSummary | Нет | 
> | alertsSummaryList | Нет | 
> | smartDetectorAlertRules | Нет | 
> | smartDetectorRuntimeEnvironments | Нет | 
> | smartGroups | Нет | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | servers | Да | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | reportFeedback | Нет | 
> | служба | Да | 
> | validateServiceName | Нет | 

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | attestationProviders | Нет | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | classicAdministrators | Нет | 
> | denyAssignments | Нет | 
> | elevateAccess | Нет | 
> | locks | Нет | 
> | permissions | Нет | 
> | policyAssignments | Нет | 
> | policyDefinitions | Нет | 
> | policySetDefinitions | Нет | 
> | providerOperations | Нет | 
> | roleAssignments | Нет | 
> | roleDefinitions | Нет | 

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | automationAccounts | Да | 
> | automationAccounts/configurations | Да | 
> | automationAccounts/jobs | Нет | 
> | automationAccounts/runbooks | Да | 
> | automationAccounts/softwareUpdateConfigurations | Нет | 
> | automationAccounts/webhooks | Нет | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | environments | Нет | 
> | environments/accounts | Нет | 
> | environments/accounts/namespaces | Нет | 
> | environments/accounts/namespaces/configurations | Нет | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | b2cDirectories | Да | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | registrations | Да | 
> | registrations/customerSubscriptions | Нет | 
> | registrations/products | Нет | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | batchAccounts | Да | 

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | billingAccounts | Нет | 
> | billingAccounts/billingProfiles | Нет | 
> | billingAccounts/billingProfiles/billingSubscriptions | Нет | 
> | billingAccounts/billingProfiles/invoices | Нет | 
> | billingAccounts/billingProfiles/invoices/pricesheet | Нет | 
> | billingAccounts/billingProfiles/operationStatus | Нет | 
> | billingAccounts/billingProfiles/paymentMethods | Нет | 
> | billingAccounts/billingProfiles/policies | Нет | 
> | billingAccounts/billingProfiles/pricesheet | Нет | 
> | billingAccounts/billingProfiles/products | Нет | 
> | billingAccounts/billingProfiles/transactions | Нет | 
> | billingAccounts/billingSubscriptions | Нет | 
> | billingAccounts/departments | Нет | 
> | billingAccounts/eligibleOffers | Нет | 
> | billingAccounts/enrollmentAccounts | Нет | 
> | billingAccounts/invoices | Нет | 
> | billingAccounts/invoiceSections | Нет | 
> | billingAccounts/invoiceSections/billingSubscriptions | Нет | 
> | billingAccounts/invoiceSections/billingSubscriptions/transfer | Нет | 
> | billingAccounts/invoiceSections/importRequests | Нет | 
> | billingAccounts/invoiceSections/initiateImportRequest | Нет | 
> | billingAccounts/invoiceSections/initiateTransfer | Нет | 
> | billingAccounts/invoiceSections/operationStatus | Нет | 
> | billingAccounts/invoiceSections/products | Нет | 
> | billingAccounts/invoiceSections/transfers | Нет | 
> | billingAccounts/products | Нет | 
> | billingAccounts/projects | Нет | 
> | billingAccounts/projects/billingSubscriptions | Нет | 
> | billingAccounts/projects/importRequests | Нет | 
> | billingAccounts/projects/initiateImportRequest | Нет | 
> | billingAccounts/projects/operationStatus | Нет | 
> | billingAccounts/projects/products | Нет | 
> | billingAccounts/transactions | Нет | 
> | billingPeriods | Нет | 
> | BillingPermissions | Нет | 
> | billingProperty | Нет | 
> | BillingRoleAssignments | Нет | 
> | BillingRoleDefinitions | Нет | 
> | CreateBillingRoleAssignment | Нет | 
> | departments | Нет | 
> | enrollmentAccounts | Нет | 
> | importRequests | Нет | 
> | importRequests/acceptImportRequest | Нет | 
> | importRequests/declineImportRequest | Нет | 
> | invoices | Нет | 
> | transfers | Нет | 
> | transfers/acceptTransfer | Нет | 
> | transfers/declineTransfer | Нет | 
> | transfers/operationStatus | Нет | 
> | usagePlans | Нет | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | mapApis | Да | 
> | updateCommunicationPreference | Нет | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | BizTalk | Да | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | blueprintAssignments | Нет | 
> | blueprintAssignments/assignmentOperations | Нет | 
> | blueprintAssignments/operations | Нет | 
> | blueprints | Нет | 
> | blueprints/artifacts | Нет | 
> | blueprints/versions | Нет | 
> | blueprints/versions/artifacts | Нет | 

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | botServices | Да | 
> | botServices/channels | Нет | 
> | botServices/connections | Нет | 

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | Redis | Да | 
> | RedisConfigDefinition | Нет | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | appliedReservations | Нет | 
> | calculatePrice | Нет | 
> | catalogs | Нет | 
> | commercialReservationOrders | Нет | 
> | reservationOrders | Нет | 
> | reservationOrders/calculateRefund | Нет | 
> | reservationOrders/merge | Нет | 
> | reservationOrders/reservations | Нет | 
> | reservationOrders/reservations/revisions | Нет | 
> | reservationOrders/return | Нет | 
> | reservationOrders/split | Нет | 
> | reservationOrders/swap | Нет | 
> | reservations | Нет | 
> | ресурсов | Нет | 
> | validateReservationOrder | Нет | 

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | edgenodes | Нет | 
> | profiles | Да | 
> | profiles/endpoints | Да | 
> | profiles/endpoints/customdomains | Нет | 
> | profiles/endpoints/origins | Нет | 
> | validateProbe | Нет | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | certificateOrders | Да | 
> | certificateOrders/certificates | Нет | 
> | validateCertificateRegistrationInformation | Нет | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | capabilities | Нет | 
> | domainNames | Нет | 
> | domainNames/capabilities | Нет | 
> | domainNames/internalLoadBalancers | Нет | 
> | domainNames/serviceCertificates | Нет | 
> | domainNames/slots | Нет | 
> | domainNames/slots/roles | Нет | 
> | moveSubscriptionResources | Нет | 
> | operatingSystemFamilies | Нет | 
> | operatingSystems | Нет | 
> | quotas | Нет | 
> | resourceTypes | Нет | 
> | validateSubscriptionMoveAvailability | Нет | 
> | virtualMachines | Нет | 
> | virtualMachines/diagnosticSettings | Нет | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | classicInfrastructureResources | Нет | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | capabilities | Нет | 
> | expressRouteCrossConnections | Нет | 
> | expressRouteCrossConnections/peerings | Нет | 
> | gatewaySupportedDevices | Нет | 
> | networkSecurityGroups | Нет | 
> | quotas | Нет | 
> | reservedIps | Нет | 
> | virtualNetworks | Нет | 
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | Нет | 
> | virtualNetworks/virtualNetworkPeerings | Нет | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | capabilities | Нет | 
> | disks | Нет | 
> | images | Нет | 
> | osImages | Нет | 
> | osPlatformImages | Нет | 
> | publicImages | Нет | 
> | quotas | Нет | 
> | storageAccounts | Нет | 
> | storageAccounts/services | Нет | 
> | storageAccounts/services/diagnosticSettings | Нет | 
> | storageAccounts/vmImages | Нет | 
> | vmImages | Нет | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | accounts | Да | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | RateCard | Нет | 
> | UsageAggregates | Нет | 

## <a name="microsoftcompute"></a>Microsoft.Compute;

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | availabilitySets | Да | 
> | disks | Да | 
> | images | Да | 
> | restorePointCollections | Да | 
> | restorePointCollections/restorePoints | Нет | 
> | sharedVMImages | Да | 
> | sharedVMImages/versions | Да | 
> | snapshots | Да | 
> | virtualMachines | Да | 
> | virtualMachines/diagnosticSettings | Нет | 
> | virtualMachines/extensions | Да | 
> | virtualMachineScaleSets | Да | 
> | virtualMachineScaleSets/extensions | Нет | 
> | virtualMachineScaleSets/networkInterfaces | Нет | 
> | virtualMachineScaleSets/publicIPAddresses | Нет | 
> | virtualMachineScaleSets/virtualMachines | Нет | 
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | Нет | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | AggregatedCost | Нет | 
> | сведения о балансе. | Нет | 
> | сведения о бюджете; | Нет | 
> | Расходы | Нет | 
> | CostTags | Нет | 
> | credits | Нет | 
> | events | Нет | 
> | Прогнозы | Нет | 
> | lots | Нет | 
> | Marketplace | Нет | 
> | Pricesheets | Нет | 
> | products | Нет | 
> | ReservationDetails | Нет | 
> | ReservationRecommendations | Нет | 
> | ReservationSummaries | Нет | 
> | ReservationTransactions | Нет | 
> | Tags | Нет | 
> | Термины | Нет | 
> | UsageDetails | Нет | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | containerGroups | Да | 
> | serviceAssociationLinks | Нет | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | registries | Да | 
> | registries/builds | Нет | 
> | registries/builds/cancel | Нет | 
> | registries/builds/getLogLink | Нет | 
> | registries/buildTasks | Да | 
> | registries/buildTasks/steps | Нет | 
> | registries/eventGridFilters | Нет | 
> | registries/getBuildSourceUploadUrl | Нет | 
> | registries/GetCredentials | Нет | 
> | registries/importImage | Нет | 
> | registries/queueBuild | Нет | 
> | registries/regenerateCredential | Нет | 
> | registries/regenerateCredentials | Нет | 
> | registries/replications | Да | 
> | registries/runs | Нет | 
> | registries/runs/cancel | Нет | 
> | registries/scheduleRun | Нет | 
> | registries/tasks | Да | 
> | registries/updatePolicies | Нет | 
> | registries/webhooks | Да | 
> | registries/webhooks/getCallbackConfig | Нет | 
> | registries/webhooks/ping | Нет | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | containerServices | Да | 
> | managedClusters | Да | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | веб-масштабированием; | Да | 
> | updateCommunicationPreference | Нет | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | accounts | Да | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | Предупреждения | Нет | 
> | BillingAccounts | Нет | 
> | Соединители | Да | 
> | Departments | Нет | 
> | Измерения | Нет | 
> | EnrollmentAccounts | Нет | 
> | Запрос | Нет | 
> | регистрация | Нет | 
> | Reportconfigs | Нет | 
> | Отчеты | Нет | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | hubs | Да | 
> | hubs/authorizationPolicies | Нет | 
> | hubs/connectors | Нет | 
> | hubs/connectors/mappings | Нет | 
> | hubs/interactions | Нет | 
> | hubs/kpi | Нет | 
> | hubs/links | Нет | 
> | hubs/profiles | Нет | 
> | hubs/roleAssignments | Нет | 
> | hubs/roles | Нет | 
> | hubs/suggestTypeSchema | Нет | 
> | hubs/views | Нет | 
> | hubs/widgetTypes | Нет | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | jobs | Да | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Да | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | workspaces | Да | 
> | workspaces/virtualNetworkPeerings | Нет | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | catalogs | Да | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | connectionManagers | Да | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | dataFactories | Да | 
> | dataFactories/diagnosticSettings | Нет | 
> | dataFactorySchema | Нет | 
> | factories | Да | 
> | factories/integrationRuntimes | Нет | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | accounts | Да | 
> | accounts/dataLakeStoreAccounts | Нет | 
> | accounts/storageAccounts | Нет | 
> | accounts/storageAccounts/containers | Нет | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | accounts | Да | 
> | accounts/eventGridFilters | Нет | 
> | accounts/firewallRules | Нет | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | services; | Да | 
> | services/projects | Да | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | servers | Да | 
> | servers/recoverableServers | Нет | 
> | servers/virtualNetworkRules | Нет | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | servers | Да | 
> | servers/recoverableServers | Нет | 
> | servers/virtualNetworkRules | Нет | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | servers | Да | 
> | servers/advisors | Нет | 
> | servers/queryTexts | Нет | 
> | servers/recoverableServers | Нет | 
> | servers/topQueryStatistics | Нет | 
> | servers/virtualNetworkRules | Нет | 
> | servers/waitStatistics | Нет | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | IotHubs | Да | 
> | IotHubs/eventGridFilters | Нет | 
> | ProvisioningServices | Да | 
> | usages | Нет | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | controllers | Да | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | labs | Да | 
> | labs/serviceRunners | Да | 
> | labs/virtualMachines | Да | 
> | schedules | Да | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | databaseAccountNames | Нет | 
> | databaseAccounts | Да | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | domains | Да | 
> | domains/domainOwnershipIdentifiers | Нет | 
> | generateSsoRequest | Нет | 
> | topLevelDomains | Нет | 
> | validateDomainRegistrationInformation | Нет | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | lcsprojects | Нет | 
> | lcsprojects/clouddeployments | Нет | 
> | lcsprojects/connectors | Нет | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | domains | Да | 
> | domains/topics | Нет | 
> | eventSubscriptions | Нет | 
> | extensionTopics | Нет | 
> | topics | Да | 
> | topicTypes | Нет | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | clusters | Да | 
> | namespaces | Да | 
> | namespaces/authorizationrules | Нет | 
> | namespaces/disasterrecoveryconfigs | Нет | 
> | namespaces/eventhubs | Нет | 
> | namespaces/eventhubs/authorizationrules | Нет | 
> | namespaces/eventhubs/consumergroups | Нет | 

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | features | Нет | 
> | providers | Нет | 

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | зарегистрировать | Нет | 
> | galleryitems | Нет | 
> | generateartifactaccessuri | Нет | 
> | myareas | Нет | 
> | myareas/areas | Нет | 
> | myareas/areas/areas | Нет | 
> | myareas/areas/areas/galleryitems | Нет | 
> | myareas/areas/galleryitems | Нет | 
> | myareas/galleryitems | Нет | 
> | регистрация | Нет | 
> | ресурсов | Нет | 
> | retrieveresourcesbyid | Нет | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | guestConfigurationAssignments | Нет | 
> | software | Нет | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | hanaInstances | Да | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | clusters | Да | 
> | clusters/applications | Нет | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | jobs | Да | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | labelGroups | Нет | 
> | labelGroups/labels | Нет | 
> | labelGroups/labels/conditions | Нет | 
> | labelGroups/labels/subLabels | Нет | 
> | labelGroups/labels/subLabels/conditions | Нет | 

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | actiongroups | Да | 
> | activityLogAlerts | Да | 
> | alertrules | Да | 
> | automatedExportSettings | Нет | 
> | autoscalesettings | Да | 
> | baseline | Нет | 
> | calculatebaseline | Нет | 
> | components | Да | 
> | components/events | Нет | 
> | components/pricingPlans | Нет | 
> | components/query | Нет | 
> | diagnosticSettings | Нет | 
> | diagnosticSettingsCategories | Нет | 
> | eventCategories | Нет | 
> | eventtypes | Нет | 
> | extendedDiagnosticSettings | Нет | 
> | logDefinitions | Нет | 
> | logprofiles | Нет | 
> | logs | Нет | 
> | metricAlerts | Да |
> | migrateToNewPricingModel | Нет | 
> | myWorkbooks | Нет | 
> | Запросы | Нет | 
> | rollbackToLegacyPricingModel | Нет | 
> | scheduledqueryrules | Да | 
> | vmInsightsOnboardingStatuses | Нет | 
> | webtests | Да | 
> | workbooks | Да | 

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | diagnosticSettings | Нет | 
> | diagnosticSettingsCategories | Нет | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | IoTApps | Да | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | График | Да | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | deletedVaults | Нет | 
> | vaults | Да | 
> | vaults/accessPolicies | Нет | 
> | vaults/secrets | Нет | 

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | clusters | Да | 
> | clusters/databases | Нет | 
> | clusters/databases/dataconnections | Нет | 
> | clusters/databases/eventhubconnections | Нет | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | labaccounts | Да | 
> | users | Нет | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | accounts | Да | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | accounts | Да | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | logs | Нет | 

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | integrationAccounts | Да | 
> | workflows | Да | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | commitmentPlans | Да | 
> | webServices | Да | 
> | Рабочие области | Да | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | accounts | Да | 
> | accounts/workspaces | Да | 
> | accounts/workspaces/projects | Да | 
> | teamAccounts | Да | 
> | teamAccounts/workspaces | Да | 
> | teamAccounts/workspaces/projects | Да | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | accounts | Да | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | workspaces | Да | 
> | workspaces/computes | Нет | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | Identities | Нет | 
> | userAssignedIdentities | Да | 

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | getEntities | Нет | 
> | managementGroups | Нет | 
> | ресурсов | Нет | 
> | startTenantBackfill | Нет | 
> | tenantBackfillStatus | Нет | 

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | accounts | Да | 
> | accounts/eventGridFilters | Нет | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | offers | Нет | 
> | offerTypes | Нет | 
> | offerTypes/publishers | Нет | 
> | offerTypes/publishers/offers | Нет | 
> | offerTypes/publishers/offers/plans | Нет | 
> | offerTypes/publishers/offers/plans/agreements | Нет | 
> | offerTypes/publishers/offers/plans/configs | Нет | 
> | offerTypes/publishers/offers/plans/configs/importImage | Нет | 
> | privategalleryitems | Нет | 
> | products | Нет | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | classicDevServices | Да | 
> | updateCommunicationPreference | Нет | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | agreements | Нет | 
> | offertypes | Нет | 

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | mediaservices | Да | 
> | mediaservices/accountFilters | Нет | 
> | mediaservices/assets | Нет | 
> | mediaservices/assets/assetFilters | Нет | 
> | mediaservices/contentKeyPolicies | Нет | 
> | mediaservices/eventGridFilters | Нет | 
> | mediaservices/liveEventOperations | Нет | 
> | mediaservices/liveEvents | Да | 
> | mediaservices/liveEvents/liveOutputs | Нет | 
> | mediaservices/liveOutputOperations | Нет | 
> | mediaservices/streamingEndpointOperations | Нет | 
> | mediaservices/streamingEndpoints | Да | 
> | mediaservices/streamingLocators | Нет | 
> | mediaservices/streamingPolicies | Нет | 
> | mediaservices/transforms | Нет | 
> | mediaservices/transforms/jobs | Нет | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | projects | Да | 

## <a name="microsoftnetwork"></a>Microsoft.Network.

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | applicationGateways | Да | 
> | applicationSecurityGroups | Да | 
> | azureFirewallFqdnTags | Нет | 
> | azureFirewalls | Да | 
> | bgpServiceCommunities | Нет | 
> | connections | Да | 
> | ddosCustomPolicies | Да | 
> | ddosProtectionPlans | Да | 
> | dnsOperationStatuses | Нет | 
> | dnszones | Да | 
> | dnszones/A | Нет | 
> | dnszones/AAAA | Нет | 
> | dnszones/all | Нет | 
> | dnszones/CAA | Нет | 
> | dnszones/CNAME | Нет | 
> | dnszones/MX | Нет | 
> | dnszones/NS | Нет | 
> | dnszones/PTR | Нет | 
> | dnszones/recordsets | Нет | 
> | dnszones/SOA | Нет | 
> | dnszones/SRV | Нет | 
> | dnszones/TXT | Нет | 
> | expressRouteCircuits | Да | 
> | expressRouteServiceProviders | Нет | 
> | frontdoors | Да | 
> | frontdoorWebApplicationFirewallPolicies | Да | 
> | getDnsResourceReference | Нет | 
> | interfaceEndpoints | Да | 
> | internalNotify | Нет | 
> | loadBalancers | Да | 
> | localNetworkGateways | Да | 
> | natGateways | Да | 
> | networkIntentPolicies | Да | 
> | networkInterfaces | Да | 
> | networkProfiles | Да | 
> | networkSecurityGroups | Да | 
> | networkWatchers | Да | 
> | networkWatchers/connectionMonitors | Да | 
> | networkWatchers/lenses | Да | 
> | networkWatchers/pingMeshes | Да | 
> | privateLinkServices | Да | 
> | publicIPAddresses | Да | 
> | publicIPPrefixes | Да | 
> | routeFilters | Да | 
> | routeTables | Да | 
> | serviceEndpointPolicies | Да | 
> | trafficManagerGeographicHierarchies | Нет | 
> | trafficmanagerprofiles | Да | 
> | trafficmanagerprofiles/heatMaps | Нет | 
> | virtualHubs | Да | 
> | virtualNetworkGateways | Да | 
> | virtualNetworks | Да | 
> | virtualNetworkTaps | Да | 
> | virtualWans | Да | 
> | vpnGateways | Да | 
> | vpnSites | Да | 
> | webApplicationFirewallPolicies | Да | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | namespaces | Да | 
> | namespaces/notificationHubs | Да | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | устройства | Нет | 
> | linkTargets | Нет | 
> | storageInsightConfigs | Нет | 
> | workspaces | Да | 
> | workspaces/dataSources | Нет | 
> | workspaces/linkedServices | Нет | 
> | workspaces/query | Нет | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | managementassociations | Нет | 
> | managementconfigurations | Да | 
> | solutions | Да | 
> | узел "Представления" | Да | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | policyEvents | Нет | 
> | policyStates | Нет | 
> | policyTrackedResources | Нет | 
> | remediations | Нет | 

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | consoles | Нет | 
> | dashboards | Да | 
> | userSettings | Нет | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | workspaceCollections | Да | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | capacities | Да | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | accounts | Да | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | backupProtectedItems | Нет | 
> | vaults | Да | 

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | namespaces | Да | 
> | namespaces/authorizationrules | Нет | 
> | namespaces/hybridconnections | Нет | 
> | namespaces/hybridconnections/authorizationrules | Нет | 
> | namespaces/wcfrelays | Нет | 
> | namespaces/wcfrelays/authorizationrules | Нет | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | ресурсов | Нет | 
> | subscriptionsStatus | Нет | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | availabilityStatuses | Нет | 
> | childAvailabilityStatuses | Нет | 
> | childResources | Нет | 
> | events | Нет | 
> | impactedResources | Нет | 
> | Уведомления | Нет | 

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | deployments | Нет | 
> | deployments/operations | Нет | 
> | links | Нет | 
> | notifyResourceJobs | Нет | 
> | providers | Нет | 
> | resourceGroups | Нет | 
> | ресурсов | Нет | 
> | subscriptions | Нет | 
> | subscriptions/providers | Нет | 
> | subscriptions/resourceGroups | Нет | 
> | subscriptions/resourcegroups/resources | Нет | 
> | subscriptions/resources | Нет | 
> | subscriptions/tagnames | Нет | 
> | subscriptions/tagNames/tagValues | Нет | 
> | tenants | Нет | 

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | веб-масштабированием; | Да | 
> | saasresources | Нет | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | flows | Да | 
> | jobcollections | Да | 

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | resourceHealthMetadata | Нет | 
> | searchServices | Да | 

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | advancedThreatProtectionSettings | Нет | 
> | оповещения | Нет | 
> | allowedConnections | Нет | 
> | appliances | Нет | 
> | applicationWhitelistings | Нет | 
> | AutoProvisioningSettings | Нет | 
> | Compliances | Нет | 
> | dataCollectionAgents | Нет | 
> | discoveredSecuritySolutions | Нет | 
> | externalSecuritySolutions | Нет | 
> | InformationProtectionPolicies | Нет | 
> | jitNetworkAccessPolicies | Нет | 
> | monitoring | Нет | 
> | monitoring/antimalware | Нет | 
> | monitoring/baseline | Нет | 
> | monitoring/patch | Нет | 
> | политики | Нет | 
> | pricings | Нет | 
> | securityContacts | Нет | 
> | securitySolutions | Нет | 
> | securitySolutionsReferenceData | Нет | 
> | securityStatus | Нет | 
> | securityStatus/endpoints | Нет | 
> | securityStatus/subnets | Нет | 
> | securityStatus/virtualMachines | Нет | 
> | securityStatuses | Нет | 
> | securityStatusesSummaries | Нет | 
> | Параметры | Нет | 
> | задачи; | Нет | 
> | topologies | Нет | 
> | workspaceSettings | Нет | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | diagnosticSettings | Нет | 
> | diagnosticSettingsCategories | Нет | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | namespaces | Да | 
> | namespaces/authorizationrules | Нет | 
> | namespaces/disasterrecoveryconfigs | Нет | 
> | namespaces/eventgridfilters | Нет | 
> | namespaces/queues | Нет | 
> | namespaces/queues/authorizationrules | Нет | 
> | namespaces/topics | Нет | 
> | namespaces/topics/authorizationrules | Нет | 
> | namespaces/topics/subscriptions | Нет | 
> | namespaces/topics/subscriptions/rules | Нет | 
> | premiumMessagingRegions | Нет | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | clusters | Да | 
> | clusters/applications | Нет | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | веб-масштабированием; | Да | 
> | gateways | Да | 
> | networks | Да | 
> | секретные коды | Да | 
> | volumes. | Да | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | SignalR | Да | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | applianceDefinitions | Да | 
> | appliances | Да | 
> | applicationDefinitions | Да | 
> | веб-масштабированием; | Да | 
> | jitRequests | Да | 

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | managedInstances | Да |
> | managedInstances/databases | Да |
> | managedInstances/databases/backupShortTermRetentionPolicies | Нет |
> | managedInstances/databases/schemas/tables/columns/sensitivityLabels | Нет |
> | managedInstances/databases/vulnerabilityAssessments | Нет |
> | managedInstances/databases/vulnerabilityAssessments/rules/baselines | Нет |
> | managedInstances/encryptionProtector | Нет |
> | managedInstances/keys | Нет |
> | managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Нет |
> | managedInstances/vulnerabilityAssessments | Нет |
> | servers | Да | 
> | servers/administrators | Нет | 
> | servers/communicationLinks | Нет | 
> | servers/databases | Да | 
> | servers/encryptionProtector | Нет | 
> | servers/firewallRules | Нет | 
> | servers/keys | Нет | 
> | servers/restorableDroppedDatabases | Нет | 
> | servers/serviceobjectives | Нет | 
> | servers/tdeCertificates | Нет | 


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Да | 
> | SqlVirtualMachineGroups/AvailabilityGroupListeners | Нет | 
> | SqlVirtualMachines | Да | 

## <a name="microsoftstorage"></a>Microsoft.Storage;

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | storageAccounts | Да | 
> | storageAccounts/blobServices | Нет | 
> | storageAccounts/fileServices | Нет | 
> | storageAccounts/queueServices | Нет | 
> | storageAccounts/services | Нет | 
> | storageAccounts/tableServices | Нет | 
> | usages | Нет | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | storageSyncServices | Да | 
> | storageSyncServices/registeredServers | Нет | 
> | storageSyncServices/syncGroups | Нет | 
> | storageSyncServices/syncGroups/cloudEndpoints | Нет | 
> | storageSyncServices/syncGroups/serverEndpoints | Нет | 
> | storageSyncServices/workflows | Нет | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | managers | Да | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | streamingjobs | Да | 
> | streamingjobs/diagnosticSettings | Нет | 

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | CreateSubscription | Нет | 
> | SubscriptionDefinitions | Нет | 
> | SubscriptionOperations | Нет | 

## <a name="microsoftsupport"></a>microsoft.support

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | supporttickets | Нет | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | providerRegistrations | Да | 
> | ресурсов | Да | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | environments | Да | 
> | environments/accessPolicies | Нет | 
> | environments/eventsources | Да | 
> | environments/referenceDataSets | Да | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | учетная запись | Да | 
> | account/extension | Да | 
> | account/project | Да | 

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | apiManagementAccounts | Нет | 
> | apiManagementAccounts/apiAcls | Нет | 
> | apiManagementAccounts/apis | Нет | 
> | apiManagementAccounts/apis/apiAcls | Нет | 
> | apiManagementAccounts/apis/connectionAcls | Нет | 
> | apiManagementAccounts/apis/connections | Нет | 
> | apiManagementAccounts/apis/connections/connectionAcls | Нет | 
> | apiManagementAccounts/apis/localizedDefinitions | Нет | 
> | apiManagementAccounts/connectionAcls | Нет | 
> | apiManagementAccounts/connections | Нет | 
> | billingMeters | Нет | 
> | certificates | Да | 
> | connectionGateways | Да | 
> | connections | Да | 
> | customApis | Да | 
> | deletedSites | Нет | 
> | functions | Нет | 
> | hostingEnvironments | Да | 
> | hostingEnvironments/multiRolePools | Нет | 
> | hostingEnvironments/multiRolePools/instances | Нет | 
> | hostingEnvironments/workerPools | Нет | 
> | hostingEnvironments/workerPools/instances | Нет | 
> | publishingUsers | Нет | 
> | к просмотру фильмов | Нет | 
> | resourceHealthMetadata | Нет | 
> | runtimes | Нет | 
> | serverFarms | Да | 
> | serverFarms/workers | Нет | 
> | sites | Да | 
> | sites/domainOwnershipIdentifiers | Нет | 
> | sites/hostNameBindings | Нет | 
> | sites/instances | Нет | 
> | sites/instances/extensions | Нет | 
> | sites/premieraddons | Да | 
> | sites/recommendations | Нет | 
> | sites/resourceHealthMetadata | Нет | 
> | sites/slots | Да | 
> | sites/slots/hostNameBindings | Нет | 
> | sites/slots/instances | Нет | 
> | sites/slots/instances/extensions | Нет | 
> | sourceControls | Нет | 
> | validate | Нет | 
> | verifyHostingEnvironmentVnet | Нет | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | diagnosticSettings | Нет | 
> | diagnosticSettingsCategories | Нет | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | DeviceServices | Да | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Тип ресурса | Удаление ресурсов в полном режиме |
> | ------------- | ----------- |
> | components | Нет | 
> | componentsSummary | Нет | 
> | monitorInstances | Нет | 
> | monitorInstancesSummary | Нет | 
> | monitors | Нет | 
> | notificationSettings | Нет | 

## <a name="next-steps"></a>Следующие шаги

Чтобы получить данные, идентичные значению файла с разделителями-запятыми, необходимо скачать [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv).