---
title: ClaimsSchema в Azure Active Directory B2C | Документация Майкрософт
description: Сведения об указании элемента ClaimsSchema настраиваемой политики в Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 132dd91ba121fc5939a0f30194fe4abdd3755414
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67847051"
---
# <a name="claimsschema"></a>ClaimsSchema

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Элемент **ClaimsSchema** определяет типы утверждений, на которые можно ссылаться в рамках политики. Схема утверждений — это место, где вы объявляете свои утверждения. Утверждением может быть имя, фамилия, отображаемое имя, номер телефона и т. д. Элемент ClaimsSchema содержит список элементов **ClaimType**. Элемент **ClaimType** содержит атрибут **Id**, который является именем утверждения.

```XML
<BuildingBlocks>
  <ClaimsSchema>
    <ClaimType Id="Id">
      <DisplayName>Surname</DisplayName>
      <DataType>string</DataType>
      <DefaultPartnerClaimTypes>
        <Protocol Name="OAuth2" PartnerClaimType="family_name" />
        <Protocol Name="OpenIdConnect" PartnerClaimType="family_name" />
        <Protocol Name="SAML2" PartnerClaimType="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" />
      </DefaultPartnerClaimTypes>
      <UserHelpText>Your surname (also known as family name or last name).</UserHelpText>
      <UserInputType>TextBox</UserInputType>
```

## <a name="claimtype"></a>ClaimType

Элемент **ClaimType** содержит следующий атрибут.

| Атрибут | Обязательно для заполнения | Описание |
| --------- | -------- | ----------- |
| Id | Да | Идентификатор, который используется для типа утверждения. Другие элементы могут использовать этот идентификатор в политике. |

Элемент **ClaimType** содержит следующие элементы.

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| DisplayName | 0:1 | Заголовок, который отображается для пользователей на различных экранах. Значение может быть [локализовано](localization.md). |
| DataType | 0:1 | Тип утверждения. Могут использоваться такие типы данных: boolean, date, dateTime, int, long, string, stringCollection, alternativeSecurityIdCollection. |
| DefaultPartnerClaimTypes | 0:1 | Типы стандартных утверждений партнера для использования по указанному протоколу. Значение может быть перезаписано в свойстве **PartnerClaimType**, указанном в элементах **InputClaim** или **OutputClaim**. Этот элемент используется для указания имени по умолчанию для протокола.  |
| Mask | 0:1 | Необязательная строка знаков маскировки, которые могут применяться при отображении утверждения. Например, телефонный номер 324-232-4343 можно замаскировать как XXX-XXX-4343. |
| UserHelpText | 0:1 | Описание типа утверждения, которое может помочь пользователям понять его назначение. Значение может быть [локализовано](localization.md). |
| UserInputType | 0:1 | Тип элемента управления для ввода, который должен быть доступен пользователю, когда он вручную вводит данные утверждения для типа утверждения. См. типы элементов для ввода данных пользователем, определенные ниже на этой странице. |
| Ограничение | 0:1 | Ограничения значений для этого утверждения, такие как регулярное выражение или список допустимых значений. Значение может быть [локализовано](localization.md). |
PredicateValidationReference| 0:1 | Ссылка на элемент **PredicateValidationsInput**. Элементы **PredicateValidationReference** позволяют выполнить процесс проверки, чтобы обеспечить ввод только правильно сформированных данных. Дополнительные сведения см. в статье [Элементы Predicates и PredicateValidations](predicates.md). |

### <a name="defaultpartnerclaimtypes"></a>DefaultPartnerClaimTypes

**DefaultPartnerClaimTypes** может содержать следующий элемент:

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| Протокол | 0:n | Список протоколов с указанием имени стандартного типа утверждения партнера. |

Элемент **Protocol** содержит следующие атрибуты.

| Атрибут | Обязательно для заполнения | Описание |
| --------- | -------- | ----------- |
| Name | Да | Имя действительного протокола, поддерживаемого Azure AD B2C. Возможные значения:  OAuth1, OAuth2, SAML2, OpenIdConnect, WsFed или WsTrust. |
| PartnerClaimType | Да | Имя типа утверждения, которое будет использоваться. |

В следующем примере, когда Инфраструктура процедур идентификации взаимодействует с поставщиком удостоверений SAML2 или приложением проверяющей стороны, утверждение **surname** сопоставляется с `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname`, а утверждения OpenIdConnect и OAuth2 сопоставляются с `family_name`.

