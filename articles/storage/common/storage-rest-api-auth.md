---
title: Вызов операций API REST служб хранилища Azure (включая аутентификацию) | Документация Майкрософт
description: Вызов операций API REST служб хранилища Azure (включая аутентификацию)
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 38a120747734cbe4af8804a3e7596fc11a2c2eb3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66306663"
---
# <a name="using-the-azure-storage-rest-api"></a>Использование REST API службы хранилища Azure

В этой статье показано, как использовать интерфейсы API REST хранилища BLOB-объектов и способы аутентификации вызова службы. Написана с точки зрения разработчика, который ничего не знает о REST и не знает как сделать вызов REST. Мы рассмотрим справочную документацию по вызову REST и посмотрим, с помощью каких полей можно сделать реальный вызов REST. Изучив, как настроить вызов REST, вы сможете использовать эти знания для использования любых других интерфейсов API REST службы хранения.

## <a name="prerequisites"></a>Технические условия 

Приложение перечисляет контейнеры в хранилище BLOB-объектов учетной записи хранения. Чтобы выполнить код, описанный в этой статье, необходимо следующее. 

* Установка [Visual Studio 2019](https://www.visualstudio.com/visual-studio-homepage-vs.aspx) с указанной ниже рабочей нагрузкой:
    - Разработка для Azure

* Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), прежде чем начинать работу.

* Учетная запись хранения общего назначения. Если у вас еще нет учетной записи хранения Azure, [создайте ее](storage-quickstart-create-account.md).

* Пример в этой статье показывает, как перечислять контейнеры в учетной записи хранения. Для просмотра выходных данных перед началом работы добавьте несколько контейнеров в хранилище BLOB-объектов в учетной записи хранения.

## <a name="download-the-sample-application"></a>Загрузка примера приложения

Образец приложения представляет собой консольное приложение, написанное на языке C#.

