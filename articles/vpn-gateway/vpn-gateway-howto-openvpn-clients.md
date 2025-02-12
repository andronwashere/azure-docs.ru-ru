---
title: Как настроить клиенты OpenVPN для VPN-шлюза Azure | Документация Майкрософт
description: Действия по настройке клиентов OpenVPN для VPN-шлюза Azure
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: cherylmc
ms.openlocfilehash: b8f1626da730178d2cd9c2f31c4f9876102b3d46
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477850"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Настройка клиентов OpenVPN для VPN-шлюза Azure

Эта статья поможет вам настроить **OpenVPN® протокола** клиентов.

## <a name="before-you-begin"></a>Перед началом работы

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Убедитесь, что шаги по настройке OpenVPN для вашего VPN-шлюза выполнены. Дополнительные сведения см. в разделе [Настройка OpenVPN для VPN-шлюза Azure](vpn-gateway-howto-openvpn.md).

## <a name="windows"></a>Клиенты Windows

1. Загрузить и установить клиент OpenVPN (версия 2.4 или более поздней версии) с официальной [OpenVPN веб-сайт](https://openvpn.net/index.php/open-source/downloads.html).
2. Загрузите профиль VPN для шлюза. Это можно сделать со вкладки конфигурации "точка — сеть" на портале Azure или с "New-AzVpnClientConfiguration" в PowerShell.
3. Распакуйте профиль. Затем откройте файл конфигурации *vpnconfig.ovpn* из папки OpenVPN в Блокноте.
4. [Экспортируйте](vpn-gateway-certificates-point-to-site.md#clientexport) сертификат клиента P2S, который вы создали и отправили в конфигурацию P2S на шлюзе.
5. Извлеките закрытый ключ и отпечаток base64 из *PFX-файла*. Это можно сделать несколькими способами. Это можно сделать с помощью OpenSSL на компьютере. Файл *profileinfo.txt* содержит закрытый ключ, отпечаток для ЦС и сертификат клиента. Не забудьте использовать отпечаток сертификата клиента.

   ```
   openssl pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
   ```
6. Откройте *profileinfo.txt* в Блокноте. Чтобы получить отпечаток сертификата клиента (дочерний), выделите текст между "-----BEGIN CERTIFICATE-----" и "-----END CERTIFICATE-----" (включая эти строки) для дочернего сертификата и скопируйте его. Дочерний сертификат можно определить, просмотрев строку subject=/.
7. Переключитесь на файл *vpnconfig.ovpn*, который вы открыли в Блокноте на шаге 3. Найдите раздел, указанный ниже, и замените весь код между cert и /cert.

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. Откройте файл *profileinfo.txt* в Блокноте. Чтобы получить закрытый ключ, выделите текст (включая и между) «---BEGIN PRIVATE KEY---» и «---END PRIVATE KEY---» и скопируйте его.
9. Вернитесь к файлу vpnconfig.ovpn в Блокноте и найдите этот раздел. Вставьте закрытый ключ, заменив все между и key и /key.

   ```
   # P2S client root certificate private key
   # please fill this field with a PEM formatted key
   <key>
   $PRIVATEKEY
   </key>
   ```
10. Не изменяйте остальные поля. Для подключения к VPN используйте заполненную конфигурацию на входе клиента.
11. Скопируйте файл vpnconfig.ovpn в папку C:\Program Files\OpenVPN\config.
12. Щелкните правой кнопкой мыши значок OpenVPN в области уведомлений и щелкните "Подключить".

## <a name="mac"></a>Клиенты Mac

1. Скачайте и установите клиент OpenVPN, такие как [TunnelBlick](https://tunnelblick.net/downloads.html). 
2. Загрузите профиль VPN для шлюза. Это можно сделать со вкладки конфигурации "точка — сеть" на портале Azure или с помощью "New-AzVpnClientConfiguration" в PowerShell.
3. Распакуйте профиль. Откройте файл конфигурации vpnconfig.ovpn из папки OpenVPN в Блокноте.
4. Заполните раздел сертификата клиента подключения "точка — сеть" открытым ключом сертификата клиента P2S в формате base64. В сертификате с форматированием PEM вы можете просто открыть файл CER и скопировать ключ в формате base64, находящийся между заголовками сертификата. Дополнительные сведения о том, как экспортировать сертификат для получения закодированного открытого ключа, см. в разделе [Экспорт открытого ключа (CER)](vpn-gateway-certificates-point-to-site.md#cer).
5. Заполните раздел секретного ключа закрытым ключом сертификата клиента P2S в base64. Дополнительные сведения о том, как извлечь закрытый ключ, см. в разделе [Экспорт закрытого ключа](https://openvpn.net/community-resources/how-to/#pki).
6. Не изменяйте остальные поля. Для подключения к VPN используйте заполненную конфигурацию на входе клиента.
7. Дважды щелкните файл профиля, чтобы создать профиль в Tunnelblick.
8. Запустите Tunnelblick из папки приложения.
9. Щелкните значок Tunnelblick на панели задач и выбора подключения.

> [!IMPORTANT]
>Протокол OpenVPN поддерживают только iOS 11.0 и MacOS 10.13 и их более поздние версии.
>

## <a name="linux"></a>Клиенты Linux

1. Запустите новый сеанс терминала. Новый сеанс можно начать, нажав CTRL+ALT+Т.
2. Для установки необходимых компонентов введите следующую команду.

   ```
   sudo apt-get install openvpn
   sudo apt-get -y install network-manager-openvpn
   sudo service network-manager restart
   ```
3. Загрузите профиль VPN для шлюза. Это можно сделать со вкладки конфигурации "точка — сеть" на портале Azure.
4. [Экспортируйте](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-certificates-point-to-site#clientexport) сертификат клиента P2S, который вы создали и отправили в конфигурацию P2S на шлюзе. 
5. Извлеките закрытый ключ и отпечаток base64 из PFX-файла. Это можно сделать несколькими способами. Это можно сделать с помощью OpenSSL на компьютере.

    ```
    openssl.exe pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
    ```
   Файл *profileinfo.txt* содержит закрытый ключ, отпечаток для ЦС и сертификат клиента. Не забудьте использовать отпечаток сертификата клиента.

6. Откройте файл *profileinfo.txt* в текстовом редакторе. Чтобы получить отпечаток сертификата клиента (дочерний), выделите текст между "-----BEGIN CERTIFICATE-----" и "-----END CERTIFICATE-----", включая эти строки, для дочернего сертификата и скопируйте его. Дочерний сертификат можно определить, просмотрев строку subject=/.

7. Откройте файл *vpnconfig.ovpn* и найдите раздел, показанный ниже. Замените весь код между cert и /cert.

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. Откройте файл profileinfo.txt в текстовом редакторе. Чтобы получить закрытый ключ, выделите текст, включая и их между «---BEGIN ЗАКРЫТЫЙ ключ---» и «---END PRIVATE KEY---» и скопируйте его.

9. Откройте файл vpnconfig.ovpn в текстовом редакторе и найдите этот раздел. Вставьте закрытый ключ, заменив все между и key и /key.

   ```
   # P2S client root certificate private key
   # please fill this field with a PEM formatted key
   <key>
   $PRIVATEKEY
   </key>
   ```

10. Не изменяйте остальные поля. Для подключения к VPN используйте заполненную конфигурацию на входе клиента.
11. Чтобы подключиться с помощью командной строки, введите следующую команду:
  
    ```
    sudo openvpn –-config <name and path of your VPN profile file>
    ```
12. Чтобы подключиться с помощью графического пользовательского интерфейса, перейдите в параметры системы.
13. Щелкните **+** для добавления нового подключения VPN.
14. В разделе **Добавить VPN** выберите **Импорт из файла...**
15. Просмотрите файл профиля и дважды щелкните или выберите **Открыть**.
16. Щелкните **Добавить** в окне **Добавить VPN**.
  
    ![Импорт из файла](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
17. Вы можете подключиться, выбрав для VPN **Вкл.** на странице **Параметры сети** или под значком сети на панели задач.

## <a name="next-steps"></a>Дальнейшие действия

Если требуется VPN-клиенты могли получить доступ к ресурсам в другой виртуальной сети, следуйте инструкциям [VNet-to-VNet](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) статью, чтобы установить подключение к сети. Обязательно включите протокол BGP на шлюзах и соединениях, иначе трафик не будет считываться.

**«OpenVPN» является товарным знаком OpenVPN Inc.**