```XML
<ClaimType Id="surname">
  <DisplayName>Surname</DisplayName>
  <DataType>string</DataType>
  <DefaultPartnerClaimTypes>
    <Protocol Name="OAuth2" PartnerClaimType="family_name" />
    <Protocol Name="OpenIdConnect" PartnerClaimType="family_name" />
    <Protocol Name="SAML2" PartnerClaimType="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" />
  </DefaultPartnerClaimTypes>
</ClaimType>
```

В результате токен JWT, выданный службой Azure AD B2C, вместо имени ClaimType **surname** порождает `family_name`.

```JSON
{
  "sub": "6fbbd70d-262b-4b50-804c-257ae1706ef2",
  "auth_time": 1535013501,
  "given_name": "David",
  "family_name": "Williams",
  "name": "David Williams",
}
```

### <a name="mask"></a>Mask

Элемент **Mask** содержит следующие атрибуты.

| Атрибут | Обязательно для заполнения | Описание |
| --------- | -------- | ----------- |
| `Type` | Да | Тип маски утверждения. Возможные значения: `Simple` или `Regex`. Значение `Simple` указывает, что простая текстовая маска применяется к начальной части строкового утверждения. Значение `Regex` указывает, что регулярное выражение применяется к строковому утверждению целиком.  Если указано значение `Regex`, необязательный атрибут также должен быть определен с используемым регулярным выражением. |
| `Regex` | Нет | Если **`Type`** параметр имеет `Regex`значение, укажите регулярное выражение для использования.

В следующем примере настраивается утверждение **PhoneNumber** с маской `Simple`:

```XML
<ClaimType Id="PhoneNumber">
  <DisplayName>Phone Number</DisplayName>
  <DataType>string</DataType>
  <Mask Type="Simple">XXX-XXX-</Mask>
  <UserHelpText>Your telephone number.</UserHelpText>
</ClaimType>
```

Инфраструктура процедур идентификации отображает номер телефона, скрывая первые шесть цифр:

![Утверждение номера телефона отображается в браузере с первыми шестью цифрами, маскированными по XS](./media/claimsschema/mask.png)

В следующем примере настраивается утверждение **AlternateEmail** с маской `Regex`:

```XML
<ClaimType Id="AlternateEmail">
  <DisplayName>Please verify the secondary email linked to your account</DisplayName>
  <DataType>string</DataType>
  <Mask Type="Regex" Regex="(?&lt;=.).(?=.*@)">*</Mask>
  <UserInputType>Readonly</UserInputType>
</ClaimType>
```

Инфраструктура процедур идентификации отображает только первую букву адреса и имя домена электронной почты:

![Утверждение сообщения электронной почты отображается в браузере с символами, маскированными звездочками](./media/claimsschema/mask-regex.png)


### <a name="restriction"></a>Ограничение

Элемент **Restriction** может содержать следующий атрибут.

| Атрибут | Обязательно для заполнения | Описание |
| --------- | -------- | ----------- |
| MergeBehavior | Нет | Метод, используемый для объединения значений перечисления с типом ClaimType в родительской политике с тем же идентификатором. Используйте этот атрибут, чтобы переопределить определенное в базовой политике утверждение. Возможные значения: `Append`, `Prepend` или `ReplaceAll`. Значение `Append` — это коллекция данных, которая должна добавляться в конец коллекции, указанной в родительской политике. Значение `Prepend` — это коллекция данных, которая должна добавляться перед коллекцией, указанной в родительской политике. Значение `ReplaceAll` представляет собой набор данных, указанных в родительской политике, которые следует игнорировать. |

Элемент **Restriction** содержит следующие элементы.

| Элемент | Вхождения | Описание |
| ------- | ----------- | ----------- |
| Перечисление. | 1:n | Варианты, которые пользователь может выбрать для утверждения в пользовательском интерфейсе, например значение в раскрывающемся списке. |
| Модель | 1:1 | Регулярное выражение для использования. |

### <a name="enumeration"></a>Перечисление.

Элемент **Enumeration** содержит следующие атрибуты.

| Атрибут | Обязательно для заполнения | Описание |
| --------- | -------- | ----------- |
| Текст | Да | Отображаемая строка, показываемая в пользовательском интерфейсе для этого параметра. |
|Значение | Да | Значение утверждения, связанное с этим вариантом. |
| SelectByDefault | Нет | Указывает, следует ли по умолчанию выбирать этот параметр в пользовательском интерфейсе. Возможные значения: Значение True или False. |

В следующем примере настраивается утверждение раскрывающегося списка **city** со значением по умолчанию `New York`:

