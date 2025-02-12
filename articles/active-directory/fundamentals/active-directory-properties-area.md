---
title: Добавление сведений о конфиденциальности организации в Azure Active Directory | Документация Майкрософт
description: Инструкции по добавлению сведений о конфиденциальности организации в область свойств Azure Active Directory.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: lizross
ms.reviewer: bpham
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 98b4e3585f25a0b081e30c5524678a0f7bb27942
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60248496"
---
# <a name="add-your-organizations-privacy-info-using-azure-active-directory"></a>Добавление сведений о конфиденциальности организации в Azure Active Directory
В этой статье объясняется, как администратор клиента может добавить сведения о конфиденциальности в клиент организации Azure Active Directory (Azure AD) на портале Azure.

Настоятельно рекомендуется добавить контактные данные глобального лица по вопросам конфиденциальности и заявление о конфиденциальности вашей организации, чтобы внутренние сотрудники и внешние гости могли просматривать ваши политики. Так как заявления о конфиденциальности создаются в единственном экземпляре и настраиваются для каждого предприятия отдельно, настоятельно рекомендуем обратиться за помощью к юристу.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

## <a name="add-your-privacy-info-on-azure-ad"></a>Добавление сведений о конфиденциальности в Azure AD
Добавьте сведения о конфиденциальности организации в области **Свойства** Azure AD.

### <a name="to-access-the-properties-area-and-add-your-privacy-information"></a>Получение доступа к области "Свойства" и добавление сведений о конфиденциальности

1.  Войдите на портал Azure как администратор клиента.

2.  На панели навигации слева, выберите **Azure Active Directory**, а затем — **Свойства**.

    Откроется область **Свойства**.

    ![Область свойств Azure AD с выделенной областью для сведений о конфиденциальности](media/active-directory-properties-area/properties-area.png)

3.  Добавьте сведения о конфиденциальности для ваших сотрудников.

    - **Контакт для связи по техническим вопросам.** Введите адрес электронной почты пользователя, к которому в вашей организации можно обратиться за технической поддержкой.
    
    - **Глобальное контактное лицо по вопросам конфиденциальности.** Введите адрес электронной почты пользователя, которому можно отправлять запросы о конфиденциальности личных данных. К этому пользователю также обращается и корпорация Майкрософт в случае нарушения безопасности данных. Если такой пользователь не существует, Майкрософт обращается к вашим глобальным администраторам.

    - **URL-адрес заявления о конфиденциальности.** Введите ссылку на документ вашей организации, описывающий, как ваша организация обрабатывает конфиденциальность данных внутренних и внешних гостей.

        >[!Important]
        >Если не включить заявление о конфиденциальности или контактные данные лица по вопросам конфиденциальности, для внешних гостей в поле **Review Permissions** (Просмотр разрешений) будет отображаться следующее сообщение: **<_название вашей организации_> has not provided links to their terms for you to review** (не предоставляет ссылки для просмотра условий). Например, это сообщение будет отображаться для гостевого пользователя при получении приглашения для получения доступа к организации через службу совместной работы B2B.

        ![Сообщение в поле "Просмотр разрешений "службы совместной работы B2B](media/active-directory-properties-area/active-directory-no-privacy-statement-or-contact.png)

4.  Щелкните **Сохранить**.

## <a name="next-steps"></a>Дальнейшие действия
- [Azure Active Directory B2B collaboration invitation redemption](https://aka.ms/b2bredemption) (Активация приглашения службы совместной работы Azure Active Directory B2B)
- [Добавление или изменение данных профиля пользователя в Azure Active Directory](active-directory-users-profile-azure-portal.md)
