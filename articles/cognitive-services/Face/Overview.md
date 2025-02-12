---
title: Что такое API Распознавания лиц?
titleSuffix: Azure Cognitive Services
description: Узнайте, как использовать службу "Распознавание лиц" для обнаружения и анализа лиц на изображениях.
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: overview
ms.date: 07/03/2019
ms.author: pafarley
ms.openlocfilehash: 4be33f781dec93fd9fe1b1846322672266cd7350
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2019
ms.locfileid: "67606908"
---
# <a name="what-is-the-azure-face-api"></a>Что такое API распознавания лиц Azure?

API Распознавания лиц Azure — это когнитивная служба, предоставляющая алгоритмы для обнаружения, распознавания и анализа человеческих лиц на изображениях. Возможность обработки информации о человеческом лице важна во многих разных сценариях использования ПО, включая безопасность, естественный пользовательский интерфейс, анализ содержимого изображения и управление им, мобильные приложения и робототехнику.

API Распознавания лиц предоставляет несколько разных функций. Все они описываются в следующих разделах. Ознакомьтесь с дополнительными сведениями, чтобы узнать больше о каждой функции.

## <a name="face-detection"></a>Определение лиц

API Распознавания лиц может выявлять лица на изображениях и возвращать координаты прямоугольника, в котором они расположены. При необходимости эта функция извлекает ряд атрибутов, связанных с лицом, например позу, пол, возраст, положение головы, волосы на лице и очки.

> [!NOTE]
> Функция определения лиц также доступна через [API Компьютерного зрения](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home). Если вы хотите выполнять дополнительные операции с информацией о лицах, используйте API Распознавания лиц (описанная в этой статье служба).

![Изображение женщины и мужчины с прямоугольниками, нарисованными вокруг их лиц, а также сведения о возрасте и поле](./Images/Face.detection.jpg)

См. подробнее об [определении лиц](concepts/face-detection.md). См. также справочную документацию по [API определения](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="face-verification"></a>Проверка лиц

API проверки выполняет проверку идентичности двух обнаруженных лиц или одного обнаруженного лица по отношению к одному человеку. Практически оно оценивает, принадлежат ли два лица одному человеку. Это может быть полезно в сценариях, связанных с обеспечением безопасности. См. подробнее в руководстве по [Распознаванию лиц](concepts/face-recognition.md) или справочной документации по [API Проверки](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a).

## <a name="find-similar-faces"></a>Поиск похожих лиц

API Поиска похожих лиц сравнивает целевое лицо и набор доступных для поиска лиц, чтобы найти небольшое количество лиц, очень похожих на целевое. Поддерживаются два режима работы: matchPerson и matchFace. Режим matchPerson возвращает похожие лица после фильтрации для того же пользователя (с помощью[API Проверки](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f3039523a)). Режим matchFace игнорирует такой фильтр. Он возвращает список обнаруженных лиц, которые могут или не могут принадлежать тому же человеку.

В следующем примере показано целевое лицо:

![Улыбающаяся женщина](./Images/FaceFindSimilar.QueryFace.jpg)

А это лица-кандидаты:

![Пять изображений улыбающихся людей. Изображения А и Б: изображения одного человека](./Images/FaceFindSimilar.Candidates.jpg)

При поиске похожих лиц режим matchPerson возвращает фотографии А и Б, на которых изображен тот же человек, что и на фотографии с целевым лицом. Режим matchFace возвращает фотографии А, Б, В, Г, т. е. четырех кандидатов, даже если некоторые из них не совпадают с целевым лицом или имеют низкое сходство. См. подробнее в руководстве по [Распознаванию лиц](concepts/face-recognition.md) или справочной документации по [API Поиска похожих лиц](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395237).

## <a name="face-grouping"></a>Группировка лиц

API группы делит неизвестные лица на несколько групп, основываясь на сходстве. Каждая группа является несвязанным подмножеством исходного набора лиц. Все лица в одной группе, скорее всего, будут принадлежать одному и тому же человеку. Для одного человека может быть несколько таких групп. Группы различаются по разным факторам, например по выражению лица. См. подробнее в руководстве по [Распознаванию лиц](concepts/face-recognition.md) или справочной документации по [API Группирования](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395238).

## <a name="person-identification"></a>Идентификация личности

API Идентификации может использоваться для определения обнаруженных лиц в базе данных. Это может быть полезно для автоматического добавления тегов в ПО для управления фотографиями. Эту базу данных можно создать заранее и редактировать по мере использования.

Ниже показан пример базы данных с именем `"myfriends"`. Каждая группа может содержать до 1 млн объектов, соответствующих разным людям. В свою очередь, для каждого объекта, соответствующего одному человеку, можно зарегистрировать до 248 лиц.

![Таблица с тремя столбцами для разных людей, каждый с тремя записями изображений лиц.](./Images/person.group.clare.jpg)

Создав и обучив базу данных, вы можете идентифицировать новое обнаруженное лицо путем сравнения с группой. Если лицо определяется как принадлежащее человеку в группе, то возвращается объект, соответствующий этому человеку.

См. подробнее об идентификации людей в руководстве по [Распознаванию лиц](concepts/face-recognition.md) или справочной документации по [API Идентификации](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395239).

## <a name="use-containers"></a>Использование контейнеров

[Контейнер API Распознавания лиц](face-how-to-install-containers.md) можно использовать для обнаружения, распознавания и идентификации лиц, установив стандартные контейнеры Docker в непосредственной близости к своим данным.

## <a name="sample-apps"></a>Примеры приложений

Следующие примеры приложений демонстрируют несколько способов использования API Распознавания лиц:

- [API распознавания лиц Майкрософт: клиентская библиотека Windows с примерами](https://github.com/Microsoft/Cognitive-Face-Windows). Это приложение WPF, демонстрирующее несколько сценариев определения, анализа и идентификации лиц.
- [Приложение UWP FamilyNotes](https://github.com/Microsoft/Windows-appsample-familynotes).Это приложение универсальной платформы Windows (UWP), которое использует идентификацию лиц вместе с речью, рукописным вводом, Cortana и камерами в сценарии совместного доступа к семейным заметкам.

## <a name="data-privacy-and-security"></a>Конфиденциальность и безопасность данных

Как и в случае с другими ресурсами Cognitive Services, разработчикам, использующим API Распознавания лиц, следует учитывать политику корпорации Майкрософт в отношении клиентских данных. См. подробнее на [странице Cognitive Services](https://www.microsoft.com/trustcenter/cloudservices/cognitiveservices) в Центре управления безопасностью Майкрософт.

## <a name="next-steps"></a>Дополнительная информация

В следующем кратком руководстве показано, как реализовать сценарий определения лиц в коде.

- [Краткое руководство Обнаружение лиц на изображении с помощью пакета SDK для .NET и C#](quickstarts/csharp.md). Также доступны и другие языки.
