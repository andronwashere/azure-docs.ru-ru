---
title: Отключение доменных служб Azure Active Directory | Документация Майкрософт
description: Отключение доменных служб Azure Active Directory с помощью портала Azure
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: 2f9b7f5342753c27bd8159056c8f162a5a4b78d3
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2019
ms.locfileid: "67473550"
---
# <a name="disable-azure-active-directory-domain-services-using-the-azure-portal"></a>Отключение доменных служб Azure Active Directory с помощью портала Azure
В этой статье объясняется, как с помощью портала Azure отключить доменные службы Azure Active Directory (AD) для каталога Azure AD.

> [!WARNING]
> **Удаление необратимо, и его невозможно отменить.**
> Выполняйте дальнейшие действия с осторожностью. При удалении управляемого домена происходит следующее:
>   * Контроллеры управляемого домена отзываются и удаляются из виртуальной сети.
>   * Данные управляемого домена удаляются без возможности восстановления. К ним относятся организационные подразделения, объекты групповой политики, пользовательские записи DNS, субъекты-службы, групповые управляемые учетные записи службы и другие данные, созданные в управляемом домене.
>   * Компьютеры, присоединенные к управляемому домену, теряют отношение доверия с доменом и должны отключиться от него.
>   * Вы не сможете войти на эти компьютеры, используя корпоративные учетные данные AD. Вместо этого используйте учетные данные локального администратора для компьютера.
> При удалении управляемого домена каталог Azure AD сохраняется, и удаление никак на нем не отразится.

Чтобы удалить управляемый домен доменных служб Azure AD, сделайте следующее:
1. Перейдите к [расширению доменных служб Azure AD](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) на портале Azure.
2. Щелкните имя управляемого домена.

    ![Выбор домена для удаления](./media/getting-started/domain-services-delete-select-domain.png)

3. На странице **Обзор** нажмите кнопку **Удалить**.

    ![Удаление домена](./media/getting-started/domain-services-delete-domain.png)

4. Чтобы подтвердить удаление, введите DNS-имя управляемого домена. Когда все будет готово, нажмите кнопку **Удалить**.

    ![Подтверждение удаления домена](./media/getting-started/domain-services-delete-domain-confirm.png)

Управляемый домен будет удален через 15–20 минут.

Рассмотрите возможность [отправить отзыв](contact-us.md) , который поможет нам понять, какие возможности убедили бы вас выбрать доменные службы Azure AD в будущем. Ваши отзывы помогают нам развивать службу, чтобы она лучше соответствовала потребностям развертываний и вариантам использования.
