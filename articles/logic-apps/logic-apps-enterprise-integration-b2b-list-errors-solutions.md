---
title: Ошибки сценариев B2B и их решения в Azure Logic Apps | Документация Майкрософт
description: Поиск ошибок и решений для сценариев B2B в Azure Logic Apps.
services: logic-apps
ms.service: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, LADocs
ms.topic: article
ms.date: 06/02/2017
ms.openlocfilehash: f0591b47ce7ba6837f300088c856c0098fb66710
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60998828"
---
# <a name="b2b-errors-and-solutions-for-azure-logic-apps"></a>Ошибки B2B и их решения для Azure Logic Apps

В этой статье содержатся сведения об устранении ошибок в сценариях Logic Apps B2B, а также приведены соответствующие действия по их решению.

## <a name="agreement-resolution"></a>Определение соглашения

### <a name="no-agreement-found"></a>Соглашение не найдено 

|   |   |  
|---|---|
| Описание ошибки | Отсутствует соглашение с параметрами определения соглашения. | 
| Рекомендуемые действия | Добавьте соглашение в учетную запись интеграции с согласованными бизнес-идентификаторами. </br>Бизнес-идентификаторы должны совпадать с идентификаторами входящего сообщения. |  
|   |   |

### <a name="no-agreement-found-with-identities"></a>Соглашение с идентификаторами отсутствует

|   |   | 
|---|---|
| Описание ошибки | Не найдено соглашение с удостоверениями: 'AS2Identity':: 'Partner1' и 'AS2Identity':: 'Partner3' | 
| Рекомендуемые действия | В соглашении определены недопустимые идентификаторы AS2-From (AS2 — от) и AS2-To (AS2 — кому). </br>Введите правильные идентификаторы AS2-From (AS2 — от) и AS2-To (AS2 — кому) для сопоставления идентификаторов AS2 в заголовках сообщения AS2 с параметрами соглашения. |
|   |   |     

## <a name="as2"></a>AS2

### <a name="missing-as2-message-headers"></a>Отсутствуют заголовки сообщения AS2  

|   |   |  
|---|---|
| Описание ошибки | Недопустимые заголовки AS2. Идентификатор AS2-From (AS2 — от) или AS2-To (AS2 — кому) пустой. | 
| Рекомендуемые действия | Получено сообщение AS2, не содержащее заголовки AS2-From (AS2 — от) или 2-To (AS2 — кому) (один из них или оба). </br> Проверьте эти заголовки и исправьте их в соответствии с конфигурацией соглашения. |
|  |  | 

### <a name="missing-as2-message-body-and-headers"></a>Отсутствуют заголовки или текст сообщения AS2    

|   |   |  
|---|---|
| Описание ошибки | Содержимое запроса пустое или имеет значение Null. | 
| Рекомендуемые действия | Получено пустое сообщение AS2 (без текста). |
|  |  | 

### <a name="as2-message-decryption-failure"></a>Ошибка расшифровки сообщения AS2

|   |   | 
|---|---|
| Описание ошибки |  [processed/Error: decryption-failed] | 
| Рекомендуемые действия | Добавьте в сообщение AS2 параметр @base64ToBinary перед отправкой партнеру. |
|||

Пример:

```json
"HTTP": {
   "inputs": {
   "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
   "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
   "method": "POST",
   "uri": "xxxxx.xxx"
},
``` 

### <a name="mdn-decryption-failure"></a>Ошибка расшифровки MDN

|   |   | 
|---|---|
| Описание ошибки |  [processed/Error: decryption-failed] | 
| Рекомендуемые действия | Добавьте в сообщение MDN параметр @base64ToBinary перед отправкой партнеру. | 
|||

Пример:

```json
"Response": {
   "inputs": {
   "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
   "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
   "statusCode": 200
},               
``` 

### <a name="missing-signing-certificate"></a>Отсутствует сертификат для подписи

|   |   |  
|---|---|
| Описание ошибки| Сертификат для подписи не настроен для стороны AS2 </br>(AS2-From: partner1 AS2-To: partner2) | 
| Рекомендуемые действия | Настройте параметры соглашения AS2 с помощью правильного сертификата для подписи. |
|  |  | 

## <a name="x12-and-edifact"></a>X12 и EDIFACT

### <a name="leading-or-trailing-space-found"></a>Лишний начальный или конечный пробел    
    
|   |   | 
|---|---|
| Описание ошибки | Ошибка, обнаруженная во время синтаксического анализа. В наборе транзакций EDIFACT с идентификатором 123456, хранящимся в заголовке обмена (без группы) с идентификатором 987654, а также в идентификаторе отправителя Partner1 и получателя Partner2 обнаружена следующая ошибка: <p>Leading Trailing separator found (Обнаружен начальный и конечный разделитель). |
| Рекомендуемые действия | Разрешите начальный и конечный пробелы </br>в параметрах соглашения. |
|   |   |

![Разрешение пробелов](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="duplicate-check-has-enabled-in-the-agreement"></a>В соглашении включены повторяющиеся параметры проверки

|   |   | 
|---|---| 
| Описание ошибки | Повторяющиеся контрольные номера. |
| Рекомендуемые действия | Эта ошибка означает, что полученное сообщение содержит повторяющиеся контрольные номера. </br>Исправьте контрольные номера и повторно отправьте сообщение. |
|   |   |

### <a name="missing-schema-in-the-agreement"></a>В соглашении отсутствует схема

|   |   | 
|---|---| 
| Описание ошибки | Ошибка, обнаруженная во время синтаксического анализа. Обработка набора транзакций X12 с ИД 564220001, содержащегося в функциональной группе с ИД 56422 в сеансе обмена с ИД 000056422, ИД отправителя 12345678 и ИД получателя 87654321, приостановлена из-за следующих ошибок: <p>Неизвестный тип документов в сообщении, который не сопоставляется со всеми имеющимися схемами в соглашении. |
| Рекомендуемые действия | Настройте схему в параметрах соглашения.  |
|   |   |

### <a name="incorrect-schema-in-the-agreement"></a>Неправильная схема соглашения

|   |   | 
|---|---| 
| Описание ошибки | Неизвестный тип документов в сообщении, который не сопоставляется со всеми имеющимися схемами в соглашении. |
| Рекомендуемые действия | Настройте схему в параметрах соглашения. |
|   |   |

## <a name="flat-file"></a>Неструктурированный файл

### <a name="input-message-with-no-body"></a>Входящее сообщение без текста

|   |   | 
|---|---|
| Описание ошибки | Недопустимый шаблон. Не удалось обработать выражения языка шаблона в действии «Flat_File_Decoding» входных данных в строке "1" и столбце "1902": "Требуется свойство «content» ожидает значение, однако получен null. Путь ''.'. |
| Рекомендуемые действия | Эта ошибка означает, что входящее сообщение не содержит текста. |
|   |   | 

