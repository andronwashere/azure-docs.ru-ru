---
title: Настройка репликации для виртуальных машин с поддержкой шифрования дисков Azure в Azure Site Recovery | Документация Майкрософт
description: В этой статье описывается, как настроить репликацию для виртуальных машин с поддержкой шифрования дисков Azure из одного региона Azure в другой с помощью Site Recovery.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 04/08/2019
ms.author: sutalasi
ms.openlocfilehash: b000610b5cba6f768a629ad797500a57597f2569
ms.sourcegitcommit: e9c866e9dad4588f3a361ca6e2888aeef208fc35
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68335697"
---
# <a name="replicate-azure-disk-encryption-enabled-virtual-machines-to-another-azure-region"></a>Репликация виртуальных машин с поддержкой шифрования дисков Azure в другой регион Azure

В этой статье описывается, как реплицировать виртуальные машины с поддержкой шифрования дисков Azure из одного региона Azure в другой.

>[!NOTE]
>В настоящее время Azure Site Recovery поддерживает только виртуальные машины Azure под управлением ОС Windows, [для которых включено шифрование с помощью Azure Active Directory (Azure AD)](https://aka.ms/ade-aad-app).

## <a id="required-user-permissions"></a>Необходимые разрешения пользователя
Site Recovery требуется, чтобы пользователь имел разрешения на создание хранилища ключей в целевом регионе и копирование ключей в этот регион.

Чтобы включить репликацию виртуальных машин с поддержкой шифрования дисков из портал Azure, пользователю требуются следующие разрешения:

- Разрешения по отношению к хранилищу ключей:
    - Перечислить, создать и получить
    
- Разрешения по отношению к секретам хранилища ключей:
    - Операции управления секретами
        - Get, List и Set
    
- Разрешения ключа хранилища ключей (требуется только в том случае, если виртуальные машины используют ключ шифрования ключей для шифрования ключей шифрования диска).
    - Операции управления ключами
        - Get, List и CREATE
    - Операции шифрования
        - Расшифровка и шифрование

Для управления разрешениями перейдите к ресурсу хранилища ключей на портале. Добавьте необходимые разрешения для пользователя. В следующем примере показано, как включить разрешения для *ContosoWeb2Keyvault*хранилища ключей, который находится в исходном регионе.

1. Перейдите к **домашней** > **Keyvaults** > **ContosoWeb2KeyVault > политики доступа**.

   ![Окно разрешений хранилища ключей](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-1.png)

2. Вы видите, что нет пользовательских разрешений. Нажмите кнопку **Добавить**. Введите сведения о пользователях и разрешениях.

   ![Разрешения Keyvault](./media/azure-to-azure-how-to-enable-replication-ade-vms/key-vault-permission-2.png)

Если пользователь, который включает аварийное восстановление, не имеет разрешений на копирование ключей, администратор безопасности с соответствующими разрешениями может использовать следующий скрипт для копирования секретов и ключей шифрования в целевой регион.

Чтобы устранить неполадки, ознакомьтесь с разрешениями на [доступ к хранилищу ключей](#trusted-root-certificates-error-code-151066) далее в этой статье.

>[!NOTE]
>Чтобы включить репликацию виртуальных машин с поддержкой шифрования дисков с портала, необходимы по крайней мере разрешения "список" для хранилищ ключей, секретов и ключей.

## <a name="copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script"></a>Копирование ключей шифрования дисков в регион аварийного восстановления с помощью сценария PowerShell

1. [Откройте необработанный код скрипта "копикэйс"](https://aka.ms/ade-asr-copy-keys-code).
2. Скопируйте сценарий в файл и назовите его **копи-Кэйс. ps1**.
3. Откройте приложение Windows PowerShell и перейдите в папку, в которую был сохранен файл.
4. Выполните копи-Кэйс. ps1.
5. Укажите учетные данные Azure для входа.
6. Выберите **подписку Azure**, к которой относятся виртуальные машины.
7. Дождитесь загрузки групп ресурсов, а затем выберите **группу ресурсов** для виртуальных машин.
8. Выберите виртуальные машины из отображаемого списка. В списке отображаются только виртуальные машины, для которых включено шифрование дисков.
9. Выберите **целевое расположение**.

    - **Хранилища ключей шифрования дисков**
    - **Хранилища ключей шифрования ключей**

   По умолчанию Site Recovery создает новое хранилище ключей в целевом регионе. Имя хранилища имеет суффикс "ASR", основанный на ключах шифрования диска исходной виртуальной машины. Если хранилище ключей уже существует, созданное Site Recovery, оно будет использовано повторно. При необходимости выберите другое хранилище ключей из списка.

## <a name="enable-replication"></a>Включение репликации

В этом примере первичный регион Azure Восточная Азия, а дополнительный регион — Южный Восточная Азия.

1. В хранилище выберите **+ реплицировать**.
2. Обратите внимание на следующие поля.
    - **Источник**. Источник происхождения виртуальных машин, в нашем примере это **Azure**.
    - **Расположение источника**. Регион Azure, в котором вы хотите защитить виртуальные машины. В этом примере исходное расположение — "Восточная Азия".
    - **Модель развертывания**. Модель развертывания Azure на исходных компьютерах.
    - **Исходная подписка**. Подписка, к которой принадлежат исходные виртуальные машины. Это может быть любая подписка, которая находится в том же Azure Active Directory клиенте, что и хранилище служб восстановления.
    - **Группа ресурсов**. Группа ресурсов, к которой принадлежат исходные виртуальные машины. Все виртуальные машины в выбранной группе ресурсов перечисляются для защиты на следующем шаге.

3. В окне **виртуальные машины** > **выберите виртуальные**машины, а затем выберите каждую виртуальную машину, которую требуется реплицировать. Можно выбрать только те компьютеры, для которых можно включить репликацию. Нажмите кнопку **ОК**.

4. В окне **Параметры**можно настроить следующие параметры целевого сайта.

    - **Целевое расположение**. Расположение, в которое будут реплицированы данные исходных виртуальных машин. Site Recovery предоставляет список подходящих целевых регионов на основе расположения выбранного компьютера. Рекомендуется использовать то же расположение, что и для хранилища служб восстановления.
    - **Целевая подписка**. Целевая подписка, используемая для аварийного восстановления. По умолчанию целевая подписка совпадает с исходной подпиской.
    - **Целевая группа ресурсов**. Группа ресурсов, в которой будут находиться все реплицированные виртуальные машины. По умолчанию Site Recovery создает новую группу ресурсов в целевом регионе. Имя получает суффикс "ASR". Если группа ресурсов, созданная Azure Site Recovery, уже существует, она используется повторно. Вы также можете настроить его, как показано в следующем разделе. Расположение целевой группы ресурсов может быть любым регионом Azure, кроме региона, в котором размещаются исходные виртуальные машины.
    - **Целевая виртуальная сеть**. По умолчанию Site Recovery создает новую виртуальную сеть в целевом регионе. Имя получает суффикс "ASR". Он сопоставлен с вашей исходной сетью и используется для защиты в будущем. [Узнайте больше](site-recovery-network-mapping-azure-to-azure.md) о сетевом сопоставлении.
    - **Целевые учетные записи хранения (если на исходной виртуальной машине не используются управляемые диски)** : По умолчанию Site Recovery создает новую целевую учетную запись хранения, копируя конфигурацию хранилища исходной виртуальной машины. Если учетная запись хранения уже существует, она используется повторно.
    - **Управляемые диски реплики (если исходная виртуальная машина использует управляемые диски)** : Site Recovery создает новые управляемые диски реплики в целевом регионе для отражения управляемых дисков исходной виртуальной машины того же типа ("Стандартный" или "Премиум"), что и управляемые диски исходной виртуальной машины.
    - **Учетные записи хранения кэша**. Site Recovery требуется Вспомогательная учетная запись хранения, называемая *хранилищем кэша* в исходном регионе. Все изменения на исходных виртуальных машинах будут записываться и отправляться в учетную запись хранения кэша. Затем они реплицируются в целевое расположение.
    - **Группа доступности**. По умолчанию Site Recovery создает новую группу доступности в целевом регионе. Имя содержит суффикс "ASR". Если группа доступности, созданная с помощью Site Recovery, уже существует, она используется повторно.
    - **Хранилища ключей для шифрования дисков**. По умолчанию Site Recovery создает новое хранилище ключей в целевом регионе. У него есть суффикс "ASR", основанный на ключах шифрования диска исходной виртуальной машины. Если хранилище ключей, созданное Azure Site Recovery, уже существует, оно будет использовано повторно.
    - **Хранилища ключей для шифрования ключей**. По умолчанию Site Recovery создает новое хранилище ключей в целевом регионе. Имя содержит суффикс "ASR", основанный на ключах шифрования ключей исходной виртуальной машины. Если хранилище ключей, созданное Azure Site Recovery, уже существует, оно будет использовано повторно.
    - **Политика репликации**. Определяет параметры для журнала хранения точек восстановления и частоты создания моментальных снимков с постоянными приложениями. По умолчанию Site Recovery создает новую политику репликации с параметрами по умолчанию *24 часа* для хранения точек восстановления и *60 минут* для периодичности создания моментальных снимков с учетом приложений.

## <a name="customize-target-resources"></a>Настройка целевых ресурсов

Выполните следующие действия, чтобы изменить целевые параметры Site Recovery умолчанию.

1. Нажмите кнопку **настроить** рядом с пунктом "Целевая подписка", чтобы изменить целевую подписку по умолчанию. Выберите подписку из списка подписок, доступных в клиенте Azure AD.

2. Нажмите кнопку **настроить** рядом с пунктом группа ресурсов, сеть, хранилище и группы доступности, чтобы изменить следующие параметры по умолчанию.
    - В поле **Целевая группа ресурсов**выберите группу ресурсов из списка групп ресурсов в целевом расположении подписки.
    - В поле **Целевая виртуальная сеть**выберите сеть из списка виртуальных сетей в целевом расположении.
    - Для **группы доступности**можно добавить параметры группы доступности в виртуальную машину, если они входят в группу доступности в исходном регионе.
    - В поле **целевые учетные записи хранения**выберите учетную запись для использования.

2. Нажмите кнопку **настроить** рядом с пунктом Параметры шифрования, чтобы изменить следующие параметры по умолчанию.
   - Для **хранилища ключей шифрования целевого диска**выберите хранилище ключей шифрования целевого диска из списка хранилищ ключей в целевом расположении подписки.
   - Для **хранилища ключей шифрования целевого**ключа выберите целевое хранилище ключей шифрования из списка хранилищ ключей в целевом расположении подписки.

3. Выберите **создать целевой ресурс** > **включить репликацию**.
4. После включения репликации для виртуальных машин можно проверить состояние работоспособности виртуальных машин в разделе реплицированные **элементы**.

>[!NOTE]
>Во время начальной репликации состояние обновления может занять некоторое время, без видимого хода выполнения. Нажмите кнопку **Обновить** , чтобы получить Последнее состояние.

## <a name="update-target-vm-encryption-settings"></a>Обновление параметров шифрования целевой виртуальной машины
В следующих сценариях потребуется обновить параметры шифрования целевой виртуальной машины:
  - Вы включили Site Recoveryную репликацию на виртуальной машине. Позже вы включили шифрование дисков на исходной виртуальной машине.
  - Вы включили Site Recoveryную репликацию на виртуальной машине. Позже вы изменили ключ шифрования диска или ключ шифрования ключа на исходной виртуальной машине.

Можно использовать [Скрипт](#copy-disk-encryption-keys-to-the-dr-region-by-using-the-powershell-script) , чтобы скопировать ключи шифрования в целевой регион, а затем обновить целевые параметры шифрования в**свойствах**  >  *реплицированного элемента* >  **хранилища** > служб восстановления. **Вычисление и сеть**.

![Диалоговое окно "обновить параметры ADE"](./media/azure-to-azure-how-to-enable-replication-ade-vms/update-ade-settings.png)

## <a id="trusted-root-certificates-error-code-151066"></a>Устранение проблем с разрешениями хранилища ключей во время репликации виртуальной машины Azure в Azure

Azure Site Recovery требуется по крайней мере разрешение на чтение в хранилище ключей исходного региона и разрешение на запись в хранилище ключей целевого региона для чтения секрета и его копирования в хранилище ключей целевого региона. 

**Причина 1**. У вас нет разрешения "получить" в **хранилище ключей исходного кода** для чтения ключей. </br>
**Как исправить:** Независимо от того, являетесь ли вы администратором подписки, важно иметь разрешение Get для хранилища ключей.

1. Перейдите к хранилищу ключей исходного кода, в данном примере — "Контососаурцекэйваулт" > **политик доступа** . 
2. В разделе **Выбор субъекта** добавьте имя пользователя, например "dradmin@contoso.com".
3. В разделе **ключевые разрешения** выберите получить. 
4. В разделе **разрешение секрета** выберите получить. 
5. Сохранение политики доступа

**Причина 2.** У вас нет необходимых разрешений для записи ключей в **хранилище ключей целевого региона** . </br>

*Пример*. Вы пытаетесь выполнить репликацию виртуальной машины, имеющей хранилище ключей *контососаурцекэйваулт* , в исходном регионе.
У вас есть все разрешения в хранилище ключей исходного региона. Но во время защиты вы можете выбрать уже созданное хранилище ключей Контосотаржеткэйваулт, у которого нет разрешений. Возникает ошибка.

Требуется разрешение в [целевом хранилище ключей](#required-user-permissions)

**Как исправить:** Перейдите к **домашней** > **Keyvaults** > контосотаржеткэйваултдоступ > **к политикам** и добавьте соответствующие разрешения.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о выполнении тестовой отработки отказа см. в [этой статье](site-recovery-test-failover-to-azure.md).
