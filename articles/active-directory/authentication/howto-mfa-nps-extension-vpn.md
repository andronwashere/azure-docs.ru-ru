---
title: Интеграция VPN с Azure MFA с помощью расширения сервера политики сети — Azure Active Directory
description: Интеграция инфраструктуры VPN с Azure MFA с помощью расширения сервера политики сети для Microsoft Azure.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4afe5c89a83f251ee354c955b6dc28b0bccc9e6c
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/04/2019
ms.locfileid: "67561128"
---
# <a name="integrate-your-vpn-infrastructure-with-azure-mfa-by-using-the-network-policy-server-extension-for-azure"></a>Интеграция инфраструктуры VPN с Azure MFA с помощью расширения сервера политики сети для Azure

## <a name="overview"></a>Обзор

Расширение сервера политики сети (NPS) для Azure позволяет организациям защитить аутентификацию клиентов RADIUS с помощью облачной [Многофакторной идентификации (MFA) Azure](howto-mfaserver-nps-rdg.md), обеспечивающей двухфакторную проверку подлинности.

В этой статье приведены инструкции по интеграции инфраструктуры NPS с Azure MFA с помощью расширения NPS для Azure. Этот процесс обеспечивает безопасную двухфакторную проверку подлинности пользователей, которые пытаются подключиться к вашей сети с помощью VPN.

Службы политики сети и доступа дают организациям следующие возможности.

* Назначение центрального расположения для управления сетевыми запросами и их контроля, позволяющего указать:

  * кто может подключаться;

  * в какое время дня подключения разрешены;

  * длительность подключений;

  * уровень безопасности, который клиенты должны использовать для подключения.

    Вместо того, чтобы указывать политики на каждом VPN-сервере или сервере шлюза удаленных рабочих столов, их можно указать в центральном расположении. Протокол RADIUS обеспечивает централизованную аутентификацию, авторизацию и учет.

* Можно устанавливать и применять политики работоспособности клиента защиты доступа к сети (NAP), которые определяют, предоставляется ли устройствам неограниченный или ограниченный доступ к сетевым ресурсам.

