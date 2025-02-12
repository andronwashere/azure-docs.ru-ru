---
title: 'Azure AD Connect выполняет следующие функции: поддерживаемые топологии | Документация Майкрософт'
description: В этой статье подробно описываются поддерживаемые и неподдерживаемые топологии Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 1034c000-59f2-4fc8-8137-2416fa5e4bfe
ms.service: active-directory
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: conceptual
ms.date: 11/27/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8b1c0d33a7d920f76bcbea6d8d6babc7390003bc
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60383887"
---
# <a name="topologies-for-azure-ad-connect"></a>Топологии Azure AD Connect.
В этой статье описываются различные локальные топологии и топологии Active Directory Azure (Azure AD), в которых служба синхронизации Azure AD Connect используется в качестве основного решения интеграции. Здесь описываются и поддерживаемые, и неподдерживаемые конфигурации.


Ниже приведены условные обозначения для изображений в статье.

| Описание | Знак |
| --- | --- |
| Локальный лес Active Directory |![Локальный лес Active Directory](./media/plan-connect-topologies/LegendAD1.png) |
| Локальная служба Active Directory с фильтрацией импорта |![Active Directory с фильтрацией импорта](./media/plan-connect-topologies/LegendAD2.png) |
| Сервер синхронизации Azure AD Connect |![Сервер синхронизации Azure AD Connect](./media/plan-connect-topologies/LegendSync1.png) |
| Промежуточный режим сервера синхронизации Azure AD Connect |![Промежуточный режим сервера синхронизации Azure AD Connect](./media/plan-connect-topologies/LegendSync2.png) |
| GALSync с Forefront Identity Manager (FIM) 2010 или Microsoft Identity Manager (MIM) 2016 |![GALSync с FIM 2010 или MIM 2016](./media/plan-connect-topologies/LegendSync3.png) |
| Сервер синхронизации Azure AD Connect, подробно |![Сервер синхронизации Azure AD Connect, подробно](./media/plan-connect-topologies/LegendSync4.png) |
| Azure AD |![Azure Active Directory](./media/plan-connect-topologies/LegendAAD.png) |
| Неподдерживаемый сценарий |![Неподдерживаемый сценарий](./media/plan-connect-topologies/LegendUnsupported.png) |


> [!IMPORTANT]
> Мы поддерживаем изменение или использование служб синхронизации Azure AD Connect только в контексте официально задокументированных конфигураций или действий. Любые иные конфигурации или действия могут привести к несогласованному или неподдерживаемому состоянию служб синхронизации Azure AD Connect. Для таких развертываний Майкрософт не предоставляет техническую поддержку.


## <a name="single-forest-single-azure-ad-tenant"></a>Один лес, один клиент Azure AD
![Топология для одного леса и одного клиента](./media/plan-connect-topologies/SingleForestSingleDirectory.png)

Стандартная топология — один локальный лес с одним или несколькими доменами и один клиент Azure AD. Для аутентификации Azure AD используется синхронизация хэша паролей. Экспресс-установка Azure AD Connect поддерживает только эту топологию.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Один лес, несколько серверов синхронизации в одном клиенте Azure AD
![Неподдерживаемая отфильтрованная топология для одного леса](./media/plan-connect-topologies/SingleForestFilteredUnsupported.png)

