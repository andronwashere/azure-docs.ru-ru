---
title: Настройка параметров — Персонализация
titleSuffix: Azure Cognitive Services
description: Конфигурация сервиса включает в себя то, как сервис рассматривает результаты, как часто он исследует, как часто переобучается модель и сколько данных хранится.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: f0ccf0e480fa57e0ffdfc94ca35cfaceded37a0b
ms.sourcegitcommit: e3b0fb00b27e6d2696acf0b73c6ba05b74efcd85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68663895"
---
# <a name="personalizer-settings"></a>Параметры Персонализатора

Конфигурация сервиса включает в себя то, как сервис рассматривает результаты, как часто он исследует, как часто переобучается модель и сколько данных хранится.

## <a name="create-personalizer-resource"></a>Создание ресурса Персонализатора

Создайте ресурс Персонализатора для каждого цикла обратной связи. 

1. Войдите на [портал Azure](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer). Предыдущая ссылка ведет на страницу **Создание** Персонализатора. 
1. Введите имя службы, выберите подписку, расположение, ценовую категорию и группу ресурсов.
1. Выберите подтверждение и щелкните **Создать**.

## <a name="configure-service-settings-in-the-azure-portal"></a>Настройка параметров службы на портале Azure

1. Войдите на [портале Azure](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer).
1. Найдите нужный ресурс Персонализатора. 
1. В разделе **Управление ресурсами** выберите **Параметры**.

    Прежде чем покидать портал Azure, скопируйте один из ключей ресурсов на странице **Ключи**. Он потребуется для использования [пакета SDK Персонализатора](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.personalizer).

### <a name="configure-reward-settings-for-the-feedback-loop-based-on-use-case"></a>Настройка параметров вознаграждения для цикла обратной связи с учетом варианта использования

Настройте параметры сервиса для использования цикла обратной связи результатов. Изменения следующих настроек приведут к сбросу текущей модели Персонализатора и ее переобучению с помощью данных за последние 2 дня.

![Настройка параметров результата для цикла обратной связи](media/settings/configure-model-reward-settings.png)

|Параметр|Цель|
|--|--|
|Время ожидания результата|Задает продолжительность времени, во время которого Персонализатор будет собирать результаты значений для вызова ранжирования, начиная с момента, когда вызов ранжирования происходит. Это значение устанавливается с помощью вопроса: "Как долго Персонализатор должен ждать результативных звонков?" Любой результат, полученный после этого окна, будет зарегистрирован, но не будет использоваться для обучения.|
|Результат по умолчанию|Если в течение времени ожидания результата, связанного с вызовом ранжирования, Персонализатор не получит результативный вызов, Персонализатор назначит результат по умолчанию. По умолчанию и в большинстве сценариев результат по умолчанию равен нулю.|
|Агрегирование результата|Если за один и тот же вызов API ранжирования получено несколько результатов, используется следующий метод агрегации: **сумма** или **самое раннее**. Самое раннее выбирает самую раннюю полученную оценку и отклоняет остальные. Это полезно, если нужен уникальный результат среди возможно повторяющихся вызовов. |

После изменения этих параметров обязательно выберите **Сохранить**.

### <a name="exploration-setting"></a>Параметр просмотра 

Персонализация может открывать новые шаблоны и, изучая альтернативы, адаптироваться к изменениям поведения пользователей с течением времени. Параметр **Просмотр** определяет, на какой процент вызовов ранжирования дается ответ просмотром. 

Изменения этой настройки приведет к сбросу текущей модели Персонализатора и ее переобучению с помощью данных за последние 2 дня:

![Параметр "Просмотр" определяет, на какой процент вызовов ранжирования дается ответ просмотром.](media/settings/configure-exploration-setting.png)

После изменения этого параметра обязательно выберите **Сохранить**.

### <a name="model-update-frequency"></a>Частота обновления модели

Последняя модель, обученная с помощью вызовов API вознаграждения из каждого активного события, автоматически не используется в вызове ранжирования Персонализатора. **Частота обновления модели** задает частоту использования модели в обновленном вызове ранжирования. 

Высокая частота обновления модели полезна в ситуациях, когда необходимо внимательно отслеживать изменения в поведении пользователей. Это могут быть сайты с новостями, вирусным содержимым или торговлей в режиме реального времени. В таких сценариях модель можно обновлять каждые 15 минут. В большинстве других случаев целесообразно задать меньшую частоту обновления. Ежеминутное обновление будет полезно при отладке кода приложения с помощью Персонализатора, выполнении демонстраций или интерактивном тестировании аспектов машинного обучения.

![Параметр "Частота обновления модели" задает частоту повторного обучения новой модели Персонализатора.](media/settings/configure-model-update-frequency-settings.png)

После изменения этого параметра обязательно выберите **Сохранить**.

### <a name="data-retention"></a>Хранение данных

**Период хранения данных** устанавливает, сколько дней Персонализатор хранит журналы данных. Журналы прошлых данных необходимы для выполнения [автономных вычислений](concepts-offline-evaluation.md), которые используются для измерения эффективности Персонализатора и оптимизации политики обучения.

После изменения этого параметра обязательно выберите **Сохранить**.

## <a name="export-the-personalizer-model"></a>Экспорт модели Персонализатора

В разделе "Управление ресурсами" для **Модели и политики** просмотрите создание модели и дату последнего обновления и экспортируйте текущую модель. Экспортировать файл модели для архивирования можно с помощью портала Azure или API Персонализатора. 

![Экспорт текущей модели Персонализатора](media/settings/export-current-personalizer-model.png)

## <a name="import-and-export-learning-policy"></a>Импорт и экспорт политики обучения

В разделе "Управление ресурсами" для **Модели и политики** импортируйте новую политику обучения или экспортируйте текущую политику обучения.

## <a name="next-steps"></a>Следующие шаги

<!--
[How to use the Personalizer container](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409)
-->
[Подробнее о доступности по регионам](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services)
