---
title: Обработка больших сообщений — Azure Logic Apps | Документация Майкрософт
description: Сведения о том, как обрабатывать сообщения большого размера с фрагментацией в Azure Logic Apps
services: logic-apps
documentationcenter: ''
author: shae-hurst
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: logic-apps
ms.workload: logic-apps
ms.devlang: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.date: 4/27/2018
ms.author: shhurst
ms.openlocfilehash: 5aa5ea2a39a0fb9f969e965fed14063522197cda
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60303796"
---
# <a name="handle-large-messages-with-chunking-in-azure-logic-apps"></a>Обработка больших сообщений с фрагментацией в Azure Logic Apps

Содержимое обрабатываемых сообщений в Logic Apps ограничено неким максимальным размером. Это ограничение помогает снизить нагрузку, которая возникает при хранении и обработке больших сообщений. Для обработки сообщений, размер которых превышает это ограничение, Logic Apps может *разбить* их на несколько маленьких. Таким образом, вы все еще можете передавать большие файлы через Logic Apps, но с определенными условиями. При взаимодействии с другими службами через соединители или HTTP служба Logic Apps может принимать большие сообщения, но *только* блоками. Это условие означает, что соединители также должны поддерживать фрагментацию или базовый обмен сообщениями HTTP с Logic Apps и эти службы должны применять фрагментацию.

В этой статье показано, как настроить фрагментацию сообщений об обработке действий, размер которых превышает ограничение. Триггеры приложения логики не поддерживают фрагментацию из-за увеличения издержек при обмене множеством сообщений. 

## <a name="what-makes-messages-large"></a>Какое сообщение является большим?

Сообщения считаются большими в зависимости от требований службы, которая их обрабатывает. Точное ограничение размера больших сообщений отличается в Logic Apps и соединителях. И Logic Apps, и соединители не могут принимать большие сообщения напрямую, а только с применением фрагментации. Максимальный размер сообщения в Logic Apps см. в статье [Ограничения и сведения о конфигурации для Azure Logic Apps](../logic-apps/logic-apps-limits-and-config.md).
Максимальный размер сообщения каждого соединителя см. в статье [Список соединителей](../connectors/apis-list.md).

### <a name="chunked-message-handling-for-logic-apps"></a>Обработка фрагментированного сообщения для Logic Apps

Logic Apps не может напрямую использовать выходные данные из фрагментированных сообщений, размер которых превышает ограничение. Содержимое сообщения в таких выходных данных может быть обработано только теми действиями, которые поддерживают фрагментацию. Действие, которое обрабатывает большие сообщения, должно соответствовать *одному* из следующих критериев:

* изначально поддерживать фрагментацию, если это действие относится к контейнеру; 
* иметь включенную поддержку фрагментации в конфигурации запуска. 