Подключение нескольких серверов синхронизации Azure AD Connect (за исключением [промежуточного сервера](#staging-server)) к одному клиенту Azure AD не поддерживается. Такая схема не поддерживается, даже если эти серверы настроены для синхронизации с взаимоисключающим набором объектов. Возможно, вы рассматривали такую топологию, если у вас нет доступа ко всем доменам леса с одного сервера или если требуется распределить нагрузку на несколько серверов.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Несколько лесов, один клиент Azure AD
![Топология для нескольких лесов и одного клиента](./media/plan-connect-topologies/MultiForestSingleDirectory.png)

Во многих организациях есть среды, которые включают несколько локальных лесов Active Directory. Есть разные причины существования нескольких локальных лесов Active Directory. Типичными примерами являются проекты с лесами ресурсов учетных записей и лесами, появившимися в результате слияния или поглощения.

Если у вас есть несколько лесов, все они должны быть доступными для одного сервера синхронизации Azure AD Connect. Присоединять этот сервер к домену не обязательно. Если необходимо получить доступ ко всем лесам, можно разместить сервер в сети периметра (также называется DMZ и промежуточной подсетью).

Мастер установки Azure AD Connect предлагает несколько вариантов консолидации пользователей, представленных в нескольких лесах. Каждый пользователь должен быть представлен в Azure AD только один раз. Есть несколько распространенных топологий, которые можно настроить в режиме выборочной установки в мастере установки. На странице **Уникальная идентификация пользователей** выберите вариант, соответствующий вашей топологии. Функция консолидации настроена только для пользователей. В конфигурации по умолчанию дубликаты групп не консолидируются.

В следующем разделе описываются такие стандартные топологии, как отдельные топологии, [полная сетка](#multiple-forests-full-mesh-with-optional-galsync) и [топология ресурсов учетной записи](#multiple-forests-account-resource-forest).

При стандартной конфигурации Azure AD Connect предполагается следующее.

* У каждого пользователя есть только одна включенная учетная запись. Лес, в котором располагается эта учетная запись, используется для проверки подлинности пользователя. Это предполагается при использовании синхронизации хэша паролей, сквозной аутентификации и федерации. Параметры UserPrincipalName и sourceAnchor/immutableID поступают из этого леса.
* У каждого пользователя есть только один почтовый ящик.
* Лес, в котором размещаются почтовые ящики пользователей, характеризуется наилучшим качеством данных для атрибутов, отображаемых в глобальном списке адресов Exchange (GAL). Если у пользователя нет почтового ящика, для передачи значений этих атрибутов может использоваться любой лес.
* Если у пользователя есть связанный почтовый ящик, то есть и другая учетная запись в другом лесу, которая используется для входа.

Если среда не соответствует этим предположениям, произойдет следующее:

* Если у вас несколько активных учетных записей или несколько почтовых ящиков, модуль синхронизации выберет одну учетную запись или один почтовый ящик и проигнорирует остальные.
* Связанный почтовый ящик, для которого нет другой активной учетной записи, не будет экспортирован в Azure AD. Учетная запись пользователя не будет представлена как член какой-либо группы. В DirSync связанный почтовый ящик всегда будет представлен как обычный почтовый ящик. Такое отличие в поведении является намеренным и служит для поддержки различных сценариев леса.

Дополнительные сведения см. в статье [Службы синхронизации Azure AD Connect: рекомендации по изменению конфигурации по умолчанию](concept-azure-ad-connect-sync-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Несколько лесов, несколько серверов синхронизации в одном клиенте Azure AD
![Неподдерживаемая топология для нескольких лесов и нескольких серверов синхронизации](./media/plan-connect-topologies/MultiForestMultiSyncUnsupported.png)

Подключение нескольких серверов синхронизации Azure AD Connect к одному клиенту Azure AD не поддерживается. В порядке исключения можно использовать [промежуточный сервер](#staging-server).

Эта топология отличается от описанной ниже тем, что подключение **нескольких серверов синхронизации** к одному клиенту Azure AD не поддерживается.

### <a name="multiple-forests-single-sync-server-users-are-represented-in-only-one-directory"></a>Несколько лесов, один сервер синхронизации, пользователи представлены только в одном каталоге
![Параметр для представления пользователей во всех каталогах только один раз](./media/plan-connect-topologies/MultiForestUsersOnce.png)

![Изображение нескольких лесов и отдельных топологий](./media/plan-connect-topologies/MultiForestSeparateTopologies.png)

В этой среде все локальные леса рассматриваются как отдельные сущности. В любых других лесах пользователи отсутствуют. У каждого леса есть своя организация Exchange. Синхронизация GALSync между лесами отсутствует. Такая топология может возникать в результате слияний и поглощений, а также в организации, в которой все подразделения функционируют независимо. В Azure AD эти леса будут находиться в одной организации и отображаться в едином глобальном списке адресов. На предыдущем рисунке каждый объект в каждом лесу представлен в метавселенной в единственном экземпляре с объединением в целевом клиенте Azure AD.

### <a name="multiple-forests-match-users"></a>Несколько лесов: сопоставление пользователей
Для всех сценариев этого рода характерно то, что группы рассылки и группы безопасности могут состоять из пользователей, контактов и внешних субъектов безопасности. Внешние участники безопасности используются в доменных службах Active Directory (AD DS) для представления элементов из других лесов в группе безопасности. Все внешние субъекты безопасности разрешаются в реальный объект в Azure AD.

### <a name="multiple-forests-full-mesh-with-optional-galsync"></a>Несколько лесов: полная сетка с дополнительным решением GALSync
![Параметр для использования атрибута почты для сопоставления в случае, когда удостоверения пользователей присутствуют в нескольких каталогах](./media/plan-connect-topologies/MultiForestUsersMail.png)

![Топология полной сетки для нескольких лесов](./media/plan-connect-topologies/MultiForestFullMesh.png)

Топология полной сетки позволяет пользователям и ресурсам размещаться в любом лесу. Как правило, между лесами образуются двусторонние отношения доверия.

Если Exchange присутствует в нескольких лесах, может быть доступным дополнительное локальное решение GALSync. Каждый пользователь будет представлен во всех остальных лесах как контакт. GALSync часто реализуется через FIM 2010 или MIM 2016. Azure AD Connect нельзя использовать в локальном решении GALSync.

В этом сценарии объекты удостоверений соединяются с помощью атрибута почты. Пользователь с почтовым ящиком в одном лесу объединяется с контактами в других лесах.

### <a name="multiple-forests-account-resource-forest"></a>Несколько лесов: лес ресурсов учетной записи
![Параметр для использования атрибутов ObjectSID и msExchMasterAccountSID для сопоставления, когда удостоверения присутствуют в нескольких каталогах](./media/plan-connect-topologies/MultiForestUsersObjectSID.png)

![Топология леса ресурсов учетной записи для нескольких лесов](./media/plan-connect-topologies/MultiForestAccountResource.png)

В топологии леса ресурсов учетной записи есть один или несколько лесов *учетных записей* с активными учетными записями пользователей. Может также существовать один (или несколько) лес *ресурсов* с отключенными учетными записями.

В этом сценарии один (или несколько) лес ресурсов доверяет всем лесам учетных записей. Обычно в лесу ресурсов используется расширенная схема Active Directory с Exchange и Lync. Все службы Exchange и Lync, а также другие общие службы находятся в этом лесу. У пользователей есть отключенные учетные записи в этом лесу. С лесом учетных записей связан почтовый ящик.

## <a name="office-365-and-topology-considerations"></a>Аспекты топологии в Office 365.
Некоторые рабочие нагрузки Office 365 налагают ряд ограничений на поддерживаемые топологии.

| Рабочая нагрузка | Ограничения |
| --------- | --------- |
| Exchange Online | Дополнительные сведения о гибридных топологиях, поддерживаемых Exchange Online, см. в разделе [Гибридные развертывания в нескольких лесах Active Directory](https://technet.microsoft.com/library/jj873754.aspx). |
| Skype для бизнеса | При использовании нескольких локальных лесов единственной поддерживаемой топологией является лес ресурсов учетной записи. Дополнительные сведения см. в статье [Требования к среде Skype для бизнеса Server 2015](https://technet.microsoft.com/library/dn933910.aspx). |

Более крупным организациям рекомендуется использовать компонент [Office 365 PreferredDataLocation](how-to-connect-sync-feature-preferreddatalocation.md). Он позволяет определить, в каком регионе центра обработки данных расположены ресурсы пользователя.

## <a name="staging-server"></a>промежуточного сервера
![Промежуточный сервер в топологии](./media/plan-connect-topologies/MultiForestStaging.png)

Azure AD Connect поддерживает установку второго сервера в *промежуточном режиме*. Сервер в этом режиме считывает данные из всех подключенных каталогов, но не записывает ничего в подключенные каталоги. Он использует обычный цикл синхронизации и поэтому содержит обновленную копию данных удостоверения.

При отказе основного сервера можно выполнить отработку отказа на промежуточный сервер. Это можно сделать в мастере Azure AD Connect. Второй сервер может находиться в другом центре обработки данных, так как он не делит инфраструктуру с основным сервером. Любые изменения конфигурации основного сервера следует вручную копировать на второй сервер.

Вы можете использовать промежуточный сервер, чтобы проверить новую пользовательскую конфигурацию и узнать, как она повлияет на данные. Вы можете просмотреть внесенные изменения и выполнить более точную настройку. Когда новая конфигурация будет завершена, вы сможете сделать промежуточный сервер активным и перевести старый сервер в промежуточный режим.

Этот метод также можно использовать для замены активного сервера синхронизации. Подготовьте новый сервер и переведите его в промежуточный режим. Убедитесь, что он работает правильно, и отключите промежуточный режим (сервер станет активным). Завершите работу текущего сервера.

Чтобы иметь несколько резервных копий в разных центрах обработки данных, можно использовать несколько промежуточных серверов.

## <a name="multiple-azure-ad-tenants"></a>Множество клиентов Azure AD
Мы рекомендуем использовать один клиент в Azure AD для всей организации.
Прежде чем использовать несколько клиентов Azure AD, ознакомьтесь со статьей [Управление административными единицами в Azure AD (общедоступной предварительной версии)](../users-groups-roles/directory-administrative-units.md). В ней описываются стандартные сценарии, в которых можно использовать один клиент.

![Топология для нескольких лесов и нескольких клиентов](./media/plan-connect-topologies/MultiForestMultiDirectory.png)

Между сервером синхронизации Azure AD Connect и клиентом Azure AD устанавливается отношение 1:1. Для каждого клиента Azure AD потребуется установить отдельный сервер синхронизации Azure AD Connect. Экземпляры клиента Azure AD являются изолированными по своей природе. Это значит, что пользователям в одном клиенте недоступны пользователи в другом клиенте. Если вам необходимо это разделение, это поддерживаемая конфигурация. В противном случае следует использовать модель одного клиента Azure AD.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Каждый объект, повторяющийся только один раз в клиенте Azure AD
![Отфильтрованная топология для одного леса](./media/plan-connect-topologies/SingleForestFiltered.png)

В этой топологии один сервер синхронизации Azure AD Connect подключается к каждому клиенту Azure AD. На серверах синхронизации Azure AD Connect необходимо настроить правила фильтрации, чтобы они обрабатывали взаимоисключающие наборы объектов. Например, каждый сервер может обслуживать определенный домен или подразделение.

Домен DNS может быть зарегистрирован только в одном клиенте Azure AD. Кроме того, имена участников-пользователей для локальных пользователей Active Directory должны располагаться в отдельных пространствах имен. Например, на предыдущем рисунке в локальном экземпляре Active Directory регистрируется три отдельных суффикса имен участников-пользователей: contoso.com, fabrikam.com и wingtiptoys.com. Пользователи каждого локального домена Active Directory используют различные пространства имен.

>[!NOTE]
>Синхронизация глобального списка адресов (GalSync) не выполняется автоматически в этой топологии. Для нее требуется дополнительная пользовательская реализация MIM, чтобы у каждого клиента был полный глобальный список адресов (GAL) в Exchange Online и Skype для бизнеса Online.


Эта топология налагает следующие ограничения на сценарии, поддерживаемые в других случаях:

* гибридное развертывание Exchange с локальным экземпляром Active Directory можно включить только для одного клиента Azure AD;
* устройство под управлением Windows 10 можно связать только с одним клиентом Azure AD;
* параметр единого входа для синхронизации хэша паролей и сквозной аутентификации можно использовать только с одним клиентом Azure AD.

Требование относительно взаимоисключающих наборов объектов также применяется и к обратной записи. Эта топология не будет поддерживать некоторые функции обратной записи, так как они предполагают наличие одной локальной конфигурации. Эти функции включают в себя следующие:

* групповая обратная запись в конфигурации по умолчанию;
* обратная запись устройств.

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Каждый объект, повторяющийся несколько раз в клиенте Azure AD
![Неподдерживаемая топология для одного леса и нескольких клиентов](./media/plan-connect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Неподдерживаемая топология для одного леса и нескольких соединителей](./media/plan-connect-topologies/SingleForestMultiConnectorsUnsupported.png)

Следующие задачи не поддерживаются:

* синхронизация одного пользователя с несколькими клиентами Azure AD;
* изменение конфигурации таким образом, чтобы пользователи одного клиента Azure AD отображались в другом клиенте Azure AD как контакты;
* изменение настройки службы синхронизации Azure AD Connect для подключения к нескольким клиентам Azure AD.

### <a name="galsync-by-using-writeback"></a>Синхронизация GALSync с помощью обратной записи
![Неподдерживаемая топология для нескольких лесов и нескольких каталогов с синхронизацией GALSync, сфокусированной на Azure AD](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![Неподдерживаемая топология для нескольких лесов и нескольких каталогов с синхронизацией GALSync, сфокусированной на локальном каталоге Active Directory](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Клиенты Azure AD являются изолированными по своей природе. Следующие задачи не поддерживаются:

* изменение настройки службы синхронизации Azure AD Connect для чтения данных из другого клиента Azure AD;
* экспорт пользователей как контакты в другой локальный экземпляр Active Directory с использованием службы синхронизации Azure AD.

### <a name="galsync-with-on-premises-sync-server"></a>Синхронизация GALSync с локальным сервером синхронизации
![Синхронизация GALSync в топологии для нескольких лесов и нескольких каталогов](./media/plan-connect-topologies/MultiForestMultiDirectoryGALSync.png)

Для синхронизации пользователей (с помощью GALSync) между двумя организациями Exchange можно локально использовать FIM 2010 или MIM 2016. Пользователи из одной организации будут отображаться в другой организации как внешние пользователи или контакты. Затем эти локальные экземпляры Active Directory можно синхронизировать с их собственными клиентами Azure AD.

## <a name="next-steps"></a>Дальнейшие действия
Больше об установке Azure AD Connect для этих сценариев можно узнать в статье [Выборочная установка Azure AD Connect](how-to-connect-install-custom.md).

Узнайте больше о настройке [службы синхронизации Azure AD Connect](how-to-connect-sync-whatis.md) .

Узнайте больше об [интеграции локальных удостоверений с Azure Active Directory](whatis-hybrid-identity.md).
