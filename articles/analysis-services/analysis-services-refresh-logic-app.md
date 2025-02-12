---
title: Обновление с помощью Logic Apps для моделей Azure Analysis Services | Документация Майкрософт
description: Узнайте, как кодировать асинхронное обновление с помощью Azure Logic Apps.
author: chrislound
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: chlound
ms.openlocfilehash: 2234a2c6cd42be45a2b2e7784c1dd5aec8839cb9
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311736"
---
# <a name="refresh-with-logic-apps"></a>Обновление с помощью Logic Apps

Используя Logic Apps и вызовы RESTFUL, можно выполнять автоматические операции обновления данных в табличных моделях анализа Azure, включая синхронизацию реплик только для чтения для горизонтального масштабирования запросов.

Дополнительные сведения об использовании API-интерфейсов RESTFUL с Azure Analysis Services см. в разделе [асинхронное обновление с помощью REST API](analysis-services-async-refresh.md).

## <a name="authentication"></a>Проверка подлинности

Все вызовы должны пройти проверку подлинности с помощью допустимого маркера Azure Active Directory (OAuth 2).  В примерах, приведенных в этой статье, для проверки подлинности в Azure Analysis Services используется субъект-служба (SPN). Дополнительные сведения см. в статье [Создание субъекта-службы с помощью портал Azure](../active-directory/develop/howto-create-service-principal-portal.md).

## <a name="design-the-logic-app"></a>Проектирование приложения логики

> [!IMPORTANT]
> В следующих примерах предполагается, что брандмауэр Azure Analysis Services отключен.  Если брандмауэр включен, общедоступный IP-адрес инициатора запроса должен быть список разрешений в брандмауэре Azure Analysis Services. Дополнительные сведения о диапазонах IP-адресов приложений логики для каждого региона см. в разделе [ограничения и сведения о конфигурации для Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md#firewall-configuration-ip-addresses).

### <a name="prerequisites"></a>предварительные требования

#### <a name="create-a-service-principal-spn"></a>Создание субъекта-службы (SPN)

Дополнительные сведения о создании субъекта-службы см. в разделе [Создание субъекта-службы с помощью портал Azure](../active-directory/develop/howto-create-service-principal-portal.md).

#### <a name="configure-permissions-in-azure-analysis-services"></a>Настройка разрешений в Azure Analysis Services
 
Создаваемый субъект-служба должен иметь разрешения администратора сервера на сервере. Дополнительные сведения см. в статье [Добавление субъекта-службы к роли администратора сервера](analysis-services-addservprinc-admins.md).

### <a name="configure-the-logic-app"></a>Настройка приложения логики

В этом примере приложение логики предназначено для активации при получении HTTP-запроса. Это позволит использовать средство оркестрации, такое как фабрика данных Azure, для активации Azure Analysis Services обновления модели.

После создания приложения логики выполните следующие действия.

1. В конструкторе приложений логики выберите первое действие, как **при получении HTTP-запроса**.

   ![Добавить действие "получено HTTP"](./media/analysis-services-async-refresh-logic-app/1.png)

Этот шаг будет заполнен URL-адресом HTTP POST после сохранения приложения логики.

2. Добавьте новый шаг и выполните поиск по запросу **http**.  

   ![Добавление действия HTTP](./media/analysis-services-async-refresh-logic-app/9.png)

   ![Добавление действия HTTP](./media/analysis-services-async-refresh-logic-app/10.png)

3. Выберите **http** , чтобы добавить это действие.

   ![Добавление действия HTTP](./media/analysis-services-async-refresh-logic-app/2.png)

Настройте действие HTTP следующим образом:

|Свойство  |Значение  |
|---------|---------|
|**Метод**     |ПОМЕСТИТЬ         |
|**URI**     | https:// */серверс/* имя*сервера консультантов*/Моделс/*имя базы данных*/рефрешес <br /> <br /> Пример: https:\//westus.asazure.Windows.NET/Servers/MyServer/Models/AdventureWorks/refreshes|
|**Заголовки**     |   Content-Type, Application/JSON <br /> <br />  ![Заголовки](./media/analysis-services-async-refresh-logic-app/6.png)    |
|**Текст**     |   Дополнительные сведения о создании текста запроса см. в разделе [асинхронное обновление с помощью REST API-POST/рефрешес](analysis-services-async-refresh.md#post-refreshes). |
|**Authentication**     |Active Directory OAuth         |
|**Клиентом**     |Заполните Azure Active Directory TenantId         |
|**Аудитория**     |HTTPS://*. asazure. Windows. NET         |
|**Идентификатор клиента**     |Введите имя участника-службы ClientID         |
|**Тип учетных данных**     |`Secret`         |
|**Секрет**     |Введите секретное имя субъекта-службы         |

Пример:

![Завершенное действие HTTP](./media/analysis-services-async-refresh-logic-app/7.png)

Теперь протестируйте приложение логики.  В конструкторе приложений логики нажмите кнопку **выполнить**.

![Тестирование приложения логики](./media/analysis-services-async-refresh-logic-app/8.png)

## <a name="consume-the-logic-app-with-azure-data-factory"></a>Использование приложения логики с фабрикой данных Azure

После сохранения приложения логики просмотрите, **когда ПОЛУЧЕН HTTP-запрос** , а затем скопируйте созданный **URL HTTP POST** .  Это URL-адрес, который может использоваться фабрикой данных Azure для выполнения асинхронного вызова для активации приложения логики.

Ниже приведен пример веб-действия фабрики данных Azure, которое выполняет это действие.

![Веб-действие фабрики данных](./media/analysis-services-async-refresh-logic-app/11.png)

## <a name="use-a-self-contained-logic-app"></a>Использование автономного приложения логики

Если вы не планируете использовать средство оркестрации, такое как фабрика данных для активации обновления модели, можно настроить приложение логики для запуска обновления по расписанию.

Используя приведенный выше пример, удалите первое действие и замените его действием **расписания** .

![Действие по расписанию](./media/analysis-services-async-refresh-logic-app/12.png)

![Действие по расписанию](./media/analysis-services-async-refresh-logic-app/13.png)

В этом примере будет использоваться повторение.

После добавления действия настройте интервал и частоту, а затем добавьте новый параметр и выберите **эти часы**.

![Действие по расписанию](./media/analysis-services-async-refresh-logic-app/16.png)

Выберите желаемые часы.

![Действие по расписанию](./media/analysis-services-async-refresh-logic-app/15.png)

Сохраните приложение логики.

## <a name="next-steps"></a>Следующие шаги

[Примеры](analysis-services-samples.md)  
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)
