---
title: Справочник по API на портале партнера Cloud | Azure Marketplace
description: Описание предварительных требований и список операций Marketplace API.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 9626fdb5dbdf720a65217421565fc559a0cee684
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935475"
---
<a name="cloud-partner-portal-api-reference"></a>Справочные материалы по API портала Cloud Partner
==================================

REST API портала Cloud Partner позволяют осуществлять программный поиск и обработку рабочих нагрузок, предложений и профилей издателей. API-интерфейсы используют управление доступом на основе ролей (RBAC) для обеспечения правильных разрешений во время обработки.

Эта ссылка предоставляет технические сведения для REST API портала Cloud Partner. Примеры полезных данных в этом документе, предназначены только для справки и могут меняться по мере добавления новых функциональных возможностей.


<a name="prerequisites-and-considerations"></a>Предварительные требования и рекомендации
-------------------------------

Прежде чем использовать API-интерфейсы, следует проверить следующее:

- статью [Предварительные требования](./cloud-partner-portal-api-prerequisites.md), чтобы узнать, как добавить субъект-службу к учетной записи и получить маркер доступа Azure Active Directory (Azure AD) для аутентификации; 
- две стратегии [Управление параллелизмом](./cloud-partner-portal-api-concurrency-control.md)
доступные для вызова API;
- дополнительные [рекомендации](./cloud-partner-portal-api-considerations.md) API, например, управление версиями и обработка ошибок.


<a name="common-tasks"></a>Стандартные задачи
------------
В этой статье содержатся сведения о API, для выполнения следующих задач.


### <a name="offers"></a>Предложения

-   [Получение всех предложений](./cloud-partner-portal-api-retrieve-offers.md)
-   [Получение определенного предложения](./cloud-partner-portal-api-retrieve-specific-offer.md)
-   [Получение состояния предложения](./cloud-partner-portal-api-retrieve-offer-status.md)
-   [Создание предложения](./cloud-partner-portal-api-creating-offer.md)
-   [Публикация предложения](./cloud-partner-portal-api-publish-offer.md)

### <a name="operations"></a>Операции

-   [Получение операций](./cloud-partner-portal-api-retrieve-operations.md)
-   [Отмена операций](./cloud-partner-portal-api-cancel-operations.md)

### <a name="publish-an-app"></a>Публикация приложения

-   [Запуск](./cloud-partner-portal-api-go-live.md)

### <a name="other-tasks"></a>Другие задачи

-   [Настройка цен на предложения виртуальных машин](./cloud-partner-portal-api-setting-price.md)

### <a name="troubleshooting"></a>Устранение неполадок

-   [Устранение неполадок с аутентификацией](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)
