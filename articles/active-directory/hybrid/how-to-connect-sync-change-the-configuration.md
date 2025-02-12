---
title: 'Синхронизация Azure AD Connect: изменение конфигурации | Документация Майкрософт'
description: Узнайте, как изменить конфигурацию службы синхронизации Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 31fe3877fd6098b18686b9d99a012cbfbef7c300
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60244049"
---
# <a name="azure-ad-connect-sync-make-a-change-to-the-default-configuration"></a>Синхронизация Azure AD Connect: изменение конфигурации по умолчанию
В этой статье предоставлены сведения об изменении конфигурации по умолчанию в службах синхронизации Azure Active Directory (Azure AD) Connect. Здесь также описываются действия для некоторых стандартных сценариев. Ознакомившись с этими сведениями, вы сможете вносить изменения в конфигурацию в соответствии с собственными бизнес-правилами.

> [!WARNING]
> Изменения, внесенные в правила синхронизации по умолчанию, будут перезаписаны при следующем обновлении Azure AD Connect. Это приведет к непредвиденным и, вероятно, нежелательным результатам синхронизации.
>
> Стандартные правила синхронизации имеют отпечаток. Если внести в правила изменения, отпечаток также изменится. Тогда при следующей попытке установить новый выпуск Azure AD Connect могут возникнуть проблемы. Выполняйте изменения только так, как описано в этой статье.

## <a name="synchronization-rules-editor"></a>Редактор правил синхронизации
Редактор правил синхронизации используется для просмотра и изменения конфигурации по умолчанию. Его можно найти в меню **Пуск** в группе **Azure AD Connect**.  
![Редактор правил синхронизации в меню "Пуск"](./media/how-to-connect-sync-change-the-configuration/startmenu2.png)

Открыв редактор, вы увидите в нем стандартные правила по умолчанию.

