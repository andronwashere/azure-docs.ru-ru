---
title: Защита отдельной базы данных или базы данных в пуле в Базе данных SQL Azure | Документация Майкрософт
description: В этом руководстве рассказывается о методах и функциях, используемых для защиты отдельной базы данных или базы данных в составе пула в Базе данных SQL Azure.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.topic: tutorial
author: VanMSFT
ms.author: vanto
ms.reviewer: carlrab
ms.date: 02/08/2019
ms.custom: seoapril2019
ms.openlocfilehash: ce9ba7c197bb604b9d71e2bf501ca67d32865f38
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566872"
---
# <a name="tutorial-secure-a-single-or-pooled-database"></a>Руководство по Защита отдельной базы данных или базы данных в пуле

Из этого руководства вы узнаете, как выполнить следующие задачи:

> [!div class="checklist"]
> - Создавать правила брандмауэра уровня сервера и уровня базы данных.
> - Назначать администратора Azure Active Directory (AD).
> - Управлять пользовательским доступом с помощью аутентификации SQL, аутентификации Azure AD и безопасных строк подключения.
> - Включать функции безопасности, такие как расширенная защита данных, аудит, маскирование данных и шифрование.

База данных SQL Azure защищает данные в отдельной базе данных или базе данных в пуле, предоставляя возможность:

- ограничить доступ с помощью правил брандмауэра;
- использовать механизмы аутентификации, требующие учетных данных;
- использовать авторизацию на основе разрешений и членства с учетом ролей.
- Включение функций безопасности

> [!NOTE]
> База данных SQL Azure в управляемом экземпляре защищена с помощью правил безопасности сети и частных конечных точек, как описано в статьях [Управляемый экземпляр Базы данных SQL Azure](sql-database-managed-instance-index.yml) и [Архитектура подключения к Управляемому экземпляру Базы данных SQL Azure](sql-database-managed-instance-connectivity-architecture.md).

Дополнительные сведения см. в статьях [Расширенные функции безопасности Базы данных SQL Azure](/azure/sql-database/sql-database-security-index) и [Обзор возможностей безопасности Базы данных SQL Azure](sql-database-security-overview.md).

## <a name="prerequisites"></a>Предварительные требования

Для работы с руководством требуется наличие следующих компонентов:

