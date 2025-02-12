---
title: Приступая к работе сервер многофакторной идентификации Azure — Azure Active Directory
description: Пошаговые инструкции по началу работы с сервером Многофакторной идентификации Azure в локальной среде
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15900758945fd5c97198caf47ff01fcfb5a6a794
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057404"
---
# <a name="getting-started-with-the-azure-multi-factor-authentication-server"></a>Приступая к работе с сервером Многофакторной идентификации Azure

<center>

![Приступая к работе с локального сервера MFA](./media/howto-mfaserver-deploy/server2.png)</center>

На этой странице рассматривается процедура установки сервера и его настройка в локальной службе Active Directory. Если сервер MFA уже установлен и вам нужно его обновить, см. сведения в статье [Обновление сервера Многофакторной идентификации Azure до последней версии](howto-mfaserver-deploy-upgrade.md). Сведения об установке только веб-службы см. в статье [Включение проверки подлинности мобильных приложений с помощью сервера Многофакторной идентификации Azure](howto-mfaserver-deploy-mobileapp.md).

> [!IMPORTANT]
> Начиная с 1 июля 2019 г. Корпорация Майкрософт больше не предоставляет многофакторной проверки Подлинности сервера для новых развертываний. Новых клиентов, которые хотите требовать многофакторную проверку подлинности от пользователей, их следует использовать многофакторную идентификацию Azure на основе облака. Существующие клиенты, которые активировали сервера MFA до 1 июля будет иметь возможность загрузить последнюю версию, будущие обновления и создать учетные данные активации обычным образом.

## <a name="plan-your-deployment"></a>Планирование развертывания

> [!WARNING]
> Начиная с марта 2019 многофакторной проверки Подлинности сервера загрузки будут доступны только для платных клиентов. Клиентов бесплатного или пробного больше нельзя скачать или создать и использовать учетные данные для активации.

Прежде чем скачивать сервер Многофакторной идентификации Azure, определите требования к нагрузке и высокому уровню доступности. Используйте эти сведения, чтобы выбрать способ и расположение развертывания.

Чтобы определить необходимый объем памяти, исходите из предполагаемого количества пользователей, которые будут регулярно выполнять проверку подлинности.

| Пользователи | ОЗУ |
| ----- | --- |
| 1–10 000 | 4 ГБ |
| 10 001–50 000 | 8 ГБ |
| 50 001–100 000 | 12 ГБ |
| 100 000–200 001 | 16 ГБ |
| 200 001+ | 32 ГБ |

Следует ли настроить несколько серверов для обеспечения высокого уровня доступности или балансировки нагрузки? Существует несколько способов настройки этой конфигурации с помощью сервера многофакторной идентификации Azure. Сервер многофакторной идентификации Azure, который устанавливается первым, становится главным. Все дополнительные серверы становятся подчиненными и автоматически синхронизируют данные о пользователях и конфигурации с главным сервером. Позже вы можете настроить один сервер-источник, а остальные использовать в качестве серверов резервного копирования или же настроить балансировку нагрузки между серверами.

Если главный сервер многофакторной идентификации Azure отключится от сети, подчиненные серверы смогут обрабатывать запросы двухфакторной проверки подлинности. Но вы не сможете добавить новых пользователей, как и существующие пользователи не смогут обновить свои параметры, пока главный сервер снова не подключится к сети или уровень подчиненного сервера не будет повышен.

### <a name="prepare-your-environment"></a>Подготовка среды

Убедитесь, что сервер, используемый для Многофакторной идентификации Azure, соответствует следующим требованиям:

| Требования к серверу Многофакторной идентификации Azure | Описание |
|:--- |:--- |
| Оборудование |<li>200 МБ места на жестком диске</li><li>32- или 64-разрядный процессор</li><li>Не менее 1 ГБ ОЗУ</li> |
| Программное обеспечение |<li>Windows Server 2016</li><li>Windows Server 2012 R2</li><li>Windows Server 2012</li><li>Windows Server 2008 R2</li><li>Windows Server 2008 с пакетом обновления 1 (SP1) и 2 (SP2)</li><li>Windows Server 2003 R2</li><li>Windows Server 2003 с пакетом обновления 1 (SP1) и 2 (SP2)</li><li>Windows 10</li><li>Windows 8.1, все выпуски</li><li>Windows 8, все выпуски</li><li>Windows 7, все выпуски</li><li>Windows Vista, все выпуски, а также выпуски с пакетом обновления 1 (SP1) и 2 (SP2)</li><li>Microsoft .NET Framework 4.0</li><li>IIS 7.0 или более поздняя версия, если вы устанавливаете SDK для пользовательского портала или веб-службы</li> |
| Разрешения | Учетная запись администратора домена или администратора предприятия для регистрации в Active Directory |

