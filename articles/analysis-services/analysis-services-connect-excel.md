---
title: Подключение к Azure Analysis Services с помощью Excel | Документы Майкрософт
description: Сведения о подключении к серверу Azure Analysis Services с помощью Excel.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 5c46d4e4d23744cf07ccf7857a33990bf405a6a1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61023278"
---
# <a name="connect-with-excel"></a>Подключение с помощью Excel

После создания сервера и развертывания на нем табличной модели клиенты смогут подключиться к нему для работы с данными. 

## <a name="before-you-begin"></a>Перед началом работы

Учетная запись, с помощью которой вы вошли в систему, должна принадлежать к роли шаблона базы данных по крайней мере с правами на чтение. Дополнительные сведения см. в руководстве по [аутентификации и настройке пользовательских разрешений](analysis-services-manage-users.md). 

## <a name="connect-in-excel"></a>Подключение в Excel

Подключиться к серверу из Excel можно с помощью функции "Получить данные" в Excel 2016 и более поздних версий. Подключение с помощью мастера импорта таблиц в Power Pivot не поддерживается. 

1. В Excel на ленте **Данные** щелкните **Получить внешние данные** > **Из других источников** > **Из служб Analysis Services**.

2. В мастере подключения к данным введите имя сервера, включая протокол и универсальный код ресурса (URI), в поле **Имя сервера**. Например, asazure://westcentralus.asazure.windows.net/advworks. Затем для параметра **Учетные данные** выберите значение **Использовать следующие имя пользователя и пароль**, введите имя пользователя, например nancy@adventureworks.com, и пароль для него.

    > [!IMPORTANT]
    > Если вы входите в систему с учетной записью Майкрософт, Live ID, Yahoo, Gmail, и т. д. или требуется войти с помощью многофакторной проверки подлинности, оставьте поле пароля пустым. Пароль запрашивается после нажатия кнопки "Далее". 

    ![Подключение из Excel: вход](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. В разделе **Выбор базы данных и таблицы** выберите нужную базу данных, а также модель или перспективу, и нажмите кнопку **Готово**.
   
    ![Подключение из Excel: выбор модели](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>См. также

[Клиентские библиотеки](analysis-services-data-providers.md)   
[Управление службами Analysis Services](analysis-services-manage.md)     


