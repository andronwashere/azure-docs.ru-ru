---
title: Проверка подлинности с помощью управляемых удостоверений в Azure Logic Apps | Документация Майкрософт
description: Чтобы выполнить проверку подлинности без входа, создайте управляемое удостоверение, ранее называвшееся "Управляемое удостоверение службы" (MSI), чтобы приложение логики могло получить доступ к ресурсам в других клиентах Azure Active Directory (Azure AD) без учетных данных или секретов.
author: kevinlam1
ms.author: klam
ms.reviewer: estfan, LADocs
services: logic-apps
ms.service: logic-apps
ms.suite: integration
ms.topic: article
ms.date: 03/29/2019
ms.openlocfilehash: b157db5032bd62ab443209f201b4ceded6e44cb5
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68385572"
---
# <a name="authenticate-and-access-resources-with-managed-identities-in-azure-logic-apps"></a>Проверка подлинности и получение доступа к ресурсам с помощью управляемых удостоверений в Azure Logic Apps

Чтобы получить доступ к ресурсам в других клиентах Azure Active Directory (Azure AD) и выполнить аутентификацию идентификатора без входа, приложение логики может использовать [управляемое удостоверение](../active-directory/managed-identities-azure-resources/overview.md), известное ранее как "Управляемое удостоверение службы" (MSI), а не учетные данные или секреты. Azure управляет этим удостоверением за вас и помогает защитить учетные данные, потому что вам не нужно предоставлять или сменять секреты. В этой статье показано, как можно настроить назначенное системой управляемое удостоверение для приложения логики и использовать его. Дополнительные сведения об управляемых удостоверениях см. в статье [Что такое управляемые удостоверения для ресурсов Azure?](../active-directory/managed-identities-azure-resources/overview.md)

> [!NOTE]
> Приложение логики может использовать управляемые удостоверения только с соединителями, поддерживающими управляемые удостоверения. В настоящее время управляемые удостоверения поддерживаются только соединителем HTTP.
>
> В настоящее время в каждой подписке Azure может быть не более 10 рабочих процессов приложения логики с назначенными системой управляемыми удостоверениями.

## <a name="prerequisites"></a>предварительные требования

* Подписка Azure. Если у вас еще ее нет, <a href="https://azure.microsoft.com/free/" target="_blank">зарегистрируйтесь для получения бесплатной учетной записи Azure</a>.

