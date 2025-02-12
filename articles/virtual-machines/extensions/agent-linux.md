---
title: Обзор агента виртуальной машины Linux в Azure | Документация Майкрософт
description: Узнайте, как установить и настроить агент Linux (waagent) для управления взаимодействием виртуальной машины с Azure Fabric Controller.
services: virtual-machines-linux
documentationcenter: ''
author: roiyz-msft
manager: gwallace
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: e41de979-6d56-40b0-8916-895bf215ded6
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/17/2016
ms.author: roiyz
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 59a0cdd29e50501f023faf323948a400f325df0b
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2019
ms.locfileid: "67706157"
---
# <a name="understanding-and-using-the-azure-linux-agent"></a>Что такое агент Linux для Azure и как его использовать

Агент Linux для Microsoft Azure (waagent) управляет подготовкой Linux и FreeBSD и взаимодействием виртуальной машины с контроллером структуры Azure. Помимо агента Linux, предоставляющего функциональные возможности подготовки, в Azure также можно использовать cloud-init для некоторых операционных систем Linux. Агент Linux предоставляет следующие функциональные возможности для развертываний IaaS в Linux и FreeBSD:

> [!NOTE]
> Дополнительные сведения см. в [файле сведений](https://github.com/Azure/WALinuxAgent/blob/master/README.md).
> 
> 

* **Подготовка образа**
  
  * Создание учетной записи пользователя
  * Настройка типов аутентификации SSH
  * Развертывание открытых ключей SSH и пар ключей
  * Настройка имени узла
  * Публикация имени узла DNS платформы
  * Отчет с отпечатком ключа узла SSH для платформы
  * Управление диском ресурсов
  * Форматирование и подключение диска ресурсов
  * Настройка пространства подкачки
* **Сеть**
  
  * Управляет маршрутами для улучшения совместимости с DHCP-серверами платформы
  * Обеспечивает стабильность имени сетевого интерфейса
* **Ядро**
  
  * Настраивает виртуальную архитектуру NUMA (отключить для ядра версий ранее `2.6.37`)
  * Использование энтропии Hyper-V для /dev/random
  * Настройка времени ожидания SCSI для корневого устройства (может быть удаленным)
* **Диагностика**
  
  * Перенаправление консоли на последовательный порт
* **Развернутые приложения SCVMM**
  
  * Обнаружение и начальная загрузка агента VMM для Linux при работе в среде System Center Virtual Machine Manager 2012 R2
* **Расширение виртуальной машины**
  
  * Вставка компонента, созданного корпорацией Майкрософт и ее партнерами, в виртуальную машину Linux (IaaS) для включения программного обеспечения и автоматизации настройки
  * Эталонная реализация расширения виртуальной машины на сайте [https://github.com/Azure/azure-linux-extensions](https://github.com/Azure/azure-linux-extensions).

## <a name="communication"></a>Обмен данными
Поток информации от платформы к агенту передается по двум каналам:

* Прилагаемый DVD, используемый во время загрузки, для развернутых приложений IaaS. На этом DVD содержится OVF-совместимый файл конфигурации, включающий всю информацию для подготовки кроме самих пар ключей SSH.
* Чтобы получить развертывание и настройку топологии, используется конечная точка TCP с REST API.

## <a name="requirements"></a>Требования
Следующие системы протестированы и гарантированно поддерживают работу с агентом Linux для Azure:

> [!NOTE]
> Этот список может отличаться от официального списка систем, поддерживаемых платформой Microsoft Azure, который доступен на странице: [https://support.microsoft.com/kb/2805216](https://support.microsoft.com/kb/2805216).
> 
> 

* CoreOS
* CentOS 6.3+
* Red Hat Enterprise Linux 6.7 и более поздней версии
* Debian 7.0+
* Ubuntu 12.04+
* openSUSE 12.3+
* SLES 11 SP3+
* Oracle Linux 6.4+

Другие поддерживаемые системы:

* FreeBSD 10 и более поздней версии (агент Linux для Azure 2.0.10 и более поздней версии)

Для правильного функционирования агента Linux требуются определенные пакеты системы:

* Python 2.6+
* OpenSSL 1.0 и более поздней версии
* OpenSSH 5.3 и более поздней версии
* Служебные программы файловой системы: sfdisk, fdisk, mkfs, parted
* Инструменты работы с паролями: chpasswd, sudo
* Инструменты обработки текста: sed, grep
* Сетевые инструменты: ip-route
* Поддержка ядра для монтирования файловых систем UDF.

## <a name="installation"></a>Установка
Установка с помощью пакета RPM или DEB из репозитория пакета дистрибутива является предпочтительным способом установки и обновления агента Linux для Azure. Все [поставщики поддерживаемых дистрибутивов](../linux/endorsed-distros.md) встраивают пакет агента Linux для Azure в свои образы и репозитории.

Дополнительные варианты установки, например установка из источника, установка в пользовательские расположения или префиксы, описаны в документации в [репозитории агента Linux для Azure на сайте GitHub](https://github.com/Azure/WALinuxAgent).

## <a name="command-line-options"></a>Параметры командной строки
### <a name="flags"></a>Флаги
* verbose: Увеличить уровень детализации указанную команду
* FORCE: Пропустить интерактивные подтверждения для некоторых команд

### <a name="commands"></a>Команды
* Справка: Список поддерживаемых команд и флагов.
* deprovision: Попытка очистить систему и сделать ее пригодной для повторной подготовки. Следующая операция удаляет:
  
  * Все ключи узла SSH (если в файле конфигурации для Provisioning.RegenerateSshHostKeyPair указано значение «y»)
  * Настройка имени сервера в /etc/resolv.conf
  * Корневой пароль из /etc/shadow (если в файле конфигурации для Provisioning.DeleteRootPassword указано значение «y»)
  * Кэшированные аренды DHCP-клиентов
  * возвращается имя узла localhost.localdomain;

> [!WARNING]
> Отмена подготовки не гарантирует, что из образа будет удалена вся конфиденциальная информация и что он будет готов к повторному распространению.
> 
> 

* deprovision + user: Выполняет все, что в - deprovision (см. выше), также удаляется последняя подготовленная учетная запись пользователя (полученная из /var/lib/waagent) и связанных данных. Этот параметр используется для отмены подготовки ранее подготовленного образа в Azure с целью его записи и повторного использования.
* Версия: Отображает версию waagent
* serialconsole: Настройка GRUB для маркировки ttyS0 (первый последовательный порт) в качестве консоли загрузки. Это гарантирует, что журналы загрузки ядра отправляются на последовательный порт и становятся доступными для отладки.
* управляющая программа: Запуск waagent как управляющая программа для управления взаимодействием с платформой. Этот аргумент указывается для waagent в сценарии инициализации waagent.
* Начало: Запуск waagent как фонового процесса

## <a name="configuration"></a>Конфигурация
Файл конфигурации (/ etc/waagent.conf) определяет действия waagent. Ниже показан пример файла конфигурации.

    ```
    Provisioning.Enabled=y
    Provisioning.DeleteRootPassword=n
    Provisioning.RegenerateSshHostKeyPair=y
    Provisioning.SshHostKeyPairType=rsa
    Provisioning.MonitorHostName=y
    Provisioning.DecodeCustomData=n
    Provisioning.ExecuteCustomData=n
    Provisioning.AllowResetSysUser=n
    Provisioning.PasswordCryptId=6
    Provisioning.PasswordCryptSaltLength=10
    ResourceDisk.Format=y
    ResourceDisk.Filesystem=ext4
    ResourceDisk.MountPoint=/mnt/resource
    ResourceDisk.MountOptions=None
    ResourceDisk.EnableSwap=n
    ResourceDisk.SwapSizeMB=0
    LBProbeResponder=y
    Logs.Verbose=n
    OS.RootDeviceScsiTimeout=300
    OS.OpensslPath=None
    HttpProxy.Host=None
    HttpProxy.Port=None
    AutoUpdate.Enabled=y
    ```

Далее подробно описаны различные параметры конфигурации. Параметры конфигурации относятся к одному из трех типов: логическое значение, строка или целое число. Логические параметры конфигурации могут принимать значение "y" или "n". Для некоторых записей конфигурации типа "строка", описанных ниже, может использоваться специальное ключевое слово "None".

**Provisioning.Enabled:**  
```
Type: Boolean  
Default: y
```
Он позволяет пользователю включить или отключить функцию подготовки в агенте. Допустимые значения: "y" или "n". Если подготовка отключена, узел SSH и пользовательские ключи в образе сохраняются, а любые настройки, указанные в API для подготовки Azure, игнорируются.

> [!NOTE]
> В образах облаков Ubuntu, использующих для подготовки пакет cloud-init, для параметра `Provisioning.Enabled` по умолчанию устанавливается значение n.
> 
> 

**Provisioning.DeleteRootPassword:**  
```
Type: Boolean  
Default: n
```
Удаляет корневой пароль в файле /etc/shadow в процессе подготовки.

**Provisioning.RegenerateSshHostKeyPair:**  
```
Type: Boolean  
Default: y
```
Если задан, удаляет все пары ключей узла SSH (ecdsa, dsa и rsa) из /etc/ssh/ в процессе подготовки. При этом генерируется одна новая пара ключей.

Тип шифрования для новой пары ключей настраивается с помощью операции Provisioning.SshHostKeyPairType. Некоторые дистрибутивы повторно создают пары ключей SSH для любых недостающих типов шифрования при перезапуске управляющей программы SSH (например, после перезагрузки).

**Provisioning.SshHostKeyPairType:**  
```
Type: String  
Default: rsa
```
Задается тип алгоритма шифрования, поддерживаемый управляющей программой SSH на виртуальной машине. Обычно поддерживаются значения "rsa", "dsa" и "ecdsa". "putty.exe" в Windows не поддерживает "ecdsa". Таким образом, если вы планируете использовать putty.exe в Windows для подключения к развертыванию Linux, используйте "rsa" или "dsa".

**Provisioning.MonitorHostName:**  
```
Type: Boolean  
Default: y
```
Если задан, waagent отслеживает виртуальную машину Linux на предмет изменений имени узла (как значение, возвращаемое командой "hostname") и автоматически обновляет сетевую конфигурацию образа в соответствии с изменениями. Для передачи изменения имени на DNS-серверы сеть на виртуальной машине будет перезапущена. Это приведет к кратковременной потере подключения к Интернету.

**Provisioning.DecodeCustomData**  
```
Type: Boolean  
Default: n
```
Если задан, waagent декодирует пользовательские данные CustomData из кодировки Base64.

**Provisioning.ExecuteCustomData**  
```
Type: Boolean  
Default: n
```
Если задан, waagent выполняет пользовательские данные CustomData после подготовки.

**Provisioning.AllowResetSysUser**
```
Type: Boolean
Default: n
```
Позволяет сбросить пароль пользователя системы; по умолчанию параметр отключен.

**Provisioning.PasswordCryptId**  
```
Type: String  
Default: 6
```
Алгоритм, используемый crypt при создании хеша пароля.  
 1 — MD5  
 2a — Blowfish  
 5 — SHA-256  
 6 — SHA-512  

**Provisioning.PasswordCryptSaltLength**  
```
Type: String  
Default: 10
```
Длина случайной соли, используемой при создании хеша пароля.

**ResourceDisk.Format:**  
```
Type: Boolean  
Default: y
```
Если задан, диск ресурсов, предоставленный платформой, форматируется и монтируется с использованием waagent, если тип файловой системы, запрошенный пользователем в "ResourceDisk.Filesystem", отличается от "ntfs". На диске доступен один раздел типа Linux (83). Обратите внимание, что этот раздел не форматируется, если он может быть успешно подключен.

**ResourceDisk.Filesystem:**  
```
Type: String  
Default: ext4
```
Указывает тип файловой системы для диска ресурса. Допустимые значения зависят от дистрибутива Linux. Если используется строка X, в образе Linux должна присутствовать строка mkfs.X. Для образов SLES 11 обычно используется значение "ext3". Для образов FreeBSD здесь следует использовать "ufs2".

**ResourceDisk.MountPoint:**  
```
Type: String  
Default: /mnt/resource 
```
Указывает путь, по которому подключен диск ресурсов. Диск ресурсов является *временным* диском и должен быть очищен при отмене подготовки виртуальной машины.

**ResourceDisk.MountOptions**  
```
Type: String  
Default: None
```
Задает параметры монтирования диска, которые должны быть переданы команде mount -o. Это список значений, разделенный запятыми, например 'nodev,nosuid'. Подробные сведения см. в разделе mount(8).

**ResourceDisk.EnableSwap:**  
```
Type: Boolean  
Default: n
```
Если задано, на диске ресурсов создается файл подкачки (/swapfile), который добавляется к пространству подкачки в системе.

**ResourceDisk.SwapSizeMB:**  
```
Type: Integer  
Default: 0
```
Размер файла подкачки в мегабайтах.

**Logs.Verbose:**  
```
Type: Boolean  
Default: n
```
Если задано, то файл журнала расширяется. Waagent записывает данные в /var/log/waagent.log, а также использует системную функцию logrotate для ротации журналов.

**OS.EnableRDMA**  
```
Type: Boolean  
Default: n
```
Если задан, агент пытается установить, а затем загрузить драйвер ядра RDMA, соответствующий версии встроенного ПО базового оборудования.

**OS.RootDeviceScsiTimeout:**  
```
Type: Integer  
Default: 300
```
Этот параметр настраивает время ожидания SCSI в секундах на дисках операционной системы и на дисках с данными. Если не установлено, будут использоваться системные значения по умолчанию.

**OS.OpensslPath:**  
```
Type: String  
Default: None
```
Этот параметр можно использовать для указания альтернативного пути для двоичного файла openssl, используемого при выполнении шифрования.

**HttpProxy.Host, HttpProxy.Port**  
```
Type: String  
Default: None
```
Если задан, агент использует этот прокси-сервер для доступа в Интернет. 

**AutoUpdate.Enabled**
```
Type: Boolean
Default: y
```
Позволяет включить или отключить автоматическое обновление для обработки целевого состояния; по умолчанию параметр включен.



## <a name="ubuntu-cloud-images"></a>Образы облаков Ubuntu
В образах облаков Ubuntu пакет [cloud-init](https://launchpad.net/ubuntu/+source/cloud-init) используется для выполнения ряда задач настройки, которыми в противном случае управлял бы агент Linux для Azure. Существуют следующие отличия.

* **Provisioning.Enabled** по умолчанию устанавливается значение "n" в образах облаков Ubuntu, использующих пакет cloud-init для выполнения задач по подготовке.
* Следующие параметры конфигурации не влияют на образы облаков Ubuntu, использующие пакеты cloud-init для управления диском ресурсов и пространством подкачки:
  
  * **ResourceDisk.Format;**
  * **ResourceDisk.Filesystem;**
  * **ResourceDisk.MountPoint;**
  * **ResourceDisk.EnableSwap;**
  * **ResourceDisk.SwapSizeMB.**

* Если вам нужно настроить точку подключения диска ресурсов и пространство подкачки в образах облаков Ubuntu во время подготовки, см. следующие ресурсы.
  
  * [Вики-сайте Ubuntu: Настройка разделов подкачки](https://go.microsoft.com/fwlink/?LinkID=532955&clcid=0x409)
  * [Включение пользовательских данных в виртуальную машину Azure](../windows/classic/inject-custom-data.md)

