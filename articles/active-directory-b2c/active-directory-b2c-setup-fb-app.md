---
title: Настройка регистрации и входа с учетной записью Facebook — Azure Active Directory B2C | Документация Майкрософт
description: Вы можете организовать в приложениях регистрацию и вход для клиентов с учетными записями Facebook, используя Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 6b90ba89f17b42f4d92c666c3f539fcad3e3227c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66733528"
---
# <a name="set-up-sign-up-and-sign-in-with-a-facebook-account-using-azure-active-directory-b2c"></a>Настройка регистрации и входа с учетной записью Facebook через Azure Active Directory B2C

## <a name="create-a-facebook-application"></a>Создание приложения Facebook

Чтобы использовать учетную запись Facebook в качестве [поставщика удостоверений](active-directory-b2c-reference-oauth-code.md) для Azure Active Directory (Azure AD) B2C, необходимо создать в клиенте приложение, которое будет представлять этого поставщика. Если у вас нет учетной записи Facebook, вы можете получить ее по адресу [https://www.facebook.com/](https://www.facebook.com/).

1. Выполните вход на [веб-сайт разработчиков Facebook](https://developers.facebook.com/) с учетными данными от учетной записи Facebook.
2. Если вы этого еще не сделали, необходимо зарегистрироваться в качестве разработчика Facebook. Чтобы сделать это, выберите **приступить к работе** в правом верхнем углу страницы, примите политики Facebook и завершите регистрацию.
3. Выберите **Мои приложения** и затем **добавить новое приложение**.
4. Введите **Отображаемое название** и **Эл. адрес для связи**.
5. Нажмите кнопку **Создайте ID приложения**. Для этого может потребоваться принять условия политики платформы Facebook и пройти проверку безопасности в сети.
6. Выберите **Параметры** > **Основные**.
7. Выберите **категорию**, например `Business and Pages`. Это значение требуется Facebook, но не используется для Azure AD B2C.
8. В нижней части страницы, выберите **Добавить платформу**, а затем выберите **Веб-сайт**.
9. В поле **URL-адрес сайта** введите `https://your-tenant-name.b2clogin.com/`, заменив `your-tenant-name` именем вашего клиента. Введите **URL-адрес политики конфиденциальности**, например `http://www.contoso.com`. URL-адрес политики — это управляемая вами страница, где предоставляются сведения о конфиденциальности для вашего приложения.
10. Щелкните **Save changes** (Сохранить изменения).
11. В верхней части страницы скопируйте значение **APP ID** (Идентификатор приложения).
12. Нажмите кнопку **Show** (Показать) и скопируйте значение **App Secret** (Секрет приложения). Оба значения потребуются вам для настройки Facebook в качестве поставщика удостоверений для вашего клиента. **Секрет приложения** — это важные учетные данные безопасности.
13. Щелкните знак «плюс» рядом **ПРОДУКТОВ**, а затем выберите **Настройка** в разделе **имени для входа Facebook**.
14. В разделе **имени для входа Facebook**выберите **параметры**.
15. В поле **Valid OAuth redirect URIs** (Допустимые URI перенаправления OAuth) введите `https://your-tenant-name.b2clogin.com/your-tenant-name.onmicrosoft.com/oauth2/authresp`. Замените `your-tenant-name` именем вашего клиента. Нажмите кнопку **Save Changes** (Сохранить изменения) в нижней части страницы.
16. Чтобы сделать приложение Facebook доступным в Azure AD B2C, щелкните селектор состояния в верхней правой части страницы и установите его в положение **Вкл.** для предоставления общего доступа к приложению, а затем нажмите кнопку **Подтвердить**.  На этом этапе состояние должно измениться с **Development** (Разработка) на **Live** (Активно).

## <a name="configure-a-facebook-account-as-an-identity-provider"></a>Настройка учетной записи Facebook в качестве поставщика удостоверений

1. Войдите на [портал Azure](https://portal.azure.com/) с правами глобального администратора клиента Azure AD B2C.
2. Убедитесь, что используете каталог, содержащий клиент Azure AD B2C, щелкнув **Фильтр каталога и подписки** в верхнем меню и выбрав каталог, содержащий ваш клиент.
3. Выберите **Все службы** в левом верхнем углу окна портала Azure, найдите службу **Azure AD B2C** и выберите ее.
4. Щелкните **Поставщики удостоверений** и выберите **Добавить**.
5. Введите **Имя**. Например, введите *Facebook*.
6. Выберите **Identity provider type** (Тип поставщика удостоверений), выберите **Facebook** и нажмите кнопку **OK**.
7. Выберите действие **Настроить этот поставщик удостоверений** и введите в поле **Идентификатор клиента** сохраненный идентификатор приложения, а в поле **Секрет клиента** — сохраненный секрет приложения от приложения Facebook, которое вы ранее создали.
8. Щелкните **ОК**, а затем **Создать**, чтобы сохранить конфигурацию Facebook.