```XML
<ClaimType Id="city">
  <DisplayName>city where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="New York" Value="new-york" SelectByDefault="true" />
  </Restriction>
</ClaimType>
```

Раскрывающийся список городов со значением по умолчанию — New York:

![Элемент управления dropdown, отображаемый в браузере и отображающий значение по умолчанию](./media/claimsschema/dropdownsingleselect.png)

### <a name="pattern"></a>Модель

Элемент **Pattern** может содержать следующие атрибуты.

| Атрибут | Обязательно для заполнения | Описание |
| --------- | -------- | ----------- |
| RegularExpression | Да | Регулярное выражение, которому должны соответствовать утверждения такого типа, чтобы быть допустимыми. |
| HelpText | Нет | Шаблон или регулярное выражение для этого утверждения. |

В следующем примере создается утверждение **email** с проверкой входных данных регулярного выражения и текстом справки:

```XML
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <DefaultPartnerClaimTypes>
    <Protocol Name="OpenIdConnect" PartnerClaimType="email" />
  </DefaultPartnerClaimTypes>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
  <Restriction>
    <Pattern RegularExpression="^[a-zA-Z0-9.!#$%&amp;'^_`{}~-]+@[a-zA-Z0-9-]+(?:\.[a-zA-Z0-9-]+)*$" HelpText="Please enter a valid email address." />
    </Restriction>
 </ClaimType>
```

Инфраструктура процедур идентификации визуализирует утверждение адреса электронной почты с помощью проверки входных данных формата электронной почты:

![Текстовое поле, отображающее сообщение об ошибке, активируемое ограничением Regex](./media/claimsschema/pattern.png)

## <a name="userinputtype"></a>UserInputType

Azure AD B2C поддерживает различные типы элементов для ввода данных пользователем, такие как текстовое поле, поле пароля и раскрывающийся список, которые можно использовать при вводе данных для типа утверждения вручную. Вам необходимо указать элемент **UserInputType**, когда вы собираете информацию от пользователя, используя [самоподтвержденный технический профиль](self-asserted-technical-profile.md).

### <a name="textbox"></a>TextBox

Тип элемента для ввода данных пользователем **TextBox** используется для предоставления однострочного текстового поля.

![Текстовое поле, отображающее свойства, указанные в типе утверждения](./media/claimsschema/textbox.png)

```XML
<ClaimType Id="displayName">
  <DisplayName>Display Name</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your display name.</UserHelpText>
  <UserInputType>TextBox</UserInputType>
</ClaimType>
```

### <a name="emailbox"></a>EmailBox

Тип элемента для ввода данных пользователем **EmailBox** используется для предоставления базового поля для ввода электронного адреса.

![Емаилбокс, отображающий свойства, указанные в типе утверждения](./media/claimsschema/emailbox.png)

```XML
<ClaimType Id="email">
  <DisplayName>Email Address</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Email address that can be used to contact you.</UserHelpText>
  <UserInputType>EmailBox</UserInputType>
  <Restriction>
    <Pattern RegularExpression="^[a-zA-Z0-9!#$%&amp;'+^_`{}~-]+(?:\.[a-zA-Z0-9!#$%&amp;'+^_`{}~-]+)*@(?:[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?\.)+[a-zA-Z0-9](?:[a-zA-Z0-9-]*[a-zA-Z0-9])?$" HelpText="Please enter a valid email address." />
  </Restriction>
</ClaimType>
```

### <a name="password"></a>Пароль

Тип элемента для ввода данных пользователем **Password** используется для записи пароля, введенного пользователем.

![Использование типа утверждения с элементом Password](./media/claimsschema/password.png)

```XML
<ClaimType Id="password">
  <DisplayName>Password</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Enter password</UserHelpText>
  <UserInputType>Password</UserInputType>
</ClaimType>
```

### <a name="datetimedropdown"></a>DateTimeDropdown

Тип элемента для ввода данных пользователем **DateTimeDropdown** используется для предоставления набора раскрывающихся списков для выбора дня, месяца и года. С помощью элементов Predicates и PredicateValidations вы можете управлять минимальным и максимальным значениями даты. Дополнительные сведения см. в разделе **Настройка диапазона дат** в статье [Элементы Predicates и PredicateValidations](predicates.md).

![Использование типа утверждения с элементом DateTimeDropdown](./media/claimsschema/datetimedropdown.png)

```XML
<ClaimType Id="dateOfBirth">
  <DisplayName>Date Of Birth</DisplayName>
  <DataType>date</DataType>
  <UserHelpText>The date on which you were born.</UserHelpText>
  <UserInputType>DateTimeDropdown</UserInputType>
