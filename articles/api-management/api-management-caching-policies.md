---
title: Политики кэширования в службе управления API Azure | Документация Майкрософт
description: Сведения о политиках кэширования, доступных для использования в службе управления API Azure.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: 8147199c-24d8-439f-b2a9-da28a70a890c
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/27/2018
ms.author: apimpm
ms.openlocfilehash: 2b74c0d8dc0daa7bb86c2884f5e574fdc579ba44
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442382"
---
# <a name="api-management-caching-policies"></a>Политики кэширования в службе управления API
В этой статье рассматриваются приведенные ниже политики управления API. Дополнительные сведения о добавлении и настройке политик см. в статье о [политиках в управлении API](https://go.microsoft.com/fwlink/?LinkID=398186).

## <a name="CachingPolicies"></a> Политики кэширования

- Политики кэширования ответов
    - [Получение из кэша](api-management-caching-policies.md#GetFromCache) — выполнение поиска в кэше и возврат допустимых кэшированных ответов при их наличии.
    - [Сохранение в кэше](api-management-caching-policies.md#StoreToCache) — помещение ответа в кэш в соответствии с заданной конфигурацией управления кэшем.
- Политики кэширования значений
    - [Получение значения из кэша](#GetFromCacheByKey) — получение кэшированного элемента по ключу.
    - [Сохранение значения в кэше](#StoreToCacheByKey) — сохранение элемента в кэше по ключу.
    - [Удалить значение из кэша](#RemoveCacheByKey) — удаление элемента в кэше по ключу.

## <a name="GetFromCache"></a> Получение из кэша
Политика `cache-lookup` используется для поиска в кэше и возврата допустимого кэшированного ответа при его наличии. Эту политику можно применять в тех случаях, когда содержимое ответа остается неизменным в течение определенного интервала времени. Кэширование ответов уменьшает требования к пропускной способности и вычислительной мощности, накладываемые на внутренний веб-сервер, а также снижает время задержки для потребителей API.

> [!NOTE]
> Одновременно с этой политикой должна быть определена соответствующая политика [сохранения в кэш](api-management-caching-policies.md#StoreToCache).

### <a name="policy-statement"></a>Правило политики

```xml
<cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" caching-type="prefer-external | external | internal" downstream-caching-type="none | private | public" must-revalidate="true | false" allow-private-response-caching="@(expression to evaluate)">
  <vary-by-header>Accept</vary-by-header>
  <!-- should be present in most cases -->
  <vary-by-header>Accept-Charset</vary-by-header>
  <!-- should be present in most cases -->
  <vary-by-header>Authorization</vary-by-header>
  <!-- should be present when allow-private-response-caching is "true"-->
  <vary-by-header>header name</vary-by-header>
  <!-- optional, can repeated several times -->
  <vary-by-query-parameter>parameter name</vary-by-query-parameter>
  <!-- optional, can repeated several times -->
</cache-lookup>
```

### <a name="examples"></a>Примеры

#### <a name="example"></a>Пример

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="none" must-revalidate="true" caching-type="internal" >
            <vary-by-query-parameter>version</vary-by-query-parameter>
        </cache-lookup>
    </inbound>
    <outbound>
        <cache-store duration="seconds" />
        <base />
    </outbound>
</policies>
```

#### <a name="example-using-policy-expressions"></a>Пример с использованием выражений политики
В этом примере показано, как настроить в службе управления API период хранения ответов в кэше, соответствующий длительности кэширования ответа в серверной службе, которая задается директивой `Cache-Control`. Настройка и использование этой политики демонстрируются в видеоролике [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ("Облачная среда", серия 177. Дополнительные возможности Управления API (Влад Виноградский)). Нужные сведения см. с 25:25.

```xml
<!-- The following cache policy snippets demonstrate how to control API Management response cache duration with Cache-Control headers sent by the backend service. -->

<!-- Copy this snippet into the inbound section -->
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >
  <vary-by-header>Accept</vary-by-header>
  <vary-by-header>Accept-Charset</vary-by-header>
</cache-lookup>

<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the default value of 5 min if none is found  -->
<cache-store duration="@{
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;
  }"
 />
```

Чтобы узнать больше, см. статью [API Management policy expressions](api-management-policy-expressions.md) (Выражения политики управления API) и раздел [Context variable](api-management-policy-expressions.md#ContextVariables) (Переменная контекста).

### <a name="elements"></a>Элементы

|Название|Описание|Обязательно|
|----------|-----------------|--------------|
|cache-lookup|Корневой элемент.|Да|
|vary-by-header|Начало кэширования ответов по значению определенного заголовка, например Accept, Accept-Charset, Accept-Encoding, Accept-Language, Authorization, Expect, From, Host, If-Match.|Нет|
|vary-by-query-parameter|Запускается процесс кэширования ответов для значения заданных параметров запроса. Введите один или несколько параметров. В качестве разделителя используйте точку с запятой. Если значение не задано, используются все параметры запроса.|Нет|

### <a name="attributes"></a>Атрибуты

| Имя                           | Описание                                                                                                                                                                                                                                                                                                                                                 | Обязательно | Значение по умолчанию           |
|--------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| allow-private-response-caching | Если задано значение `true`, разрешается кэширование запросов, содержащих заголовок авторизации.                                                                                                                                                                                                                                                                        | Нет       | False             |
| тип кэширования               | Выберите одно из следующих значений атрибута:<br />- `internal` — использование встроенного кэша в службе управления API;<br />- `external` — использование внешнего кэша, как описано в статье [Использование внешнего кэша Redis для Azure в Управлении API Azure](api-management-howto-cache-external.md);<br />- `prefer-external` — использование внешнего кэша, если он настроен. В противном случае используется внутренний кэш. | Нет       | `prefer-external` |
| downstream-caching-type        | Для этого атрибута следует указать одно из таких значений:<br /><br /> - none — нисходящее кэширование не разрешено;<br />- private — разрешено нисходящее частное кэширование;<br />- public — разрешено частное и совместно используемое нисходящее кэширование.                                                                                                          | Нет       | none              |
| must-revalidate                | Если включено нисходящее кэширование, этот атрибут включает или отключает директиву управления кэшем `must-revalidate` в ответах шлюза.                                                                                                                                                                                                                      | Нет       | true              |
| vary-by-developer              | Установите значение `true`, если нужно кэшировать ответы в зависимости от [ключа подписки](https://docs.microsoft.com/azure/api-management/api-management-subscriptions).                                                                                                                                                                                                                                                                                                         | Да      |         False          |
| vary-by-developer-groups       | Установите значение `true`, если нужно кэшировать ответы в зависимости от [группы пользователя](https://docs.microsoft.com/azure/api-management/api-management-howto-create-groups).                                                                                                                                                                                                                                                                                                             | Да      |       False            |

### <a name="usage"></a>Использование
Эта политика может использоваться в следующих [разделах](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) и [областях](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **Разделы политики:** inbound.
- **Области политики:** все области.

## <a name="StoreToCache"></a> Сохранение в кэше
Политика `cache-store` помещает в кэш ответы в соответствии с заданными настройками кэша. Эту политику можно применять в тех случаях, когда содержимое ответа остается неизменным в течение определенного интервала времени. Кэширование ответов уменьшает требования к пропускной способности и вычислительной мощности, накладываемые на внутренний веб-сервер, а также снижает время задержки для потребителей API.

> [!NOTE]
> Одновременно с этой политикой должна быть определена соответствующая политика [получения из кэша](api-management-caching-policies.md#GetFromCache).

### <a name="policy-statement"></a>Правило политики

```xml
<cache-store duration="seconds" />
```

### <a name="examples"></a>Примеры

#### <a name="example"></a>Пример

```xml
<policies>
    <inbound>
        <base />
        <cache-lookup vary-by-developer="true | false" vary-by-developer-groups="true | false" downstream-caching-type="none | private | public" must-revalidate="true | false">
            <vary-by-query-parameter>parameter name</vary-by-query-parameter> <!-- optional, can repeated several times -->
        </cache-lookup>
    </inbound>
    <outbound>
        <base />
        <cache-store duration="3600" />
    </outbound>
</policies>
```

#### <a name="example-using-policy-expressions"></a>Пример с использованием выражений политики
В этом примере показано, как настроить в службе управления API период хранения ответов в кэше, соответствующий длительности кэширования ответа в серверной службе, которая задается директивой `Cache-Control`. Настройка и использование этой политики демонстрируются в видеоролике [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) ("Облачная среда", серия 177. Дополнительные возможности Управления API (Влад Виноградский)). Нужные сведения см. с 25:25.

```xml
<!-- The following cache policy snippets demonstrate how to control API Management response cache duration with Cache-Control headers sent by the backend service. -->

<!-- Copy this snippet into the inbound section -->
<cache-lookup vary-by-developer="false" vary-by-developer-groups="false" downstream-caching-type="public" must-revalidate="true" >
  <vary-by-header>Accept</vary-by-header>
  <vary-by-header>Accept-Charset</vary-by-header>
</cache-lookup>

<!-- Copy this snippet into the outbound section. Note that cache duration is set to the max-age value provided in the Cache-Control header received from the backend service or to the default value of 5 min if none is found  -->
<cache-store duration="@{
    var header = context.Response.Headers.GetValueOrDefault("Cache-Control","");
    var maxAge = Regex.Match(header, @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value;
    return (!string.IsNullOrEmpty(maxAge))?int.Parse(maxAge):300;
  }"
 />
```

Чтобы узнать больше, см. статью [API Management policy expressions](api-management-policy-expressions.md) (Выражения политики управления API) и раздел [Context variable](api-management-policy-expressions.md#ContextVariables) (Переменная контекста).

### <a name="elements"></a>Элементы

|Название|Описание|Обязательно|
|----------|-----------------|--------------|
|cache-store|Корневой элемент.|Да|

### <a name="attributes"></a>Атрибуты

| Название             | Описание                                                                                                                                                                                                                                                                                                                                                 | Обязательно | Значение по умолчанию           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| duration         | Срок жизни кэшированных записей (в секундах).                                                                                                                                                                                                                                                                                                   | Да      | Н/Д               |

### <a name="usage"></a>Использование
Эта политика может использоваться в следующих [разделах](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) и [областях](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **Разделы политики:** outbound.
- **Области политики:** все области.

## <a name="GetFromCacheByKey"></a> Получение значения из кэша
Используйте политику `cache-lookup-value`, чтобы выполнять поиск в кэше по ключу и возвращать кэшированное значение. Ключ может содержать произвольное строковое значение и обычно указывается с помощью выражения политики.

> [!NOTE]
> Одновременно с этой политикой должна быть определена соответствующая политика [сохранения значения в кэш](#StoreToCacheByKey).

### <a name="policy-statement"></a>Правило политики

```xml
<cache-lookup-value key="cache key value"
    default-value="value to use if cache lookup resulted in a miss"
    variable-name="name of a variable looked up value is assigned to"
    caching-type="prefer-external | external | internal" />
```

### <a name="example"></a>Пример
Дополнительные сведения и примеры этой политики см. в статье [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/) (Пользовательское кэширование в службе управления API Azure).

```xml
<cache-lookup-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    variable-name="userprofile" />

```

### <a name="elements"></a>Элементы

|Имя|Описание|Обязательно|
|----------|-----------------|--------------|
|cache-lookup-value|Корневой элемент.|Да|

### <a name="attributes"></a>Атрибуты

| Имя             | Описание                                                                                                                                                                                                                                                                                                                                                 | Обязательно | Значение по умолчанию           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| тип кэширования | Выберите одно из следующих значений атрибута:<br />- `internal` — использование встроенного кэша в службе управления API;<br />- `external` — использование внешнего кэша, как описано в статье [Использование внешнего кэша Redis для Azure в Управлении API Azure](api-management-howto-cache-external.md);<br />- `prefer-external` — использование внешнего кэша, если он настроен. В противном случае используется внутренний кэш. | Нет       | `prefer-external` |
| default-value    | Значение, которое присваивается переменной, если поиск в кэше по ключу не дал результатов. Если этот атрибут не указан, присваивается значение `null`.                                                                                                                                                                                                           | Нет       | `null`            |
| ключ              | Значение ключа кэша, которое нужно использовать при поиске.                                                                                                                                                                                                                                                                                                                       | Да      | Н/Д               |
| variable-name    | Имя [переменной контекста](api-management-policy-expressions.md#ContextVariables), которой присваивается найденное значение, если поиск завершится успешно. Если поиск не даст результатов, этой переменной присваивается значение, указанное в атрибуте `default-value`. Если атрибут `default-value` не задан, присваивается значение `null`.                                       | Да      | Н/Д               |

### <a name="usage"></a>Использование
Эта политика может использоваться в следующих [разделах](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) и [областях](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **Разделы политики:** inbound, outbound, backend, on-error.
- **Области политики:** все области.

## <a name="StoreToCacheByKey"></a> Сохранение значения в кэш
Политика `cache-store-value` выполняет сохранение в кэш по ключу. Ключ может содержать произвольное строковое значение и обычно указывается с помощью выражения политики.

> [!NOTE]
> Одновременно с этой политикой должна быть определена соответствующая политика [получения значения из кэша](#GetFromCacheByKey).

### <a name="policy-statement"></a>Правило политики

```xml
<cache-store-value key="cache key value" value="value to cache" duration="seconds" caching-type="prefer-external | external | internal" />
```

### <a name="example"></a>Пример
Дополнительные сведения и примеры этой политики см. в статье [Custom caching in Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-sample-cache-by-key/) (Пользовательское кэширование в службе управления API Azure).

```xml
<cache-store-value
    key="@("userprofile-" + context.Variables["enduserid"])"
    value="@((string)context.Variables["userprofile"])" duration="100000" />

```

### <a name="elements"></a>Элементы

|Название|Описание|Обязательно|
|----------|-----------------|--------------|
|cache-store-value|Корневой элемент.|Да|

### <a name="attributes"></a>Атрибуты

| Название             | Описание                                                                                                                                                                                                                                                                                                                                                 | Обязательно | Значение по умолчанию           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| тип кэширования | Выберите одно из следующих значений атрибута:<br />- `internal` — использование встроенного кэша в службе управления API;<br />- `external` — использование внешнего кэша, как описано в статье [Использование внешнего кэша Redis для Azure в Управлении API Azure](api-management-howto-cache-external.md);<br />- `prefer-external` — использование внешнего кэша, если он настроен. В противном случае используется внутренний кэш. | Нет       | `prefer-external` |
| duration         | Кэшированные значения сохраняются в течение указанного здесь времени (в секундах).                                                                                                                                                                                                                                                                                 | Да      | Н/Д               |
| ключ              | Ключ кэша, под которым будет храниться значение.                                                                                                                                                                                                                                                                                                                   | Да      | Н/Д               |
| value            | Значение, которое нужно кэшировать.                                                                                                                                                                                                                                                                                                                                     | Да      | Н/Д               |
### <a name="usage"></a>Использование
Эта политика может использоваться в следующих [разделах](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) и [областях](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **Разделы политики:** inbound, outbound, backend, on-error.
- **Области политики:** все области.

### <a name="RemoveCacheByKey"></a> Удаление значения из кэша
Политика `cache-remove-value` удаляет кэшированный элемент, определяемый по соответствующему ключу. Ключ может содержать произвольное строковое значение и обычно указывается с помощью выражения политики.

#### <a name="policy-statement"></a>Правило политики

```xml

<cache-remove-value key="cache key value" caching-type="prefer-external | external | internal"  />

```

#### <a name="example"></a>Пример

```xml

<cache-remove-value key="@("userprofile-" + context.Variables["enduserid"])"/>

```

#### <a name="elements"></a>Элементы

|Имя|Описание|Обязательно|
|----------|-----------------|--------------|
|cache-remove-value|Корневой элемент.|Да|

#### <a name="attributes"></a>Атрибуты

| Название             | Описание                                                                                                                                                                                                                                                                                                                                                 | Обязательно | Значение по умолчанию           |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------|
| тип кэширования | Выберите одно из следующих значений атрибута:<br />- `internal` — использование встроенного кэша в службе управления API;<br />- `external` — использование внешнего кэша, как описано в статье [Использование внешнего кэша Redis для Azure в Управлении API Azure](api-management-howto-cache-external.md);<br />- `prefer-external` — использование внешнего кэша, если он настроен. В противном случае используется внутренний кэш. | Нет       | `prefer-external` |
| ключ              | Ключ кэшированного ранее значения, которое нужно удалить из кэша.                                                                                                                                                                                                                                                                                        | Да      | Н/Д               |

#### <a name="usage"></a>Использование
Эта политика может использоваться в следующих [разделах](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) и [областях](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

- **Разделы политики:** inbound, outbound, backend, on-error.
- **Области политики:** все области.

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения о работе с политиками см. в следующих статьях:

+ [Политики в управлении API](api-management-howto-policies.md)
+ [Преобразование API-интерфейсов](transform-api.md).
+ Полный перечень операторов политик и их параметров см. в [справочнике по политикам](api-management-policy-reference.md).
+ [Примеры политик](policy-samples.md).
