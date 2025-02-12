---
title: Отладка единого входа на основе SAML в Azure Active Directory | Документы Майкрософт
description: Отладка единого входа на основе SAML в приложениях в Azure Active Directory.
services: active-directory
author: rwike77
documentationcenter: na
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/18/2019
ms.author: ryanwi
ms.custom: aaddev
ms.reviewer: luleon, hirsin, smalser
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4331acf639af90448b5508e3487f4979e9b82c45
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/01/2019
ms.locfileid: "67482732"
---
# <a name="debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Отладка единого входа на основе SAML в приложениях в Azure Active Directory

Узнайте, как найти и устранить проблемы с [единым входом](../manage-apps/what-is-single-sign-on.md) для приложений в Azure Active Directory (Azure AD) с поддержкой [языка разметки заявлений системы безопасности (SAML) 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language). 

## <a name="before-you-begin"></a>Перед началом работы

Мы рекомендуем установить [расширение защищенного входа в мои приложения](../user-help/my-apps-portal-end-user-troubleshoot.md#im-having-trouble-installing-the-my-apps-secure-sign-in-extension). Это расширение браузера позволяет легко собирать запрос SAML и ответ SAML сведения необходимо устранить все проблемы с единым входом. Если вы не можете установить расширение, ознакомьтесь с этой статьей, которая описывает решение проблем при наличии или отсутствии этого расширения.

Перейдите по одной из следующих ссылок, чтобы загрузить и установить расширение защищенного входа в мои приложения.

- [Chrome](https://go.microsoft.com/fwlink/?linkid=866367)
- [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)
- [Firefox](https://go.microsoft.com/fwlink/?linkid=866366)

## <a name="test-saml-based-single-sign-on"></a>Тестирование единого входа на основе SAML

Для тестирования на основе SAML единого входа между Azure AD и целевое приложение:

1. Войдите на [портал Azure](https://portal.azure.com) как глобальный администратор или другой администратор с правами на управление приложениями.
1. В колонке слева, выберите **Azure Active Directory**, а затем выберите **корпоративные приложения**. 
1. В списке корпоративных приложений, выберите приложение, для которого требуется проверка единого входа, а затем в меню слева выберите **единого входа**.
1. Чтобы открыть в на основе SAML единого входа в интерфейс тестирования, перейдите к статье **проверка единого входа** (шаг 5). Если **теста** кнопка неактивна, необходимо сначала заполните и сохранить необходимые атрибуты **базовой конфигурации SAML** раздел.
1. В колонке **Тестирование единого входа** введите корпоративные учетные данные, чтобы войти в целевое приложение. Вы можете выполнить вход как текущий пользователь или как другой пользователь. Если вы войдете от имени другого пользователя, откроется окно проверки подлинности.

    ![Снимок экрана, показывающий страницу единого входа SAML теста](./media/howto-v1-debug-saml-sso-issues/test-single-sign-on.png)

Если вы успешно выполнили вход, тест пройден. В этом случае Azure AD выдает маркер ответа SAML приложению. Приложение использует маркер SAML, чтобы обработать ваш вход.

Если на странице входа компании или на странице приложения отображается ошибка, воспользуйтесь инструкциями из следующих разделов, чтобы решить ее.

## <a name="resolve-a-sign-in-error-on-your-company-sign-in-page"></a>Решение ошибки входа на странице входа компании

При попытке войти на страницу входа в систему компании, аналогичный приведенному ниже может появиться ошибка.

![Пример, демонстрирующий ошибку на странице входа в систему компании](./media/howto-v1-debug-saml-sso-issues/error.png)

Чтобы устранить эту ошибку, вам понадобится сообщение об ошибке и запрос SAML. Расширение защищенного входа в мои приложения автоматически собирает эту информацию и отображает инструкции по решению проблемы в Azure AD.

### <a name="to-resolve-the-sign-in-error-with-the-my-apps-secure-sign-in-extension-installed"></a>Для устранения этой ошибки входа в систему с My защищенного приложения вход расширением установлен

1. При возникновении ошибки, расширение вы будете перенаправлены в Azure AD **проверка единого входа** колонке.
1. На **проверка единого входа** колонке **загрузить запрос SAML**.
1. Отобразятся инструкции по устранению в зависимости от ошибки и значений в запросе SAML.
1. Вы увидите **решение** кнопку, чтобы автоматически обновить конфигурацию в Azure AD, чтобы устранить проблему. Если вы не видите эту кнопку, затем войти проблема не вызвана неправильной настройкой в Azure AD.

Если разрешение не предоставляется для ошибки входа, мы рекомендуем использовать сообщать нам отзывы текстового поля.

### <a name="to-resolve-the-error-without-installing-the-my-apps-secure-sign-in-extension"></a>Чтобы устранить эту ошибку без установки Мои расширение защищенного приложения входа в систему

1. Скопируйте сообщение об ошибке в нижнем правом углу страницы. В сообщении об ошибке указывается:
    - CorrelationID и Timestamp. Эти значения важно указать при обращении в службу поддержки корпорации Майкрософт. Это поможет нашим инженерам быстрее определить проблему и найти ей решение.
    - Инструкция, определяющая первопричину проблемы.
1. Вернитесь в Azure AD и найдите колонку **Тестирование единого входа**.
1. В текстовое поле над пунктом **Получить рекомендации по устранению** вставьте сообщение об ошибке.
1. Нажмите **Получить рекомендации по устранению**, чтобы просмотреть инструкции по устранению ошибки. Для рекомендаций может потребоваться информация из запроса SAML или ответа SAML. Если вы не используете Мои расширение защищенного приложения входа в систему, может потребоваться средство например [Fiddler](https://www.telerik.com/fiddler) для извлечения SAML-запросе и ответе.
1. Убедитесь, что назначения в запросе SAML соответствует SAML Sign-On Service URL-адрес единого полученного из Azure AD.
1. Убедитесь, что издателем в запросе SAML является тем же идентификатором, который определен для приложения в Azure AD. В Azure AD значение издателя используется для поиска приложения в каталоге.
1. Убедитесь, что AssertionConsumerServiceURL где приложение ожидает токен SAML из Azure AD. Это значение можно настроить в Azure AD, но он не является обязательным, если он является частью запроса SAML.


## <a name="resolve-a-sign-in-error-on-the-application-page"></a>Устранение ошибки входа на странице приложения

Ошибка на странице приложения может отображаться даже после успешного входа. Это происходит в случае, когда Azure AD выдает приложению маркер, но приложение не принимает ответ.

Чтобы устранить ошибку, выполните следующие действия.

1. Если приложение находится в коллекции Azure AD, убедитесь, что вы выполнили все действия для интеграции приложения с Azure AD. Инструкции по интеграции для приложения вы найдете в [руководствах по интеграции приложений SaaS](../saas-apps/tutorial-list.md).
1. Получите ответ SAML.
    - Если расширение защищенного входа в мои приложения установлено, в колонке **Тестирование единого входа** нажмите **Загрузить ответ SAML**.
    - Если расширение не установлено, получите ответ SAML с помощью дополнительного средства, например [Fiddler](https://www.telerik.com/fiddler).
1. Обратите внимание на следующие элементы в маркере ответа SAML:
   - Уникальный идентификатор пользователя значения и формата NameID
   - Утверждения, выданные в маркере
   - Сертификат, использованный для подписи маркера.

     Дополнительные сведения об ответе SAM см. в статье [Протокол единого входа SAML](single-sign-on-saml-protocol.md).

1. Теперь, когда вы просмотрели ответ SAML, см. в разделе [ошибка на странице приложения после входа в систему](../manage-apps/application-sign-in-problem-application-error.md) рекомендации о том, как устранить проблему. 
1. Если вы по-прежнему не можете успешно войти, можно попросить поставщику приложения, что здесь отсутствует в ответе SAML.

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда приложение работает единый вход, вы могли бы [Автоматическая подготовка пользователей и отмену подготовки для приложений SaaS](../manage-apps/user-provisioning.md) или [приступить к работе с условным доступом](../conditional-access/app-based-conditional-access.md).
