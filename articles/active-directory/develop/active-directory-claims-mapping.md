---
title: Настройка утверждений для приложения в клиенте Azure AD (общедоступная Предварительная версия)
description: На этой странице описываются сопоставления утверждений Azure Active Directory.
services: active-directory
author: rwike77
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.custom: aaddev
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2019
ms.author: ryanwi
ms.reviewer: paulgarn, hirsin, jeedes, luleon
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97de45ef94afa9da8a5e928a3d4a8911db052107
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68381066"
---
# <a name="how-to-customize-claims-emitted-in-tokens-for-a-specific-app-in-a-tenant-preview"></a>Практическое руководство. Настройка утверждений, добавляемых в токены для определенных служб в клиенте (предварительная версия)

> [!NOTE]
> Эта функция заменяет собой компонент [настройки утверждений](active-directory-saml-claims-customization.md), предлагаемый сегодня на портале. При настройке утверждений в одном и том же приложении с помощью портала, помимо метода Graph/PowerShell, подробно описанного в данном документе, токены, выдаваемые для этого приложения, будут игнорировать конфигурацию на портале. Конфигурации, настроенные с помощью способов, рассмотренных в этом документе, не будут отражены на портале.

Эта функция используется администраторами клиента для настройки утверждений, добавленных в токены, для определенных приложений в своих клиентах. Политики сопоставления утверждений можно использовать в следующих целях.

- Выбор утверждений, добавляемых в токены.
- Создание типов утверждений, которые еще не существуют.
- Выбор или изменение источника данных, добавленного в определенные утверждения.

> [!NOTE]
> Эта функция сейчас предоставляется в режиме общедоступной предварительной версии. Вы должны быть готовы отменить или удалить все изменения. На этапе общедоступной предварительной версии эта функция доступна во всех подписках Azure Active Directory (Azure AD). Однако, когда функция станет общедоступной, для некоторых аспектов компонента может потребоваться подписка Azure AD Premium. Эта функция поддерживает настройку политик сопоставления утверждений для протоколов WS-Fed, SAML, OAuth и OpenID Connect.

## <a name="claims-mapping-policy-type"></a>Тип политики сопоставления утверждений

В Azure AD объект **политика** представлен набором правил, которые применяются к отдельным приложениям или ко всем приложениям в организации. Каждый тип политики имеет уникальную структуру и набор свойств, которые применяются к объектам, для которых назначена эта политика.

Тип политики сопоставления утверждений — это тип объекта **политики**, который изменяет утверждения, которые добавлены в токены, выданные определенным приложениям.

## <a name="claim-sets"></a>Наборы утверждений

Существуют определенные наборы утверждений, которые определяют, как и когда они используются в токенах.

| Набор утверждений | Описание |
|---|---|
| Набор основных утверждений | Имеется в каждом токене, независимо от политики. Эти утверждения также считаются ограниченными, изменить их не удастся. |
| Набор базовых утверждений | Включает в себя утверждения, которые по умолчанию добавляются в токены (в дополнение к основному набору утверждений). Вы можете опустить или изменить базовые утверждения с помощью политик сопоставления утверждений. |
| Набор ограниченных утверждений | Невозможно изменить с помощью политики. Для таких утверждений также невозможно изменить источник данных, а при их создании не применяются преобразования. |

### <a name="table-1-json-web-token-jwt-restricted-claim-set"></a>Таблица 1. Набор ограниченных утверждений JSON Web Token (JWT)

| Тип утверждения (имя) |
| ----- |
| _claim_names |
| _claim_sources |
| access_token |
| account_type |
| acr |
| actor |
| actortoken |
| aio |
| altsecid |
| amr |
| app_chain |
| app_displayname |
| app_res |
| appctx |
| appctxsender |
| appid |
| appidacr |
| assertion |
| at_hash |
| aud |
| auth_data |
| auth_time |
| authorization_code |
| azp |
| azpacr |
| c_hash |
| ca_enf |
| cc |
| cert_token_use |
| client_id |
| cloud_graph_host_name |
| cloud_instance_name |
| cnf |
| code |
| controls |
| credential_keys |
| csr |
| csr_type |
| deviceid |
| dns_names |
| domain_dns_name |
| domain_netbios_name |
| e_exp |
| email |
| endpoint |
| enfpolids |
| exp |
| expires_on |
| grant_type |
| graph |
| group_sids |
| groups |
| hasgroups |
| hash_alg |
| home_oid |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant` |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod` |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration` |
| `http://schemas.microsoft.com/ws/2008/06/identity/claims/expired` |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress` |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` |
| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` |
| iat |
| identityprovider |
| idp |
| in_corp |
| instance |
| ipaddr |
| isbrowserhostedapp |
| iss |
| jwk |
| key_id |
| key_type |
| mam_compliance_url |
| mam_enrollment_url |
| mam_terms_of_use_url |
| mdm_compliance_url |
| mdm_enrollment_url |
| mdm_terms_of_use_url |
| nameid |
| nbf |
| netbios_name |
| nonce |
| oid |
| on_prem_id |
| onprem_sam_account_name |
| onprem_sid |
| openid2_id |
| password |
| platf |
| polids |
| pop_jwk |
| preferred_username |
| previous_refresh_token |
| primary_sid |
| puid |
| pwd_exp |
| pwd_url |
| redirect_uri |
| refresh_token |
| refreshtoken |
| request_nonce |
| resource |
| role |
| roles |
| scope |
| scp |
| sid |
| signature |
| signin_state |
| src1 |
| src2 |
| sub |
| tbid |
| tenant_display_name |
| tenant_region_scope |
| thumbnail_photo |
| tid |
| tokenAutologonEnabled |
| trustedfordelegation |
| unique_name |
| upn |
| user_setting_sync_url |
| username |
| uti |
| ver |
| verified_primary_email |
| verified_secondary_email |
| wids |
| win_ver |

### <a name="table-2-saml-restricted-claim-set"></a>Таблица 2. Набор ограниченных утверждений SAML

| Тип утверждения (URI) |
| ----- |
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/expired`|
|`http://schemas.microsoft.com/identity/claims/accesstoken`|
|`http://schemas.microsoft.com/identity/claims/openid2_id`|
|`http://schemas.microsoft.com/identity/claims/identityprovider`|
|`http://schemas.microsoft.com/identity/claims/objectidentifier`|
|`http://schemas.microsoft.com/identity/claims/puid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier [MR1]`|
|`http://schemas.microsoft.com/identity/claims/tenantid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod`|
|`http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/groups`|
|`http://schemas.microsoft.com/claims/groups.link`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/role`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/wids`|
|`http://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant`|
|`http://schemas.microsoft.com/2014/02/devicecontext/claims/isknown`|
|`http://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged`|
|`http://schemas.microsoft.com/2014/03/psso`|
|`http://schemas.microsoft.com/claims/authnmethodsreferences`|
|`http://schemas.xmlsoap.org/ws/2009/09/identity/claims/actor`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/samlissuername`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/confirmationkey`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authorizationdecision`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarygroupsid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarysid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/denyonlysid`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlywindowsdevicegroup`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdeviceclaim`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsfqbnversion`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowssubauthority`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsuserclaim`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/x500distinguishedname`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn`|
|`http://schemas.microsoft.com/ws/2008/06/identity/claims/ispersistent`|
|`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier`|
|`http://schemas.microsoft.com/identity/claims/scope`|

## <a name="claims-mapping-policy-properties"></a>Свойства политики сопоставления утверждений

Чтобы контролировать, какие утверждения добавляются и где находится источник данных, используйте свойства политики сопоставления утверждений. Если политика не задана, система выдает токены, содержащие набор основных утверждений, набор базовых утверждений и любые [необязательные утверждения](active-directory-optional-claims.md), выбранные приложением для получения.