В противном случае при попытке получить доступ к выходным данных большого содержимого возникнет ошибка среды выполнения. Сведения о включении фрагментации см. в разделе [Настройка фрагментации по протоколу HTTP](#set-up-chunking).

### <a name="chunked-message-handling-for-connectors"></a>Обработка фрагментированного сообщения для соединителей

Службы, которые взаимодействуют с Logic Apps, могут иметь собственные ограничения на размер сообщения. Эти ограничения часто меньше, чем ограничения Logic Apps. Например, соединитель, поддерживающий фрагментацию, может рассматривать сообщение размером 30 МБ как большое, в то время как Logic Apps — нет. Чтобы соблюсти это ограничение соединителя, Logic Apps разбивает любое сообщение размером свыше 30 МБ на небольшие блоки.

В соединителях, которые поддерживают фрагментацию, базовый протокол фрагментации невидим для пользователей. Однако не все соединители поддерживают фрагментацию, и если размер входящих сообщений превышает ограничения в таком соединителе, возникнет ошибка среды выполнения.

<a name="set-up-chunking"></a>

## <a name="set-up-chunking-over-http"></a>Настройка фрагментации по протоколу HTTP

В общих сценариях с HTTP большие файлы, загружаемые и передаваемые по протоколу HTTP, можно разделить, чтобы позволить приложению логики и конечной точке обмениваться большими сообщениями. Тем не менее сообщения необходимо фрагментировать так, как того ожидает Logic Apps. 

Если в конечной точке включена фрагментация для загрузки или передачи, действия HTTP в приложении логики автоматически разбивают большие сообщения на блоки. В противном случае в конечной точке необходимо настроить поддержку фрагментации. Если вы не владеете конечной точкой либо соединителем или у вас нет контроля над ними, возможно, вы не сможете настроить фрагментацию.

Кроме того, если в действии HTTP не включена фрагментация, вам также нужно настроить фрагментацию в свойстве `runTimeConfiguration` этого действия. Это свойство можно задать внутри действия непосредственно в редакторе представления кода, как описано далее, либо в конструкторе Logic Apps, как описано здесь:

1. В правом верхнем углу в окне действия HTTP нажмите кнопку с многоточием ( **...** ), а затем выберите **Параметры**.

   ![В окне действия откройте меню "Параметры"](./media/logic-apps-handle-large-messages/http-settings.png)

2. В разделе **Передача содержимого** переведите переключатель **Разрешить фрагментирование** в положение **Вкл.**

   ![Включение фрагментации](./media/logic-apps-handle-large-messages/set-up-chunking.png)

3. Чтобы настроить фрагментацию для загрузки или передачи, перейдите к следующим разделам.

<a name="download-chunks"></a>

## <a name="download-content-in-chunks"></a>Загрузка содержимого блоками

Много конечных точек автоматически отправляют большие сообщения блоками при загрузке с помощью HTTP-запроса GET. Для загрузки фрагментированных сообщений из конечной точки по протоколу HTTP конечная точка должна поддерживать запросы частичного содержимого или *поблочную загрузку*. Если приложение логики отправляет в конечную точку HTTP-запрос GET для загрузки содержимого и конечная точка отвечает с кодом состояния "206", в ответе содержится фрагментированное содержимое. Logic Apps не может контролировать то, поддерживает ли конечная точка частичные запросы. Тем не менее, когда приложение логики получает первый ответ с кодом "206", оно автоматически отправляет несколько запросов, чтобы загрузить все содержимое.

Чтобы проверить, поддерживает ли конечная точка частичное содержимое, отправьте запрос HEAD. Этот запрос позволяет определить, содержит ли ответ заголовок `Accept-Ranges`. Таким образом, если конечная точка поддерживает поблочную загрузку, но не отправляет фрагментированное содержимое, вы можете *настроить* это, задав заголовок `Range` в HTTP-запросе GET. 

Далее подробно описан процесс, который Logic Apps использует для загрузки фрагментированного содержимого из конечной точки в приложение логики.

1. Ваше приложение логики отправляет HTTP-запрос GET в конечную точку.

   При необходимости в заголовке запроса может содержаться поле `Range`, описывающее диапазон байтов для запроса блоков содержимого.

2. Конечная точка возвращает код состояния "206" и основной текст сообщения HTTP.

    Сведения о содержимом в этом блоке содержатся в заголовке `Content-Range` ответа, включая информацию о приложении, на основе которой Logic Apps определяет начало и конец блока данных, а также общий размер всего содержимого до фрагментации.

3. Приложение логики автоматически отправляет последующие HTTP-запросы GET.

    Они отправляются, пока не будет получено все содержимое.

В приведенном ниже определении действия показан HTTP-запрос GET, в котором задан заголовок `Range`. Заголовок *предполагает*, что конечная точка должна возвращать поблочное содержимое:

```json
"getAction": {
    "inputs": {
        "headers": {
            "Range": "bytes=0-1023"
        },
       "method": "GET",
       "uri": "http://myAPIendpoint/api/downloadContent"
    },
    "runAfter": {},
    "type": "Http"
}
```

Запрос GET устанавливает заголовок "Range" со значением "bytes=0-1023", что представляет собой диапазон байтов. Если конечная точка поддерживает запросы частичного содержимого, она возвращает блок содержимого из запрошенного диапазона. Точный формат поля заголовка "Range" может отличаться в зависимости от конечной точки.

<a name="upload-chunks"></a>

## <a name="upload-content-in-chunks"></a>Передача содержимого блоками

Чтобы отправить фрагментированное содержимое из действия HTTP, в действии должна быть включена поддержка фрагментации с помощью свойства `runtimeConfiguration`. Это свойство позволяет действию использовать протокол фрагментации. После этого приложение логики может отправлять начальное сообщение POST или PUT в целевую конечную точку. Когда конечная точка вернет в ответе предлагаемый размер блока, приложение логики начнет отправлять HTTP-запросы PATCH, содержащие блоки содержимого.

Далее подробно описан процесс, который Logic Apps использует для передачи фрагментированного содержимого из приложения логики в конечную точку.

1. Приложение логики отправляет начальный HTTP-запрос POST или PUT с пустым сообщением. Заголовок запроса содержит такие сведения о содержимом, которое приложение логики хочет передать поблочно:

   | Поле заголовка запроса Logic Apps | Значение | type | Описание |
   |---------------------------------|-------|------|-------------|
   | **x-ms-transfer-mode** | chunked | String | Указывает, что содержимое передается блоками |
   | **x-ms-content-length** | <*длина_содержимого*> | Integer | Размер всего содержимого (в байтах) перед фрагментацией |
   ||||

2. Конечная точка возвращает ответ с кодом состояния "200" и такую дополнительную информацию:

   | Поле заголовка ответа конечной точки | type | Обязательно для заполнения | Описание |
   |--------------------------------|------|----------|-------------|
   | **x-ms-chunk-size** | Integer | Нет | Предлагаемый размер блока в байтах |
   | **Location** | String | Нет | URL-адрес расположения для отправки сообщений HTTP PATCH |
   ||||

3. Приложение логики создает и отправляет последующие сообщения HTTP PATCH с приведенными ниже сведениями.

   * Блок содержимого с размером, указанным в поле **x-ms-chunk-size**, или некоторым внутренне вычисленным размером. Блоки отправляются, пока все содержимое, вместе равное **x-ms-content-length**, не будет последовательно передано.

   * Следующие сведения заголовка о блоке содержимого отправляются в каждом сообщении PATCH:

     | Поле заголовка запроса Logic Apps | Значение | type | Описание |
     |---------------------------------|-------|------|-------------|
     | **Content-Range** | <*диапазон*> | String | Диапазон байтов для текущего блока содержимого, включая начальное значение, конечное значение и общий размер содержимого, например "bytes=0-1023/10100" |
     | **Content-Type** | <*тип_содержимого*> | String | Тип фрагментированного содержимого |
     | **Content-Length** | <*длина_содержимого*> | String | Длина размера в байтах для текущего блока |
     |||||

4. После каждого запроса PATCH конечная точка подтверждает получение каждого блока, отправляя ответ с кодом состояния "200".

Например, в этом определении действия показан HTTP-запрос POST на передачу фрагментированного содержимого в конечную точку. В свойстве `runTimeConfiguration` действия содержится свойство `contentTransfer`, в котором для `transferMode` задано значение `chunked`:

```json
"postAction": {
    "runtimeConfiguration": {
        "contentTransfer": {
            "transferMode": "chunked"
        }
    },
    "inputs": {
        "method": "POST",
        "uri": "http://myAPIendpoint/api/action",
        "body": "@body('getAction')"
    },
    "runAfter": {
    "getAction": ["Succeeded"]
    },
    "type": "Http"
}
```