---
title: Общие сведения о безопасности в Центре Интернета вещей Azure | Документация Майкрософт
description: Руководство разработчика. Управление доступом к Центру Интернета вещей для внутренних приложений или приложений для устройств. Содержит сведения о маркерах безопасности и поддержке сертификатов X.509.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/18/2018
ms.openlocfilehash: bb402a5a059fb6f2836bddbd951220271ca77ba3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60400616"
---
# <a name="control-access-to-iot-hub"></a>Управление доступом к Центру Интернета вещей

В этой статье описаны возможности защиты Центра Интернета вещей. Центр Интернета вещей использует *разрешения* для предоставления доступа к каждой из своих конечных точек. Разрешения ограничивают доступ к Центру Интернета вещей на основе функций.

В этой статье описаны:

* Различные разрешения, которые можно предоставить устройству или серверному приложению для доступа к Центру Интернета вещей.
* Процесс аутентификации и маркеры, используемые для проверки разрешений.
* Определение области действия учетных данных для ограничения доступа к определенным ресурсам.
* Поддержка сертификатов X.509 в Центре Интернета вещей.
* Механизмы настраиваемой аутентификации устройства, использующие существующие реестры удостоверений устройств или схемы аутентификации.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Для доступа к любой конечной точке Центра Интернета вещей необходимы соответствующие разрешения. Например, устройство должно содержать маркер с учетными данными безопасности, а также все сообщения, отправленные в Центр Интернета вещей.

## <a name="access-control-and-permissions"></a>Контроль доступа и разрешений

