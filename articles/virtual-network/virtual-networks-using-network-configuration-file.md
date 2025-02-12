---
title: Настройка (классической) виртуальной сети Azure с использованием файла конфигурации сети | Документация Майкрософт
description: Узнайте, как изменить и создать (классические) виртуальные сети путем экспорта, изменения и импорта файла конфигурации сети.
services: virtual-network
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: genli
ms.custom: ''
ms.openlocfilehash: e26ec4d268b9bd8852ef8cd2c522995902e15923
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "62108017"
---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>Настройка (классической) виртуальной сети с помощью файла конфигурации сети
> [!IMPORTANT]
> В Azure предлагаются две модели развертывания для создания ресурсов и работы с ними: [модель развертывания с помощью Resource Manager и классическая модель](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). В этой статье рассматривается использование классической модели развертывания. Для большинства новых развертываний корпорация Майкрософт рекомендует использовать модель развертывания с помощью Resource Manager.

Вы можете создать и настроить (классическую) виртуальную сеть с файлом конфигурации сети с помощью классического интерфейса командной строки Azure или Azure PowerShell. Вы не можете создавать или изменять виртуальную сеть с помощью модели развертывания Azure Resource Manager с использованием файла конфигурации сети. Использовать портал Azure для создания или изменения (классической) виртуальной сети с помощью файла конфигурации сети нельзя. Однако портал Azure можно использовать для создания такой сети без использования файла конфигурации сети.

Для создания и настройки (классической) виртуальной сети с помощью файла конфигурации сети необходимо экспортировать, изменить и импортировать файл.

## <a name="export"></a>Экспорт файла конфигурации сети

Чтобы экспортировать файл конфигурации сети, можно использовать PowerShell или классический Azure CLI. PowerShell экспортирует XML-файл, а классический Azure CLI — JSON-файл.

### <a name="powershell"></a>PowerShell
 
1. [Установите Azure PowerShell и войдите в Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).
2. При необходимости измените каталог (убедитесь, что он существует) и имя файла в указанной ниже команде. Затем выполните команду, чтобы экспортировать файл конфигурации сети.

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-classic-cli"></a>Классический Azure CLI

1. [Установите классический Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Выполните оставшиеся шаги в командной строке классического интерфейса командной строки.
2. Войдите в Azure, выполнив команду `azure login`.
3. Выполните команду `azure config mode asm`, чтобы убедиться, что используется режим ASM.
4. При необходимости измените каталог (убедитесь, что он существует) и имя файла в указанной ниже команде. Затем выполните команду, чтобы экспортировать файл конфигурации сети.
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a>Создание и изменение файла конфигурации сети

Файл конфигурации сети имеет формат XML (при использовании PowerShell) или JSON (при использовании классического интерфейса командной строки). Файл можно изменить в любом текстовом редакторе и в редакторе XML или JSON. Сведения о параметрах схемы файла конфигурации сети см. в [этой статье](https://msdn.microsoft.com/library/azure/jj157100.aspx). Дополнительные сведения о параметрах см. в разделе [Просмотр виртуальных сетей и параметров](manage-virtual-network.md#view-virtual-networks-and-settings). Изменения, внесенные в файл:

- Должны соответствовать схеме. В противном случае импорт файла конфигурации сети завершится ошибкой.
- Перезапишут все имеющиеся параметры сети подписки. Поэтому соблюдайте предельную осторожность при внесении изменений. Например, просмотрите указанные ниже примеры файла конфигурации сети. Предположим, что исходный файл содержит два экземпляра **VirtualNetworkSite** и вы его изменили, как показано в примерах. При импорте файла Azure удаляет виртуальную сеть для экземпляра **VirtualNetworkSite**, удаленного в файле. Этот упрощенный сценарий предполагает, что в виртуальной сети отсутствуют ресурсы. В противном случае виртуальную сеть не удастся удалить, а импорт завершится сбоем.

> [!IMPORTANT]
> Azure рассматривает подсеть с имеющимся развертыванием как **используемую**. Когда подсеть используется, она не может быть изменена. Перед изменением информации о подсети в файле конфигурации сети переместите все, что было развернуто в подсети, в другую подсеть, которая не изменяется. Дополнительные сведения см. в статье [Перемещение (классической) виртуальной машины или экземпляра роли облачных служб в другую подсеть с помощью PowerShell](virtual-networks-move-vm-role-to-subnet.md).

### <a name="example-xml-for-use-with-powershell"></a>Пример кода XML для использования с PowerShell

В следующем примере файла конфигурации сети создается виртуальная сеть с именем *myVirtualNetwork* и адресным пространством *10.0.0.0/16* в регионе Azure — *восточная часть США*. Виртуальная сеть содержит одну подсеть с именем *mySubnet* и префиксом адреса *10.0.0.0/24*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns />
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myVirtualNetwork" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="mySubnet">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

Если экспортированный файл конфигурации сети пуст, можно скопировать код XML из предыдущего примера и вставить его в новый файл.

### <a name="example-json-for-use-with-the-classic-cli"></a>Пример кода JSON для использования с классическим интерфейсом командной строки

В следующем примере файла конфигурации сети создается виртуальная сеть с именем *myVirtualNetwork* и адресным пространством *10.0.0.0/16* в регионе Azure — *восточная часть США*. Виртуальная сеть содержит одну подсеть с именем *mySubnet* и префиксом адреса *10.0.0.0/24*.

```json
{
   "VirtualNetworkConfiguration" : {
      "Dns" : "",
      "VirtualNetworkSites" : [
         {
            "AddressSpace" : [ "10.0.0.0/16" ],
            "Location" : "East US",
            "Name" : "myVirtualNetwork",
            "Subnets" : [
               {
                  "AddressPrefix" : "10.0.0.0/24",
                  "Name" : "mySubnet"
               }
            ]
         }
      ]
   }
}
```

Если экспортируемый файл конфигурации сети пуст, можно скопировать код JSON из предыдущего примера и вставить его в новый файл.

## <a name="import"></a>Импорт файла конфигурации сети

Чтобы импортировать файл конфигурации сети, можно использовать PowerShell или классический интерфейс командной строки. PowerShell импортирует XML-файл, а классический интерфейс командной строки — JSON-файл. При сбое импорта убедитесь, что файл соответствует [схеме конфигурации сети](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

### <a name="powershell"></a>PowerShell
 
1. [Установите Azure PowerShell и войдите в Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).
2. При необходимости измените каталог и имя файла в указанной ниже команде. Затем выполните команду, чтобы импортировать файл конфигурации сети.
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-classic-cli"></a>Классический Azure CLI

1. [Установите классический Azure CLI](/cli/azure/install-classic-cli). Выполните оставшиеся шаги в командной строке классического интерфейса командной строки.
2. Войдите в Azure, выполнив команду `azure login`.
3. Выполните команду `azure config mode asm`, чтобы убедиться, что используется режим ASM.
4. При необходимости измените каталог и имя файла в указанной ниже команде. Затем выполните команду, чтобы импортировать файл конфигурации сети.

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
