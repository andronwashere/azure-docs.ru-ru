---
title: Переход с PhoneFactor на сервер Azure MFA. Azure Active Directory
description: Начало работы с сервером Многофакторной идентификации Azure и замена старой версии агента PhoneFactor.
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
ms.openlocfilehash: bcb1bf4996e381db933eac91135093a101e504bb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055977"
---
# <a name="upgrade-the-phonefactor-agent-to-azure-multi-factor-authentication-server"></a>Переход с агента PhoneFactor на сервер Многофакторной идентификации Azure

Чтобы перейти с агента PhoneFactor версии 5.x или более ранней версии на сервер Многофакторной идентификации Azure, сначала удалите агент PhoneFactor и связанные с ним компоненты. После этого можно установить сервер Многофакторной идентификации и его компоненты.

> [!IMPORTANT]
> Начиная с 1 июля 2019 г. Корпорация Майкрософт больше не предоставляет многофакторной проверки Подлинности сервера для новых развертываний. Новых клиентов, которые хотите требовать многофакторную проверку подлинности от пользователей, их следует использовать многофакторную идентификацию Azure на основе облака. Существующие клиенты, которые активировали сервера MFA до 1 июля будет иметь возможность загрузить последнюю версию, будущие обновления и создать учетные данные активации обычным образом.

## <a name="uninstall-the-phonefactor-agent"></a>Удаление агента PhoneFactor

1. Сначала создайте резервную копию файла данных PhoneFactor. По умолчанию файл хранится в расположении C:\Program Files\PhoneFactor\Data\Phonefactor.pfdata.

2. Если установлен пользовательский портал, сделайте вот что:
   1. Перейдите в папку установки и создайте резервную копию файла web.config. По умолчанию файл хранится в расположении C:\inetpub\wwwroot\PhoneFactor.

   2. Если вы добавили на портал настраиваемые темы, создайте резервную копию настраиваемой папки, которая хранится в каталоге C:\inetpub\wwwroot\PhoneFactor\App_Themes.

   3. Удалите пользовательский портал с помощью агента PhoneFactor (если портал установлен на том же сервере, что и агент PhoneFactor) или с помощью компонента «Программы и компоненты» в ОС Windows.

3. Если веб-служба мобильного приложения установлена:

   1. Перейдите в папку установки и создайте резервную копию файла web.config. По умолчанию файл хранится в расположении C:\inetpub\wwwroot\PhoneFactorPhoneAppWebService.

   2. Удалите веб-службу мобильного приложения через компонент "Программы и компоненты" в ОС Windows.

4. Если установлен пакет SDK для веб-службы, удалите его либо с помощью агента PhoneFactor, либо с помощью компонента «Программы и компоненты» в ОС Windows.

5. Удалите агент PhoneFactor с помощью компонента «Программы и компоненты» в ОС Windows.

## <a name="install-the-multi-factor-authentication-server"></a>Установка сервера Многофакторной идентификации

Путь установки берется из реестра предыдущей установки агента PhoneFactor, поэтому сервер должен установиться в то же расположение (например, C:\Program Files\PhoneFactor). В случае новой установки будет создан другой путь (например, C:\Program Files\Multi-Factor Authentication Server). Файл данных, оставшийся от агента PhoneFactor, будет обновлен во время установки, поэтому после установки нового сервера Многофакторной идентификации ваши пользователи и параметры будут в сохранности.

1. При появлении запроса активируйте сервер Многофакторной идентификации и убедитесь, что он назначен правильной группе репликации.

2. Если ранее был установлен пакет SDK для веб-службы, установите новый пакет SDK через пользовательский интерфейс сервера Многофакторной идентификации.

   Теперь у виртуального каталога по умолчанию будет имя **MultiFactorAuthWebServiceSdk**, а не **PhoneFactorWebServiceSdk**. Если вы хотите использовать предыдущее имя, измените имя виртуального каталога на этапе установки. Если во время установки не изменять имя каталога по умолчанию, вам придется изменить URL-адрес во всех приложениях, которые ссылаются на пакет SDK для веб-службы (например, пользовательский портал и веб-служба мобильного приложения). Новый URL-адрес должен ссылаться на правильное расположение.

3. Если пользовательский портал был ранее установлен на сервере с агентом PhoneFactor, установите новый пользовательский портал Многофакторной идентификации через пользовательский интерфейс сервера Многофакторной идентификации.

   Теперь у виртуального каталога по умолчанию будет имя **MultiFactorAuth**, а не **PhoneFactor**. Если вы хотите использовать предыдущее имя, измените имя виртуального каталога на этапе установки. Если во время установки не изменять имя каталога по умолчанию, щелкните значок пользовательского портала на сервере Многофакторной идентификации и измените URL-адрес пользовательского портала на вкладке "Параметры".

4. Если пользовательский портал или веб-служба мобильного приложения были ранее установлены на другом сервере (не на том, где установлен агент PhoneFactor):

   1. Перейдите в папку установки (например, C:\Program Files\PhoneFactor) и скопируйте один или несколько установщиков на другой сервер. Для пользовательского портала и веб-службы мобильного приложения существуют 32- и 64-разрядные установщики. Файлы имеют имена MultiFactorAuthenticationUserPortalSetupXX.msi и MultiFactorAuthenticationMobileAppWebServiceSetupXX.msi.

   2. Чтобы установить пользовательский портал на веб-сервер, откройте командную строку от имени администратора и запустите файл MultiFactorAuthenticationUserPortalSetupXX.msi.

      Теперь у виртуального каталога по умолчанию будет имя **MultiFactorAuth**, а не **PhoneFactor**. Если вы хотите использовать предыдущее имя, измените имя виртуального каталога на этапе установки. Если во время установки не изменять имя каталога по умолчанию, щелкните значок пользовательского портала на сервере Многофакторной идентификации и измените URL-адрес пользовательского портала на вкладке "Параметры". Новый URL-адрес следует сообщить всем существующим пользователям.

   3. Перейдите в папку установки пользовательского портала (например, C:\inetpub\wwwroot\MultiFactorAuth) и внесите изменения в файл web.config. Скопируйте значения в разделах appSettings и applicationSettings из исходного файла web.config, резервная копия которого была создана до появления нового файла web.config. Если при установке пакета SDK для веб-службы вы оставили новое имя виртуального каталога по умолчанию, измените URL-адрес в разделе applicationSettings так, чтобы он указывал на правильное расположение. Если в предыдущем файле web.config были изменены еще какие-то значения по умолчанию, внесите такие же изменения в новый файл web.config.

> [!NOTE]
> После обновления старой версии сервера Azure MFA (до 8.0) до новой версии (выше 8.0) веб-службу мобильного приложения можно удалить.

## <a name="next-steps"></a>Дальнейшие действия

- [Установите пользовательский портал](howto-mfaserver-deploy-userportal.md) для сервера Многофакторной идентификации Azure.

- [Настройте проверку подлинности Windows](howto-mfaserver-windows.md) для приложений. 
