---
title: Настройка сквозного режима связи SSL для шлюза приложений Azure
description: В этой статье описывается настройка сквозного режима связи SSL для шлюза приложений Azure с помощью PowerShell.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/8/2019
ms.author: victorh
ms.openlocfilehash: d7b909bf88fde2277aa2a285bbf36916191db1f3
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "67973389"
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-powershell"></a>Настройка сквозного режима связи SSL для Шлюза приложений с помощью PowerShell

## <a name="overview"></a>Обзор

Шлюз приложений Azure поддерживает сквозное шифрование трафика. Для этого он завершает SSL-соединение. Затем шлюз применяет правила маршрутизации к трафику, повторно шифрует пакет и пересылает его в соответствующую серверную часть согласно определенным правилам маршрутизации. Любой ответ веб-сервера проходит через тот же процесс на пути к пользователю.

Шлюз приложений поддерживает определение пользовательских вариантов SSL и Он также поддерживает отключение следующих версий протокола: **TLSv1.0**, **TLSv1.1** и **TLSv1.2**. Кроме того, шлюз поддерживает определение комплектов шифров для использования и их приоритет. Дополнительные сведения о настраиваемых параметрах SSL см. в статье [Общие сведения о политике SSL шлюза приложений](application-gateway-SSL-policy-overview.md).

> [!NOTE]
> Протоколы SSL 2.0 и SSL 3.0 отключены по умолчанию, и их нельзя включать. Они считаются небезопасными и не могут использоваться со шлюзом приложений.

![Изображение для сценария][scenario]

## <a name="scenario"></a>Сценарий

В этом сценарии вы узнаете, как создать шлюз приложений со сквозным режимом связи SSL с помощью PowerShell.

Вы узнаете:

* как создать группу ресурсов с именем **appgw-rg**;
* как создать виртуальную сеть **appgwvnet** с адресным пространством **10.0.0.0/16**;
* как создать две подсети, **appgwsubnet** и **appsubnet**;
* как создать небольшой шлюз приложений, поддерживающий сквозное шифрование SSL и ограничивающий определенные версии протокола SSL и комплекты шифров.

## <a name="before-you-begin"></a>Перед началом работы

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Чтобы настроить сквозной режим связи SSL для шлюза приложений, требуются сертификат для шлюза и сертификаты для внутренних серверов. Сертификат шлюза используется для получения симметричного ключа согласно спецификации протокола SSL. Затем используется симметричный ключ шифрования и расшифровки трафика, отправляемого на шлюз. Сертификат шлюза должен быть представлен в формате PFX (файл обмена личной информацией). Этот формат файла позволяет экспортировать закрытый ключ, необходимый шлюзу приложений для шифрования и расшифровки трафика.

Для сквозного шифрования SSL серверная части должна быть явно разрешена шлюзом приложений. Передайте общий сертификат серверных частей в шлюз приложения. Это гарантирует, что шлюз приложений взаимодействует только с известными экземплярами серверной части, тем самым защищая сквозной обмен данными.

Процесс настройки описан в следующих разделах.

## <a name="create-the-resource-group"></a>Создание группы ресурсов

В этом разделе описывается создание группы ресурсов, содержащей шлюз приложений.

1. Войдите в учетную запись Azure.

   ```powershell
   Connect-AzAccount
   ```

2. Выберите подписку для этого сценария.

   ```powershell
   Select-Azsubscription -SubscriptionName "<Subscription name>"
   ```

3. Создайте группу ресурсов. Если используется существующая группа ресурсов, пропустите этот шаг.

   ```powershell
   New-AzResourceGroup -Name appgw-rg -Location "West US"
   ```

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Создание виртуальной сети и подсети для шлюза приложений.

Следующий пример создает виртуальную сеть и две подсети. Одна подсеть используется для размещения шлюза приложений, а другая — для внутренних серверов, на которых размещается веб-приложение.

1. Назначьте диапазон адресов подсети для шлюза приложений.

   ```powershell
   $gwSubnet = New-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
   ```

   > [!NOTE]
   > Подсети, настроенные для шлюза приложений, должны иметь соответствующий размер. У шлюза приложений может быть до 10 экземпляров. Для каждого экземпляра требуется отдельный IP-адрес из подсети. Слишком маленький размер подсети может отрицательно сказаться на возможности масштабирования шлюза приложений.
   >

