---
title: Защита веб-служб с помощью SSL
titleSuffix: Azure Machine Learning service
description: Узнайте, как защитить веб-службу, развернутую с помощью службы Машинное обучение Azure, включив протокол HTTPS. Протокол HTTPS обеспечивает защиту данных от клиентов с помощью протокола TLS, заменяющего протоколы SSL. Клиенты также используют протокол HTTPS для проверки удостоверения веб-службы.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 04/29/2019
ms.custom: seodec18
ms.openlocfilehash: ee8af77ce8f3897fdf1cb3da9a125acca28f9419
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68358696"
---
# <a name="use-ssl-to-secure-a-web-service-through-azure-machine-learning"></a>Использование SSL для защиты веб-службы с помощью Машинное обучение Azure

В этой статье показано, как защитить веб-службу, развернутую с помощью службы Машинное обучение Azure.

[Протокол HTTPS](https://en.wikipedia.org/wiki/HTTPS) используется для ограничения доступа к веб-службам и защиты данных, отправляемых клиентами. Протокол HTTPS обеспечивает безопасность обмена данными между клиентом и веб-службой путем шифрования обмена данными между ними. Шифрование использует [протокол TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security). Протокол TLS иногда по-прежнему называется *SSL* (SSL), который был предшественником TLS.

> [!TIP]
> В пакете SDK для Машинное обучение Azure используется термин "SSL" для свойств, связанных с безопасной связью. Это не означает, что веб-служба не использует *TLS*. SSL — это просто более широко распознанный термин.

Протоколы TLS и SSL используют *цифровые сертификаты*, которые помогают в шифровании и проверке личности. Дополнительные сведения о работе цифровых сертификатов см. в статье об инфраструктуре [открытых ключей](https://en.wikipedia.org/wiki/Public_key_infrastructure)в разделе Википедии.

> [!WARNING]
> Если для веб-службы не используется протокол HTTPS, данные, отправляемые в службу и из нее, могут быть видны другим пользователям в Интернете.
>
> HTTPS также позволяет клиенту проверять подлинность сервера, к которому он подключается. Эта функция защищает клиентов от атак типа ["злоумышленник в середине"](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) .

Это общий процесс защиты веб-службы:

1. Получите имя домена.

2. Получение цифрового сертификата.

3. Развертывание или обновление веб-службы с включенным протоколом SSL.

4. Обновите имя DNS, чтобы оно указывало на веб-службу.

> [!IMPORTANT]
> При развертывании в службе Azure Kubernetes Service (AKS) можно приобрести собственный сертификат или использовать сертификат, предоставляемый корпорацией Майкрософт. Если вы используете сертификат от корпорации Майкрософт, вам не нужно получать имя домена или SSL-сертификат. Дополнительные сведения см. в разделе [Включение SSL и развертывание](#enable) этой статьи.

Существуют небольшие отличия при защите веб-служб между [целями развертывания](how-to-deploy-and-where.md).

## <a name="get-a-domain-name"></a>Получение доменного имени

Если вы еще не владеете доменным именем, приобретите его у *регистратора доменных имен*. Процесс и цена отличаются между регистраторами. Регистратор предоставляет средства для управления доменным именем. Эти средства используются для преобразования полного доменного имени (например, www\.contoso.com) в IP-адрес, на котором размещена веб-служба.

## <a name="get-an-ssl-certificate"></a>Получите SSL-сертификат.

Есть много способов получить SSL-сертификат (цифровой сертификат). Наиболее распространенным является приобретение одного из *центра сертификации* (ЦС). Независимо от того, где вы получаете сертификат, вам потребуются следующие файлы:

* **Сертификат**. Сертификат должен содержать всю цепочку сертификатов и должен иметь значение "PEM-Encoded".
* **Ключ**. Ключ также должен быть закодирован в виде PEM.

При запросе сертификата необходимо указать полное доменное имя адреса, который планируется использовать для веб-службы (например, www\.contoso.com). Адрес, который был отмечен в сертификате, и адрес, используемый клиентами, сравнивается с целью проверки удостоверения веб-службы. Если эти адреса не совпадают, клиент получает сообщение об ошибке.

> [!TIP]
> Если центр сертификации не может предоставить сертификат и ключ в виде файлов в кодировке PEM, для изменения формата можно использовать служебную программу, например [OpenSSL](https://www.openssl.org/) .

> [!WARNING]
> Используйте *самозаверяющие* сертификаты только для разработки. Не используйте их в рабочих средах. Самозаверяющие сертификаты могут вызвать проблемы в клиентских приложениях. Дополнительные сведения см. в документации по сетевым библиотекам, используемым клиентским приложением.

## <a id="enable"></a>Включение SSL и развертывание

Чтобы развернуть (или повторно развернуть) службу с включенным протоколом SSL, задайте для параметра *ssl_enabled* значение "true" везде, где это применимо. Присвойте параметру *ssl_certificate* значение файла *сертификата* . Задайте для *ssl_key* значение файла *ключа* .

### <a name="deploy-on-aks-and-field-programmable-gate-array-fpga"></a>Развертывание в AKS и программируемом массиве Gates (FPGA)

  > [!NOTE]
  > Сведения в этом разделе также применяются при развертывании безопасной веб-службы для визуального интерфейса. Если вы не знакомы с использованием пакета SDK для Python, см. статью [что такое пакет SDK для машинное обучение Azure для Python?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

При развертывании в AKS можно создать новый кластер AKS или подключить существующий.
  
-  При создании нового кластера используется **[акскомпуте. provisionining_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none--cluster-purpose-none-)** .
- При присоединении существующего кластера используется **[акскомпуте. attach_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.akscompute#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)** . Оба возвращают объект конфигурации с методом **enable_ssl** .

Метод **enable_ssl** может использовать сертификат, предоставляемый корпорацией Майкрософт или сертификатом, который вы приобрели.

  * При использовании сертификата от корпорации Майкрософт необходимо использовать параметр *leaf_domain_label* . Этот параметр создает DNS-имя для службы. Например, значение «MyService» создает доменное имя «MyService\<с шестью случайными символами» >.\< Благодаря >. cloudapp. Azure. com ", где \<благодаря > — это регион, содержащий службу. При необходимости можно использовать параметр *overwrite_existing_domain* , чтобы перезаписать существующий *leaf_domain_label*.

    Чтобы развернуть (или повторно развернуть) службу с включенным протоколом SSL, задайте для параметра *ssl_enabled* значение "true" везде, где это применимо. Присвойте параметру *ssl_certificate* значение файла *сертификата* . Задайте для *ssl_key* значение файла *ключа* .

    > [!IMPORTANT]
    > При использовании сертификата от корпорации Майкрософт вам не нужно приобретать собственный сертификат или имя домена.

    В следующем примере показано, как создать конфигурацию, которая включает SSL-сертификат от корпорации Майкрософт:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(leaf_domain_label = "myservice")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                          cluster_name = cluster_name)
    attach_config.enable_ssl(leaf_domain_label = "myservice")
    ```

  * При использовании *приобретенного сертификата*вы используете параметры *ssl_cert_pem_file*, *ssl_key_pem_file*и *ssl_cname* . В следующем примере показано, как использовать *PEM* -файлы для создания конфигурации, использующей приобретенный сертификат SSL.

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable SSL
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
    attach_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

Дополнительные сведения о *enable_ssl*см. в разделе [акспровисионингконфигуратион. enable_ssl ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksprovisioningconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-) и [аксаттачконфигуратион. enable_ssl ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksattachconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-).

### <a name="deploy-on-azure-container-instances"></a>Развертывание в службе "экземпляры контейнеров Azure"

При развертывании в службе "экземпляры контейнеров Azure" вы предоставляете значения для параметров, связанных с SSL, как показано в следующем фрагменте кода:

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(
    ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
```

Дополнительные сведения см. в разделе [аЦивебсервице. deploy_configuration ()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none-).

## <a name="update-your-dns"></a>Обновление DNS

Теперь необходимо обновить DNS, чтобы оно указывало на веб-службу.

+ **Для экземпляров контейнеров:**

  Используйте средства регистратора доменных имен, чтобы обновить запись DNS для доменного имени. Запись должна указывать на IP-адрес службы.

  Может быть задержка в минутах или часах, прежде чем клиенты смогут разрешить доменное имя в зависимости от регистратора и срока жизни (TTL), настроенного для доменного имени.

+ **Для AKS:**

  > [!WARNING]
  > Если вы использовали *leaf_domain_label* для создания службы с помощью сертификата от Майкрософт, не обновляйте вручную значение DNS для кластера. Значение должно быть задано автоматически.

  Обновите DNS на вкладке **Конфигурация** общедоступного IP-адреса кластера AKS. (См. следующее изображение.) Общедоступный IP-адрес — это тип ресурса, который создается в группе ресурсов, содержащей узлы агента AKS и другие сетевые ресурсы.

  ![Служба машинного обучения Azure. Защита веб-служб с помощью SSL](./media/how-to-secure-web-service/aks-public-ip-address.png)

## <a name="next-steps"></a>Следующие шаги
Вы узнаете, как выполнять следующие задачи:
+ [Использование модели Машинного обучения, развернутой в качестве веб-службы](how-to-consume-web-service.md)
+ [Безопасный запуск экспериментов и вывод в виртуальной сети Azure](how-to-enable-virtual-network.md)
