---
title: Настройка приложения для предоставления интерфейсов веб-API с помощью платформы удостоверений Майкрософт
description: Узнайте, как настроить приложение для доступа к новому разрешению или области и роли, чтобы сделать приложение доступным для клиентских приложений.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/08/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: aragra, lenalepa, sureshja
ms.collection: M365-identity-device-management
ms.openlocfilehash: f770539e2f60a960a52a877dd4eaa79c60a1af4a
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482671"
---
# <a name="quickstart-configure-an-application-to-expose-web-apis"></a>Краткое руководство. Настройка приложения для предоставления интерфейсов веб-API

Вы можете разработать веб-API и сделать его доступным для клиентских приложений, предоставив [разрешения или области](developer-glossary.md#scopes) и [роли](developer-glossary.md#roles). Доступ к правильно настроенному веб-API предоставляется по аналогии с другими веб-API корпорации Майкрософт, включая API Graph и API Office 365.

В этом кратком руководстве вы узнаете, как настроить приложение для доступа к новой области, чтобы сделать его доступным для клиентских приложений.

## <a name="prerequisites"></a>Предварительные требования

Чтобы приступить к работе, выполните следующие предварительные требования:

* Ознакомьтесь с общими сведениями о поддерживаемых [разрешениях и согласии](v2-permissions-and-consent.md). Для создания приложений, которые будут использовать другие пользователи или приложения, это важно понимать.
* Установите клиент, содержащий зарегистрированные приложения.
  * Если у вас нет зарегистрированных приложений, [ознакомьтесь со сведениями о регистрации приложений с помощью платформы удостоверений Майкрософт](quickstart-register-app.md).

## <a name="sign-in-to-the-azure-portal-and-select-the-app"></a>Войдите на портал Azure и выберите приложение.

Чтобы настроить приложение, выполните следующие действия.

1. Войдите на [портал Azure](https://portal.azure.com) с помощью личной учетной записи Майкрософт либо рабочей или учебной учетной записи.
1. Если учетная запись предоставляет доступ нескольким клиентам, выберите свою учетную запись в правом верхнем углу и нужный клиент Azure AD для этого сеанса портала.
1. В области навигации слева выберите службу **Azure Active Directory**, а затем выберите **Регистрация приложений**.
1. Найдите и выберите приложение, которое нужно настроить. После выбора приложения появится страница **Обзор** приложения или главная страница регистрации.
1. Выберите, какой метод вы хотите использовать для предоставления новой области (пользовательский интерфейс или манифест приложения):
    * [Предоставление новой области через пользовательский интерфейс](#expose-a-new-scope-through-the-ui)
    * [Предоставление новой области или роли с помощью манифеста приложения](#expose-a-new-scope-or-role-through-the-application-manifest)

## <a name="expose-a-new-scope-through-the-ui"></a>Предоставление новой области через пользовательский интерфейс

[![Показано, как предоставить доступ к API с помощью пользовательского интерфейса](./media/quickstart-update-azure-ad-app-preview/expose-api-through-ui-expanded.png)](./media/quickstart-update-azure-ad-app-preview/expose-api-through-ui-expanded.png#lightbox)

Для предоставления новой области через пользовательский интерфейс:

1. На странице **Обзор** выберите раздел **Предоставление API**.

1. Нажмите **Добавить группу**.

1. Если вы не задали **URI идентификатора приложения**, то увидите запрос на его ввод. Введите URI идентификатора приложения или используйте предоставленный, а затем выберите **Сохранить и продолжить**.

1. Когда откроется страница **Добавить группу**, введите сведения данной области:

    | Поле | ОПИСАНИЕ |
    |-------|-------------|
    | **Имя области** | Введите понятное имя для области.<br><br>Например, `Employees.Read.All`. |
    | **Кто может давать согласие** | Выберите, могут ли пользователи предоставлять согласие этой области или требуется согласие администратора. Выберите **Admins only** (Только администраторы) для более высокого уровня разрешений. |
    | **Отображаемое имя согласия администратора** | Введите понятное описание области, которое увидят администраторы.<br><br>Например `Read-only access to Employee records`. |
    | **Описание согласия администратора** | Введите понятное описание области, которое увидят администраторы.<br><br>Например `Allow the application to have read-only access to all Employee data.`. |

    Если пользователи могут предоставлять согласие для области, также добавьте значения для следующих полей:

    | Поле | ОПИСАНИЕ |
    |-------|-------------|
    | **Отображаемое имя согласия пользователя** | Введите понятное имя для области, которое увидят пользователи.<br><br>Например `Read-only access to your Employee records`. |
    | **Описание согласия пользователя** | Введите понятное описание области, которое увидят пользователи.<br><br>Например `Allow the application to have read-only access to your Employee data.`. |

1. Задайте **Состояние** и выберите **Добавить область** после завершения.

1. Выполните действия, чтобы [проверить доступность веб-API другим приложениям](#verify-the-web-api-is-exposed-to-other-applications).

## <a name="expose-a-new-scope-or-role-through-the-application-manifest"></a>Предоставление новой области или роли с помощью манифеста приложения

[![Предоставление новой области с помощью коллекции oauth2Permissions в манифесте](./media/quickstart-update-azure-ad-app-preview/expose-new-scope-through-app-manifest-expanded.png)](./media/quickstart-update-azure-ad-app-preview/expose-new-scope-through-app-manifest-expanded.png#lightbox)

Для предоставления новой области через манифест приложения:

1. На странице **Обзор** выберите раздел **Манифест**. Откроется веб-редактор манифеста, который позволяет **изменить** манифест на портале. При необходимости можно выбрать **Скачать** и изменить манифест локально, а затем повторно применить его для приложения с помощью команды **Отправить**.
    
    В следующем примере показано предоставление новой области с именем `Employees.Read.All` в ресурсе или API путем добавления следующего элемента JSON в коллекцию `oauth2Permissions`.

      ```json
      {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
      }
      ```

   > [!NOTE]
   > В качестве значения `id` должен выступать новый GUID, созданный программным путем или с помощью инструмента создания [guidgen](https://msdn.microsoft.com/library/ms241442%28v=vs.80%29.aspx). Значение `id` представляет уникальный идентификатор для области в качестве предоставляемого веб-API. Когда клиент соответствующим образом настроен и имеет разрешения на доступ к веб-API, Azure AD выдает маркер доступа OAuth 2.0. Во время вызова веб-API клиент предоставляет маркер доступа с утверждением области (scp), для которого заданы разрешения, запрашиваемые в его регистрации приложения.
   >
   > Позднее вы можете по мере необходимости предоставлять дополнительные области доступа. Помните о том, что веб-API может предоставлять несколько областей, связанных с множеством разных функций. Ресурс может управлять доступом к веб-API в среде выполнения, вычисляя утверждения области (`scp`) в полученном маркере доступа OAuth 2.0.

1. По завершении щелкните **Сохранить**. Теперь веб-интерфейс API настроен для использования другими приложениями в каталоге.
1. Выполните действия, чтобы [проверить доступность веб-API другим приложениям](#verify-the-web-api-is-exposed-to-other-applications).

## <a name="verify-the-web-api-is-exposed-to-other-applications"></a>Проверка доступности веб-API другим приложениям

1. Вернитесь к клиенту Azure AD, выберите **Регистрация приложений**, найдите и выберите приложение клиента, которое необходимо настроить.
1. Повторите шаги, как описано в статье "Настройка клиентского приложения для доступа к веб-API".
1. На шаге **Выбор API** выберите ресурс. Отобразится новая область, доступная для запросов разрешений клиента.

## <a name="more-on-the-application-manifest"></a>Дополнительная информация о манифесте приложения

Манифест приложения является механизмом обновления сущности приложения, который определяет все атрибуты конфигурации удостоверения приложения Azure AD. Дополнительные сведения см. в разделе, посвященном сущности "Приложение" и ее схеме (в [документации по сущности "Приложение API Graph"](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity)). В этой статье вы найдете полную информацию об элементах сущности "Приложение", которые используются при указании разрешений для API, включая:  

* элемент appRoles — коллекция сущностей [AppRole](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type), которые используются при определении [разрешений приложения](developer-glossary.md#permissions) для веб-API;
* элемент oauth2Permissions — коллекция сущностей [OAuth2Permission](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type), которые используются при определении [делегированных разрешений](developer-glossary.md#permissions) для веб-API.

Дополнительные сведения о концепции манифеста приложения см. в статье [Основные сведения о манифесте приложения Azure Active Directory](reference-app-manifest.md).

## <a name="next-steps"></a>Дополнительная информация

Дополнительные сведения о других кратких руководствах по управлению приложениями см. в следующих ресурсах:

* [Краткое руководство: регистрация приложения с помощью платформы удостоверений Майкрософт (предварительная версия)](quickstart-register-app.md)
* [Настройка клиентского приложения для доступа к веб-API](quickstart-configure-app-access-web-apis.md)
* [Краткое руководство. Изменение учетных записей, поддерживаемых приложением (предварительная версия)](quickstart-modify-supported-accounts.md)
* [Краткое руководство. Удаление приложения, зарегистрированного с помощью платформы удостоверений Майкрософт (предварительная версия)](quickstart-remove-app.md)

Дополнительные сведения о двух объектах Azure AD, представляющих зарегистрированное приложение, и о взаимосвязи между ними см. в разделе [Объекты приложения и субъекта-службы в Azure Active Directory (Azure AD)](app-objects-and-service-principals.md).

Дополнительные сведения о фирменной символике, которой следует пользоваться при разработке приложений в среде Azure Active Directory (Azure AD) см. в разделе [Рекомендации по фирменной символике для приложений](howto-add-branding-in-azure-ad-apps.md).
