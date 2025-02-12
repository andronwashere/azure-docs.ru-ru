---
title: Планирование Azure Active Directory единый вход развертывания
description: Руководство, которое поможет планирование, развертывание и Управление единым ВХОДОМ в вашей организации.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e5278d504c43688bf064b869982938db52b1b1bf
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67164953"
---
# <a name="plan-a-single-sign-on-deployment"></a>Планирование развертывания единый вход

Единый вход (SSO) означает, что доступ к все приложения и ресурсы, необходимые пользователю вход только в том случае, когда с помощью одной учетной записи. С помощью единого входа пользователям все необходимые приложения без повторной проверки подлинности во второй раз.

## <a name="benefits-of-sso"></a>Преимущества единого входа

Единый вход (SSO) добавляет безопасность и удобство при входе пользователей приложения в Azure Active Directory (Azure AD). 

Во многих организациях полагаются на программное обеспечение как услуга (SaaS) приложений, таких как Office 365, Box и Salesforce, для повышения производительности работы конечного пользователя. Исторически сложилось так, что ИТ-специалистам приходилось отдельно создавать и обновлять учетные записи пользователей каждого приложения SaaS, а пользователям требовалось помнить пароли для каждого.

В магазине Azure содержатся более 3000 приложений с помощью предварительно интегрированных подключения единого входа, что позволяет легко интегрировать их в клиенте.

## <a name="licensing"></a>Лицензирование

