---
title: Атрибуты безопасности для Azure Key Vault
description: Контрольный список атрибутов безопасности для оценки Azure Key Vault
services: key-vault
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 836d025c5bc69da9606c9a6172ac6a43caaaf29b
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444021"
---
# <a name="security-attributes-for-azure-key-vault"></a>Атрибуты безопасности для Azure Key Vault

В этой статье описываются атрибуты безопасности, встроенные в Azure Key Vault. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>профилактическая;

| Атрибут безопасности | Да или нет | Примечания |
|---|---|--|
| Шифрование неактивных (таких как шифрование на стороне сервера, шифрование на стороне сервера с помощью управляемых клиентом ключей и другие функции шифрования).| Да | Будут шифроваться все объекты. |
| Шифрование при передаче (например, шифрование ExpressRoute, Шифрование виртуальной сети и шифрование виртуальной сети)| Да | Вся связь осуществляется через зашифрованные вызовы API |
| Обработка ключа шифрования (CMK, BYOK и т. д.)| Да | Клиент контролирует все ключи в своих Key Vault. При указании ключей, защищенных аппаратным обеспечением (HSM), ключ, сертификат или секрет защищается HSM-модулем FIPS уровня 2. |
| Шифрование на уровне столбцов (службы данных Azure)| Н/Д |  |
| Вызовы API в зашифрованном виде| Да | С использованием HTTPS. |

## <a name="network-segmentation"></a>сегментация сети;

| Атрибут безопасности | Да или нет | Примечания |
|---|---|--|
| Поддержка конечных точек службы| Да | Использование конечных точек службы виртуальной сети (VNet). |
| Поддержка внедрения виртуальной сети| Нет |  |
| Поддержка сетевой изоляции и брандмауэров| Да | Использование правил брандмауэра виртуальной сети. |
| Поддержка принудительного туннелирования| Нет |  |

## <a name="detection"></a>Обнаружение

| Атрибут безопасности | Да или нет | Примечания|
|---|---|--|
| Поддержка мониторинга Azure (log Analytics, App Insights и т. д.)| Да | С использованием Log Analytics. |

## <a name="identity-and-access-management"></a>Управление удостоверениями и доступом

| Атрибут безопасности | Да или нет | Примечания|
|---|---|--|
| Проверка подлинности| Да | Аутентификация выполняется с помощью Azure Active Directory. |
| Authorization| Да | С использованием политики доступа к хранилищу Key Vault. |


## <a name="audit-trail"></a>журнал аудита;

| Атрибут безопасности | Да или нет | Примечания|
|---|---|--|
| Ведение журнала и аудит плоскости управления и контроля| Да | С использованием Log Analytics. |
| Ведение журнала и аудит в плоскости данных| Да | С использованием Log Analytics. |

## <a name="access-controls"></a>Элементы управления доступом

| Атрибут безопасности | Да или нет | Примечания|
|---|---|--|
| Элементы управления доступом в плоскости управления | Да | Управление доступом на основе ролей Azure Active Directory (RBAC) |
| Элементы управления доступом в плоскости данных (на каждом уровне обслуживания) | Да | Политика доступа к Key Vault |