2. Назначьте диапазон адресов для внутреннего пула адресов.

   ```powershell
   $nicSubnet = New-AzVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
   ```

3. Создайте виртуальную сеть с подсетями, определенными на предыдущих шагах.

   ```powershell
   $vnet = New-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
   ```

4. Получите ресурс виртуальной сети и ресурсы подсетей, которые будут использоваться при выполнении следующих действий.

   ```powershell
   $vnet = Get-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
   $gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
   $nicSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
   ```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Создание общедоступного IP-адреса для конфигурации интерфейсной части

Создайте ресурс общедоступного IP-адреса для шлюза приложений. Этот общедоступный IP-адрес используется на одном из следующих шагов.

```powershell
$publicip = New-AzPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Шлюз приложений не поддерживает использование общедоступного IP-адреса, созданного с определенной меткой домена. Поддерживается только общедоступный IP-адрес с динамически создаваемой меткой домена. Если для шлюза приложений требуется понятное DNS-имя, мы рекомендуем использовать в качестве псевдонима запись CNAME.

## <a name="create-an-application-gateway-configuration-object"></a>Создание объекта конфигурации шлюза приложений.

Перед созданием шлюза приложений необходимо настроить все элементы конфигурации. В ходе следующих шагов создаются необходимые элементы конфигурации для ресурса шлюза приложений.

1. Создайте IP-конфигурацию шлюза приложений. Она определяет, какую подсеть использует шлюз приложений. При запуске шлюз приложений получает IP-адрес из настроенной подсети. Затем шлюз маршрутизирует сетевой трафик на IP-адреса во внутреннем пуле IP-адресов. Помните, что для каждого экземпляра требуется отдельный IP-адрес.

   ```powershell
   $gipconfig = New-AzApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
   ```

2. Создайте IP-конфигурацию внешнего интерфейса, в которой частный или общедоступный IP-адрес сопоставляется с внешним интерфейсом шлюза приложений. На следующем шаге общедоступный IP-адрес, указанный на предыдущем шаге, будет связан конфигурацией IP внешнего интерфейса.

   ```powershell
   $fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
   ```

3. Настройте IP-адрес внутренних веб-серверов во внутреннем пуле IP-адресов. Эти IP-адреса будут использоваться для получения сетевого трафика от конечной точки с интерфейсным IP-адресом. Замените IP-адреса в примере и добавьте конечные точки IP-адресов своего приложения.

   ```powershell
   $pool = New-AzApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
   ```

   > [!NOTE]
   > Для внутренних серверов вместо IP-адресов можно также использовать полные доменные имена. Для этого используйте параметр **-BackendFqdns**. 

4. Настройте интерфейсный порт IP для конечной точки с общедоступным IP-адресом. Это порт, к которому подключаются пользователи.

   ```powershell
   $fp = New-AzApplicationGatewayFrontendPort -Name 'port01'  -Port 443
   ```

5. Настройте сертификат для шлюза приложений. Этот сертификат используется для шифрования и расшифровки трафика на шлюзе приложений.

   ```powershell
   $passwd = ConvertTo-SecureString  <certificate file password> -AsPlainText -Force 
   $cert = New-AzApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password $passwd 
   ```

   > [!NOTE]
   > Этот пример настраивает сертификат для SSL-соединения. Сертификат должен иметь формат PFX, а пароль к сертификату должен содержать от 4 до 12 символов.

6. Создайте прослушиватель HTTP для шлюза приложений. Назначьте используемую IP-конфигурацию внешнего интерфейса, порт и SSL-сертификат.

   ```powershell
   $listener = New-AzApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
   ```

7. Передайте сертификат, который будет использоваться для ресурсов внутреннего пула, поддерживающих протокол SSL.

   > [!NOTE]
   > Проба по умолчанию получает открытый ключ из привязки SSL *по умолчанию* по IP-адресу серверной части и сравнивает полученное значение открытого ключа со значением, которое предоставляете вы. 
   > 
   > Если на серверной части используются заголовки узлов и указание имени сервера (SNI), полученный открытый ключ не обязательно будет ключом сайта, на который направляется интернет-трафик. Если вы сомневаетесь, перейдите по адресу https://127.0.0.1/ на внутренних серверах, чтобы проверить, какой сертификат используется для привязки SSL *по умолчанию*. В этом разделе используйте открытый ключ из этого запроса. Если в привязках HTTPS используются заголовки узлов и указание имен сервера (SNI) и вы не получаете ответ и сертификат, после того как вручную отправили в браузере запрос на адрес https://127.0.0.1/ для внутренних серверов, на них необходимо настроить привязку SSL по умолчанию. Если этого не сделать, пробы будут завершены ошибкой и серверная часть не будет добавлена в список разрешений.

   ```powershell
   $authcert = New-AzApplicationGatewayAuthenticationCertificate -Name 'allowlistcert1' -CertificateFile C:\cert.cer
   ```

   > [!NOTE]
   > Сертификат, указанный на предыдущем шаге, должен быть открытым ключом PFX-сертификата, присутствующего в серверной части. Экспортируйте сертификат (не корневой) в формате CER, установленный на внутреннем сервере, и используйте его на этом шаге. Это действие добавляет серверную часть в список разрешений шлюза приложений.

   Если вы используете номер SKU Шлюза приложений версии 2, создайте доверенный корневой сертификат, а не сертификат проверки подлинности. Дополнительные сведения см. в статье [Обзор сквозного режима связи SSL в шлюзе приложений](ssl-overview.md#end-to-end-ssl-with-the-v2-sku):

   ```powershell
   $trustedRootCert01 = New-AzApplicationGatewayTrustedRootCertificate -Name "test1" -CertificateFile  <path to root cert file>
   ```

8. Настройте параметры HTTP для серверной части шлюза приложений. В параметрах HTTP назначьте сертификат, переданный на предыдущем шаге.

   ```powershell
   $poolSetting = New-AzApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
   ```

   Для номера SKU Шлюза приложений версии 2 используйте следующую команду:

   ```powershell
   $poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name “setting01” -Port 443 -Protocol Https -CookieBasedAffinity Disabled -TrustedRootCertificate $trustedRootCert01 -HostName "test1"
   ```

9. Создайте правило маршрутизации для балансировщика нагрузки, которое настраивает его поведение. В этом примере создается базовое правило с циклическим перебором.

   ```powershell
   $rule = New-AzApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