### <a name="include-basic-claim-set"></a>Включение набора базовых утверждений

**Строка.** IncludeBasicClaimSet

**Тип данных**: Логическое значение: true или false

**Описание.** Это свойство определяет, включен ли набор базовых утверждений в токены, на которые распространяется эта политика.

- Если задано значение True, все утверждения в наборе базовых утверждений добавляются в токены, на которые распространяется эта политика. 
- Если задано значение False, утверждения в наборе базовых утверждений не добавляются в токены, если только они не добавлены по отдельности в свойстве схемы утверждения этой же политики.

> [!NOTE] 
> Утверждения из набора основных утверждений имеются в каждом токене, независимо от политики. 

### <a name="claims-schema"></a>Схема утверждений

**Строка.** ClaimsSchema

**Тип данных**: большой двоичный объект JSON с одной или несколькими записями схемы утверждения

**Описание.** Это свойство определяет, какие утверждения присутствуют в токенах, на которые распространяется эта политика, помимо набора базовых утверждений и набора основных утверждений.
Для каждой записи схемы утверждения, определенной в этом свойстве, требуются определенные сведения. Укажите, откуда поступают данные (**Значение** или **Пара источник/идентификатор**), а также утверждение, в которое они добавляются (**Тип утверждения**).

### <a name="claim-schema-entry-elements"></a>Элементы записи схемы утверждения

**Значение:** элемент "Значение" определяет статическое значение в качестве данных для добавления в утверждение.

**Пара "источник-идентификатор":** элементы Source и ID определяют, где находится источник данных в утверждении. 

Для элемента "Источник" необходимо задать одно из следующих значений. 

- "user": данные в утверждении являются свойством объекта "User". 
- "application": данные в утверждении являются свойством субъекта-службы приложения (клиента). 
- "resource": данные в утверждении являются свойством субъекта-службы ресурса.
- "audience": данные в утверждении — это свойство субъекта-службы, который является аудиторией токена (либо клиент, либо субъект-служба ресурса).
- “company”: данные в утверждении являются свойством объекта "Company" в клиенте ресурса.
- "transformation": данные в утверждении берутся из преобразования утверждений (см. раздел "Преобразование утверждений" далее в этой статье).

Если источником является преобразование, в это определение утверждения также необходимо добавить элемент **TransformationID**.

Элемент ID определяет, какое свойство в источнике предоставляет значение для утверждения. В следующей таблице перечислены значения идентификатора, допустимые для каждого значения источника.

#### <a name="table-3-valid-id-values-per-source"></a>Таблица 3. Допустимый идентификатор значения для источника

