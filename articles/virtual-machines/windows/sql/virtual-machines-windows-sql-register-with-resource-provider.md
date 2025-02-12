---
title: Регистрация SQL Server виртуальной машины в Azure с помощью поставщика ресурсов виртуальной машины SQL | Документация Майкрософт
description: Зарегистрируйте виртуальную машину SQL Server с помощью поставщика ресурсов виртуальной машины SQL, чтобы улучшить управляемость.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/24/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 95a167cbc8fde4488e3f46ffd850f0619cf1b651
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305868"
---
# <a name="register-sql-server-virtual-machine-in-azure-with-the-sql-vm-resource-provider"></a>Регистрация SQL Server виртуальной машины в Azure с помощью поставщика ресурсов виртуальной машины SQL

В этой статье описывается, как зарегистрировать виртуальную машину Azure SQL Server с помощью поставщика ресурсов виртуальной машины SQL. 

Развертывание образа SQL Server VM Marketplace с помощью портал Azure автоматически регистрирует SQL Server виртуальную машину с помощью поставщика ресурсов. Однако если вы решили самостоятельно установить SQL Server на виртуальной машине Azure вместо того, чтобы выбирать образ из Azure Marketplace, вы должны зарегистрировать виртуальную машину SQL Server в следующем поставщике ресурсов:

-  **Соответствие требованиям** . в соответствии с условиями продукта Майкрософт клиенты, использующие [преимущество гибридного использования Azure](https://azure.microsoft.com/pricing/hybrid-benefit/) , должны указать корпорации Майкрософт при использовании преимущество гибридного использования Azure. для этого они должны зарегистрироваться у поставщика ресурсов виртуальной машины SQL. 

-  **Преимущества функций** . Регистрация SQL Server виртуальной машины с помощью поставщика ресурсов разблокирует [Автоматическое исправление](virtual-machines-windows-sql-automated-patching.md), [Автоматическое резервное копирование](virtual-machines-windows-sql-automated-backup-v2.md), функции мониторинга и управления, а также гибкость [лицензирования](virtual-machines-windows-sql-ahb.md) и [выпуска](virtual-machines-windows-sql-change-edition.md) . Ранее они были доступны только для SQL Server образов виртуальных машин из Azure Marketplace.

Самостоятельная установка SQL Server на виртуальной машине Azure или Подготовка виртуальной машины Azure из пользовательского виртуального жесткого диска с SQL Server соответствует Преимущество гибридного использования Azure для SQL Server с условием, что клиенты указывают на использование в Майкрософт путем регистрации в ресурсе виртуальной машины SQL. поставщики. 

Чтобы использовать поставщик ресурсов виртуальной машины SQL, необходимо также зарегистрировать поставщик ресурсов виртуальной машины SQL в подписке. Это можно сделать с помощью портал Azure, Azure CLI и PowerShell. 

## <a name="prerequisites"></a>Предварительные требования

Чтобы зарегистрировать виртуальную машину SQL Server с поставщиком ресурсов, вам потребуется следующее: 

- [Подписка Azure](https://azure.microsoft.com/free/).
- [Виртуальная машина SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision). 
- [Azure CLI](/cli/azure/install-azure-cli) и [PowerShell](/powershell/azure/new-azureps-module-az). 

## <a name="register-with-sql-vm-resource-provider"></a>Регистрация с помощью поставщика ресурсов виртуальной машины SQL
Если [расширение IaaS для SQL](virtual-machines-windows-sql-server-agent-extension.md) уже установлено на виртуальной машине, то регистрация в поставщике ресурсов ВИРТУАЛЬНОЙ машины SQL выполняется просто путем создания ресурса метаданных типа Microsoft. Склвиртуалмачине/склвиртуалмачинес. Ниже приведен фрагмент кода для регистрации в поставщике ресурсов виртуальной машины SQL, если расширение IaaS для SQL уже установлено на виртуальной машине. Необходимо указать тип SQL Server лицензии при регистрации с помощью поставщика ресурсов виртуальной машины SQL: "PAYG" или "AHUB". 

Зарегистрируйте SQL Server виртуальную машину с помощью PowerShell, выполнив следующий фрагмент кода:

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register with SQL VM resource provider
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'}  
  
  ```

Если расширение IaaS для SQL не установлено на виртуальной машине, можно зарегистрировать его с помощью поставщика ресурсов виртуальной машины SQL, указав упрощенный режим управления SQL. В упрощенном режиме управления SQL поставщик ресурсов виртуальной машины SQL будет автоматическая установка расширения SQL IaaS в [упрощенном режиме](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) и проверка метаданных экземпляра SQL Server. Это не приведет к перезапуску службы SQL Server. Необходимо указать тип SQL Server лицензии при регистрации с помощью поставщика ресурсов виртуальной машины SQL: "PAYG" или "AHUB". 

Зарегистрируйте SQL Serverную виртуальную машину в упрощенном режиме управления SQL с помощью PowerShell, выполнив следующий фрагмент кода:

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
     # Register SQL VM with 'Lightweight' SQL IaaS agent
     New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
        -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
        -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB';sqlManagement='LightWeight'}  
  
  ```

Регистрация в поставщике ресурсов виртуальной машины SQL в [упрощенном режиме](virtual-machines-windows-sql-server-agent-extension.md#install-in-lightweight-mode) обеспечит соответствие требованиям и обеспечивает гибкие возможности лицензирования, а также на месте SQL Server обновлений выпуска. Экземпляры отказоустойчивого кластера и развертывания с несколькими экземплярами могут быть зарегистрированы в поставщике ресурсов виртуальной машины SQL только в упрощенном режиме. Вы можете следовать инструкциям, приведенным на портал Azure, чтобы выполнить обновление до [полного режима](virtual-machines-windows-sql-server-agent-extension.md#full-mode-installation) и обеспечить комплексный набор функций управления с SQL Server перезапуском в любое время. 

## <a name="register-sql-server-2008r2-on-windows-server-2008-vms"></a>Регистрация SQL Server 2008/R2 на виртуальных машинах Windows Server 2008

SQL Server 2008 и 2008 R2, установленные на Windows Server 2008, можно зарегистрировать в ресурсе виртуальной машины SQL в режиме "без [агента](virtual-machines-windows-sql-server-agent-extension.md) ". Этот параметр обеспечивает соответствие требованиям и позволяет отслеживать SQL Serverную виртуальную машину в портал Azure с ограниченными функциональными возможностями.

В следующей таблице приведены допустимые значения параметров, указанных во время регистрации.

| Параметр | Допустимые значения                                 |
| :------------------| :--------------------------------------- |
| **скллиценсетипе** | `'AHUB'`, или`'PAYG'`                    |
| **склимажеоффер**  | `'SQL2008-WS2008'` или `'SQL2008R2-WS2008`|
| &nbsp;             | &nbsp;                                   |


Чтобы зарегистрировать SQL Server 2008 или 2008 R2 в экземпляре Windows Server 2008, используйте следующий фрагмент кода PowerShell:  

  ```powershell-interactive
     # Get the existing  Compute VM
     $vm = Get-AzVM -Name <vm_name> -ResourceGroupName <resource_group_name>
          
    New-AzResource -Name $vm.Name -ResourceGroupName $vm.ResourceGroupName -Location $vm.Location `
      -ResourceType Microsoft.SqlVirtualMachine/SqlVirtualMachines `
      -Properties @{virtualMachineResourceId=$vm.Id;SqlServerLicenseType='AHUB'; `
       sqlManagement='NoAgent';sqlImageSku='Standard';sqlImageOffer='SQL2008R2-WS2008'}
  ```

## <a name="verify-registration-status"></a>Проверка состояния регистрации
Вы можете проверить, зарегистрирован ли SQL Server в поставщике ресурсов виртуальной машины SQL с помощью портал Azure, Azure CLI или PowerShell. 

### <a name="azure-portal"></a>Портал Azure 
Чтобы проверить состояние регистрации с помощью портал Azure, выполните следующие действия.

1. Войдите на [портал Azure](https://portal.azure.com). 
1. Перейдите к [виртуальным машинам SQL](virtual-machines-windows-sql-manage-portal.md).
1. Выберите SQL Server виртуальную машину из списка. Если ваша SQL Server виртуальная машина не указана здесь, скорее всего, SQL Server виртуальная машина не зарегистрирована в поставщике ресурсов виртуальной машины SQL. 
1. Просмотрите значение в разделе `Status`. Если `Status = Succeeded`задано значение, то SQL Server виртуальная машина успешно зарегистрирована в поставщике ресурсов виртуальной машины SQL. 

    ![Проверка состояния с регистрацией SQL RP](media/virtual-machines-windows-sql-register-with-rp/verify-registration-status.png)

### <a name="az-cli"></a>AZ CLI

Проверьте текущее состояние регистрации виртуальной машины SQL Server, выполнив следующую команду AZ CLI. `ProvisioningState`Отобразится `Succeeded` , если регистрация прошла успешно. 

  ```azurecli-interactive
  az sql vm show -n <vm_name> -g <resource_group>
  ```


### <a name="powershell"></a>PowerShell

Проверьте текущее состояние регистрации виртуальной машины SQL Server с помощью следующего командлета PowerShell. `ProvisioningState`Отобразится `Succeeded` , если регистрация прошла успешно. 

  ```powershell-interactive
  Get-AzResource -ResourceName <vm_name> -ResourceGroupName <resource_group> -ResourceType Microsoft.SqlVirtualMachine/sqlVirtualMachines
  ```
Ошибка указывает на то, что SQL Server виртуальная машина не зарегистрирована в поставщике ресурсов. 

---

## <a name="register-sql-vm-resource-provider-with-subscription"></a>Регистрация поставщика ресурсов виртуальной машины SQL с подпиской 

Чтобы зарегистрировать виртуальную машину SQL Server с поставщиком ресурсов виртуальной машины SQL, необходимо зарегистрировать поставщик ресурсов в подписке. Это можно сделать с помощью портал Azure или Azure CLI.

### <a name="azure-portal"></a>Портал Azure

Следующие шаги зарегистрируют поставщик ресурсов виртуальной машины SQL в подписке Azure с помощью портал Azure. 

1. Войдите на портал Azure и откройте раздел **Все службы**. 
1. Перейдите к разделу **Подписки** и выберите нужную подписку.  
1. На странице **подписки** перейдите к элементу **поставщики ресурсов**. 
1. Введите в фильтр значение `sql`, чтобы отсортировать поставщиков ресурсов, имеющих отношение к SQL. 
1. Выберите действие *Зарегистрировать*, *Перерегистрировать* или *Отменить регистрацию* для поставщика **Microsoft.SqlVirtualMachine** в зависимости от ваших намерений. 

   ![Изменение поставщика](media/virtual-machines-windows-sql-ahb/select-resource-provider-sql.png)

### <a name="az-cli"></a>AZ CLI
В следующем фрагменте кода поставщик ресурсов виртуальной машины SQL будет зарегистрирован в подписке Azure. 

```azurecli-interactive
# Register the new SQL VM resource provider to your subscription 
az provider register --namespace Microsoft.SqlVirtualMachine 
```

### <a name="powershell"></a>PowerShell

В следующем фрагменте кода PowerShell будет зарегистрирован поставщик ресурсов виртуальной машины SQL в подписке Azure.

```powershell-interactive
# Register the new SQL VM resource provider to your subscription
Register-AzResourceProvider -ProviderNamespace Microsoft.SqlVirtualMachine
```
---

## <a name="remarks"></a>Примечания

 - Поставщик ресурсов виртуальной машины SQL поддерживает только SQL Server виртуальных машин, развернутых с помощью "диспетчер ресурсов". SQL Server виртуальные машины, развернутые с помощью классической модели, не поддерживаются. 
 - Поставщик ресурсов виртуальной машины SQL поддерживает только SQL Server виртуальных машин, развернутых в общедоступном облаке. Развертывания в частном или правительственном облаке не поддерживаются. 
 
## <a name="frequently-asked-questions"></a>Часто задаваемые вопросы 

**Следует ли зарегистрировать виртуальную машину SQL Server, подготовленную из образа SQL в Azure Marketplace?**

Нет. Microsoft зарегистрирует виртуальные машины, подготовленные на основе образов SQL в Azure Marketplace. Регистрация в поставщике ресурсов виртуальной машины SQL требуется только в том случае, если виртуальная машина не была подготовлена из образов SQL в Azure Marketplace и SQL Server была самостоятельно установлена.

**Доступен ли поставщик ресурсов виртуальной машины SQL для всех клиентов?** 

Да. Клиенты должны зарегистрировать свою SQL Server виртуальную машину с помощью поставщика ресурсов виртуальной машины SQL, если они не использовали SQL Server образ из Azure Marketplace и самостоятельно установленный SQL Server или добавили свой пользовательский виртуальный жесткий диск. Виртуальные машины, принадлежащие всем типам подписок (Direct, EA и CSP), могут зарегистрироваться у поставщика ресурсов виртуальной машины SQL.

**Следует ли зарегистрироваться в поставщике ресурсов виртуальной машины SQL, если на моей SQL Server виртуальной машине уже установлено расширение SQL IaaS?**

Если виртуальная машина SQL Server самостоятельно установлена, а не подготовлена из образов SQL в Azure Marketplace, то следует зарегистрироваться в поставщике ресурсов виртуальной машины SQL, даже если вы установили расширение SQL IaaS. При регистрации с помощью поставщика ресурсов виртуальной машины SQL создается новый ресурс типа Microsoft. Склвиртуалмачинес. При установке расширения IaaS SQL этот ресурс не создается.

**Какой режим управления SQL используется по умолчанию при регистрации с помощью поставщика ресурсов виртуальной машины SQL?**

Режим управления SQL по умолчанию при регистрации в виртуальной машине SQL с помощью RP _полон_. Если свойство управления SQL не задано при регистрации с помощью поставщика ресурсов виртуальной машины SQL, то режим будет установлен как полный управляемый. Наличие расширения SQL IaaS, установленного на виртуальной машине, является необходимым условием для регистрации в поставщике ресурсов виртуальной машины SQL в режиме полного управления.

**Каковы предварительные требования для регистрации в поставщике ресурсов виртуальной машины SQL?**

Отсутствуют предварительные требования для регистрации в поставщике ресурсов виртуальной машины SQL в режиме "в упрощенном режиме" или "без агента". Предварительные требования для регистрации в поставщике ресурсов виртуальной машины SQL в полном режиме — наличие расширения SQL IaaS, установленного на виртуальной машине.

**Можно ли зарегистрироваться в поставщике ресурсов виртуальной машины SQL, если на виртуальной машине не установлено расширение SQL IaaS?**

Да, вы можете зарегистрироваться в поставщике ресурсов виртуальной машины SQL в режиме упрощенного управления, если на виртуальной машине не установлено расширение SQL IaaS. В упрощенном режиме поставщик ресурсов виртуальной машины SQL будет использовать консольное приложение для проверки версии и выпуска экземпляра SQL. Консольное приложение завершит свою работу после проверки того, что на виртуальной машине работает хотя бы один экземпляр SQL. Регистрация в поставщике ресурсов виртуальной машины SQL в упрощенном режиме не приведет к перезапуску SQL Server и не создаст агент на виртуальной машине.

**Будет ли регистрация в поставщике ресурсов виртуальной машины SQL установить агент на виртуальной машине?**

Нет. При регистрации в поставщике ресурсов виртуальной машины SQL будет создан только новый ресурс метаданных, а на виртуальной машине не будет установлен агент. Расширение IaaS SQL требуется только для обеспечения полной управляемости, поэтому обновление режима управляемости с Lightweight на Full установит расширение SQL IaaS и перезагрузит SQL Server.

**Будет ли регистрация с помощью SQL Server перезапуска поставщика ресурсов виртуальной машины SQL на моей виртуальной машине?**

Нет. При регистрации в поставщике ресурсов виртуальной машины SQL будет создан только новый ресурс метаданных, который не будет перезапущен SQL Server на виртуальной машине. Перезапуск SQL Server требуется только для установки расширения SQL IaaS; для обеспечения полной управляемости требуется расширение IaaS SQL. При обновлении режима управляемости с Lightweight на Full будет установлено расширение SQL IaaS, которое будет перезапущено SQL Server.

**В чем разница между простыми и неагентными режимами управления при регистрации с помощью поставщика ресурсов виртуальной машины SQL?** 

Режим управления без агента доступен только для SQL Server 2008/R2 на Windows Server 2008; и это единственный доступный режим управления для этих версий. Для всех других версий SQL Server доступны два режима управляемости: Облегченный и полный. Для режима "без агента" требуется, чтобы свойства версии и выпуска SQL Server были заданы клиентом. упрощенный режим отправляет запрос виртуальной машине для поиска версии и выпуска экземпляра SQL.

**Можно ли зарегистрироваться в поставщике ресурсов виртуальной машины SQL в режиме "упрощенный" или "без агента" с Azure CLI?**

Нет. Свойство режима управления SQL доступно только при регистрации с помощью поставщика ресурсов виртуальной машины SQL с Azure PowerShell. Azure CLI не поддерживает задание свойства управляемости SQL и всегда регистрируется в поставщике ресурсов виртуальной машины SQL в режиме по умолчанию — полная управляемость.

**Можно ли зарегистрироваться в поставщике ресурсов виртуальной машины SQL, не указывая тип лицензии SQL?**

Нет. Тип лицензии SQL не является дополнительным свойством при регистрации с помощью RP виртуальной машины SQL. Необходимо задать тип лицензии SQL как PAYG или AHUB при регистрации с помощью поставщика ресурсов виртуальной машины SQL во всех режимах управления (без агента, lightweight и Full) с помощью AZ CLI и PowerShell.

**Можно ли обновить расширение SQL IaaS из режима без агента до полного режима?**

Нет. Обновление режима управляемости SQL до Full или lightweight недоступно для режима без агента. Это техническое ограничение Windows Server 2008.

**Можно ли обновить расширение SQL IaaS с упрощенного режима на полный?**

Да. Обновление режима управляемости SQL с Lightweight на Full поддерживается с помощью PowerShell или портал Azure; Он требует перезапуска SQL Server.

**Можно ли перейти к расширению SQL IaaS из полного режима в режим "без агента" или "упрощенный режим управления"?**

Нет. Переход на более раннюю версию режима управления расширениями SQL IaaS не поддерживается. Режим управляемости SQL не может быть понижен из полного режима в режим "простой" или "без агента"; и не может быть понижено из упрощенного режима в режим без агента. Для изменения режима управляемости с полной управляемостью; Удалите расширение SQL IaaS. Удалите ресурс записи. Склвиртуалмачине и повторно зарегистрируйте SQL Server виртуальную машину с помощью поставщика ресурсов виртуальной машины SQL.

**Можно ли зарегистрироваться в поставщике ресурсов виртуальной машины SQL из портал Azure?**

Нет. Регистрация в поставщике ресурсов виртуальной машины SQL недоступна в портал Azure. Регистрация в поставщике ресурсов виртуальной машины SQL в режиме полного управления поддерживается Azure CLI или PowerShell. регистрация в поставщике ресурсов виртуальной машины SQL в упрощенных режимах управляемости или без агентов поддерживается только интерфейсами API Azure PowerShell.

**Можно ли зарегистрировать виртуальную машину с помощью поставщика ресурсов виртуальной машины SQL перед установкой SQL Server?**

Нет. Для успешной регистрации в поставщике ресурсов виртуальной машины SQL виртуальная машина должна иметь хотя бы один экземпляр SQL. Если на виртуальной машине нет экземпляра SQL, новый ресурс micosoft. Склвиртуалмачине будет находиться в состоянии сбоя.

**Можно ли зарегистрировать виртуальную машину с ресурсом виртуальной машины SQL, если имеется несколько экземпляров SQL?**

Да. Поставщик ресурсов виртуальной машины SQL будет регистрировать только один экземпляр SQL. Поставщик ресурсов виртуальной машины SQL будет регистрировать экземпляр SQL по умолчанию в случае нескольких экземпляров. Если экземпляр по умолчанию отсутствует, то поддерживается только регистрация в упрощенном режиме. Чтобы выполнить обновление с Lightweight на полный режим управления, должен существовать либо экземпляр SQL по умолчанию, либо виртуальная машина должна иметь только один именованный экземпляр SQL.

**Можно ли зарегистрировать экземпляр отказоустойчивого кластера SQL Server с помощью поставщика ресурсов виртуальной машины SQL?**

Да. Экземпляры SQL FCI на виртуальной машине Azure можно зарегистрировать с помощью поставщика ресурсов виртуальной машины SQL в упрощенном режиме. Однако экземпляры SQL FCI нельзя обновить до полного режима управления.

**Можно ли зарегистрировать виртуальную машину с помощью виртуальной машины SQL RP, если настроена группа доступности AlwaysOn?**

Да. Регистрация SQL Server экземпляра на виртуальной машине Azure с помощью поставщика ресурсов виртуальной машины SQL при участии в конфигурации группы доступности AlwaysOn не имеет ограничений.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения см. в следующих статьях: 

* [Обзор SQL Server на виртуальных машинах Windows](virtual-machines-windows-sql-server-iaas-overview.md).
* [Вопросы и ответы по SQL Server на виртуальных машинах Windows](virtual-machines-windows-sql-server-iaas-faq.md).
* [Руководство по выбору ценовой категории для виртуальных машин SQL Server в Azure](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Заметки о выпуске SQL Server на виртуальных машинах Windows](virtual-machines-windows-sql-server-iaas-release-notes.md).
