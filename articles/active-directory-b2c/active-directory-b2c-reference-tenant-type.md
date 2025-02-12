---
title: Доступность в регионах и местонахождение данных в Azure Active Directory B2C | Документация Майкрософт
description: Статья о типах клиентов Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/10/2017
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 30f13a3b85e68babcaef62b9200a296105b3ce83
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66509005"
---
# <a name="azure-active-directory-b2c-region-availability--data-residency"></a>Azure Active Directory B2C Доступность в регионах и размещение данных
Доступность в регионах и местонахождение данных — это два разных понятия, которые применяются к Azure AD B2C не так, как к другим службам Azure. В этой статье рассматриваются различия между этими двумя понятиями и сравнивается то, как они применяются к Azure AD B2C и к Azure в целом.

## <a name="summary"></a>Сводка
Служба Azure AD B2C **доступна во всем мире**, но **данные могут находиться в США или Европе**.

## <a name="concepts"></a>Основные понятия
* **Доступность в регионах** определяет то, где служба доступна для использования.
* **Местонахождение данных** определяет то, где хранятся данные пользователей.

## <a name="region-availability"></a>Регионы доступности
Служба Azure AD B2C доступна во всем мире посредством общедоступного облака Azure. 

Для большинства других служб Azure используется другая модель, в соответствии с которой доступность связана с местонахождением данных. Примеры этого можно увидеть на странице [Доступность продуктов по регионам](https://azure.microsoft.com/regions/services/) и в [калькуляторе цен Azure Active Directory B2C](https://azure.microsoft.com/pricing/details/active-directory-b2c/).

## <a name="data-residency"></a>Местонахождение данных
Служба Azure AD B2C сохраняет данные пользователей в США или Европе.

Местонахождение данных зависит от страны или региона, выбранных при [создании клиента Azure AD B2C](active-directory-b2c-get-started.md).

![Снимок экрана: предварительная версия клиента](./media/active-directory-b2c-reference-tenant-type/data-residency-b2c-tenant.png)

Данные находятся в США при выборе следующих стран или регионов:

> Гватемала, Доминиканская Республика, Канада, Коста-Рика, Мексика, Панама, Пуэрто-Рико, США, Тринидад и Тобаго, Эль-Сальвадор.

Данные находятся в Европе при выборе следующих стран или регионов:

> Алжир, Австрия, Азербайджан, Бахрейн, Беларусь, Бельгия, Болгария, Хорватия, Кипр, Чешская Республика, Дания, Египет, Эстония, Финляндия, Франция, Германия, Греция, Венгрия, Исландия, Ирландия, Израиль, Италия, Иордания, Казахстан, Кения, Кувейт, Lativa, Ливан, Лихтенштейн, Литва, Люксембург, Северная Македония, Мальта, Черногория, Марокко, Нидерланды, Нигерия, Норвегия, Оман, Пакистан, Польша, Португалия, Катар, Румыния, Россия, Саудовская Аравия, Сербия, Словакия, Словения, Южно-Африканская Республика, Испания, Швеция, Швейцария, Тунис, Турция, Украина, Объединенные Арабские Эмираты и Соединенное Королевство.

Остальные страны и регионы будут постепенно добавляться в список.  Пока же вы можете использовать Azure AD B2C, выбрав любую из стран или регионов, приведенных выше.

> Афганистан, Аргентина, Австралия, Бразилия, Чили, Колумбия, Эквадор, Гонконг, САР, Индия, Индонезия, Ирак, Япония, Корея, Малайзия, Новая Зеландия, Парагвай, Перу, Филиппины, Сингапур, Шри-Ланка, Тайвань, Таиланд, Уругвай и Венесуэла.

## <a name="preview-tenant"></a>Предварительная версия клиента
Если клиент B2C создан в течение периода действия предварительной версии Azure AD B2C, для параметра **Тип клиента** задано значение **Предварительная версия клиента**. В этом случае клиент НЕОБХОДИМО использовать для разработки и тестирования, а НЕ для рабочих приложений.

> [!IMPORTANT]
> Способа перехода с предварительной версии клиента B2C на рабочую не существует. Обратите внимание, что при удалении существующего клиента B2C предварительной версии и повторном создании клиента B2C рабочей версии с тем же доменным именем могут возникнуть известные проблемы. Создавайте клиент B2C рабочей версии с другим доменным именем.


![Снимок экрана: предварительная версия клиента](./media/active-directory-b2c-reference-tenant-type/preview-b2c-tenant.png)
