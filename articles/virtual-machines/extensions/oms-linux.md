---
title: Расширение виртуальной машины Azure Monitor для Linux | Документация Майкрософт
description: Развертывание агента Log Analytics на виртуальной машине Linux с помощью расширения виртуальной машины.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: c7bbf210-7d71-4a37-ba47-9c74567a9ea6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: roiyz
ms.openlocfilehash: 7d8192a3b6ff732481a4d48f6e188b4bb3989cda
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705934"
---
# <a name="azure-monitor-virtual-machine-extension-for-linux"></a>Расширение виртуальной машины Azure Monitor для Linux

## <a name="overview"></a>Обзор

Журналы Azure Monitor предоставляет возможности исправления наблюдения, оповещений и предупреждений для облачных и локальных ресурсов. Расширение виртуальной машины агента Log Analytics для Linux предоставляет и поддерживает корпорация Майкрософт. Это расширение устанавливает агент Log Analytics на виртуальных машинах Azure и регистрирует виртуальные машины в существующей рабочей области Log Analytics. В этом документе описаны поддерживаемые платформы, конфигурации и параметры развертывания для расширения виртуальной машины Azure Monitor для Linux.

>[!NOTE]
>В рамках текущей передачи материалов из Microsoft Operations Management Suite (OMS) в Azure Monitor агент OMS для операционных систем будет называться агентом Log Analytics для Windows и Log Analytics для Linux.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="prerequisites"></a>предварительные требования

### <a name="operating-system"></a>Операционная система

Расширение агента Log Analytics можно выполнять с использованием указанных ниже дистрибутивов Linux.

| Дистрибутив | Version |
|---|---|
| CentOS Linux | 6 (x86 или x64) и 7 (x64) |
| Amazon Linux | 2017.09 (x64) | 
| Oracle Linux | 6 и 7 (x86/x64) |
| Сервер Red Hat Enterprise Linux | 6 (x86 или x64) и 7 (x64) |
| Debian GNU/Linux | 8 и 9 (x86/x64) |
| Ubuntu | 14.04 LTS (x86/x64), 16.04 LTS (x86/x64) и 18.04 LTS (x64) |
| SUSE Linux Enterprise Server | 12 (x 64) и 15 (x 64) |

>[!NOTE]
>OpenSSL ниже версии 1.x не поддерживается на любой платформе, а версия 1.10 поддерживается только на платформах x86_64 (64-разрядная версия).  
>

### <a name="agent-prerequisites"></a>Предварительные требования к агенту

В следующей таблице выделены пакеты, необходимые для поддерживаемых дистрибутивов Linux, которые агент будет устанавливаться на.

|Требуемый пакет |Описание |Минимальная версия |
|-----------------|------------|----------------|
|Glibc |    Библиотека C GNU | 2.5-12 
|Openssl    | Библиотеки OpenSSL | 1.0.x или 1.1.x |
|Curl | Веб-клиент cURL | 7.15.5 |
|Python-ctypes | | 
|PAM | Подключаемые модули аутентификации | | 

>[!NOTE]
>Для сбора сообщений системного журнала требуется rsyslog или syslog-ng. Управляющая программа syslog по умолчанию не поддерживается для сбора событий системного журнала в Red Hat Enterprise Linux версии 5, CentOS и Oracle Linux (sysklog). Чтобы собирать данные системного журнала из дистрибутивов этих версий, требуется установить и настроить управляющую программу rsyslog, которая заменит sysklog.

### <a name="agent-and-vm-extension-version"></a>Версия агента и расширения виртуальной машины
Следующая таблица предоставляет сопоставление версии расширения виртуальной Машины Azure Monitor и пакетов агента Log Analytics для каждого выпуска. В ней также указана ссылка на заметки о выпуске для версии пакета агента Log Analytics. Заметки о выпуске содержат сведения об исправлениях ошибок и новых функциях, доступных в данном выпуске агента.  

