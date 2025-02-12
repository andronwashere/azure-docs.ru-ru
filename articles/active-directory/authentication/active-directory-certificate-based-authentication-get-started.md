---
title: Начало работы с проверкой подлинности на основе сертификатов — Azure Active Directory
description: Узнайте, как настроить в своей среде аутентификацию на основе сертификата.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 01/15/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: annaba
ms.collection: M365-identity-device-management
ms.openlocfilehash: f57d4615fc80df6c5df9ba295288ad71ae12fa23
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60359081"
---
# <a name="get-started-with-certificate-based-authentication-in-azure-active-directory"></a>Приступая к работе с аутентификацией на основе сертификата в Azure Active Directory

Аутентификация на основе сертификата позволяет Azure Active Directory выполнять аутентификацию с помощью сертификата клиента на устройстве Windows, Android или iOS при подключении учетной записи Exchange Online к:

- мобильным приложениям Microsoft, таким как Microsoft Outlook и Microsoft Word;
- клиентам Exchange ActiveSync (EAS).

Настройка данной функции избавляет от необходимости ввода имени пользователя и пароля в определенных почтовых клиентах и приложениях Microsoft Office на мобильных устройствах.

В этой статье:

- Показано, как настроить и использовать аутентификацию на основе сертификата для пользователей клиентов в тарифных планах Office 365 корпоративный, бизнес, для образования и для государственных организаций США. В тарифных планах Office 365 China, US Government Defense и US Government Federal доступна предварительная версия этой функции.
- Предполагается, что у вас уже настроены [инфраструктура открытых ключей (PKI)](https://go.microsoft.com/fwlink/?linkid=841737) и [AD FS](../hybrid/how-to-connect-fed-whatis.md).

## <a name="requirements"></a>Требования

Для настройки аутентификации на основе сертификата должны выполняться следующие условия:

- Аутентификация на основе сертификата (CBA) поддерживается только для браузерных приложений и собственных клиентов в федеративных средах, использующих современную аутентификацию (ADAL). Единственным исключением является решение Exchange Active (EAS) для Exchange Online (EXO), которое можно использовать для федеративных и управляемых учетных записей.
- Корневой центр сертификации и все промежуточные центры сертификации должны быть настроены в Azure Active Directory.
- У каждого центра сертификации должен быть список отзыва сертификатов (CRL), на который можно сослаться с помощью URL-адреса для Интернета.
- В Azure Active Directory должен быть настроен хотя бы один центр сертификации. Соответствующие действия описаны в разделе [Настройка центров сертификации](#step-2-configure-the-certificate-authorities).
- Для клиентов Exchange ActiveSync: в сертификате клиента в поле "Альтернативное имя субъекта" в качестве значения имени субъекта или имени RFC822 должен быть указан маршрутизируемый адрес электронной почты пользователя в Exchange Online. Azure Active Directory сопоставляет значение RFC822 с атрибутом прокси-адреса в каталоге.
- Устройство клиента должно иметь доступ хотя бы к одному центру сертификации, выдающему сертификаты клиента.
- Для аутентификации вашего клиента должен быть выдан сертификат клиента.

## <a name="step-1-select-your-device-platform"></a>Шаг 1. Выбор платформы устройства

При выборе платформы устройства для начала необходимо ознакомиться со следующими сведениями:

- Поддержка мобильных приложений Office
- Особые требования к реализации

Эта информация доступна для следующих платформ устройств:

- [Android](active-directory-certificate-based-authentication-android.md)
- [iOS](active-directory-certificate-based-authentication-ios.md)

## <a name="step-2-configure-the-certificate-authorities"></a>Шаг 2. Настройка центров сертификации

Чтобы настроить в Azure Active Directory центры сертификации, для каждого центра сертификации отправьте следующие данные:

* открытую часть сертификата в формате *CER* ;
* URL-адреса для Интернета, по которым находятся списки отзыва сертификатов (CRL).

Схема для центра сертификации выглядит следующим образом:

    class TrustedCAsForPasswordlessAuth
    {
       CertificateAuthorityInformation[] certificateAuthorities;
    }

    class CertificateAuthorityInformation

    {
        CertAuthorityType authorityType;
        X509Certificate trustedCertificate;
        string crlDistributionPoint;
        string deltaCrlDistributionPoint;
        string trustedIssuer;
        string trustedIssuerSKI;
    }

    enum CertAuthorityType
    {
        RootAuthority = 0,
        IntermediateAuthority = 1
    }

Для настройки можно использовать [Azure Active Directory PowerShell версии 2](/powershell/azure/install-adv2?view=azureadps-2.0):

1. Запустите Windows PowerShell с правами администратора.
2. Установите модуль Azure AD версии [2.0.0.33](https://www.powershellgallery.com/packages/AzureAD/2.0.0.33) или более поздней.

        Install-Module -Name AzureAD –RequiredVersion 2.0.0.33

В качестве первого шага настройки необходимо установить подключение к клиенту. Когда будет установлено подключение к клиенту, вы сможете просмотреть, добавить, удалить или изменить доверенные центры сертификации, определенные в каталоге.

### <a name="connect"></a>Подключение

Чтобы установить подключение к клиенту, используйте командлет [Connect-AzureAD](/powershell/module/azuread/connect-azuread?view=azureadps-2.0):

    Connect-AzureAD

### <a name="retrieve"></a>Получение

Чтобы получить доверенные центры сертификации, определенные в каталоге, используйте командлет [Get-AzureADTrustedCertificateAuthority](/powershell/module/azuread/get-azureadtrustedcertificateauthority?view=azureadps-2.0):

    Get-AzureADTrustedCertificateAuthority

### <a name="add"></a>Добавить

Чтобы создать доверенный центр сертификации, используйте командлет [New-AzureADTrustedCertificateAuthority](/powershell/module/azuread/new-azureadtrustedcertificateauthority?view=azureadps-2.0) и задайте правильное значение атрибута **crlDistributionPoint**.

    $cert=Get-Content -Encoding byte "[LOCATION OF THE CER FILE]"
    $new_ca=New-Object -TypeName Microsoft.Open.AzureAD.Model.CertificateAuthorityInformation
    $new_ca.AuthorityType=0
    $new_ca.TrustedCertificate=$cert
    $new_ca.crlDistributionPoint="<CRL Distribution URL>"
    New-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $new_ca

### <a name="remove"></a>Удалить

Чтобы удалить доверенный центр сертификации, используйте командлет [Remove-AzureADTrustedCertificateAuthority](/powershell/module/azuread/remove-azureadtrustedcertificateauthority?view=azureadps-2.0):

    $c=Get-AzureADTrustedCertificateAuthority
    Remove-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[2]

### <a name="modify"></a>Изменить

Чтобы изменить доверенный центр сертификации, используйте командлет [Set-AzureADTrustedCertificateAuthority](/powershell/module/azuread/set-azureadtrustedcertificateauthority?view=azureadps-2.0):

    $c=Get-AzureADTrustedCertificateAuthority
    $c[0].AuthorityType=1
    Set-AzureADTrustedCertificateAuthority -CertificateAuthorityInformation $c[0]

## <a name="step-3-configure-revocation"></a>Шаг 3. Настройка отзыва

Чтобы отозвать сертификат клиента, Azure Active Directory извлекает список отзыва сертификатов (CRL) из URL-адресов, переданных вместе с информацией центра сертификации, и кэширует его. Метка времени последней публикации (свойство**Дата вступления в силу** ) в списке отзыва сертификатов обеспечивает допустимость этого списка. Список отзыва сертификатов периодически опрашивается для отзыва доступа к сертификатам, которые числятся в этом списке.

Если требуется более быстрый отзыв (например, если пользователь потерял устройство), то маркер авторизации пользователя можно сделать недействительным. Чтобы сделать маркер авторизации недействительным, с помощью Windows PowerShell определите поле **StsRefreshTokenValidFrom** для этого пользователя. Поле **StsRefreshTokenValidFrom** необходимо обновить для каждого пользователя, доступ для которого будет отозван.

Чтобы отзыв оставался в силе, для свойства **Дата вступления в силу** списка отзыва сертификатов необходимо указать дату, которая наступит после даты, заданной в поле **StsRefreshTokenValidFrom**, а также убедиться, что этот сертификат есть в списке отзыва сертификатов.

Ниже описан процесс обновления и аннулирования маркера авторизации с помощью поля **StsRefreshTokenValidFrom** .

**Чтобы настроить отзыв сертификата, выполните следующие действия:**

1. Используя учетные данные администратора, подключитесь к службе MSOL:

        $msolcred = get-credential
        connect-msolservice -credential $msolcred

2. Получите текущее значение StsRefreshTokensValidFrom для пользователя:

        $user = Get-MsolUser -UserPrincipalName test@yourdomain.com`
        $user.StsRefreshTokensValidFrom

3. Настройте новое значение StsRefreshTokensValidFrom для пользователя, равное текущей метке времени:

        Set-MsolUser -UserPrincipalName test@yourdomain.com -StsRefreshTokensValidFrom ("03/05/2016")

Задаваемая дата должна быть в будущем. Если дата не в будущем, свойство **StsRefreshTokensValidFrom** не будет задано. Если дата в будущем, для **StsRefreshTokensValidFrom** задается актуальное время (не дата, указанная командой Set-MsolUser).

## <a name="step-4-test-your-configuration"></a>Шаг 4. Тестирование конфигурации

### <a name="testing-your-certificate"></a>Тестирование сертификата

В качестве первой проверки конфигурации попытайтесь войти в [Outlook Web Access](https://outlook.office365.com) или [SharePoint Online](https://microsoft.sharepoint.com), используя **браузер на устройстве**.

Успешный вход подтверждает, что:

- для тестируемого устройства подготовлен сертификат пользователя;
- службы AD FS настроены правильно.

### <a name="testing-office-mobile-applications"></a>Тестирование мобильных приложений Office

**Чтобы протестировать аутентификацию на основе сертификата в мобильном приложении Office, выполните следующие действия:**

1. На тестируемом устройстве установите мобильное приложение Office (например, OneDrive).
3. Запустите приложение.
4. Введите свое имя пользователя, а затем выберите сертификат пользователя, который хотите использовать.

Вы должны без проблем войти в систему.

### <a name="testing-exchange-activesync-client-applications"></a>Тестирование клиентских приложений Exchange ActiveSync

Для доступа к Exchange ActiveSync (EAS) с использованием аутентификации на основе сертификата приложению должен быть доступен профиль EAS, содержащий сертификат клиента.

В профиле EAS должны содержаться следующие сведения:

- сертификат пользователя, который будет использоваться для аутентификации;

- конечная точка EAS (например, outlook.office365.com).

Профиль EAS можно настроить и поместить на устройство с помощью системы управления мобильными устройствами (MDM), такой как Intune, либо вручную поместить сертификат в профиль EAS на устройстве.

### <a name="testing-eas-client-applications-on-android"></a>Тестирование клиентских приложений EAS на Android

**Чтобы протестировать аутентификацию на основе сертификата, выполните следующие действия:**

1. Настройте профиль EAS в приложении, удовлетворяющем требованиям, изложенным в предыдущем разделе.
2. Откройте приложение и убедитесь, что почта синхронизируется.

## <a name="next-steps"></a>Дальнейшие действия

[Дополнительные сведения о проверке подлинности на основе сертификата на устройствах Android](active-directory-certificate-based-authentication-android.md).

[Дополнительные сведения о проверке подлинности на основе сертификата на устройствах iOS](active-directory-certificate-based-authentication-ios.md).
