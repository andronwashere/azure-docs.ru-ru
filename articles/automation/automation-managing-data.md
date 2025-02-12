---
title: Управление данными службы автоматизации Azure
description: Эта статья содержит несколько разделов об управлении средой службы автоматизации Azure.  Сейчас включает в себя хранение данных и резервное копирование для аварийного восстановления службы автоматизации Azure.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: bobbytreed
ms.author: robreed
ms.date: 03/16/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9de5909ddca5fd36f3fafcb79e2a4ad519402c9c
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2019
ms.locfileid: "67476588"
---
# <a name="managing-azure-automation-data"></a>Управление данными службы автоматизации Azure
Эта статья содержит несколько разделов об управлении средой службы автоматизации Azure.

## <a name="data-retention"></a>Хранение данных
При удалении ресурса в службе автоматизации Azure он сохраняется еще 90 дней в целях аудита, прежде чем будет окончательно удален.  В течение этого времени его нельзя увидеть или использовать.  Эта политика применяется также к ресурсам, принадлежащим удаляемой учетной записи службы автоматизации.

Служба автоматизации Azure автоматически и окончательно удаляет задания старше 90 дней.

В следующей таблице кратко описана политика хранения для различных ресурсов.

| Data | Политика |
|:--- |:--- |
| учетные записи; |Окончательно удаляются через 90 дней после удаления учетной записи пользователем. |
| Активы |Окончательно удаляются через 90 дней после удаления актива пользователем или через 90 дней после удаления пользователем учетной записи, содержащей актив. |
| модули |Окончательно удаляются через 90 дней после удаления модуля пользователем или через 90 дней после удаления пользователем учетной записи, содержащей модуль. |
| Модули Runbook |Окончательно удаляются через 90 дней после удаления ресурса пользователем или через 90 дней после удаления пользователем учетной записи, содержащей ресурс. |
| Задания |Окончательно удаляются через 90 дней после последнего изменения. Это может быть после завершения, остановки или приостановки задания. |
| Конфигурации узлов и MOF-файлы |Старая конфигурация узла окончательно удаляется через 90 дней после создания новой конфигурации узла. |
| Узлы DSC |Окончательно удаляются через 90 дней после отмены регистрации узла в учетной записи автоматизации с помощью портала Azure или командлета [Unregister-AzureRMAutomationDscNode](https://docs.microsoft.com/powershell/module/azurerm.automation/unregister-azurermautomationdscnode) в Windows PowerShell. Узлы также окончательно удаляются через 90 дней после удаления содержащей узел учетной записи. |
| Отчеты узла |Окончательно удаляются через 90 дней после создания нового отчета для этого узла. |

Политика хранения применяется ко всем пользователям и в настоящее время не может быть изменена.

Тем не менее если вам нужно сохранить данные за длительный период времени, вы можете перенаправить журналы заданий runbook журналы Azure Monitor.  Для получения дополнительных сведений просмотрите [пересылка данных задания службы автоматизации Azure для журналов Azure Monitor](automation-manage-send-joblogs-log-analytics.md).   

## <a name="backing-up-azure-automation"></a>Резервное копирование службы автоматизации Azure
При удалении учетной записи службы автоматизации в Microsoft Azure все объекты в ней удаляются, включая модули Runbook, модули, параметры, конфигурации, задания и ресурсы. Объекты нельзя восстановить после удаления учетной записи.  Можно использовать следующую информацию, чтобы создать резервную копию содержимого учетной записи службы автоматизации перед его удалением. 

### <a name="runbooks"></a>Модули Runbook
Модули Runbook можно экспортировать в файлы сценариев с помощью портала Azure или командлета [Get-AzureAutomationRunbookDefinition](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationrunbookdefinition) в Windows PowerShell.  Эти файлы сценариев можно импортировать в другую учетную запись службы автоматизации, как описано в разделе [Создание или импорт модуля Runbook](/previous-versions/azure/dn643637(v=azure.100)).

### <a name="integration-modules"></a>Модули интеграции
Модули интеграции нельзя экспортировать из службы автоматизации Azure.  Необходимо обеспечить, чтобы они были доступны за пределами учетной записи службы автоматизации.

### <a name="assets"></a>Активы
Нельзя экспортировать [ресурсы](/previous-versions/azure/dn939988(v=azure.100)) из службы автоматизации Azure.  Используя портал Azure, вам необходимо записать сведения о переменных, учетных данных, сертификатах, подключениях и расписаниях.  Затем необходимо вручную создать все ресурсы, используемые модулями Runbook, которые импортируются в другую службу автоматизации.

Вы можете использовать [командлеты Azure](https://docs.microsoft.com/powershell/module/azurerm.automation#automation) , чтобы получить информацию о незашифрованных ресурсах и либо сохранить ее для дальнейшего использования, либо создать эквивалентные ресурсы в другой учетной записи службы автоматизации.

С помощью командлетов нельзя получить значение зашифрованных переменных или поле пароля учетных данных.  Если вы не знаете этих значений, то их можно получить из модуля Runbook с помощью действий [Get-AutomationVariable](/previous-versions/azure/dn940012(v=azure.100)) и [Get-AutomationPSCredential](/previous-versions/azure/dn940015(v=azure.100)).

Нельзя экспортировать сертификаты из службы автоматизации Azure.  Необходимо обеспечить, чтобы все сертификаты были доступны за пределами Azure.

### <a name="dsc-configurations"></a>Конфигурации DSC
Конфигурации можно экспортировать в файлы сценариев с помощью портала Azure или командлета [Export-AzureRmAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/azurerm.automation/export-azurermautomationdscconfiguration) в Windows PowerShell. Эти конфигурации можно импортировать и использовать в другой учетной записи автоматизации.

## <a name="geo-replication-in-azure-automation"></a>Георепликация в службе автоматизации Azure
Георепликация, которая обычно применяется в учетных записях службы автоматизации Azure, осуществляет резервное копирование данных учетной записи в другой географический регион для обеспечения избыточности. При настройке учетной записи можно выбрать основной регион, после чего дополнительный регион назначается автоматически. Дополнительные данные, скопированные из основного региона, постоянно обновляются на случай потери данных.  

В таблице ниже показаны доступные пары основных и дополнительных регионов.

| Главный | Вторичная |
| --- | --- |
| Центрально-южная часть США |Центрально-северная часть США |
| Восточная часть США 2 |Центральный регион США |
| Западная Европа |Северная Европа |
| Юго-Восточная Азия |Восточная Азия |
| Восточная часть Японии |Западная часть Японии |

В маловероятном случае потери данных основного региона корпорация Майкрософт попытается восстановить их. Если восстановить основные данные невозможно, выполняется географическая отработка отказа и затронутые клиенты получают уведомления об этом через свои подписки.


