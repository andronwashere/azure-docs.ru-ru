---
title: Рекомендации по безопасности для службы приложений Azure
description: Рекомендации по безопасности для службы приложений Azure. Реализации этих рекомендаций справки выполнения при выполнении обязательств, безопасности, как описано в нашей модели разделения ответственности и улучшит общий уровень безопасности для приложений свои веб-решения.
services: app-service
author: barclayn
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 06/17/2019
ms.author: barclayn
ms.custom: security-recommendations
ms.openlocfilehash: 53cd2b1dde1158a1c46f798e57911dad110dc87e
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718266"
---
# <a name="security-recommendations-for-app-service"></a>Рекомендации по безопасности для службы приложений

Эта статья содержит рекомендации по безопасности для службы приложений Azure. Реализации этих рекомендаций справки выполнения при выполнении обязательств, безопасности, как описано в нашей модели разделения ответственности и улучшит общий уровень безопасности для решений веб-приложения. Дополнительные сведения о какие Microsoft делает для выполнения обязанностей поставщик службы, о [безопасности инфраструктуры Azure](../security/azure-security-infrastructure.md).

## <a name="general"></a>Общие сведения

| Рекомендации | Комментарии |
|-|-|----|
| Последние новости | Используйте последние версии поддерживаемых платформ, языков программирования, протоколы и платформ. |

## <a name="identity-and-access-management"></a>Управление удостоверениями и доступом

| Рекомендации | Комментарии |
|-|----|
| Отключение анонимного доступа | Если вам не требуются для поддержки анонимных запросов, отключение анонимного доступа. Дополнительные сведения о параметрах проверки подлинности службы приложений Azure, см. в разделе [проверки подлинности и авторизации в службе приложений Azure](overview-authentication-authorization.md).|
| Требовать проверку подлинности | По возможности используйте модуль проверки подлинности службы приложений вместо написания кода для обработки проверки подлинности и авторизации. См. в разделе [проверки подлинности и авторизации в службе приложений Azure](overview-authentication-authorization.md). |
| Защита ресурсов серверной части с помощью доступа с проверкой подлинности | Можно использовать удостоверение пользователя или использовать удостоверение приложения для проверки подлинности к внутреннему ресурсу. Если вы решили использовать удостоверение приложения [управляемого удостоверения](overview-managed-identity.md).
| Требуется проверка подлинности сертификата клиента | Проверка подлинности сертификата клиента повышает безопасность, разрешая подключения только от клиентов, которые могут проверять подлинность с помощью сертификатов, которые вы указываете. |

## <a name="data-protection"></a>Защита данных

| Рекомендации | Комментарии |
|-|-|
| Перенаправление трафика HTTP в HTTPs | По умолчанию клиенты могут подключаться к веб-приложений по протоколу HTTP или HTTPS. Мы рекомендуем перенаправление HTTP на HTTPs, так как HTTPS по протоколу SSL/TLS для обеспечения безопасного соединения, которая является одновременно шифрование и проверка подлинности. |
| Шифровать подключения к ресурсам Azure | Когда приложение подключается к ресурсам Azure, такие как [базы данных SQL](https://azure.microsoft.com/services/sql-database/) или [хранилища Azure](/azure/storage/), подключение остается в Azure. Так как подключение проходит через общего сетевого подключения в Azure, всегда следует зашифровать весь обмен данными. |
| Требования самой последней версии TLS | Так как новые приложения службы приложений Azure 2018 г. использование TLS 1.2. Более новые версии TLS, включают усовершенствования системы безопасности более старой версии протокола. |
| Использовать FTPS | Служба приложений поддерживает протоколы FTP и FTPS для развертывания файлов. Используйте FTPS вместо FTP, если это возможно. Если один из этих протоколов или оба протокола не используются, нужно [их отключить](deploy-ftp.md#enforce-ftps). |
| Защита данных приложения | Не храните секретные данные приложения, например учетные данные базы данных, токены API или закрытые ключи в файлах кода или конфигурации. Общепринятым подходом является доступ к ним как [переменным среды](https://wikipedia.org/wiki/Environment_variable) с использованием стандартного шаблона на выбранном вами языке. В службе приложений Azure, можно определить переменные среды посредством [параметры приложения](web-sites-configure.md) и [строки подключения](web-sites-configure.md). Параметры приложения и строки подключения хранятся в Azure. Параметры приложения, расшифровываются только до, введенного в памяти процесса вашего приложения при запуске приложения. Ключи шифрования регулярно меняются. Кроме того, вы можете интегрировать приложения службы приложений Azure с помощью [Azure Key Vault](/azure/key-vault/) для управления дополнительных секретов. При получении [доступа к Key Vault с помощью управляемого удостоверения](../key-vault/tutorial-web-application-keyvault.md) приложение службы приложений может безопасно получить доступ к нужным секретам. |

## <a name="networking"></a>Сеть

| Рекомендации | Комментарии |
|-|-|
| Использовать ограничения статических IP-адресов | Служба приложений Azure в Windows позволяет определить список IP-адресов, которые могут получить доступ к приложению. Список разрешений может содержать отдельные IP-адреса или их диапазон, который определяется маской подсети. Дополнительные сведения см. в статье [Ограничения статических IP-адресов в Службе приложений Azure](app-service-ip-restrictions.md).  |
| Использование изолированного ценовой категории | За исключением изолированной ценовую категорию на всех уровнях запускать приложения в общей сетевой инфраструктуре в службе приложений Azure. Изолированном уровне дает полный сетевой изоляции запуская приложения внутри специальной [среды службы приложений](environment/intro.md). Среда службы приложений запускается в вашем экземпляре [виртуальной сети Azure](/azure/virtual-network/).|
| Использование безопасных подключений при доступе к локальным ресурсам | Можно использовать [гибридных подключений](app-service-hybrid-connections.md), [Интеграция виртуальной сети](web-sites-integrate-with-vnet.md), или [среды службы приложений](environment/intro.md) для подключения к локальным ресурсам. |
| Снизить уязвимость для входящего сетевого трафика | Группы безопасности сети позволяют ограничить сетевой доступ и контролировать число предоставленных конечных точек. Дополнительные сведения см. в разделе [как к управлению входящим трафиком в среде службы приложений](environment/app-service-app-service-environment-control-inbound-traffic.md). |

## <a name="monitoring"></a>Мониторинг

| Рекомендации | Комментарии |
|-|-|
|В центре безопасности Azure уровня "стандартный" | [Центр безопасности Azure](../security-center/security-center-app-services.md) изначально интегрирован со службой приложений Azure. Его можно выполнять оценку и предоставляет рекомендации по безопасности. |

## <a name="next-steps"></a>Следующие шаги

Обратитесь к поставщику приложения, чтобы узнать, есть дополнительные требования к безопасности. Дополнительные сведения о разработке безопасных приложений, см. в разделе [безопасного документации по разработке приложений](../security/abstract-develop-secure-apps.md).
