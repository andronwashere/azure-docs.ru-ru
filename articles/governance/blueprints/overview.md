---
title: Обзор Azure Blueprints
description: Описание службы Azure Blueprints, с помощью которой можно создавать, определять и развертывать артефакты в среде Azure.
author: DCtheGeek
ms.author: dacoulte
ms.date: 02/08/2019
ms.topic: overview
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 5f403ee7d2bf4a0730ceaa9b5a95b3e7b11f3695
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67428773"
---
# <a name="overview-of-the-azure-blueprints-service"></a>Обзор службы Azure Blueprints

Так же, как чертеж позволяет инженеру или архитектору набросать конструктивные параметры для проекта, схемы Azure Blueprint позволяют облачным архитекторам и центральным ИТ-группам определять воспроизводимый набор ресурсов Azure, который реализует стандарты, шаблоны и требования организации и соблюдает их. С помощью Azure Blueprint группы разработчиков могут быстро создавать и настраивать новые среды, зная, что они создаются в соответствии с требованиями организации и с рядом встроенных компонентов, таких как сети, для ускорения разработки и поставки.

Схемы — это декларативный способ оркестрации развертывания различных шаблонов ресурсов и других артефактов, таких как:

- Назначения ролей
- Назначения политик
- Шаблоны Azure Resource Manager
- Группы ресурсов

Служба Azure Blueprint поддерживает глобально распределенную базу данных [Azure Cosmos DB](../../cosmos-db/introduction.md).
Объекты схемы реплицируются в нескольких регионах Azure. Такая репликация обеспечивает низкую задержку, высокий уровень доступности и согласованный доступ к объектам схемы, независимо от того, в каком регионе служба Blueprints развернула ресурсы.

## <a name="how-its-different-from-resource-manager-templates"></a>В чем отличие от шаблонов Resource Manager

Служба предназначена для помощи с _настройкой среды_. Эта настройка часто состоит из набора групп ресурсов, политик, назначений ролей и развертываний шаблона Resource Manager. Схема — это пакет, который объединяет все эти _артефакты_, и вы можете компоновать такой пакет и создавать его варианты, в том числе через конвейер CI/CD. В конечном счете каждый из них назначается подписке за одну операцию, которую можно контролировать и отслеживать.

Почти все, что вам захочется включить в схемы для развертывания, можно выполнить с помощью шаблона Resource Manager. Однако шаблон Resource Manager является документом, который не существует в исходном виде в Azure — каждый из них хранится локально или в системе управления версиями. Шаблон используется для развертывания одного или нескольких ресурсов Azure, но после развертывания этих ресурсов нет ни активного подключения с используемым шаблоном, ни отношения к нему.

В случае схем связь между определением схемы (тем, _что должно быть_ развернуто) и назначением схемы (тем, _что было_ развернуто) сохраняется. Это подключение поддерживает улучшенные отслеживание и аудит развертываний. Схемы также могут одновременно обновить несколько подписок, управляемых одной схемой.

Нет необходимости выбирать между схемой и шаблоном Resource Manager. В каждой схеме может быть от нуля до нескольких _артефактов_ шаблона Resource Manager. Эта поддержка означает, что предыдущие действия по разработке и поддержке библиотеки шаблонов Resource Manager не пропадут зря, так как все это можно использовать в схемах.

## <a name="how-its-different-from-azure-policy"></a>В чем отличие от Политики Azure

Схема — это пакет или контейнер для составления специальных наборов стандартов, шаблонов и требований к реализации облачных служб Azure, безопасности и разработке, которые можно использовать многократно для поддержания согласованности и соответствия требованиям.

[Политика](../policy/overview.md) — это система разрешений по умолчанию и явных запретов, ориентированная на свойства ресурсов во время развертывания и предназначенная для уже существующих ресурсов. Она поддерживает облачное управление, проверяя соответствие ресурсов в подписке требованиям и стандартам.

Включение политики в схему позволяет создавать правильный шаблон или проект во время назначения схемы. Применение политики также гарантирует, что в среду могут вноситься только утвержденные или планируемые изменения. Это обеспечивает постоянное соответствие целям схемы.

Политику можно включать в определение схем в качестве одного из многих _артефактов_. Схемы также поддерживают использование параметров с политиками и инициативами.

## <a name="blueprint-definition"></a>Определение схемы

Схема состоит из _артефактов_. В настоящее время схемы в качестве артефактов поддерживают следующие ресурсы.

|Ресурс  | Параметры иерархии| ОПИСАНИЕ  |
|---------|---------|---------|
|Группы ресурсов | Подписка | Создайте новую группу ресурсов для использования другими артефактами в схеме.  Эти группы ресурсов-заполнителей позволяют упорядочивать ресурсы в точном соответствии с требуемой структурой и предоставляют механизм ограничения области для включенных артефактов назначения политик и ролей и шаблонов Azure Resource Manager. |
|Шаблон Azure Resource Manager | Подписка, группа ресурсов | Шаблоны используются для создания сложных сред. Примеры сред: ферма SharePoint, платформа настройки состояния службы автоматизации Azure или рабочая область Log Analytics. |
|Назначение политики | Подписка, группа ресурсов | Позволяет назначать политику или инициативу подписке, которой назначена схема. Эта политика или инициатива должна быть в области расположения определения схемы. Если политика или инициатива имеет параметры, эти параметры назначаются при создании схемы или во время назначения схемы. |
|Назначение ролей | Подписка, группа ресурсов | Добавьте существующего пользователя или группу во встроенную роль, чтобы соответствующие пользователи всегда имели соответствующий доступ к ресурсам. Назначения ролей можно определить для всей подписки или в определенной группе ресурсов, включенной в схему. |