- [SQL Server Management Studio](/sql/ssms/download-sql-server-management-studio-ssms)
- Сервер SQL Azure и база данных.
  - Создайте их с помощью [портала Azure](sql-database-single-database-get-started.md), [CLI](sql-database-cli-samples.md) или [PowerShell](sql-database-powershell-samples.md).

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Чтобы выполнить все действия из этого руководства, войдите на [портал Azure](https://portal.azure.com/).

## <a name="create-firewall-rules"></a>Создание правил брандмауэра

Базы данных SQL защищены брандмауэрами в Azure. По умолчанию все подключения к серверу и базе данных отклоняются, за исключением подключений служб Azure. Дополнительные сведения см. в статье [Правила брандмауэра уровня базы данных и уровня сервера Базы данных SQL Azure](sql-database-firewall-configure.md).

Чтобы обеспечить наибольшую безопасность, для параметра **Разрешить доступ к службам Azure** следует установить значение **Выключено**. Затем создайте [зарезервированный IP-адрес (классическое развертывание)](../virtual-network/virtual-networks-reserved-public-ip.md) для ресурса, от которого нужно принять подключение (виртуальная машина Azure или облачная служба), и разрешите доступ через брандмауэр только этому IP-адресу. Если вы используете модель развертывания [Resource Manager](/azure/virtual-network/virtual-network-ip-addresses-overview-arm), для каждого ресурса необходим выделенный общедоступный IP-адрес.

> [!NOTE]
> База данных SQL обменивается данными через порт 1433. Если вы пытаетесь подключиться из корпоративной сети, исходящий трафик через порт 1433 может быть запрещен сетевым брандмауэром. В таком случае вы не сможете подключиться к серверу Базы данных SQL Azure, пока ваш администратор не откроет порт 1433.

### <a name="set-up-sql-database-server-firewall-rules"></a>Настройка правил брандмауэра для сервера Базы данных SQL

Правила брандмауэра для IP-адресов уровня сервера применяются ко всем базам данных на одном сервере Базы данных SQL.

Чтобы установить правило брандмауэра на уровне сервера, сделайте следующее:

1. На портале Azure в меню слева выберите пункт **Базы данных SQL** и на странице **Базы данных SQL** выберите имя своей базы данных.

    ![правило брандмауэра для сервера](./media/sql-database-security-tutorial/server-name.png)

    > [!NOTE]
    > Не забудьте скопировать полное имя сервера (например, *ваш_сервер.database.windows.net*) для дальнейшего использования в этом руководстве.

1. На странице **Обзор** выберите **Настройка брандмауэра для сервера**. Для сервера базы данных откроется страница **Параметры брандмауэра**.

   1. На панели инструментов выберите **Добавить IP-адрес клиента**, чтобы добавить текущий IP-адрес в новое правило брандмауэра. С использованием правила можно открыть порт 1433 для одного IP-адреса или диапазона IP-адресов. Щелкните **Сохранить**.

      ![Настройка правила брандмауэра для сервера](./media/sql-database-security-tutorial/server-firewall-rule2.png)

   1. Щелкните **ОК**, а затем закройте страницу **Параметры брандмауэра**.

Теперь можно подключиться к любой базе данных на сервере с указанным IP-адресом или диапазоном IP-адресов.

> [!IMPORTANT]
> По умолчанию доступ через брандмауэр Базы данных SQL в разделе **Разрешить доступ к службам Azure** включен для всех служб Azure. Щелкните **Выключено**, чтобы отключить доступ для всех служб Azure.

### <a name="setup-database-firewall-rules"></a>Настройка правил брандмауэра для базы данных

Правила брандмауэра на уровне базы данных применяются только к отдельным базам данных. Базы данных сохранит эти правила при отработке отказа сервера. Правила брандмауэра уровня базы данных можно настроить только с помощью инструкций Transact-SQL (T-SQL) и только после настройки правила брандмауэра уровня сервера.

Чтобы настроить правила брандмауэра уровня базы данных:

1. Подключитесь к базе данных, например с помощью [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

1. В **обозревателе объектов** щелкните правой кнопкой мыши базу данных и выберите команду **Создать запрос**.

1. Добавьте этот оператор в окне запроса и измените IP-адрес на свой общедоступный IP-адрес:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

1. На панели инструментов выберите **Выполнить**, чтобы создать правило брандмауэра.

> [!NOTE]
> Вы также можете создать правило брандмауэра уровня сервера в среде SSMS с помощью команды [sp_set_firewall_rule](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database?view=azuresqldb-current), однако сначала нужно подключиться к базе данных *master*.

## <a name="create-an-azure-ad-admin"></a>Создание администратора Azure AD

Убедитесь, что вы используете соответствующий управляемый домен Azure Active Directory (AD). Щелкните в правом верхнем углу окна портала Azure, чтобы выбрать домен AD. С помощью этого процесса подтверждается, что одна подписка используется для Azure AD и SQL Server, на котором размещена База данных SQL Azure или хранилище данных.

   ![choose-ad](./media/sql-database-security-tutorial/8choose-ad.png)

Чтобы настроить администратора Azure AD:

1. На портале Azure на странице **SQL Server** выберите **Администратор Active Directory**. Затем щелкните **Задать администратора**.

    ![выбор Active Directory](./media/sql-database-security-tutorial/admin-settings.png)  

    > [!IMPORTANT]
    > Вы должны быть администратором организации или глобальным администратором, чтобы выполнить эту задачу.

1. На странице **Добавление администратора** найдите и выберите группу или пользователя AD, а затем щелкните **Выбрать**. На странице перечислены все участники и группы Active Directory, а те, которые выделены серым цветом, не поддерживаются как администраторы Azure AD. Обратитесь к разделу [Функции и ограничения Azure AD](sql-database-aad-authentication.md#azure-ad-features-and-limitations).

    ![Выбор администратора](./media/sql-database-security-tutorial/admin-select.png)

    > [!IMPORTANT]
    > Управление доступом на основе ролей (RBAC) применяется только к порталу и не распространяется на SQL Server.

1. В верхней части страницы **Администратор Active Directory** нажмите кнопку **Сохранить**.

    Изменение администратора может занять несколько минут. После этого новый администратор появится в поле **Администратор Active Directory**.

> [!NOTE]
> При настройке администратора Azure AD новое имя администратора (пользователя или группы) не может существовать в базе данных *master* как пользователь аутентификации SQL Server. Если оно присутствует, настройка завершится ошибкой. Будет выполнен откат изменений и появится сообщение, что такое имя администратора уже существует. Такой пользователь аутентификации SQL Server не входит в Azure AD, поэтому любая попытка подключиться с помощью аутентификации Azure AD завершится сбоем.

Сведения о настройке Azure AD см. в статьях:

- [Что собой представляет гибридная идентификация](../active-directory/hybrid/whatis-hybrid-identity.md)
- [Добавление имени личного домена с помощью портала Azure Active Directory](../active-directory/active-directory-domains-add-azure-portal.md)
- [Windows Azure now supports federation with Windows Server Active Directory](https://azure.microsoft.com/blog/20../../windows-azure-now-supports-federation-with-windows-server-active-directory/) (Теперь Microsoft Azure поддерживает федерацию с Windows Server AD)
- [Администрирование каталога Azure AD](../active-directory/fundamentals/active-directory-administer.md)
- [Общие сведения об Azure PowerShell](/powershell/azure/overview?view=azureadps-2.0)
- [Порты и протоколы, необходимые для гибридной идентификации](../active-directory/hybrid/reference-connect-ports.md)

## <a name="manage-database-access"></a>Управление доступом к базе данных

Управляйте доступом к базе данных, добавляя пользователей в базу данных или предоставляя доступ с помощью безопасных строк подключения. Строки подключения можно использовать для внешних приложений. Дополнительные сведения см. в статьях [Управление доступом к службам "База данных SQL Azure" и "Хранилище данных SQL"](sql-database-control-access.md) и [Использование аутентификации Azure Active Directory для аутентификации с помощью SQL](sql-database-aad-authentication.md).

Чтобы добавить пользователей, выберите тип аутентификации базы данных:

- **Аутентификация SQL**: для входа используются имя пользователя и пароль, действительные только в контексте конкретной базы данных на сервере.

- **Аутентификация Azure AD**: используются удостоверения, управляемые Azure AD.

### <a name="sql-authentication"></a>Аутентификация SQL

Чтобы добавить пользователя с аутентификацией SQL:

1. Подключитесь к базе данных, например с помощью [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

1. В **обозревателе объектов** щелкните правой кнопкой мыши базу данных и выберите **Создать запрос**.

1. В окне запроса введите следующую команду:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

1. На панели инструментов выберите **Выполнить**, чтобы создать пользователя.

1. По умолчанию пользователь может подключаться к базе данных, но не имеет разрешений на чтение и запись данных. Чтобы предоставить эти разрешения, выполните следующие две команды в новом окне запроса.

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

> [!NOTE]
> Создайте учетные записи без прав администратора на уровне базы данных, если только не требуется выполнять задачи администрирования, например создавать пользователей.

### <a name="azure-ad-authentication"></a>Аутентификация Azure AD

Для аутентификации Azure Active Directory необходимо, чтобы пользователи базы данных создавались как автономные. Пользователь автономной базы данных сопоставляется с удостоверением в каталоге Azure AD, связанном с базой данных, и у него нет имени для входа в базе данных *master*. Удостоверение Azure AD может быть для отдельного пользователя или группы. Дополнительные сведения об аутентификации с помощью Azure AD см. в статьях [Пользователи автономной базы данных — создание переносимой базы данных](https://msdn.microsoft.com/library/ff929188.aspx) и [Настройка и администрирование аутентификации Azure Active Directory с помощью SQL](./sql-database-aad-authentication-configure.md).

> [!NOTE]
> Пользователей баз данных (за исключением администраторов) невозможно создавать с помощью портала Azure. Роли RBAC Azure не распространяются на серверы SQL, базы данных или хранилища данных. Они используются для управления ресурсами Azure и не относятся к разрешениям базы данных.
>
> Например, роль *Участник SQL Server* не предоставляет разрешение на подключение к базе данных или хранилищу данных. Это разрешение должно быть предоставлено в рамках базы данных с помощью инструкций T-SQL.

> [!IMPORTANT]
> Специальные символы, например двоеточие `:` или амперсанд `&`, не поддерживаются в именах пользователей в инструкциях T-SQL `CREATE LOGIN` и `CREATE USER`.

Чтобы добавить пользователя с аутентификацией Azure AD:

1. Подключитесь к своему серверу Azure SQL Server с помощью учетной записи Azure AD, используя по крайней мере разрешение *ALTER ANY USER*.

1. В **обозревателе объектов** щелкните правой кнопкой мыши базу данных и выберите команду **Создать запрос**.

1. В окне запроса введите следующую команду и измените `<Azure_AD_principal_name>` на имя участника-пользователя Azure AD или отображаемое имя для группы Azure AD:

   ```sql
   CREATE USER <Azure_AD_principal_name> FROM EXTERNAL PROVIDER;
   ```

> [!NOTE]
> Для пользователей Azure AD в метаданных базы данных указывается тип `E (EXTERNAL_USER)`, а для групп — тип `X (EXTERNAL_GROUPS)`. Дополнительные сведения см. в статье [sys.database_principals (Transact-SQL)](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql).

### <a name="secure-connection-strings"></a>Безопасные строки подключения

Чтобы обеспечить защищенное зашифрованное подключение между клиентским приложением и Базой данных SQL, строка подключения должна быть настроена таким образом, чтобы:

- запрашивалось зашифрованное подключение;
- сертификат сервера не считался доверенным.

Устанавливается подключение по протоколу TLS, и уменьшается риск атак "злоумышленник в середине". Строки подключения доступны отдельно для каждой базы данных и предварительно настроены на поддержку клиентских драйверов, например ADO.NET, JDBC, ODBC и PHP. Чтобы получить сведения о протоколе TLS и проверить возможность подключения, просмотрите [рекомендации по использованию протокола TLS](sql-database-connect-query.md#tls-considerations-for-sql-database-connectivity).

Чтобы скопировать безопасную строку подключения, сделайте следующее:

1. На портале Azure в меню слева выберите пункт **Базы данных SQL** и на странице **Базы данных SQL** выберите имя своей базы данных.

1. На странице **Обзор** щелкните **Показать строки подключения к базам данных**.

1. Выберите вкладку "Драйвер" и скопируйте полную строку подключения.

    ![Строка подключения по протоколу ADO.NET](./media/sql-database-security-tutorial/connection.png)

## <a name="enable-security-features"></a>Включение функций безопасности

База данных SQL Azure предоставляет возможности безопасности, доступ к которым можно получить на портале Azure. Эти возможности представлены и для сервера, и для базы данных, за исключением маскирования данных, доступного только в базе данных. См. дополнительные сведения о [расширенной защите данных](sql-database-advanced-data-security.md), [аудите](sql-database-auditing.md), [динамическом маскировании данных](sql-database-dynamic-data-masking-get-started.md) и [прозрачном шифровании данных](transparent-data-encryption-azure-sql.md).

### <a name="advanced-data-security"></a>Расширенная защита данных

Функция расширенной защиты данных обнаруживает потенциальные угрозы по мере их возникновения и отправляет оповещения системы безопасности о подозрительных действиях. Пользователи могут анализировать подозрительные события с помощью функции аудита и определять их причину (попытка получить доступ к данным в базе данных, нарушить безопасность или использовать их уязвимость). Пользователям также предоставляются общие сведения о безопасности, включающие оценку уязвимостей, а также средство поиска и классификации данных.

> [!NOTE]
> Примером угрозы является атака путем внедрения кода SQL — это процесс, при котором злоумышленники внедряют вредоносный код SQL во входные данные приложения. Затем приложение может невольно выполнить вредоносный код SQL и предоставить злоумышленникам доступ на проникновение или изменение данных в базе данных.

Чтобы включить расширенную защиту данных, сделайте следующее:

1. На портале Azure в меню слева выберите пункт **Базы данных SQL** и на странице **Базы данных SQL** выберите имя своей базы данных.

1. На странице **Обзор** выберите ссылку **Имя сервера**. Откроется страница сервера базы данных.

1. На странице **SQL Server** найдите раздел **Безопасность** и выберите **Расширенная защита данных**.

   1. Щелкните **Включить** в разделе **Расширенная защита данных**, чтобы включить эту функцию. Выберите учетную запись хранения для записи результатов оценки уязвимостей. Затем нажмите кнопку **Save** (Сохранить).

      ![Область навигации](./media/sql-database-security-tutorial/threat-settings.png)

      Вы также можете настроить сообщения электронной почты для получения оповещений системы безопасности, подробностей о хранилище и типах обнаружения угроз.

1. Вернитесь на страницу **Базы данных SQL** вашей базы данных и выберите **Расширенная защита данных** в разделе **Безопасность**. Здесь вы найдете различные индикаторы безопасности, доступные для базы данных.

    ![Состояние угрозы](./media/sql-database-security-tutorial/threat-status.png)

В случае обнаружения подозрительной активности вам будет отправлено сообщение электронной почты со сведениями о событии. Это такая информация, как характер активности, сведения о базе данных, сервере, времени события, возможных причинах, а также рекомендуемые действия по поиску и устранению потенциальной угрозы. Если вы получили такое сообщение, выберите ссылку **Журнал аудита SQL Azure**, чтобы открыть портал Azure и отобразить соответствующие записи аудита, появившиеся во время возникновения подозрительного события.

   ![Электронное сообщение об обнаружении угроз](./media/sql-database-security-tutorial/threat-email.png)

### <a name="auditing"></a>Аудит

Функция аудита отслеживает события базы данных и записывает их в журнал аудита в службе хранилища Azure, журналы Azure Monitor или концентратор событий. Аудит помогает вам соблюсти требования нормативов, проанализировать работу с базой данных и получить представление о расхождениях и аномалиях, которые могут указывать на предполагаемые нарушения безопасности.

Чтобы включить аудит:

1. На портале Azure в меню слева выберите пункт **Базы данных SQL** и на странице **Базы данных SQL** выберите имя своей базы данных.

1. В разделе **Безопасность** щелкните **Аудит**.

1. В разделе параметров **Аудит** установите следующие значения:

   1. Выберите для параметра **Аудит** значение **Включено**.

   1. Выберите в качестве **места назначения журнала аудита** любой из следующих ресурсов:

       - **Хранилище** — это учетная запись хранения Azure, в которой будут сохраняться журналы событий, доступные для загрузки как файлы *XEL*.

          > [!TIP]
          > Для того чтобы в полной мере воспользоваться возможностями шаблонов отчетов об аудите, используйте одну и ту же учетную запись хранения для всех баз данных.

       - **Служба Log Analytics**, которая автоматически сохраняет события для запроса или для дальнейшего анализа.

           > [!NOTE]
           > **Рабочая область Log Analytics** является обязательной для поддержки расширенных функций, таких как аналитика, настраиваемые правила генерации оповещений, а также экспорт из Excel или Power BI. Без рабочей области доступен только редактор запросов.

       - **Концентратор событий**, который позволяет событиям перенаправляться для использования в других приложениях.

   1. Щелкните **Сохранить**.

      ![Параметры аудита](./media/sql-database-security-tutorial/audit-settings.png)

1. Теперь вы можете щелкнуть **Ознакомиться с журналами аудита**, чтобы просмотреть данные о событиях в базе данных.

    ![Записи аудита](./media/sql-database-security-tutorial/audit-records.png)

> [!IMPORTANT]
> Дополнительные сведения о том, как настроить события аудита с помощью PowerShell или REST API, см. в статье [Приступая к работе с аудитом базы данных SQL](sql-database-auditing.md).

### <a name="dynamic-data-masking"></a>Динамическое маскирование данных

Функция маскирования данных автоматически скрывает конфиденциальные данные в базе данных.

Чтобы включить маскирование данных:

1. На портале Azure в меню слева выберите пункт **Базы данных SQL** и на странице **Базы данных SQL** выберите имя своей базы данных.

1. В разделе **Безопасность** выберите **Динамическое маскирование данных**.

1. В параметрах **Динамического маскирования данных** щелкните **Добавить маску**, чтобы добавить правило маскирования. Azure будет автоматически заполнять доступные для выбора схемы базы данных, таблицы и столбцы.

    ![Параметры маскирования](./media/sql-database-security-tutorial/mask-settings.png)

1. Щелкните **Сохранить**. Теперь выбранные данные скрыты для обеспечения конфиденциальности.

    ![Пример маскирования](./media/sql-database-security-tutorial/mask-query.png)

### <a name="transparent-data-encryption"></a>Прозрачное шифрование данных

Функция шифрования позволяет автоматически шифровать неактивные данные, не внося каких-либо изменений в приложения, обращающиеся к зашифрованной базе данных. Для новых баз данных шифрование включено по умолчанию. Кроме того, можно зашифровать данные с помощью SSMS и функции [Постоянное шифрование](sql-database-always-encrypted.md).

Чтобы включить и проверить шифрование:

1. На портале Azure в меню слева выберите пункт **Базы данных SQL** и на странице **Базы данных SQL** выберите имя своей базы данных.

1. В разделе **Безопасность** выберите **Прозрачное шифрование данных**.

1. Если требуется, для параметра **Шифрование данных** задайте значение **Включено**. Щелкните **Сохранить**.

    ![Прозрачное шифрование данных](./media/sql-database-security-tutorial/encryption-settings.png)

> [!NOTE]
> Чтобы просмотреть состояние шифрования, подключитесь к базе данных, используя [SSMS](./sql-database-connect-query-ssms.md), и запросите столбец `encryption_state` представления [sys.dm_database_encryption_keys](/sql/relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql). Состояние `3` указывает, что база данных зашифрована.

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как повысить безопасность базы данных с помощью нескольких простых шагов. Вы научились выполнять следующие задачи:

> [!div class="checklist"]
> - Создавать правила брандмауэра уровня сервера и уровня базы данных.
> - Назначать администратора Azure Active Directory (AD).
> - Управлять пользовательским доступом с помощью аутентификации SQL, аутентификации Azure AD и безопасных строк подключения.
> - Включать функции безопасности, такие как расширенная защита данных, аудит, маскирование данных и шифрование.

Перейдите к следующему руководству, чтобы узнать, как реализовать географическое распределение.

> [!div class="nextstepaction"]
>[Реализация географически распределенной базы данных](sql-database-implement-geo-distributed-database.md)
