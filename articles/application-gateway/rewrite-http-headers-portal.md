---
title: Перепишите заголовки запросов и ответов HTTP со шлюзом приложений Azure — портал Azure | Документация Майкрософт
description: Узнайте, как использовать портал Azure для настройки шлюза приложений Azure переписывать заголовки HTTP в запросах и ответах, проходящие через шлюз
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/10/2019
ms.author: absha
ms.custom: mvc
ms.openlocfilehash: e144214a58f9fe383cf4edd878554792d9d6a6f9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64947165"
---
# <a name="rewrite-http-request-and-response-headers-with-azure-application-gateway---azure-portal"></a>Перепишите заголовки запросов и ответов HTTP со шлюзом приложений Azure — портал Azure

В этой статье описывается, как с помощью портала Azure настроить [SKU шлюза приложений версии 2](<https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant>) экземпляр переписывать заголовки HTTP в запросах и ответах.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

## <a name="before-you-begin"></a>Перед началом работы

Необходимо иметь экземпляр SKU шлюза приложений версии 2 для выполнения действий, описанных в этой статье. Переопределения заголовков не поддерживается в v1 SKU. Если у вас нет v2 SKU, создать [SKU шлюза приложений версии 2](https://docs.microsoft.com/azure/application-gateway/tutorial-autoscale-ps) экземпляра перед началом работы.

## <a name="create-required-objects"></a>Создание необходимых объектов

Чтобы настроить перезапись заголовка HTTP, вам потребуется для выполнения этих действий.

1. Создайте объекты, которые требуются для переопределения заголовка HTTP:

   - **Перепишите действие**: Используется для указания запроса и полей заголовка запроса, которые вы собираетесь переписать и новое значение для заголовков. Может быть связано одно или несколько переписать условий с помощью действия переопределения.

   - **Перепишите условие**: Дополнительная настройка. Перезапись условия оценки содержимого HTTP (S) запросов и ответов. Действие переопределения произойдет в том случае, если запрос HTTP (S) или ответ соответствует условию перезаписи.

     Если более одного условия связывается с действием, действие происходит только в том случае, если выполнены все условия. Другими словами операция остается логическую операцию и.

   - **Правила переопределения**: Содержит несколько действий перезаписи / переписать сочетания условий.

   - **Правила последовательности**: Помогает определить порядок, в которой выполняются правила подстановки. Эта конфигурация полезно при наличии нескольких правила подстановки в наборе перезаписи. Правило переопределения, которое имеет меньшее значение последовательности правило выполняется первой. Если два правила подстановки назначается то же значение последовательности правило, порядок выполнения является недетерминированным.

   - **Перепишите набора**: Содержит несколько правил переопределения, которые будут связаны с правило маршрутизации запроса.

2. Подключите перезаписи, задайте для правила маршрутизации. Параметры перезаписи, присоединяется к исходному прослушивателю посредством правила маршрутизации. При использовании базового правила маршрутизации, конфигурация переопределения заголовка связывается с исходным прослушивателем и перезапись глобального заголовка. При использовании правила маршрутизации на основе пути конфигурация переопределения заголовка задается для сопоставления URL-пути. В этом случае он применяется только к области конкретного пути узла.

Можно создать несколько наборов переопределения заголовка HTTP и применить каждой перезаписи присвоено несколько прослушивателей. Но вы можете применить только один переписать набора для конкретного прослушивателя.

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на [портал Azure](https://portal.azure.com/) с помощью своей учетной записи Azure.

## <a name="configure-header-rewrite"></a>Настройка переопределения заголовка

В этом примере мы изменим URL-адрес перенаправления, переписав под заголовком location в HTTP-ответа, отправленные из приложения серверной части.

1. Выберите **все ресурсы**, а затем выберите шлюз приложений.

2. Выберите **перезаписывает** в левой области.

3. Выберите **переписать набора**:

   ![Добавление новой версии набора](media/rewrite-http-headers-portal/add-rewrite-set.png)

4. Укажите имя для переопределения набора и связать его с правило маршрутизации:

   - Введите имя для переопределения, в **имя** поле.
   - Выберите одну или несколько правил, перечисленные в **связанные правила маршрутизации** списка. Вы можете выбрать только те правила, которые еще не связан с другими наборами перезаписи. Правила, которые уже были связаны с другими наборами перезаписи недоступны.
   - Щелкните **Далее**.
   
     ![Добавьте имя и связь](media/rewrite-http-headers-portal/name-and-association.png)

5. Создайте правила переопределения:

   - Выберите **правила переопределения добавить**.

     ![Добавление правила переопределения](media/rewrite-http-headers-portal/add-rewrite-rule.png)

   - Введите имя для правила перезаписи в **имя правила перезаписи** поле. Введите номер в **правила последовательности** поле.

     ![Добавьте имя правила переопределения](media/rewrite-http-headers-portal/rule-name.png)

6. В этом примере мы будем перепишите заголовок location, только в том случае, если он содержит ссылку на azurewebsites.net. Чтобы сделать это, добавьте условие для оценки под заголовком location в ответе содержит azurewebsites.net ли:

   - Выберите **добавить условие** и затем выберите поле, содержащую **Если** инструкции, чтобы развернуть его.

     ![Добавить условие](media/rewrite-http-headers-portal/add-condition.png)

   - В **тип переменной, чтобы проверить** выберите **заголовок HTTP**.

   - В **тип заголовка** выберите **ответа**.

   - Так как в этом примере мы оценке заголовок location, который является заголовком common, выберите **общая область** под **имя заголовка**.

   - В **общая область** выберите **расположение**.

   - В разделе **с учетом регистра**выберите **нет**.

   - В **оператор** выберите **равно (=)** .

   - Введите шаблон регулярного выражения. В этом примере мы будем использовать шаблон `(https?):\/\/.*azurewebsites\.net(.*)$`.

   - Нажмите кнопку **ОК**.

     ![Настройка If условие](media/rewrite-http-headers-portal/condition.png)

7. Добавьте действие, которое перезаписывает заголовок расположения:

   - В **тип действия** выберите **задать**.

   - В **тип заголовка** выберите **ответа**.

   - В разделе **имя заголовка**выберите **распространенных заголовка**.

   - В **общая область** выберите **расположение**.

   - Введите значение заголовка. В этом примере мы будем использовать `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` как значение заголовка. Это значение заменит *azurewebsites.net* с *contoso.com* в заголовке location.

   - Нажмите кнопку **ОК**.

     ![Добавление действия](media/rewrite-http-headers-portal/action.png)

8. Выберите **создать** Создание перезаписи набора:

   ![Нажмите кнопку "Создать"](media/rewrite-http-headers-portal/create.png)

9. Откроется представление набора перезаписи. Убедитесь, что набор переопределения, который вы создали в списке наборов данных о перезаписи:

   ![Перепишите представления набора](media/rewrite-http-headers-portal/rewrite-set-list.png)

## <a name="next-steps"></a>Дальнейшие действия

Дополнительные сведения о том, как настроить наиболее распространенные варианты использования, см. в разделе [общая область переписывать сценарии](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers).