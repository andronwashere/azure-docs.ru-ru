---
title: Распространенные сценарии в управлении назначениями Azure AD (Предварительная версия) — Azure Active Directory
description: Ознакомьтесь с общими действиями, которые следует выполнить для распространенных сценариев в Azure Active Directory управления назначением (Предварительная версия).
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.subservice: compliance
ms.date: 04/23/2019
ms.author: ajburnle
ms.reviewer: mwahl
ms.collection: M365-identity-device-management
ms.openlocfilehash: f8e7709abcc97baac9bf657b9fff2110cb2e72c1
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68489038"
---
# <a name="common-scenarios-in-azure-ad-entitlement-management-preview"></a>Распространенные сценарии в управлении назначениями Azure AD (Предварительная версия)

> [!IMPORTANT]
> Управление правами Azure Active Directory (Azure AD) сейчас предоставляется в общедоступной предварительной версии.
> Эта предварительная версия предоставляется без соглашения об уровне обслуживания и не рекомендована для использования рабочей среде. Некоторые функции могут не поддерживаться или их возможности могут быть ограничены.
> Дополнительные сведения см. в статье [Дополнительные условия использования предварительных выпусков Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Существует несколько способов настройки управления назначениями для Организации. Однако если вы только начинаете работу, полезно понимать распространенные сценарии для администраторов, утверждающих и запрашивающих.

## <a name="administrators"></a>Администраторы

### <a name="im-new-to-entitlement-management-and-i-want-help-with-getting-started"></a>Я не являюсь новым руководством по управлению назначением и хочу получить помощь по началу работы

> [!div class="mx-tableFixed"]
> | Шаги | Пример |
> | --- | --- |
> | [Следуйте указаниям в руководстве по созданию пакета первого доступа](entitlement-management-access-package-first.md) | [![Значок портал Azure](./media/entitlement-management-scenarios/azure-portal.png)](./media/entitlement-management-scenarios/azure-portal-expanded.png#lightbox) |

### <a name="i-want-to-allow-users-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Я хочу разрешить пользователям в моем каталоге запрашивать доступ к группам, приложениям или сайтам SharePoint

> [!div class="mx-tableFixed"]
> | Шаги | Пример |
> | --- | --- |
> | **1.** [Создание нового пакета Access в каталоге](entitlement-management-access-package-create.md#start-new-access-package) | ![Создание пакета для доступа](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Добавление ролей ресурсов в пакет Access](entitlement-management-access-package-edit.md#add-resource-roles)<ul><li>Группы</li><li>Приложения</li><li>Сайты SharePoint</li></ul> | ![Добавление ролей ресурсов](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Добавление политики](entitlement-management-access-package-edit.md#policy-for-users-in-your-directory)<ul><li>Для пользователей в вашем каталоге</li><li>Требуется утверждение</li><li>Параметры срока действия</li></ul> | ![Добавить политику](./media/entitlement-management-scenarios/policy.png) |

### <a name="i-want-to-allow-users-from-my-business-partners-directory-including-users-not-yet-in-my-directory-to-request-access-to-groups-applications-or-sharepoint-sites"></a>Я хочу разрешить пользователям из каталога бизнес-партнеров (включая пользователей, которых еще нет в моем каталоге) запрашивать доступ к группам, приложениям или сайтам SharePoint

> [!div class="mx-tableFixed"]
> | Шаги | Пример |
> | --- | --- |
> | **1.** [Создание нового пакета Access в каталоге](entitlement-management-access-package-create.md#start-new-access-package) | ![Создание пакета для доступа](./media/entitlement-management-scenarios/access-package.png) |
> | **2.** [Добавление ролей ресурсов в пакет Access](entitlement-management-access-package-edit.md#add-resource-roles) | ![Добавление ролей ресурсов](./media/entitlement-management-scenarios/resource-roles.png) |
> | **3.** [Добавление политики для внешних пользователей](entitlement-management-access-package-edit.md#policy-for-users-not-in-your-directory)<ul><li>Для пользователей, не наличныхся в вашем каталоге</li><li>Требуется утверждение</li><li>Параметры срока действия</li></ul> | ![Добавление политики для внешних пользователей](./media/entitlement-management-scenarios/policy-external.png) |
> | **4.** [Отправьте ссылку на портал доступа, чтобы запросить пакет Access для вашего бизнес-партнера](entitlement-management-access-package-edit.md#copy-my-access-portal-link)<ul><li>Бизнес-партнер может поделиться связью с пользователями</li></ul> |  |

### <a name="i-want-to-change-the-groups-applications-or-sharepoint-sites-in-an-access-package"></a>Я хочу изменить группы, приложения или сайты SharePoint в пакете Access

> [!div class="mx-tableFixed"]
> | Шаги | Пример |
> | --- | --- |
> | **1.** Открытие пакета Access | ![Добавление ролей ресурсов](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Добавление или удаление ролей ресурсов](entitlement-management-access-package-edit.md#add-resource-roles) | ![Добавление ролей ресурсов](./media/entitlement-management-scenarios/resource-roles-add.png) |

### <a name="i-want-to-view-who-has-an-assignment-to-groups-applications-or-sharepoint-sites"></a>Я хочу просмотреть, кому назначены группы, приложения или сайты SharePoint

> [!div class="mx-tableFixed"]
> | Шаги | Пример |
> | --- | --- |
> | **1.** Открытие пакета Access | ![Добавление ролей ресурсов](./media/entitlement-management-scenarios/resource-roles.png) |
> | **2.** [Просмотр назначений](entitlement-management-access-package-edit.md#view-who-has-an-assignment)<ul><li>Просмотр пользователей, имеющих доступ к пакету Access</li><li>Просмотр истечения срока действия доступа пользователя</li></ul> |  |

### <a name="i-want-to-view-groups-applications-or-sharepoint-sites-a-user-has-access-to"></a>Я хочу просмотреть группы, приложения или сайты SharePoint, к которым у пользователя есть доступ

> [!div class="mx-tableFixed"]
> | Шаги | Пример |
> | --- | --- |
> | [Просмотр отчета о назначениях пользователей](entitlement-management-reports.md)<ul><li>Просмотр, когда они запрошены и кто утвердил</li></ul> |  |

## <a name="approvers"></a>Утверждающие

### <a name="i-want-to-approve-requests-to-access-groups-applications-or-sharepoint-sites"></a>Я хочу утверждать запросы на доступ к группам, приложениям или сайтам SharePoint

> [!div class="mx-tableFixed"]
> | Шаги | Пример |
> | --- | --- |
> | **1.** [Открыть запрос на портале доступа](entitlement-management-request-approve.md#open-request) | [![Значок портала доступа](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** [Утвердить запрос на доступ](entitlement-management-request-approve.md#approve-or-deny-request) | ![Утвердить доступ](./media/entitlement-management-scenarios/approve-access.png) |

## <a name="requestors"></a>Запрашивающие стороны

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-available-to-me-and-request-access"></a>Я хочу просмотреть доступные мне группы, приложения или сайты SharePoint и запросить доступ

> [!div class="mx-tableFixed"]
> | Шаги | Пример |
> | --- | --- |
> | **1.** [Войдите на портал доступа](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Значок портала доступа](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Найти пакет Access |  |
> | **3.** [Запрос доступа](entitlement-management-request-access.md#request-an-access-package) | ![Запросить доступ](./media/entitlement-management-scenarios/request-access.png) |

### <a name="im-an-external-user-and-i-want-to-request-access-to-groups-applications-or-sharepoint-sites-with-a-direct-link"></a>Я являюсь внешним пользователем и хочу запросить доступ к группам, приложениям или сайтам SharePoint с помощью прямой ссылки

> [!div class="mx-tableFixed"]
> | Шаги | Пример |
> | --- | --- |
> | **1.** [Найдите полученную ссылку на портале доступа](entitlement-management-access-package-edit.md#copy-my-access-portal-link) |  |
> | **2.** [Войдите на портал доступа](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Значок портала доступа](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **3.** [Запрос доступа](entitlement-management-request-access.md#request-an-access-package) | ![Запрос доступа внешних пользователей](./media/entitlement-management-scenarios/request-access-external.png) |

### <a name="i-want-to-view-the-groups-applications-or-sharepoint-sites-i-already-have-access-to"></a>Я хочу просмотреть группы, приложения или сайты SharePoint, к которым у меня уже есть доступ

> [!div class="mx-tableFixed"]
> | Шаги | Пример |
> | --- | --- |
> | **1.** [Войдите на портал доступа](entitlement-management-request-access.md#sign-in-to-the-my-access-portal) | [![Значок портала доступа](./media/entitlement-management-scenarios/my-access-portal.png)](./media/entitlement-management-scenarios/my-access-portal-expanded.png#lightbox) |
> | **2.** Просмотр пакетов активного доступа |  |

## <a name="next-steps"></a>Следующие шаги

- [Учебник. Создание первого пакета Access](entitlement-management-access-package-first.md)
- [Делегирование задач](entitlement-management-delegate.md)
