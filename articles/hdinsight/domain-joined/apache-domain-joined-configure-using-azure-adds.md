---
title: Настройка Корпоративного пакета безопасности с помощью доменных служб Azure Active Directory — Azure HDInsight
description: Узнайте, как установить и настроить кластер HDInsight с Корпоративным пакетом безопасности с помощью доменных служб Azure Active Directory.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.custom: seodec18
ms.date: 04/23/2019
ms.openlocfilehash: 1ad3c446df2f2ce62024dfdda589669653f65ef4
ms.sourcegitcommit: bafb70af41ad1326adf3b7f8db50493e20a64926
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2019
ms.locfileid: "68488710"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Настройка кластера HDInsight с корпоративным пакетом безопасности с помощью доменных служб Azure Active Directory

Кластеры Azure HDInsight с корпоративным пакетом безопасности предоставляют многопользовательский режим доступа. Кластеры HDInsight с корпоративным пакетом безопасности подключаются к доменам. Таким образом, пользователи домена могут использовать свои учетные данные домена для проверки подлинности в кластерах и выполнения заданий по обработке больших объемов данных.

Из этой статьи вы узнаете, как настроить присоединенный к домену кластер HDInsight с корпоративным пакетом безопасности с помощью доменных служб Azure Active Directory (Azure AD-DS).

> [!NOTE]  
> Протокол ESP общедоступен в HDInsight 3,6 и 4,0 для типов кластера: Apache Spark, Interactive, Apache Hadoop и HBase. Для типа кластера Apache Kafka используется предварительная версия ESP.

## <a name="enable-azure-ad-ds"></a>Включение Azure AD-DS