* Можно внедрить средства аутентификации и авторизации для доступа к точкам беспроводного доступа и коммутаторам Ethernet, поддерживающим протокол 802.1x.
  Дополнительную информацию см. в разделе [Сервер политики сети (NPS)](https://docs.microsoft.com/windows-server/networking/technologies/nps/nps-top).

Чтобы повысить безопасность и обеспечить высокий уровень соответствия, организации могут интегрировать сервер политики сети с Azure MFA, чтобы применять двухфакторную проверку подлинности пользователей, подключающихся к виртуальному порту на VPN-сервере. Чтобы получить доступ, пользователю необходимо предоставить свои имя пользователя и пароль, а также другие сведения, которыми он управляет. Эти сведения должны быть надежными и их должно быть сложно скопировать. Это может быть номер мобильного телефона, номер стационарного телефона или приложение на мобильном устройстве.

До появления расширения NPS для Azure клиентам, желавшим внедрить двухфакторную проверку подлинности для интегрированных сред NPS и MFA, приходилось настраивать и обслуживать отдельный сервер MFA в локальной среде. Аутентификацию такого типа обеспечивают шлюз удаленных рабочих столов и сервер Многофакторной идентификации Azure, использующие RADIUS.

Используя расширение NPS для Azure, организации имеют возможность развернуть локальное или облачное решение MFA для защиты аутентификации клиентов RADIUS.

## <a name="authentication-flow"></a>Поток проверки подлинности

Когда пользователи подключаются к виртуальному порту на VPN-сервере, они должны сначала пройти аутентификацию с помощью различных протоколов. Протоколы позволяют использовать сочетание имени пользователя и пароля, а также методы проверки подлинности на основе сертификатов.

Помимо выполнения аутентификации и проверки удостоверения, пользователи должны иметь соответствующие разрешения на входящее подключение. В простых реализациях эти разрешения на входящее подключение, которые обеспечивают доступ, задаются непосредственно в объектах-пользователях Active Directory.

![Вкладка в свойствах пользователя Active Directory — пользователи и компьютеры](./media/howto-mfa-nps-extension-vpn/image1.png)

В простых реализациях каждый VPN-сервер предоставляет или запрещает доступ на основе политик, определенных на каждом локальном VPN-сервере.

В крупных и более масштабных реализациях политики, предоставляющие или запрещающие доступ через VPN, сосредоточены на серверах RADIUS. В этих случаях VPN-сервер действует как сервер доступа (клиент RADIUS), который перенаправляет запросы на подключение и сообщения учетной записи на сервер RADIUS. Чтобы подключиться к виртуальному порту на VPN-сервере, пользователи должны пройти аутентификацию и выполнить условия, определенные централизованно на серверах RADIUS.

Когда расширение NPS для Azure интегрировано с сервером политики сети, успешная аутентификация выглядит следующим образом:

1. VPN-сервер получает запрос на аутентификацию от пользователя VPN, содержащий имя пользователя и пароль, для подключения к ресурсу (например, к сеансу удаленного рабочего стола).
2. Выступая в качестве клиента RADIUS, VPN-сервер преобразовывает этот запрос в сообщение *Access-Request* RADIUS (пароль при этом шифруется) и отправляет его на сервер RADIUS, на котором установлено расширение NPS.
3. Сочетание имени пользователя и пароля проверяется в Active Directory. Если имя пользователя и пароль указаны неправильно, сервер RADIUS отправляет сообщение *Access-Reject*.
4. Если выполнены все условия, указанные в запросе на подключение NPS и политиках сети (например, ограничение времени дня или членства в группах), расширение NPS активирует запрос на дополнительную аутентификацию с помощью Azure MFA.
5. Служба Azure MFA обращается к Azure Active Directory, получает сведения о пользователе и выполняет дополнительную аутентификацию по тому методу, который настроен пользователем (звонок на мобильный телефон, текстовое сообщение или мобильное приложение).
6. В случае успешного выполнения запроса защиты MFA служба Azure MFA передает результат в расширение NPS.
7. После аутентификации и авторизации подключения сервер политики сети, на котором установлено расширение NPS, отправляет сообщение *Access-Accept* RADIUS на VPN-сервер (клиент RADIUS).
8. Пользователю предоставляется доступ к виртуальному порту на VPN-сервере, и создается зашифрованный VPN-туннель.

## <a name="prerequisites"></a>Технические условия

В этом разделе описаны предварительные условия, которые должны быть завершены перед выполнением многофакторной проверки Подлинности можно интегрировать с виртуальной частной сети. Прежде чем приступить к работе, следует подготовить приведенные ниже необходимые компоненты:

* Инфраструктура VPN
* роль "Службы политики сети и доступа";
* лицензия Azure MFA;
* программное обеспечение Windows Server;
* Библиотеки
* синхронизация Azure Active Directory (Azure AD) с локальной службой Active Directory;
* идентификатор GUID Azure Active Directory.

### <a name="vpn-infrastructure"></a>Инфраструктура VPN

В этой статье предполагается наличие рабочей инфраструктуры VPN на основе Microsoft Windows Server 2016. Также предполагается, что VPN-сервер не настроен для пересылки запросов на подключение на сервер RADIUS. В этой статье вы настроите инфраструктуру VPN для использования центрального сервера RADIUS.

Если у вас нет рабочей инфраструктуры VPN, можно быстро создать ее, следуя инструкциям в многочисленных руководствах по установке VPN, которые можно найти на сайтах корпорации Майкрософт и сайтах сторонних поставщиков.

### <a name="the-network-policy-and-access-services-role"></a>Роль "Службы политики сети и доступа"

Роль "Службы политики сети и доступа" обеспечивает функциональные возможности сервера и клиента RADIUS. В этой статье предполагается, что вы установили роль "Службы политики сети и доступа" на рядовой сервер или контроллер домена в своей среде. Вы настроите RADIUS для использования конфигурации VPN в данном руководстве. Установите роль "Службы политики сети и доступа" на сервере, *отличном от* VPN-сервера.

Дополнительные сведения об установке службы роли "Службы политики сети и доступа" на платформе Windows Server 2012 или более поздней версии см. в разделе [Install a NAP Health Policy Server](https://technet.microsoft.com/library/dd296890.aspx) (Установка сервера политики работоспособности NAP). Служба NAP объявлена нерекомендуемой в Windows Server 2016. Описание рекомендаций для сервера политики сети, включая рекомендации по установке сервера политики сети на контроллер домена, см. в разделе [Best Practices for NPS](https://technet.microsoft.com/library/cc771746) (Рекомендации для сервера политики сети).

### <a name="azure-mfa-license"></a>лицензия Azure MFA;

Для работы с Azure MFA требуется лицензия. Ее можно получить, приобретя отдельную лицензию на решение Azure AD Premium, Enterprise Mobility + Security или "Многофакторная идентификация". Лицензии на основе потребления Azure MFA, такие как лицензии на пользователя или лицензии на аутентификацию, несовместимы с расширением NPS. Дополнительные сведения см. в разделе [Как получить службу Многофакторной идентификации Azure](concept-mfa-licensing.md). Для тестирования можно использовать пробную подписку.

### <a name="windows-server-software"></a>программное обеспечение Windows Server;

Для работы расширения NPS требуется Windows Server 2008 R2 с пакетом обновления 1 (SP1) или более поздней версии с установленной ролью "Службы политики сети и доступа". Все действия в этом руководстве были выполнены с помощью Windows Server 2016.

### <a name="libraries"></a>Библиотеки

Приведенные ниже библиотеки устанавливаются автоматически вместе с расширением NPS.

-   [Распространяемые пакеты Visual C++ для Visual Studio 2013 (X64)](https://www.microsoft.com/download/details.aspx?id=40784)
-   [Модуль Microsoft Azure Active Directory для Windows PowerShell, версия 1.1.166](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)

Если модуль PowerShell для Microsoft Azure Active Directory отсутствует, то он устанавливается с помощью сценария настройки, который запускается в процессе установки. Нет необходимости устанавливать этот модуль заранее.

### <a name="azure-active-directory-synced-with-on-premises-active-directory"></a>Синхронизация Azure Active Directory с локальной службой Active Directory

Для использования расширения NPS локальные пользователи должны быть синхронизированы с Azure Active Directory и настроены для использования MFA. В этом руководстве предполагается, что синхронизация локальных пользователей с Azure Active Directory выполнена с помощью Azure AD Connect. Ниже приведены инструкции по включению Многофакторной идентификации для пользователей.

Дополнительные сведения об Azure AD Connect см. в статье [История выпусков версий соединителей](../hybrid/whatis-hybrid-identity.md).

### <a name="azure-active-directory-guid-id"></a>идентификатор GUID Azure Active Directory.

Чтобы установить расширение NPS, необходимо знать идентификатор GUID Azure Active Directory. Инструкции по поиску идентификатора GUID Azure Active Directory приведены в следующем разделе.

## <a name="configure-radius-for-vpn-connections"></a>Настройка RADIUS для VPN-подключений

Если вы установили роль NPS на рядовой сервер, ее необходимо настроить для аутентификации и авторизации VPN-клиента, запрашивающего VPN-подключение. 

В этом разделе предполагается, вы установили роль "Службы политики сети и доступа", но еще не настроили ее для использования в инфраструктуре.

> [!NOTE]
> Если у вас уже имеется рабочий VPN-сервер, использующий центральный сервер RADIUS для аутентификации, то этот раздел можно пропустить.
>

### <a name="register-server-in-active-directory"></a>Регистрация сервера в Active Directory

Для правильной работы в этом сценарии NPS-сервер должен быть зарегистрирован в Active Directory.

1. Откройте диспетчер сервера.

2. В диспетчере сервера щелкните **Средства**, а затем щелкните **Сервер политики сети**.

3. В консоли сервера политики сети щелкните правой кнопкой мыши **Сервер сетевых политик (локальный)** , а затем щелкните **Зарегистрировать сервер в Active Directory**. Дважды нажмите кнопку **ОК**.

    ![Регистрация сервера в пункте меню Active Directory](./media/howto-mfa-nps-extension-vpn/image2.png)

4. Оставьте консоль открытой для выполнения следующей процедуры.

### <a name="use-wizard-to-configure-the-radius-server"></a>Использование мастера для настройки сервера RADIUS

Для настройки сервера RADIUS можно использовать стандартную (с помощью мастера) или расширенную настройку. В этом разделе предполагается использование стандартной настройки с помощью мастера.

1. В консоли сервера политики сети щелкните **Сервер сетевых политик (локальный)** .

2. В разделе **Стандартная конфигурация** выберите **RADIUS-сервер для подключений удаленного доступа или VPN**, а затем щелкните **Настройка VPN или удаленного доступа**.

    ![Настройка сервера RADIUS для удаленного доступа или VPN-подключений](./media/howto-mfa-nps-extension-vpn/image3.png)

3. В окне **Выберите тип подключения - удаленный доступ или виртуальная частная сеть** выберите **Virtual Private Network Connections** (Подключения к виртуальной частной сети) и нажмите кнопку **Далее**.

    ![Настройка подключений виртуальной частной сети](./media/howto-mfa-nps-extension-vpn/image4.png)

4. В окне **Укажите сервер удаленного доступа или VPN-сервер** щелкните **Добавить**.

5. В окне **Новый RADIUS-клиент** введите понятное имя, разрешаемое имя или IP-адрес VPN-сервера и общий секретный пароль. Этот секретный пароль должен быть длинным и сложным. Запишите его, так как он понадобится в следующем разделе.

    ![Создать окно клиента новый RADIUS](./media/howto-mfa-nps-extension-vpn/image5.png)

6. Нажмите кнопку **ОК**, а затем щелкните **Создать**.

7. В окне **Настройка методов проверки подлинности** примите параметр по умолчанию (**Шифрованная проверка (Microsoft, версия 2, MS-CHAP v2)** ) или выберите другой параметр, после чего нажмите кнопку **Далее**.

    > [!NOTE]
    > При настройке протокола EAP необходимо использовать протокол проверки пароля CHAPv2 (Майкрософт) или протокол PEAP. Другие варианты EAP не поддерживаются.

8. В окне **Настройка групп пользователей** щелкните **Добавить** и выберите соответствующую группу. Если группа не существует, оставьте поле пустым, чтобы предоставить доступ всем пользователям.

    ![Укажите окно группы пользователей, чтобы разрешить или запретить доступ](./media/howto-mfa-nps-extension-vpn/image7.png)

9. Щелкните **Далее**.

10. В окне **Задайте IP-фильтры** щелкните **Далее**.

11. В окне **Задайте параметры шифрования** примите параметры по умолчанию и нажмите кнопку **Далее**.

    ![Окно "Задайте параметры шифрования"](./media/howto-mfa-nps-extension-vpn/image8.png)

12. В окне **Укажите имя сферы** не указывайте имя, примите параметр по умолчанию и нажмите кнопку **Далее**.

    ![Окно "Укажите имя сферы"](./media/howto-mfa-nps-extension-vpn/image9.png)

13. В окне **Завершение создания подключений удаленного доступа или виртуальной частной сети и RADIUS-клиентов** нажмите кнопку **Готово**.

    ![Настройка окна](./media/howto-mfa-nps-extension-vpn/image10.png)

### <a name="verify-the-radius-configuration"></a>Проверка конфигурации RADIUS

В этом разделе описана конфигурация, созданная с помощью мастера.

1. На сервере политики сети в консоли "Сервер сетевых политик (локальный)" разверните элемент **RADIUS-клиенты** и выберите **RADIUS-клиенты**.

2. В области сведений щелкните правой кнопкой мыши созданный вами клиент RADIUS и выберите **Свойства**. Свойства клиента RADIUS (VPN-сервера) должны быть аналогичны показанным ниже.

    ![Проверьте свойства VPN и конфигурации](./media/howto-mfa-nps-extension-vpn/image11.png)

3. Нажмите кнопку **Отмена**.

4. На сервере политики сети в консоли "Сервер сетевых политик (локальный)" разверните элемент **Политики** и выберите **Политики запросов на подключение**. Отобразится политика VPN-подключений, как показано на следующем рисунке.

    ![Отображение политики подключения VPN политики запросов на подключение](./media/howto-mfa-nps-extension-vpn/image12.png)

5. В разделе **Политики** выберите **Сетевые политики**. Вы должны увидеть политику VPN-подключений, напоминающую политику, показанную на рисунке ниже.

    ![Политики сети, виртуальные частные сети подключения политики](./media/howto-mfa-nps-extension-vpn/image13.png)

## <a name="configure-your-vpn-server-to-use-radius-authentication"></a>Настройка VPN-сервера для использования аутентификации RADIUS

В этом разделе вы настроите VPN-сервер для использования аутентификации RADIUS. В инструкциях предполагается, что у вас имеется рабочая конфигурация VPN-сервера, но вы не настроили его для использования аутентификации RADIUS. После настройки VPN-сервера убедитесь, что конфигурация работает надлежащим образом.

> [!NOTE]
> Если у вас уже имеется рабочая конфигурация VPN-сервера, использующего аутентификацию RADIUS, то этот раздел можно пропустить.
>

### <a name="configure-authentication-provider"></a>Настройка поставщика проверки подлинности

1. На VPN-сервере откройте диспетчер сервера.

2. В диспетчере сервера выберите **Средства**, а затем — **Маршрутизация и удаленный доступ**.

3. В окне **Маршрутизация и удаленный доступ** щелкните правой кнопкой мыши **\<имя_сервера> (локальный)** , а затем выберите **Свойства**.

4. В окне **\<имя_сервера> (локальный) - свойства** щелкните вкладку **Безопасность**.

5. На вкладке **Безопасность** в разделе **Поставщик проверки подлинности** щелкните **Проверка подлинности RADIUS**, а затем — **Настройка**.

    ![Настройка поставщика проверки подлинности RADIUS](./media/howto-mfa-nps-extension-vpn/image15.png)

6. В окне **Проверка подлинности RADIUS** щелкните **Добавить**.

7. В окне **Добавление RADIUS-сервера** выполните следующие действия.

    a. В поле **Имя сервера** добавьте имя или IP-адрес сервера RADIUS, настроенного в предыдущем разделе.

    2\. В разделе **Общий секрет** щелкните **Изменить** и введите общий секретный пароль, который вы создали и записали ранее.

    c. В **время ожидания (в секундах)** введите значение **30**.  
    Это необходимо, чтобы предоставить достаточно времени для второго фактора аутентификации.

    ![Добавление окна RADIUS-сервер, Настройка времени ожидания](./media/howto-mfa-nps-extension-vpn/image16.png)

8. Нажмите кнопку **ОК**.

### <a name="test-vpn-connectivity"></a>Проверка VPN-подключения

В этом разделе вы убедитесь, что VPN-клиент проходит аутентификацию и авторизацию на сервере RADIUS при попытке подключения к виртуальному порту VPN. В инструкциях предполагается, что вы используете VPN-клиент на платформе Windows 10.

> [!NOTE]
> Если вы настроили VPN-клиент для подключения к VPN-серверу и сохранили параметры, то можете пропустить шаги, относящиеся к настройке и сохранению объекта VPN-подключения.
>

1. На компьютере VPN-клиента нажмите кнопку **Пуск**, затем — кнопку **Параметры**.

2. В окне **Параметры Windows** выберите **Сеть и Интернет**.

3. Выберите **VPN**.

4. Щелкните **Добавить VPN-подключение**.

5. В окне **Добавление VPN-подключения** в поле **Поставщик услуг VPN** выберите **Windows (встроенная)** , а затем заполните оставшиеся поля соответствующим образом и нажмите кнопку **Сохранить**.

    ![Окно "Добавление VPN-подключения"](./media/howto-mfa-nps-extension-vpn/image17.png)

6. Перейдите к **панели управления**, а затем щелкните **Управление сетями и общим доступом**.

7. Щелкните **Изменение параметров адаптера**.

    ![Центр управления сетями — изменение параметров адаптера](./media/howto-mfa-nps-extension-vpn/image18.png)

8. Щелкните правой кнопкой мыши VPN-подключение и выберите **Свойства**.

9. В окне свойств VPN щелкните вкладку **Безопасность**.

10. На вкладке **Безопасность** убедитесь, что установлен только флажок **Протокол MS-CHAP, версия 2**, и нажмите кнопку **ОК**.

    ![Параметр "Разрешить следующие протоколы"](./media/howto-mfa-nps-extension-vpn/image20.png)

11. Щелкните правой кнопкой мыши VPN-подключение и выберите **Подключить**.

12. В окне **Параметры** выберите **Подключить**.  
    Успешное подключение отобразится в журнале безопасности на сервере RADIUS, как события с Идентификатором 6272, как показано ниже:

    ![Окно свойств событий, отображающее успешное подключение](./media/howto-mfa-nps-extension-vpn/image21.png)

## <a name="troubleshooting-radius"></a>Устранение неполадок с RADIUS

Предположим, конфигурация VPN работала до того, как вы настроили VPN-сервер для использования центрального сервера RADIUS для аутентификации и авторизации. Если конфигурация работала, вероятнее всего, проблема заключается в неправильной настройке сервера RADIUS либо использовании недействительного имени пользователя или пароля. Например, если вы указали альтернативный суффикс имени участника-пользователя в имени пользователя, попытка входа может завершиться сбоем. Используйте одно и то же имя учетной записи для получения наилучших результатов.

Чтобы устранить эти неполадки, лучше всего начать с проверки журналов событий безопасности на сервере RADIUS. Чтобы сэкономить время при поиске событий, можно использовать настраиваемое представление политик сети и сервера доступа в службе "Просмотр событий", как показано на следующем рисунке. Идентификатор события 6273 обозначает события, в которых сервер политики сети отказал пользователю в доступе.

![Отображение NPAS событий средства просмотра событий](./media/howto-mfa-nps-extension-vpn/image22.png)

## <a name="configure-multi-factor-authentication"></a>Настройка Многофакторной идентификации

Помощь в настройке пользователей для многофакторной проверки подлинности см. в статьях [Планирование развертывания многофакторной идентификации Azure облачной](howto-mfa-getstarted.md#create-conditional-access-policy) и [Настройка учетной записи для двухфакторной проверки подлинности](../user-help/multi-factor-authentication-end-user-first-time.md)

## <a name="install-and-configure-the-nps-extension"></a>Установка и настройка расширения NPS

Этот раздел содержит инструкции по настройке инфраструктуры VPN для использования MFA для аутентификации клиентов с помощью VPN-сервера.

После установки и настройки расширения NPS во всех операциях аутентификации клиентов RADIUS, обрабатываемых этим сервером, будет обязательно использование MFA. Если все ваши пользователи VPN не зарегистрированы в службе Azure MFA, необходимо выполнить одно из следующих действий.

* Настройте еще один сервер RADIUS для аутентификации пользователей, для которых не настроено использование MFA.

* Создайте запись реестра, позволяющую проверяемым пользователям использовать второй фактор аутентификации только в том случае, если они зарегистрированы в службе Azure MFA.

Создайте новый строковый параметр _REQUIRE_USER_MATCH в разделе HKLM\SOFTWARE\Microsoft\AzureMfa_ и задайте для него значение *True* или *False*.

![Параметр "Требование сопоставления пользователей"](./media/howto-mfa-nps-extension-vpn/image34.png)

Если задано значение *True* или значение не указано, то все запросы на аутентификацию обрабатываются с помощью запроса защиты MFA. Если задано значение *False*, то запросы защиты MFA выдаются только для пользователей, зарегистрированных в службе Azure MFA. Используйте параметр со значением *False* в тестовой или рабочей средах только в период подключения.

### <a name="obtain-the-azure-active-directory-guid-id"></a>Получение идентификатора GUID Azure Active Directory

При настройке расширения NPS необходимо ввести учетные данные администратора и идентификатор клиента Azure AD. Получите этот идентификатор следующим образом.

1. Войдите на [портал Azure](https://portal.azure.com) как глобальный администратор клиента Azure.

2. В области слева нажмите кнопку **Azure Active Directory**.

3. Выберите **Свойства**.

4. Чтобы скопировать идентификатор Azure AD, нажмите кнопку **Копировать**.

    ![Идентификатор каталога Azure AD на портале Azure](./media/howto-mfa-nps-extension-vpn/image35.png)

### <a name="install-the-nps-extension"></a>Установка расширения NPS

Расширение NPS должно быть установлено на сервере, на котором установлена роль "Службы политики сети и доступа" и который действует как сервер RADIUS в инфраструктуре. Сделать *не* устанавливать расширение NPS на сервере VPN.

1. Скачайте расширение NPS из [Центра загрузки Майкрософт](https://aka.ms/npsmfa).

2. Скопируйте исполняемый установочный файл (*NpsExtnForAzureMfaInstaller.exe*) на NPS-сервер.

3. На NPS-сервере дважды щелкните файл **NpsExtnForAzureMfaInstaller.exe** и, если появится соответствующий запрос, выберите **Запустить**.

4. В окне **NPS Extension For Azure MFA Setup** (Установка расширения NPS для Azure MFA) ознакомьтесь с условиями лицензии на программное обеспечение, установите флажок **Я подтверждаю согласие с условиями лицензии** и нажмите кнопку **Установить**.

    ![Окно "NPS Extension For Azure MFA Setup" (Установка расширения NPS для Azure MFA)](./media/howto-mfa-nps-extension-vpn/image36.png)

5. В окне **NPS Extension For Azure MFA Setup** (Установка расширения NPS для Azure MFA) щелкните **Закрыть**.  

    ![Окно подтверждения "Установка успешно завершена"](./media/howto-mfa-nps-extension-vpn/image37.png)

### <a name="configure-certificates-for-use-with-the-nps-extension-by-using-a-powershell-script"></a>Настройка сертификатов для расширения NPS с помощью сценария PowerShell

Чтобы обеспечить безопасную связь и контроль, настройте сертификаты для расширения NPS. Компоненты NPS включают в себя сценарий Windows PowerShell, предназначенный для настройки самозаверяющего сертификата для сервера политики сети.

Этот сценарий выполняет следующие действия:

* создает самозаверяющий сертификат;
* связывает открытый ключ сертификата с субъектом-службой в Azure AD;
* сохраняет сертификат в хранилище на локальном компьютере;
* предоставляет сетевому пользователю доступ к закрытому ключу сертификата;
* перезапускает службу NPS.

Если вы хотите использовать собственные сертификаты, необходимо привязать открытый ключ сертификата к субъекту-службе в Azure AD и т. д.

Чтобы использовать этот сценарий, укажите в расширении свои учетные данные администратора Azure Active Directory и идентификатор клиента Azure Active Directory, скопированный ранее. Запустите сценарий на каждом NPS-сервере, на котором установлено расширение NPS.

1. Запустите Windows PowerShell от имени администратора.

2. В командной строке PowerShell введите команду **cd "c:\Program Files\Microsoft\AzureMfa\Config"** и нажмите клавишу ВВОД.

3. В следующей командной строке введите **.\AzureMfaNpsExtnConfigSetup.ps1**, а затем нажмите клавишу ВВОД. Сценарий проверяет, установлен ли модуль PowerShell для Azure Active Directory. Если он не установлен, сценарий автоматически установит этот модуль.

    ![Выполнение сценария конфигурации AzureMfsNpsExtnConfigSetup.ps1](./media/howto-mfa-nps-extension-vpn/image38.png)

    Проверив установку модуля PowerShell, сценарий отобразит диалоговое окно входа для модуля PowerShell Azure для Active Directory.

4. Введите учетные данные администратора Azure AD, включая пароль, и щелкните **Вход**.

    ![Проверку подлинности в Azure AD PowerShell](./media/howto-mfa-nps-extension-vpn/image39.png)

5. Вставьте идентификатор клиента, скопированный ранее, в командную строку и нажмите клавишу ВВОД.

    ![Входные данные с Идентификатором каталога Azure AD, скопированы, прежде чем](./media/howto-mfa-nps-extension-vpn/image40.png)

    Этот сценарий создает самозаверяющий сертификат и выполняет другие изменения конфигурация. Выходные данные должны иметь вид, как показано на рисунке ниже.

    ![Самозаверяющий сертификат и отображение в окне PowerShell](./media/howto-mfa-nps-extension-vpn/image41.png)

6. Перезагрузите сервер.

### <a name="verify-the-configuration"></a>Проверка конфигурации

Чтобы проверить конфигурацию, необходимо установить новое VPN-подключение к VPN-серверу. После успешного ввода учетных данных для основной аутентификации VPN-подключение ожидает прохождения дополнительной аутентификации, прежде чем подключение будет установлено, как показано ниже.

![Окно настройки VPN в Windows](./media/howto-mfa-nps-extension-vpn/image42.png)

После успешного прохождения дополнительной проверки, настроенной ранее в службе Azure MFA, вы будете подключены к ресурсу. Однако если вы не пройдете дополнительную аутентификацию, доступ к ресурсу будет запрещен.

В следующем примере приложение Microsoft Authenticator в Windows Phone используется для дополнительной аутентификации.

![Пример запрос на многофакторную Идентификацию на Windows Phone](./media/howto-mfa-nps-extension-vpn/image43.png)

После успешного прохождения аутентификации с помощью дополнительного метода вам предоставляется доступ к виртуальному порту на VPN-сервере. Так как вы должны были пройти дополнительную аутентификацию с помощью мобильного приложения на доверенном устройстве, процесс входа защищен лучше, чем при использовании только имени пользователя и пароля.

### <a name="view-event-viewer-logs-for-successful-sign-in-events"></a>Просмотр событий успешного входа в журналах службы "Просмотр событий"

Чтобы просмотреть события успешного входа в журналах просмотра событий Windows запрос журнале безопасности Windows на сервере политики сети, введя следующую команду PowerShell:

    `Get-WinEvent -Logname Security | where {$_.ID -eq '6272'} | FL`

![Запрос журналов безопасности из службы "Просмотр событий" с помощью PowerShell](./media/howto-mfa-nps-extension-vpn/image44.png)

Можно также просмотреть настраиваемое представление "Службы политики сети и доступа", как показано ниже.

![Пример журнала сервера политики сети](./media/howto-mfa-nps-extension-vpn/image45.png)

На сервере с расширением NPS для Azure MFA можно найти журналы приложений службы "Просмотр событий", относящиеся к этому расширению: *Application and Services Logs\Microsoft\AzureMfa*.

    `Get-WinEvent -Logname Security | where {$_.ID -eq '6272'} | FL`

![Панель журналов пример AuthZ средства просмотра событий](./media/howto-mfa-nps-extension-vpn/image46.png)

## <a name="troubleshooting-guide"></a>Руководство по устранению неполадок

Если конфигурация не работает должным образом, то, чтобы устранить неполадку, в первую очередь следует убедиться, что пользователь настроен для использования MFA. Подключите пользователя к [порталу Azure](https://portal.azure.com). Если ему предлагается дополнительная аутентификация и он может успешно пройти ее, то можно исключить ошибку в конфигурации MFA.

Если пользователь успешно использует MFA, следует изучить соответствующие журналы службы "Просмотр событий". К ним относятся журналы событий безопасности, операционные журналы шлюза и журналы Azure MFA, рассмотренные в предыдущем разделе.

Ниже показан пример журнала безопасности, который содержит событие входа, завершившегося сбоем (идентификатор события 6273).

![Журнал безопасности, который содержит событие входа, завершившееся сбоем](./media/howto-mfa-nps-extension-vpn/image47.png)

Ниже показано связанное событие из журнала Azure MFA.

![Журналы Azure MFA](./media/howto-mfa-nps-extension-vpn/image48.png)

Чтобы расширить возможности устранения неполадок, просмотрите файлы журнала в формате базы данных NPS в расположении, в котором установлена служба NPS. Эти файлы журналов создаются в папке _%SystemRoot%\System32\Logs_ в виде текстовых файлов с разделителями-запятыми. Описание этих файлов журнала см. в разделе [Interpret NPS Database Format Log Files](https://technet.microsoft.com/library/cc771748.aspx) (Интерпретация файлов журнала в формате базы данных NPS).

Записи в этих файлах журнала сложно интерпретировать, не импортировав их в электронную таблицу или базу данных. В Интернете можно найти множество инструментов анализа для службы проверки подлинности в Интернете (IAS), которые упростят интерпретацию файлов журнала. Ниже показаны выходные данные одной из таких скачиваемых [условно бесплатных программ](https://www.deepsoftware.com/iasviewer).

![Пример средства синтаксического анализа для условно-бесплатных приложений IAS](./media/howto-mfa-nps-extension-vpn/image49.png)

Чтобы еще больше расширить возможности устранения неполадок, можно использовать анализатор протокола, например Wireshark или [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx). На следующем снимке экрана Wireshark показаны сообщения RADIUS, передаваемые между VPN-сервером и сервером политики сети.

![Microsoft Message Analyzer с фильтрацией трафика](./media/howto-mfa-nps-extension-vpn/image50.png)

Дополнительные сведения см. в разделе [Интеграция существующей инфраструктуры NPS с Многофакторной идентификацией Azure](howto-mfa-nps-extension.md).

## <a name="next-steps"></a>Дальнейшие действия

[Как получить службу Azure Multi-Factor Authentication](concept-mfa-licensing.md)

[Шлюз удаленных рабочих столов и сервер Многофакторной идентификации Azure, использующие проверку подлинности RADIUS](howto-mfaserver-nps-rdg.md)

[Интеграция локальных каталогов с Azure Active Directory](../hybrid/whatis-hybrid-identity.md)