### <a name="azure-mfa-server-components"></a>Компоненты сервера Azure MFA

Сервер Azure MFA состоит из трех веб-компонентов:

* Пакет SDK веб-службы — обеспечивает взаимодействие с другими компонентами и устанавливается на сервере приложений Azure MFA.
* Пользовательский портал — это веб-сайт IIS, с помощью которого пользователи могут зарегистрироваться в службе Многофакторной идентификации Azure (MFA) и управлять своими учетными записями.
* Веб-служба мобильного приложения — позволяет использовать мобильное приложение, например Microsoft Authenticator, для двухфакторной проверки подлинности.

Все три компонента можно установить на одном сервере, если он поддерживает выход в Интернет. Если вы планируете установить компоненты на разных серверах, установите пакет SDK веб-службы на сервере приложений Azure MFA, а пользовательский портал и веб-службу мобильного приложения — на сервере с выходом в Интернет.

### <a name="azure-multi-factor-authentication-server-firewall-requirements"></a>Требования к брандмауэру сервера Многофакторной идентификации Azure

На каждом сервере MFA порт 443 должен быть открыт для передачи данных на следующие адреса:

* https://pfd.phonefactor.net
* https://pfd2.phonefactor.net
* https://css.phonefactor.net

Если в брандмауэре на порту 443 есть ограничения для исходящего трафика, откройте следующие диапазоны IP-адресов:

| IP-подсеть | Маска сети | Диапазон IP-адресов |
|:---: |:---: |:---: |
| 134.170.116.0/25 |255.255.255.128 |134.170.116.1–134.170.116.126 |
| 134.170.165.0/25 |255.255.255.128 |134.170.165.1–134.170.165.126 |
| 70.37.154.128/25 |255.255.255.128 |70.37.154.129–70.37.154.254 |

Если вы не используете функции подтверждения событий и пользователи не проходят проверку подлинности при помощи мобильных приложений с устройств в корпоративной сети, вам необходимо использовать следующие диапазоны:

| IP-подсеть | Маска сети | Диапазон IP-адресов |
|:---: |:---: |:---: |
| 134.170.116.72/29 |255.255.255.248 |134.170.116.72–134.170.116.79 |
| 134.170.165.72/29 |255.255.255.248 |134.170.165.72–134.170.165.79 |
| 70.37.154.200/29 |255.255.255.248 |70.37.154.201–70.37.154.206 |

## <a name="download-the-mfa-server"></a>Скачивание сервера Многофакторной идентификации

> [!WARNING]
> Начиная с марта 2019 многофакторной проверки Подлинности сервера загрузки будут доступны только для платных клиентов. Клиентов бесплатного или пробного больше нельзя скачать или создать и использовать учетные данные для активации.

Чтобы скачать сервер Многофакторной идентификации Azure с портала Azure, сделайте следующее:

1. Войдите на [портал Azure](https://portal.azure.com) с использованием учетной записи администратора.
2. Выберите **Azure Active Directory** > **MFA Server** (Сервер MFA).
3. Выберите **Параметры сервера**.
4. Выберите **Загрузить** и следуйте инструкциям на странице скачивания, чтобы сохранить установщик. 

   ![Скачивание сервера MFA на портале Azure](./media/howto-mfaserver-deploy/downloadportal.png)

5. Не закрывайте эту страницу — мы вернемся к ней после запуска установщика.

## <a name="install-and-configure-the-mfa-server"></a>Установка и настройка сервера Многофакторной идентификации

Загрузив сервер, установите и настройте его. Убедитесь, что компьютер, на который вы устанавливаете сервер, соответствует требованиям, перечисленным в разделе планирования.

1. Дважды щелкните исполняемый файл.
2. На экране "Выбор папки для установки" убедитесь, что выбрана правильная папка, и нажмите кнопку **Далее**.
3. После завершения установки нажмите кнопку **Готово**. Запустится мастер настройки.
4. На экране приветствия мастера настройки установите флажок **Пропустить использование мастера настройки проверки подлинности** и нажмите кнопку **Далее**. Мастер закроется, и сервер запустится.

   ![Пропустить использование мастера настройки проверки подлинности](./media/howto-mfaserver-deploy/skip2.png)

5. На странице, с которой вы скачали сервер, нажмите кнопку **Создать учетные данные для активации**. Скопируйте эти данные в соответствующие поля на сервере Azure MFA и нажмите кнопку **Activate**(Активировать).

## <a name="send-users-an-email"></a>Отправка электронного письма пользователям

Чтобы упростить развертывание, разрешите серверу MFA взаимодействовать с пользователями. С сервера MFA отправьте пользователям сообщение о том, что они зарегистрированы для прохождения двухфакторной проверки подлинности.

Содержимое отправленного сообщения зависит от настройки двухфакторной проверки подлинности для пользователей. Например, если вы импортировали номера телефонов из каталога организации, сообщение должно содержать номера телефонов по умолчанию, чтобы пользователи знали, чего ожидать. Если номера телефонов не были импортированы или пользователи планируют использовать мобильное приложение, отправьте им сообщение, в котором предлагается завершить регистрацию учетной записи. Также прикрепите к письму гиперссылку на пользовательский портал Многофакторной идентификации Azure.

Содержимое электронного письма также будет различным в зависимости от метода проверки подлинности, установленного для пользователя (телефонный звонок, SMS или мобильное приложение). Например, если пользователь должен использовать ПИН-код при проверке подлинности, в электронном письме будет указано начальное значение ПИН-кода. ПИН-код требуется изменить при первой проверке подлинности.

### <a name="configure-email-and-email-templates"></a>Настройка электронной почты и шаблонов электронной почты

Щелкните значок электронной почты в левой части окна, чтобы настроить параметры отправки этих сообщений. Здесь можно указать параметры SMTP почтового сервера и отправить письмо, установив флажок **Отправлять электронную почту пользователям**.

![Настройки электронной почты для сервера MFA](./media/howto-mfaserver-deploy/email1.png)

На вкладке Email Content (Содержимое электронного письма) отображаются различные шаблоны электронной почты. В зависимости от настроек многофакторной проверки подлинности можно выбрать тот шаблон, который максимально вам подходит.

![Шаблоны писем для сервера многофакторной проверки Подлинности в консоли](./media/howto-mfaserver-deploy/email2.png)

## <a name="import-users-from-active-directory"></a>Импорт пользователей из Active Directory

Теперь, когда сервер установлен, необходимо добавить пользователей. Вы можете создать их вручную, импортировать из Active Directory или настроить автоматическую синхронизацию с Active Directory.

### <a name="manual-import-from-active-directory"></a>Импорт из Active Directory вручную

1. На сервере Azure MFA в левой части окна выберите элемент **Пользователи**.
2. Внизу выберите команду **Импортировать из Active Directory**.
3. Теперь вы можете искать в каталоге AD отдельных пользователей. Вы также можете искать подразделения и входящих в них пользователей. В этом случае будет указано подразделение, к которому принадлежит пользователь.
4. Выделите всех пользователей справа и нажмите кнопку **Импортировать**. Должно появиться всплывающее окно с сообщением о том, что импорт выполнен успешно. Закройте окно импорта.

   ![Импорт пользователей многофакторной проверки Подлинности сервера из Active Directory](./media/howto-mfaserver-deploy/import2.png)

### <a name="automated-synchronization-with-active-directory"></a>Автоматическая синхронизация с Active Directory

1. На сервере Azure MFA в левой части окна выберите **Интеграция каталогов**.
2. Перейдите на вкладку **Синхронизация**.
3. В нижней части окна нажмите кнопку **Добавить**.
4. В открывшемся окне **Добавление объекта синхронизации** задайте домен, подразделение **или** группу безопасности, параметры, настройки метода по умолчанию и языковые настройки по умолчанию для этой задачи синхронизации, а затем нажмите кнопку **Добавить**.
5. Установите флажок **Включить синхронизацию с Active Directory** и укажите **интервал синхронизации** от одной минуты до 24 часов.

## <a name="how-the-azure-multi-factor-authentication-server-handles-user-data"></a>Как сервер Многофакторной идентификации Azure обрабатывает данные пользователя?

При использовании локального сервера Многофакторной идентификации (MFA) данные пользователя хранятся на локальных серверах. В облаке данные пользователя постоянно не хранятся. Когда пользователь выполняет двухфакторную проверку подлинности, сервер MFA отправляет данные в облачную службу Многофакторной идентификации Azure для проверки подлинности. При отправке запросов проверки подлинности в облачную службу в запросы и журналы включаются указанные ниже поля, что позволяет использовать их в отчетах о проверке подлинности клиентов или об использовании. Некоторые поля являются необязательными и могут включаться и выключаться на сервере Многофакторной идентификации. Взаимодействие сервера MFA сервера с облачной службой многофакторной проверки подлинности осуществляется по протоколу SSL/TLS через порт 443 для исходящих подключений. В число этих полей входят следующие.

* Уникальный идентификатор: имя пользователя или внутренний идентификатор сервера MFA
* Имя и фамилия (необязательно).
* Адрес электронной почты (необязательно).
* Номер телефона (при выполнении голосовых вызовов или проверке подлинности по SMS).
* Маркер устройства: при выполнении проверки подлинности мобильного приложения
* Режим проверки подлинности
* Результат проверки подлинности.
* Имя сервера MFA.
* IP-адрес сервера MFA
* IP-адрес клиента: если он доступен

Кроме указанных выше полей, результат проверки подлинности (пройдена или получен отказ) и причины отказов также хранятся с данными проверки подлинности и могут быть получены из отчетов о проверке подлинности или использовании.

> [!IMPORTANT]
> Начиная с марта 2019 г параметры телефонного звонка будет недоступен для пользователей сервера многофакторной проверки Подлинности в клиентах бесплатного или пробного Azure AD. Это изменение не затрагивает SMS-сообщения. Телефонный звонок будет продолжать будет доступен пользователям в платных клиентов Azure AD. Это изменение влияет только на клиентов бесплатного или пробного Azure AD.

## <a name="back-up-and-restore-azure-mfa-server"></a>Резервное копирование и восстановление сервера Azure MFA

Обеспечение надежного резервного копирования является важным шагом при работе с любой системой.

Чтобы создать резервную копию сервера Azure MFA, создайте копию папки **C:\Program Files\Multi-Factor Authentication Server\Data** вместе с файлом **PhoneFactor.pfdata**. 

Если потребуется восстановить сервер, сделайте следующее:

1. Переустановите сервер Azure MFA на новом компьютере.
2. Активируйте новый сервер Azure MFA.
3. Остановите службу **MultiFactorAuth**.
4. Перезапишите файл **PhoneFactor.pfdata** его резервной копией.
5. Запустите службу **MultiFactorAuth**.

Теперь новый сервер запущен и работает с исходной конфигурацией и данными пользователя из резервной копии.

## <a name="managing-the-tlsssl-protocols-and-cipher-suites"></a>Управление протоколами TLS и SSL, а также комплектами шифров

После установки сервера MFA 8.x или более поздней версии (либо обновления до этой версии) рекомендуется отключить или удалить прежние, менее надежные комплекты шифров (если они не требуются в организации). Соответствующие инструкции см. в статье, посвященной [управлению протоколами SSL и TLS, а также комплектами шифров для AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/manage-ssl-protocols-in-ad-fs).

## <a name="next-steps"></a>Дальнейшие действия

- Установка и настройка [пользовательского портала](howto-mfaserver-deploy-userportal.md) для самообслуживания пользователей.
- Установите и настройте сервер Azure MFA со [службой федерации Active Directory](multi-factor-authentication-get-started-adfs.md), [проверкой подлинности RADIUS](howto-mfaserver-dir-radius.md) или [проверкой подлинности LDAP](howto-mfaserver-dir-ldap.md).
- Установка и настройка [шлюза удаленных рабочих столов и сервера Многофакторной идентификации Azure, использующих проверку подлинности RADIUS](howto-mfaserver-nps-rdg.md).
- [Развертывание веб-службы мобильных приложений сервера Многофакторной идентификации Azure](howto-mfaserver-deploy-mobileapp.md).
- [Расширенные сценарии с использованием Многофакторной идентификации Azure и VPN сторонних поставщиков](howto-mfaserver-nps-vpn.md).
