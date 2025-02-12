---
title: Одностраничный вход с использованием неявного потока — Azure Active Directory B2C
description: Узнайте, как добавить одностраничный вход с помощью неявного потока OAuth 2,0 с Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/19/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 1196f3b186abcd914c409db06b52654f82f4158b
ms.sourcegitcommit: b49431b29a53efaa5b82f9be0f8a714f668c38ab
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68377317"
---
# <a name="single-page-sign-in-using-the-oauth-20-implicit-flow-in-azure-active-directory-b2c"></a>Одностраничный вход с использованием неявного потока OAuth 2,0 в Azure Active Directory B2C

Многие современные приложения имеют внешний интерфейс одностраничных приложений, написанный в основном на JavaScript. Часто приложение записывается с помощью такой платформы, как реагирующий, угловой или Vue. js. При аутентификации одностраничные и другие приложения JavaScript, которые запускаются в основном в браузере, сталкиваются с некоторыми дополнительными проблемами:

- Характеристики безопасности этих приложений отличаются от традиционных серверных веб-приложений.
- Многие серверы авторизации и поставщики удостоверений не поддерживают запросы на общий доступ к ресурсам независимо от источника (CORS).
- Переход от приложения к полноэкранному браузеру может полагаться на работу пользователя.

Для поддержки этих приложений в Azure Active Directory B2C (Azure AD B2C) используется неявный поток OAuth 2.0. Описание неявного потока предоставления авторизации OAuth 2.0 см. в [разделе 4.2 спецификации OAuth 2.0](https://tools.ietf.org/html/rfc6749). В неявном потоке приложение получает маркеры из конечной точки авторизации Azure Active Directory (Azure AD) напрямую, без какого-либо обмена данными между серверами. Вся логика аутентификации и обработка сеансов полностью выполняются в клиенте JavaScript с помощью перенаправления страницы или всплывающего окна.

В Azure AD B2C стандартный неявный поток OAuth 2.0 выходит за рамки простой аутентификации и авторизации. В Azure AD B2C вводится [параметр политики](active-directory-b2c-reference-policies.md). Он позволяет использовать OAuth 2.0 для добавления в приложение политик (потоков пользователей), таких как регистрация, вход и управление профилями. В примере HTTP-запросов в этой статье **fabrikamb2c.onmicrosoft.com** используется в качестве примера. Вы можете заменить `fabrikamb2c` именем своего клиента, если он есть, и создать поток пользователя.

Неявный поток входа выглядит примерно так, как показано на приведенном ниже рисунке. Каждый шаг описан более подробно далее в этой статье.

![Диаграмма в стиле дорожки, показывающая неявный поток подключения OpenID Connect](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Отправка запросов проверки подлинности

Если веб-приложению требуется проверить подлинность пользователя и запустить поток пользователя, он может направить пользователя в `/authorize` конечную точку. Пользователь принимает действие в зависимости от потока пользователя.

В этом запросе клиент указывает разрешения, которые необходимо получить от пользователя в `scope` параметре, и поток пользователя для выполнения `p` в параметре. В следующих разделах приведены три примера для разных потоков пользователей (переносы строк добавлены для удобства чтения). Чтобы понять, как работает каждый запрос, попробуйте вставить его в браузер и запустить. Вы можете заменить `fabrikamb2c` именем своего клиента, если он есть, и создать поток пользователя.

### <a name="use-a-sign-in-user-flow"></a>Использование потока пользователя для входа

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

### <a name="use-a-sign-up-user-flow"></a>Использование потока пользователя для регистрации
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

### <a name="use-an-edit-profile-user-flow"></a>Использование потока пользователя для изменения профиля
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Параметр | Обязательное значение | Описание |
| --------- | -------- | ----------- |
| client_id | Да | Идентификатор приложения, назначенный приложению [портал Azure](https://portal.azure.com/) . |
| response_type | Да | Должен включать `id_token` для входа в OpenID Connect. Он также может содержать значение `token` типа ответа. Использование `token` позволяет приложению получать маркер доступа непосредственно от конечной точки авторизации, не отправляя новый запрос на вход в эту конечную точку.  При использовании типа ответа `token` параметр `scope` должен содержать область, определяющую ресурсы, для которых необходимо выдавать маркер. |
| redirect_uri | Нет | Универсальный код ресурса (URI) перенаправления приложения, на который можно отправлять ответы аутентификации для их получения приложением. Он должен в точности соответствовать одному из URI перенаправления, зарегистрированных на портале, но иметь форму закодированного URL-адреса. |
| response_mode | Нет | Указывает метод, с помощью которого результирующий маркер будет отправлен приложению.  Для неявных потоков используйте `fragment`. |
| область | Да | Список областей с разделителями-пробелами. При указании одной области также обозначаются запрашиваемые разрешения. Область `openid` определяет разрешение на вход пользователя и получение данных о пользователе в форме маркеров идентификации. Область `offline_access` для веб-приложений является необязательной. Она означает, что вашему приложению потребуется маркер обновления для продолжительного доступа к ресурсам. |
| state | Нет | Значение, включенное в запрос, которое также возвращается в ответе маркера. Это может быть строка любого содержимого. Как правило, с целью предотвращения подделки межсайтовых запросов используется генерируемое случайным образом уникальное значение. Этот параметр также включает сведения о состоянии пользователя в приложении перед созданием запроса на аутентификацию (например, об открытой в тот момент странице). |
| nonce | Да | Значение, включенное в запрос (созданное приложением), которое входит в состав полученного маркера идентификации в качестве утверждения. Приложение может проверить это значение во избежание атак с использованием воспроизведения токена. Это значение обычно представляет собой случайную уникальную строку, которую можно использовать для определения источника запроса. |
| p | Да | Политика, которую необходимо выполнить. Это имя политики (потока пользователя), созданной в клиенте Azure AD B2C. Оно должно начинаться с **b2c\_1\_** . |
| prompt | Нет | Требуемый тип взаимодействия с пользователем. В настоящее время поддерживается только значение `login`. Этот параметр заставляет пользователя вводить учетные данные для этого запроса. Единый вход не вступит в силу. |

На этом этапе пользователю предлагается выполнить рабочий процесс политики. Пользователю может потребоваться ввести имя пользователя и пароль, войти с помощью социальных удостоверений, зарегистрироваться в каталоге или любое другое количество действий. Действия пользователя зависят от того, как определен его поток.

Когда пользователь завершит свой поток, служба Azure AD отправит в приложение ответ, используя указанное для `redirect_uri` значение. Она использует метод, указанный в параметре `response_mode`. Какие бы действия пользователь ни выполнял и каким бы ни был его поток, ответ всегда будет одинаковым.

### <a name="successful-response"></a>Успешный ответ
Успешный ответ с использованием `response_mode=fragment` и `response_type=id_token+token` выглядит следующим образом (разрывы строк — для удобства чтения):

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
```

| Параметр | Описание |
| --------- | ----------- |
| access_token | Маркер доступа, запрошенный приложением. |
| token_type | Значение типа маркера. Единственный тип, поддерживаемый Azure AD — носитель. |
| expires_in | Срок действия маркера доступа (в секундах). |
| область | Области, для которых действителен маркер. Области также можно использовать, чтобы кэшировать маркеры для последующего использования. |
| id_token | Маркер идентификации, запрошенный приложением. Вы можете использовать маркер идентификации для проверки личности пользователя и запуска сеанса пользователя. Дополнительные сведения о маркерах идентификации и их содержимом см. в [справочнике по маркерам Azure AD B2C](active-directory-b2c-reference-tokens.md). |
| state | Если запрос содержит параметр `state`, то в ответе должно отображаться то же значение. Приложение должно проверить, совпадают ли значения параметра `state` в запросе и ответе. |

### <a name="error-response"></a>Сообщение об ошибке
Сообщения об ошибках также можно отправлять на универсальный код ресурса (URI) перенаправления, чтобы приложение могло их правильно обработать:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Параметр | Описание |
| --------- | ----------- |
| error | Код, используемый для классификации типов произошедших ошибок. |
| error_description | Конкретное сообщение об ошибке, с помощью которого можно определить первопричину возникновения ошибки аутентификации. |
| state | Если запрос содержит параметр `state`, то в ответе должно отображаться то же значение. Приложение должно проверить, совпадают ли значения параметра `state` в запросе и ответе.|

## <a name="validate-the-id-token"></a>Проверка маркера идентификации

Получение маркера идентификации не является достаточным для прохождения пользователем аутентификации. Проверьте подпись маркера идентификации и проверьте утверждения в маркере на соответствие требованиям приложения. В Azure AD B2C для подписи маркеров и проверки их правильности используются [веб-маркеры JSON Web Token (JWT)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) и шифрование с открытым ключом.

Для проверки маркеров JWT доступны многие библиотеки с открытым исходным кодом в зависимости от выбранного языка. Рекомендуется изучить доступные библиотеки с открытым исходным кодом, а не реализовывать логику проверки самостоятельно. Сведения, приведенные в данной статье, помогут вам узнать, как правильно использовать эти библиотеки.

Служба Azure AD B2C имеет конечную точку метаданных OpenID Connect. Приложение может использовать конечную точку для получения сведений об Azure AD B2C в среде выполнения. Эти сведения включают конечные точки, содержимое маркеров и ключи подписи маркеров. Для каждого потока пользователя в клиенте Azure AD B2C есть документ метаданных JSON. Например, документ метаданных для потока пользователя b2c_1_sign_in в клиенте fabrikamb2c.onmicrosoft.com расположен здесь:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Одно из свойств этого документа конфигурации — `jwks_uri`. Значение для того же потока пользователя будет таким:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`

Определить, какой поток пользователя был задействован для подписания маркера идентификации (и откуда получать метаданные), можно двумя способами. Во-первых, имя потока пользователя включено в утверждение `acr` в `id_token`. Сведения об анализе утверждений маркера идентификации см. в [справочнике по маркерам Azure AD B2C](active-directory-b2c-reference-tokens.md). Другой способ — закодировать поток пользователя в значении параметра `state` при отправке запроса, а затем декодировать параметр `state` и определить, какой поток был использован. Каждый из этих методов является допустимым.

После получения документа метаданных из конечной точки метаданных OpenID Connect можно использовать открытые ключи RSA-256, расположенные в этой конечной точке, для проверки подписи маркера идентификации. В каждый момент времени в этой конечной точке может находиться множество ключей, каждый из которых определяется `kid`. Заголовок `id_token` также содержит утверждение `kid`. Оно указывает на ключ, который был использован для подписи маркера идентификации. Дополнительные сведения см. в [справочнике по маркерам Azure AD B2C](active-directory-b2c-reference-tokens.md), включая раздел [Проверка маркеров](active-directory-b2c-reference-tokens.md).
<!--TODO: Improve the information on this-->

После проверки подписи маркера идентификации для некоторых утверждений требуется проверка. Пример:

* Проверьте утверждение `nonce` во избежание атак с использованием воспроизведения маркеров. Его значение должно совпадать с указанным вами в запросе на вход.
* Проверьте утверждение `aud`, чтобы убедиться, что для вашего приложения был выдан маркер идентификации. Его значением должен быть идентификатор приложения вашего приложения.
* Проверьте утверждения `iat` и `exp`, чтобы убедиться, что срок действия маркера идентификации не истек.

Вам нужно выполнить дополнительные проверки, которые подробно описаны в [спецификации OpenID Connect Core](https://openid.net/specs/openid-connect-core-1_0.html). Вам также может потребоваться проверить дополнительные утверждения в зависимости от сценария. Ниже приведены некоторые из стандартных проверок:

* Обеспечение регистрации пользователя или организации в приложении.
* Предоставление пользователю необходимого уровня авторизации и привилегий.
* Обеспечение определенного уровня проверки подлинности, например с помощью Многофакторной идентификации Azure.

Дополнительные сведения об утверждениях в маркере идентификации см. в [справочнике по маркерам Azure AD B2C](active-directory-b2c-reference-tokens.md).

После проверки маркера идентификации можно запустить сеанс пользователя. В приложении используйте утверждения в маркере идентификации для получения сведений о пользователе. Эти сведения можно использовать для отображения, записей, авторизации и т. д.

## <a name="get-access-tokens"></a>Получение маркеров доступа
Если ваши веб-приложения будут только выполнять потоки пользователей, следующие несколько разделов можно пропустить. Сведения в следующих разделах применимы только к веб-приложениям, которые должны выполнять проверку подлинности вызовов к веб-API и которые защищены Azure AD B2C.

Теперь, когда пользователь подписан в одностраничном приложении, вы можете получить маркеры доступа для вызова веб-API, защищенных с помощью Azure AD. Даже если вы уже получили маркер с помощью типа ответа `token`, то этот метод можно использовать для получения маркеров для дополнительных ресурсов. При этом пользователям не нужно повторно выполнять вход.

В типичном потоке веб-приложения необходимо выполнить запрос к `/token` конечной точке. Однако конечная точка не поддерживает запросы CORS, поэтому вызовы AJAX для получения маркера обновления не являются вариантами. Вместо этого для получения новых маркеров для других веб-API можно использовать неявный поток данных в скрытом HTML-элементе iframe. Ниже приведен пример (разрывы строк — для удобства чтения):

```
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
&response_mode=fragment
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&prompt=none
&p=b2c_1_sign_in
```

| Параметр | Обязательный? | Описание |
| --- | --- | --- |
| client_id |Обязательное значение |Идентификатор приложения, назначенный вашему приложению на [портале Azure](https://portal.azure.com). |
| response_type |Обязательное значение |Должен включать `id_token` для входа в OpenID Connect.  Этот параметр также может содержать `token` типа ответа. Если использовать `token`, то приложение сможет получить маркер доступа непосредственно от конечной точки авторизации, не отправляя новый запрос на вход в эту конечную точку. При использовании типа ответа `token` параметр `scope` должен содержать область, определяющую ресурсы, для которых необходимо выдавать маркер. |
| redirect_uri |Рекомендуется |Универсальный код ресурса (URI) перенаправления приложения, на который можно отправлять ответы аутентификации для их получения приложением. Он должен в точности соответствовать одному из универсальных кодов ресурса (URI) перенаправления, зарегистрированных на портале, но иметь форму URL-адреса. |
| область |Обязательное значение |Список областей с разделителями-пробелами.  Для получения маркеров укажите все области, которые необходимы для целевого ресурса. |
| response_mode |Рекомендуется |Указывает метод, с помощью которого результирующий маркер будет отправлен приложению.  Возможные значения: `query`, `form_post` или `fragment`. |
| state |Рекомендуется |Значение, включенное в запрос, которое также возвращается в ответе маркера.  Это может быть строка любого содержимого.  Как правило, с целью предотвращения подделки межсайтовых запросов используется генерируемое случайным образом уникальное значение.  Параметр state используется также для кодирования сведений о состоянии пользователя в приложении перед созданием запроса на аутентификацию. Например, сведений об открытой на тот момент странице или представлении. |
| nonce |Обязательное значение |Значение, включенное в запрос и созданное приложением, которое входит в состав полученного маркера идентификации в качестве утверждения.  Приложение может проверить это значение во избежание атак с использованием воспроизведения токена. Обычно это значение представляет собой случайную уникальную строку, которая определяет источник запроса. |
| prompt |Обязательное значение |Для обновления и получения маркеров в скрытом запросе iframe используйте `prompt=none`. Таким образом, iframe не перестает отвечать на странице входа приложения и сразу же возвращает данные. |
| login_hint |Обязательное значение |Для обновления и получения маркеров в скрытом запросе iframe включите в эту подсказку имя пользователя. Это позволяет различать сеансы пользователя в определенный момент времени. Вы можете извлечь имя пользователя из более раннего входа, используя `preferred_username` утверждение `profile` (область требуется для получения `preferred_username` утверждения). |
| domain_hint |Обязательное значение |Может иметь значение `consumers` или `organizations`.  Для обновления и получения маркеров в скрытом IFRAME включите `domain_hint` значение в запрос.  Извлеките `profile` `tid` утверждение из маркера идентификатора более раннего входа, чтобы определить, какое значение использовать (область требуется для получения утверждения). `tid` Если значение утверждения `tid` — `9188040d-6c67-4c5b-b112-36a304b66dad`, то используйте `domain_hint=consumers`.  Или используйте `domain_hint=organizations`. |

Если задать параметр `prompt=none`, то этот запрос успешно выполнится или немедленно завершится с ошибкой, и вы вернетесь к своему приложению.  Успешный ответ будет отправлен в ваше приложение на указанный URI перенаправления с помощью метода, заданного в параметре `response_mode`.

### <a name="successful-response"></a>Успешный ответ
Успешный ответ с использованием `response_mode=fragment` выглядит, как в следующем примере:

```
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| Параметр | Описание |
| --- | --- |
| access_token |Маркер, запрошенный приложением. |
| token_type |Маркер всегда будет иметь тип Bearer (Носитель). |
| state |Если запрос содержит параметр `state`, то в ответе должно отображаться то же значение. Приложение должно проверить, совпадают ли значения параметра `state` в запросе и ответе. |
| expires_in |Срок действия маркера доступа (в секундах). |
| область |Области, для которых действителен маркер доступа. |

### <a name="error-response"></a>Сообщение об ошибке
Сообщения об ошибках также можно отправлять на универсальный код ресурса (URI) перенаправления, чтобы приложение могло их правильно обработать.  Для `prompt=none`ожидаемая ошибка выглядит как в следующем примере:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Параметр | Описание |
| --- | --- |
| error |Строка кода ошибки, которую можно использовать для классификации типов возникающих ошибок. Эту строку также можно использовать для реагирования на ошибки. |
| error_description |Конкретное сообщение об ошибке, с помощью которого можно определить первопричину возникновения ошибки аутентификации. |

После появления этой ошибки в запросе iframe пользователю необходимо войти еще раз в интерактивном режиме для получения нового маркера.

## <a name="refresh-tokens"></a>Маркеры обновления
Срок действия маркеров идентификации и доступа достаточно короткий. Поэтому приложение должно быть готово периодически обновлять их.  Чтобы обновить маркер любого типа, выполните тот же скрытый запрос iframe, который использовался в предыдущем примере, используя параметр `prompt=none` для управления действиями Azure AD.  Чтобы получить новое значение `id_token`, обязательно используйте `response_type=id_token` и `scope=openid`, а также параметр `nonce`.

## <a name="send-a-sign-out-request"></a>Отправка запроса на выход
Для выхода пользователя из приложения перенаправьте его в Azure AD для выполнения выхода. Если вы не перенаправите пользователя, он сможет выполнить повторную проверку подлинности в приложении без повторного ввода учетных данных, так как у них есть допустимый сеанс единого входа в Azure AD.

Можно просто перенаправить пользователя на точку `end_session_endpoint`, которая указана в том же документе метаданных OpenID Connect, описанном выше в разделе [Проверка маркера идентификации](#validate-the-id-token). Пример:

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Параметр | Обязательный? | Описание |
| --- | --- | --- |
| p |Обязательное значение |Политика, которую необходимо использовать для выхода пользователя из приложения. |
| post_logout_redirect_uri |Рекомендуется |URL-адрес, на который пользователя следует перенаправить после успешного выхода. Если он не указан, то Azure AD B2C покажет пользователю универсальное сообщение. |

> [!NOTE]
> При перенаправлении пользователя на `end_session_endpoint` очищается часть его данных состояния единого входа через Azure AD B2C. Хотя фактически пользователь не выходит из сеанса поставщика удостоверений социальных сетей. Если пользователь выбирает одного и того же поставщика удостоверений во время последующего входа, он повторно проходит проверку подлинности без ввода учетных данных. Например, если пользователь хочет выйти из приложения Azure AD B2C, то это совершенно не означает, что он также хочет полностью выйти из учетной записи Facebook. Тем не менее для локальных учетных записей необходимо правильно завершить сеанс пользователя.
>

## <a name="next-steps"></a>Следующие шаги

### <a name="code-sample-hellojs-with-azure-ad-b2c"></a>Пример кода: Hello. js с Azure AD B2C

[Одностраничное приложение, созданное на основе Hello. js с Azure AD B2C][github-hello-js-example] GitHub

Этот пример на сайте GitHub поможет вам начать работу с Azure AD B2C в простом веб-приложении, созданном на основе [Hello. js][github-hello-js] , и использовать проверку подлинности во всплывающем стиле.

<!-- Links - EXTERNAL -->
[github-hello-js-example]: https://github.com/azure-ad-b2c/apps/tree/master/spa/javascript-hellojs-singlepageapp-popup
[github-hello-js]: https://github.com/MrSwitch/hello.js
