---
title: Что собой представляет гибридная идентификация с использованием Azure Active Directory
description: Гибридная идентификация — это общая система идентификации пользователей для аутентификации и авторизации как в локальной среде, так и в облаке.
keywords: Введение в Azure AD Connect, обзор функций Azure AD Connect, что такое Azure AD Connect, установка Аctive Directory.
services: active-directory
author: billmath
manager: daveba
ms.assetid: 59bd209e-30d7-4a89-ae7a-e415969825ea
ms.service: active-directory
ms.workload: identity
ms.topic: overview
ms.date: 05/17/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c43238d44b2309d105ef14e696a5a16848d0b58
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2019
ms.locfileid: "65896829"
---
# <a name="what-is-hybrid-identity-with-azure-active-directory"></a>Что собой представляет гибридная идентификация с использованием Azure Active Directory

Сегодня предприятия и организации все чаще комбинируют локальные и облачные приложения.  Пользователям требуется доступ к этим приложениям как локально, так и в облаке. Управление пользователями и в локальной среде, и в облаке — непростая задача. 

Решения корпорации Майкрософт для идентификации охватывают как локальные, так и облачные сценарии.  Они позволяют создавать общие пользовательские удостоверения для проверки подлинности и авторизации для всех ресурсов, независимо от их расположения. Мы называем это **гибридной идентификацией**.

Гибридная идентификация Azure AD и механизм управления ею позволяют реализовать такие сложные сценарии.

Обеспечить гибридную идентификацию с использованием Azure AD можно с помощью одного из трех методов проверки подлинности в зависимости от сценария.   Доступны следующие методы: 

- **[синхронизация хэша паролей](whatis-phs.md)** ;  
- **[сквозная проверка подлинности](how-to-connect-pta.md)** ;  
- **[федерация AD FS](whatis-fed.md)** . 

Эти методы проверки подлинности также предоставляют возможности [единого входа](how-to-connect-sso.md).  Единый вход обеспечивает пользователям автоматический вход в систему, когда они работают на корпоративных устройствах, подключенных к корпоративной сети.

См. дополнительные сведения о [выборе правильного метода аутентификации для гибридного решения для идентификации Azure Active Directory](https://docs.microsoft.com/azure/security/azure-ad-choose-authn). 

## <a name="common-scenarios-and-recommendations"></a>Распространенные сценарии и рекомендации 

В этом разделе приводится несколько распространенных сценариев гибридной идентификации и контроля доступа с рекомендациями по выбору оптимального варианта (вариантов) гибридной идентификации в каждом конкретном случае. 

|Мне нужно:|PHS и SSO<sup>1</sup>| PTA и SSO<sup>2</sup> | AD FS<sup>3</sup>| 
|-----|-----|-----|-----| 
|Автоматически синхронизировать с облаком новые учетные записи пользователей, контакты и группы, создаваемые в моем локальном каталоге Active Directory|![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| ![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png) |![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| 
|Настроить мой клиент для гибридных сценариев Office 365.|![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| ![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png) |![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| 
|Разрешить моим пользователям выполнять вход и обращаться к облачным службам с помощью локального пароля.|![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| ![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png) |![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| 
|Реализовать единый вход с использованием учетных данных организации.|![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| ![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png) |![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)|  
|Обеспечить хранение хэша паролей вне облака.| |![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)|![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| 
|Использовать облачные решения для многофакторной проверки подлинности.|![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)|![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)|![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| 
|Использовать локальные решения для Многофакторной идентификации.| | |![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| 
|Обеспечить поддержку проверки подлинности с использованием смарт-карт<sup>4</sup>.| | |![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| 
|Отображать уведомления об истечения срока действия пароля на портале Office и на рабочем столе Windows 10.| | |![Рекомендуется](./media/whatis-hybrid-identity/ic195031.png)| 

> <sup>1</sup> Синхронизация хэша паролей (PHS) и единый вход (SSO). 
> 
> <sup>2</sup> Сквозная проверка подлинности и единый вход.  
> 
> <sup>3</sup> Федеративный единый вход с AD FS.  
>  
> <sup>4</sup> AD FS можно интегрировать с корпоративной инфраструктурой открытых ключей PKI, чтобы разрешить вход с использованием сертификатов. Это могут быть программные сертификаты, развернутые с помощью доверенного канала подготовки, например MDM, GPO, или смарт-сертификаты (включая карты PIV/CAC) или Hello для бизнеса (доверенный сертификат). Дополнительные сведения о поддержке проверки подлинности с использованием смарт-карт см. в [этом блоге](https://blogs.msdn.microsoft.com/samueld/2016/07/19/adfs-certauth-aad-o365/). 
> 

## <a name="license-requirements-for-using-azure-ad-connect"></a>Лицензионные требования для Azure AD Connect

[!INCLUDE [active-directory-free-license.md](../../../includes/active-directory-free-license.md)]

## <a name="next-steps"></a>Дальнейшие действия 

- [Использование Azure AD Connect с расширением Azure AD Connect Health](whatis-azure-ad-connect.md) 
- [Использование синхронизации хэша паролей](whatis-phs.md) 
- [Использование сквозной проверки подлинности](how-to-connect-pta.md) 
- [Использование федерации](whatis-fed.md) 
- [Использование единого входа](how-to-connect-sso.md) 

