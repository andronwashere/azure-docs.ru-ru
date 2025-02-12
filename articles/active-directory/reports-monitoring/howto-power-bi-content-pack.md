---
title: Как использовать пакет содержимого Azure Active Directory Power BI | Документация Майкрософт
description: Узнайте, как использовать пакет содержимого Azure Active Directory Power BI
services: active-directory
author: MarkusVi
manager: daveba
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: ''
ms.topic: conceptual
ms.tgt_pltfrm: ''
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 934562147fedcc81b16fd1ad2534af5662ef4b78
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60437701"
---
# <a name="how-to-use-the-azure-active-directory-power-bi-content-pack"></a>Как использовать пакет содержимого Azure Active Directory Power BI

|  |
|--|
|В настоящее время пакет содержимого Azure AD Power BI использует API-интерфейсы Azure AD Graph для получения данных от вашего клиента Azure AD. Таким образом, можно заметить некоторые несоответствия между данными, доступными в пакете содержимого и данными, полученными с помощью [API-интерфейсов Microsoft Graph для создания отчетов](concept-reporting-api.md). |
|  |

Пакет содержимого Power BI для Azure Active Directory (Azure AD) содержит предварительно созданные отчеты, которые помогут понять, как ваши пользователи внедряют и используют функции Azure AD. Это позволяет получить представление обо всех действиях в каталоге, используя разные средства визуализации в Power BI. Кроме того, вы можете создавать собственные информационные панели и предоставлять к ним доступ любому пользователю в организации. 

## <a name="prerequisites"></a>Технические условия

Для использования пакета содержимого требуется лицензия Azure AD Premium (P1 или P2). Чтобы обновить выпуск Azure Active Directory, ознакомьтесь со статьей [Регистрация для работы с выпусками Azure Active Directory Premium](../fundamentals/active-directory-get-started-premium.md).

## <a name="install-the-content-pack"></a>Установка пакета содержимого

Ознакомьтесь с [кратким руководством](quickstart-install-power-bi-content-pack.md) по установке пакета содержимого Azure AD Power BI.

### <a name="reports-included-in-this-version-of-azure-ad-logs-content-pack"></a>Отчеты, включенные в эту версию пакета содержимого журналов Azure AD

В пакет содержимого Azure AD Power BI включены приведенные ниже отчеты. Отчеты содержат данные за **последние 30 дней**.

**Отчет об использовании приложения и соответствующие тренды**.  Этот отчет дает вам представление о приложениях, используемых в организации. Вы можете получить список самых популярных приложений или узнать об использовании приложений, которые были недавно развернуты в организации. Это позволяет отслеживать и улучшать использование с течением времени.

**Входы в систему по пользователям и расположению**. В этом отчете предоставляются данные обо всех входах в систему с помощью удостоверения Azure. С помощью этого отчета вы можете выполнить детализацию до отдельных входов и ответить на следующие вопросы.

- Откуда этот пользователь входит в систему?
- Какой пользователь чаще входит в систему и откуда он выполняет вход? 
- Удалось ли пользователю войти?  
 
Кроме того, вы можете отфильтровать результаты, выбрав определенную дату или расположение.

**Число уникальных пользователей на каждое приложение**.  В этом отчете представлены сведения обо всех уникальных пользователях, использующих определенное приложение. Он включает только тех пользователей, которые *успешно* вошли в приложение.

**Входы с устройств**. С помощью этого отчета вы можете узнать о профилях разных корпоративных устройств и определить соответствующие политики устройств. В этом отчете предоставлены данные о типе ОС и браузерах, используемых для входа в приложения, а также подробные сведения о пользователях, включая следующее:

- Имя пользователя
- IP-адрес
- Расположение 
- состояние входа. 

**Воронка SSPR**. С помощью этого отчета вы узнаете, как в организации используется средство SSPR. Вы можете просмотреть данные о том, сколько попыток сброса пароля предпринято с помощью средства SSPR и сколько из них были успешными. Получите подробную информацию о сбоях во время сброса пароля, чтобы проанализировать причины конкретных сбоев. 

## <a name="customize-azure-ad-activity-content-pack"></a>Настройка пакета содержимого действий Azure AD

**Изменение визуализации**.  Чтобы изменить визуализацию отчетов, нажмите кнопку **Изменить отчет** и выберите нужную визуализацию.
 
![Пакет содержимого Azure Active Directory Power BI](./media/howto-power-bi-content-pack/09.png) 
 
![Пакет содержимого Azure Active Directory Power BI](./media/howto-power-bi-content-pack/10.png) 

**Включение дополнительных полей**.  Чтобы добавить поле в отчет или удалить его, выберите визуальный элемент, к которому требуется добавить поле или из которого требуется удалить поле. Например, вы можете добавить поле "состояние входа"в представление таблицы, как показано ниже. 
 
