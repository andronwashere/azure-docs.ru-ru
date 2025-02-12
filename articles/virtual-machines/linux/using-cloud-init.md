---
title: Поддержка cloud-init для виртуальных машин Linux в Azure | Документы Microsoft
description: Общие сведения о возможностях cloud-init в Microsoft Azure
services: virtual-machines-linux
documentationcenter: ''
author: rickstercdn
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 057f7c42c037dac4cb2be686df09287de7113f0d
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67695387"
---
# <a name="cloud-init-support-for-virtual-machines-in-azure"></a>Поддержка cloud-init для виртуальных машин в Azure
Из этой статьи можно узнать об имеющейся поддержке [cloud-init](https://cloudinit.readthedocs.io) для настройки виртуальной машины или масштабируемых наборов виртуальных машин во время подготовки в Azure. Эти скрипты cloud-init выполняются при первой загрузке, если в Azure подготовлены все нужные ресурсы.  

## <a name="cloud-init-overview"></a>Обзор cloud-Init
[Пакет cloud-init](https://cloudinit.readthedocs.io) — широко используемое средство, используемое для настройки виртуальной машины Linux при ее первой загрузке. Вы можете использовать cloud-init для установки пакетов, записи файлов или настройки пользователей и параметров безопасности. Так как cloud-init вызывается при начальной загрузке, к вашей конфигурации не нужно применять какие-либо дополнительные действия или агентов.  Дополнительные сведения о том, как правильно отформатировать файлы, `#cloud-config`, см. на [сайте документации по cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/format.html#cloud-config-data).  Файлы `#cloud-config` — это текстовые файлы, закодированные в формате base64.

Кроме того, cloud-init работает с разными дистрибутивами. Например, для установки пакета не используется **apt-get install** или **yum install**. Вместо этого можно определить список пакетов для установки. Файл cloud-init автоматически использует собственный инструмент управления пакетами из выбранного дистрибутива.

 Мы активно сотрудничаем с нашими утвержденными партнерами, работающими над дистрибутивами Linux, чтобы образы с поддержкой cloud-init стали доступными в Azure Marketplace. Эти образы обеспечивают бесперебойную работу развертываний и конфигураций cloud-init с виртуальными машинами и масштабируемыми наборами виртуальных машин (VMSS). В следующей таблице приведены сведения о текущей доступности образов с поддержкой cloud-init на платформе Azure:

| Издатель | ПРЕДЛОЖЕНИЕ | номер SKU | Version | готовность к использованию cloud-init |
|:--- |:--- |:--- |:--- |:--- |
|Canonical |UbuntuServer |18.04-LTS |latest |Да | 
|Canonical |UbuntuServer |17.10 |latest |Да | 
|Canonical |UbuntuServer |16.04-LTS |latest |Да | 
|Canonical |UbuntuServer |14.04.5-LTS |latest |Да |
|CoreOS |CoreOS |Stable |latest |Да |
|OpenLogic |CentOS |7-CI |latest |предварительный просмотр |
|RedHat |RHEL |7-RAW-CI |latest |предварительный просмотр |

Сейчас Azure Stack не поддерживает подготовку RHEL 7.4 и CentOS 7.4 с помощью cloud-init.

## <a name="what-is-the-difference-between-cloud-init-and-the-linux-agent-wala"></a>Разница между cloud-init и агентом Linux (WALA)
WALA — это уникальный агент платформы Azure, использующийся для подготовки и настройки виртуальных машин и обработки расширений Azure. Мы улучшили задачу настройки виртуальных машин для использования cloud-init вместо агента Linux таким образом, чтобы имеющиеся пользователи cloud-init смогли применять свои текущие скрипты cloud-init.  Если у вас уже есть скрипты cloud-init для настройки систем Linux, то для их включения **дополнительные параметры не требуются**. 

Если не указать параметр AzureCLI `--custom-data` во время подготовки, WALA принимает минимальные необходимые параметры подготовки для виртуальной машины и выполняет развертывание с параметрами по умолчанию.  При ссылке на параметр cloud-init `--custom-data` сведения, содержащиеся в пользовательских данных (отдельные параметры или весь скрипт), переопределят значения WALA по умолчанию. 

Конфигурации WALA для виртуальных машин ограничены максимальным временем подготовки виртуальных машин.  Конфигурация cloud-init применяется к виртуальным машинам без временных ограничений, которые не приведут к сбою развертывания по времени. 

## <a name="deploying-a-cloud-init-enabled-virtual-machine"></a>Развертывание виртуальной машины с поддержкой cloud-init
Развертывать виртуальную машину с включенным cloud-init так же просто, как и ссылаться на распределение с включенным cloud-init во время развертывания.  Распространители дистрибутивов Linux должны включать и интегрировать cloud-init в свои базовые опубликование образы Azure. Убедившись, что в образе, который нужно развернуть, включен cloud-init, вы можете использовать Azure CLI для его развертывания. 

Первым шагом при развертывании образа является создание группы ресурсов с использованием команды [az group create](/cli/azure/group). Группа ресурсов Azure является логическим контейнером, в котором происходит развертывание ресурсов Azure и управление ими. 

В следующем примере создается группа ресурсов с именем *myResourceGroup* в расположении *eastus*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
Следующим действием является создание файла *cloud-init.txt* в текущей оболочке и вставка следующей конфигурации. Для этого примера создайте файл в Cloud Shell (не на локальном компьютере). Вы можете использовать любой редактор. Введите `sensible-editor cloud-init.txt`, чтобы создать файл и просмотреть список доступных редакторов. Выберите первый пункт, чтобы использовать редактор **nano**. Убедитесь, что весь файл cloud-init скопирован правильно, особенно первая строка:

```yaml
#cloud-config
package_upgrade: true
packages:
  - httpd
```
Нажмите клавиши `ctrl-X`, чтобы выйти из файла, введите `y`, чтобы сохранить изменения в файле, а затем нажмите клавишу `enter`, чтобы подтвердить имя файла при выходе.

Последним шагом является создание виртуальной машины с использованием команды [az vm create](/cli/azure/vm). 

В следующем примере создаются виртуальная машина *centos74* и ключи SSH, если они не существуют в расположении ключей по умолчанию. Чтобы использовать определенный набор ключей, используйте параметр `--ssh-key-value`.  Используйте параметр `--custom-data`, чтобы передать файл конфигурации cloud-init. Укажите полный путь к конфигурации *cloud-init.txt*, если этот файл сохранен вне текущего рабочего каталога. В следующем примере создается виртуальная машина с именем *centos74*.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS-CI:7-CI:latest \
  --custom-data cloud-init.txt \
  --generate-ssh-keys 
```

После создания виртуальной машины в Azure CLI появятся сведения о вашем развертывании. Запишите значение `publicIpAddress`. Этот адрес используется для доступа к виртуальной машине.  На создание виртуальной машины, установку пакетов и запуск приложения может потребоваться некоторое время. Некоторые фоновые задачи продолжают работу после возврата к командной строке в Azure CLI. Вы можете подключиться к виртуальной машине по протоколу SSH и выполнить шаги, описанные в разделе "Устранение неполадок", чтобы просмотреть журналы cloud-init. 

## <a name="troubleshooting-cloud-init"></a>Устранение неполадок cloud-init
После подготовки виртуальной машины cloud-init запустит все модули и скрипты, заданные в параметре `--custom-data`, для ее настройки.  Если вам необходимо устранить ошибки или пропуски в конфигурации, найдите имя модуля (`disk_setup` или `runcmd`, к примеру) в журнале cloud-init, который находится в файле **/var/log/cloud-init.log**.

> [!NOTE]
> Не каждый сбой модуля приводит к неустранимому сбою всей конфигурации cloud-init. Например, если используется модуль `runcmd`, при сбое скрипта cloud-init все еще будет отображать сообщение об успешной подготовке, так как выполнен модуль runcmd.

Дополнительные сведения о журнале cloud-init см. в [документации по cloud-init](https://cloudinit.readthedocs.io/en/latest/topics/logging.html) 

## <a name="next-steps"></a>Следующие шаги
Примеры изменения конфигураций с помощью cloud-init см. в следующих документах:
 
- [Добавление пользователя Linux к виртуальной машине](cloudinit-add-user.md)
- [Запуск диспетчера пакетов для обновления существующих пакетов при первой загрузке](cloudinit-update-vm.md)
- [Изменение имени локального узла виртуальной машины](cloudinit-update-vm-hostname.md) 
- [Установка пакета приложения, обновление файлов конфигурации и внедрение ключей](tutorial-automate-vm-deployment.md)
