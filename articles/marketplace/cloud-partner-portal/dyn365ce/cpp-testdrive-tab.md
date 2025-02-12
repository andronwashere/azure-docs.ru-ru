---
title: Dynamics 365 для взаимодействия с клиентами приложения предлагают вкладку тестовый выпуск | Azure Marketplace
description: Узнайте, как настроить тестовый выпуск в предложении приложения Dynamics 365 for Customer Engagement в AppSource Marketplace.
services: Azure, Marketplace, AppSource, Cloud Partner Portal, Dynamics 365 for Customer Engagement
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 04/24/2019
ms.author: pabutler
ms.openlocfilehash: 5bb5f39ef5f5bce09a8639ba9eedc6d042e60c1d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64942365"
---
# <a name="dynamics-365-for-customer-engagement-application-test-drive-tab"></a>Вкладка "Тестовый выпуск" в приложении Dynamics 365 for Customer Engagement

Вкладка **Тестовый выпуск** позволяет создать пробную версию предложения для клиентов.  Клиенты смогут самостоятельно протестировать ее, а также ознакомиться с ключевыми функциями и преимуществами на практике.  Из доступных вариантов пробной версии тестовый выпуск наиболее эффективен для генерирования высококачественных потенциальных клиентов и их конверсии в реальных.  Дополнительные сведения см. в статье о [тестовом выпуске](../test-drive/what-is-test-drive.md).

Версия тестового выпуска для приложений Dynamics 365 автоматически выполняется как решение, размещенное в Майкрософт.  Дополнительные сведения см. в статье [Размещенный тестовый выпуск](https://docs.microsoft.com/azure/marketplace/cloud-partner-portal/test-drive/hosted-test-drive).

На вкладке "Тестовый выпуск" есть три раздела: **Тестовый выпуск**, **Подробности** и **Technical Configuration** (Техническая конфигурация).  Последние два раздела отображаются только после включения функции "Тестовый выпуск".  Символ звездочки (*) после имени поля указывает, что оно является обязательным. 


## <a name="test-drive-section"></a>Раздел "Тестовый выпуск"

Чтобы включить эту функцию, выберите **Да** на вкладке **Enable a Test Drive** (Включить тестовый выпуск).


## <a name="details-section"></a>Раздел "Подробности"

Укажите основные сведения о тестовом выпуске в разделе **Подробности**.   

![Раздел "Подробности" тестового выпуска](./media/test-drive-tab-details.png)

В приведенной ниже таблице описаны поля, необходимые для настройки тестового выпуска приложения Dynamics 365. Обязательные поля помечены звездочкой (*).

|      Поле                    |    Описание                  |
|    ---------                  |  ---------------                |
|      Описание\*            |   Опишите возможности своего тестового выпуска. Для форматирования описания можно использовать базовые теги HTML. Например, &lt;p&gt;, &lt;em&gt;, &lt;ul&gt;, &lt;li&gt;, &lt;ol&gt; и заголовки.  |
|  Руководство пользователя\*                |   Отправьте руководство пользователя, которое ваши клиенты могут использовать как руководство по тестовому выпуску. Этот документ должен быть в формате PDF. |
|  Демонстрационное видео о тестовом выпуске (необязательно) |  Вы можете предоставить видеоруководство для своего тестового выпуска. Клиент может посмотреть этот видеоролик до начала работы с тестовым выпуском. Укажите URL-адрес видео на YouTube или Vimeo. При выборе параметра **+ Add Video** (+ Добавить видео) вам будет предложено предоставить следующие требования.<ul><li>ИМЯ</li><li>URL-адрес</li><li>Эскиз (в формате PNG, 533 x 324 пикселей)</li></ul>  |
|   |   |


## <a name="technical-configuration-section"></a>Раздел Technical Configuration (Техническая конфигурация)

В этом разделе следует предоставить технические сведения о тестовом выпуске.

![Раздел "Подробности" тестового выпуска](./media/test-drive-tab-tech-config.png)

Где поля имеют следующих целей.  Обязательные поля помечены звездочкой (*).

|      Поле                    |    Описание                  |
|    ---------                  |  ---------------                |
| Тип тестового выпуска\*            | Выберите **Microsoft Hosted (Dynamics 365 for Customer Engagement)** (Размещено в Майкрософт (Dynamics 365 for Customer Engagement)).  |
| Максимальное количество одновременных тестовые выпуски\*    | Количество параллельных экземпляров активного тестового выпуска в любой момент времени. Каждый пользователь будет использовать лицензию Dynamics, пока активен его тестовый выпуск, поэтому вам необходимо убедиться в наличии по крайней мере такого количества доступных лицензий Dynamics для пользователей тестового выпуска. Рекомендуемое значение: 3–5.  |
| Диск продолжительность теста (в часах)\*   | Максимальное количество часов, в течение которых будет активен экземпляр тестового выпуска пользователя. При превышении этого времени экземпляр будет удален из вашего клиента. Рекомендуемое значение: 2–24 часа (в зависимости от сложности приложения). Пользователь всегда может запросить еще один тестовый выпуск, если ему не хватило времени и требуется повторное вычисление.  |
| URL-адрес экземпляра\*                  | URL-адрес, на который перейдет тестовый выпуск. Как правило, это URL-адрес экземпляра Dynamics 365, в котором установлено приложение и содержатся демонстрационные данные.  |
| Идентификатор клиента Azure AD\*            | Глобальный уникальный идентификатор клиента Azure для экземпляра Dynamics 365. Чтобы получить это значение, войдите на портал Azure и последовательно выберите **Azure Active Directory** > **Выбрать свойства** > **Copy the Directory ID (Скопировать идентификатор каталога)** .  |
| Идентификатор приложения Azure AD\*               | Глобальный уникальный идентификатор приложения Azure AD  |
| Ключ приложения Azure AD\*              | Секрет приложения Azure AD, например: `IJUgaIOfq9b9LbUjeQmzNBW4VGn6grr1l/n3aMrnfdk=` |
| Имя клиента Azure AD\*          | Имя клиента Azure для экземпляра Dynamics 365. Используйте формат <tenantname.>onmicrosoft.com, например: `testdrive.onmicrosoft.com`  |
| URL-адрес экземпляра веб-API\*          | URL-адрес веб-API для экземпляра Dynamics 365. Чтобы получить это значение, войдите в экземпляр Microsoft Dynamics 365 и последовательно выберите **Параметры** > **Настройка** > **Ресурсы для разработчиков** > **Instance Web API (Copy this URL) (Веб-API экземпляра (копировать этот URL-адрес))** . Пример значения: `https://testdrive.crm.dynamics.com/api/data/v9.0`  |
| Имя роли\*                     | Имя настраиваемой роли безопасности в Dynamics 365, которое вы создали для тестового выпуска и которое будет назначаться пользователям при запуске, например `testdriveuser`. |
|  |  |

Указав все необходимые сведения, нажмите кнопку **Сохранить**.


## <a name="next-steps"></a>Дальнейшие действия

Далее предоставьте маркетинговую информацию на [вкладке со сведениями об онлайн-магазине](./cpp-storefront-details-tab.md).