| Версия расширения Azure Monitor виртуальной Машины Linux | Версия пакета агента Log Analytics | 
|--------------------------------|--------------------------|
| 1.11.9 | [1.11.0-7](https://github.com/microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.11.0-7) |
| 1.10.0 | [1.10.0-1](https://github.com/microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.10.0-1) |
| 1.9.1 | [1.9.0-0](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.9.0-0) |
| 1.8.11 | [1.8.1-256](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.8.1.256)| 
| 1.8.0 | [1.8.0-256](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/1.8.0-256)| 
| 1.7.9 | [1.6.1–3](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.1.3)| 
| 1.6.42.0 | [1.6.0–42](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_v1.6.0-42)| 
| 1.4.60.2 | [1.4.4-210](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.4-210)| 
| 1.4.59.1 | [1.4.3–174](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.3-174)|
| 1.4.58.7 | [14.2–125](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-125)|
| 1.4.56.5 | [1.4.2–124](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.2-124)|
| 1.4.55.4 | [1.4.1–123](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-123)|
| 1.4.45.3 | [1.4.1–45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.1-45)|
| 1.4.45.2 | [1.4.0-45](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent_GA_v1.4.0-45)|
| 1.3.127.5 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)| 
| 1.3.127.7 | [1.3.5-127](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201705-v1.3.5-127)|
| 1.3.18.7 | [1.3.4-15](https://github.com/Microsoft/OMS-Agent-for-Linux/releases/tag/OMSAgent-201704-v1.3.4-15)|  

### <a name="azure-security-center"></a>Центр безопасности Azure

Центр безопасности Azure автоматически подготавливает агент Log Analytics и подключает его к рабочей области Log Analytics по умолчанию, созданной ASC в подписке Azure. Если вы используете центр безопасности Azure, не выполняйте инструкции в этом документе. Это приведет к перезаписи настроенной рабочей области и разрыву подключения с центром безопасности Azure.

### <a name="internet-connectivity"></a>Подключение к Интернету

Для расширения агента Log Analytics для Linux требуется, чтобы целевая виртуальная машина была подключена к Интернету. 

## <a name="extension-schema"></a>Схема расширения

В следующем объекте JSON показана схема для расширения агента Log Analytics. Расширение требует идентификатор и ключ целевой рабочей области Log Analytics, которые можно найти в [рабочей области Log Analytics](../../azure-monitor/learn/quick-collect-linux-computer.md#obtain-workspace-id-and-key) на портале Azure. Так как ключ рабочей области должен рассматриваться в качестве конфиденциальных данных, его следует хранить в защищенной конфигурации параметров. Данные защищенных параметров расширения виртуальной машины Azure зашифрованы. Они расшифровываются только на целевой виртуальной машине. Обратите внимание, что в **workspaceId** и **workspaceKey** учитывается регистр знаков.

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

>[!NOTE]
>В приведенной выше схеме предполагается, что он будет размещен на корневом уровне шаблона. Если поместить его в ресурс виртуальной машины в шаблоне, свойства `type` и `name` следует изменить, как это описано [далее](#template-deployment).
>

### <a name="property-values"></a>Значения свойств

| ИМЯ | Значение и пример |
| ---- | ---- |
| apiVersion | 2018-06-01 |
| publisher | Microsoft.EnterpriseCloud.Monitoring |
| type | OmsAgentForLinux |
| typeHandlerVersion | 1.7 |
| workspaceID (пример) | 6f680a37-00c6-41c7-a93f-1437e3462574 |
| workspaceKey (пример) | z4bU3p1/GrnWpQkky4gdabWXAhbWSTz70hm4m2Xt92XI+rSRgE8qVvRhsGo9TXffbrTahyrwv35W0pOqQAU7uQ== |


## <a name="template-deployment"></a>Развертывание шаблона

Расширения виртуальной машины Azure можно развернуть с помощью шаблонов Azure Resource Manager. Шаблоны идеально подходят для развертывания виртуальных машин, требующих настройки после развертывания, например подключения к журналам Azure Monitor. Пример шаблона Resource Manager, включающего в себя расширение виртуальной машины агента Log Analytics, можно найти в [коллекции быстрого запуска Azure](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-ubuntu-vm). 

Конфигурацию JSON для расширения виртуальной машины можно вложить в ресурс виртуальной машины или поместить в корень или на верхний уровень JSON-файла шаблона Resource Manager. Размещение конфигурации JSON влияет на значения имени и типа ресурса. Дополнительные сведения см. в разделе [Указание имени и типа дочернего ресурса в шаблоне Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md#child-resources). 

В следующем примере предполагается, что расширение виртуальной машины вложено в ресурс виртуальной машины. При вложении ресурса расширения JSON помещается в объект `"resources": []` виртуальной машины.

```json
{
  "type": "extensions",
  "name": "OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

При размещении JSON расширения в корне шаблона имя ресурса содержит ссылку на родительскую виртуальную машину, а тип отражает вложенную конфигурацию.  

```json
{
  "type": "Microsoft.Compute/virtualMachines/extensions",
  "name": "<parentVmResource>/OMSExtension",
  "apiVersion": "2018-06-01",
  "location": "<location>",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', <vm-name>)]"
  ],
  "properties": {
    "publisher": "Microsoft.EnterpriseCloud.Monitoring",
    "type": "OmsAgentForLinux",
    "typeHandlerVersion": "1.7",
    "settings": {
      "workspaceId": "myWorkspaceId"
    },
    "protectedSettings": {
      "workspaceKey": "myWorkSpaceKey"
    }
  }
}
```

## <a name="azure-cli-deployment"></a>Развертывание с помощью Azure CLI

Azure CLI можно использовать для развертывания расширения виртуальной машины агента Log Analytics на существующей виртуальной машине. Замените *workspaceId* и *workspaceKey* идентификатором и ключом своей рабочей области Log Analytics. 

```azurecli
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.7 --protected-settings '{"workspaceKey": "omskey"}' \
  --settings '{"workspaceId": "omsid"}'
```

## <a name="troubleshoot-and-support"></a>Устранение неполадок и поддержка

### <a name="troubleshoot"></a>Устранение неполадок

Данные о состоянии развертывания расширения можно получить на портале Azure, а также использовав Azure CLI. Чтобы просмотреть состояние развертывания расширений для определенной виртуальной машины, выполните следующую команду в Azure CLI.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

Выходные данные выполнения расширения регистрируются в следующем файле:

```
/opt/microsoft/omsagent/bin/stdout
```

### <a name="error-codes-and-their-meanings"></a>Коды ошибок и их описание

| Код ошибки | Значение | Возможное действие |
| :---: | --- | --- |
| 9 | Преждевременный вызов операции включения | [Обновите агент Azure Linux](https://docs.microsoft.com/azure/virtual-machines/linux/update-agent) до новейшей версии. |
| 10 | Виртуальная машина уже подключена к рабочей области Log Analytics | Для подключения виртуальной машины к рабочей области, указанной в схеме расширения, задайте для stopOnMultipleConnections значение false в общих параметрах или удалите это свойство. Счет для этой виртуальной машины выставляется за каждую рабочую область, к которой она подключена. |
| 11 | Для расширения предоставлена недопустимая конфигурация | Изучите приведенные выше примеры, чтобы задать все значения свойств, необходимые для развертывания. |
| 17 | Сбой установки пакета Log Analytics | 
| 19 | Сбой установки пакета OMI | 
| 20 | Сбой установки пакета SCX |
| 51 | Это расширение не поддерживается в операционной системе виртуальной машины | |
| 55 | Не удается подключиться к службе Azure Monitor или необходимые пакеты заблокирован отсутствует или dpkg диспетчера пакетов| Убедитесь, что система имеет доступ к Интернету или что предоставлен допустимый прокси-сервер HTTP. Кроме того проверьте правильность идентификатора рабочей области и убедитесь, что установлены служебные программы curl и tar. |

Дополнительные сведения об устранении неполадок см. в [руководстве по устранению неполадок агента Log Analytics для Linux](../../azure-monitor/platform/vmext-troubleshoot.md).

### <a name="support"></a>Поддержка

Если в любой момент при изучении этой статьи вам потребуется дополнительная помощь, вы можете обратиться к экспертам по Azure на [форумах MSDN Azure и Stack Overflow](https://azure.microsoft.com/support/forums/). Кроме того, можно зарегистрировать обращение в службу поддержки Azure. Перейдите на [сайт поддержки Azure](https://azure.microsoft.com/support/options/) и щелкните "Получить поддержку". Дополнительные сведения об использовании службы поддержки Azure см. в статье [Часто задаваемые вопросы о поддержке Microsoft Azure](https://azure.microsoft.com/support/faq/).