Предоставить [разрешения](#iot-hub-permissions) можно следующими способами:

* **Политики общего доступа на уровне Центра Интернета вещей**. Политики общего доступа могут предоставлять любое сочетание [разрешений](#iot-hub-permissions). Политики можно определить на [портале Azure](https://portal.azure.com) программно, используя [интерфейсы REST API поставщика ресурсов Центра Интернета вещей](/rest/api/iothub/iothubresource), или с помощью команды интерфейса командной строки [az iot hub policy](/cli/azure/iot/hub/policy?view=azure-cli-latest). По умолчанию для только что созданного Центра Интернета вещей заданы такие политики:
  
  | Политика общего доступа | Разрешения |
  | -------------------- | ----------- |
  | iothubowner | Все разрешения |
  | служба | Разрешения **ServiceConnect** |
  | device | Разрешения **DeviceConnect** |
  | registryRead | Разрешения **RegistryRead** |
  | registryReadWrite | Разрешения **RegistryRead** и **RegistryWrite** |

* **Учетные данные безопасности на уровне отдельного устройства**. Каждый Центр Интернета вещей содержит [реестр удостоверений](iot-hub-devguide-identity-registry.md). Для каждого устройства в этом реестре удостоверений вы можете задать учетные данные безопасности, дающие вам разрешения **DeviceConnect**, которые соответствуют конечным точкам устройств.

Например, в стандартном решении IoT:

* Компонент управления устройством использует политику *registryReadWrite* .
* Компонент обработчика событий использует политику *service* .
* Компонент бизнес-логики устройства среды выполнения использует политику *service*.
* Отдельные устройства подключаются с помощью учетных данных, которые хранятся в реестре удостоверений Центра Интернета вещей.

> [!NOTE]
> Дополнительные сведения см. в статье о [разрешениях](#iot-hub-permissions).

## <a name="authentication"></a>Authentication

Центр Интернета вещей Azure предоставляет доступ к конечным точкам, проверяя маркер на соответствие политикам общего доступа и учетным данным безопасности в реестре удостоверений.

Учетные данные безопасности, например симметричные ключи, никогда не отправляются по сети.

> [!NOTE]
> Безопасность для поставщика ресурсов Центра Интернета вещей Azure обеспечивается с помощью подписки Azure, это касается всех поставщиков в [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

Дополнительные сведения о способах создания и использования маркеров безопасности см. в разделе [Маркеры безопасности Центра Интернета вещей](iot-hub-devguide-security.md#security-tokens).

### <a name="protocol-specifics"></a>Особенности протокола

Каждый поддерживаемый протокол, например MQTT, AMQP и HTTPS, передает маркеры разными способами.

При использовании протокола MQTT пакет CONNECT содержит код deviceId как значение ClientId, `{iothubhostname}/{deviceId}` — в поле "Имя пользователя", а маркер SAS — в поле "Пароль". `{iothubhostname}` — это полная запись CName Центра Интернета вещей (например, contoso.azure-devices.net).

При использовании [AMQP](https://www.amqp.org/) Центр Интернета вещей поддерживает [SASL PLAIN](https://tools.ietf.org/html/rfc4616) и [защиты AMQP на основе утверждений](https://www.oasis-open.org/committees/download.php/50506/amqp-cbs-v1%200-wd02%202013-08-12.doc).

Стандарт защиты AMQP на основе утверждений определяет, как следует передавать эти маркеры.

Для SASL PLAIN **имя пользователя** может быть следующим:

* `{policyName}@sas.root.{iothubName}` — при использовании маркеров уровня Центра Интернета вещей;
* `{deviceId}@sas.{iothubname}` — при использовании маркеров уровня устройства.

В обоих случаях поле пароля содержит маркер, как описано в разделе [Маркеры безопасности Центра Интернета вещей](iot-hub-devguide-security.md#security-tokens).

Протокол HTTPS реализует аутентификацию путем включения допустимого маркера в заголовок запроса **авторизации**.

#### <a name="example"></a>Пример

Имя пользователя (значение DeviceId следует вводить с учетом регистра): `iothubname.azure-devices.net/DeviceId`

Пароль (можно создать маркер SAS с помощью инструмента [Обозреватель устройств](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer), команды расширения интерфейса командной строки [az iot hub generate-sas-token](/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-generate-sas-token) или [расширения "Средства Интернета вещей Azure" для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)).

`SharedAccessSignature sr=iothubname.azure-devices.net%2fdevices%2fDeviceId&sig=kPszxZZZZZZZZZZZZZZZZZAhLT%2bV7o%3d&se=1487709501`

> [!NOTE]
> [Пакеты SDK для Azure IoT](iot-hub-devguide-sdks.md) автоматически создают маркеры при подключении к службе. В некоторых случаях пакеты SDK для Azure IoT поддерживают не все протоколы или не все методы проверки подлинности.

### <a name="special-considerations-for-sasl-plain"></a>Специальные рекомендации для SASL PLAIN

При использовании SASL PLAIN с протоколом AMQP клиент, подключающийся к Центру Интернета вещей, может использовать по одному маркеру для каждого TCP-подключения. Когда срок действия маркера истекает, TCP-подключение к службе прерывается и выполняется попытка повторного подключения. Хотя это поведение и не является проблематичным для внутреннего приложения, оно может навредить приложению для устройства по следующим причинам:

* Шлюзы обычно подключаются от имени многих устройств. Если используется SASL PLAIN, шлюзам нужно создать отдельное TCP-подключение для каждого устройства, подключающегося к Центру Интернета вещей. Этот сценарий значительно повышает потребление электроэнергии и сетевых ресурсов и увеличивает задержку подключения устройства.

* Если потребление ресурсов увеличится, устройства с ограниченными ресурсами должны будут выполнять повторное подключение после истечения срока действия маркера.

## <a name="scope-iot-hub-level-credentials"></a>Определение области действия учетных данных на уровне Центра Интернета вещей

Чтобы определить область действия для политик безопасности на уровне Центра Интернета вещей, создайте маркеры с помощью универсального кода (URI) ограниченного ресурса. Например, конечная точка для отправки сообщений с устройства в облако — **/devices/{deviceId}/messages/events**. Кроме того, вы можете использовать политику общего доступа на уровне Центра Интернета вещей с разрешениями **DeviceConnect**. С ее помощью можно подписать маркер, значение resourceURI которого — **/devices/{deviceId}** . В результате такого подхода создается маркер, который можно использовать только для отправки сообщений от имени устройства с кодом **deviceId**.

Этот механизм похож на [политику издателя Центров событий](https://code.msdn.microsoft.com/Service-Bus-Event-Hub-99ce67ab). Он позволяет реализовывать методы настраиваемой проверки подлинности.

## <a name="security-tokens"></a>Маркеры безопасности Центра Интернета вещей

Центр Интернета вещей использует маркеры безопасности для проверки подлинности устройств и служб, чтобы избежать отправки ключей по сети. Кроме того, маркеры безопасности ограничены по времени и области действия. [Пакеты SDK для Azure IoT](iot-hub-devguide-sdks.md) автоматически создают маркеры без специальной настройки. В некоторых случаях требуется напрямую создать и использовать маркеры безопасности. Ниже приведены соответствующие сценарии.

* Непосредственное использование поверхностей MQTT, AMQP или HTTPS.

* Реализация схемы службы маркеров, как описано в разделе [Настраиваемая проверка подлинности устройства](iot-hub-devguide-security.md#custom-device-and-module-authentication).

Центр Интернета вещей также позволяет устройствам использовать [сертификаты X.509](iot-hub-devguide-security.md#supported-x509-certificates) для проверки подлинности с помощью Центра Интернета вещей.

### <a name="security-token-structure"></a>Структура маркера безопасности

Маркеры безопасности используются для предоставления устройствам и службам ограниченного по времени доступа к определенным функциям в Центр Интернета вещей. Чтобы получить авторизацию для подключения к Центру Интернета вещей, устройства и службы должны отправить маркеры безопасности, подписанные с помощью общего ключа доступа или симметричного ключа, сохраненного с удостоверением устройства в реестре удостоверений.

Маркер, подписанный с помощью ключа общего доступа, предоставляет доступа ко всем функциям, связанным с разрешениями политики общего доступа. Маркер, подписанный с помощью симметричного ключа удостоверения устройства, предоставляет только разрешение **DeviceConnect** для связанного удостоверения устройства.

Маркер безопасности имеет следующий формат:

`SharedAccessSignature sig={signature-string}&se={expiry}&skn={policyName}&sr={URL-encoded-resourceURI}`

Это ожидаемые значения:

| Значение | Описание |
| --- | --- |
| {signature} |Строка подписи HMAC-SHA256 формата `{URL-encoded-resourceURI} + "\n" + expiry`. **Важно!** Ключ шифруется в кодировке base64 и используется для вычислений HMAC-SHA256. |
| {resourceURI} |Начинающийся с имени узла Центра Интернета вещей (без протокола) префикс URI (по сегменту) для конечных точек, доступ к которым можно получить с помощью этого маркера. Например: `myHub.azure-devices.net/devices/device1` |
| {expiry} |Строки в формате UTF8, отображающие количество секунд с начала эры 00:00:00 (в формате UTC) 1 января 1970 г. |
| {URL-encoded-resourceURI} |Строчное URL-кодирование строчного URL ресурса |
| {policyName} |Имя политики общего доступа, к которой относится этот маркер. Отсутствует, если маркер относится к учетным данным реестра устройства. |

**Обратите внимание**, что префикс универсального кода ресурса (URI) вычисляется по сегменту, а не по символу. Например, `/a/b` — это префикс для `/a/b/c`, а не для `/a/bc`.

В следующем фрагменте кода Node.js показан функция **generateSasToken**, которая вычисляет маркер, используя входные данные`resourceUri, signingKey, policyName, expiresInMins`. В следующих разделах показано, как инициализировать различные входные данные для различных сценариев использования маркеров.

```javascript
var generateSasToken = function(resourceUri, signingKey, policyName, expiresInMins) {
    resourceUri = encodeURIComponent(resourceUri);

    // Set expiration in seconds
    var expires = (Date.now() / 1000) + expiresInMins * 60;
    expires = Math.ceil(expires);
    var toSign = resourceUri + '\n' + expires;

    // Use crypto
    var hmac = crypto.createHmac('sha256', new Buffer(signingKey, 'base64'));
    hmac.update(toSign);
    var base64UriEncoded = encodeURIComponent(hmac.digest('base64'));

    // Construct authorization string
    var token = "SharedAccessSignature sr=" + resourceUri + "&sig="
    + base64UriEncoded + "&se=" + expires;
    if (policyName) token += "&skn="+policyName;
    return token;
};
```

Для сравнения приведен эквивалентный код Python для создания маркера безопасности.

```python
from base64 import b64encode, b64decode
from hashlib import sha256
from time import time
from urllib import quote_plus, urlencode
from hmac import HMAC

def generate_sas_token(uri, key, policy_name, expiry=3600):
    ttl = time() + expiry
    sign_key = "%s\n%d" % ((quote_plus(uri)), int(ttl))
    print sign_key
    signature = b64encode(HMAC(b64decode(key), sign_key, sha256).digest())

    rawtoken = {
        'sr' :  uri,
        'sig': signature,
        'se' : str(int(ttl))
    }

    if policy_name is not None:
        rawtoken['skn'] = policy_name

    return 'SharedAccessSignature ' + urlencode(rawtoken)
```

Принцип создания маркера безопасности в C#:

```csharp
using System;
using System.Globalization;
using System.Net;
using System.Net.Http;
using System.Security.Cryptography;
using System.Text;

public static string generateSasToken(string resourceUri, string key, string policyName, int expiryInSeconds = 3600)
{
    TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
    string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + expiryInSeconds);

    string stringToSign = WebUtility.UrlEncode(resourceUri) + "\n" + expiry;

    HMACSHA256 hmac = new HMACSHA256(Convert.FromBase64String(key));
    string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));

    string token = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}", WebUtility.UrlEncode(resourceUri), WebUtility.UrlEncode(signature), expiry);

    if (!String.IsNullOrEmpty(policyName))
    {
        token += "&skn=" + policyName;
    }

    return token;
}

```


> [!NOTE]
> Так как срок действия маркера проверяется на компьютерах Центра Интернета вещей, нужно обеспечить минимальное смещение на часах компьютера, где создается маркер.

### <a name="use-sas-tokens-in-a-device-app"></a>Использование маркеров SAS в приложении для устройства

Существует два способа получения разрешений **DeviceConnect** для Центра Интернета вещей с маркерами безопасности: с помощью [симметричного ключа устройства из реестра удостоверений](#use-a-symmetric-key-in-the-identity-registry) или [ключа общего доступа](#use-a-shared-access-policy).

Помните, что все функциональные возможности, доступные с устройств, намеренно предоставляются в конечных точках с префиксом `/devices/{deviceId}`.

> [!IMPORTANT]
> Единственный способ проверки подлинности конкретного устройства в Центре Интернета вещей предполагает использование симметричного ключа удостоверения устройства. В случаях, когда для доступа к функциям устройства применяется политика общего доступа, решение должно считать компонент, который выдает маркер безопасности, доверенным компонентом.

Далее указаны конечные точки, доступные с устройства (вне зависимости от протокола).

| Конечная точка | Функция |
| --- | --- |
| `{iot hub host name}/devices/{deviceId}/messages/events` |Отправка сообщений с устройства в облако. |
| `{iot hub host name}/devices/{deviceId}/messages/devicebound` |Получение сообщений из облака на устройство. |

### <a name="use-a-symmetric-key-in-the-identity-registry"></a>Использование симметричного ключа в реестре удостоверений

Если для создания маркера используется симметричный ключ удостоверения устройства, то элемент policyName (`skn`) пропускается.

Например, маркер, созданный для доступа ко всем функциям устройства, должен иметь следующие параметры:

* универсальный код ресурса (URI): `{IoT hub name}.azure-devices.net/devices/{device id}`;
* ключ подписывания: любой симметричный ключ для удостоверения `{device id}` ;
* имя политики не требуется;
* время окончания срока действия.

Далее приведен пример использования предыдущей функции Node.js:

```javascript
var endpoint ="myhub.azure-devices.net/devices/device1";
var deviceKey ="...";

var token = generateSasToken(endpoint, deviceKey, null, 60);
```

Результат, предоставляющий доступ ко всем возможностям устройства device1, будет иметь следующий вид:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697`

> [!NOTE]
> Можно создать маркер SAS с помощью инструмента [Device Explorer](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/tools/DeviceExplorer), команды расширения интерфейса командной строки [az iot hub generate-sas-token](/cli/azure/ext/azure-cli-iot-ext/iot/hub?view=azure-cli-latest#ext-azure-cli-iot-ext-az-iot-hub-generate-sas-token) или [Средства Интернета вещей Azure для Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

### <a name="use-a-shared-access-policy"></a>Использование политики общего доступа

При создании маркера из политики общего доступа в поле `skn` укажите имя политики. Эта политика должна предоставить разрешение **DeviceConnect**.

Существует два основных сценария использования политик общего доступа для доступа к возможностям устройств:

* [облачные шлюзы протоколов](iot-hub-devguide-endpoints.md);
* [службы маркеров](iot-hub-devguide-security.md#custom-device-and-module-authentication), используемые для реализации настраиваемых схем проверки подлинности.

Так как политика общего доступа может предоставлять доступ для подключения в качестве любого устройства, при создании маркеров безопасности важно использовать правильный URI ресурса. Этот параметр имеет особое значение для служб маркеров, которые должны определять область действия маркера для конкретного устройства с помощью URI ресурса. Он менее критичен для шлюзов протоколов, так как они уже обрабатывают трафик для всех устройств.

Например, служба маркеров, использующая предварительно созданную политику общего доступа с именем **device** , создаст маркер со следующими параметрами:

* универсальный код ресурса (URI): `{IoT hub name}.azure-devices.net/devices/{device id}`;
* ключ подписывания: один из ключей политики `device` ;
* имя политики: `device`;
* время окончания срока действия.

Далее приведен пример использования предыдущей функции Node.js:

```javascript
var endpoint ="myhub.azure-devices.net/devices/device1";
var policyName = 'device';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Результат, предоставляющий доступ ко всем возможностям устройства device1, будет иметь следующий вид:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices%2fdevice1&sig=13y8ejUk2z7PLmvtwR5RqlGBOVwiq7rQR3WZ5xZX3N4%3D&se=1456971697&skn=device`

Шлюз протокола может использовать этот же маркер для всех устройств, задав `myhub.azure-devices.net/devices`в качестве универсального кода ресурса (URI).

### <a name="use-security-tokens-from-service-components"></a>Использование маркеров безопасности из компонентов службы

Компоненты службы могут создавать маркеры безопасности только с помощью политик общего доступа, предоставляющих соответствующие разрешения, как описано ранее.

Ниже приведены функции службы, предоставляемые в конечных точках.

| Конечная точка | Функция |
| --- | --- |
| `{iot hub host name}/devices` |Создание, обновление, извлечение и удаление удостоверений устройств. |
| `{iot hub host name}/messages/events` |Получение сообщений с устройства в облако. |
| `{iot hub host name}/servicebound/feedback` |Получение ответа на сообщения, отправляемые из облака на устройство. |
| `{iot hub host name}/devicebound` |Отправка сообщений из облака на устройство. |

Например, служба, использующая предварительно созданную политику общего доступа с именем **registryRead** , создаст маркер со следующими параметрами:

* универсальный код ресурса (URI): `{IoT hub name}.azure-devices.net/devices`;
* ключ подписывания: один из ключей политики `registryRead` ;
* имя политики: `registryRead`;
* время окончания срока действия.

```javascript
var endpoint ="myhub.azure-devices.net/devices";
var policyName = 'registryRead';
var policyKey = '...';

var token = generateSasToken(endpoint, policyKey, policyName, 60);
```

Результат, предоставляющий доступ для чтения всех идентификаторов устройств, будет иметь следующий вид:

`SharedAccessSignature sr=myhub.azure-devices.net%2fdevices&sig=JdyscqTpXdEJs49elIUCcohw2DlFDR3zfH5KqGJo4r4%3D&se=1456973447&skn=registryRead`

## <a name="supported-x509-certificates"></a>Поддерживаемые сертификаты X.509

Вы можете выполнить аутентификацию для устройства в Центре Интернета вещей при помощи любого сертификата X.509. Для этого отправьте отпечаток сертификата или данные центра сертификации (ЦС) в Центр Интернета вещей. При аутентификации с использованием отпечатков сертификатов проверяется только соответствие предоставленного отпечатка настроенному. При аутентификации с использованием центра сертификации проверяется цепочка сертификатов. 

Поддерживаемые сертификаты включают:

* **Существующий сертификат X.509**. Возможно, устройство уже имеет связанный сертификат X.509. Устройство может использовать этот сертификат для проверки подлинности с помощью Центра Интернета вещей. Работает с аутентификацией при помощи отпечатков или ЦС. 

* **Сертификат X.509, подписанный центром сертификации**. Для идентификации устройства и его аутентификации с помощью Центра Интернета вещей можно использовать сертификат X.509, созданный и подписанный центром сертификации (ЦС). Работает с аутентификацией при помощи отпечатков или ЦС.

* **Самостоятельно сформированный и самозаверяющий сертификат X-509**. Производитель устройства или внутренний специалист по развертыванию может создать эти сертификаты и сохранить соответствующий закрытый ключ (и сертификат) на устройстве. Для этого вы можете использовать такие инструменты, как [OpenSSL](https://www.openssl.org/) и служебную программу [Windows SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate). Работает только с аутентификацией при помощи отпечатков. 

Для аутентификации устройство может использовать только один из вариантов: либо сертификат X.509, либо маркер безопасности.

Дополнительные сведения об аутентификации с помощью центра сертификации см. в статье [Проверка подлинности устройства с помощью сертификатов ЦС X.509](iot-hub-x509ca-overview.md).

### <a name="register-an-x509-certificate-for-a-device"></a>Регистрация сертификата X.509 для устройства

[Пакет SDK службы Интернета вещей Azure для C#](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service) (версия 1.0.8+) поддерживает регистрацию устройства, которое использует сертификат X.509 для проверки подлинности. Другие интерфейсы API (например, импорт и экспорт устройств) также поддерживают сертификаты X.509.

Для настройки сертификатов X.509 для устройств можно также использовать команду интерфейса командной строки расширения [az iot hub device-identity](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity?view=azure-cli-latest).

### <a name="c-support"></a>Поддержка C\#

Класс **RegistryManager** предоставляет программный способ регистрации устройства. В частности, методы **AddDeviceAsync** и **UpdateDeviceAsync** позволяют регистрировать и обновлять устройства в реестре удостоверений Центра Интернета вещей. Эти два метода используют экземпляр **устройства** в качестве входных данных. Класс **Device** включает свойство **Authentication**, которое позволяет указывать основной и дополнительный отпечатки сертификата X.509. Этот отпечаток представляет собой хэш SHA256 сертификата X.509 (сохраненного в двоичной кодировке DER). Вы можете указать основной или вторичный отпечаток или оба отпечатка сразу. Основной и вторичный отпечатки поддерживаются для обработки сценариев переключения сертификатов.

Ниже приведен пример фрагмента кода C\# для регистрации устройства с использованием отпечатка сертификата X.509:

```csharp
var device = new Device(deviceId)
{
  Authentication = new AuthenticationMechanism()
  {
    X509Thumbprint = new X509Thumbprint()
    {
      PrimaryThumbprint = "B4172AB44C28F3B9E117648C6F7294978A00CDCBA34A46A1B8588B3F7D82C4F1"
    }
  }
};
RegistryManager registryManager = RegistryManager.CreateFromConnectionString(deviceGatewayConnectionString);
await registryManager.AddDeviceAsync(device);
```

### <a name="use-an-x509-certificate-during-run-time-operations"></a>Использование сертификата X.509 во время выполнения операций среды выполнения

[Пакет SDK для устройств Azure IoT для .NET](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device) (версия 1.0.11+) поддерживает использование сертификатов X.509.

### <a name="c-support"></a>Поддержка C\#

Класс **DeviceAuthenticationWithX509Certificate** поддерживает создание экземпляров **DeviceClient** с помощью сертификата X.509. Сертификат X.509 должен быть в формате PFX (также называется PKCS № 12), который содержит закрытый ключ.

Ниже приведен образец фрагмента кода:

```csharp
var authMethod = new DeviceAuthenticationWithX509Certificate("<device id>", x509Certificate);

var deviceClient = DeviceClient.Create("<IotHub DNS HostName>", authMethod);
```

## <a name="custom-device-and-module-authentication"></a>Настраиваемая аутентификация устройства и модуля

Вы можете использовать [Реестр удостоверений](iot-hub-devguide-identity-registry.md) Центра Интернета вещей для настройки учетных данных безопасности для каждого устройства или модуля и контроля доступа с помощью [токенов](iot-hub-devguide-security.md#security-tokens). Если в решении Интернета вещей уже есть настраиваемый реестр удостоверений и (или) схема аутентификации, то рекомендуем создать *службу маркеров*, чтобы интегрировать эту инфраструктуру с Центром Интернета вещей. Таким образом можно использовать и другие функции IoT в решении.

Служба маркеров — это пользовательская облачная служба. Она использует *политику общего доступа* Центра Интернета вещей с разрешениями **DeviceConnect** или **ModuleConnect** для создания маркеров *уровня устройства* или *уровня модуля*. Эти маркеры позволяют устройству или модулю подключиться к Центру Интернета вещей.

![Этапы для схемы службы маркеров.](./media/iot-hub-devguide-security/tokenservice.png)

Ниже приведены основные этапы для схемы службы маркеров.

1. Создайте политику общего доступа Центра Интернета вещей с разрешениями **DeviceConnect** или **ModuleConnect** для Центра Интернета вещей. Эту политику можно создать на [портале Azure](https://portal.azure.com) или программным способом. Эту политику служба маркеров будет использовать для подписания создаваемых маркеров.

2. Когда устройству и (или) модулю требуется доступ к Центру Интернета вещей, они запрашивают у службы маркеров подписанный маркер. Для выбора удостоверения устройства и (или) модуля, используемого службой маркеров для создания маркера, устройство может использовать настраиваемую схему аутентификации или реестр удостоверений.

3. Служба маркеров возвращает маркер. Чтобы создать маркер, используйте `/devices/{deviceId}` или `/devices/{deviceId}/module/{moduleId}` в качестве `resourceURI`, где `deviceId` обозначает аутентифицируемое устройство, а `moduleId` — аутентифицируемый модуль. Служба маркеров использует политики общего доступа для создания маркера.

4. Устройство или модуль используют маркер непосредственно для подключения к Центру Интернета вещей.

> [!NOTE]
> Для создания маркера в службе маркеров можно использовать класс .NET [SharedAccessSignatureBuilder](https://msdn.microsoft.com/library/microsoft.azure.devices.common.security.sharedaccesssignaturebuilder.aspx) или класс Java [IotHubServiceSasToken](/java/api/com.microsoft.azure.sdk.iot.service.auth.iothubservicesastoken).

При необходимости служба маркеров может задать срок действия маркера. По истечении срока действия маркера Центр Интернета вещей разрывает подключение к устройству или модулю. После этого устройство или модуль должны запросить новый маркер у службы маркеров. Короткий срок действия увеличит нагрузку как на устройства и модули, так и на службу маркеров.

Чтобы подключить устройство или модуль к Центру Интернета вещей, такое устройство или модуль нужно добавить в реестр удостоверений этого Центра Интернета вещей, даже если для подключения используется маркер, а не ключ. Это означает, что вы можете управлять доступом на уровне отдельных устройств или модулей обычным образом, то есть включая или отключая их удостоверения в [реестре удостоверений](iot-hub-devguide-identity-registry.md). Это уменьшает риск существования маркеров с длительным сроком действия.

### <a name="comparison-with-a-custom-gateway"></a>Сравнение с настраиваемым шлюзом

Вариант со службой маркеров является рекомендованным способом внедрения настраиваемого реестра удостоверений или схемы проверки подлинности с использованием Центра Интернета вещей. Этот вариант рекомендуется, так как Центр Интернета вещей продолжает обрабатывать большую часть трафика решения. Однако, если настраиваемая схема аутентификации настолько тесно переплетена с протоколом, то для обработки всего трафика может потребоваться *настраиваемый шлюз*. В качестве примера сценария можно привести использование [Протокола безопасности транспортного уровня (TLS) и предварительных общих ключей (PSK)](https://tools.ietf.org/html/rfc4279). Дополнительные сведения см. в статье о [шлюзе протокола](iot-hub-protocol-gateway.md).

## <a name="reference-topics"></a>Справочные материалы

В следующих материалах предоставлены дополнительные сведения об управлении доступом к Центру Интернета вещей.

## <a name="iot-hub-permissions"></a>Разрешения Центра Интернета вещей

В следующей таблице указаны разрешения, с помощью которых можно управлять доступом к Центру Интернета вещей.

| Разрешение | Примечания |
| --- | --- |
| **RegistryRead** |Предоставляет доступ на чтение к реестру удостоверений. Дополнительные сведения см. в разделе о [реестре удостоверений](iot-hub-devguide-identity-registry.md). <br/>Это разрешение используется серверными облачными службами. |
| **RegistryReadWrite** |Предоставляет доступ на чтение и запись к реестру удостоверений. Дополнительные сведения см. в разделе о [реестре удостоверений](iot-hub-devguide-identity-registry.md). <br/>Это разрешение используется серверными облачными службами. |
| **ServiceConnect** |Предоставляет доступ к конечным точкам обмена данными и мониторинга, обращенным к облачной службе. <br/>Предоставляет разрешение облачным службам получать сообщения с устройств, отправлять сообщения на устройства и получать соответствующие уведомления о доставке. <br/>Предоставляет разрешение на получение подтверждения передачи файлов. <br/>Предоставляет разрешение на доступ к двойникам для обновления тегов и требуемых свойств, извлечения сообщаемых свойств и выполнения запросов. <br/>Это разрешение используется серверными облачными службами. |
| **DeviceConnect** |Предоставляет доступ к конечным точкам для устройств. <br/>Позволяет отправлять сообщения с устройства в облако и получать сообщения, отправленные из облака на устройство. <br/>Предоставляет разрешение на передачу файлов с устройства. <br/>Предоставляет разрешение на получение уведомлений о требуемых свойствах для двойников устройств и на обновление сообщенных свойств двойных устройств. <br/>Предоставляет разрешение на передачу файлов. <br/>Это разрешение используют устройства. |

## <a name="additional-reference-material"></a>Дополнительные справочные материалы

Другие справочные статьи в руководстве разработчика для Центра Интернета вещей:

* В [справочнике по конечным точкам Центра Интернета вещей](iot-hub-devguide-endpoints.md) содержатся сведения о конечных точках, которые каждый центр Интернета вещей предоставляет для операций управления и среды выполнения.

* Руководство [Квоты и регулирование в Центре Интернета вещей](iot-hub-devguide-quotas-throttling.md) содержит сведения о квотах, применимых к службе Центра Интернета вещей, и поведении регулирования.

* В статье о [пакетах SDK для устройств и служб Azure IoT](iot-hub-devguide-sdks.md) указаны различные языковые пакеты SDK, которые можно использовать при разработке приложений для устройств и служб, взаимодействующих с Центром Интернета вещей.

* В статье [Язык запросов Центра Интернета вещей для двойников устройств и двойников модулей, заданий и маршрутизации сообщений](iot-hub-devguide-query-language.md) описывается язык запросов, который можно использовать для получения сведений о двойниках устройств и заданиях из Центра Интернета вещей.

* Статья [Взаимодействие с Центром Интернета вещей с помощью протокола MQTT](iot-hub-mqtt-support.md) содержит дополнительные сведения о поддержке протокола MQTT в Центре Интернета вещей.

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы узнали, как управлять доступом к Центру Интернета вещей, вас могут заинтересовать следующие статьи в руководстве разработчика для Центра Интернета вещей:

* [Общие сведения о двойниках устройств и их использование в Центре Интернета вещей](iot-hub-devguide-device-twins.md)
* [Общие сведения о прямых методах и информация о вызове этих методов из Центра Интернета вещей](iot-hub-devguide-direct-methods.md)
* [Планирование заданий на нескольких устройствах](iot-hub-devguide-jobs.md)

Чтобы применить на практике некоторые основные понятия, описанные в этой статье, просмотрите следующие руководства по Центру Интернета вещей:

* [Приступая к работе с Центром Интернета вещей Azure](quickstart-send-telemetry-node.md).
* [Как отправлять сообщения из облака на устройства с помощью Центра Интернета вещей](iot-hub-csharp-csharp-c2d.md)
* [Как обрабатывать сообщения, отправляемые с устройства Центра Интернета вещей в облако](tutorial-routing.md)
