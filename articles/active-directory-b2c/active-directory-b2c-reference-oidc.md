---
title: Вход в веб-приложения с помощью OpenID Connect в Azure Active Directory B2C | Документация Майкрософт
description: Создание веб-приложений с помощью протокола проверки подлинности OpenID Connect Connect в Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: marsma
ms.subservice: B2C
ms.custom: fasttrack-edit
ms.openlocfilehash: c5626e2ddfc24eeaeed562f3eaf73d16626eb458
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68278041"
---
# <a name="web-sign-in-with-openid-connect-in-azure-active-directory-b2c"></a>Вход в веб-приложения с помощью OpenID Connect в Azure Active Directory B2C

OpenID Connect — это протокол проверки подлинности на основе OAuth 2.0, который может использоваться для безопасного входа пользователей в веб-приложения. Используя реализацию OpenID Connect в Azure Active Directory B2C (Azure AD B2C), можно передать Azure Active Directory (Azure AD) регистрацию, вход в систему и другие действия по управлению пользователями в веб-приложениях. В этом руководстве показано, как это сделать (независимо от языка программирования). и описывает, как отправлять и получать сообщения HTTP, не используя ни одну из наших библиотек с открытым исходным кодом.

[OpenID Connect](https://openid.net/specs/openid-connect-core-1_0.html) расширяет возможности протокола *авторизации* OAuth 2.0 и позволяет использовать его в качестве протокола *проверки подлинности*, Этот протокол проверки подлинности позволяет выполнять единый вход. В нем представлено понятие *маркера идентификации*, которое позволяет клиенту проверить подлинность пользователя и получить основные сведения о профиле пользователя.

Так как он расширяет OAuth 2,0, он также позволяет приложениям безопасно получать *маркеры доступа*. обеспечивая возможность доступа к ресурсам, защищенным [сервером авторизации](active-directory-b2c-reference-protocols.md). OpenID Connect Connect рекомендуется использовать, если вы создаете веб-приложение, размещенное на сервере и доступ к которому осуществляется через браузер. Если вы хотите добавить управление удостоверениями в мобильные или классические приложения с помощью Azure AD B2C, следует использовать [OAuth 2,0](active-directory-b2c-reference-oauth-code.md) , а не OpenID Connect Connect. Дополнительные сведения об маркерах см. в разделе [Общие сведения о токенах в Azure Active Directory B2C](active-directory-b2c-reference-tokens.md)

В Azure AD B2C стандартный протокол OpenID Connect выходит за рамки простой проверки подлинности и авторизации. В нем представлен [параметр потока пользователя](active-directory-b2c-reference-policies.md), который позволяет использовать OpenID Connect Connect для добавления в приложение пользовательских интерфейсов, таких как регистрация, вход и управление профилями.

## <a name="send-authentication-requests"></a>Отправка запросов проверки подлинности

Если веб-приложению требуется проверить подлинность пользователя и запустить поток пользователя, он может направить пользователя в `/authorize` конечную точку. Пользователь принимает действие в зависимости от потока пользователя.

В этом запросе клиент указывает разрешения, которые необходимо получить от пользователя в `scope` параметре, и поток пользователя для выполнения `p` в параметре. В следующих разделах приведены три примера для разных потоков пользователей (переносы строк добавлены для удобства чтения). Чтобы понять, как работает каждый запрос, попробуйте вставить его в браузер и запустить. Вы можете заменить `fabrikamb2c` именем своего клиента, если он есть, и создать поток пользователя.

#### <a name="use-a-sign-in-user-flow"></a>Использование потока пользователя для входа
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_in
```

#### <a name="use-a-sign-up-user-flow"></a>Использование потока пользователя для регистрации
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_sign_up
```

#### <a name="use-an-edit-profile-user-flow"></a>Использование потока пользователя для изменения профиля
```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=code+id_token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=form_post
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&p=b2c_1_edit_profile
```

| Параметр | Обязательно для заполнения | Описание |
| --------- | -------- | ----------- |
| client_id | Да | Идентификатор приложения, назначенный приложению [портал Azure](https://portal.azure.com/) . |
| response_type | Да | Необходимо включить маркер идентификации для OpenID Connect Connect. Если веб-приложению также требуются маркеры для вызова веб-API, можно использовать `code+id_token`. |
| redirect_uri | Нет | `redirect_uri` Параметр приложения, где приложения могут отправлять и получать ответы на проверку подлинности. Он должен точно соответствовать одному из `redirect_uri` параметров, зарегистрированных в портал Azure, за исключением того, что он должен быть закодирован URL-адресом. |
| область | Да | Список областей с разделителями-пробелами. Область `openid` определяет разрешение на вход пользователя и получение данных о пользователе в форме маркеров идентификации. `offline_access` Область является необязательной для веб-приложений. Это означает, что для расширенного доступа к ресурсам приложению потребуется *маркер обновления* . |
| response_mode | Нет | Метод, который используется для отправки полученного кода авторизации обратно в приложение. Это может быть `query`, `form_post` или `fragment`.  Для максимальной безопасности рекомендуется использовать режим ответа `form_post`. |
| state | Нет | Значение, включаемое в запрос, которое также возвращается в ответе маркера. Это может быть строка любого контента. Как правило, с целью предотвращения подделки межсайтовых запросов используется генерируемое случайным образом уникальное значение. Состояние также используется для кодирования сведений о состоянии пользователя в приложении до того, как был выполнен запрос проверки подлинности, например на странице, на которой они находились. |
| nonce | Да | Значение, включенное в запрос (созданное приложением), включенное в результирующий маркер идентификации в качестве утверждения. Затем приложение может проверить это значение, чтобы устранить атаки, направленные на воспроизведение маркеров. Это значение обычно представляет собой случайную уникальную строку, которую можно использовать для определения источника запроса. |
| p | Да | Выполняемый пользователем поток. Это имя пользовательского потока, созданного в клиенте Azure AD B2C. Имя потока пользователей должно начинаться с `b2c\_1\_`. |
| prompt | Нет | Требуемый тип взаимодействия с пользователем. Сейчас единственное допустимое значение — `login`, при котором пользователю приходится вводить учетные данные по запросу. |

На этом этапе пользователю предлагается завершить рабочий процесс. Пользователю может потребоваться ввести имя пользователя и пароль, войти с помощью социальных удостоверений или зарегистрироваться в каталоге. В зависимости от того, как определен поток пользователя, может существовать любое другое количество шагов.

После того, как пользователь завершит поток пользователя, в приложении в указанном `redirect_uri` параметре будет возвращен ответ с помощью метода, указанного `response_mode` в параметре. Ответ одинаков для каждого из предыдущих случаев, независимо от потока пользователя.

Успешный ответ с использованием метода `response_mode=fragment` выглядит следующим образом:

```
GET https://aadb2cplayground.azurewebsites.net/#
id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...
&state=arbitrary_data_you_can_receive_in_the_response
```

| Параметр | Описание |
| --------- | ----------- |
| id_token | Маркер идентификации, запрошенный приложением. Вы можете использовать маркер идентификации для проверки личности пользователя и запуска сеанса пользователя. |
| code | Код авторизации, запрошенный приложением, если вы использовали `response_type=code+id_token`. Приложение может использовать код авторизации для запроса маркера доступа к целевому ресурсу. Срок действия кодов авторизации обычно истекает через 10 минут. |
| state | Если запрос содержит параметр `state`, то в ответе должно отображаться то же значение. Приложение должно проверить, `state` совпадают ли значения в запросе и ответе. |

Сообщения об ошибках также могут быть отправлены в `redirect_uri` параметр, чтобы приложение могла соответствующим образом обрабатывать их:

```
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Параметр | Описание |
| --------- | ----------- |
| error | Код, который можно использовать для классификации типов произошедших ошибок. |
| error_description | Конкретное сообщение об ошибке, которое может помочь определить основную причину ошибки проверки подлинности. |
| state | Если запрос содержит параметр `state`, то в ответе должно отображаться то же значение. Приложение должно проверить, `state` совпадают ли значения в запросе и ответе. |

## <a name="validate-the-id-token"></a>Проверка маркера идентификации

Получение маркера идентификации не является достаточным для прохождения пользователем проверки подлинности. Проверьте подпись маркера идентификации и проверьте утверждения в маркере на соответствие требованиям приложения. В Azure AD B2C для подписи маркеров и проверки их правильности используются [веб-маркеры JSON Web Token (JWT)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) и шифрование с открытым ключом. Для проверки JWT доступны многие библиотеки с открытым исходным кодом в зависимости от выбранного языка программирования. Мы рекомендуем изучить различные библиотеки, а не реализовывать логику проверки самостоятельно. 

Azure AD B2C имеет конечную точку метаданных OpenID Connect Connect, которая позволяет приложению получать сведения о Azure AD B2C во время выполнения. Эти сведения включают конечные точки, содержимое маркеров и ключи подписи маркеров. Для каждого потока пользователя в клиенте B2C есть собственный документ метаданных JSON. Например, документ метаданных для потока пользователя `b2c_1_sign_in` в `fabrikamb2c.onmicrosoft.com` находится в каталоге:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/v2.0/.well-known/openid-configuration?p=b2c_1_sign_in`

Одно из свойств этого документа конфигурации — `jwks_uri`, значение которого для точно такого же потока пользователя будет следующим:

`https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/discovery/v2.0/keys?p=b2c_1_sign_in`.

Чтобы определить, какой поток пользователя использовался при подписывании маркера идентификации (и откуда можно получить метаданные), у вас есть два варианта. Во-первых, имя потока пользователя включено в утверждение `acr` маркера идентификации. Другой вариант — закодировать поток пользователя в значении параметра `state` при отправке запроса, а затем декодировать его, чтобы определить используемый поток. Каждый из этих методов является допустимым.

После получения документа метаданных из конечной точки метаданных подключения OpenID Connect можно использовать открытые ключи RSA 256 для проверки подписи маркера идентификации. В этой конечной точке может присутствовать несколько ключей, каждая из которых идентифицируется `kid` утверждением. Заголовок маркера идентификации также содержит утверждение `kid`, указывающее на ключ, который был использован для подписывания маркера идентификации.

Чтобы проверить маркеры от Azure AD B2C, необходимо создать открытый ключ с помощью экспоненты (e) и модуля (n). Необходимо определить, как это сделать на соответствующем языке программирования. Официальную документацию по созданию открытого ключа с помощью протокола RSA можно найти здесь: https://tools.ietf.org/html/rfc3447#section-3.1

После проверки подписи маркера идентификации вам потребуется проверить несколько утверждений. Например:

- Проверьте утверждение `nonce` во избежание атак с использованием воспроизведения маркеров. Его значение должно совпадать с указанным вами в запросе на вход.
- `aud` Проверьте утверждение, чтобы убедиться, что маркер идентификатора был выдан для вашего приложения. Его значение должно быть ИДЕНТИФИКАТОРом приложения.
- Проверьте утверждения `exp` и, чтобы убедиться, что срок действия маркера идентификатора не истек. `iat`

Также существуют некоторые дополнительные проверки, которые необходимо выполнить. Эти проверки подробно описаны в [спецификации OpenID Connect Connect Core](https://openid.net/specs/openid-connect-core-1_0.html). Вам также может потребоваться проверить дополнительные утверждения в зависимости от сценария. Ниже приведены некоторые из стандартных проверок:

- Проверка того, что пользователь или организация зарегистрированы в приложении.
- Предоставление пользователю необходимого уровня авторизации и привилегий.
- Обеспечение определенного уровня проверки подлинности, например с помощью службы Многофакторной идентификации Azure.

После проверки маркера идентификации можно начать сеанс с пользователем. Утверждения в маркере идентификации можно использовать для получения сведений о пользователе в приложении. Эти сведения используются для отображения, получения записей и проверки подлинности.

## <a name="get-a-token"></a>Получение маркера

Если требуется, чтобы веб-приложение выполняло только потоки пользователей, можно пропустить несколько следующих разделов. Эти разделы применимы только к веб-приложениям, которые должны выполнять проверку подлинности вызовов к веб-API и также защищены с помощью Azure AD B2C.

Чтобы воспользоваться кодом авторизации маркера для необходимого ресурса (полученным с помощью `response_type=code+id_token`), отправьте запрос `POST` на конечную точку `/token`. В Azure AD B2C можно [запросить маркеры доступа для других API](active-directory-b2c-access-tokens.md#request-a-token) как обычно, указав их области (в запросе).

Вы также можете запросить маркер доступа для собственного серверного веб-API в соответствии с соглашением об использовании идентификатора клиента приложения в качестве запрошенной области (что приведет к маркеру доступа с таким ИДЕНТИФИКАТОРом клиента, как "аудитория"):

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access&code=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Параметр | Обязательно для заполнения | Описание |
| --------- | -------- | ----------- |
| p | Да | Поток пользователя, который использовался для получения кода авторизации. В этом запросе нельзя использовать другой поток пользователя. Добавьте этот параметр в строку запроса, а не в тело сообщения POST. |
| client_id | Да | Идентификатор приложения, назначенный приложению [портал Azure](https://portal.azure.com/) . |
| grant_type | Да | Тип предоставления. Для потока кода авторизации это должен быть тип `authorization_code`. |
| область | Нет | Список областей с разделителями-пробелами. Область `openid` определяет разрешение на вход пользователя и получение данных о пользователе в форме маркеров идентификации. Его можно использовать для получения маркеров для собственного серверного веб-интерфейса API, который представляется тем же ИДЕНТИФИКАТОРом приложения, что и клиент. `offline_access` Область указывает, что приложению требуется маркер обновления для расширенного доступа к ресурсам. |
| code | Да | Код авторизации, полученный в начале потока пользователя. |
| redirect_uri | Да | Параметр `redirect_uri` приложения, в котором вы получили код авторизации. |
| client_secret | Да | Секрет приложения, созданный в [портал Azure](https://portal.azure.com/). Это важный артефакт безопасности, и он должен безопасно храниться на сервере. Измените секрет клиента на периодической основе. |

Успешный ответ маркера выглядит следующим образом:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Параметр | Описание |
| --------- | ----------- |
| not_before | Момент времени, в который маркер начинает считаться действительным (с начала эпохи). |
| token_type | Значение типа маркера. `Bearer`— единственный поддерживаемый тип. |
| access_token | Подписанный маркер JWT, который вы запрашивали. |
| область | Области, для которых действителен маркер. |
| expires_in | Срок действия маркера доступа (в секундах). |
| refresh_token | Маркер обновления OAuth 2.0. Приложение может использовать этот токен для получения дополнительных маркеров после истечения срока действия текущего маркера. Маркеры обновления можно использовать для получения доступа к ресурсам в течение продолжительного периода времени. Для получения `offline_access` маркера обновления область должна использоваться как в запросах авторизации, так и на маркерах. |

Сообщения об ошибках выглядят следующим образом:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Параметр | Описание |
| --------- | ----------- |
| error | Код, который можно использовать для классификации типов произошедших ошибок. |
| error_description | Сообщение, которое может помочь определить основную причину ошибки проверки подлинности. |

## <a name="use-the-token"></a>Использование маркера

После успешного получения маркера доступа маркер можно использовать в запросах к веб-API серверной части приложения путем включения маркера в заголовок `Authorization`:

```
GET /tasks
Host: https://mytaskwebapi.com
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
```

## <a name="refresh-the-token"></a>Обновление маркера

Срок действия маркеров идентификатора истекает через короткий период времени. Обновите токены после истечения срока их действия, чтобы продолжить доступ к ресурсам. Токен можно обновить, отправив другой `POST` запрос `/token` в конечную точку. На этот раз укажите параметр `refresh_token` вместо параметра `code`:

```
POST fabrikamb2c.onmicrosoft.com/oauth2/v2.0/token?p=b2c_1_sign_in HTTP/1.1
Host: https://fabrikamb2c.b2clogin.com
Content-Type: application/x-www-form-urlencoded

grant_type=refresh_token&client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6&scope=openid offline_access&refresh_token=AwABAAAAvPM1KaPlrEqdFSBzjqfTGBCmLdgfSTLEMPGYuNHSUYBrq...&redirect_uri=urn:ietf:wg:oauth:2.0:oob&client_secret=<your-application-secret>
```

| Параметр | Обязательно для заполнения | Описание |
| --------- | -------- | ----------- |
| p | Да | Поток пользователя, который использовался для получения исходного маркера обновления. В этом запросе нельзя использовать другой поток пользователя. Добавьте этот параметр в строку запроса, а не в тело сообщения POST. |
| client_id | Да | Идентификатор приложения, назначенный приложению [портал Azure](https://portal.azure.com/) . |
| grant_type | Да | Тип предоставления, который должен быть маркером обновления для этой части потока кода авторизации. |
| область | Нет | Список областей с разделителями-пробелами. Область `openid` определяет разрешение на вход пользователя и получение данных о пользователе в форме маркеров идентификации. Его можно использовать для отправки маркеров в собственный серверный веб-API приложения, который представляется тем же ИДЕНТИФИКАТОРом приложения, что и клиент. `offline_access` Область указывает, что приложению требуется маркер обновления для расширенного доступа к ресурсам. |
| redirect_uri | Нет | Параметр `redirect_uri` приложения, в котором вы получили код авторизации. |
| refresh_token | Да | Исходный маркер обновления, полученный во второй части последовательности. Для получения маркера обновления областьдолжнаиспользоватьсякаквзапросахавторизации,такинамаркерах.`offline_access` |
| client_secret | Да | Секрет приложения, созданный в [портал Azure](https://portal.azure.com/). Это важный артефакт безопасности, и он должен безопасно храниться на сервере. Измените секрет клиента на периодической основе. |

Успешный ответ маркера выглядит следующим образом:

```
{
    "not_before": "1442340812",
    "token_type": "Bearer",
    "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...",
    "scope": "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
    "expires_in": "3600",
    "refresh_token": "AAQfQmvuDy8WtUv-sd0TBwWVQs1rC-Lfxa_NDkLqpg50Cxp5Dxj0VPF1mx2Z...",
}
```
| Параметр | Описание |
| --------- | ----------- |
| not_before | Момент времени, в который маркер начинает считаться действительным (с начала эпохи). |
| token_type | Значение типа маркера. `Bearer`— единственный поддерживаемый тип. |
| access_token | Подписанный токен JWT, который был запрошен. |
| область | Область, для которой токен является допустимым. |
| expires_in | Срок действия маркера доступа (в секундах). |
| refresh_token | Маркер обновления OAuth 2.0. Приложение может использовать этот токен для получения дополнительных маркеров после истечения срока действия текущего маркера. Маркеры обновления можно использовать для получения доступа к ресурсам в течение продолжительного периода времени. |

Сообщения об ошибках выглядят следующим образом:

```
{
    "error": "access_denied",
    "error_description": "The user revoked access to the app.",
}
```

| Параметр | Описание |
| --------- | ----------- |
| error | Код, который можно использовать для классификации типов произошедших ошибок. |
| error_description | Сообщение, которое может помочь определить основную причину ошибки проверки подлинности. |

## <a name="send-a-sign-out-request"></a>Отправка запроса на выход

Если вы хотите подписать пользователя из приложения, достаточно очистить файлы cookie приложения или иным способом завершить сеанс с пользователем. Перенаправление пользователя в Azure AD B2C для выхода. Если этого не сделать, пользователь может выполнить повторную проверку подлинности в приложении без повторного ввода учетных данных.

Можно просто перенаправить пользователя на `end_session` конечную точку, которая указана в документе OpenID Connect Connect metadata, описанном ранее.

```
GET https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/oauth2/v2.0/logout?
p=b2c_1_sign_in
&post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Параметр | Обязательно для заполнения | Описание |
| --------- | -------- | ----------- |
| p | Да | Поток пользователя, который вы хотите использовать для выхода пользователя из приложения. |
| post_logout_redirect_uri | Нет | URL-адрес, по которому необходимо перенаправить пользователя после успешного выхода. Если он не указан, Azure AD B2C показывает пользователю универсальное сообщение. |

При перенаправлении пользователя в `end_session` конечную точку часть состояния единого входа пользователя отменяется Azure AD B2C, но не выполняется выход пользователя из сеанса поставщика удостоверений социальных сетей (IDP). Если пользователь выбирает один и тот же IDP во время последующего входа, они проходят повторную проверку подлинности без ввода учетных данных. Если пользователь хочет выйти из приложения, это не обязательно означает, что нужно выйти из своей учетной записи Facebook. Однако если используются локальные учетные записи, то сеанс пользователя завершается правильно.