10. Настройте размер экземпляра шлюза приложений. Доступные размеры: **Standard\_Small**, **Standard\_Medium** и **Standard\_Large**.  Доступны значения емкости: от **1** до **10**.

    ```powershell
    $sku = New-AzApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
    ```

    > [!NOTE]
    > Для тестирования можно выбрать число экземпляров, равное 1. Важно знать, что если указать число экземпляров менее двух, то развертывание не попадает под действие соглашения об уровне обслуживания, и поэтому это не рекомендуется. Мелкие шлюзы предназначены для разработки и тестирования, а не для производства.

11. Настройте политику SSL для шлюза приложений. Шлюз приложений поддерживает возможность установить минимальную версию для версии протокола SSL.

    Ниже приведен список значений для версий протокола, которые можно определить:

    - **TLSv1_0**;
    - **TLSv1_1**;
    - **TLSv1_2**.
    
    В следующем примере показано, как задать **TLSv1_2** в качестве минимальной версии протокола и разрешить только шифры **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384** и **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256**.

    ```powershell
    $SSLPolicy = New-AzApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -PolicyType Custom
    ```

## <a name="create-the-application-gateway"></a>Создание шлюза приложений

Создайте шлюз приложений в соответствии с действиями, выполненными на предыдущих шагах. Выполнение этого процесса занимает много времени.