![Пакет содержимого Azure Active Directory Power BI](./media/howto-power-bi-content-pack/11.png) 

**Закрепление визуализаций на панели мониторинга**.  Вы можете настроить информационную панель и включить собственные визуализации в отчет, а затем закрепить его на информационной панели. 

![Пакет содержимого Azure Active Directory Power BI](./media/howto-power-bi-content-pack/13.png) 
 
**Совместное использование информационной панели**. Вы можете совместно использовать информационную панель с пользователями в организации. Когда вы предоставите общий доступ к отчету, пользователи смогут просматривать поля, выбранные вами в отчете.
 
![Пакет содержимого Azure Active Directory Power BI](./media/howto-power-bi-content-pack/14.png) 

## <a name="schedule-a-daily-refresh-of-your-power-bi-report"></a>Планирование ежедневного обновления отчета Power BI

Чтобы запланировать ежедневное обновление отчета Power BI, щелкните **Наборы данных** > **Параметры** > **Запланировать обновление** и настройте обновление, как показано ниже.
 
![Пакет содержимого Azure Active Directory Power BI](./media/howto-power-bi-content-pack/15.png) 

## <a name="update-to-newer-version-of-content-pack"></a>Обновление до новой версии пакета содержимого

Если вы хотите обновить пакет содержимого, выполните следующие действия.

- Скачайте новый пакет содержимого и настройте его согласно инструкциям, приведенным в этой статье.

- Настроив пакет содержимого, щелкните **Источник данных** > **Параметры** > **Учетные данные источника данных** и повторно введите учетные данные.

    ![Пакет содержимого Azure Active Directory Power BI](./media/howto-power-bi-content-pack/16.png) 

Как только вы убедитесь, что новая версия пакета содержимого работает ожидаемым образом, можно при необходимости удалить старую. Для этого нужно удалить базовые отчеты и наборы данных, связанные со старым пакетом содержимого.

## <a name="troubleshoot-content-pack-errors"></a>Устранение ошибок пакетов содержимого

При работе с пакетом содержимого могут произойти такие ошибки: 

- [Сбой обновления](#refresh-failed) 
- [Не удалось обновить учетные данные источников данных](#failed-to-update-data-source-credentials) 
- [Importing of data is taking too long](#data-import-is-too-slow) (Импорт данных занимает слишком много времени) 

Общие сведения о Power BI см. в [справочных статьях](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).

### <a name="refresh-failed"></a>"Сбой обновления" 
 
**Как отображается эта ошибка**: электронное сообщение от Power BI или состояние ошибки в журнале обновлений. 


| Причина: | Как устранить |
| ---   | ---        |
| Сбои обновления могут возникнуть, если учетные данные пользователей, подключающихся к пакету содержимого, были сброшены, но не обновлены в параметрах подключения пакета содержимого. | В Power BI найдите набор данных, соответствующий панели мониторинга журналов действий Azure Active Directory (**журналы действий Azure Active Directory**), выберите "Запланировать обновление" и введите учетные данные AAD. |
| Обновление может завершиться сбоем из-за проблем с данными в базовом пакете содержимого. | [Отправьте запрос в службу поддержки](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
 
### <a name="failed-to-update-data-source-credentials"></a>"Не удалось обновить учетные данные источников данных" 
 
**Как отображается эта ошибка**: в Power BI при подключении к пакету содержимого журналов действий Azure Active Directory. 

| Причина: | Как устранить |
| ---   | ---        |
| Подключающийся пользователь не является глобальным администратором, читателем сведений о безопасности или администратором безопасности. | Используйте учетную запись глобального администратора, читателя сведений о безопасности или администратора безопасности для доступа к пакетам содержимого. |
| Клиент не имеет лицензии Premium или по крайней мере одного пользователя с файлом лицензии Premium. | [Отправьте запрос в службу поддержки](../fundamentals/active-directory-troubleshooting-support-howto.md).|
 
### <a name="data-import-is-too-slow"></a>Импорт данных выполняется слишком медленно 
 
**Как отображается эта ошибка**: в Power BI после подключения к пакету содержимого процесс импорта данных приступает к подготовке панели мониторинга для журналов действий Azure Active Directory. Отобразится сообщение: **Импорт данных...** без каких-либо дальнейших изменений.  

| Причина: | Как устранить |
| ---   | ---        |
| В зависимости от размера клиента шаг может длиться от нескольких минут до получаса. | Если в течение часа сообщение не исчезнет и не отобразится панель мониторинга, [отправьте запрос в службу поддержки](../fundamentals/active-directory-troubleshooting-support-howto.md).|
  
## <a name="next-steps"></a>Дальнейшие действия

* [Краткое руководство. Установка пакета содержимого Power BI для Azure Active Directory](quickstart-install-power-bi-content-pack.md).
* [Что такое отчеты в Azure Active Directory](overview-reports.md).