| Source | id | Описание |
|-----|-----|-----|
| пользователя | surname | Фамилия |
| пользователя | givenname | Заданное имя |
| пользователя | displayname | Название |
| пользователя | objectid | ObjectID |
| пользователя | почта | Электронная почта |
| пользователя | userprincipalname | Имя участника-пользователя |
| пользователя | department|Department|
| пользователя | onpremisessamaccountname | Имя локальной учетной записи SAM |
| пользователя | netbiosname| NetBIOS-имя |
| пользователя | dnsdomainname | Доменное имя DNS |
| пользователя | onpremisesecurityidentifier | Локальный идентификатор безопасности |
| пользователя | companyname| Название организации |
| пользователя | streetaddress | Почтовый адрес |
| пользователя | postalcode | Почтовый индекс |
| пользователя | preferredlanguange | Предпочитаемый язык |
| пользователя | onpremisesuserprincipalname | Имя участника-пользователя в локальной среде |
| пользователя | mailNickname | Почтовый псевдоним |
| пользователя | extensionattribute1 | Атрибут расширения 1 |
| пользователя | extensionattribute2 | Атрибут расширения 2 |
| пользователя | extensionattribute3 | Атрибут расширения 3 |
| пользователя | extensionattribute4 | Атрибут расширения 4 |
| пользователя | extensionattribute5 | Атрибут расширения 5 |
| пользователя | extensionattribute6 | Атрибут расширения 6 |
| Пользователь | extensionattribute7 | Атрибут расширения 7 |
| пользователя | extensionattribute8 | Атрибут расширения 8 |
| пользователя | extensionattribute9 | Атрибут расширения 9 |
| пользователя | extensionattribute10 | Атрибут расширения 10 |
| пользователя | extensionattribute11 | Атрибут расширения 11 |
| пользователя | extensionattribute12 | Атрибут расширения 12 |
| пользователя | extensionattribute13 | Атрибут расширения 13 |
| пользователя | extensionattribute14 | Атрибут расширения 14 |
| пользователя | extensionattribute15 | Атрибут расширения 15 |
| пользователя | othermail | Остальные сообщения |
| пользователя | country | Country |
| пользователя | city | City |
| пользователя | state | Область |
| пользователя | jobtitle | Должность |
| пользователя | employeeid | Код сотрудника |
| пользователя | facsimiletelephonenumber | Номер телефона, факса |
| application, resource, audience | displayName | Название |
| application, resource, audience | objected | ObjectID |
| application, resource, audience | теги | Тег субъекта-службы |
| Компания | tenantcountry | Страна клиента |

**TransformationID:** элемент TransformationID нужно указывать, только если для элемента "Источник" задано значение "transformation".

- Этот элемент должен соответствовать элементу ID записи преобразования в свойстве **ClaimsTransformation**, которое определяет порядок создания данных для этого утверждения.

**Тип утверждения:** элементы **JwtClaimType** и **SamlClaimType** определяют, к какому утверждению относится эта запись схемы утверждения.

- Тип JwtClaimType должен содержать имя утверждения для добавления в JWT.
- Тип SamlClaimType должен содержать URI утверждения для добавления в токены SAML.

> [!NOTE]
> Имена и URI утверждений в наборе ограниченных утверждений не удастся использовать для элементов типа утверждения. Дополнительные сведения см. в разделе "Исключения и ограничения" далее в этой статье.

### <a name="claims-transformation"></a>Преобразование утверждений

**Строка.** ClaimsTransformation

**Тип данных**: большой двоичный объект JSON с одной или несколькими записями преобразования. 

**Описание.** Это свойство используется для применения общих преобразований источника данных, для создания выходных данных для утверждений, указанных в схеме утверждений.

**ID:** элемент ID следует использовать для ссылки на эту запись преобразования в записи схемы утверждений TransformationID. Это значение должно быть уникальным для каждой записи преобразования в этой политике.

**TransformationMethod:** элемент TransformationMethod определяет, какая операция выполняется для создания данных для утверждения.

В зависимости от выбранного метода ожидается набор входных и выходных данных. Определите входные и выходные данные с помощью элементов **InputClaims**, **InputParameters** и **OutputClaims**.

#### <a name="table-4-transformation-methods-and-expected-inputs-and-outputs"></a>Таблица 4. Методы преобразования, ожидаемые входные и выходные данные

|TransformationMethod|Ожидаемые входные данные|Ожидаемые выходные данные|Описание|
|-----|-----|-----|-----|
|Присоединиться|строка 1, строка 2, разделитель|outputClaim|Объединение входных строк с помощью разделителя между ними. Например, результатом строка 1:"foo@bar.com", строка 2:"sandbox", разделитель:"." будет outputClaim:"foo@bar.com.sandbox"|
|ExtractMailPrefix|почта|outputClaim|Извлекает локальную часть адреса электронной почты. Например, результатом mail:"foo@bar.com" будет outputClaim:"foo". Если символ \@ отсутствует, то исходная входная строка возвращается в состоянии "как есть".|