![Редактор правил синхронизации](./media/how-to-connect-sync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Перемещение в редакторе
С помощью раскрывающихся списков, расположенных в верхней части редактора, можно быстро найти определенное правило. Например, чтобы просмотреть правила, содержащие атрибут proxyAddresses, необходимо выбрать в раскрывающихся списках следующее:  
![Фильтрация в редакторе правил синхронизации](./media/how-to-connect-sync-change-the-configuration/filtering.png)  
Чтобы сбросить параметры фильтрации и загрузить новую конфигурацию, нажмите клавишу F5.

В правом верхнем углу расположена кнопка **Добавить новое правило**. С ее помощью можно создать собственное настраиваемое правило.

Внизу находятся кнопки, позволяющие выполнять определенные действия с выбранным правилом синхронизации. Кнопки **Изменить** и **Удалить** используются для удаления и изменения правил. Кнопка **Экспорт** позволяет создать сценарий PowerShell для повторного создания правила синхронизации. При этом правило синхронизации перемещается с одного сервера на другой.

## <a name="create-your-first-custom-rule"></a>Создание настраиваемого правила
Чаще всего изменяются потоки атрибутов. Данные в исходном каталоге могут отображаться не так, как в Azure AD. В примере, рассматриваемом в этом разделе, необходимо убедиться, что имя пользователя всегда задано в *правильном регистре*.

### <a name="disable-the-scheduler"></a>Отключение планировщика
По умолчанию [планировщик](how-to-connect-sync-feature-scheduler.md) запускается каждые 30 минут. Чтобы внести изменения и устранить неполадки, связанные с новыми правилами, может потребоваться отключить его. Чтобы временно отключить планировщик, запустите PowerShell и выполните команду `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![Отключение планировщика](./media/how-to-connect-sync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>Создание правила
1. Щелкните **Добавить новое правило**.
2. На странице **Описание** заполните следующие поля.  
   ![Фильтрация входящего правила синхронизации](./media/how-to-connect-sync-change-the-configuration/description2.png)  
   * **Имя**. Введите описательное имя правила.
   * **Описание** Информация, позволяющая другим пользователям определить предназначение правила.
   * **Connected System** (Подключенная система). Система, в которой находится объект. В этом случае выберите **соединитель Active Directory**.
   * **Connected System/Metaverse Object Type** (Подключенная система или тип объекта метавселенной). Выберите **User** и **Person** соответственно.
   * **Тип связи**. Измените значение этого параметра на **Присоединить**.
   * **Приоритет**. Укажите уникальное в системе значение. Более низкие значения имеют более высокий приоритет.
   * **Тег**. Оставьте это поле пустым. Его нужно заполнять только при использовании стандартных правил Майкрософт.
3. На странице **Scoping filter** (Фильтр области действия) введите **givenName ISNOTNULL**.  
   ![Фильтр области входящего правила](./media/how-to-connect-sync-change-the-configuration/scopingfilter.png)  
   Этот раздел позволит определить, к каким объектам должно применяться правило. Если оставить эти поля пустыми, правило будет применяться ко всем объектам пользователя, в том числе к конференц-залам, учетным записям службы и другим неодушевленным объектам.
4. На странице **Join rules** (Правила присоединения) оставьте поле пустым.
5. На странице **Преобразования** для параметра **FlowType** (Тип потока) задайте значение **Выражение**. Для параметра **Целевой атрибут** выберите **givenName**. Для параметра **Источник** введите **PCase([givenName])** .
   ![Преобразования входящего правила](./media/how-to-connect-sync-change-the-configuration/transformations.png)  
   Для модуля синхронизации имя функции и имя атрибута необходимо указывать с учетом регистра. Если не учесть регистр, при добавлении правила отобразится предупреждение. Вы можете сохранить и продолжить работу, но вам понадобится вернуться и исправить правило.
6. Щелкните **Добавить** , чтобы сохранить правило.

Новое настраиваемое правило отобразится в системе вместе с другими правилами синхронизации.

### <a name="verify-the-change"></a>Проверка изменений
После внесения изменений в правило его можно проверить на наличие ошибок. В зависимости от количества объектов это можно сделать двумя разными способами:

- Выполнить полную синхронизацию всех объектов.
- Выполнить предварительный просмотр и полную синхронизацию одного объекта.

Из меню **Пуск** запустите **службу синхронизации**. Все действия, описанные в этом разделе, выполняются в этом средстве.

**Полная синхронизация всех объектов**  

   1. Выберите **Соединители** вверху. Определите соединитель, измененный в предыдущем разделе (в этом примере "Доменные службы Active Directory"), и выберите его. 
   2. В разделе **Действия** выберите **Run** (Запустить).
   3. Выберите **Full Synchronization** (Полная синхронизация) и нажмите кнопку **ОК**.
   ![Полная синхронизация](./media/how-to-connect-sync-change-the-configuration/fullsync.png)  
   Теперь объекты в метавселенной обновлены, и это можно проверить, просмотрев отдельный объект.

**Предварительный просмотр и полная синхронизация одного объекта**  

   1. Выберите **Соединители** вверху. Определите соединитель, измененный в предыдущем разделе (в этом примере "Доменные службы Active Directory"), и выберите его.
   2. Выберите **Search Connector Space**(Поиск пространства соединителя). 
   3. Используйте **область поиска**, чтобы найти необходимый объект для проверки изменений. Выберите объект и щелкните **Предварительный просмотр**. 
   4. В открывшемся окне выберите **Просмотр перед фиксацией**.  
   ![Просмотр перед фиксацией](./media/how-to-connect-sync-change-the-configuration/commitpreview.png)  
   Теперь изменение зафиксировано в метавселенной.

**Просмотр объектов в метавселенной**  

1. Выберите несколько образцов объектов, чтобы убедиться, что для них задано правильное значение и применено необходимое правило. 
2. В верхней части экрана выберите **Metaverse Search** (Поиск в метавселенной). Добавьте любой фильтр, чтобы найти соответствующие объекты. 
3. Откройте объект, выбрав его в результатах поиска. Просмотрите значения атрибута, а также проверьте применение правила в столбце **Правила синхронизации**.  
![Поиск в метавселенной](./media/how-to-connect-sync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>Включение планировщика
Если все параметры заданы правильно, планировщик можно включить снова. В PowerShell выполните команду `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Другие основные изменения потоков атрибутов
В предыдущем разделе описан процесс изменения потока атрибута. В этом разделе приведены дополнительные примеры. Действия по созданию правила синхронизации упомянуты лишь в общих чертах. Полное описание можно просмотреть в предыдущем разделе.

### <a name="use-an-attribute-other-than-the-default"></a>Использование атрибута не по умолчанию
В этом сценарии компании Fabrikam есть лес, в котором для указания имени, фамилии и отображаемого имени используются локальные алфавиты. Представления этих атрибутов хранятся в атрибутах расширения, отображенные символами латиницы. При формировании глобального списка адресов в Azure AD и Office 365 организации требуется использовать эти атрибуты.

В конфигурации по умолчанию объект из локального леса выглядит следующим образом:  
![Поток атрибутов 1](./media/how-to-connect-sync-change-the-configuration/attributeflowjp1.png)

Чтобы создать правило с другими потоками атрибутов, выполните следующие действия.

1. В меню **Пуск** запустите **редактор правил синхронизации**.
2. Не снимая флажок **Входящие** слева, нажмите кнопку **Добавить новое правило**.
3. Присвойте правилу имя и описание. Выберите экземпляр локального каталога Active Directory и соответствующие типы объектов. В поле **Тип связи** выберите **Join** (Присоединение). В поле **Приоритет** выберите число, которое не используется другим правилом. Приоритет стандартных правил начинается со 100, поэтому в этом примере используется значение 50.
  ![Поток атрибутов 2](./media/how-to-connect-sync-change-the-configuration/attributeflowjp2.png)
4. Оставьте поле **Scoping filter** (Фильтр области) пустым. (Это означает, что правило должно применяться для всех объектов-пользователей в лесу.)
5. Оставьте поле **Join rules** (Правила присоединения) пустым. (Это означает, что стандартные правила могут обрабатывать любые присоединения.)
6. В разделе **Преобразования** создайте следующие потоки.  
  ![Поток атрибутов 3](./media/how-to-connect-sync-change-the-configuration/attributeflowjp3.png)
7. Щелкните **Добавить** , чтобы сохранить правило.
8. Откройте **Synchronization Service Manager**. В разделе **Соединители** выберите соединитель, в который вы добавили правило. Выберите **Run** (Запустить), а затем — **Full Synchronization** (Полная синхронизация). Функция полной синхронизации повторно вычисляет все объекты, используя текущие правила.

Это результат для того же объекта с применением следующего настраиваемого правила.  
![Поток атрибутов 4](./media/how-to-connect-sync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Длина атрибутов
Строковые атрибуты по умолчанию являются индексируемыми. Их длина не может превышать 448 знаков. При работе с атрибутами, которые могут содержать больше символов, необходимо включить в поток атрибутов следующее:  
`attributeName` <- `Left([attributeName],448)`.

### <a name="changing-the-userprincipalsuffix"></a>Изменение атрибута userPrincipalSuffix
Атрибут userPrincipalName в Active Directory не всегда известен пользователям. Также он может не подходить в качестве идентификатора для входа. Мастер установки службы синхронизации Azure AD Connect позволяет выбрать другой атрибут, например *mail*. Но в некоторых случаях этот атрибут должен вычисляться.

Например, у компании Contoso есть два каталога Azure AD: один для рабочей среды, а другой для тестовой. Пользователи в тестовом клиенте должны просто изменить суффикс в идентификаторе входа.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`.

В этом выражении берется вся часть слева от первого знака @-sign (Word) и сцепляется с фиксированной строкой.

### <a name="convert-a-multi-value-attribute-to-single-value"></a>Преобразование атрибута с несколькими значениями в атрибут с одним
Некоторые атрибуты в Active Directory имеют несколько значений в схеме, даже если в средстве администрирования "Пользователи и компьютеры Active Directory" они выглядят как атрибуты с одним значением. Пример — атрибут description.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`.

В этом выражении в случае, если атрибут имеет значение, берется первый элемент (*Item*) в атрибуте, удаляются начальные и конечные пробелы (*Trim*), а затем сохраняются первые 448 знаков (*Left*) в строке.

### <a name="do-not-flow-an-attribute"></a>Отключение потока для атрибута
Общие сведения о сценарии в этом разделе см. в разделе [Управление потоком атрибута](concept-azure-ad-connect-sync-declarative-provisioning.md#control-the-attribute-flow-process).

Существует два способа отключения потока для атрибута. Первый — [удалить выбранные атрибуты](how-to-connect-install-custom.md#azure-ad-app-and-attribute-filtering) в мастере установки. Им можно воспользоваться, если атрибут никогда не синхронизировался. Однако если вы начали синхронизацию этого атрибута и позже удалили его с помощью этой функции, то модуль синхронизации прекратит управление атрибутом и в Azure AD будут оставлены имеющиеся значения.

Если вы хотите удалить значение атрибута и гарантировать, что в будущем для него не будет включен поток, нужно создать настраиваемое правило.

В этом сценарии компании Fabrikam мы обнаружили, что некоторые атрибуты, синхронизируемые с облаком, лишние. Мы хотим удалить эти атрибуты из Azure AD.  
![Неправильные атрибуты расширения](./media/how-to-connect-sync-change-the-configuration/badextensionattribute.png)

1. Создайте правило входящей синхронизации и укажите описание.
  ![Описания](./media/how-to-connect-sync-change-the-configuration/syncruledescription.png)
2. Создайте потоки атрибутов, выбрав для параметра **FlowType** (Тип потока) значение **Выражение**, а для параметра **Источник** — **AuthoritativeNull**. Литеральный тип **AuthoritativeNull** указывает, что значение должно быть пустым в метавселенной, даже если правило синхронизации с более низким приоритетом пытается заполнить значение.
  ![Преобразование атрибутов расширения](./media/how-to-connect-sync-change-the-configuration/syncruletransformations.png)
3. Сохраните правило синхронизации. Запустите **службу синхронизации**, найдите соединитель, выберите **Run** (Запустить), а затем — **Full Synchronization** (Полная синхронизация). Это приведет к пересчету всех потоков атрибутов.
4. Убедитесь, что требуемые изменения будут экспортированы, выполнив поиск в пространстве соединителя.
  ![Промежуточное удаление](./media/how-to-connect-sync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>Создание правил с помощью PowerShell
Редактор правил синхронизации удобно использовать, если требуется внести всего несколько изменений. Если требуется внести много изменений, то лучше использовать PowerShell. Некоторые дополнительные функции доступны только в PowerShell.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>Получение сценария PowerShell для стандартного правила
Чтобы получить сценарий PowerShell для создания стандартного правила, выберите правило в редакторе правил синхронизации и щелкните **Экспорт**. Это позволит получить сценарий PowerShell, который создал данное правило.

### <a name="advanced-precedence"></a>Расширенный приоритет
Значение приоритета стандартного правила синхронизации начинается со 100. Если имеется несколько лесов и нужно внести множество пользовательских изменений, то 99 правил синхронизации может быть недостаточно.

Можно указать модулю синхронизации, что вам нужно вставить дополнительные правила перед стандартными правилами. Чтобы сделать это, выполните следующее.

1. Выберите первое стандартное правило синхронизации (**In from AD-User Join**) в редакторе правил синхронизации и щелкните **Экспорт**. Скопируйте значение идентификатора SR.  
![PowerShell до изменения](./media/how-to-connect-sync-change-the-configuration/powershell1.png)  
2. Создайте новое правило синхронизации. Для этого можно использовать редактор правил синхронизации. Экспортируйте это правило в сценарий PowerShell.
3. В свойство **PrecedenceBefore** вставьте значение идентификатора из стандартного правила. Задайте для параметра **Precedence** значение **0**. Убедитесь, что атрибут Identifier является уникальным и вы не используете повторно GUID из другого правила. Убедитесь также, что свойство **ImmutableTag** не задано. Это свойство должно быть задано только для стандартных правил.
4. Сохраните сценарий PowerShell и запустите его. Результатом будет назначение приоритета со значением 100 вашему настраиваемому правилу, при этом приоритет всех прочих стандартных правил будет увеличен.  
![PowerShell после изменения](./media/how-to-connect-sync-change-the-configuration/powershell2.png)  

При необходимости можно использовать несколько настраиваемых правил синхронизации с одинаковым значением **PrecedenceBefore**.

## <a name="enable-synchronization-of-usertype"></a>Включение синхронизации атрибута UserType
Azure AD Connect поддерживает синхронизацию атрибута **UserType** для объектов **User** в версии 1.1.524.0 и более поздних. В частности, были введены следующие изменения:

- Схема типа объекта **User** в соединителе Azure AD расширена для включения атрибута UserType, который имеет строковый тип и является однозначным.
- Схема типа объекта **Person** в метавселенной расширена для включения атрибута UserType, который имеет строковый тип и является однозначным.

По умолчанию синхронизация атрибута UserType не включена, так как в локальном каталоге Active Directory отсутствует соответствующий атрибут UserType. Необходимо вручную включить синхронизацию. Перед этим следует принять во внимание приведенные ниже особенности, обусловленные использованием Azure AD.

- Azure AD принимает только два значения атрибута UserType: **Member** и **Guest**.
- Если атрибут UserType не участвует в синхронизации в Azure AD Connect, то для пользователей Azure AD, созданных с помощью синхронизации службы каталогов, для атрибута UserType будет задано значение **Member**.
- Azure AD не разрешает службе Azure AD Connect изменять атрибут UserType для существующих пользователей Azure AD. Его можно задать только во время создания пользователей Azure AD.

Перед включением синхронизации атрибута UserType необходимо решить, каким образом атрибут будет получен из локальной службы Active Directory. Ниже перечислены наиболее распространенные подходы.

- Назначьте неиспользуемый локальный атрибут AD (например, extensionAttribute1) в качестве исходного атрибута. Указанный локальный атрибут AD должен быть типа **строка**, являться однозначным и содержать значение **Member** или **Guest**. 

    Если выбран этот подход, то перед включением синхронизации атрибута UserType необходимо убедиться, что указанный атрибут заполняется правильным значением для всех существующих объектов-пользователей в локальной службе Active Directory, синхронизируемых с Azure AD.

- Кроме того, можно получить значение атрибута UserType из других свойств. Например, требуется синхронизировать всех пользователей в качестве **гостевых**, если их локальный атрибут AD userPrincipalName заканчивается доменной частью "<em>@partners.fabrikam123.org</em>". 

    Как было сказано ранее, Azure AD не разрешает службе Azure AD Connect изменять атрибут UserType для существующих пользователей Azure AD. Следовательно, нужно убедиться, что выбранная логика согласована с текущей конфигурацией атрибута UserType для всех существующих пользователей Azure AD в клиенте.

Включение синхронизации атрибута UserType можно вкратце описать следующим образом.

1.  Отключите планировщик синхронизации и убедитесь, что синхронизация не выполняется.
2.  Добавьте исходный атрибут в схему локального соединителя AD.
3.  Добавьте UserType в схему соединителя Azure AD.
4.  Создайте правило входящей синхронизации для передачи значения атрибута из локального каталога Active Directory.
5.  Создайте правило исходящей синхронизации для передачи значения атрибута в Azure AD.
6.  Запустите цикл полной синхронизации.
7.  Включите планировщик синхронизации.

>[!NOTE]
> Эти шаги описываются в остальной части данного раздела. Они описаны в контексте развертывания Azure AD в топологии с отдельным лесом и без пользовательских правил синхронизации. Если используется топология с несколькими лесами, настроенными пользовательскими правилами синхронизации или промежуточным сервером, то необходимо соответствующим образом изменить действия.

### <a name="step-1-disable-the-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Шаг 1. Отключите планировщик синхронизации и убедитесь, что синхронизация не выполняется.
Во избежание экспорта непреднамеренных изменений в Azure AD, изменяя правила синхронизации, убедитесь, что синхронизация не выполняется. Чтобы отключить встроенный планировщик синхронизации, сделайте следующее:

 1. Запустите сеанс PowerShell на сервере Azure AD Connect.
 2. Отключите плановую синхронизацию, выполнив командлет `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 3. Запустите Synchronization Service Manager, выбрав **Пуск** > **Служба синхронизации**.
 4. Перейдите на вкладку **Операции** и убедитесь, что на ней не отображаются операции в состоянии *Выполняется*.

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>Шаг 2. Добавьте исходный атрибут в схему локального соединителя AD.
Не все атрибуты Azure AD импортируются в пространство локального соединителя AD. Чтобы добавить исходный атрибут в список импортируемых атрибутов, сделайте следующее.

 1. Откройте вкладку **Connectors** (Соединители) в Synchronization Service Manager.
 2. Щелкните правой кнопкой мыши локальный соединитель AD и выберите **Свойства**.
 3. Во всплывающем диалоговом окне перейдите на вкладку **Выбор атрибутов**.
 4. Убедитесь, что исходный атрибут выбран в списке атрибутов.
 5. Нажмите кнопку **OK**, чтобы сохранить изменения.
![Добавление исходного атрибута в схему локального соединителя AD](./media/how-to-connect-sync-change-the-configuration/usertype1.png)

### <a name="step-3-add-the-usertype-to-the-azure-ad-connector-schema"></a>Шаг 3. Добавьте UserType в схему соединителя Azure AD.
По умолчанию атрибут UserType не импортируется в пространство Azure AD Connect. Чтобы добавить атрибут UserType в список импортируемых атрибутов, сделайте следующее.

 1. Откройте вкладку **Connectors** (Соединители) в Synchronization Service Manager.
 2. Щелкните правой кнопкой мыши **соединитель Azure AD** и выберите **Properties** (Свойства).
 3. Во всплывающем диалоговом окне перейдите на вкладку **Выбор атрибутов**.
 4. Убедитесь, что атрибут UserType выбран в списке атрибутов.
 5. Нажмите кнопку **OK**, чтобы сохранить изменения.

![Добавление исходного атрибута в схему соединителя Azure AD](./media/how-to-connect-sync-change-the-configuration/usertype2.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Шаг 4. Создайте правило входящей синхронизации для передачи значения атрибута из локального каталога Active Directory.
Правило входящей синхронизации позволяет передавать значение исходного атрибута из локального каталога Active Directory в метавселенную.

1. Запустите редактор правил синхронизации, выбрав **Пуск** > **Редактор правил синхронизации**.
2. В фильтре поиска для параметра **Направление** задайте значение **Входящие**.
3. Нажмите кнопку **Добавить новое правило**, чтобы создать правило входящей синхронизации.
4. На вкладке **Описание** укажите следующую конфигурацию.

    | Атрибут | Значение | Сведения |
    | --- | --- | --- |
    | Name | *Укажите имя* | Например, *In from AD – User UserType*. |
    | Описание | *Укажите описание* |  |
    | Подключенная система | *Выберите локальный соединитель AD*. |  |
    | Тип объекта подключенной системы | **User** |  |
    | Тип объекта метавселенной | **Person** |  |
    | Тип связи | **Join** |  |
    | Приоритет | *Выберите число от 1 до 99* | Значения 1–99 зарезервированы для настраиваемых правил синхронизации. Не выбирайте значение, которое используется в другом правиле синхронизации. |

5. Перейдите на вкладку **Scoping filter** (Фильтр области) и добавьте **отдельную группу фильтра области** со следующим предложением.

    | Атрибут | Оператор | Значение |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Пользователь\_ |

    Фильтр области определяет локальные объекты AD, к которым применяется данное правило входящей синхронизации. В этом примере мы используем тот же фильтр области, что и в стандартном правиле синхронизации *In from AD – User Common*, который запрещает применять правило синхронизации к объектам-пользователям, созданным с помощью функции обратной записи пользователей Azure AD. Возможно, вам необходимо настроить фильтр области в соответствии с развертыванием Azure AD Connect.

6. Перейдите на вкладку **Преобразования** и реализуйте нужное правило преобразования. Предположим, вы назначили неиспользуемый локальный атрибут AD (например, extensionAttribute1) в качестве исходного атрибута для UserType. Вы можете реализовать непосредственный поток атрибутов.

    | Flow type (Тип потока) | Целевой атрибут | source | Применить однократно | Merge Type (Тип объединения) |
    | --- | --- | --- | --- | --- |
    | Напрямую | UserType | extensionAttribute1 | Флажок снят. | Блокировка изменений |

    Еще один пример: требуется получить значение атрибута UserType из других свойств. Например, требуется синхронизировать всех пользователей в качестве гостевых, если их локальный атрибут AD UserPrincipalName заканчивается доменной частью "<em>@partners.fabrikam123.org</em>". Можно реализовать следующее выражение.

    | Flow type (Тип потока) | Целевой атрибут | source | Применить однократно | Merge Type (Тип объединения) |
    | --- | --- | --- | --- | --- |
    | Expression | UserType | IIF(IsPresent([userPrincipalName]),IIF(CBool(InStr(LCase([userPrincipalName]),"@partners.fabrikam123.org")=0),"Member","Guest"),Error("UserPrincipalName is not present to determine UserType")) | Флажок снят. | Блокировка изменений |

7. Щелкните **Добавить**, чтобы создать правило входящей синхронизации.

![Создание правила входящей синхронизации](./media/how-to-connect-sync-change-the-configuration/usertype3.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Шаг 5. Создайте правило исходящей синхронизации для передачи значения атрибута в Azure AD.
Правило исходящей синхронизации позволяет передать значение атрибута из метавселенной в атрибут UserType в Azure AD.

1. Перейдите в редактор правил синхронизации.
2. В фильтре поиска для параметра **Направление** задайте значение **Исходящие**.
3. Нажмите кнопку **Добавить новое правило**.
4. На вкладке **Описание** укажите следующую конфигурацию.

    | Атрибут | Значение | Сведения |
    | ----- | ------ | --- |
    | Name | *Укажите имя* | Например, *Out to AAD – User UserType*. |
    | Описание | *Укажите описание* ||
    | Подключенная система | *Выберите соединитель AAD* ||
    | Тип объекта подключенной системы | **User** ||
    | Тип объекта метавселенной | **Person** ||
    | Тип связи | **Join** ||
    | Приоритет | *Выберите число от 1 до 99* | Значения 1–99 зарезервированы для настраиваемых правил синхронизации. Не выбирайте значение, которое используется в другом правиле синхронизации. |

5. Перейдите на вкладку **Scoping filter** (Фильтр области) и добавьте **отдельную группу фильтра области** с двумя приведенными ниже предложениями.

    | Атрибут | Оператор | Значение |
    | --- | --- | --- |
    | sourceObjectType | EQUAL | Пользователь |
    | cloudMastered | NOTEQUAL | Истина |

    Фильтр области определяет объекты Azure AD, к которым применяется данное правило исходящей синхронизации. В этом примере мы используем фильтр области из стандартного правила синхронизации *Out to AD – User Identity*. Он предотвращает применение правила синхронизации к объектам User в локальном каталоге Active Directory, которые не синхронизируются. Возможно, вам необходимо настроить фильтр области в соответствии с развертыванием Azure AD Connect.

6. Перейдите на вкладку **Преобразование** и реализуйте приведенное ниже правило преобразования.

    | Flow type (Тип потока) | Целевой атрибут | source | Применить однократно | Merge Type (Тип объединения) |
    | --- | --- | --- | --- | --- |
    | Напрямую | UserType | UserType | Флажок снят. | Блокировка изменений |

7. Щелкните **Добавить**, чтобы создать правило исходящей синхронизации.

![Создание правила исходящей синхронизации](./media/how-to-connect-sync-change-the-configuration/usertype4.png)

### <a name="step-6-run-a-full-synchronization-cycle"></a>Шаг 6. Запустите цикл полной синхронизации.
Как правило, полный цикл синхронизации обязателен, так как мы добавили новые атрибуты в схемы Active Directory и соединителя Azure AD, а также ввели настраиваемые правила синхронизации. Рекомендуется проверить изменения перед их экспортом в Azure AD. 

Можно следовать приведенным ниже инструкциям, чтобы проверить изменения во время ручного выполнения действий, составляющих полный цикл синхронизации.

1. Выполните **полный импорт** для **локального соединителя AD**.

   1. Откройте вкладку **Operations** (Операции) в Synchronization Service Manager.
   2. Щелкните правой кнопкой мыши **локальный соединитель AD** и выберите **Run** (Запустить).
   3. Во всплывающем диалоговом окне выберите **Full Import** (Полный импорт), а затем нажмите кнопку **ОК**.
   4. Дождитесь завершения операции.

      > [!NOTE]
      > Можно пропустить полный импорт локального соединителя AD, если исходный атрибут уже включен в список импортируемых атрибутов. Другими словами, можно было не вносить изменения во время выполнения шага 2 [ Добавление исходного атрибута в схему локального соединителя AD](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Выполните **полный импорт** для **соединителя Azure AD**.

   1. Щелкните правой кнопкой мыши **соединитель Azure AD** и выберите **Run** (Запустить).
   2. Во всплывающем диалоговом окне выберите **Full Import** (Полный импорт), а затем нажмите кнопку **ОК**.
   3. Дождитесь завершения операции.

3. Проверьте изменения правила синхронизации на существующем объекте User.

    Исходный атрибут из локального каталога Active Directory и атрибут UserType из Azure AD были импортированы в соответствующие пространства соединителя. Перед выполнением шага полной синхронизации выполните **предварительный просмотр** существующего объекта User в пространстве локального соединителя AD. У выбранного объекта должен быть заполнен исходный атрибут.
    
    Если при **предварительном просмотре** атрибут UserType успешно заполнен, то это хороший показатель того, что вы верно настроили правила синхронизации. Сведения о том, как выполнить **предварительный просмотр**, см. в разделе [Проверка изменений](#verify-the-change).

4. Выполните **полную синхронизацию** для **локального соединителя AD**.

   1. Щелкните правой кнопкой мыши **локальный соединитель AD** и выберите **Run** (Запустить).
   2. Во всплывающем диалоговом окне выберите **Full Synchronization** (Полная синхронизация), а затем нажмите кнопку **ОК**.
   3. Дождитесь завершения операции.

5. Проверьте **ожидающие операции экспорта** в Azure AD.

   1. Щелкните правой кнопкой мыши соединитель **Azure AD** и выберите **Search Connector Space** (Поиск пространства соединителя).
   2. Во всплывающем диалоговом окне **Search Connector Space** (Поиск пространства соединителя) сделайте следующее.

      - Для параметра **Scope** (Область) укажите значение **Pending Export** (Ожидающая операция экспорта).
      - Установите все три флажка: **Добавить**, **Изменить** и **Удалить**.
      - Нажмите кнопку **Найти**, чтобы получить список объектов с изменениями для экспорта. Чтобы проверить изменения указанного объекта, дважды щелкните его.
      - Убедитесь, что изменения являются ожидаемыми.

6. Выполните **экспорт** в **соединителе Azure AD**.

   1. Щелкните правой кнопкой мыши **соединитель Azure AD** и выберите **Run** (Запустить).
   2. Во всплывающем диалоговом окне **Run Connector** (Запуск соединителя) выберите **Экспорт**, а затем нажмите кнопку **ОК**.
   3. Дождитесь завершения экспорта в Azure AD.

> [!NOTE]
> Для соединителя Azure AD отсутствуют шаги полной синхронизации и экспорта. Они и не требуются, так как значения атрибутов передаются только из локального каталога Active Directory в Azure AD.

### <a name="step-7-re-enable-the-sync-scheduler"></a>Шаг 7. Повторно включите планировщик синхронизации.
Повторно включите встроенный планировщик синхронизации.

1. Запустите сеанс PowerShell.
2. Повторно включите плановую синхронизацию, выполнив командлет `Set-ADSyncScheduler -SyncCycleEnabled $true`.


## <a name="next-steps"></a>Дальнейшие действия
* Дополнительные сведения о модели конфигурации, см. в статье о [принципах декларативной подготовки](concept-azure-ad-connect-sync-declarative-provisioning.md).
* Дополнительные сведения о языке выражений см. в статье [Служба синхронизации Azure AD Connect: общие сведения о выражениях декларативной подготовки](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md).

**Обзорные статьи**

* [Синхронизация Azure AD Connect: общие сведений о синхронизации и ее настройка](how-to-connect-sync-whatis.md)
* [Интеграция локальных удостоверений с Azure Active Directory](whatis-hybrid-identity.md)