* Приложение логики, в котором необходимо использовать управляемое удостоверение, назначенное системой. Если у вас нет приложения логики, ознакомьтесь с кратким руководством [Создание первого автоматизированного рабочего процесса с помощью Azure Logic Apps на портале Azure](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="enable-identity"></a>

## <a name="enable-managed-identity"></a>Включение управляемого удостоверения

Для назначенных системой управляемых удостоверений этот идентификатор не обязательно создавать вручную. Настроить назначенное системой управляемое удостоверение для приложения логики можно следующими способами: 

* [портал Azure](#azure-portal) 
* [Шаблоны диспетчера ресурсов Azure](#template) 
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/howto-assign-access-powershell.md) 

<a name="azure-portal"></a>

### <a name="azure-portal"></a>портала Azure

Чтобы включить назначенное системой управляемое удостоверение для приложения логики на портале Azure, включите параметр **Назначено системой** в параметрах идентификатора приложения логики.

1. На [портале Azure](https://portal.azure.com) откройте приложение логики в конструкторе приложений логики.

1. В меню приложения логики в разделе **Параметры** выберите **Идентификатор**. 

1. В разделе **Назначено системой** > **Состояние** выберите **Включено**. Затем выберите **Сохранить** > **Да**.

   ![Включение параметра управляемого удостоверения](./media/create-managed-service-identity/turn-on-managed-service-identity.png)

   У приложения логики теперь есть назначенное системой управляемое удостоверение, зарегистрированное в Azure Active Directory:

   ![Глобальные уникальные идентификаторы для идентификатора объекта](./media/create-managed-service-identity/object-id.png)

   | Свойство | Значение | Описание | 
   |----------|-------|-------------| 
   | **Идентификатор объекта** | <*identity-resource-ID*> | Глобальный уникальный идентификатор (GUID), который представляет присвоенное системой управляемое удостоверение приложения логики в клиенте Azure AD | 
   ||| 

<a name="template"></a>

### <a name="azure-resource-manager-template"></a>Шаблон Azure Resource Manager

Если вы хотите автоматизировать создание и развертывание таких ресурсов Azure, как приложения логики, можно настроить [шаблоны Azure Resource Manager](../logic-apps/logic-apps-azure-resource-manager-templates-overview.md). Чтобы создать назначенное системой управляемое удостоверение для приложения логики с помощью шаблона, добавьте элемент `"identity"` и свойство `"type"` в определение рабочего процесса приложения логики в шаблоне развертывания. 

```json
"identity": {
   "type": "SystemAssigned"
}
```

Пример:

```json
{
   "apiVersion": "2016-06-01", 
   "type": "Microsoft.logic/workflows", 
   "name": "[variables('logicappName')]", 
   "location": "[resourceGroup().location]", 
   "identity": { 
      "type": "SystemAssigned" 
   }, 
   "properties": { 
      "definition": { 
         "$schema": "https://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#", 
         "actions": {}, 
         "parameters": {}, 
         "triggers": {}, 
         "contentVersion": "1.0.0.0", 
         "outputs": {} 
   }, 
   "parameters": {}, 
   "dependsOn": [] 
}
```

Когда Azure создает приложение логики, определение рабочего процесса приложения логики содержит такие дополнительные свойства:

```json
"identity": {
   "type": "SystemAssigned",
   "principalId": "<principal-ID>",
   "tenantId": "<Azure-AD-tenant-ID>"
}
```

| Свойство | Значение | Описание | 
|----------|-------|-------------|
| **principalId** | <*ИД субъекта*> | Глобальный уникальный идентификатор (GUID), который представляет приложение логики в клиенте Azure AD и иногда появляется в качестве идентификатора объекта или `objectID` | 
| **tenantId** | <*ИД клиента Azure AD*> | Глобальный уникальный идентификатор (GUID), представляющий клиент Azure AD, участником которого является приложение логики. В клиенте Azure AD субъект-служба имеет то же имя, что и экземпляр приложения логики. | 
||| 

<a name="access-other-resources"></a>

## <a name="access-resources-with-managed-identity"></a>Получение доступа к ресурсам с помощью управляемого удостоверения

После создания назначенного системой управляемого удостоверения для приложения логики [удостоверению можно предоставить доступ к другим ресурсам Azure](../active-directory/managed-identities-azure-resources/howto-assign-access-portal.md). Затем удостоверение можно использовать для аутентификации так же, как и любой другой [субъект-службу](../active-directory/develop/app-objects-and-service-principals.md). 

> [!NOTE]
> И назначенное системой управляемое удостоверение, и ресурс, к которому необходимо назначить доступ, должны находиться в одной подписке Azure.

### <a name="assign-access-to-managed-identity"></a>Назначение доступа к управляемому удостоверению

Чтобы предоставить доступ к другому ресурсу Azure для назначенного системой управляемого удостоверения приложения логики, выполните следующие действия:

1. На портале Azure перейдите к ресурсу Azure, к которому вы хотите назначить доступ для управляемого удостоверения. 

1. В меню ресурса выберите **Управление доступом (IAM)** . На панели инструментов нажмите кнопку **Добавить** > **добавить назначение ролей**.

   ![Добавить назначение ролей](./media/create-managed-service-identity/add-permissions-logic-app.png)

1. В меню **Добавить назначение ролей** выберите **роль**, которая требуется для удостоверения. 

1. В свойстве **Назначение доступа** выберите значение **Пользователь, группа или субъект-служба Azure AD**, если оно еще не выбрано.

1. В поле **Выбор** начните вводить имя приложения логики. Когда появится приложение логики, выберите его.

   ![Выбор приложения логики с управляемым удостоверением](./media/create-managed-service-identity/add-permissions-select-logic-app.png)

1. Когда все будет готово, нажмите **Сохранить**.

### <a name="authenticate-with-managed-identity-in-logic-app"></a>Проверка подлинности на основе управляемых удостоверений в приложении логики

После настройки приложения логики с назначенным системой управляемым удостоверением, которому предоставлен доступ к необходимому ресурсу, удостоверение можно использовать для проверки подлинности. Например, теперь можно использовать действие HTTP для того, чтобы приложение логики могло отправить HTTP-запрос или вызов к этому ресурсу. 

1. Добавьте действие **HTTP** в приложение логики.

1. Предоставьте необходимые сведения для этого действия, такие как **метод** запроса и расположение **URI** ресурса, который необходимо вызвать.

   Например, предположим, что вы используете аутентификацию Azure Active Directory (Azure AD) с [одной из этих служб Azure, поддерживающих Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). 
   В поле **URI** введите URL-адрес конечной точки для соответствующей службы Azure. 
   Поэтому, если вы используете Azure Resource Manager, введите это значение в свойство **URI**:

   `https://management.azure.com/subscriptions/<Azure-subscription-ID>?api-version=2016-06-01`

1. В действии HTTP выберите **Показать расширенные параметры**.

1. В списке **Проверка подлинности** выберите **Управляемое удостоверение**. После выбора этой аутентификации свойство **Аудитория** отображается со значением по умолчанию для идентификатора ресурса:

   ![Выбор пункта "Управляемое удостоверение"](./media/create-managed-service-identity/select-managed-service-identity.png)

   > [!IMPORTANT]
   > 
   > В свойстве **Аудитория** значение идентификатора ресурса должно точно совпадать со значением, ожидаемым Azure AD, включая все необходимые знаки косой черты. 
   > Эти значения идентификатор ресурса можно найти в этой [таблице, описывающей службы Azure, поддерживающие Azure AD](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-azure-ad-authentication). 
   > Например, если вы используете идентификатор ресурса Azure Resoruce Manager, убедитесь, что в URI есть знак косой черты.

1. Продолжите создание приложения логики нужным образом.

<a name="remove-identity"></a>

## <a name="remove-managed-identity"></a>Удаление управляемого удостоверения

Чтобы отключить назначенное системой управляемое удостоверение приложения логики, выполните шаги как при создании идентификатора с помощью портала Azure, Azure PowerShell или шаблонов развертывания Azure Resource Manager.

При удалении приложения логики Azure автоматически удаляет присвоенный системой идентификатор приложения логики из Azure AD.

### <a name="azure-portal"></a>портала Azure

Чтобы удалить назначенное системой управляемое удостоверение для приложения логики на портале Azure, включите параметр **Назначено системой** в параметрах идентификатора приложения логики.

1. На [портале Azure](https://portal.azure.com) откройте приложение логики в конструкторе приложений логики.

1. В меню приложения логики в разделе **Параметры** выберите **Идентификатор**. 

1. В разделе **Назначено системой** > **Состояние** выберите **Выключено**. Затем выберите **Сохранить** > **Да**.

   ![Отключение параметра управляемого удостоверения](./media/create-managed-service-identity/turn-off-managed-service-identity.png)

### <a name="deployment-template"></a>Шаблон развертывания

Если вы создали назначенное системой управляемое удостоверение приложения логики с помощью шаблона развертывания Azure Resource Manager, задайте свойству `"type"` элемента `"identity"` значение `"None"`. Это действие также удаляет идентификатор участника из Azure AD.

```json
"identity": {
   "type": "None"
}
```

## <a name="get-support"></a>Поддержка

* Если у вас возникли вопросы, то посетите [форум Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Отправить идею по поводу возможности или проголосовать за нее вы можете на [сайте отзывов пользователей Logic Apps](https://aka.ms/logicapps-wish).
