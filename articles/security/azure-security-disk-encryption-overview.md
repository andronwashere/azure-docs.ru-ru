---
title: Что такое шифрование дисков Azure?
description: В этой статье приводятся общие сведения о шифровании дисков Azure.
author: msmbaldwin
ms.service: security
ms.topic: article
ms.author: mbaldwin
ms.date: 07/17/2019
ms.custom: seodec18
ms.openlocfilehash: a67f19f0823827dad74e7aba15a92d696fbf580b
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68304265"
---
# <a name="azure-disk-encryption-overview"></a>Общие сведения о шифровании дисков Azure

Шифрование дисков Azure помогает защитить данные в соответствии с обязательствами по обеспечению безопасности и соответствия требованиям Организации. Он использует функцию [BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview) в Windows и функцию [DM-](https://en.wikipedia.org/wiki/Dm-crypt) Encryption в Linux, чтобы обеспечить шифрование томов для дисков ОС и данных виртуальных машин Azure. Кроме того, она интегрирована с [Azure Key Vault](https://azure.microsoft.com/documentation/services/key-vault/) для управления ключами и секретами шифрования дисков и управления ими, а также обеспечивает шифрование всех данных на дисках виртуальной машины в службе хранилища Azure. Шифрование дисков Azure для виртуальных машин Windows и Linux находится в общем доступе во всех общедоступных регионах Azure и регионах Azure для государственных организаций для стандартных виртуальных машин и виртуальных машин с хранилищем Azure класса Premium. 

Если вы используете центр безопасности Azure, он оповестит вас при наличии незашифрованных виртуальных машин. Вы получите оповещение высокого уровня серьезности вместе c рекомендацией о шифровании таких виртуальных машин.

![Оповещение о шифровании диска в центре безопасности Azure](media/azure-security-disk-encryption/security-center-disk-encryption-fig1.png)

> [!NOTE]
> Выполнение некоторых приведенных рекомендаций может привести к более интенсивному использованию данных, сети или вычислительных ресурсов, а следовательно к дополнительным затратам на лицензии или подписки.


## <a name="encryption-scenarios"></a>Сценарии шифрования

С помощью шифрования дисков Azure вы можете решать требования к безопасности и соответствию Организации, обеспечивая защиту виртуальных машин Azure при хранении, используя стандартную промышленную технологию шифрования. Кроме того, можно настроить загрузку виртуальных машин с помощью управляемых клиентом ключей и политик (BYOK), а также выполнить аудит использования этих ключей в хранилище ключей.

Шифрование дисков Azure поддерживает следующие сценарии для клиентов:

* Включение и отключение шифрования на новых виртуальных машинах, созданных из поддерживаемых образов коллекции Azure.
* Включение и отключение шифрования на существующих виртуальных машинах, работающих в Azure.
* Включение и отключение шифрования на новых виртуальных машинах Windows, созданных на основе предварительно зашифрованного виртуального жесткого диска и ключей шифрования.
* Включение и отключение шифрования в масштабируемых наборах виртуальных машин Windows.
* Включение и отключение шифрования на дисках с данными для масштабируемых наборов виртуальных машин Linux.
* Включение и отключение шифрования виртуальных машин управляемого диска.
* Обновление параметров шифрования существующей зашифрованной виртуальной машины хранилища класса Premium и без класса Premium.
* Резервное копирование и восстановление зашифрованных виртуальных машин.
* Используйте собственное шифрование (БЙОЕ) и перенесите собственные сценарии (BYOK), в которых клиенты используют собственные ключи шифрования и сохраняют их в хранилище ключей Azure.

Он также поддерживает следующие сценарии для виртуальных машин, включенных в Microsoft Azure:

* интеграция с Azure Key Vault;
* [Виртуальные машины уровня "Стандартный](https://azure.microsoft.com/pricing/details/virtual-machines/) ", соответствующие [минимальным требованиям к памяти](azure-security-disk-encryption-prerequisites.md#supported-vm-sizes). 
* Включение шифрования на виртуальных машинах Windows и Linux, управляемых дисках и масштабируемых наборах на виртуальных машинах из поддерживаемых образов коллекции Azure.
* Отключение шифрования на дисках ОС и данных для виртуальных машин Windows, виртуальных машин масштабируемого набора и виртуальных машин управляемого диска.
* Отключение шифрования на дисках данных для виртуальных машин Linux, виртуальных машин масштабируемого набора и управляемых виртуальных машин.
* Включение шифрования для виртуальных машин под управлением клиентской ОС Windows.
* Включение шифрования томов с путями подключения.
* Включение шифрования на виртуальных машинах Linux, для которых настроено чередование дисков (RAID) с помощью mdadm.
* Включение шифрования на виртуальных машинах Linux, использующих LVM для дисков данных.
* Включение шифрования для ОС виртуальных машин Linux и дисков данных.

   > [!NOTE]
   > Шифрование диска операционной системы для некоторых дистрибутивов Linux не поддерживается. Дополнительные сведения см. в [разделе Поддерживаемые операционные системы шифрования дисков Azure: Linux](azure-security-disk-encryption-prerequisites.md#linux).
   
* Включение шифрования на виртуальных машинах, настроенных с помощью дисковых пространств Windows, начиная с Windows Server 2016. Локальные дисковые пространства (S2D) пока не поддерживается.
* Резервное копирование и восстановление зашифрованных виртуальных машин для ключей шифрования ключей (KEK) и сценариев, отличных от KEK.

Шифрование дисков Azure не работает в следующих сценариях, функциях и технологиях:

* Шифрование ВИРТУАЛЬНЫХ машин базового уровня или виртуальных машин, созданных с помощью классического метода создания виртуальной машины.
* Отключение шифрования диска операционной системы или диска данных виртуальной машины Linux при шифровании диска ОС.
* Шифрование диска ОС для масштабируемых наборов виртуальных машин Linux.
* Шифрование виртуальных машин Windows, настроенных на основе программных RAID-систем.
* Шифрование пользовательских образов на виртуальных машинах Linux.
* Интеграция с локальной системой управления ключами.
* служба файлов Azure (общая файловая система);
* сетевая файловая система (NFS);
* динамические тома;

## <a name="encryption-features"></a>Функции шифрования

При включении и развертывании шифрования дисков Azure для виртуальных машин Azure можно настроить следующие возможности для включения:

* Шифрование тома операционной системы для защиты загрузочного тома при хранении в хранилище.
* Шифрование томов данных для защиты неактивных томов данных в хранилище.
* Отключение шифрования дисков операционной системы и данных для виртуальных машин Windows.
* Отключение шифрования на дисках данных для виртуальных машин Linux (только в том случае, если диск ОС не зашифрован).
* Защита ключей шифрования и секретов в подписке Azure Key Vault.
* Сообщает о состоянии шифрования зашифрованной виртуальной машины.
* Удаление параметров конфигурации шифрования диска из виртуальной машины.
* Резервное копирование и восстановление зашифрованных виртуальных машин с помощью службы Azure Backup.

Шифрование дисков Azure для виртуальных машин для Windows и Linux включает:

* [Расширение шифрования дисков для Windows](../virtual-machines/extensions/azure-disk-enc-windows.md).
* [Расширение шифрования дисков для Linux](../virtual-machines/extensions/azure-disk-enc-linux.md).
* [Командлеты шифрования дисков PowerShell](/powershell/module/az.compute/set-azvmdiskencryptionextension?view=azps-2.2.0).
* [Azure CLI командлетов шифрования дисков](/cli/azure/vm/encryption?view=azure-cli-latest).
* [Azure Resource Manager шаблоны шифрования дисков](azure-security-disk-encryption-appendix.md#resource-manager-templates).

> [!NOTE]
> Дополнительная плата за шифрование дисков Azure для виртуальных машин не взимается. К хранилищу ключей, используемому для хранения ключей шифрования, применяются [цены на Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) ценовой категории "Стандартный". 

## <a name="encryption-workflow"></a>Рабочий процесс шифрования

Чтобы включить шифрование дисков для виртуальных машин Windows и Linux, выполните следующие шаги.

1. Дайте согласие на включение шифрования дисков с помощью шаблона Resource Manager, командлетов PowerShell или Azure CLI для шифрования дисков Azure, а также укажите конфигурацию шифрования.

   * В случае использования зашифрованного клиентом виртуального жесткого диска передайте этот диск в свою учетную запись хранения, а ключевой материал шифрования — в хранилище ключей. Затем укажите конфигурацию шифрования, чтобы включить шифрование на новой виртуальной машине.
   * Для новых виртуальных машин, созданных из поддерживаемых образов коллекции, и существующих виртуальных машин, уже работающих в Azure, укажите конфигурацию шифрования, чтобы включить шифрование на виртуальной машине.

1. Предоставьте доступ к платформе Azure для чтения материала ключа шифрования (ключи шифрования BitLocker для систем Windows и парольная фраза для Linux) из хранилища ключей, чтобы включить шифрование на виртуальной машине.

1. Azure вносит изменения в модель службы виртуальной машины на основе конфигурации шифрования и хранилища ключей, а затем подготавливает вашу зашифрованную виртуальную машину.

   ![Антивредоносное ПО Майкрософт в Azure](./media/azure-security-disk-encryption/disk-encryption-fig1.png)

## <a name="decryption-workflow"></a>Рабочий процесс расшифровки
Чтобы отключить шифрование дисков для виртуальных машин, выполните следующие общие действия.

1. Выберите отключение шифрования (расшифровки) на работающей виртуальной машине в Azure и укажите конфигурацию расшифровки. Отключить шифрование можно с помощью шаблона Resource Manager, командлетов PowerShell или Azure CLI для шифрования дисков Azure.

   Этот шаг отключает шифрование операционной системы или тома данных либо на работающей виртуальной машине Windows. Как отмечено в предыдущем разделе, отключение шифрования диска ОС Linux не поддерживается. Расшифровать диски данных можно только на виртуальных машинах Linux и только в тех случаях, когда диск ОС не зашифрован.

1. Azure обновляет модель службы виртуальной машины, и виртуальная машина помечается как Расшифрованная. Неактивное содержимое виртуальной машины больше не шифруется.

   > [!NOTE]
   > При операции отключения шифрования хранилище ключей и ключевой материал шифрования (ключи шифрования BitLocker для Windows или парольная фраза для Linux) не удаляются.
   >
   > Отключение шифрования диска операционной системы для Linux не поддерживается. Расшифровать можно только диски данных на виртуальных машинах Linux.
   >
   > Отключение шифрования диска данных для Linux не поддерживается, если диск ОС зашифрован.


## <a name="encryption-workflow-previous-release"></a>Рабочий процесс шифрования (предыдущий выпуск)

В новом выпуске шифрования дисков Azure устранена необходимость предоставлять параметр приложения Azure AD для включения шифрования дисков виртуальной машины. В новом выпуске больше не требуется вводить учетные данные Azure AD на этапе включения шифрования. При использовании нового выпуска все новые виртуальные машины должны быть зашифрованы без параметров приложения Azure AD. Виртуальные машины, которые уже были зашифрованы с помощью параметров приложения Azure AD, по-прежнему поддерживаются. Их следует и дальше обслуживать с использованием синтаксиса Azure AD. Чтобы включить шифрование дисков для виртуальных машин Windows и Linux (предыдущий выпуск), выполните следующие шаги.

1. Выберите сценарий шифрования из перечисленных в разделе [Сценарии шифрования](#encryption-scenarios).

1. Дайте согласие на включение шифрования дисков с помощью шаблона Resource Manager, командлетов PowerShell или Azure CLI для шифрования дисков Azure, а также укажите конфигурацию шифрования.

   * В случае использования зашифрованного клиентом виртуального жесткого диска передайте этот диск в свою учетную запись хранения, а ключевой материал шифрования — в хранилище ключей. Затем укажите конфигурацию шифрования, чтобы включить шифрование на новой виртуальной машине.
   * Для новых виртуальных машин, созданных из Marketplace и существующих виртуальных машин, которые уже выполняются в Azure, укажите конфигурацию шифрования, чтобы включить шифрование на виртуальной машине.

1. Предоставьте доступ к платформе Azure для чтения материала ключа шифрования (ключи шифрования BitLocker для систем Windows и парольная фраза для Linux) из хранилища ключей, чтобы включить шифрование на виртуальной машине.

1. Укажите удостоверение приложения Azure AD для записи ключевого материала шифрования в хранилище ключей. Этот шаг включает шифрование на виртуальной машине для сценариев, упомянутых в шаге 2.

1. Azure вносит изменения в модель службы виртуальной машины на основе конфигурации шифрования и хранилища ключей, а затем подготавливает вашу зашифрованную виртуальную машину.


## <a name="terminology"></a>Терминология
В следующей таблице описаны некоторые распространенные термины, используемые в документации по шифрованию дисков Azure.

| Терминология | Определение |
| --- | --- |
| Azure AD | Учетная запись [Azure AD](https://azure.microsoft.com/documentation/services/active-directory/) используется для проверки подлинности, хранения секретов в хранилище ключей и извлечения их из него. |
| Azure Key Vault | Key Vault представляет собой службу управления криптографическими ключами. Она основана на аппаратных модулях безопасности, соответствующих Федеральному стандарту обработки информации (FIPS). Эти стандарты позволяют надежно хранить криптографические ключи и конфиденциальные данные. Дополнительные сведения см. в документации по [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). |
| BitLocker |[BitLocker](https://technet.microsoft.com/library/hh831713.aspx) — это известная промышленным стандартам технология шифрования томов Windows, которая используется для включения шифрования дисков на виртуальных машинах Windows. |
| BEK | Ключи шифрования BitLocker (BEK) используются для шифрования загрузочного тома ОС и томов данных. Ключи BitLocker хранятся в хранилище ключей в виде секретов. |
| Инфраструктура CLI Azure | [Azure CLI](/cli/azure/install-azure-cli) оптимизирован для администрирования ресурсов Azure и управления ими из командной строки.|
| DM-Crypt |[DM-](https://gitlab.com/cryptsetup/cryptsetup/wikis/DMCrypt) Encryption — это прозрачная подсистема шифрования дисков на основе Linux, которая используется для включения шифрования дисков на виртуальных машинах Linux. |
| Ключ шифрования ключа (KEK) | Асимметричный ключ (RSA 2048), который можно использовать для защиты или перетекания секрета. Вы можете использовать защищенный HSM ключ или ключ с программной защитой. Дополнительные сведения см. в документации по [Azure Key Vault](https://azure.microsoft.com/services/key-vault/). |
| Командлеты PowerShell | Дополнительные сведения см. в статье [Общие сведения об Azure PowerShell](/powershell/azure/overview). |

## <a name="next-steps"></a>Следующие шаги

Чтобы приступить к работе, ознакомьтесь с [предварительными требованиями к шифрованию дисков Azure](azure-security-disk-encryption-prerequisites.md).