- **Лицензирование Azure AD** -бесплатна единого входа для предварительно интегрированных приложений SaaS. Однако количество объектов в каталоге и возможности, которые вы хотите развернуть может потребоваться дополнительные лицензии. Полный список требований к лицензиям см. в разделе [Azure Active Directory: цены](https://azure.microsoft.com/pricing/details/active-directory/).
- **Лицензирование приложений** -потребуется соответствующие лицензии для приложений SaaS в соответствии с потребностями бизнеса. Обратитесь к владельцу приложения проверить соответствующие лицензии для их роли в приложении пользователей, назначенных для приложения. Если Azure AD управляет автоматической подготовки на основе ролей, роли, назначенные в Azure AD необходимо согласовать с количеством лицензий, принадлежащих в приложении. Неправильное количество лицензий, принадлежащих в приложении может привести к ошибкам во время подготовки или обновления пользователя.

## <a name="plan-your-sso-team"></a>Планирование вашей команде единого входа

- **Привлекать заинтересованных лиц к правой** — когда технология неудачи, проектов, правило из-за несоответствия ожиданий на влияния, результаты и обязанности. Чтобы избежать подобных просчетов [убедитесь, что вы сотрудничество с заинтересованными лицами правой](https://aka.ms/deploymentplans) а заинтересованным лицам ознакомьтесь с их ролями.
- **Планирование обмена данными** -связь критически важна для успеха любой новой службы. Заблаговременно взаимодействовать для пользователей о, как изменится впечатления пользователей, когда она станет и как получить поддержку, если они возникли проблемы с. Ознакомиться с вариантами для [приложений как конечные пользователи получают доступ к их SSO с поддержкой](end-user-experiences.md)и создать связи в соответствии с выбором. 

## <a name="plan-your-sso-protocol"></a>Планирование протокол единого входа

Реализация единого входа на основе протоколов федерации повышает безопасность, надежность, и конечного пользователя возникает, проще в реализации. Многие приложения, предварительно интегрированных в Azure AD с [шаг за шагом проведет доступных](../saas-apps/tutorial-list.md). Их можно найти на нашем [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). Подробные сведения для каждого метода единого входа можно найти в статье [единого входа в приложения в Azure Active Directory](what-is-single-sign-on.md).

Существует два основных способа, в которых можно включить пользователей единый вход в свои приложения:

- **С помощью федеративного единого входа** Azure AD проверяет подлинность пользователя в приложение с помощью своей учетной записью Azure AD. Этот метод поддерживается для приложений, поддержка протоколов, таких как SAML 2.0, WS-Federation или OpenID Connect и широчайший режим единого входа. Мы рекомендуем использовать федеративный единый вход с помощью Azure AD, если приложение поддерживает его, вместо единого входа по паролю и служб федерации Active Directory.

- **С использованием пароля единого входа** пользователи входят в приложение с именем пользователя и пароль первый раз, они доступ к нему. После первого входа Azure AD предоставляет приложению имя пользователя и пароль. Единый вход на основе пароля позволяет безопасно хранить пароли приложений и воспроизводить его с помощью расширения веб-браузера или мобильного приложения. Этот параметр использует существующий процесс входа предоставляется приложением, позволяет администратору управлять паролями и не требует от пользователя запоминать пароль.

### <a name="considerations-for-federation-based-sso"></a>Рекомендации для единого входа на основе федерации

- **С помощью OpenID Connect и OAuth** — Если приложение, вы подключаетесь к поддерживает его, используйте метод OIDC/OAuth 2.0 для включения приложение SSO к этому приложению. Этот метод требуется сократить количество конфигураций и позволяет пользователям более широкие возможности. Дополнительные сведения см. в разделе [OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md), [OpenID Connect 1.0](../develop/v2-protocols-oidc.md), и [руководство разработчика по Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).
- **Конфигурации конечных точек для единого входа на основе SAML** -при использовании SAML, ваши разработчики должны будут определенные сведения, прежде чем настраивать службу приложения. Дополнительные сведения см. в разделе [настроить основные параметры SAML](configure-single-sign-on-portal.md).
- **Сертификат управления для единого входа на основе SAML** — при включении федеративный единый вход для вашего приложения, Azure AD создает сертификат, который по умолчанию для трех лет. Вы можете настроить срок действия этого сертификата, при необходимости. Убедитесь, что у вас есть процессы обновление сертификатов до их истечения. Дополнительные сведения см. в разделе [сертификатах управления Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs).

### <a name="considerations-for-password-based-sso"></a>Рекомендации для единого входа по паролю

С помощью Azure AD для единого входа на основе пароля необходимо развернуть расширение обозревателя будет безопасно получить учетные данные и заполнять формы входа. Определяет механизм для развертывания расширения нужного масштаба с помощью [поддерживаемые браузеры](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction). Возможны следующие значения.

- [Групповой политики для Internet Explorer](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)
- [System Center Configuration Manager (SCCM) для Internet Explorer](https://docs.microsoft.com/sccm/core/clients/deploy/deploy-clients-to-windows-computers)
- [Управляемая загрузку и конфигурацию для Chrome, Firefox, Microsoft Edge или IE пользователем](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Дополнительные сведения см. в разделе [как для настройки пароль единого входа](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-password-sso-non-gallery).

#### <a name="capturing-login-forms-metadata-for-applications-that-arent-in-the-gallery"></a>Запись метаданных формы входа для приложений, которые отсутствуют в коллекции.

Корпорация Майкрософт поддерживает записи метаданных веб-приложения для пароля, хранение (перехват поля имени пользователя и пароль). Перейдите к URL-адрес входа во время процесса настройки приложения для получения метаданных форм. Для URL-адрес входа точное, попросите владельца приложения. Эта информация используется в процессе единого входа сопоставление учетные данные Azure AD в приложение во время единого входа.

Дополнительные сведения см. в разделе [что такое доступ к приложениям и ЕДИНЫЙ вход Azure AD? — единый вход по паролю](https://azure.microsoft.com/documentation/articles/active-directory-appssoaccess-whatis/).

#### <a name="indications-that-metadata-in-forms-needs-to-be-recaptured"></a>Чтобы узнать эти метаданные в формах необходимо освободить

Когда приложения меняются их HTML-разметку, может потребоваться повторная запись метаданных с учетом изменений. Перечислены общие признаки, указывающие на наличие изменений в HTML-разметку.

- Пользователи отчетов, что нажатие кнопки в приложении «зависает» на странице входа
- Пользователи отчетов, что имя пользователя или пароль не заполнен

#### <a name="shared-accounts"></a>Общие учетные записи

С точки зрения входа в систему приложения с общим учетным записям не отличается от приложения из коллекции, использующий единый вход с паролем для отдельных пользователей. Однако существуют некоторые дополнительные действия, необходимые при планировании и настройке приложения предназначены для использования общих учетных записей:

1. Работать с бизнес-пользователей приложения документировать следующее:
   1. Набор пользователей в организации, который будет использовать приложение
   1. Существующий набор учетных данных в приложении, связанном с набором пользователей 
1. Для каждого сочетания набор пользователей и учетные данные создайте группу безопасности в облаке или локально на основе требований.
1. Сбросьте общих учетных данных. После развертывания приложения в Azure AD отдельных пользователей не требуется пароль общей учетной записи. Так как Azure AD будет сохранить пароль, рассмотрите возможность установки этого очень длинные и сложные. 
1. Настройте автоматическую смену пароля, если ее поддерживает приложение. Таким образом, даже не администратор, выполнивший первоначальной настройки будет знать пароль общей учетной записи. 

## <a name="plan-your-authentication-method"></a>Планирование метод проверки подлинности

Выбор правильного метода аутентификации является основным первым решением при настройке решения для гибридной идентификации Azure AD. Реализация метода аутентификации настраивается с помощью Azure AD Connect, что также дает возможность подготовить пользователей в облаке.

Чтобы выбрать метод аутентификации, необходимо учитывать время, имеющуюся инфраструктуру, сложность и стоимость реализации решения. Эти факторы отличаются для каждой организации и могут изменяться с течением времени. Необходимо выбрать тот, который наиболее близко соответствует конкретного сценария. Дополнительные сведения см. в разделе [Выбор правильного метода аутентификации для гибридной идентификации Azure Active Directory](https://docs.microsoft.com/azure/security/azure-ad-choose-authn).

## <a name="plan-your-security-and-governance"></a>Планирование безопасности и управления 

Удостоверение обусловлено тем, новый элемент управления pivot основной для безопасности и инвестиций периметра сети становятся все более проницаемым и вступают в силу с развертывании устройств BYOD и облачных приложений. 

### <a name="plan-access-reviews"></a>Планирование проверки доступа

[Проверки доступа](https://docs.microsoft.com/azure/active-directory/governance/create-access-review) позволяют организациям эффективно управлять членством, доступ к корпоративным приложениям и назначения ролей. Необходимо проверить доступ пользователя на регулярной основе, чтобы убедиться, что только авторизованным пользователям постоянный доступ.

Ниже перечислены некоторые ключевые разделы для планирования при настройке проверки доступа.

1. Определяющий периодичность для проверки доступа, который соответствует требованиям бизнеса. Это может быть, как часто один раз в неделю, ежемесячно, ежегодно или в качестве упражнения по запросу.

1. Создайте группы, представляющие рецензентов для приложения доступ к отчетам. Необходимо обеспечить участников вашей проверки доступа заинтересованных лиц, знакомые с приложением и его целевых пользователей и вариантов использования

1. Завершение проверки доступа включает в себя устранив разрешения на доступ приложения пользователям, которые больше не нужен доступ. Планирование обработки потенциальных запросов в службу поддержки от запрещенного пользователей. Удаленного пользователя останется удаленные в Azure AD в течение 30 дней, во время которого они могут быть восстановлены администратором при необходимости. По истечении 30 дней пользователь будет окончательно удален. С помощью портала Azure Active Directory, глобальный администратор может явно окончательно удалить недавно удаленного пользователя до достижения этого периода времени.

### <a name="plan-auditing"></a>План аудита

Azure AD предоставляет [отчеты, содержащие технические и бизнес-аналитики](https://azure.microsoft.com/documentation/articles/active-directory-view-access-usage-reports/). 

Доступны безопасности и отчеты о действиях. Отчеты о безопасности Показать пользователей в группе риска и рискованных входов в систему. Отчеты о действиях помогут вам понять поведение пользователей в вашей организации за счет входа в систему с подробным описанием и журналы аудита всех имен входа. Отчеты можно использовать для управления рисками, повышают производительность и мониторинг соответствия требованиям.

| Тип отчета | Проверки доступа | Отчеты о безопасности | В отчете |
|-------------|---------------|------------------|----------------|
| Использовать для просмотра | Разрешения приложения и использования. | Потенциально скомпрометированных учетных записях | Кто получает доступ к приложениям |
| Возможные действия | Аудит доступа; REVOKE, Отмена разрешений | Отозвать доступ; принудительно сброса безопасности | Отозвать доступ |

Azure AD хранит большинство данных аудита в течение 30 дней и делает данные доступными через портал администрирования Azure или API для загрузки в системы анализа.

### <a name="consider-using-microsoft-cloud-application-security"></a>Рассмотрите возможность использования Microsoft Cloud Application Security

Безопасность приложений Microsoft Cloud (MCAS) — это решение брокер безопасности облачных доступа (CASB). Он обеспечивает видимость облачных приложений и служб, предоставляет реализовать сложную аналитику для идентификации и борьбы с киберугроз и позволяет контролировать, каким образом передаются данные.

Развертывание MCAS позволяет делать следующее:

- Используйте Cloud Discovery для сопоставления и определения облачной среды и облачных приложений, который используется в вашей организации.
- Разрешает и отменяет санкционирование приложений в облаке
- Использование соединителей легко развертываемых приложений, которые используют преимущества интерфейсов API поставщиков, для обеспечения видимости и контроля приложений, которые подключаются к
- Используйте защиты Conditional Access App Control, чтобы получить в режиме реального времени отслеживать и контролировать доступ и действий в ваших облачных приложениях
- Помогает непрерывный контроль благодаря параметр и их постоянной точной настройке, политики.

Управление сеансом безопасности приложения Microsoft Cloud (MCAS) доступна для любых браузеров на любых популярных платформах, в любой операционной системе. Мобильные и Классические приложения можно также заблокирован или разрешен. За счет полной интеграции с Azure AD, все приложения, которые используют конфигурацию SAML, Open ID Connect приложений или с помощью единого входа в Azure AD может поддерживаться, включая [несколько рекомендуемых приложений](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad).

Сведения о MCAS, см. в разделе [Общие сведения о Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security). MCAS — это служба подписки на основе пользователя. Вы можете просмотреть сведения о лицензировании в [таблицу лицензирования MCAS](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE2NXYO).

### <a name="use-conditional-access"></a>Использование условного доступа

С помощью условного доступа вы можете автоматизировать решений по управлению доступом на основе критериев для облачных приложений.

Политики условного доступа применяются после завершения первой двойной проверки подлинности. Таким образом условного доступа не является первой линией обороны строки для сценариев, такие как атаки типа "отказ в обслуживании" (DoS), но можно использовать сигналы, поступающие из этих событий для определения доступа к. Например уровень риска входа можно использовать расположение данного запроса и т. д. Дополнительные сведения об условном доступе см. в разделе [Обзор](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access) и [плана развертывания](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access).

## <a name="azure-sso-technical-requirements"></a>Технические требования Azure единого входа

В следующем разделе описаны требования к настройке конкретного приложения, включая необходимые окружений, конечные точки, сопоставление утверждения, необходимые атрибуты, сертификаты и протоколы, используемые. Вам потребуется эту информацию, чтобы настроить единый вход в [портала Azure AD](https://portal.azure.com/).

### <a name="authentication-mechanism-details"></a>Сведения о механизм проверки подлинности

Для всех предварительно интегрированных приложений SaaS Корпорация Майкрософт предоставляет руководство и эти сведения не потребуются. Если приложение не находится в нашем магазине приложений / коллекции, может потребоваться собрать следующие данные:

- **Текущий поставщик удостоверений, приложение использует для единого входа, если применимо** — например: AD FS, PingFederate, Okta
- **Протоколы, поддерживаемые целевым приложением** — например, SAML 2.0, OpenID Connect, OAuth, проверка подлинности на основе форм, WS-Fed, WS-Trust
- **Протокол, настраивается с помощью Azure AD** — например, SAML 2.0 или 1.1, OpenID Connect, OAuth, на основе форм, WS-Fed

### <a name="attribute-requirements"></a>Требования к атрибутам

Имеется ряд предварительно настроенных атрибутов и сопоставлений атрибутов между объектами пользователей Azure AD и каждого приложения SaaS. Некоторые приложения управляют другими типами объектов, таких как группы. Планирование сопоставления атрибутов пользователей из Azure AD в приложение и [настройки сопоставления атрибута по умолчанию](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) в соответствии с вашего бизнеса требуется.

### <a name="certificate-requirements"></a>Требования к сертификатам

Для этого приложения сертификат актуальными должны быть или есть риск, пользователи не могут получить доступ к приложению. Большинство сертификатов приложения SaaS хорошо подходят для 36 месяцев. Можно изменить, в течение сертификата в колонке приложения. Убедитесь в том, что для документирования истечения срока действия и знать, как вы будете управлять обновлением сертификата. 

Существует два способа управления сертификатами. 

- **Автоматическая смена сертификатов** -Корпорация Майкрософт поддерживает [смена ключей подписывания в Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Хотя это предпочтительным средством для управления сертификатами, не все ISV поддерживает такой сценарий.

- **Обновление вручную** -каждое приложение имеет свой собственный сертификат, истечения срока действия зависимости от того, как он определен. До истечения срока действия сертификата приложения, создать новый сертификат и отправить его для независимых поставщиков программного обеспечения. Эта информация может быть извлечено из метаданных федерации. [Узнайте больше о метаданных федерации здесь.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata)

## <a name="implement-sso"></a>Реализация единого входа

Используйте следующие этапы для планирования и развертывания решения в вашей организации:

### <a name="user-configuration-for-sso"></a>Конфигурация пользователя для единого входа

- **Определение тестовых пользователей**

   Обратитесь к владельцу приложения и попросите его для создания как минимум три тестовых пользователей в приложении. Убедитесь, сведения, которое будет использоваться в качестве первичного идентификатора заполнен правильно и соответствует атрибуту, который доступен в Azure AD. В большинстве случаев это будет сопоставлен «NameID» для приложений на основе SAML. Для токенов JWT является «preferred_username.»
   
   Создайте пользователя в Azure AD либо вручную, как пользователь облачных или синхронизировать пользователя из локально с помощью модуля синхронизации Azure AD Connect. Убедитесь, что соответствуют утверждения, отправляемые в приложение.

- **Настройка единого входа**

   Из [список приложений](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list), найдите и откройте руководства единого входа для приложения, а затем сделайте данного учебного курса на успешно настроить приложение SaaS.

   Если вы не можете найти приложения, см. в разделе [документации пользовательское приложение](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Это поможет выполнить о том, как добавить приложение, которое не находится в коллекции Azure AD.

   При необходимости можно использовать утверждений, выпущенных в маркере SAML для корпоративных приложений с помощью [документацией корпорации Майкрософт](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping). Убедитесь, что это значение соответствует значению, что вы рассчитываете получить в ответе SAML для вашего приложения. Если возникают проблемы во время настройки на наше руководство [отладка единого входа интеграции](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging).

Адаптация пользовательского приложения является средством лицензии Azure AD Premium P1 или P2.

### <a name="provide-sso-change-communications-to-end-users"></a>Обеспечения связи Изменение единого входа для конечных пользователей

Реализуйте план коммуникации. Убедитесь, что вы даете конечным пользователям узнать, что изменение поступает, при его поступлении, что делать и как обратиться за помощью.

### <a name="verify-end-user-scenarios-for-sso"></a>Проверка сценариев конечного пользователя для единого входа

Можно использовать следующие тестовые случаи для проведения тестов на корпоративных и личных устройств, чтобы убедиться в конфигурации единого входа работают надлежащим образом. В ситуациях ниже предполагается, что пользователь переход к URL-адрес приложения и через поток проверки подлинности, инициированный поставщиком услуг (поток проверки подлинности, инициированный поставщиком услуг).

| Сценарий | Ожидаемый результат в поток проверки подлинности, инициированный поставщиком услуг пользователем |
|----------|---------------------------------------------------|
| Имя для входа в приложение с IE, а в корпоративной сети. | Встроенную проверку подлинности Windows (IWA) происходит с без дополнительных запросов. |
| Имя для входа в приложение с IE, хотя перестают с новой попытки входа. | Запрос на основе форм на сервере AD FS. Пользователь успешно входит в и браузер предлагает для многофакторной проверки Подлинности. |
| Войдите в приложение с IE, хотя перестают с текущим сеансом, никогда не выполнялась многофакторной проверки Подлинности. | Пользователь не получает запрос на первый фактор. Пользователь получает приглашение для многофакторной проверки Подлинности. |
| Войдите в приложение с IE, хотя перестают с текущим сеансом и уже прошел многофакторной проверки Подлинности в этом сеансе. | Пользователь не получает запрос на первый фактор. Пользователи не получают многофакторной проверки Подлинности. Правильнее пользователя в приложение. |
| Имя входа для приложения с помощью Chrome или Firefox и Safari из корпоративной сети с текущим сеансом системы, уже выполнялась многофакторной проверки Подлинности в этом сеансе. | Пользователь не получает запрос на первый фактор. Пользователи не получают многофакторной проверки Подлинности. Пользователь единого входа в приложение. |
| Войдите в приложение с Chrome или Firefox и Safari из корпоративной сети с помощью новой попытки входа системы. | Запрос на основе форм на сервере AD FS. Пользователь успешно входит в и браузер предлагает для многофакторной проверки Подлинности. |
| Имя входа для приложения с помощью Chrome или Firefox, а в корпоративной сети с текущим сеансом. | Пользователь не получает запрос на первый фактор. Пользователи не получают многофакторной проверки Подлинности. Пользователь единого входа в приложение. |
| Имя входа для приложения с мобильным приложением приложения с новой попытки входа. | Запрос на основе форм на сервере AD FS. Пользователь успешно входит в и клиентом ADAL будет запрашивать параметры многофакторной проверки Подлинности. |
| Неавторизованный пользователь пытается войти в приложение с URL-адрес входа. | Запрос на основе форм на сервере AD FS. Пользователю не удается выполнить вход с помощью первый фактор. |
| Авторизованный пользователь пытается войти систему, но вводит неправильный пароль. | Пользователь переходит по URL-АДРЕСУ приложения и получает сообщение об ошибке ввода неправильного имени пользователя и пароля. |
| Авторизованный пользователь щелкает ссылку в сообщении электронной почты и уже прошел проверку подлинности. | Пользователь щелкает URL-адрес и входят в приложение с без дополнительных запросов. |
| Авторизованный пользователь щелкает ссылку в сообщении электронной почты и не еще проверку подлинности. | Пользователь нажимает кнопку на URL-адрес и запрос на проверку подлинности с помощью первый фактор. |
| Право входа пользователя в приложение с мобильным приложением приложения (инициированный поставщиком услуг) с новой попытки входа. | Запрос на основе форм на сервере AD FS. Пользователь успешно входит в и клиентом ADAL будет запрашивать параметры многофакторной проверки Подлинности. |
| Неавторизованный пользователь пытается войти в приложение с URL-адрес входа (инициированный поставщиком услуг). | Запрос на основе форм на сервере AD FS. Пользователю не удается выполнить вход с помощью первый фактор. |
| Авторизованный пользователь пытается войти систему, но вводит неправильный пароль.| Пользователь переходит по URL-АДРЕСУ приложения и получает сообщение об ошибке ввода неправильного имени пользователя и пароля. |
| Авторизованный пользователь выполняет выход и затем вход. | Если настройки URL-адрес выхода пользователя из всех служб и запрос на проверку подлинности. |
| Авторизованный пользователь выполняет выход и затем вход. | Если URL-адрес выхода не настроен, пользователь автоматически войти снова с использованием существующего токена из существующего сеанса обозревателя Azure AD. |
| Авторизованный пользователь щелкает ссылку в сообщении электронной почты и уже прошел проверку подлинности. | Пользователь щелкает URL-адрес и входят в приложение с без дополнительных запросов. |
| Авторизованный пользователь щелкает ссылку в сообщении электронной почты и не еще проверку подлинности. | Пользователь нажимает кнопку на URL-адрес и запрос на проверку подлинности с помощью первый фактор. |

## <a name="manage-sso"></a>Управление единым ВХОДОМ

В этом разделе описываются требования и рекомендации для успешного управления единого входа.

### <a name="required-administrative-roles"></a>Необходимые административные роли

Всегда используете роль с наименьшим количеством разрешений, доступных для выполнения требуемой задачи в Azure Active Directory. Корпорация Майкрософт рекомендует [просмотрите разные роли, доступные](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) и выбрать нужные решать различные задачи для каждого пользователя для этого приложения. Некоторые роли может потребоваться временно применяться и удалены после завершения развертывания.

| Пользователь| Роли | Роль Azure AD (при необходимости) |
|--------|-------|-----------------------------|
| Помочь администратору службы поддержки | Поддержка уровня 1 | Нет |
| Удостоверение администратора | Настройка и отладить, когда проблемы влиять на Azure AD | глобальный администратор; |
| Администратор приложений | Аттестация пользователя в приложении, конфигурация на пользователей с разрешениями | Нет |
| Администраторы инфраструктуры | Смена владельца сертификата | глобальный администратор; |
| Владелец заинтересованные | Аттестация пользователя в приложении, конфигурация на пользователей с разрешениями | Нет |

Мы рекомендуем использовать [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) пользователями (PIM) для управления вашей роли для предоставления дополнительных аудита, управления и доступа просмотрите для пользователей с разрешениями каталога.

### <a name="sso-certificate-lifecycle-management"></a>Управление жизненным циклом сертификата единого входа

Очень важно для определения правильных ролей и электронной почты в списки рассылки, занимающихся обеспечением жизненного цикла сертификата для подписи между Azure AD и приложения, которое настраивается с помощью единого входа. Ниже приведены некоторые из ключевых функций, которые мы рекомендуем использовать на месте.

- Владелец для обновления свойств пользователя в приложении
- Владелец по требованию для поддержки замены или ремонта приложения
- Список распространения тесно отслеживаемых сообщений электронной почты для уведомлений об изменениях, связанных с сертификатами

Максимальное время существования сертификата составляет три года. Мы рекомендуем налаживания процессов на том, как будут обрабатываться изменение сертификата между Azure AD и приложения. Это может помочь предотвратить или свести к минимуму сбоя из-за истечения срока действия сертификата или принудительно Смена сертификатов.

### <a name="rollback-process"></a>Отката

После завершения тестирования в зависимости от тестовых случаев, пришло время для перемещения в рабочую среду с вашим приложением. Перемещение в рабочую среду означает, что будет реализовать ваш запланированных проверенных конфигураций в своем клиенте и разверните ее для пользователей. Тем не менее очень важно для планирования, что делать в случае, если развертывание не могу как планировалось. При сбое конфигурацию единого входа во время развертывания, необходимо понять, как избежать любой сбой и сократить воздействие на пользователей.

Доступность методов проверки подлинности в приложении определит оптимальным вариантом. Необходимо всегда проверять, подробную документацию для владельцев приложения о том, как именно вернуться в исходное состояние конфигурации имени входа в случае развертывания возникают проблемы.

- **Если приложение поддерживает несколько поставщиков удостоверений**, для примера LDAP и AD FS и запрос проверки связи, не удалить существующую конфигурацию единого входа во время развертывания. Вместо этого отключите ее во время миграции на случай необходимости перевести его обратно позже. 

- **Если приложение не поддерживает несколько поставщиков удостоверений** , но позволяет пользователям вход с использованием проверки подлинности на основе форм (имя пользователя и пароль), убедитесь, что пользователи могут переключиться на этот подход в случае сбоя нового развертывания конфигурации единого входа.

### <a name="access-management"></a>управление доступом

Рекомендуется выбирать масштабированное подход при управлении доступом к ресурсам. Типичные подходы включают использование локальных групп, синхронизируя через Azure AD Connect, [создание динамических групп в Azure AD на основе атрибутов пользователя](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal), или создание [групп самообслуживания](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) в Azure AD управляет владелец ресурса.

### <a name="monitor-security"></a>Мониторинг безопасности

Мы рекомендуем настроить регулярно выпускать, в котором вы просмотрите различные аспекты безопасности приложения SaaS и выполнения корректирующих действий, которые являются обязательными.

### <a name="troubleshooting"></a>Устранение неполадок

Следующие ссылки предоставляют сценарии устранения неполадок. Можно создать структуру, определенных для сотрудников поддержки, который содержит эти сценарии и действия для их устранения.

#### <a name="consent-issues"></a>Разрешение проблем

- [Ошибка Непредвиденная согласия](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-prompt)

- [Ошибка при предоставлении согласия пользователем](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-error)

#### <a name="sign-in-issues"></a>Проблемы при входе

- [Проблемы при входе с пользовательского портала](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-deeplink)

- [Проблемы при входе с панели доступа](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-access-panel)

- [Ошибка на странице входа в приложение](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-application-error)

- [Проблема при входе в приложение Майкрософт](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-first-party-microsoft)

#### <a name="sso-issues-for-applications-listed-in-the-azure-application-gallery"></a>Проблемы единого входа для приложений, перечисленных в коллекции приложений Azure

- [Проблема с паролем единого входа для приложений, перечисленных в коллекции приложений Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-gallery) 

- [Проблема с федеративного единого входа для приложений, перечисленных в коллекции приложений Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-gallery)   

#### <a name="sso-issues-for-applications-not-listed-in-the-azure-application-gallery"></a>Проблемы единого входа для приложения, не определенные в коллекции приложений Azure

- [Проблема с паролем единого входа для приложения, не определенные в коллекции приложений Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-non-gallery) 

- [Проблема с федеративного единого входа для приложения, не определенные в коллекции приложений Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-non-gallery)

## <a name="next-steps"></a>Дальнейшие действия

[Отладка единого входа на основе SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

[Сопоставление утверждений для приложений с помощью PowerShell](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping)

[Настройка утверждений, выпущенных в токене SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

[Протокол единого входа SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

[Протокол единого выхода SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-out-protocol-reference)

[Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) (для внешних пользователей, например партнерами и поставщиками)

[Условный доступ Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)

[Защита идентификации Azure](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

[Доступ с единым входом](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

[Руководство по использованию единого входа приложения](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
