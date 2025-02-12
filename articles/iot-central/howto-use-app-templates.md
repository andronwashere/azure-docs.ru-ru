---
title: Использование шаблонов приложений в Azure IoT Central | Документация Майкрософт
description: Вы узнаете, как оператор может использовать наборы устройств в приложении Azure IoT Central.
author: dominicbetts
ms.author: dobett
ms.date: 05/30/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: a26f70c5a61f3855a3de991072a7e84103e87b69
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66499224"
---
# <a name="use-application-templates"></a>Использование шаблонов приложений

В этой статье описывается как менеджер решений для создания и использования шаблонов приложений.

При создании приложения Azure IoT Central, вы увидите список встроенные образцы шаблонов. Можно также создать собственные шаблоны приложений из существующих приложений IoT Central. Затем можно использовать собственные шаблоны приложений, при создании новых приложений.

Создавая шаблон приложения, он включает следующие элементы существующего приложения:

- Панели мониторинга по умолчанию приложения, включая макет панели мониторинга и все плитки, которые вы определили.
- Шаблоны для устройств, включая измерения, параметры, свойства, команды и панели мониторинга.
- Правила. Включены все определения правил. Тем не менее действия, за исключением действия электронной почты, не включаются.
- Задает устройства, включая условия и панели мониторинга.

> [!WARNING]
> Если панель мониторинга содержит плитки, которые отображают сведения о конкретных устройств, а затем отобразить эти плитки **запрошенный ресурс не найден** в новом приложении. Необходимо перенастроить эти плитки для отображения сведений об устройствах в новое приложение.

Создавая шаблон приложения, она не включает следующие элементы:

- Устройства
- Пользователи
- Определения заданий
- Определения экспортов непрерывных данных

Вручную добавьте эти элементы для любого приложения, созданные на основе шаблона приложения.

## <a name="create-an-application-template"></a>Создание шаблона приложения

Чтобы создать шаблон приложения из существующего приложения IoT Central:

1. Перейдите к **администрирования** раздела приложения.
1. Выберите **Экспорт шаблона приложения**.
1. На **экспорта шаблона приложений** введите имя и описание шаблона.
1. Выберите **Экспорт** кнопку, чтобы создать шаблон приложения. Вы можете скопировать **ссылку с общим доступом** , позволяющий кто-то создать новое приложение на основе шаблона:

![Создание шаблона приложения](media/howto-use-app-templates/create-template.png)

## <a name="use-an-application-template"></a>С помощью шаблона приложения

Чтобы использовать шаблон приложения для создания нового приложения IoT Central, вам потребуется ранее созданный объект **ссылку с общим доступом**. Вставить **ссылку с общим доступом** в адресную строку браузера. **Создать приложение** отобразилась страница с выбран шаблон пользовательского приложения:

![Создание приложения с помощью шаблона](media/howto-use-app-templates/create-app.png)

Выберите ваш план платежей и заполните другие поля в форме. Затем выберите **создать** для создания нового приложения IoT Central на основе шаблона приложения.

## <a name="manage-application-templates"></a>Управление шаблонами приложений

На **экспорта шаблона приложений** страницы, вы можете удалить или обновить данный шаблон приложения.

Если удалить шаблон приложения, больше не можно использовать ранее созданный ссылку с общим доступом для создания новых приложений.

Для обновления шаблона приложения, измените имя шаблона или описание на **экспорта шаблона приложений** страницы. Затем выберите **Экспорт** еще раз. Это действие создает новую **ссылку с общим доступом** и сделает недействительным все предыдущие **ссылку с общим доступом** URL-адрес.

## <a name="next-steps"></a>Дальнейшие действия

Теперь, когда вы узнали, как использовать шаблоны приложений, мы предлагаем — Узнайте, как [управление IoT Central на портале Azure](howto-manage-iot-central-from-portal.md)