</ClaimType>
```

### <a name="radiosingleselect"></a>RadioSingleSelect

Тип элемента для ввода данных пользователем **RadioSingleSelect** используется для предоставления коллекции переключателей, позволяющей пользователю выбрать один из вариантов.

![Использование типа утверждения с элементом RadioSingleSelect](./media/claimsschema/radiosingleselect.png)

```XML
<ClaimType Id="color">
  <DisplayName>Preferred color</DisplayName>
  <DataType>string</DataType>
  <UserInputType>RadioSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Blue" Value="Blue" SelectByDefault="false" />
    <Enumeration Text="Green " Value="Green" SelectByDefault="false" />
    <Enumeration Text="Orange" Value="Orange" SelectByDefault="true" />
  </Restriction>
</ClaimType>
```

### <a name="dropdownsingleselect"></a>DropdownSingleSelect

Тип элемента для ввода данных пользователем **DropdownSingleSelect** используется для предоставления раскрывающегося списка, позволяющего пользователю выбрать один из вариантов.

![Использование типа утверждения с элементом DropdownSingleSelect](./media/claimsschema/dropdownsingleselect.png)

```XML
<ClaimType Id="city">
  <DisplayName>City where you work</DisplayName>
  <DataType>string</DataType>
  <UserInputType>DropdownSingleSelect</UserInputType>
  <Restriction>
    <Enumeration Text="Bellevue" Value="bellevue" SelectByDefault="false" />
    <Enumeration Text="Redmond" Value="redmond" SelectByDefault="false" />
    <Enumeration Text="New York" Value="new-york" SelectByDefault="true" />
  </Restriction>
</ClaimType>
```

### <a name="checkboxmultiselect"></a>CheckboxMultiSelect

Тип элемента для ввода данных пользователем **CheckboxMultiSelect** используется для предоставления коллекции флажков, позволяющей пользователю выбрать несколько вариантов.

![Использование типа утверждения с элементом CheckboxMultiSelect](./media/claimsschema/checkboxmultiselect.png)

```XML
<ClaimType Id="languages">
  <DisplayName>Languages you speak</DisplayName>
  <DataType>string</DataType>
  <UserInputType>CheckboxMultiSelect</UserInputType>
  <Restriction>
    <Enumeration Text="English" Value="English" SelectByDefault="true" />
    <Enumeration Text="France " Value="France" SelectByDefault="false" />
    <Enumeration Text="Spanish" Value="Spanish" SelectByDefault="false" />
  </Restriction>
</ClaimType>
```

### <a name="readonly"></a>readOnly

Тип элемента для ввода данных пользователем **Readonly** предоставляет поле только для чтения для отображения утверждения и значения.

![Использование типа утверждения с элементом Readonly](./media/claimsschema/readonly.png)

```XML
<ClaimType Id="membershipNumber">
  <DisplayName>Membership number</DisplayName>
  <DataType>string</DataType>
  <UserHelpText>Your membership number (read only)</UserHelpText>
  <UserInputType>Readonly</UserInputType>
</ClaimType>
```


### <a name="paragraph"></a>Paragraph

Тип элемента для ввода данных пользователем **Paragraph** используется для предоставления поля, которое отображает текст только в теге параграфа. Например, &lt;p&gt;текст&lt;/p&gt;.

![Использование типа утверждения с элементом Paragraph](./media/claimsschema/paragraph.png)

```XML
<ClaimType Id="responseMsg">
  <DisplayName>Error message: </DisplayName>
  <DataType>string</DataType>
  <AdminHelpText>A claim responsible for holding response messages to send to the relying party</AdminHelpText>
  <UserHelpText>A claim responsible for holding response messages to send to the relying party</UserHelpText>
  <UserInputType>Paragraph</UserInputType>
  <Restriction>
    <Enumeration Text="B2C_V1_90001" Value="You cant sign in because you are a minor" />
    <Enumeration Text="B2C_V1_90002" Value="This action can only be performed by gold members" />
    <Enumeration Text="B2C_V1_90003" Value="You have not been enabled for this operation" />
  </Restriction>
</ClaimType>
```

Чтобы отобразить одно из значений элемента **Enumeration** в утверждении **responseMsg**, используйте преобразование утверждений `GetMappedValueFromLocalizedCollection` или `CreateStringClaim`. Дополнительные сведения см. в разделе о [преобразовании строковых утверждений](string-transformations.md).
