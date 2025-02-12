---
title: Доменные службы Azure Active Directory. Политика паролей | Документация Майкрософт
description: Общие сведения о политиках паролей в управляемых доменах
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 1a14637e-b3d0-4fd9-ba7a-576b8df62ff2
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2019
ms.author: iainfou
ms.openlocfilehash: ecf38543b2c4e5187aa5c6593c3bccf6668b8a8a
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2019
ms.locfileid: "67472766"
---
# <a name="password-and-account-lockout-policies-on-managed-domains"></a>Политики паролей и блокировки учетных записей в управляемых доменах
В этой статье описываются политики паролей по умолчанию в управляемом домене. В ней также описывается, как вы можете настроить эти политики.

## <a name="fine-grained-password-policies-fgpp"></a>Детальные политики паролей
Детальные политики паролей позволяют указать несколько политик паролей в одном домене. С их помощью можно применять различные ограничения для политик паролей и блокировки учетных записей для разных наборов пользователей в домене. Например, вы можете применить строгие настройки пароля к привилегированным учетным записям.

С помощью детальных политик паролей можно настроить следующие параметры паролей:
* Минимальная длина пароля
* Журнал паролей
* Требование соответствия паролей требованиям к сложности.
* Минимальный срок действия пароля.
* Максимальный срок действия пароля.
* Политика блокировки учетных записей:
    * длительность блокировки учетных записей;
    * разрешенное число неудачных попыток входа в систему;
    * время, по истечении которого сбрасываются неудачные попытки входа в систему.


## <a name="default-fine-grained-password-policy-settings-on-a-managed-domain"></a>Стандартные параметры детальной политики паролей в управляемом домене
На следующем снимке экрана показана детальная политика паролей по умолчанию, настроенная в управляемом домене доменных служб Azure AD.

![Детальная политика паролей по умолчанию](./media/how-to/default-fgpp.png)

> [!NOTE]
> Вы не можете изменять или удалять встроенную детальную политику паролей по умолчанию. Участники группы "Администраторы контроллера домена AAD" могут создавать пользовательские детальные политики паролей и настраивать их для переопределения (с более высоким приоритетом) такой встроенной политики по умолчанию.
>
>

## <a name="password-policy-settings"></a>Параметры политики паролей
В управляемом домене по умолчанию настроены следующие политики паролей:
* Минимальная длина пароля (знаков): 7
* Максимальный срок действия пароля (время существования): 90 дней
* Требование соответствия паролей требованиям к сложности.

### <a name="account-lockout-settings"></a>Параметры блокировки учетных записей
В управляемом домене по умолчанию настроены следующие политики блокировки учетных записей:
* Длительность блокировки учетных записей: 30
* Разрешенное число неудачных попыток входа в систему: 5
* Время, по истечении которого сбрасываются неудачные попытки входа в систему: 30 минут

По сути, учетные записи пользователей блокируются на 30 минут, если в течение 2 минут используются пять недействительных паролей. Учетные записи автоматически разблокируются через 30 минут.


## <a name="create-a-custom-fine-grained-password-policy-fgpp-on-a-managed-domain"></a>Создание пользовательской детальной политики паролей в управляемом домене
Вы можете создать пользовательскую детальную политику паролей и применить ее к определенным группам в управляемом домене. Эта конфигурация эффективно переопределяет стандартную детальную политику паролей, настроенную для управляемого домена.

> [!TIP]
> Только участники группы **Администраторы контроллера домена AAD** имеют разрешения на создание пользовательских детальных политик паролей.
>
>

Кроме того, вы также можете создавать пользовательские детальные политики паролей и применять их к любым настраиваемым подразделениям, которые вы создаете в управляемом домене.

Вы можете настроить пользовательскую детальную политику паролей по следующим причинам:
* Чтобы установить другую политику блокировки учетных записей.
* Чтобы настроить параметр времени существования пароля по умолчанию для управляемого домена.

Чтобы создать пользовательскую детальную политику паролей в управляемом домене:
1. Войдите на виртуальную машину Windows, используемую для администрирования управляемого домена. Если у вас ее нет, следуйте инструкциям, чтобы [управление домен доменных служб Azure AD](manage-domain.md).
2. Запустите **центр администрирования Active Directory** на виртуальной машине.
3. Щелкните имя домена (например, contoso100.com).
4. Дважды щелкните **Система**, чтобы открыть контейнер системы.
5. Дважды щелкните **Контейнер параметров паролей**.
6. Вы увидите встроенную детальную политику паролей по умолчанию для управляемого домена с именем **AADDSSTFPSO**. Вы не можете изменить эту встроенную политику. Однако можно создать новую пользовательскую детальную политику паролей, которая переопределяет политику по умолчанию.
7. На панели **Задачи** в правой области выберите **Создать** и щелкните **Параметры паролей**.
8. В диалоговом окне **создания параметров пароля** укажите пользовательские параметры паролей, которые должны применяться как часть пользовательской детальной политики паролей. Не забудьте задать приоритет так, чтобы переопределить детальную политику паролей по умолчанию.

   ![Создание пользовательской детальной политики паролей](./media/how-to/custom-fgpp.png)

   > [!TIP]
   > **Не забудьте снять флажок Protect from accidental deletion (Защитить от случайного удаления).** Если этот параметр выбран, детальная политика паролей не может быть сохранена.
   >
   >

9. В разделе **Directly Applies To** (Применяется напрямую к) нажмите кнопку **Добавить**. В диалоговом окне **Select Users or Groups** (Выбор пользователей или групп) нажмите кнопку **Расположения**.

   ![Выбор элемента "Пользователи и группы"](./media/how-to/fgpp-applies-to.png)

10. В диалоговом окне **Расположения** разверните имя домена и щелкните **AADDC Users** (Пользователи контроллера домена AAD). Теперь можно выбрать группу из встроенного подразделения пользователей, к которой будет применяться детальная политика паролей.

    ![Выбор подразделения, к которому принадлежит группа](./media/how-to/fgpp-container.png)

11. Введите имя группы и нажмите кнопку **Проверить имена**, чтобы проверить существование группы.

    ![Выбор группы для применения детальной политики паролей](./media/how-to/fgpp-apply-group.png)

12. Имя группы отображается в разделе **Directly Applies To** (Применяется напрямую к). Нажмите кнопку **ОК**, чтобы сохранить эти изменения.

    ![Примененная детальная политика паролей](./media/how-to/fgpp-applied.png)

> [!TIP]
> **Применение настраиваемых политик паролей для учетных записей пользователей в настраиваемом подразделении:** детальные политики паролей могут применяться только к группам. Чтобы настроить пользовательскую политику паролей только для пользователей из настраиваемого подразделения, создайте группу, включающую пользователей в этом подразделении.
>
>

## <a name="next-steps"></a>Дальнейшие действия
* [Дополнительные сведения о детальных политиках паролей для Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770394(v=ws.10))
* [Сведения о настройке детальных политик паролей в центре администрирования AD](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/adac/introduction-to-active-directory-administrative-center-enhancements--level-100-#fine_grained_pswd_policy_mgmt)