**InputClaims:** элемент InputClaims используется для передачи данных из записи схемы утверждения в преобразование. Он имеет два атрибута: **ClaimTypeReferenceId** и **TransformationClaimType**.

- **ClaimTypeReferenceId** соединяется с элементом ID записи схемы утверждения для поиска соответствующего входного утверждения. 
- **TransformationClaimType** используется для предоставления уникального имени для этих входных данных. Это имя должно соответствовать одному из ожидаемых входных данных для метода преобразования.

**InputParameters:** элемент InputParameters используется для передачи значения константы в преобразование. Он имеет два атрибута: **Value** и **ID**.

- **Value** — это фактическое значение константы для передачи.
- **Идентификатор** используется для предоставления уникального имени для входных данных. Это имя должно соответствовать одному из ожидаемых входных данных для метода преобразования.

**OutputClaims:** элемент OutputClaims используется для хранения данных, сгенерированных преобразованием, и привязки этих данных к записи схемы утверждения. Он имеет два атрибута: **ClaimTypeReferenceId** и **TransformationClaimType**.

- **ClaimTypeReferenceId** соединяется с элементом ID записи схемы утверждения для поиска соответствующего выходного утверждения.
- **TransformationClaimType** используется для предоставления уникального имени для выходных данных. Это имя должно соответствовать одному из ожидаемых выходных данных для метода преобразования.

### <a name="exceptions-and-restrictions"></a>Исключения и ограничения

**Идентификатор имени SAML NameID и имя участника-пользователя:** список атрибутов, из которых берутся значения NameID и UPN, а также список допустимых преобразований утверждений ограничены. Чтобы увидеть допустимые значения, см. таблицы 5 и 6.

#### <a name="table-5-attributes-allowed-as-a-data-source-for-saml-nameid"></a>Таблица 5. Атрибуты, разрешенные в качестве источника данных для идентификатора имени SAML NameID

|Source|id|Описание|
|-----|-----|-----|
| пользователя | почта|Электронная почта|
| пользователя | userprincipalname|Имя участника-пользователя|
| пользователя | onpremisessamaccountname|Имя локальной учетной записи SAM|
| пользователя | employeeid|Код сотрудника|
| пользователя | extensionattribute1 | Атрибут расширения 1 |
| пользователя | extensionattribute2 | Атрибут расширения 2 |
| пользователя | extensionattribute3 | Атрибут расширения 3 |
| пользователя | extensionattribute4 | Атрибут расширения 4 |
| пользователя | extensionattribute5 | Атрибут расширения 5 |
| пользователя | extensionattribute6 | Атрибут расширения 6 |
| Пользователь | extensionattribute7 | Атрибут расширения 7 |
| пользователя | extensionattribute8 | Атрибут расширения 8 |
| пользователя | extensionattribute9 | Атрибут расширения 9 |
| пользователя | extensionattribute10 | Атрибут расширения 10 |
| пользователя | extensionattribute11 | Атрибут расширения 11 |
| пользователя | extensionattribute12 | Атрибут расширения 12 |
| пользователя | extensionattribute13 | Атрибут расширения 13 |
| пользователя | extensionattribute14 | Атрибут расширения 14 |
| пользователя | extensionattribute15 | Атрибут расширения 15 |

#### <a name="table-6-transformation-methods-allowed-for-saml-nameid"></a>Таблица 6. Разрешенные методы преобразования для идентификатора имени SAML NameID

| TransformationMethod | Ограничения |
| ----- | ----- |
| ExtractMailPrefix | Отсутствуют |
| Присоединиться | Присоединяемый суффикс должен быть подтвержденным доменом клиента ресурса. |

### <a name="custom-signing-key"></a>Пользовательский ключ подписывания

