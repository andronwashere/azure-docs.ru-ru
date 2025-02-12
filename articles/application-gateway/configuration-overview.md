---
title: Общие сведения о настройке шлюза приложений Azure
description: В этой статье описывается настройка компоненты шлюза приложений Azure
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 6/1/2019
ms.author: absha
ms.openlocfilehash: 86b41bb2554dbefeaeb724e746bcaf757463f8ae
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67795907"
---
# <a name="application-gateway-configuration-overview"></a>Общие сведения о конфигурации шлюза приложений

Шлюз приложений Azure состоит из нескольких компонентов, которые можно настроить по-разному для разных сценариев. В этой статье демонстрируется настройка каждого компонента.

![Блок-схема компонентов приложения шлюза](./media/configuration-overview/configuration-overview1.png)

Этот образ показано приложение, имеющее три прослушиватели. Первые два — многосайтовые прослушиватели для `http://acme.com/*` и `http://fabrikam.com/*`, соответственно. Оба прослушивать порт 80. Третий — базовый прослушиватель, который имеет завершение end-to-end Secure Sockets Layer (SSL).


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>предварительные требования

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Виртуальная сеть Azure и выделенную подсеть

Шлюз приложений — это специальное развертывание в виртуальной сети. В виртуальной сети в выделенной подсети является обязательным для шлюза приложений. Может иметь несколько экземпляров определенного шлюза развертывания приложения в подсети. Вы также можете развернуть другие шлюзы приложений в подсети. Но вы не можете развернуть любой другой ресурс в подсети шлюза приложений.

> [!NOTE]
> Нельзя смешивать Standard_v2 и стандартный шлюз приложений Azure в той же подсети.

#### <a name="size-of-the-subnet"></a>Размер подсети

Шлюз приложений получает один частный IP-адрес каждого экземпляра, а также другой частный IP-адрес, если настроен частный внешний IP-адрес.

Azure также резервирует 5 IP-адресов в каждой подсети для внутреннего использования: 4 первый и последний IP-адреса. Например рассмотрим 15 экземпляров шлюза приложений без частный интерфейсный IP-адреса. Требуется по крайней мере 20 IP-адресов подсети: 5 для внутреннего использования и 15 для экземпляров шлюза приложений. Итак, вы должны типа/27 подсети с размером или больше.

Рассмотрите возможность подсети, которая содержит 27 экземпляров шлюза приложений и IP-адреса для частного IP-адреса внешнего интерфейса. В этом случае вам потребуется 33 IP-адресов: 27 для экземпляров шлюза приложений, 1 частный интерфейсный и 5 для внутреннего использования. Итак, вы должны /26 подсети с размером или больше.

Мы рекомендуем использовать размер подсети, по крайней мере размером/28. Этот размер позволяет 11 пригодных IP-адресов. Если в соответствии с загруженностью приложения требует более чем 10 IP-адресов, рассмотрите возможность типа/27 или/26 размер подсети.

#### <a name="network-security-groups-on-the-application-gateway-subnet"></a>Группы безопасности сети для подсети шлюза приложений

Группы безопасности сети (Nsg), поддерживаются в шлюзе приложений. Но существует ряд ограничений:

- Необходимо включить исключения для входящий трафик на порты 65503 – 65534 для номера SKU шлюза приложений версии 1, поэтому 65200 – 65535 для v2 SKU. Такой диапазон портов требуется для обмена данными в рамках инфраструктуры Azure. Эти порты будут защищены (заблокированы) посредством сертификатов Azure. Внешние сущности, включая клиентов этих шлюзов, не могут инициировать изменения на эти конечные точки без соответствующие сертификаты на месте.

- Исходящее подключение к Интернету не может быть заблокировано. Правила для исходящих подключений по умолчанию в группу безопасности сети разрешает подключение к Интернету. Примите во внимание следующие рекомендации.

  - Не удаляйте правила для исходящего трафика по умолчанию.
  - Нельзя создавать другие правила для исходящих подключений, которые запрещают исходящие подключения к Интернету.

