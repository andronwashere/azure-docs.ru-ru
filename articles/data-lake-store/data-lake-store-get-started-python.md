---
title: 'Python: Начало работы с Gen1 хранилища Озера данных Azure | Документация Майкрософт'
description: Узнайте, как использовать пакет Python SDK для работы с операциями управления учетными записями Azure Data Lake Storage 1-го поколения.
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: 75f6de6f-6fd8-48f4-8707-cb27d22d27a6
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: twooley
ms.openlocfilehash: b6ef5a5c12bb766fb7106d5c7a8189c4b92980d2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60877263"
---
# <a name="account-management-operations-on-azure-data-lake-storage-gen1-using-python"></a>Операции управления учетными записями в Azure Data Lake Storage 1-го поколения c использованием Python
> [!div class="op_single_selector"]
> * [ПАКЕТ SDK .NET](data-lake-store-get-started-net-sdk.md)
> * [REST API](data-lake-store-get-started-rest-api.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Узнайте, как использовать пакет SDK для Python для Azure Data Lake Storage 1-го поколения для выполнения основных операций управления учетными записями, таких как создание учетной записи Data Lake Storage 1-го поколения, перечисление учетных записей Data Lake Storage 1-го поколения и т. д. Дополнительные сведения о том, как выполнять операции файловой системы в Data Lake Storage 1-го поколения с помощью Python, см. в [этой статье](data-lake-store-data-operations-python.md).

## <a name="prerequisites"></a>Технические условия

* **Python**. Скачать Python можно [здесь](https://www.python.org/downloads/). В этой статье используется версия Python 3.6.2.

* **Подписка Azure**. См. страницу [бесплатной пробной версии Azure](https://azure.microsoft.com/pricing/free-trial/).

* **Группа ресурсов Azure.** Инструкции см. в статье [Управление ресурсами Azure через портал](../azure-resource-manager/manage-resource-groups-portal.md).

## <a name="install-the-modules"></a>Установка модулей

Для работы с Data Lake Storage 1-го поколения с использованием Python необходимо установить три модуля.

* Модуль `azure-mgmt-resource`, который включает в себя модули Azure для Active Directory и т. д.
* Модуль `azure-mgmt-datalake-store`, который включает в себя операции по управлению учетной записью Azure Data Lake Storage 1-го поколения. Дополнительные сведения см. в [справочнике по модулю управления Azure Data Lake Storage 1-го поколения](https://docs.microsoft.com/python/api/azure.mgmt.datalake.store?view=azure-python).
* Модуль `azure-datalake-store`, который включает в себя операции с файловой системой Azure Data Lake Storage 1-го поколения. Дополнительные сведения см. в [справочнике по модулю файловой системы Azure Data Lake Store](https://azure-datalake-store.readthedocs.io/en/latest/).

Чтобы установить модули, используйте следующие команды.

```
pip install azure-mgmt-resource
pip install azure-mgmt-datalake-store
pip install azure-datalake-store
```

## <a name="create-a-new-python-application"></a>Создание приложения Python

1. Для создания приложения Python используйте IDE по своему усмотрению, например **mysample.py**.

2. Чтобы импортировать необходимые модули, добавьте следующий фрагмент кода.

    ```
    ## Use this only for Azure AD service-to-service authentication
    from azure.common.credentials import ServicePrincipalCredentials

    ## Use this only for Azure AD end-user authentication
    from azure.common.credentials import UserPassCredentials

    ## Use this only for Azure AD multi-factor authentication
    from msrestazure.azure_active_directory import AADTokenCredentials

    ## Required for Data Lake Storage Gen1 account management
    from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
    from azure.mgmt.datalake.store.models import DataLakeStoreAccount

    ## Required for Data Lake Storage Gen1 filesystem management
    from azure.datalake.store import core, lib, multithread

    # Common Azure imports
    from azure.mgmt.resource.resources import ResourceManagementClient
    from azure.mgmt.resource.resources.models import ResourceGroup

    ## Use these as needed for your application
    import logging, getpass, pprint, uuid, time
    ```

3. Сохраните изменения в mysample.py.

## <a name="authentication"></a>Authentication

В этом разделе мы рассмотрим различные способы проверки подлинности в Azure AD. Доступны следующие варианты.

* Дополнительные сведения о проверке подлинности пользователей в приложении см. в статье [Аутентификация пользователей в Data Lake Store с помощью Python](data-lake-store-end-user-authenticate-python.md).
* Дополнительные сведения о проверке подлинности между службами в приложении см. в статье [Проверка подлинности между службами в Data Lake Storage 1-го поколения с использованием Python](data-lake-store-service-to-service-authenticate-python.md).

## <a name="create-client-and-data-lake-storage-gen1-account"></a>Создание клиента и учетной записи Data Lake Storage 1-го поколения

Следующий фрагмент кода сначала создает клиент учетной записи Data Lake Storage 1-го поколения. Затем он использует объект клиента для создания учетной записи Data Lake Storage 1-го поколения. И наконец, он создает объект клиента файловой системы.

    ## Declare variables
    subscriptionId = 'FILL-IN-HERE'
    adlsAccountName = 'FILL-IN-HERE'
    resourceGroup = 'FILL-IN-HERE'
    location = 'eastus2'

    ## Create Data Lake Storage Gen1 account management client object
    adlsAcctClient = DataLakeStoreAccountManagementClient(armCreds, subscriptionId)

    ## Create a Data Lake Storage Gen1 account
    adlsAcctResult = adlsAcctClient.account.create(
        resourceGroup,
        adlsAccountName,
        DataLakeStoreAccount(
            location=location
        )
    ).wait()

    
## <a name="list-the-data-lake-storage-gen1-accounts"></a>Перечисление учетных записей Data Lake Storage 1-го поколения

    ## List the existing Data Lake Storage Gen1 accounts
    result_list_response = adlsAcctClient.account.list()
    result_list = list(result_list_response)
    for items in result_list:
        print(items)

## <a name="delete-the-data-lake-storage-gen1-account"></a>Удаление учетной записи Data Lake Storage 1-го поколения

    ## Delete an existing Data Lake Storage Gen1 account
    adlsAcctClient.account.delete(adlsAccountName)
    

## <a name="next-steps"></a>Дальнейшие действия
* [Операции файловой системы в Data Lake Storage 1-го поколения c использованием Python](data-lake-store-data-operations-python.md).

## <a name="see-also"></a>См. также

* [Справочник по azure-datalake-store для Python. Файловая система](https://azure-datalake-store.readthedocs.io/en/latest)
* [Приложения больших данных с открытым исходным кодом, которые работают с Azure Data Lake Storage 1-го поколения](data-lake-store-compatible-oss-other-applications.md)