### <a name="blueprint-definition-locations"></a>Расположения определений схемы

При создании определения схемы вы указываете место сохранения схемы. Схемы можно сохранять только в [группах управления](../management-groups/overview.md) или подписках, к которым у вас есть доступ **участника**. Если расположение представляет собой группу управления, схему можно назначать любой дочерней подписке этой группы управления.

### <a name="blueprint-parameters"></a>Параметры схемы

Схемы могут передавать параметры в политику/инициативу или в шаблон Azure Resource Manager.
При добавлении в схему какого-либо _артефакта_ автор выбирает, предоставить ли определенное значение каждому назначению схемы или разрешить предоставлять значение каждому назначению схемы во время назначения. Такая гибкость позволяет задать предопределенное значение для всех видов использования схемы или разрешить принимать это решение во время назначения.

> [!NOTE]
> Схема может иметь собственные параметры, но в настоящее время их можно создать только в том случае, если схема была создана из REST API, а не на портале.

Дополнительные сведения см. в описании [параметров схемы](./concepts/parameters.md).

### <a name="blueprint-publishing"></a>Публикация схемы

Изначально, когда схема создается, она находится в режиме **черновика**. Когда она будет готова к назначению, ее следует **опубликовать**. Для публикации необходимо задать строку **версия** (буквы, цифры и дефисы, не более 20 символов) и при желании указать необязательные **сведения об изменениях**. Каждый вариант этой схемы, полученный в результате будущих изменений, имеет свою **версию**, и можно назначать любую версию этой схемы. Кроме того, можно также назначать разные **версии** одной и той же схемы одной подписке. Когда в схему вносятся дополнительные изменения, **опубликованная** **версия** продолжает существовать как версия с **неопубликованными изменениями**. После завершения изменений обновленная схема **публикуется** с новой и уникальной **версией**, и с этого момента ее также можно назначать.

## <a name="blueprint-assignment"></a>Назначение схемы

Каждую **опубликованную** **версию** схемы можно назначить существующей подписке. На портале схеме по умолчанию назначается **версия**, соответствующая версии последней **опубликованной** схемы. Если имеются параметры артефакта (или параметры схемы), они будут задаваться в процессе назначения.

## <a name="permissions-in-azure-blueprints"></a>Разрешения в Azure Blueprint

Чтобы использовать схемы, вам должны быть предоставлены разрешения с помощью [управления доступом на основе ролей](../../role-based-access-control/overview.md) (RBAC). Для создания схем ваша учетная запись должна иметь следующие разрешения:

- `Microsoft.Blueprint/blueprints/write` — создание определения схемы
- `Microsoft.Blueprint/blueprints/artifacts/write` — создание артефактов в определении схемы
- `Microsoft.Blueprint/blueprints/versions/write` — публикация схемы

Для удаления схем ваша учетная запись должна иметь следующие разрешения:

- `Microsoft.Blueprint/blueprints/delete`
- `Microsoft.Blueprint/blueprints/artifacts/delete`
- `Microsoft.Blueprint/blueprints/versions/delete`

> [!NOTE]
> Разрешения на определение схем должны быть предоставлены или унаследованы в области группе управления или подписке, где они хранятся.

Для назначения или отмены назначения схем ваша учетная запись должна иметь следующие разрешения:

- `Microsoft.Blueprint/blueprintAssignments/write` — назначение схемы
- `Microsoft.Blueprint/blueprintAssignments/delete` — отмена назначения схемы

> [!NOTE]
> Так как назначения схем создаются в подписке, разрешения на назначение и отмену назначения схем должны быть предоставлены в области подписки или унаследованы в области подписки.

Все эти разрешения добавляются в роль **владельца**. Роль **участника** имеет разрешения на создание и удаление схем, но не имеет разрешения на назначение схем. Если эти встроенные роли не соответствуют вашим требованиям безопасности, можно создать [настраиваемую роль](../../role-based-access-control/custom-roles.md).

> [!NOTE]
> При использовании управляемого удостоверения, назначенного системой, субъекту-службе для Azure Blueprints требуется роль **владельца** в назначенной подписке, чтобы включить развертывание. При использовании портала эта роль автоматически предоставляется для развертывания и отменяется после завершения развертывания. При использовании REST API эту роль необходимо предоставлять вручную, но она тоже автоматически отменяется после завершения развертывания. При использовании управляемого удостоверения, назначенного пользователем, роль **владельца** нужна только пользователю, создающему назначение схемы.

## <a name="video-overview"></a>Видеообзор

Обзор Azure Blueprint из цикла видеоматериалов "Пятницы с Azure". См. видеообзор [Azure Blueprint из цикла видеоматериалов "Пятницы с Azure"](https://channel9.msdn.com/Shows/Azure-Friday/An-overview-of-Azure-Blueprints) на канале Channel 9.

> [!VIDEO https://www.youtube.com/embed/cQ9D-d6KkMY]

## <a name="next-steps"></a>Дополнительная информация

- [Создание схемы°— портал](create-blueprint-portal.md)
- [Создание схемы°— REST API](create-blueprint-rest-api.md)
