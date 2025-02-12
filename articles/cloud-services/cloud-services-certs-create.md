---
title: Облачные службы и сертификаты управления | Документация Майкрософт
description: Узнайте, как создавать и использовать сертификаты с помощью Microsoft Azure
services: cloud-services
documentationcenter: .net
author: georgewallace
ms.service: cloud-services
ms.topic: article
ms.date: 04/19/2017
ms.author: gwallace
ms.openlocfilehash: 3c84c6832856986a45be7d275fb94a6c5fc066f0
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2019
ms.locfileid: "68359190"
---
# <a name="certificates-overview-for-azure-cloud-services"></a>Общие сведения о сертификатах для облачных служб Azure
Сертификаты используются в Azure для облачных служб ([сертификаты службы](#what-are-service-certificates)), а также для аутентификации с помощью службы управления API ([сертификаты управления](#what-are-management-certificates)). В этой статье приводится общий обзор этих двух типов сертификатов, а также описывается процесс их [создания](#create) и развертывания в Azure.

Сертификаты, используемые в Azure, являются сертификатами x.509 v3; они могут быть заверены другим доверенным сертификатом или быть самозаверяющими. Поскольку самозаверяющий сертификат подписывает его автор, он не считается надежным по умолчанию. Большинство браузеров может игнорировать эту проблему. Самозаверяющие сертификаты должны использоваться только при разработке и тестировании облачных служб. 

Сертификаты, используемые в Azure, могут содержать закрытый или открытый ключ. Сертификаты имеют отпечаток, служащий для их однозначной идентификации. Этот отпечаток включен в [файл конфигурации](cloud-services-configure-ssl-certificate-portal.md) Azure для определения сертификата, который будет использоваться облачной службой. 

>[!Note]
>Облачные службы Azure не принимают зашифрованный сертификат AES256-SHA256.

## <a name="what-are-service-certificates"></a>Что такое сертификаты службы
Сертификаты службы присоединяются к облачным службам, обеспечивая безопасное взаимодействие со службой. Например, при развертывании веб-роли вам нужно указать сертификат, который может выполнять проверку подлинности доступной конечной точки HTTPS. Сертификаты службы, определенные в определении службы, автоматически развертываются в виртуальной машине с запущенным экземпляром роли. 

Сертификаты службы можно передать в Azure с помощью портала Azure или классической модели развертывания. Эти сертификаты связаны с конкретной облачной службой. Они назначаются для развертывания в файле определения службы.

Сертификатами службы можно управлять отдельно от служб. Это могут делать разные пользователи. Например, разработчик может передать пакет служб, который ссылается на сертификат, ранее загруженный ИТ-менеджером в Azure. ИТ-менеджер может управлять сертификатом, а также обновлять его (изменяя конфигурацию службы) без необходимости загрузки нового пакета службы. Такое обновление возможно, поскольку логическое имя сертификата, а также имя хранилища и расположение сертификата указаны в файле определения службы, а отпечаток сертификата указан в файле конфигурации службы. Чтобы обновить сертификат, необходимо просто загрузить новый сертификат и изменить значение отпечатка в файле конфигурации службы.

>[!Note]
>В статье [вопросы и ответы по настройке и управлению облачными службами](cloud-services-configuration-and-management-faq.md) содержатся полезные сведения о сертификатах.

## <a name="what-are-management-certificates"></a>Что такое сертификаты управления
Сертификаты управления позволяют выполнять проверку подлинности с помощью классической модели развертывания. Многие программы и инструменты (например, Visual Studio или пакет SDK Azure) будут использовать эти сертификаты для автоматизации настройки и развертывания разных служб Azure. Фактически они не связаны с облачными службами. 

> [!WARNING]
> Будьте осторожны! Эти типы сертификатов позволяют каждому, кто прошел проверку подлинности, управлять подпиской, с которой они связаны. 
> 
> 

### <a name="limitations"></a>Ограничения
Существует ограничение в 100 сертификатов управления на одну подписку. Существует ограничение в 100 сертификатов управления для всех подписок в рамках конкретного идентификатора пользователя администратора службы. Если идентификатор пользователя администратора учетной записи уже используется для добавления 100 сертификатов управления и есть потребность в большем количестве сертификатов, можно добавить соадминистратора для добавления дополнительных сертификатов. 

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Создание самозаверяющего сертификата
Можно использовать любое доступное средство для создания самозаверяющего сертификата при условии, что оно соответствует следующим параметрам.

* Используется сертификат X.509.
* Содержит закрытый ключ.
* Создано для обмена ключами (PFX-файл).
* Имя субъекта должно совпадать с именем домена, которое используется для обращения к облачной службе.

    > Вы не можете приобрести SSL-сертификат для домена cloudapp.net (или для любого другого домена, относящегося к Azure), поэтому имя субъекта сертификата должно совпадать с пользовательским именем домена, которое используется для доступа к вашему приложению. Например, **contoso.net**, а не **contoso.cloudapp.net**.

* Минимум 2048-разрядное шифрование.
* **Только сертификат службы**. Клиентский сертификат должен находиться в *личном* хранилище сертификатов.

Существует два простых способа создания сертификатов в Windows: с помощью служебной программы `makecert.exe` или службы IIS.

### <a name="makecertexe"></a>Makecert.exe
Эта программа устарела и в данном документе не описывается. Дополнительные сведения см. в [этой статье в библиотеке MSDN](/windows/desktop/SecCrypto/makecert).

### <a name="powershell"></a>PowerShell
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My" -KeyLength 2048 -KeySpec "KeyExchange"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> Если вы хотите использовать сертификат, указав IP-адрес, а не домен, задайте этот IP-адрес в параметре -DnsName.


Если вы хотите использовать этот [сертификат на портале управления](../azure-api-management-certs.md), экспортируйте его в **CER-файл** .

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet Information Services (IIS)
В Интернете вы найдете множество информации о том, как это сделать с помощью IIS. [этой странице](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) . 

### <a name="linux"></a>Linux
В [этой](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) статье рассматривается создание сертификатов с использованием SSH.

## <a name="next-steps"></a>Следующие шаги
[Передача сертификата службы на портал Azure](cloud-services-configure-ssl-certificate-portal.md).

Передайте на портал Azure [сертификат API управления](../azure-api-management-certs.md).