Чтобы политика сопоставления утверждений вступила в силу, объекту субъекта-службы необходимо назначить пользовательский ключ подписывания. Это гарантирует подтверждение того, что токены были изменены создателем политики сопоставления утверждений и защищают приложения от политик сопоставления утверждений, созданных злоумышленниками.  Приложения с включенным сопоставлением утверждений должны проверять Специальный универсальный код ресурса (URI) для ключей `appid={client_id}` подписывания маркера путем добавления к своим [запросам метаданных Connect OpenID Connect](v2-protocols-oidc.md#fetch-the-openid-connect-metadata-document).  

### <a name="cross-tenant-scenarios"></a>Межклиентские сценарии

Политики сопоставления утверждений не применяются к гостевым пользователям. Если гость пытается получить доступ к приложению, к субъекту-службе которого применяется политика сопоставления утверждений, выдается токен по умолчанию (политика не применяется).

## <a name="claims-mapping-policy-assignment"></a>Назначение политики сопоставления утверждений

Политики сопоставления утверждений можно назначить только объектам субъекта-службы.

### <a name="example-claims-mapping-policies"></a>Примеры политик сопоставления утверждений

В Azure AD существует множество сценариев, когда можно настроить утверждения, добавляемые в токены для определенных субъектов-служб. В этом разделе рассматриваются наиболее распространенные сценарии, которые помогут вам понять, как использовать политики сопоставления утверждений.

#### <a name="prerequisites"></a>предварительные требования

В следующих примерах мы будем создавать, обновлять, связывать и удалять политики для субъектов-служб. Если вы еще не работали с Azure AD, прежде чем продолжить работу с этими примерами, советуем ознакомиться со статьей [Краткое руководство: настройка среды разработки](quickstart-create-new-tenant.md).

Чтобы начать работу, сделайте следующее:

1. Cкачайте последнюю [общедоступную предварительную версию модуля Azure AD PowerShell](https://www.powershellgallery.com/packages/AzureADPreview).
1. Чтобы войти в учетную запись администратора Azure AD, выполните команду Connect. Эту команду следует выполнять при каждом запуске сеанса.

   ``` powershell
   Connect-AzureAD -Confirm
   ```
1. Чтобы просмотреть все политики, которые созданы в организации, выполните следующую команду. Эту команду рекомендуется запускать после выполнения большинства операций в следующих сценариях, чтобы проверить, что политики создаются должным образом.

   ``` powershell
   Get-AzureADPolicy
   ```

#### <a name="example-create-and-assign-a-policy-to-omit-the-basic-claims-from-tokens-issued-to-a-service-principal"></a>Пример Создание и назначение политики для пропуска базовых утверждений от маркеров, выданных субъекту-службе

В этом примере мы создадим политику, которая удаляет набор базовых утверждений из токенов, выданных связанным субъектам-службам.

1. Создайте политику сопоставления утверждений. Эта политика, связанная с определенными субъектами-службами, удаляет из токенов набор базовых утверждений.
   1. Чтобы создать политику, выполните следующую команду: 
    
      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"false"}}') -DisplayName "OmitBasicClaims" -Type "ClaimsMappingPolicy"
      ```
   2. Чтобы просмотреть созданную политику и получить ее идентификатор объекта, выполните следующую команду:
    
      ``` powershell
      Get-AzureADPolicy
      ```
1. Назначьте политику для субъекта-службы. Вам потребуется также получить идентификатор объекта субъекта-службы.
   1. Чтобы просмотреть список всех субъектов-служб в организации, можно запросить Microsoft Graph или открыть проводник Azure AD Graph и войти в учетную запись Azure AD.
   2. Получив идентификатор объекта субъекта-службы, выполните следующую команду:  
     
      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

#### <a name="example-create-and-assign-a-policy-to-include-the-employeeid-and-tenantcountry-as-claims-in-tokens-issued-to-a-service-principal"></a>Пример создание и назначение политики для добавления элементов EmployeeID и TenantCountry в качестве утверждений в токены, выданные субъекту-службе.

В этом примере мы создадим политику, которая добавляет элементы EmployeeID и TenantCountry в токены, выданные связанным субъектам-службам. Элемент EmployeeID добавляется как тип утверждения имени в токены SAML и JWT. Элемент TenantCountry добавляется как тип утверждения страны в токены SAML и JWT. В этом примере мы продолжаем включать набор базовых утверждений в токены.

1. Создайте политику сопоставления утверждений. Эта политика, связанная с субъектами-службами, добавляет утверждения EmployeeID и TenantCountry в токены.
   1. Чтобы создать политику, выполните следующую команду:  
     
      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name","JwtClaimType":"name"},{"Source":"company","ID":"tenantcountry","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country","JwtClaimType":"country"}]}}') -DisplayName "ExtraClaimsExample" -Type "ClaimsMappingPolicy"
      ```
    
   2. Чтобы просмотреть созданную политику и получить ее идентификатор объекта, выполните следующую команду:
     
      ``` powershell  
      Get-AzureADPolicy
      ```
