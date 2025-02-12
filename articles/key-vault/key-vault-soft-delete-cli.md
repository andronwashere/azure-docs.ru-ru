---
title: Как использовать обратимое удаление в Azure Key Vault с помощью интерфейса командной строки
description: Примеры использования обратимого удаления с фрагментами кода для интерфейса командной строки.
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: mbaldwin
ms.openlocfilehash: aa9b89b9afec069e97236b7652e0f1d37644f5cf
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60640486"
---
# <a name="how-to-use-key-vault-soft-delete-with-cli"></a>Как использовать обратимое удаление в Key Vault с помощью интерфейса командной строки

Функция обратимого удаления в Azure Key Vault позволяет восстанавливать удаленные хранилища и объекты хранилищ. В частности, обратимое удаление применяется в следующих сценариях:

- Поддержка восстанавливаемого удаления хранилища ключей
- Поддержка восстанавливаемого удаления объектов хранилища ключей (например, ключей, секретов и сертификатов).

## <a name="prerequisites"></a>Технические условия

- Azure CLI. Если в вашей среде этот инструмент не установлен, ознакомьтесь со статьей [Управление Key Vault с помощью интерфейса командной строки 2.0](key-vault-manage-with-cli2.md).

Дополнительные сведения об использовании интерфейса командной строки с Key Vault приведены в [справочнике по Azure CLI для Key Vault](https://docs.microsoft.com/cli/azure/keyvault).

## <a name="required-permissions"></a>Необходимые разрешения

Операции Key Vault контролируются отдельно, посредством разрешений управления доступом на основе ролей (RBAC). Это осуществляется следующим образом.

| Операция | Описание | Разрешение пользователя |
|:--|:--|:--|
|список|Выводит список удаленных хранилищ ключей.|Microsoft.KeyVault/deletedVaults/read|
|Recover|Восстанавливает удаленное хранилище ключей.|Microsoft.KeyVault/vaults/write|
|Purge|Окончательно удаляет удаленное хранилище ключей и все его содержимое.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Дополнительные сведения о разрешениях и управлении доступом см. в разделе [Защита хранилища ключей](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Включение обратимого удаления

Включите обратимое удаление, чтобы разрешить восстановление удаленного хранилища ключей или хранящихся в нем объектов.

> [!IMPORTANT]
> Включение обратимого удаления в хранилище ключей необратимо. После установки для свойства обратимого удаления значения true его нельзя изменить или удалить.  

### <a name="existing-key-vault"></a>Существующее хранилище ключей

Включите обратимое удаление для существующего хранилища ключей ContosoVault следующим образом. 

```azurecli
az resource update --id $(az keyvault show --name ContosoVault -o tsv | awk '{print $1}') --set properties.enableSoftDelete=true
```

### <a name="new-key-vault"></a>Новое хранилище ключей

Обратимое удаление для нового хранилища ключей включается во время создания путем добавления флага --enable-soft-delete в команду create.

```azurecli
az keyvault create --name ContosoVault --resource-group ContosoRG --enable-soft-delete true --location westus
```

### <a name="verify-soft-delete-enablement"></a>Проверка включения обратимого удаления

Чтобы проверить, включено ли обратимое удаление в хранилище ключей, выполните команду *show* и в выходных данных найдите атрибут "Soft Delete Enabled?" :

```azurecli
az keyvault show --name ContosoVault
```

## <a name="deleting-a-soft-delete-protected-key-vault"></a>Удаление хранилища ключей, защищенного через обратимое удаление

В зависимости от того, включено ли обратимое удаление, поведение команды для удаления хранилища ключей может изменяться.

> [!IMPORTANT]
>Если выполнить следующую команду для хранилища ключей, в котором не включено обратимое удаление, оно и его содержимое будут окончательно удалены без возможности восстановления.

```azurecli
az keyvault delete --name ContosoVault
```

### <a name="how-soft-delete-protects-your-key-vaults"></a>Как обратимое удаление защищает хранилище ключей

Если обратимое удаление включено:

- При удалении хранилище ключей удаляется из соответствующей группы ресурсов и помещается в зарезервированное пространство имен, которое связано с расположением, в котором оно было создано. 
- Удаленные объекты, такие как ключи, секреты и сертификаты, становятся недоступными и остаются таковыми, пока содержащее их хранилище ключей пребывает в удаленном состоянии. 
- DNS-имя удаленного хранилища ключей зарезервировано, что предотвращает создание хранилища ключей с тем же именем.  

Чтобы просмотреть хранилища ключей в удаленном состоянии, связанные с вашей подпиской, можно выполнить следующую команду.

```azurecli
az keyvault list-deleted
```
- *Идентификатор* можно использовать для определения ресурса при восстановлении или очистке. 
- *Идентификатор ресурса* — это исходный идентификатор ресурса этого хранилища. Так как это хранилище ключей теперь находится в удаленном состоянии, ресурс с таким идентификатором ресурса не существует. 
- Поле *запланированной даты очистки* указывает, когда хранилище ключей будет окончательно удалено, если не предпринять какое-либо действие. Срок хранения по умолчанию, используемый для вычисления *запланированной даты очистки*, составляет 90 дней.

## <a name="recovering-a-key-vault"></a>Восстановление хранилища ключей

Чтобы восстановить хранилище ключей, необходимо указать его имя, группу ресурсов и расположение. Запишите расположение и группу ресурсов удаленного хранилища ключей, так как их необходимо знать, чтобы осуществить восстановление.

```azurecli
az keyvault recover --location westus --resource-group ContosoRG --name ContosoVault
```

После восстановления хранилища ключей вы получите новый ресурс с исходным идентификатором ресурса хранилища ключей. Если исходная группа ресурсов удалена, то прежде чем выполнить восстановление, нужно создать группу ресурсов с тем же именем.

## <a name="deleting-and-purging-key-vault-objects"></a>Удаление и очистка объектов хранилища ключей

Приведенная ниже команда удалит ключ ContosoFirstKey из хранилища ключей ContosoVault, в котором включено обратимое удаление:

```azurecli
az keyvault key delete --name ContosoFirstKey --vault-name ContosoVault
```

Если в хранилище ключей включено обратимое удаление, то удаленный ключ отображается как удаленный, за исключением случаев, когда вы явным образом выводите список удаленных ключей или извлекаете их. Большинство операций с ключом в удаленном состоянии завершится сбоем, кроме вывода списка удаленных ключей, восстановления этого ключа или его удаления. 

Например, чтобы запросить список удаленных ключей в хранилище ключей, используйте следующую команду.

```azurecli
az keyvault key list-deleted --vault-name ContosoVault
```

### <a name="transition-state"></a>Переходное состояние 

Удаление ключа в хранилище ключей, в котором включено обратимое удаление, может занять несколько секунд. Во время этого переходного состояния может создаться впечатление, что ключ не находится ни в активном, ни в удаленном состоянии. 

### <a name="using-soft-delete-with-key-vault-objects"></a>Использование обратимого удаления с объектами хранилища ключей

Как и хранилища ключей, удаленный ключ, секрет или сертификат останется в удаленном состоянии в течение 90 дней, если его не восстановить или не очистить.

#### <a name="keys"></a>ключей

Чтобы восстановить обратимо удаленный ключ, выполните команду, указанную ниже:

```azurecli
az keyvault key recover --name ContosoFirstKey --vault-name ContosoVault
```

Чтобы удалить без возможности восстановления обратимо удаленный ключ, выполните команду, указанную ниже:

> [!IMPORTANT]
> Очистка ключа приведет к его окончательному удалению, то есть восстановить его будет невозможно. 

```azurecli
az keyvault key purge --name ContosoFirstKey --vault-name ContosoVault
```

Для действий **восстановления** и **очистки** в политике доступа хранилища ключей заданы собственные разрешения. Чтобы пользователь или субъект-служба могли выполнить **восстановление** или **очистку**, они должны иметь соответствующее разрешение для этого ключа или секрета. По умолчанию разрешение на **очистку** не добавляется в политику доступа хранилища ключей, если для предоставления всех разрешений используется параметр "Все". Необходимо отдельно предоставить разрешение на **очистку**. 

#### <a name="set-a-key-vault-access-policy"></a>Настройка политики доступа хранилища ключей

Приведенная ниже команда предоставляет разрешение user@contoso.com на выполнение нескольких операций с ключами в *ContosoVault*, включая **очистку**.

```azurecli
az keyvault set-policy --name ContosoVault --key-permissions get create delete list update import backup restore recover purge
```

>[!NOTE] 
> Если у вас есть хранилище ключей, в котором только что было включено обратимое удаление, то у вас может не быть разрешений на **восстановление** и **очистку**.

#### <a name="secrets"></a>Секреты

Секретами (как и ключами) можно управлять с помощью специальных команд:

- Удаление секрета SQLPassword. 
  ```azurecli
  az keyvault secret delete --vault-name ContosoVault -name SQLPassword
  ```

- Вывод списка всех удаленных секретов в хранилище ключей. 
  ```azurecli
  az keyvault secret list-deleted --vault-name ContosoVault
  ```

- Восстановление секрета в удаленном состоянии. 
  ```azurecli
  az keyvault secret recover --name SQLPassword --vault-name ContosoVault
  ```

- Очистка секрета в удаленном состоянии. 

  > [!IMPORTANT]
  > Очистка секрета приведет к его окончательному удалению, то есть восстановить его будет невозможно. 

  ```azurecli
  az keyvault secret purge --name SQLPAssword --vault-name ContosoVault
  ```

## <a name="purging-a-soft-delete-protected-key-vault"></a>Очистка хранилища ключей, защищенного через обратимое удаление

> [!IMPORTANT]
> Очистка хранилища ключей или одного из содержащихся в нем объектов приведет к его окончательному удалению, то есть восстановить его будет невозможно.

Функция очистки используется, чтобы полностью удалить объект хранилища ключей или всей хранилища ключей, который был ранее обратимо удаленные. Как показано в предыдущем разделе, объекты, содержащиеся в хранилище ключей со включенной функцией обратимого удаления, могут быть в нескольких состояниях:

- **Активные**: перед удалением.
- **Обратимо удалены**: после удаления; можно отобразить в списке и вернуть обратно в активное состояние.
- **Окончательно удалены**: после очистки; невозможно восстановить.

То же самое относится и к хранилищу ключей. Чтобы окончательно удалить обратимо удаленное хранилище ключей и его содержимое, необходимо очистить само хранилище ключей.

### <a name="purging-a-key-vault"></a>Очистка хранилища ключей

При очистке хранилища ключей все его содержимое, включая ключи, секреты и сертификаты, удаляется без возможности восстановления. Для очистки обратимо удаленного хранилища ключей используйте команду `az keyvault purge`. Найти расположение удаленных хранилищ ключей для своей подписки можно с помощью команды `az keyvault list-deleted`.

```azurecli
az keyvault purge --location westus --name ContosoVault
```

### <a name="purge-permissions-required"></a>Необходимые разрешения на очистку
- Чтобы очистить удаленное хранилище ключей, пользователю нужно разрешение RBAC на выполнение операции *Microsoft.KeyVault/locations/deletedVaults/purge/action*. 
- Чтобы вывести в списке удаленное хранилище ключей, пользователю нужно разрешение RBAC на выполнение операции *Microsoft.KeyVault/deletedVaults/read*. 
- По умолчанию администратор подписки имеет эти разрешения. 

### <a name="scheduled-purge"></a>Очистка по расписанию

При выводе списка объектов удаленного хранилища ключей также отображается запланированное время их очистки службой Key Vault. Поле *запланированной даты очистки* указывает, когда объект хранилища ключей будет окончательно удален, если не предпринять какое-либо действие. По умолчанию срок хранения объекта удаленного хранилища ключей составляет 90 дней.

>[!IMPORTANT]
>После очистки объекта хранилища, активированной значением его поля *Scheduled Purge Date* (Запланированная дата очистки), этот объект будет окончательно удален. Восстановить его будет невозможно.

## <a name="enabling-purge-protection"></a>Включение защиты от очистки

При использовании очистки защиты в хранилище или объект в удаленных не удается очистить состояние, пока не истечет срок 90 дней. Такое хранилище или его объект все еще подлежат восстановлению. Этот компонент предоставляет ли он хранилища или объект не может быть окончательно удалить, пока срок хранения период истек.

Защита от очистки можно включить только в том случае, если обратимого удаления включена. 

Чтобы включить оба обратимое удаление и очистку защиты при создании хранилища, используйте [создать az keyvault](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-create) команды:

```
az keyvault create --name ContosoVault --resource-group ContosoRG --location westus --enable-soft-delete true --enable-purge-protection true
```

Для добавления защиты от очистки для существующего хранилища (с уже включенным обратимым удалением) введите [az keyvault update](/cli/azure/keyvault?view=azure-cli-latest#az-keyvault-update) команды:

```
az keyvault update --name ContosoVault --resource-group ContosoRG --enable-purge-protection true
```

## <a name="other-resources"></a>Другие ресурсы

- Обзор функции обратимого удаления Key Vault см. в разделе [Общие сведения об обратимом удалении в Azure Key Vault](key-vault-ovw-soft-delete.md).
- Общие сведения об использовании Azure Key Vault см. в [этой статье](key-vault-overview.md).