- Трафик из **AzureLoadBalancer** тег должен быть разрешен.

##### <a name="allow-application-gateway-access-to-a-few-source-ips"></a>Шлюз приложений доступа к нескольким исходным IP-адресов

В этом сценарии необходимо используйте группы безопасности сети в подсети шлюза приложений. Установлены следующие ограничения для подсети в указанном порядке приоритета:

1. Разрешите входящий трафик из диапазона IP-адрес или IP-адрес источника.
2. Разрешить входящие запросы от всех источников на порты 65503 – 65534 для [взаимодействие работоспособности серверной части](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics). Такой диапазон портов требуется для обмена данными в рамках инфраструктуры Azure. Эти порты будут защищены (заблокированы) посредством сертификатов Azure. Без соответствующие сертификаты на месте внешние сущности не могут инициировать изменения на эти конечные точки.
3. Разрешить входящие Azure Load Balancer проверяет (*AzureLoadBalancer* тег) и входящий трафик виртуальной сети (*VirtualNetwork* тега) на [группы безопасности сети](https://docs.microsoft.com/azure/virtual-network/security-overview).
4. Блокировать весь входящий трафик с помощью Отклонить все правила.
5. Разрешить исходящий трафик в Интернет для всех назначений.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Определяемые пользователем маршруты, поддерживаемые в подсети шлюза приложений

Для SKU v1 определяемые пользователем маршруты (Udr) поддерживаются в подсети шлюза приложений, до тех пор, пока они не изменяют end-to-end запрос-ответ. Например можно настроить определяемый пользователем Маршрут в подсети шлюза приложений для указания устройство брандмауэра для проверки пакетов. Но необходимо убедиться в том, что пакет может связаться с попасть в пункт назначения после проверки. Невыполнение этого требования может привести неправильное проверку работоспособности или поведения маршрутизации трафика. Сюда входят обнаруженные маршруты или маршруты по умолчанию 0.0.0.0/0, которые распространяются Azure ExpressRoute или VPN-шлюза в виртуальной сети.

Для SKU v2 определяемые пользователем маршруты не поддерживаются в подсети шлюза приложений. Дополнительные сведения см. в разделе [SKU шлюза приложений Azure версии 2](application-gateway-autoscaling-zone-redundant.md#differences-with-v1-sku).

> [!NOTE]
> Определяемые пользователем маршруты не поддерживаются для v2 SKU.  Если требуется, чтобы определяемые пользователем маршруты должны продолжать развертывание v1 SKU.

> [!NOTE]
> Использование определяемых пользователем маршрутов в подсети шлюза приложений приводит состояние работоспособности в [представление работоспособности серверной части](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health) отображались как «Неизвестно». Она вызывает создание журналов шлюза приложений и метрики, переход на другой. Мы рекомендуем не использовать определяемые пользователем маршруты в подсети шлюза приложений, и вы сможете просматривать, работоспособности серверной части, журналы и метрики.

## <a name="front-end-ip"></a>Внешний IP-адрес

Можно настроить шлюз приложений требуется общедоступный IP-адрес и частный IP-адрес. Общедоступный IP-адрес является обязательным при размещении серверной части, клиенты должны быть доступны в Интернете через Интернет-виртуальный IP-адрес (VIP). 

Общедоступный IP-адрес не является обязательным для внутренней конечной точки, не подключены к Интернету. Это называется *внутренней подсистемы балансировки нагрузки* конечной точки (ILB). Шлюз приложений с внутренним балансировщиком Нагрузки полезен для внутренних бизнес-приложений, которые не подключены к Интернету. Это также полезно для служб и уровней многоуровневого приложения в пределах границ безопасности, не подключены к Интернету, но, требующие циклической загрузки распространения, закрепления сеансов или завершения SSL-запросов.

Поддерживается только один общедоступный IP-адрес или 1 частный IP-адрес. Выберите внешний IP-адрес, при создании шлюза приложений.

- Для общедоступного IP-адреса можно создать новый общедоступный IP-адрес, или использовать существующий общедоступный IP-адрес в том же расположении, что и шлюз приложений. Если создать новый общедоступный IP-адрес, тип IP-адреса, который выбран (статический или динамический) нельзя изменить позже. Дополнительные сведения см. в разделе [статический и динамический общедоступный IP-адрес](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#static-versus-dynamic-public-ip-address).

- Для частного IP-адреса можно указать частный IP-адрес из подсети, в котором создается шлюз приложений. Если этого не сделать, произвольные IP-адрес из подсети выбирается автоматически. Дополнительные сведения см. в разделе [Создание шлюза приложений с внутренним балансировщиком нагрузки](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm).

Интерфейсный IP-адрес связан *прослушивателя*, который проверяет входящие запросы через внешний IP-адрес.

## <a name="listeners"></a>Прослушиватели

Прослушиватель — это логическая сущность, которое проверяет наличие входящих запросов на подключение, используя порт, протокол, узел и IP-адрес. При настройке прослушивателя, необходимо ввести значения для них, которые совпадают с соответствующими значениями во входящем запросе на шлюзе.

При создании шлюза приложений с помощью портала Azure, вы также создайте прослушиватель по умолчанию, выбрав протокол и порт для прослушивателя. Можно выбрать, следует ли включить поддержку HTTP2 на прослушивателе. После создания шлюза приложений, можно изменить параметры этого прослушивателя, по умолчанию (*appGatewayHttpListener*/*appGatewayHttpsListener*) или создать новые прослушиватели.

### <a name="listener-type"></a>Тип прослушивателя

При создании нового прослушивателя, вы выбираете между [ *основные* и *многосайтовой*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners).

- Если вы размещаете одного сайта за шлюзом приложений, выберите базовый. Узнайте, [как создать шлюз приложений с помощью базовый прослушиватель](https://docs.microsoft.com/azure/application-gateway/quick-create-portal).

- Если вы настраиваете более одного веб-приложение или несколько поддоменов одного родительского домена в одном экземпляре шлюза приложений, выберите многосайтового прослушивателя. Для многосайтового прослушивателя необходимо также ввести имя узла. Это обусловлено тем, шлюз приложений использует заголовки узлов HTTP 1.1 для размещения нескольких веб-сайта на том же общедоступный IP-адрес и порт.

#### <a name="order-of-processing-listeners"></a>Порядок обработки прослушиватели

Для SKU v1 прослушиватели обрабатываются в порядке, в котором они перечислены. Если основной прослушиватель соответствует входящий запрос, обрабатывает этот запрос сначала. Таким образом настройте прослушиватели для нескольких сайтов прежде, чем базовые прослушиватели, чтобы убедиться в том, что трафик направляется к серверной части правильный.

Для SKU v2 многосайтовые прослушиватели обрабатываются перед базовые прослушиватели.

### <a name="front-end-ip"></a>Внешний IP-адрес

Выберите интерфейсный IP-адрес, который планируется связать с этим прослушивателем. Прослушиватель будет прослушивать входящие запросы через этот IP-адрес.

### <a name="front-end-port"></a>Интерфейсный порт

Выберите внешний порт. Выберите существующий порт или создать новую. Выберите любое значение от [допустимый диапазон портов](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports). Можно использовать не только известных портов, например 80 и 443, но допустимый пользовательский порт, который подходит. Порт может использоваться для прослушивателей общедоступных или выходом частного прослушивателей.

### <a name="protocol"></a>Протокол

Выберите HTTP или HTTPS:

- Если выбрано HTTP, трафик между клиентом и шлюз приложений не шифруется.

- При необходимости выбрать HTTPS [завершение запросов SSL](https://docs.microsoft.com/azure/application-gateway/overview#secure-sockets-layer-ssltls-termination) или [end-to-end SSL-шифрование](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Для шифрования трафика между клиентом и шлюза приложений. И завершение SSL-соединение на шлюзе приложений. Если требуется SSL-шифрование end-to-end, необходимо выбрать HTTPS и настроить **HTTP серверной части** параметр. Это гарантирует, что трафик шифруется повторно при его передаче из серверной части шлюза приложений.

Чтобы настроить завершение запросов SSL и SSL-шифрования end-to-end, необходимо добавить сертификат в прослушиватель, чтобы включить шлюз приложений для получения симметричного ключа. Это зависит от спецификации протокола SSL. Симметричный ключ используется для шифрования и расшифровки трафика, отправляемого к шлюзу. Сертификат шлюза должен быть в формате обмена личной информацией (PFX). Этот формат позволяет экспортировать закрытый ключ, который использует шлюз для шифрования и расшифровки трафика.

#### <a name="supported-certificates"></a>Поддерживаемые сертификаты

См. в разделе [сертификаты, поддерживаемые для завершения SSL-запросов](https://docs.microsoft.com/azure/application-gateway/ssl-overview#certificates-supported-for-ssl-termination).

### <a name="additional-protocol-support"></a>Поддержка дополнительных протоколов

#### <a name="http2-support"></a>Поддержка HTTP2

Поддержка протокола HTTP/2 доступна для клиентов, подключающихся к прослушивателям шлюза приложений только. Подключение, для пулов тыловых серверов, по протоколу HTTP/1.1. Поддержка HTTP/2 отключена по умолчанию. В следующем фрагменте кода Azure PowerShell показано, как для этого:

```azurepowershell
$gw = Get-AzApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>Поддержка WebSocket

Поддержка WebSocket включен по умолчанию. Нет не настраиваемый пользователем параметр, чтобы включить или отключить его. Вы можете использовать WebSocket с прослушивателями HTTP и HTTPS.

### <a name="custom-error-pages"></a>Пользовательские страницы ошибок

Можно определить пользовательские ошибки на глобальном уровне или на уровне прослушивателя. Но Создание глобального уровня настраиваемые страницы ошибок с помощью портала Azure в настоящее время не поддерживается. Можно настроить на пользовательскую страницу ошибки для ошибки 403 web application брандмауэра или страницы 502 обслуживания на уровне прослушивателя. Необходимо также указать URL-адрес общедоступного BLOB-объектов для кода состояния возникшей ошибки. Дополнительные сведения см. в статье [Create Application Gateway custom error pages](https://docs.microsoft.com/azure/application-gateway/custom-error) (Создание пользовательских страниц ошибок с помощью Шлюза приложений).

![Коды ошибок службы "Шлюз приложений"](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Чтобы настроить на глобальный пользовательскую страницу ошибки, см. в разделе [конфигурации Azure PowerShell](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration).

### <a name="ssl-policy"></a>Политика SSL

Можно централизовать управление SSL-сертификатами и сократить шифрование расшифровка затраты для фермы серверов серверной части. Централизованная обработка SSL также позволяет задать центральную политику SSL, подходящий для ваших требований безопасности. Вы можете выбрать *по умолчанию*, *предопределенные*, или *пользовательских* политики SSL.

Можно настроить политику SSL для версий протокола SSL для элемента управления. Можно настроить шлюз приложений, чтобы запретить TLS1.0, TLS1.1 и TLS1.2. По умолчанию SSL 2.0 и 3.0 отключены и не настраивается. Дополнительные сведения см. в разделе [Общие сведения о политике SSL шлюза приложений](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview).

После создания прослушивателя, связывается с правилом маршрутизации запросов. Это правило определяет, маршрутизацией запросов, полученных на прослушивателе к серверной части.

## <a name="request-routing-rules"></a>Запрос правила маршрутизации

При создании шлюза приложений с помощью портала Azure, вы создаете правило по умолчанию (*rule1*). Это правило связывает прослушиватель по умолчанию (*appGatewayHttpListener*) с помощью внутреннего пула по умолчанию (*appGatewayBackendPool*) и параметры HTTP серверной части по умолчанию ( *appGatewayBackendHttpSettings*). После создания шлюза, можно изменить параметры правила по умолчанию или создать новые правила.

### <a name="rule-type"></a>Тип правила

При создании правила вы выбираете между [ *основные* и *основаны на путях*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rules).

- Выберите базовую установку, если необходимо передавать все запросы на связанный прослушиватель (например, *блог<i></i>.contoso.com/\*)* в единый пул серверной части.
- Выберите на основе пути, если вы хотите перенаправить запросы от конкретных путей URL-адрес для конкретного внутренних пулов. Шаблон пути применяется только к пути URL-адрес, а не на своих параметрах запроса.

#### <a name="order-of-processing-rules"></a>Порядок обработки правил

Для SKU v1 сопоставления шаблонов входящие запросы обрабатываются в порядке, что пути указаны в сопоставления путей URL-адрес правила на основе путей. Если запрос соответствует шаблону в два или более контуров в сопоставления пути, пути, указанному сначала сопоставляется. И запрос передается к серверной части, который связан с этого пути.

Для SKU v2 точное совпадение — высокий приоритет, чем пути порядок сопоставления URL-пути. Если запрос соответствует шаблону в два или более контуров, запрос перенаправляется к серверной части, который связан с пути, которая точно совпадает с запросом. Если путь во входящем запросе не точно соответствует любой путь на карте, сопоставление шаблонов запроса обрабатывается в порядке пути карты для этого правила на основе пути.

### <a name="associated-listener"></a>Связанный прослушиватель

Связать прослушивателя к правилу таким образом, чтобы *правило маршрутизации запросов* , связанный с прослушиватель будет оценен для определения внутреннего пула для маршрутизации запроса.

### <a name="associated-back-end-pool"></a>Связанный пул серверной части

Свяжите правило пул серверной части, который содержит целевые объекты серверной части, которые обслуживают запросы, получаемые прослушивателем.

 - Для базовое правило допускается только один пул серверной части. Все запросы на связанный прослушиватель, перенаправляются в пул серверной части.

 - Для правила на основе пути добавьте несколько пулов серверной части, соответствующие каждой пути URL-адреса. Запросы, которые соответствуют URL-пути, который вводится перенаправляются в соответствующий пул серверной части. Кроме того добавьте пул серверной части по умолчанию. Запросы, которые не соответствуют любой путь URL-адрес в правиле, перенаправляются в этой группе.

### <a name="associated-back-end-http-setting"></a>Связанные параметры HTTP серверной части

Добавьте параметры HTTP серверной части для каждого правила. Запросы направляются из шлюза приложения для внутренних целей, используя номер порта, протокола и прочие сведения, указанные в параметре.

Для базовое правило допускается только один параметр HTTP серверной части. Все запросы на связанный прослушиватель, перенаправляются на соответствующие целевые серверной части, с помощью этого параметра HTTP.

Добавьте параметры HTTP серверной части для каждого правила. Запросы направляются из шлюза приложения для внутренних целей, используя номер порта, протокола и другие сведения, указанные в параметре.

Для базовое правило допускается только один параметр HTTP серверной части. Все запросы на связанный прослушиватель, перенаправляются на соответствующие целевые серверной части с помощью этого параметра HTTP.

Для правила на основе пути Добавление нескольких параметрах HTTP серверной части, которые соответствуют для каждого пути URL-адрес. Запросы, соответствующие URL-адрес в этом параметре, перенаправляются на соответствующие целевые серверной части, используя параметры HTTP, которые соответствуют каждый путь URL-адрес. Кроме того добавьте параметр HTTP по умолчанию. Запросы, которые не соответствуют любой путь URL-адрес в этом правиле, перенаправляются в пул серверной части по умолчанию с помощью HTTP по умолчанию.

### <a name="redirection-setting"></a>Параметр перенаправления

Если перенаправление настроена для базовое правило, все запросы на связанный прослушиватель, перенаправляются на целевой объект. Это *глобального* перенаправления. Если перенаправление настроена для правила на основе пути, откроется только запросы, в указанную область сайта. Например, Корзина область корзины для покупок, который обозначается */cart/\** . Это *основаны на путях* перенаправления.

Дополнительные сведения о перенаправлении см. в разделе [Общие сведения о перенаправлении для шлюза приложений](https://docs.microsoft.com/azure/application-gateway/redirect-overview).

#### <a name="redirection-type"></a>Тип перенаправления

Выберите тип перенаправления, требуется: *Permanent(301)* , *Temporary(307)* , *Found(302)* , или *см. в разделе other(303)* .

#### <a name="redirection-target"></a>Целевой объект перенаправления

Выберите другой прослушиватель или внешний узел в качестве целевой объект перенаправления.

##### <a name="listener"></a>Прослушиватель

Выберите прослушиватель в качестве целевой объект перенаправления для перенаправления трафика из одного прослушивателя в другую на шлюзе. Этот параметр является обязательным, если вы хотите включить перенаправление HTTP-HTTPS. Он перенаправляет трафик из источника прослушиватель, который проверяет входящие запросы HTTP к целевой прослушиватель, который проверяет наличие входящих HTTPS-запросов. Также можно включить в запрос, который переадресуется в целевой объект перенаправления строки запроса и путь исходного запроса.

![Диалоговое окно компоненты шлюза приложения](./media/configuration-overview/configure-redirection.png)

Дополнительные сведения о перенаправлении HTTP-HTTPS см. в разделе:
- [Перенаправление HTTP-HTTPS с помощью портала Azure](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-portal)
- [Перенаправление HTTP-HTTPS с помощью PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-powershell)
- [Перенаправление HTTP-HTTPS с помощью Azure CLI](https://docs.microsoft.com/azure/application-gateway/redirect-http-to-https-cli)

##### <a name="external-site"></a>Внешний узел

Выберите внешний сайт, если вы хотите перенаправить трафик в прослушиватель, который связан с этим правилом на внешний сайт. Вы можете включить строку запроса из исходного запроса в запрос, который переадресуется в целевой объект перенаправления. Невозможно передать путь к внешний сайт, который был в исходном запросе.

Дополнительные сведения о перенаправлении см. в разделе:
- [Перенаправить трафик на внешний сайт с помощью PowerShell](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-powershell)
- [Перенаправить трафик на внешний сайт с помощью интерфейса командной строки](https://docs.microsoft.com/azure/application-gateway/redirect-external-site-cli)

#### <a name="rewrite-the-http-header-setting"></a>Перепишите параметр заголовка HTTP

Этот параметр добавляет, удаляет или обновляет заголовки запросов и ответов HTTP, во время запроса и перемещение пакетов ответа между клиентом и внутренних пулов. Можно настроить только такую возможность посредством PowerShell. Портал Azure и поддержка CLI еще не доступны. Дополнительные сведения можно найти в разделе

 - [Перепишите Общие сведения о заголовках HTTP](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers)
 - [Настройка переопределения заголовка HTTP](https://docs.microsoft.com/azure/application-gateway/add-http-header-rewrite-rule-powershell#specify-the-http-header-rewrite-rule-configuration)

## <a name="http-settings"></a>Параметры HTTP

Шлюз приложений направляет трафик на внутренних серверах с помощью указанной конфигурации здесь. После создания Настройка HTTP, необходимо связать ее с помощью одного или нескольких правил маршрутизации запросов.

### <a name="cookie-based-affinity"></a>Сходство на основе файлов cookie,

Это полезно, если вы хотите сохранить сеанс пользователя на одном сервере. Управляемые шлюзом файлы cookie позволяют приложения шлюза прямой последующий трафик из сеанса пользователя на тот же сервер для обработки. Это важно в тех случаях, когда состояние сеанса сохраняется локально на сервере для сеанса пользователя. Если приложение не поддерживает сходство на основе файлов cookie, нельзя использовать эту функцию. Чтобы использовать его, убедитесь, что клиенты поддерживают файлы cookie.

### <a name="connection-draining"></a>фильтрация подключений;

Фильтрации подключений позволяет корректно удалить участников пула серверной части во время планового служебного обновления. При создании правила можно применить этот параметр ко всем членам пулу серверной части. Это гарантирует, что всех отмену регистрации экземпляров внутреннего пула не получают новых запросов. В то же время существующие запросы могут завершить в течение настроенного времени. Фильтрации подключений применяет к серверной части, которые явно удалены из пула серверной части путем вызова API. Это также относится к экземплярам серверной части, которые отображаются как *неработоспособное* по работоспособности пробы.

### <a name="protocol"></a>Протокол

Шлюз приложений поддерживает HTTP и HTTPS для маршрутизации запросов для внутренних серверов. Если выбрано HTTP, трафик на внутренние серверы не шифруется. Если причина неприемлем, выберите пункт "HTTPS".

Этот параметр в сочетании с помощью протокола HTTPS в поддерживает прослушиватель [-сквозного режима связи SSL](https://docs.microsoft.com/azure/application-gateway/ssl-overview). Это позволяет безопасно передавать конфиденциальные данные, зашифрованные в серверной части. Каждый тыловых серверов в пуле серверной части с поддержкой протокола SSL end-to-end настраиваются с помощью сертификата, чтобы обеспечивать безопасное взаимодействие.

### <a name="port"></a>Порт

Этот параметр указывает порт, где внутренние серверы прослушивать трафик из шлюза приложений. Вы можете настроить порты, от 1 до 65535.

### <a name="request-timeout"></a>Время ожидания запроса

Этот параметр является количество секунд ожидания шлюза приложений, получил ответ из пула серверной части, перед тем как возвратить сообщение об ошибке «превышено время ожидания подключения».

### <a name="override-back-end-path"></a>Переопределять путь серверной части

Этот параметр позволяет настроить необязательные пользовательские пересылку для использования при запросе пересылается к серверной части. Любую часть входящего путь, соответствующий пользовательский путь в **переопределить путь внутреннего сервера** поле копируется перенаправленных путь. В следующей таблице показаны порядок работы этой функции:

- При присоединении параметр HTTP базовое правило маршрутизации запросов:

  | Исходный запрос  | Переопределять путь серверной части | Запросах к серверной части |
  | ----------------- | --------------------- | ---------------------------- |
  | /Home/            | /override/            | / override/home /              |
  | / home/secondhome / | /override/            | / Переопределение/home/secondhome /   |

- При присоединении параметр HTTP на основе пути правило маршрутизации запросов:

  | Исходный запрос           | Правило для пути       | Переопределять путь серверной части | Запросах к серверной части |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home /            | /pathrule*      | /override/            | / override/home /              |
  | / pathrule/home/secondhome / | /pathrule*      | /override/            | / Переопределение/home/secondhome /   |
  | /Home/                     | /pathrule*      | /override/            | / override/home /              |
  | / home/secondhome /          | /pathrule*      | /override/            | / Переопределение/home/secondhome /   |
  | /pathrule/home /            | / pathrule/home * | /override/            | /override/                   |
  | / pathrule/home/secondhome / | / pathrule/home * | /override/            | / override/secondhome /        |
  | /pathrule/                 | /pathrule/      | /override/            | /override/                   |

### <a name="use-for-app-service"></a>Использование для службы приложений

Это ярлык пользовательского интерфейса, который выбирает двух необходимых параметров для серверной части службы приложений Azure. Он позволяет **выбрать имя узла из внутренних адресов**, и он создает новый пользовательский зонд. (Дополнительные сведения см. в разделе [подбора имя узла из внутренних адресов](#pick) Установка этой статьи.) Создается новую проверку, и заголовок пробы берется из адреса серверной части элемента.

### <a name="use-custom-probe"></a>Использовать пользовательский зонд

Связывает этот параметр [пользовательской пробы](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe) с параметром HTTP. Можно связать только один пользовательский зонд с параметром HTTP. Если пользовательскую проверку, явно не связать [Проба по умолчанию](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#default-health-probe-settings) используется для мониторинга работоспособности серверной части. Мы рекомендуем создать пользовательскую проверку для большего контроля над мониторингом работоспособности Вашей серверной части.

> [!NOTE]
> Пользовательская проверка не отслеживает работоспособность пула серверной части, соответствующий параметр HTTP явным образом связано с прослушивателем.

### <a id="pick"/></a>Выберите имя узла из внутренних адресов

Эта возможность динамически задает *узла* заголовка в запросе на имя узла пула серверной части. Он использует IP-адрес или полное доменное имя.

Эта функция помогает, когда имя домена серверной части отличается от DNS-имя шлюза приложений, и серверной части зависит от конкретного заголовка узла или расширения Указание имени сервера (SNI) при разрешении в правильную конечную точку.

Примеры мультитенантных служб — как серверной части. Служба приложений — это мультитенантная служба, использующий общем пространстве с одного IP-адреса. Таким образом служба приложений может осуществляться только через имена узлов, которые настроены в настройках личного домена.

По умолчанию является имя личного домена *example.azurewebsites.<i> </i>net*. Для доступа к службе приложений с помощью шлюза приложений через имя узла, который явно не зарегистрирован в службе приложений или полное доменное имя шлюза приложений, можно переопределить имя узла в исходном запросе имя узла службы приложений. Чтобы сделать это, необходимо включить **выбрать имя узла из адресов серверной части** параметр.

Для личного домена в службе приложений сопоставляется которого существующего настраиваемого DNS-имени не нужно включить этот параметр.

> [!NOTE]
> Этот параметр не является обязательным для среды службы приложений для PowerApps, которой — это специальное развертывание.

### <a name="host-name-override"></a>Переопределение имени узла

Эта функция заменяет *узла* заголовка во входящем запросе в шлюзе приложений с вами именем узла.

Например если *www.contoso<i></i>.com* указывается в **имя узла** задание исходного запроса *https:/<i></i>/appgw.eastus.cloudapp.net/path1* изменяется на *https:/<i></i>/www.contoso.com/path1* при запрос передается на сервер серверной части.

## <a name="back-end-pool"></a>Внутренний пул

Можно указать четыре типа членов внутреннего пула серверной части: конкретной виртуальной машины, масштабируемый набор виртуальных машин, IP адрес или полное доменное имя или службу приложений. Каждый пул серверной части могут указывать на несколько элементов того же типа. Указывает на элементы разных типов в один и тот же пул серверной части не поддерживается.

После создания пула серверной части, необходимо связать ее с помощью одного или нескольких правил маршрутизации запросов. Необходимо также настроить проверки работоспособности для каждого пула серверной части на шлюзе приложений. При соблюдении условия для правила маршрутизации запросов шлюза приложений перенаправляет трафик на работоспособные серверы (что определяется пробы работоспособности), в соответствующий внутренний пул.

## <a name="health-probes"></a>Пробы работоспособности

Шлюз приложений отслеживает работоспособность всех ресурсов в его серверной части по умолчанию. Но мы настоятельно рекомендуем создать пользовательскую проверку для каждого параметра HTTP серверной части получить больший контроль над наблюдение за работоспособностью. Чтобы узнать, как настроить пользовательскую проверку, см. в разделе [настраиваемые параметры пробы работоспособности](https://docs.microsoft.com/azure/application-gateway/application-gateway-probe-overview#custom-health-probe-settings).

> [!NOTE]
> После создания пользовательской пробы работоспособности, необходимо связать его с параметром HTTP серверной части. Пользовательская проверка не будет проверять работоспособность пула серверной части, соответствующий параметр HTTP явным образом связано с прослушивателем.

## <a name="next-steps"></a>Следующие шаги

Теперь, когда вы знаете о компонентах шлюза приложений, вы можете:

- [Создание шлюза приложений на портале Azure](quick-create-portal.md)
- [Создание шлюза приложений с помощью PowerShell](quick-create-powershell.md)
- [Создание шлюза приложений с помощью Azure CLI](quick-create-cli.md)