> [!NOTE]  
> Только администраторы клиента имеют разрешение на включение Azure AD-DS. Если хранилище кластера имеет Azure Data Lake Storage (ADLS) Gen1 или Gen2, необходимо отключить многофакторную проверку подлинности (MFA) только для тех пользователей, которым требуется доступ к кластеру с использованием обычной проверки подлинности Kerberos. Вы можете использовать [Надежные IP-адреса](../../active-directory/authentication/howto-mfa-mfasettings.md#trusted-ips) или [Условный доступ](../../active-directory/conditional-access/overview.md) , чтобы отключить MFA для конкретных пользователей, только если они обращаются к диапазону IP виртуальной сети кластера HDInsight. При использовании условного доступа убедитесь, что конечная точка службы AD включена в виртуальной сети HDInsight.
>
> Если системой хранения данных кластера является хранилище BLOB-объектов Azure (WASB), оставьте Многофакторную идентификацию включенной.

Включение доменных служб Azure AD является необходимым предварительным условием, которое нужно выполнить, прежде чем вы сможете создать кластер HDInsight с ESP. Дополнительные сведения см. в разделе [Включение доменных служб Azure Active Directory с помощью портала Azure](../../active-directory-domain-services/create-instance.md). 

Если доменные службы Azure AD включены, все пользователи и объекты начинают синхронизацию из Azure Active Directory (AAD) в Azure AD DS по умолчанию. Продолжительность операции синхронизации зависит от числа объектов в Azure AD. Синхронизация сотен тысяч объектов может занять несколько дней. 

Доменное имя, используемое с Azure AD-DS, не должно превышать 39 символов для работы с HDInsight.

Можно выбрать синхронизацию только тех групп, которым требуется доступ к кластерам HDInsight. Этот вариант синхронизации только определенных групп называется *синхронизацией определенных объектов*. Инструкции см. в статье [Configure Scoped Synchronization from Azure AD to your managed domain](../../active-directory-domain-services/scoped-synchronization.md) (Настройка синхронизации определенных объектов из Azure AD в управляемый домен).

При включении защищенных протоколов LDAP укажите в сертификате доменное имя в качестве имени субъекта или альтернативного имени субъекта. Например, если ваше доменное имя — *contoso100.onmicrosoft.com*, убедитесь, что в сертификате существует такое же имя субъекта или альтернативное имя субъекта. Дополнительные сведения см. в разделе [Настройка защищенного протокола LDAP для управляемого домена Azure AD-DS](../../active-directory-domain-services/configure-ldaps.md). Ниже приведен пример создания самозаверяющего сертификата с именем домена (*contoso100.onmicrosoft.com*) в имени субъекта и DnsName (альтернативное имя субъекта).

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.onmicrosoft.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.onmicrosoft.com, contoso100.onmicrosoft.com
```

## <a name="check-azure-ad-ds-health-status"></a>Проверка состояния работоспособности Azure AD DS
Просмотрите состояние работоспособности доменных служб Active Directory Azure, выбрав **Работоспособность** в категории **Управление**. Убедитесь, что состояние Azure AD DS отображается с зеленым значком (выполняется) и синхронизация завершена.

![Работоспособность доменных служб Azure Active Directory](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="create-and-authorize-a-managed-identity"></a>Создание и авторизация управляемого удостоверения

**Управляемое удостоверение, назначаемое пользователем,** позволяет упростить и защитить операции в доменных службах. При назначении управляемому удостоверению роли участника доменных служб HDInsight можно считывать, создавать, изменять и удалять операции доменных служб. Для Корпоративный пакет безопасности HDInsight требуются некоторые операции доменных служб, такие как создание подразделений и субъектов-служб. Управляемые удостоверения можно создать в любой подписке. Дополнительные сведения об управляемых удостоверениях см. в статье [управляемые удостоверения для ресурсов Azure](../../active-directory/managed-identities-azure-resources/overview.md). Дополнительные сведения о работе управляемых удостоверений в Azure HDInsight см. [в статье управляемые удостоверения в Azure hdinsight](../hdinsight-managed-identities.md).

Чтобы настроить кластеры ESP, создайте управляемое удостоверение, назначаемое пользователем, если у вас его еще нет. Инструкции доступны в статье [Создание, получение списка, удаление ролей и их назначение для управляемого удостоверения, назначаемого пользователем, с помощью портала Azure](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md). Затем назначьте управляемому удостоверению роль **участника доменных служб HDInsight** в управлении доступом Azure AD DS (для такого назначения роли требуются привилегии администратора AAD-DS).

![Служба контроля доступа доменных служб Azure Active Directory](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Назначение роли **Участник доменных служб HDInsight** гарантирует, что в удостоверения есть подходящий (или не совсем) уровень доступа для выполнения определенных операций доменных служб в домене AAD-DS, например, подразделения, удаление подразделений и т. д.

После создания управляемого удостоверения и назначения нужной роли администратор AAD DS может настроить список тех, кто может использовать это управляемое удостоверение. Для настройки пользователей управляемого удостоверения администратор должен выбрать управляемое удостоверение на портале, а затем щелкнуть **Управление доступом (IAM)** в разделе **Обзор**. Затем в правой области назначьте пользователям или группам, которым нужно создать кластеры HDInsight ESP, роль **Оператора управляемого удостоверения**. Например, администратор AAD-DS может назначить эту роль группе **маркетингтеам** для управляемого удостоверения **сжмси** , как показано на следующем рисунке. Это гарантирует, что нужные люди в организации имеют доступ к использованию этого управляемого удостоверения для создания кластеров ESP.

![Назначение роли оператора управляемого удостоверения HDInsight](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-managed-identity-operator-role-assignment.png)

## <a name="networking-considerations"></a>Рекомендации по работе с сетями

> [!NOTE]  
> Azure AD-DS необходимо развертывать в виртуальной сети на базе Azure Resource Manager (ARM). Azure AD-DS не поддерживает классические виртуальные сети. Дополнительные сведения см. в статье [Включение доменных служб Azure Active Directory с помощью портала Azure](../../active-directory-domain-services/active-directory-ds-getting-started-network.md).

После включения доменных служб Azure AD локальный сервер службы доменных имен (DNS) будет запущен на виртуальных машинах AD. Настройте виртуальную сеть Azure AD DS, чтобы использовать эти настраиваемые DNS-серверы. Чтобы найти нужные IP-адреса, выберите **Свойства** в категории **управления** и просмотрите IP-адреса, появившиеся в списке под параметром **IP-адрес в виртуальной сети**.

![Поиск IP-адресов для локальных DNS-серверов](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns.png)

Измените конфигурацию DNS-серверов в виртуальной сети Azure AD DS, чтобы использовать эти настраиваемые IP-адреса, выбрав **DNS-серверы** в категории **Параметры**. Затем щелкните переключатель рядом с параметром **Custom** (Пользовательский), введите в указанное ниже текстовое поле первый IP-адрес и нажмите кнопку **Сохранить**. Добавьте дополнительные IP-адреса, используя те же шаги.

![Обновление конфигурации DNS виртуальной сети](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)

Проще разместить экземпляр Azure AD-DS и кластер HDInsight в одной и той же виртуальной сети Azure. Если вы планируете использовать разные виртуальные сети, необходимо их сверять, чтобы контроллер домена был виден в HDI виртуальных машин. Дополнительные сведения см. в статье [Пиринг между виртуальными сетями](../../virtual-network/virtual-network-peering-overview.md). 

После установки пиринговой связи между виртуальными сетями настройте виртуальную сеть HDInsight, чтобы использовать настраиваемый DNS-сервер, и введите частные IP-адреса Azure AD DS в качестве адресов DNS-сервера. Если обе виртуальные сети используют одни DNS-серверы, настраиваемое доменное имя разрешится к правильному IP-адресу и будет доступно в HDInsight. Например, `contoso.com` `ping contoso.com` если после выполнения этого шага имя домена будет разрешаться в правильный IP-адрес Azure AD-DS.

![Настройка пользовательских DNS-серверов для пиринговой виртуальной сети](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

Если вы используете правила групп безопасности сети (NSG) в подсети HDInsight, следует разрешить [необходимые IP-адреса](../hdinsight-management-ip-addresses.md) для входящего и исходящего трафика. 

**Чтобы проверить** правильность установки сети, присоедините виртуальную машину Windows к виртуальной сети или подсети HDInsight и проверьте связь по доменному имени (оно должно разрешить протокол IP), а затем запустите **ldp.exe** для доступа к домену Azure AD DS. Затем **присоедините эту виртуальную машину Windows к домену, чтобы подтвердить**, что все необходимые вызовы RPC между клиентом и сервером успешно выполнены. Можно также использовать **nslookup** для подтверждения сетевого доступа к учетной записи хранения или любой внешней базе данных (например, внешнему хранилищу метаданных Hive или базе данных Ranger).
Если доменные службы AAD защищены группой безопасности сети, убедитесь, что все [необходимые порты](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10)#communication-to-domain-controllers) находятся в списке разрешений в правилах NSG подсети AAD DS. Если присоединение к домену этого окна виртуальной машины прошло успешно, можно переходить к следующему шагу и создавать кластеры ESP.

## <a name="create-a-hdinsight-cluster-with-esp"></a>Создание кластера HDInsight с корпоративным пакетом безопасности

После правильной настройки предыдущих шагов следующим шагом будет создание кластера HDInsight с поддержкой ESP. При создании кластера HDInsight вы можете включить Корпоративный пакет безопасности на **пользовательской** вкладке. Если необходимо применить шаблон Azure Resource Manager для развертывания, используйте интерфейс портала один раз и загрузите предварительно заполненный шаблон на последней странице "Итоги" для повторного пользования в будущем.

> [!NOTE]  
> Первые шесть символов имени кластера ESP должно быть уникальными в вашей среде. Например, если имеется несколько кластеров ESP в разных виртуальных сетях, следует использовать соглашение об именовании, гарантирующее, что первые шесть символов имени кластера будут уникальными.

![Проверка домена Azure HDInsight с корпоративным пакетом безопасности](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

После включения корпоративного пакета безопасности будет выполняться автоматическое обнаружение и проверка распространенных неправильных настроек, связанных с Azure AD-DS. После исправления этих ошибок можно перейти к следующему шагу: 

![Непройденные проверки домена в Azure HDInsight с корпоративным пакетом безопасности](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

При создании кластера HDInsight с корпоративным пакетом безопасности необходимо указать следующие параметры.

- **Администратор кластера**. Выберите администратора кластера в синхронизированных службах Azure AD DS. Эта учетная запись домена должна быть синхронизирована и доступна в Azure AD-DS.

- **Группы доступа к кластеру**. Группы безопасности, пользователей которых нужно синхронизировать и предоставить им доступ к кластеру, должны быть доступны в Azure AD-DS. Например, группа HiveUsers. Дополнительные сведения см. в статье [Создание группы и добавление в нее пользователей в Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **URL-адрес LDAPS**. Например, `ldaps://contoso.com:636`.

На следующем снимке экрана показана успешная конфигурация на портале Azure:

![Конфигурация доменных служб Active Directory в Azure HDInsight с корпоративным пакетом безопасности](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).

Созданное управляемое удостоверение можно выбрать из раскрывающегося списка назначаемых пользователем управляемых удостоверений при создании нового кластера.

![Конфигурация доменных служб Active Directory в Azure HDInsight с корпоративным пакетом безопасности](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-identity-managed-identity.png).

## <a name="next-steps"></a>Следующие шаги

* Сведения о настройке политик Hive и выполнении запросов Hive см. в статье [Настройка политик Apache Hive в HDInsight с Корпоративным пакетом безопасности](apache-domain-joined-run-hive.md).
* Сведения о подключении к кластерам HDInsight с корпоративным пакетом безопасности с использованием SSH см. в разделе [Проверка подлинности при использовании присоединенного к домену кластера HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
