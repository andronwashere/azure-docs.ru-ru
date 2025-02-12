---
title: Аутентификация, запросы и ответы
description: Проверка подлинности в AD для использования Key Vault
services: key-vault
author: msmbaldwin
manager: barbkess
tags: azure-resource-manager
ms.service: key-vault
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: mbaldwin
ms.openlocfilehash: 4160d6ce324cf419cd4b9a61b68bb39b0443321c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64694740"
---
# <a name="authentication-requests-and-responses"></a>Аутентификация, запросы и ответы

Azure Key Vault поддерживает запросы и ответы в формате JSON. Запросы к Azure Key Vault направляются на допустимый URL-адрес Azure Key Vault с помощью HTTPS с некоторыми параметрами URL-адреса и текстом запроса и ответа в формате JSON.

Здесь рассматривается служба Azure Key Vault. Общие сведения об использовании интерфейсов Azure REST, в том числе об аутентификации и авторизации, а также о том, как получить маркер доступа, см. в [справочнике по Azure REST API](https://docs.microsoft.com/rest/api/azure).

## <a name="request-url"></a>Request URL (URL-адрес запроса)  
 Операции управления ключами используют команды HTTP DELETE, GET, PATCH, PUT и HTTP POST, а криптографические операции с существующими объектами ключей используют HTTP POST. Клиенты, не поддерживающие определенные HTTP-команды, также могут использовать HTTP POST, указав нужную команду в заголовке X-HTTP-REQUEST. Запросы, которым обычно не требуется текст, должны включать пустой текст при использовании HTTP POST, например при использовании команды POST вместо DELETE.  

 Ниже приведены примеры URL-адресов для работы с объектами в Azure Key Vault.  

- Чтобы создать ключ под названием TESTKEY в Key Vault, используйте команду `PUT /keys/TESTKEY?api-version=<api_version> HTTP/1.1`  

- Чтобы импортировать ключ под названием IMPORTEDKEY в Key Vault, используйте команду `POST /keys/IMPORTEDKEY/import?api-version=<api_version> HTTP/1.1`  

- Чтобы получить секрет под названием MYSECRET в Key Vault, используйте команду `GET /secrets/MYSECRET?api-version=<api_version> HTTP/1.1`  

- Чтобы подписать хэш-код с помощью ключа под названием TESTKEY в Key Vault, используйте команду `POST /keys/TESTKEY/sign?api-version=<api_version> HTTP/1.1`  

  Объект авторизации для запроса к Key Vault всегда выглядит следующим образом: `https://{keyvault-name}.vault.azure.net/`  

  Ключи всегда хранятся в пути /keys, а секреты всегда хранятся в пути /secrets.  

## <a name="api-version"></a>Версия API  
 Служба Azure Key Vault поддерживает управление версиями протоколов для обеспечения совместимости с клиентами нижнего уровня, хотя этим клиентам будут доступны не все возможности. Клиенты должны использовать параметр строки запроса `api-version`, чтобы указать поддерживаемую версию протокола, так как значение по умолчанию отсутствует.  

 Версии протокола Azure Key Vault следуют схеме нумерации дат в формате {ГГГГ}.{ММ}.{ДД}.  

## <a name="request-body"></a>Текст запроса  
 Согласно спецификации протокола HTTP операции GET не должны иметь текст запроса, а операции POST и PUT должны. Текст в операциях DELETE является необязательным в HTTP.  

 Если иное не указано в описании операции, то тип содержимого текста запроса должен быть "application/json" и текст должен содержать сериализованный объект JSON, совместимый с типом содержимого.  

 Если иное не указано в описании операции, заголовок запроса Accept должен содержать тип носителя "application/json".  

## <a name="response-body"></a>Текст ответа  
 Если иное не указано в описании операции, типом содержимого для успешных и неудачных операций будет "application/json", а текст запроса будет содержать подробные сведения об ошибках.  

## <a name="using-http-post"></a>Использование HTTP POST  
 Некоторые клиенты могут не иметь возможность использовать определенные HTTP-команды, например PATCH или DELETE. В качестве альтернативы для таких клиентов Azure Key Vault поддерживает HTTP POST. При этом клиент также должен включить заголовок X-HTTP-METHOD для определенной исходной HTTP-команды. Поддержка HTTP POST указана для каждого из API в этом документе.  

## <a name="error-responses"></a>Сообщения об ошибках  
 Для обработки ошибок используются коды состояния HTTP. Ниже приведены примеры типичных результатов.  

- 2xx: Success (успешно) — используется для нормальной работы. Текст ответа будет содержать ожидаемый результат.  

- 3xx: Redirection (перенаправление) — ответ 304 Not Modified (объект не изменялся) может быть возвращен для выполнения условной операции GET. Другие коды 3xx могут использоваться в дальнейшем для указания изменений DNS и пути.  

- 4xx: Client Error (ошибка клиента) — используется для неправильных запросов, отсутствующих ключей, синтаксических ошибок, недопустимых параметров, ошибок проверки подлинности и т. д. Текст ответа будет содержать подробные сведения об ошибке.  

- 5xx: Server Error (ошибка сервера) — используется для внутренних ошибок сервера. Текст ответа будет содержать сводные сведения об ошибке.  

  Система должна работать с использованием прокси-сервера или брандмауэра. Таким образом, клиент может получать другие коды ошибок.  

  Azure Key Vault также возвращает сведения об ошибке в тексте ответа при возникновении проблемы. Текст ответа возвращается в формате JSON и имеет такой вид:  

```  

{  
  "error":  
  {  
    "code": "BadArgument",  
    "message":  

      "’Foo’ is not a valid argument for ‘type’."  
    }  
  }  
}  

```  

## <a name="authentication"></a>Authentication  
 Все запросы к Azure Key Vault должны пройти проверку подлинности. Azure Key Vault поддерживает маркеры доступа Azure Active Directory, которые могут быть получены с помощью OAuth2 [[RFC6749](https://tools.ietf.org/html/rfc6749)]. 
 
 Дополнительные сведения о регистрации приложения и выполнении проверки подлинности для использования Azure Key Vault см. в статье о [регистрации клиентского приложения в Azure AD](https://docs.microsoft.com/rest/api/azure/index#register-your-client-application-with-azure-ad).
 
 Маркеры доступа должны отправляться в службу с помощью заголовка авторизации HTTP:  

```  
PUT /keys/MYKEY?api-version=<api_version>  HTTP/1.1  
Authorization: Bearer <access_token>  

```  

 Если маркер доступа не задан или не принимается службой, клиенту будет возвращена ошибка HTTP 401 с заголовком WWW-Authenticate, например:  

```  
401 Not Authorized  
WWW-Authenticate: Bearer authorization="…", resource="…"  

```  

 Параметры в заголовке WWW-Authenticate:  

-   authorization: адрес службы авторизации OAuth2, который может использоваться для получения маркера доступа для запроса.  

-   resource: имя ресурса для использования в запросе авторизации.  

## <a name="see-also"></a>См. также  
 [Сведения о ключах, секретах и сертификатах](about-keys-secrets-and-certificates.md)
