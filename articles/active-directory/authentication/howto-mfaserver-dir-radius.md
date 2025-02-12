---
title: Проверка подлинности RADIUS и сервер Azure MFA — Azure Active Directory
description: Развертывание аутентификации RADIUS на сервере Многофакторной идентификации Azure.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/31/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 17e040492b1d986215aeb77ea14ebff53946f78e
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67056018"
---
# <a name="integrate-radius-authentication-with-azure-multi-factor-authentication-server"></a>Интеграция аутентификации RADIUS с сервером Многофакторной идентификации Azure

RADIUS — это стандартный протокол принятия и обработки запросов проверки подлинности. Сервер Многофакторной идентификации Azure может выполнять роль сервер RADIUS. Установите его между клиентом RADIUS (например, VPN-устройством) и целевым объектом проверки подлинности, чтобы добавить двухфакторную проверку подлинности. Целевым объектом проверки подлинности может быть каталог Active Directory, каталог LDAP или другой сервер RADIUS. Для нормального функционирования Многофакторной идентификации Azure (MFA) необходимо настроить сервер Azure MFA так, чтобы он мог взаимодействовать как с клиентскими серверами, таки и с целевым объектом аутентификации. Сервер Azure MFA принимает запросы от клиента RADIUS, проверяет учетные данные на целевом объекте проверки подлинности, добавляет Многофакторную идентификацию Azure и отправляет ответ клиенту RADIUS. Запрос на проверку подлинности будет успешно выполнен только в случае успешного прохождения и основной проверки подлинности, и Многофакторной идентификации Azure.

> [!IMPORTANT]
> Начиная с 1 июля 2019 г. Корпорация Майкрософт больше не предоставляет многофакторной проверки Подлинности сервера для новых развертываний. Новых клиентов, которые хотите требовать многофакторную проверку подлинности от пользователей, их следует использовать многофакторную идентификацию Azure на основе облака. Существующие клиенты, которые активировали сервера MFA до 1 июля будет иметь возможность загрузить последнюю версию, будущие обновления и создать учетные данные активации обычным образом.

> [!NOTE]
> Выполняя роль RADIUS, сервер MFA поддерживает только такие протоколы RADIUS: PAP (протокол проверки пароля) и MSCHAPv2 (протокол проверки пароля Майкрософт).  Другие протоколы, такие как EAP (расширяемый протокол аутентификации), можно использовать, если сервер MFA выполняет роль прокси-сервера RADIUS для другого сервера RADIUS, который поддерживает этот протокол.
>
> При такой конфигурации односторонние SMS и OATH-токены не будут работать, так как сервер MFA не сможет инициировать успешный ответ на запрос RADIUS с использованием альтернативных протоколов.

![Проверка подлинности RADIUS на сервере MFA](./media/howto-mfaserver-dir-radius/radius.png)

## <a name="add-a-radius-client"></a>Добавление клиента RADIUS

Для настройки проверки подлинности RADIUS установите сервер Многофакторной идентификации Azure на сервере Windows. Если имеется среда Active Directory, сервер должен быть присоединен к домену в сети. Для настройки сервера Многофакторной идентификации Azure используйте следующую процедуру.

1. На сервере Многофакторной идентификации Azure щелкните значок аутентификации RADIUS в левом меню.
2. Установите флажок **Включить проверку подлинности RADIUS**.
3. На вкладке "Клиенты" измените порты для аутентификации и учетных данных, если службе Azure MFA RADIUS требуется ожидать передачи данных через нестандартные порты.
4. Щелкните **Добавить**.
5. Введите IP-адрес устройства или сервера, который будет проходить аутентификацию на сервере Многофакторной идентификации Azure, имя приложения (необязательно) и общий секрет.

   Имя приложения отображается в отчетах, а также может отображаться в SMS-сообщениях или сообщениях о проверке подлинности в мобильном приложении.

   Общий секрет должен быть одинаковым на сервере Многофакторной идентификации Azure и на устройстве или сервере.

6. Установите флажок **Требуется сопоставление пользователей многофакторной проверки подлинности**, если все пользователи, импортированные на сервер, подлежат многофакторной проверке подлинности. Если значительное число пользователей еще не импортировано на сервер или исключено из двухфакторной проверки подлинности, не устанавливайте флажок.
7. Установите флажок **Включить резервный OATH-токен**, чтобы использовать секретные коды OATH из мобильных приложений проверки в качестве резервной меры.
8. Последовательно выберите **ОК**.

Для добавления необходимого числа клиентов RADIUS повторите шаги с 4 по 8.

## <a name="configure-your-radius-client"></a>Настройка клиента RADIUS

1. Перейдите на вкладку **Целевой объект**.
   * Если сервер Azure MFA установлен на сервере, присоединенном к домену в среде Active Directory, выберите **домен Windows**.
   * Если пользователи должны проходить аутентификацию для каталога LDAP, выберите **привязку LDAP**.
      Щелкните значок интеграции каталогов и измените конфигурацию LDAP на вкладке "Параметры", чтобы можно было привязать сервер к каталогу. Инструкции по настройке LDAP можно найти в [руководстве по настройке прокси-сервера LDAP](howto-mfaserver-dir-ldap.md).
   * Если пользователи должны проходить аутентификацию в отношении другого сервера RADIUS, выберите **серверы RADIUS**.
1. Щелкните **Добавить**, чтобы настроить сервер, к которому сервер Azure MFA будет передавать запросы RADIUS.
1. В диалоговом окне добавления сервера RADIUS введите IP-адрес сервера RADIUS и общий секрет.

   Общий секрет должен быть одинаковым на сервере Многофакторной идентификации Azure и на сервере RADIUS. Измените порт проверки подлинности и порт учета, если сервер RADIUS используют другие порты.

1. Последовательно выберите **ОК**.
1. Добавьте сервер Azure MFA в качестве клиента RADIUS на другом сервере RADIUS, чтобы он мог обрабатывать запросы на доступ, отправляемые на него с сервера Azure MFA. Необходимо использовать тот же общий секрет, который настроен на сервере Многофакторной идентификации Azure.

Повторите эти шаги, чтобы добавить другие серверы RADIUS. Настройте порядок, в котором сервер Azure MFA должен их вызывать, с помощью кнопок перемещения **вверх** и **вниз**.

Вы успешно настроили сервер Многофакторной идентификации Microsoft Azure. Теперь сервер прослушивает в настроенных портах запросы на доступ RADIUS, поступающие из настроенных клиентов.

## <a name="radius-client-configuration"></a>Настройка клиента RADIUS

Для настройки клиента RADIUS используйте следующие рекомендации.

* Настройте устройство или сервер для проверки подлинности с помощью RADIUS по IP-адресу сервера Многофакторной идентификации Azure, который будет действовать как сервер RADIUS.
* Используйте тот же общий секрет, который был настроен ранее.
* Установите время ожидания RADIUS в диапазоне 30–60 секунд, чтобы его было достаточно для проверки учетных данных пользователя, двухфакторной проверки подлинности, получения ответа и отправки ответа на запрос на доступ к RADIUS.

## <a name="next-steps"></a>Дальнейшие действия

Узнайте, как [интегрировать проверку подлинности RADIUS](howto-mfa-nps-extension.md) со службой Многофакторной идентификации Azure в облаке. 
