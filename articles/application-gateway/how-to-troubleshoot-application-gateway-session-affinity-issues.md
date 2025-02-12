---
title: Устранение проблем со сходством сеансов шлюза приложения Azure
description: В этой статье сведения об устранении проблем со сходством сеансов шлюза приложений Azure
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/22/2019
ms.author: absha
ms.openlocfilehash: 66f61b5d6fcb86ed93e4dbae802ae7a80613c83d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66397839"
---
# <a name="troubleshoot-azure-application-gateway-session-affinity-issues"></a>Устранение проблем со сходством сеансов шлюза приложения Azure

Узнайте, как для диагностики и устранения проблем со сходством сеансов шлюза приложений Azure.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Обзор

Функция сходства сеансов на основе файлов cookie удобна, если нужно, чтобы сеанс пользователя выполнялся на одном и том же сервере. Используя управляемые шлюзом файлы cookie, шлюз приложений может направлять последующий трафик из сеанса пользователя на тот же сервер для обработки. Эта функция важна, когда состояние сеанса сохраняется локально на сервере для сеанса пользователя.

## <a name="possible-problem-causes"></a>Причины, возможно, ошибка

Проблема в обеспечение сходства сеансов на основе файлов cookie может произойти из-за следующих причин:

- Параметр «сходство на основе файлов cookie» не включен
- Приложение не может обрабатывать сходство на основе файлов cookie
- Приложение использует сходство на основе файлов cookie, но запросы по-прежнему перебрасываются между внутренних серверов

### <a name="check-whether-the-cookie-based-affinity-setting-is-enabled"></a>Проверьте, включен ли параметр «сходство на основе файлов Cookie»

Иногда проблем со сходством сеансов может произойти, если вы забудете включить параметр «Сходство на основе файлов Cookie». Чтобы определить, включен ли параметр «Сходство на основе файлов Cookie» на вкладке Параметры HTTP на портале Azure, следуйте инструкциям:

1. Войдите на [портал Azure](https://portal.azure.com/).

2. В **слева навигации** панели щелкните **все ресурсы**. Щелкните имя шлюза приложений в колонке "все ресурсы". Если выбранная подписка имеет несколько ресурсов, можно ввести имя шлюза приложений в **фильтровать по имени...** чтобы быстро получить доступ к необходимому шлюзу приложений.

3. Выберите **параметры HTTP** открыть, выбрав вкладку **параметры**.

   ![Устранение неполадок сеанса сходство проблемы-1](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-1.png)

4. Нажмите кнопку **appGatewayBackendHttpSettings** справа от оператора, чтобы проверить, выбран ли **включено** для сходства на основе файлов Cookie.

   ![Устранение неполадок сеанса сходство проблемы-2](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-2.jpg)



Можно также проверить значение "**CookieBasedAffinity**" имеет значение *включено*в разделе "**backendHttpSettingsCollection**" с помощью одного из следующих методов:

- Запустите [Get-AzApplicationGatewayBackendHttpSetting](https://docs.microsoft.com/powershell/module/az.network/get-azapplicationgatewaybackendhttpsetting) в PowerShell
- Просмотрите файл JSON с помощью шаблона Azure Resource Manager

```
"cookieBasedAffinity": "Enabled", 
```

### <a name="the-application-cannot-handle-cookie-based-affinity"></a>Приложение не поддерживает сходство на основе файлов cookie

#### <a name="cause"></a>Причина:

Шлюз приложений можно выполнить только сходство на основе сеанса с помощью файла cookie.

#### <a name="workaround"></a>Возможное решение

Если приложение не поддерживает сходство на основе файлов cookie, необходимо использовать внешний или внутренний azure load balancer или другое решение для независимых производителей.

### <a name="application-is-using-cookie-based-affinity-but-requests-still-bouncing-between-back-end-servers"></a>Приложение использует сходство на основе файлов cookie, но запросы по-прежнему перебрасываются между внутренних серверов

#### <a name="symptom"></a>Симптом

Вы включили параметр сходство на основе файлов Cookie, при доступе к шлюза приложений с помощью URL-адреса короткого имени в Internet Explorer, например: [ http://website ](http://website/) , запрос по-прежнему перебрасываются между внутренних серверов.

Чтобы определить эту проблему, следуйте инструкциям:

1. Проведение трассировки отладчик web «Клиент» которого подключается к приложению за Gateway(We are using Fiddler in this example) приложения.
    **Совет** Если вы не знаете, как использовать приложение Fiddler, установите флажок "**требуется сбор сетевого трафика и анализировать их с помощью веб-отладчик**" внизу.

2. Проверьте и анализ журналов сеанса, чтобы определить, имеют ли файлы cookie, предоставленных клиентом ARRAffinity сведения. Если вы не нашли ARRAffinity таких сведений, таких как "**ARRAffinity =** *ARRAffinityValue*" в наборе файлов cookie, это значит, что клиент не отвечает с файлом cookie СОЗД, предоставляемый Шлюз приложений.
    Например:

    ![Устранение неполадок сеанса сходство проблемы-3](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-3.png)

    ![Устранение неполадок сеанса сходство проблемы-4](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-4.png)

Приложение продолжает попытки задать файл cookie при каждом запросе, пока не получит ответа.

#### <a name="cause"></a>Причина:

Эта проблема возникает, так как Internet Explorer и другие браузеры не могут хранить или использовать куки-файл с URL-адреса короткого имени.

#### <a name="resolution"></a>Способы устранения:

Чтобы устранить эту проблему, для доступа к Шлюзу приложений следует использовать полное доменное имя. Например, использовать [ http://website.com ](https://website.com/) или [ http://appgw.website.com ](http://appgw.website.com/) .

## <a name="additional-logs-to-troubleshoot"></a>Дополнительные журналы для устранения неполадок

Можно собрать дополнительные журналы и анализировать их для устранения проблем связанных сеансов на основе файлов cookie сходства

### <a name="analyze-application-gateway-logs"></a>Анализ журналов шлюза приложений

Чтобы собирать журналы шлюза приложений, следуйте инструкциям:

Включение ведения журнала на портале Azure

1. В [портала Azure](https://portal.azure.com/), найдите нужный ресурс и нажмите кнопку **журналы диагностики**.

   Для шлюза приложений доступны три журнала: Журнал доступа "," Журнал производительности "," Журнал брандмауэра.

2. Чтобы начать сбор данных, нажмите кнопку **включить диагностику**.

   ![Устранение неполадок сеанса сходство проблемы-5](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-5.png)

3. В колонке **Параметры диагностики** представлены параметры журналов диагностики. В этом примере для хранения журналов используется Log Analytics. Нажмите кнопку **Настройка** под **Log Analytics** задать рабочую область. хранения журналов диагностики можно также использовать концентраторы событий и учетную запись хранения.

   ![Устранение неполадок сеанса сходство проблемы-6](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-6.png)

4. Проверьте параметры и нажмите кнопку **Сохранить**.

   ![Устранение неполадок сеанса сходство проблемы-7](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-7.png)

#### <a name="view-and-analyze-the-application-gateway-access-logs"></a>Просмотр и анализ журналов доступа шлюза приложений

1. На портале Azure в представлении ресурсов шлюза приложений, выберите **журналы диагностики** в **МОНИТОРИНГ** раздел.

   ![Устранение неполадок сеанса сходство проблемы-8](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-8.png)

2. В правой части, выберите "**ApplicationGatewayAccessLog**" в раскрывающемся списке **категории журналов.**  

   ![Устранение неполадок сеанса сходство проблемы-9](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-9.png)

3. В списке журнал доступа шлюза приложений выберите журнал, который вы хотите проанализировать и экспортировать, а затем экспортировать JSON-файл.

4. Преобразование JSON-файл, экспортированный на шаге 3 в CSV-файл и просматривать их в Excel, Power BI или любой другой инструмент визуализации данных.

5. Проверьте следующие данные:

- **ClientIP**— это IP-адрес клиента из подключающегося клиента.
- **ClientPort** — это исходный порт подключающегося клиента для запроса.
- **RequestQuery** – это указывает конечный сервер, что получен запрос.
- **Server-Routed**. Экземпляр внутреннего пула, что получен запрос.
- **X-AzureApplicationGateway-LOG-ID**. Идентификатор корреляции, использованный для запроса. Он может использоваться для устранения неполадок с трафиком на внутренних серверах. Например:  X-AzureApplicationGateway-CACHE-HIT=0&SERVER-ROUTED=10.0.2.4.

  - **SERVER-STATUS**. Код отклика HTTP, полученный Шлюзом приложений из серверной части.

  ![Устранение неполадок сеанса сходство проблемы-11](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-11.png)

Если вы увидите два элемента поступают с тем же ClientIP и порт клиента, и они отправляются на тот же сервер серверной части, значит правильно настроенный шлюз приложения.

Если вы увидите два элемента поступают с тем же ClientIP и порт клиента, и их отправки в разные внутренние серверы, то это значит, запрос перебрасываются между внутренних серверов, выберите "**приложением на основе файлов cookie сходства, но запросы по-прежнему перебрасываются между внутренних серверов**«внизу, чтобы устранить эту проблему.

### <a name="use-web-debugger-to-capture-and-analyze-the-http-or-https-traffics"></a>Использовать веб-отладчик собирать и анализировать трафик HTTP или HTTPS

Веб-средства отладки, например Fiddler, может помочь в отладке веб-приложений, записывающие сетевой трафик между компьютерами, Интернетом и тестирования. Эти средства позволяют проверять входящие и исходящие данные, как обозреватель получает и отправляет их. Fiddler, в этом примере имеет параметр воспроизведения HTTP, который может помочь при отладке проблем на стороне клиента с веб-приложений, особенно для проверки подлинности типа ошибки.

Используйте веб-отладчик по своему усмотрению. В этом примере мы будем использовать Fiddler, записывать и анализировать трафик http или https, следуйте инструкциям:

1. Скачайте средство Fiddler из <https://www.telerik.com/download/fiddler>.

    > [!NOTE]
    > Выберите Fiddler4, если записи компьютере установлена платформа .NET 4. В противном случае выберите Fiddler2.

2. Щелкните правой кнопкой мыши исполняемый файл установки и запуска от имени администратора для установки.

    ![Устранение неполадок сеанса сходство проблемы-12](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-12.png)

3. При открытии Fiddler, она автоматически начнет записывать трафик (Обратите внимание, что запись в левом нижнем углу). Нажмите клавишу F12, чтобы запустить или остановить запись трафика.

    ![Устранение неполадок сеанса сходство проблемы-13](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-13.png)

4. Скорее всего, вас могут заинтересовать расшифрованный HTTPS-трафика, и расшифровки HTTPS можно включить, установив **средства** > **параметры Fiddler**и установите флажок " **расшифровки HTTPS-трафика**«.

    ![Устранение неполадок сеанса сходство проблемы-14](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-14.png)

5. Вы можете удалить предыдущие сеансы несвязанных перед воспроизводящий проблему, щелкнув **X** (значок) > **удалить все** как выполните снимок экрана: 

    ![Устранение неполадок сеанса сходство проблемы-15](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-15.png)

6. Как только воспроизвести проблему, сохраните файл на проверку, выбрав **файл** > **Сохранить** > **все сеансы...** . 

    ![Устранение неполадок сеанса сходство проблемы-16](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-16.png)

7. Проверьте и проанализировать журналы сеанса, чтобы определить, какая проблема.

    Примеры приведены ниже.

- **Пример а.** Поиск журнала сеанса, запрос отправляется от клиента, который передается в общедоступный IP-адрес шлюза приложений, щелкните этот журнал, чтобы просмотреть сведения.  Справа от оператора данные в поле, — это шлюз приложений возвращаются к клиенту. Перейдите на вкладку «НЕОБРАБОТАННЫХ» и определить, получает ли клиент "**Set-Cookie: ARRAffinity =** *ARRAffinityValue*.» Если нет файл cookie, сходство сеансов не задано, или шлюз приложений не применяет файл cookie обратно клиенту.

   > [!NOTE]
   > Это значение ARRAffinity является идентификатором файла cookie, шлюз приложений задает для клиента отправку определенного сервера серверной части.

   ![Устранение неполадок сеанса сходство проблемы-17](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-17.png)

- **Пример б** Следующий журнал сеансов следуют предыдущий ответа обратно в шлюз приложений, который задан ARRAAFFINITY клиента. Если идентификатор cookie ARRAffinity совпадает, пакет должен отправляться на том же сервере серверной части, который использовался ранее. Проверьте следующие несколько строк из HTTP-соединений, чтобы увидеть, меняется ли файл cookie ARRAffinity клиента.

   ![Устранение неполадок сеанса сходство проблемы-18](./media/how-to-troubleshoot-application-gateway-session-affinity-issues/troubleshoot-session-affinity-issues-18.png)

> [!NOTE]
> Для одного сеанса связи файл cookie должен не позволяют менять. Проверьте поле сверху, справа от оператора, перейдите на вкладку «Файлы cookie» для определения соответствия клиента с помощью файла cookie и отправкой обратно в шлюз приложений. В противном случае браузера клиента не сохранение, используя файл cookie для диалогов. В некоторых случаях может быть клиент.

 

## <a name="next-steps"></a>Дальнейшие действия

Если описанные выше шаги не устранят проблему, отправьте [запрос в службу поддержки](https://azure.microsoft.com/support/options/).