1. Назначьте политику для субъекта-службы. Вам потребуется также получить идентификатор объекта субъекта-службы. 
   1. Чтобы просмотреть список всех субъектов-служб в организации, можно запросить Microsoft Graph или открыть проводник Azure AD Graph и войти в учетную запись Azure AD.
   2. Получив идентификатор объекта субъекта-службы, выполните следующую команду:  
     
      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

#### <a name="example-create-and-assign-a-policy-that-uses-a-claims-transformation-in-tokens-issued-to-a-service-principal"></a>Пример создание и назначение политики, которая использует преобразование утверждений в токенах, выданных субъекту-службе.

В этом примере мы создадим политику, которая добавляет пользовательское утверждение "JoinedData" в токены JWT, выданные связанным субъектам-службам. Это утверждение содержит значение, созданное путем объединения данных, хранящихся в атрибуте extensionattribute1 в объекте пользователя с элементом ".sandbox". В этом примере мы исключим набор базовых утверждений из токенов.

1. Создайте политику сопоставления утверждений. Эта политика, связанная с субъектами-службами, добавляет утверждения EmployeeID и TenantCountry в токены.
   1. Чтобы создать политику, выполните следующую команду:
     
      ``` powershell
      New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema":[{"Source":"user","ID":"extensionattribute1"},{"Source":"transformation","ID":"DataJoin","TransformationId":"JoinTheData","JwtClaimType":"JoinedData"}],"ClaimsTransformations":[{"ID":"JoinTheData","TransformationMethod":"Join","InputClaims":[{"ClaimTypeReferenceId":"extensionattribute1","TransformationClaimType":"string1"}], "InputParameters": [{"ID":"string2","Value":"sandbox"},{"ID":"separator","Value":"."}],"OutputClaims":[{"ClaimTypeReferenceId":"DataJoin","TransformationClaimType":"outputClaim"}]}]}}') -DisplayName "TransformClaimsExample" -Type "ClaimsMappingPolicy"
      ```
    
   2. Чтобы просмотреть созданную политику и получить ее идентификатор объекта, выполните следующую команду: 
     
      ``` powershell
      Get-AzureADPolicy
      ```
1. Назначьте политику для субъекта-службы. Вам потребуется также получить идентификатор объекта субъекта-службы. 
   1. Чтобы просмотреть список всех субъектов-служб в организации, можно запросить Microsoft Graph или открыть проводник Azure AD Graph и войти в учетную запись Azure AD.
   2. Получив идентификатор объекта субъекта-службы, выполните следующую команду: 
     
      ``` powershell
      Add-AzureADServicePrincipalPolicy -Id <ObjectId of the ServicePrincipal> -RefObjectId <ObjectId of the Policy>
      ```

## <a name="see-also"></a>См. также

Сведения о настройке утверждений, выпущенных в токене SAML, с помощью портал Azure [см. в разделе как Настройка утверждений, выданных в токене SAML для корпоративных приложений](active-directory-saml-claims-customization.md)