---
title: Расширенные сценарии с Azure MFA и VPN сторонних — Azure Active Directory
description: Пошаговые руководства по настройке Azure MFA для интеграции с устройствами Cisco, Citrix и Juniper.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2ad2b3075ae9d5ccd7e32f039fbbbc8583cde73c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055955"
---
# <a name="advanced-scenarios-with-azure-multi-factor-authentication-and-third-party-vpn-solutions"></a>Расширенные сценарии с использованием Многофакторной идентификации Azure и VPN-решений сторонних поставщиков

Многофакторную идентификацию Azure можно использовать для быстрого подключения к широкому спектру VPN-решений сторонних поставщиков. Эта статья посвящена применению устройств VPN Cisco® ASA, Citrix NetScaler SSL, а также Juniper Networks Secure Access/Pulse Secure Connect Secure SSL. Мы разработали руководства по настройке для этих трех распространенных устройств. Сервер Многофакторной идентификации также можно интегрировать с большинством других систем, которые используют для доступа в AD FS технологии RADIUS, LDAP, IIS или проверку подлинности на основе утверждений. Дополнительные сведения см. в разделе [Дополнительные конфигурации сервера Azure Multi-Factor Authentication](howto-mfaserver-deploy.md#next-steps).

> [!IMPORTANT]
> Начиная с 1 июля 2019 г. Корпорация Майкрософт больше не предоставляет многофакторной проверки Подлинности сервера для новых развертываний. Новых клиентов, которые хотите требовать многофакторную проверку подлинности от пользователей, их следует использовать многофакторную идентификацию Azure на основе облака. Существующие клиенты, которые активировали сервера MFA до 1 июля будет иметь возможность загрузить последнюю версию, будущие обновления и создать учетные данные активации обычным образом.

## <a name="cisco-asa-vpn-appliance-and-azure-multi-factor-authentication"></a>Устройство VPN Cisco ASA и Многофакторная идентификация Azure
Многофакторная идентификация Azure интегрируется с устройством VPN Cisco® ASA, обеспечивая дополнительную защиту учетных данных VPN Cisco AnyConnect® и доступа к порталу.  Вы можете использовать протокол LDAP или RADIUS.  Щелкните одну из следующих ссылок и загрузите подробную пошаговую инструкцию по настройке.

| Руководство по настройке | Описание |
| --- | --- |
| [Настройка Cisco ASA с конфигурацией Anyconnect VPN и многофакторной проверки подлинности Azure для LDAP](https://download.microsoft.com/download/A/2/0/A201567C-C3DE-4227-AF89-4567A470899E/Cisco_ASA_Azure_MFA_LDAP.docx) | Интеграция устройства VPN Cisco ASA с Azure MFA с помощью LDAP |
| [Настройка Cisco ASA с конфигурацией Anyconnect VPN и многофакторной проверки подлинности Azure для RADIUS](https://download.microsoft.com/download/4/5/7/4579C1CF-35B0-4FBE-8A1A-B49CB2CC0382/Cisco_ASA_Azure_MFA_RADIUS.docx) | Интеграция устройства VPN Cisco ASA с Azure MFA с помощью RADIUS |

## <a name="citrix-netscaler-ssl-vpn-and-azure-multi-factor-authentication"></a>VPN Citrix NetScaler SSL и Многофакторная идентификация Azure
Многофакторная идентификация Azure интегрируется с устройством VPN Citrix NetScaler SSL, обеспечивая дополнительную защиту учетных данных VPN Citrix NetScaler SSL и доступа к порталу.  Вы можете использовать протокол LDAP или RADIUS.  Щелкните одну из следующих ссылок и загрузите подробную пошаговую инструкцию по настройке.

| Руководство по настройке | Описание |
| --- | --- |
| [Настройка VPN Citrix NetScaler SSL и многофакторной проверки подлинности Azure для LDAP](https://download.microsoft.com/download/2/4/E/24E1E722-72DF-471F-A88A-D1338DB1AF83/Citrix_NS_Azure_MFA_LDAP.docx) | Интеграция устройства VPN Citrix NetScaler SSL с Azure MFA с помощью LDAP |
| [Настройка VPN Citrix NetScaler SSL и многофакторной проверки подлинности Azure для RADIUS](https://download.microsoft.com/download/1/A/4/1A482764-4A63-45C2-A5EC-2B673ACCDD12/Citrix_NS_Azure_MFA_RADIUS.docx) | Интеграция устройства VPN Citrix NetScaler SSL с Azure MFA с помощью RADIUS |

## <a name="juniperpulse-secure-ssl-vpn-appliance-and-azure-multi-factor-authentication"></a>Устройство VPN Juniper/Pulse Secure SSL и Многофакторная идентификация Azure
Служба Многофакторной идентификации Azure интегрируется с устройством VPN Juniper/Pulse Secure SSL, обеспечивая дополнительную защиту учетных данных VPN Juniper/Pulse Secure SSL и доступа к порталу.  Вы можете использовать протокол LDAP или RADIUS.  Щелкните одну из следующих ссылок и загрузите подробную пошаговую инструкцию по настройке.

| Руководство по настройке | Описание |
| --- | --- |
| [Настройка VPN Juniper/Pulse Secure SSL и многофакторной проверки подлинности Azure для LDAP](https://download.microsoft.com/download/6/5/8/6587B418-75B1-4FCB-84D4-984BC479309E/JuniperPulse_Azure_MFA_LDAP.docx) | Интеграция устройства VPN Juniper/Pulse Secure SSL с Azure MFA с помощью LDAP |
| [Настройка VPN Juniper/Pulse Secure SSL и многофакторной проверки подлинности Azure для RADIUS](https://download.microsoft.com/download/7/9/A/79AB3DAD-4799-4379-B1DA-B95ABDF231DC/JuniperPulse_Azure_MFA_RADIUS.docx) | Интеграция устройства VPN Juniper/Pulse Secure SSL с Azure MFA с помощью RADIUS |

## <a name="next-steps"></a>Дальнейшие действия

- [Внедрение расширения NPS для Многофакторной идентификации Azure в существующую инфраструктуру проверки подлинности.](howto-mfa-nps-extension.md)

- [Настройка параметров Многофакторной идентификации Azure](howto-mfa-mfasettings.md)