Для номера SKU v1 используйте приведенную ниже команду.
```powershell
$appgw = New-AzApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

Для SKU v2 используйте приведенную ниже команду.
```powershell
$appgw = New-AzApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -TrustedRootCertificate $trustedRootCert01 -Verbose
```

## <a name="apply-a-new-certificate-if-the-back-end-certificate-is-expired"></a>Применить новый сертификат, если истек срок действия серверного сертификата

Используйте эту процедуру, чтобы применить новый сертификат, если истек срок действия сертификата внутренней части.

1. Получите шлюз приложений, который требуется обновить.

   ```powershell
   $gw = Get-AzApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```
   
2. Добавьте новый ресурс сертификата из CER-файла, который содержит открытый ключ сертификата, и также может быть тем же сертификатом, добавленным в прослушиватель для завершения SSL в шлюзе приложений.

   ```powershell
   Add-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw -Name 'NewCert' -CertificateFile "appgw_NewCert.cer" 
   ```
    
3. Получите новый объект сертификата проверки подлинности в переменной (TypeName: Microsoft. Azure. Commands. Network. Models. Псаппликатионгатевайаусентикатионцертификате).

   ```powershell
   $AuthCert = Get-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw -Name NewCert
   ```
 
 4. Назначьте новый сертификат параметру **баккендхттп** и используйте переменную $AuthCert. (Укажите имя параметра HTTP, которое требуется изменить.)
 
   ```powershell
   $out= Set-AzApplicationGatewayBackendHttpSetting -ApplicationGateway $gw -Name "HTTP1" -Port 443 -Protocol "Https" -CookieBasedAffinity Disabled -AuthenticationCertificates $Authcert
   ```
    
 5. Зафиксируйте изменения в шлюзе приложений и передайте новую конфигурацию, содержащуюся в переменной $out.
 
   ```powershell
   Set-AzApplicationGateway -ApplicationGateway $gw  
   ```

## <a name="remove-an-unused-expired-certificate-from-http-settings"></a>Удаление неиспользуемого сертификата с истекшим сроком действия из параметров HTTP

Используйте эту процедуру для удаления неиспользуемого сертификата с истекшим сроком действия из параметров HTTP.

1. Получите шлюз приложений, который требуется обновить.

   ```powershell
   $gw = Get-AzApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```
   
2. Перечислите имя сертификата проверки подлинности, который требуется удалить.

   ```powershell
   Get-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw | select name
   ```
    
3. Удалите сертификат проверки подлинности из шлюза приложений.

   ```powershell
   $gw=Remove-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw -Name ExpiredCert
   ```
 
 4. Зафиксируйте изменение.
 
   ```powershell
   Set-AzApplicationGateway -ApplicationGateway $gw
   ```

   
## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a>Ограничение версий протокола SSL на имеющемся шлюзе приложений

Выше было описано создание шлюза приложения со сквозным режимом связи SSL и отключение определенных версий протокола SSL. В следующем примере отключаются определенные политики SSL на существующем шлюзе приложений.

1. Получите шлюз приложений, который требуется обновить.

   ```powershell
   $gw = Get-AzApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```

2. Определите политику SSL. Ниже представлен пример отключения версий **TLSv1.0** и **TLSv1.1** и разрешения только комплектов шифров **TLS\_ECDHE\_ECDSA\_WITH\_AES\_128\_GCM\_SHA256**, **TLS\_ECDHE\_ECDSA\_WITH\_AES\_256\_GCM\_SHA384**, и **TLS\_RSA\_WITH\_AES\_128\_GCM\_SHA256**.

   ```powershell
   Set-AzApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

   ```

3. Наконец, обновите шлюз. Этот последний шаг занимает много времени. По завершении на шлюзе приложений будет настроен сквозной режим связи SSL.

   ```powershell
   $gw | Set-AzApplicationGateway
   ```

## <a name="get-an-application-gateway-dns-name"></a>Получение DNS-имени шлюза приложений

После создания шлюза следует настроить внешний интерфейс для обмена данными. Если вы используете общедоступный IP-адрес, шлюзу приложений требуется динамически назначаемое непонятное имя DNS. Чтобы пользователи гарантированно попали на шлюз приложений, можно использовать запись CNAME для указания общедоступной конечной точки шлюза приложений. Дополнительные сведения см. в статье [Настройка пользовательского доменного имени для облачной службы Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Чтобы настроить псевдоним, извлеките подробные сведения о шлюзе приложений и соответствующий IP-адрес или DNS-имя с помощью элемента **PublicIPAddress**, связанного со шлюзом приложений. Используйте DNS-имя шлюза приложений для создания записи CNAME, указывающей двум веб-приложениям на это DNS-имя. Мы не рекомендуем использовать записи типа A, так как виртуальный IP-адрес может измениться после перезапуска шлюза приложения.

```powershell
Get-AzPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Следующие шаги

Дополнительные сведения об усилении безопасности веб-приложений с помощью брандмауэра веб-приложения в шлюзе приложений см. в статье [Брандмауэр веб-приложения (WAF)](application-gateway-webapplicationfirewall-overview.md).

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