Используйте команду [git](https://git-scm.com/), чтобы скачать копию приложения в среду разработки. 

```bash
git clone https://github.com/Azure-Samples/storage-dotnet-rest-api-with-auth.git
```

Эта команда клонирует репозиторий в локальную папку git. Чтобы открыть решение Visual Studio, найдите папку storage-dotnet-rest-api-with-auth, откройте ее и дважды щелкните файл StorageRestApiAuth.sln. 

## <a name="what-is-rest"></a>Что такое REST?

REST означает *передачу репрезентативного состояния*. Точное определение см. в [Википедии](https://en.wikipedia.org/wiki/Representational_state_transfer).

По сути, REST — это архитектура, которую вы можете использовать, чтобы вызвать API-интерфейсы или сделать так, чтобы их можно было вызвать. Она не зависит от того, что происходит с обеих сторон и какое программное обеспечение используется при отправке или получении REST вызывает. Вы можете написать приложение, которое работает на Mac, Windows, Linux, телефоне или планшете Android, iPhone, iPod или веб-сайте и использовать один и тот же REST API для всех этих платформ. При вызове API REST данные можно передавать в обе стороны. Не имеет значения, на какой платформе вызывается REST API. Важны лишь передаваемая в запросе информация и предоставляемые в ответе данные.

Знание, как использовать REST, является полезным навыком. Группа разработки Azure часто выпускает новые функции. Во многих случаях новые возможности, доступные через интерфейс REST. В некоторых случаях, функции, которые еще не доступны через **все** клиентских библиотек службы хранилища или пользовательского интерфейса (например, портал Azure). Если вы всегда хотите использовать лучшие, новейшие функции, вам потребуется REST. Кроме того, если вы планируете написать собственную библиотеку для взаимодействия со службой хранилища Azure или вы хотите получить доступ к хранилищу Azure с помощью языка программирования, для которого нет клиентской библиотеки хранилища или пакета SDK, вы можете использовать API REST.

## <a name="about-the-sample-application"></a>Сведения о примере приложения

Пример приложения содержит список контейнеров в учетной записи хранения. После того, как вы поймете, как сведения в руководстве коррелируют с фактическим кодом, другие вызовы REST будет легче понять. 

Если вы посмотрите на [API REST службы BLOB-объектов](/rest/api/storageservices/Blob-Service-REST-API), то увидите, что все операции можно выполнять в хранилище BLOB-объектов. Библиотеки клиентских хранилищ представляют собой программы-оболочки вокруг API REST, которые упрощают доступ к хранилищу без прямого использования API REST. Но, как указано выше, иногда требуется использовать интерфейс API REST вместо клиентской библиотеки хранилища.

## <a name="rest-api-reference-list-containers-api"></a>Справочник по REST API. Список API контейнеров

Давайте взглянем на странице в справочнике по REST API для [ListContainers](/rest/api/storageservices/List-Containers2) операции. Эта информация поможет вам понять, откуда некоторые поля берутся в запросе и ответе.

**Метод запроса**: GET. Эта команда является методом HTTP, который вы указываете как свойство объекта запроса. Другие значения для этой команды включают в себя: HEAD, PUT и DELETE, в зависимости от API, который вы вызываете.

**URI запроса**: https://myaccount.blob.core.windows.net/?comp=list. Создается из конечной точки учетной записи хранения BLOB-объектов `http://myaccount.blob.core.windows.net` и `/?comp=list` строки ресурса.

[Параметры URI](/rest/api/storageservices/List-Containers2#uri-parameters): существуют дополнительные параметры запроса, которые вы можете использовать при вызове ListContainers. Некоторыми из этих параметров являются *timeout* для вызова (в секундах) и *prefix*, который используется для фильтрации.

Другим полезным параметром является *maxresults*. Если доступно больше контейнеров, чем это значение, текст ответа будет содержать элемент *NextMarker*, который указывает, что следующий контейнер будет возвращен при следующем запросе. Чтобы использовать эту функцию, следует указать *NextMarker* в качестве параметра *маркера* в URI при выполнении следующего запроса. Использование этой функции аналогично постраничному просмотру результатов. 

Чтобы использовать дополнительные параметры, добавьте их в строку ресурса со значением, например, в этом примере:

```
/?comp=list&timeout=60&maxresults=100
```

[Заголовки запроса](/rest/api/storageservices/List-Containers2#request-headers) **:** в этом разделе перечислены обязательные и необязательные заголовки запросов. Три заголовка являются обязательными: *Authorization*, *x-ms-date* (содержит время запроса в формате UTC) и *x-ms-version* (версия интерфейса REST API для использования). Включать *x-ms-client-request-id* в заголовки необязательно. Вы можете установить любое значение в это поле. Оно записывается в журналы аналитики хранилища при включении ведения журнала.

[Текст запроса](/rest/api/storageservices/List-Containers2#request-body) **:** текст запроса для ListContainers отсутствует. Текст запроса используется во всех операциях PUT при загрузке больших двоичных объектов, а также в SetContainerAccessPolicy, что позволяет вам отправлять список сохраненных политик в формате XML, которые требуется применить. Хранимые политики доступа рассматриваются в статье [Использование подписанных URL-адресов (SAS)](storage-dotnet-shared-access-signature-part-1.md).

[Код состояния отклика](/rest/api/storageservices/List-Containers2#status-code) **:** сообщает о любых кодах состояния, о которых вам нужно знать. В этом примере кодом состояния HTTP является 200 — это нормально. Полный список кодов состояния HTTP см. [на этой странице](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html). Коды ошибок, относящихся к хранилищу интерфейсов REST API, см. в статье [Common REST API Error Codes](/rest/api/storageservices/common-rest-api-error-codes) (Общие коды ошибок API-интерфейса REST).

[Заголовки ответов](/rest/api/storageservices/List-Containers2#response-headers) **:** К ним относятся *тип содержимого*; *x-ms-request-id*, который является идентификатором запроса, переданного в; *x-ms-version*, который указывает версию службы BLOB-объектов, используемой; и *даты*, который указывается в формате UTC и о том, что время запроса был сделан.

[Текст ответа](/rest/api/storageservices/List-Containers2#response-body): Это поле является XML-структурой, предоставляющей запрашиваемые данные. В этом примере ответ представляет собой список контейнеров и их свойств.

## <a name="creating-the-rest-request"></a>Создание запроса REST

Несколько примечаний перед началом работы: для обеспечения безопасности при запуске в рабочей среде всегда используйте HTTPS вместо HTTP. Для целей этого упражнения вы должны использовать HTTP, чтобы просмотреть данные запроса и ответа. Чтобы просмотреть сведения запроса и ответа в фактических вызовах REST, загрузите [Fiddler](https://www.telerik.com/fiddler) или похожее приложение. В Visual Studio решения, имя учетной записи хранения и ключ «зашиты» в классе. Метод ListContainersAsyncREST передает имя учетной записи хранения и ключ учетной записи хранения в методы, которые используются для создания различных компонентов запроса REST. В случае реального приложения имя и ключ учетной записи хранения будут находиться в файле конфигурации, переменных среды или извлекаться из хранилища Azure Key Vault.

В нашем примерном проекте код для создания заголовка авторизации находится в отдельном классе. Идея заключается в том, что может весь класс и добавьте его в свое собственное решение и использовать его «как есть». Код заголовка авторизации работает для большинства вызовов REST API в хранилище Azure.

Чтобы создать запрос, который является объектом HttpRequestMessage, перейдите в ListContainersAsyncREST в Program.cs. Для создания запроса, необходимо: 

* Создайте URI, используемый для вызова службы. 
* Создайте объект HttpRequestMessage и настройте полезную нагрузку. Полезная нагрузка равна нулю для ListContainersAsyncREST, потому что мы ничего не передаем.
* Добавьте заголовки запросов для x-ms-date и x-ms-version.
* Получите заголовок авторизации и добавьте его.

Необходимые базовые сведения: 

*  Для ListContainers **методом** является `GET`. Это значение устанавливается при создании экземпляра запроса. 
*  **Ресурс** является частью запроса URI, который указывает на вызываемый API, поэтому значением будет `/?comp=list`. Как отмечалось ранее, ресурс находится на странице справочной документации о [ListContainers API](/rest/api/storageservices/List-Containers2).
*  URI создается путем создания конечной точки службы BLOB-объектов для этой учетной записи хранения и сцепки ресурсов. Значением для **URI запроса** будет `http://contosorest.blob.core.windows.net/?comp=list`.
*  Для ListContainers **requestBody** имеет значение null и не имеет дополнительных **заголовков**.

Различные интерфейсы API могут передавать другие параметры, например *ifMatch*. Пример того, где вы можете использовать ifMatch, — это вызов PutBlob. В этом случае установите ifMatch для тега сущности, и он обновит большой двоичный объект только в том случае, если тег сущности, который вы предоставляете, соответствует текущему тегу в большом двоичном объекте. Если кто-то другой обновил большой двоичный объект с момента получения тега сущности, изменение не будет переопределено. 

Сначала следует задать `uri` и `payload`. 

```csharp
// Construct the URI. This will look like this:
//   https://myaccount.blob.core.windows.net/resource
String uri = string.Format("http://{0}.blob.core.windows.net?comp=list", storageAccountName);

// Set this to whatever payload you desire. Ours is null because 
//   we're not passing anything in.
Byte[] requestPayload = null;
```

Затем создайте экземпляр запроса, установив в качестве метода `GET` и предоставив URI.

```csharp 
//Instantiate the request message with a null payload.
using (var httpRequestMessage = new HttpRequestMessage(HttpMethod.Get, uri)
{ Content = (requestPayload == null) ? null : new ByteArrayContent(requestPayload) })
{
```

Добавьте заголовки запросов для x-ms-date и x-ms-version. В этом месте кода также можно добавить дополнительные заголовки запросов, необходимые для вызова. В этом примере нет дополнительных заголовков. Примером API, который передается в дополнительных заголовках, является SetContainerACL. Для хранилища BLOB-объектов он добавляет заголовок, называемый x-ms-blob-public-access, и значение для уровня доступа.

```csharp
    // Add the request headers for x-ms-date and x-ms-version.
    DateTime now = DateTime.UtcNow;
    httpRequestMessage.Headers.Add("x-ms-date", now.ToString("R", CultureInfo.InvariantCulture));
    httpRequestMessage.Headers.Add("x-ms-version", "2017-07-29");
    // If you need any additional headers, add them here before creating
    //   the authorization header. 
```

Вызовите метод, который создает заголовок авторизации и добавьте его в заголовки запросов. Сведения о том, как создать заголовок авторизации, приведены далее в этой статье. Именем метода является GetAuthorizationHeader, которое можно увидеть в этом фрагменте кода:

```csharp
    // Get the authorization header and add it.
    httpRequestMessage.Headers.Authorization = AzureStorageAuthenticationHelper.GetAuthorizationHeader(
        storageAccountName, storageAccountKey, now, httpRequestMessage);
```

На этом этапе `httpRequestMessage` содержит запрос REST с заголовками авторизации. 

## <a name="call-the-rest-api-with-the-request"></a>Вызов REST API с помощью запроса

Теперь, когда у вас есть запрос, вы можете вызвать SendAsync, чтобы отправить запрос REST. SendAsync вызывает API и возвращает ответ. Изучите ответ StatusCode (200 является нормой),а затем проанализируйте ответ. В этом случае можно получить XML-список контейнеров. Давайте рассмотрим код вызова метода GetRESTRequest, после чего создадим запрос, выполним его и изучим ответ со списком контейнеров.

```csharp 
    // Send the request.
    using (HttpResponseMessage httpResponseMessage = 
      await new HttpClient().SendAsync(httpRequestMessage, cancellationToken))
    {
        // If successful (status code = 200), 
        //   parse the XML response for the container names.
        if (httpResponseMessage.StatusCode == HttpStatusCode.OK)
        {
            String xmlString = await httpResponseMessage.Content.ReadAsStringAsync();
            XElement x = XElement.Parse(xmlString);
            foreach (XElement container in x.Element("Containers").Elements("Container"))
            {
                Console.WriteLine("Container name = {0}", container.Element("Name").Value);
            }
        }
    }
}
```

Если во время вызова SendAsync вы запускаете средство прослушивания сети, например [Fiddler](https://www.telerik.com/fiddler), вы можете увидеть сведения о запросе и ответе. Давай посмотрим. Имя учетной записи хранения — *contosorest*.

**Запрос:**

```
GET /?comp=list HTTP/1.1
```

**Заголовки запросов:**

```
x-ms-date: Thu, 16 Nov 2017 23:34:04 GMT
x-ms-version: 2014-02-14
Authorization: SharedKey contosorest:1dVlYJWWJAOSHTCPGiwdX1rOS8B4fenYP/VrU0LfzQk=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Код состояния и заголовки ответов возвращены после выполнения:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 3e889876-001e-0039-6a3a-5f4396000000
x-ms-version: 2017-07-29
Date: Fri, 17 Nov 2017 00:23:42 GMT
Content-Length: 1511
```

**Текст ответа (XML):** для ListContainers отображает список контейнеров и их свойства.

```xml  
<?xml version="1.0" encoding="utf-8"?>
<EnumerationResults 
  ServiceEndpoint="http://contosorest.blob.core.windows.net/">
  <Containers>
    <Container>
      <Name>container-1</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:39:48 GMT</Last-Modified>
        <Etag>"0x8D46CBD5A7C301D"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-2</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:40:50 GMT</Last-Modified>
        <Etag>"0x8D46CBD7F49E9BD"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-3</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:41:10 GMT</Last-Modified>
        <Etag>"0x8D46CBD8B243D68"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
      </Properties>
    </Container>
    <Container>
      <Name>container-4</Name>
      <Properties>
        <Last-Modified>Thu, 16 Mar 2017 22:41:25 GMT</Last-Modified>
        <Etag>"0x8D46CBD93FED46F"</Etag>
        <LeaseStatus>unlocked</LeaseStatus>
        <LeaseState>available</LeaseState>
        </Properties>
      </Container>
      <Container>
        <Name>container-5</Name>
        <Properties>
          <Last-Modified>Thu, 16 Mar 2017 22:41:39 GMT</Last-Modified>
          <Etag>"0x8D46CBD9C762815"</Etag>
          <LeaseStatus>unlocked</LeaseStatus>
          <LeaseState>available</LeaseState>
        </Properties>
      </Container>
  </Containers>
  <NextMarker />
</EnumerationResults>
```

Теперь, когда вы знаете, как создавать запросы, вызывать службу и анализировать результаты, давайте посмотрим, как создать заголовок авторизации. Создать этот заголовок сложно, но он хорошо работает при наличии рабочего кода для всех интерфейсов REST API службы хранилища.

## <a name="creating-the-authorization-header"></a>Создание заголовка авторизации

> [!TIP]
> Служба хранилища Azure теперь поддерживает интеграцию Azure Active Directory (Azure AD) для больших двоичных объектов и очередей. Azure AD предоставляет гораздо более простой процесс авторизации запроса к службе хранилища Azure. Дополнительные сведения об использовании Azure AD для авторизации операций REST см. в разделе [проверка подлинности с помощью Azure Active Directory](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory). Обзор Azure AD интеграция со службой хранилища Azure, см. в разделе [проверки подлинности при доступе к службе хранилища Azure с помощью Azure Active Directory](storage-auth-aad.md).

Существует статья, в которой концептуально (без кода) объясняется, как выполнить [аутентификацию службы хранилища Azure](/rest/api/storageservices/Authorization-for-the-Azure-Storage-Services).
Давайте уточним, что необходимо, и посмотрим на код.

Во-первых, используйте аутентификацию на основе общего ключа. Формат заголовка авторизации выглядит следующим образом:

```  
Authorization="SharedKey <storage account name>:<signature>"  
```

Поле подписи — это код аутентификации сообщения на основе хэш-кода (HMAC), созданный из запроса и вычисленный с использованием алгоритма SHA256, а затем закодированный с использованием кодировки Base64. Все понятно? (Держитесь, это мы еще не дошли до термина *канонический*.)

Этот фрагмент кода отображает формат строки подписи общего ключа:

```csharp  
StringToSign = VERB + "\n" +  
               Content-Encoding + "\n" +  
               Content-Language + "\n" +  
               Content-Length + "\n" +  
               Content-MD5 + "\n" +  
               Content-Type + "\n" +  
               Date + "\n" +  
               If-Modified-Since + "\n" +  
               If-Match + "\n" +  
               If-None-Match + "\n" +  
               If-Unmodified-Since + "\n" +  
               Range + "\n" +  
               CanonicalizedHeaders +  
               CanonicalizedResource;  
```

Большинство этих полей используются редко. Для хранилища BLOB-объектов вы указываете VERB, md5, длину содержимого, канонические заголовки и канонический ресурс. Вы можете оставить остальные пустыми (но введите `\n`, чтобы он знал, что они пусты).

Что такое CanonicalizedHeaders и CanonicalizedResource? Хороший вопрос. На самом деле, что означает "канонический"? В нашем контексте это слово никак не связано с церковью. Сведения, приведенные в [англоязычной Википедии о канонизации](https://en.wikipedia.org/wiki/Canonicalization). *В информатике канонизация (иногда стандартизация или нормализация) — это процесс преобразования данных, которые имеют несколько возможных представлений, в "стандартную", "нормальную", "каноническую" форму.* Проще говоря, — это означает взять список элементов (например, заголовки в случае канонизированных заголовков) и стандартизовать их в требуемом формате. По сути корпорация Майкрософт выбрала этот формат, и вам необходимо соответствовать ему.

Начнем с тех двух канонических полей, потому что они необходимы для создания заголовка авторизации.

**Канонические заголовки**

Чтобы создать это значение, загрузите заголовки, начинающиеся с x-ms-, и отсортируйте их, а затем отформатируйте их в строку экземпляров `[key:value\n]`, объединенных в одну строку. В этом примере канонические заголовки будут выглядеть следующим образом: 

```
x-ms-date:Fri, 17 Nov 2017 00:44:48 GMT\nx-ms-version:2017-07-29\n
```

Вот код, используемый для создания этих выходных данных:

```csharp 
private static string GetCanonicalizedHeaders(HttpRequestMessage httpRequestMessage)
{
    var headers = from kvp in httpRequestMessage.Headers
        where kvp.Key.StartsWith("x-ms-", StringComparison.OrdinalIgnoreCase)
        orderby kvp.Key
        select new { Key = kvp.Key.ToLowerInvariant(), kvp.Value };

    StringBuilder headersBuilder = new StringBuilder();

    // Create the string in the right format; this is what makes the headers "canonicalized" --
    //   it means put in a standard format. https://en.wikipedia.org/wiki/Canonicalization
    foreach (var kvp in headers)
    {
        headersBuilder.Append(kvp.Key);
        char separator = ':';

        // Get the value for each header, strip out \r\n if found, then append it with the key.
        foreach (string headerValue in kvp.Value)
        {
            string trimmedValue = headerValue.TrimStart().Replace("\r\n", string.Empty);
            headersBuilder.Append(separator).Append(trimmedValue);

            // Set this to a comma; this will only be used
            // if there are multiple values for one of the headers.
            separator = ',';
        }

        headersBuilder.Append("\n");
    }

    return headersBuilder.ToString();
}
```

**Канонический ресурс**

Эта часть строки подписи представляет собой учетную запись хранения, предназначенную для запроса. Помните, что URI запроса является `<http://contosorest.blob.core.windows.net/?comp=list>` с именем фактической учетной записи (в данном случае `contosorest`). В этом примере возвращается следующее:

```
/contosorest/\ncomp:list
```

Если у вас есть параметры запроса, этот пример включает эти параметры также. Вот код, который также обрабатывает дополнительные параметры запроса и параметры запроса с несколькими значениями. Помните, что вы создаете следующий код будет работать для всех интерфейсов API REST. Вы хотите включить все возможности, даже если методу ListContainers требуются не все из них.

```csharp 
private static string GetCanonicalizedResource(Uri address, string storageAccountName)
{
    // The absolute path will be "/" because for we're getting a list of containers.
    StringBuilder sb = new StringBuilder("/").Append(storageAccountName).Append(address.AbsolutePath);

    // Address.Query is the resource, such as "?comp=list".
    // This ends up with a NameValueCollection with 1 entry having key=comp, value=list.
    // It will have more entries if you have more query parameters.
    NameValueCollection values = HttpUtility.ParseQueryString(address.Query);

    foreach (var item in values.AllKeys.OrderBy(k => k))
    {
        sb.Append('\n').Append(item).Append(':').Append(values[item]);
    }

    return sb.ToString();
}
```

Теперь, когда канонические строки заданы, давайте посмотрим, как создать сам заголовок авторизации. Сначала нужно создать строку подписи сообщения в формате StringToSign, ранее описанную в этой статье. Эту концепцию легче объяснить с помощью комментариев в коде. Ниже приведен окончательный метод, который возвращает заголовок авторизации:

```csharp
internal static AuthenticationHeaderValue GetAuthorizationHeader(
    string storageAccountName, string storageAccountKey, DateTime now,
    HttpRequestMessage httpRequestMessage, string ifMatch = "", string md5 = "")
{
    // This is the raw representation of the message signature.
    HttpMethod method = httpRequestMessage.Method;
    String MessageSignature = String.Format("{0}\n\n\n{1}\n{5}\n\n\n\n{2}\n\n\n\n{3}{4}",
                method.ToString(),
                (method == HttpMethod.Get || method == HttpMethod.Head) ? String.Empty
                  : httpRequestMessage.Content.Headers.ContentLength.ToString(),
                ifMatch,
                GetCanonicalizedHeaders(httpRequestMessage),
                GetCanonicalizedResource(httpRequestMessage.RequestUri, storageAccountName),
                md5);

    // Now turn it into a byte array.
    byte[] SignatureBytes = Encoding.UTF8.GetBytes(MessageSignature);

    // Create the HMACSHA256 version of the storage key.
    HMACSHA256 SHA256 = new HMACSHA256(Convert.FromBase64String(storageAccountKey));

    // Compute the hash of the SignatureBytes and convert it to a base64 string.
    string signature = Convert.ToBase64String(SHA256.ComputeHash(SignatureBytes));

    // This is the actual header that will be added to the list of request headers.
    AuthenticationHeaderValue authHV = new AuthenticationHeaderValue("SharedKey",
        storageAccountName + ":" + Convert.ToBase64String(SHA256.ComputeHash(SignatureBytes)));
    return authHV;
}
```

При выполнении этого кода, полученный MessageSignature выглядит следующим образом:

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 01:07:37 GMT\nx-ms-version:2017-07-29\n/contosorest/\ncomp:list
```

Вот конечное значение для AuthorizationHeader:

```
SharedKey contosorest:Ms5sfwkA8nqTRw7Uury4MPHqM6Rj2nfgbYNvUKOa67w=
```

AuthorizationHeader — это последний заголовок, помещенный в заголовки запроса перед отправкой ответа.

Содержащий все, что нужно знать, чтобы собрать класс, с помощью которого можно создать запрос для вызова API REST служб хранилища.

## <a name="how-about-another-example"></a>Вот еще один пример: 

Давайте рассмотрим, как изменить код, чтобы вызвать ListBlobs для контейнера *container-1*. Этот код почти идентичен коду для перечисления контейнеров, единственными отличиями которого являются URI и как вы анализируете ответ. 

Если взглянуть на справочную документацию по [ListBlobs](/rest/api/storageservices/List-Blobs), обнаружится, что методом является *GET*, а свойством RequestURI является:

```
https://myaccount.blob.core.windows.net/container-1?restype=container&comp=list
```

В ListContainersAsyncREST измените код, который устанавливает URI в API для ListBlob. Имя контейнера — **container-1**.

```csharp
String uri = 
    string.Format("http://{0}.blob.core.windows.net/container-1?restype=container&comp=list",
      storageAccountName);

```

Затем, там где вы обрабатываете ответ, измените код, чтобы найти BLOB-объекты вместо контейнеров.

```csharp
foreach (XElement container in x.Element("Blobs").Elements("Blob"))
{
    Console.WriteLine("Blob name = {0}", container.Element("Name").Value);
}
```

При запуске этого примера можно получить следующие результаты:

**Канонические заголовки:**

```
x-ms-date:Fri, 17 Nov 2017 05:16:48 GMT\nx-ms-version:2017-07-29\n
```

**Канонический ресурс:**

```
/contosorest/container-1\ncomp:list\nrestype:container
```

**MessageSignature:**

```
GET\n\n\n\n\n\n\n\n\n\n\n\nx-ms-date:Fri, 17 Nov 2017 05:16:48 GMT
  \nx-ms-version:2017-07-29\n/contosorest/container-1\ncomp:list\nrestype:container
```

**AuthorizationHeader:**

```
SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
```

В [Fiddler](https://www.telerik.com/fiddler) доступны следующие значения:

**Запрос:**

```
GET http://contosorest.blob.core.windows.net/container-1?restype=container&comp=list HTTP/1.1
```

**Заголовки запросов:**

```
x-ms-date: Fri, 17 Nov 2017 05:16:48 GMT
x-ms-version: 2017-07-29
Authorization: SharedKey contosorest:uzvWZN1WUIv2LYC6e3En10/7EIQJ5X9KtFQqrZkxi6s=
Host: contosorest.blob.core.windows.net
Connection: Keep-Alive
```

**Код состояния и заголовки ответов возвращены после выполнения:**

```
HTTP/1.1 200 OK
Content-Type: application/xml
Server: Windows-Azure-Blob/1.0 Microsoft-HTTPAPI/2.0
x-ms-request-id: 7e9316da-001e-0037-4063-5faf9d000000
x-ms-version: 2017-07-29
Date: Fri, 17 Nov 2017 05:20:21 GMT
Content-Length: 1135
```

**Текст ответа (XML):** этот ответ XML отображает список BLOB-объектов и их свойства. 

```xml
<?xml version="1.0" encoding="utf-8"?>
<EnumerationResults 
    ServiceEndpoint="http://contosorest.blob.core.windows.net/" ContainerName="container-1">
    <Blobs>
        <Blob>
            <Name>DogInCatTree.png</Name>
            <Properties><Last-Modified>Fri, 17 Nov 2017 01:41:14 GMT</Last-Modified>
            <Etag>0x8D52D5C4A4C96B0</Etag>
            <Content-Length>419416</Content-Length>
            <Content-Type>image/png</Content-Type>
            <Content-Encoding />
            <Content-Language />
            <Content-MD5 />
            <Cache-Control />
            <Content-Disposition />
            <BlobType>BlockBlob</BlobType>
            <LeaseStatus>unlocked</LeaseStatus>
            <LeaseState>available</LeaseState>
            <ServerEncrypted>true</ServerEncrypted>
            </Properties>
        </Blob>
        <Blob>
            <Name>GuyEyeingOreos.png</Name>
            <Properties>
                <Last-Modified>Fri, 17 Nov 2017 01:41:14 GMT</Last-Modified>
                <Etag>0x8D52D5C4A25A6F6</Etag>
                <Content-Length>167464</Content-Length>
                <Content-Type>image/png</Content-Type>
                <Content-Encoding />
                <Content-Language />
                <Content-MD5 />
                <Cache-Control />
                <Content-Disposition />
                <BlobType>BlockBlob</BlobType>
                <LeaseStatus>unlocked</LeaseStatus>
                <LeaseState>available</LeaseState>
                <ServerEncrypted>true</ServerEncrypted>
            </Properties>
            </Blob>
        </Blobs>
    <NextMarker />
</EnumerationResults>
```

## <a name="summary"></a>Сводка

В этой статье вы узнали, как создать запрос к REST API хранилища BLOB-объектов. В запросе можно получить список контейнеров или список больших двоичных объектов в контейнере. Вы узнали, как создать подпись авторизации для вызова REST API и способы ее использования в запросе REST. Наконец вы узнали, как проверить ответ.

## <a name="next-steps"></a>Дальнейшие действия

* [Blob Service REST API](/rest/api/storageservices/blob-service-rest-api) (API-интерфейс REST службы BLOB-объектов)
* [File Service REST API](/rest/api/storageservices/file-service-rest-api) (API-интерфейс REST файловой службы)
* [REST API службы очередей](/rest/api/storageservices/queue-service-rest-api)
