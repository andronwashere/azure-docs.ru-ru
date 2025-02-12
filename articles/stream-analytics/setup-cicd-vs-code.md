---
title: Непрерывная интеграция и разработка с помощью Azure Stream Analytics Непрерывной интеграции и пакета npm
description: В этой статье описывается использование Azure Stream Analytics Непрерывной интеграции и пакета npm для настройки непрерывной интеграции и процесса развертывания.
services: stream-analytics
author: su-jie
ms.author: sujie
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/15/2019
ms.openlocfilehash: fa5a57afa379c6bbe027be80f400fc176800d289
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66158505"
---
# <a name="continuously-integrate-and-develop-with-stream-analytics-cicd-npm-package"></a>Непрерывная интеграция и разработка с помощью Stream Analytics Непрерывной интеграции и пакета npm
В этой статье описывается использование пакета npm для Azure Stream Analytics Непрерывной интеграции и настройке непрерывной интеграции и процесса развертывания.

## <a name="build-the-vs-code-project"></a>Постройте проект VS Code

Вы можете включить непрерывную интеграцию и развертывание для заданий Azure Stream Analytics, с помощью **asa-streamanalytics-cicd** пакета npm. Пакет npm предоставляет средства для формирования шаблонов Azure Resource Manager из [проектов Stream Analytics Visual Studio Code](quick-create-vs-code.md). Его можно использовать в Windows, macOS и Linux без установки Visual Studio Code.

При наличии [пакет](https://www.npmjs.com/package/azure-streamanalytics-cicd), используйте следующую команду для вывода шаблонов Azure Resource Manager. **ScriptPath** аргумент — это абсолютный путь к **asaql** файл в проекте. Убедитесь, что asaproj.json и JobConfig.json файлы находятся в той же папке с файлом сценария. Если **outputPath** не указан, шаблоны будут помещены в **развернуть** папку к проекту **bin** папки.

```powershell
azure-streamanalytics-cicd build -scriptPath <scriptFullPath> -outputPath <outputPath>
```
Пример (в macOS)
```powershell
azure-streamanalytics-cicd build -scriptPath "/Users/roger/projects/samplejob/script.asaql" 
```

При успешном выполнении сборки проекта Stream Analytics Visual Studio Code, он создает следующие два файла шаблона Azure Resource Manager в разделе **bin / [Debug/Retail] / Deploy** папку: 

*  файл шаблона Resource Manager;

       [ProjectName].JobTemplate.json 

*  файл параметров Resource Manager.

       [ProjectName].JobTemplate.parameters.json   

Параметры по умолчанию в файле parameters.json, на основе параметров в проекте Visual Studio Code. При необходимости развертывания в другой среде замените соответствующие параметры.

> [!NOTE]
> Для всех учетных данных значения по умолчанию имеют значение NULL. Их **необходимо** установить перед развертыванием в облаке.

```json
"Input_EntryStream_sharedAccessPolicyKey": {
      "value": null
    },
```
Узнайте, как [развернуть файл шаблона Resource Manager с помощью Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Узнайте, как [использовать объект в качестве параметра в шаблоне Resource Manager](https://docs.microsoft.com/azure/architecture/building-blocks/extending-templates/objects-as-parameters).

Чтобы в качестве приемника выходных данных использовать управляемое удостоверение для Azure Data Lake Storage 1-го поколения, предоставите доступ субъекту-службе с помощью PowerShell перед развертыванием в Azure. Дополнительные сведения см. в разделе о [развертывании ADLS 1-го поколения с управляемым удостоверением с помощью шаблона Resource Manager](stream-analytics-managed-identities-adls.md#resource-manager-template-deployment).
## <a name="next-steps"></a>Дальнейшие действия

* [Краткое руководство Создать задание Azure Stream Analytics cloud в Visual Studio Code (Предварительная версия)](quick-create-vs-code.md)
* [Тестирование запросов Stream Analytics локально с помощью Visual Studio Code (Предварительная версия)](vscode-local-run.md)
* [Знакомство с Azure Stream Analytics с помощью Visual Studio Code (Предварительная версия)](vscode-explore-jobs.md)
