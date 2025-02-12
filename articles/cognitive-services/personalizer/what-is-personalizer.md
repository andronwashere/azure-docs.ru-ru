---
title: Что такое служба "Персонализатор"?
titleSuffix: Azure Cognitive Services
description: Персонализатор — это облачная служба API, с помощью которой приложение максимально удобный режим работы для своих пользователей, изучая их поведение в реальном времени.
services: cognitive-services
author: edjez
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: overview
ms.date: 05/07/2019
ms.author: edjez
ms.openlocfilehash: 286a19207236392367b924bea7e26e90fd0db8d5
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2019
ms.locfileid: "68253465"
---
# <a name="what-is-personalizer"></a>Что такое служба "Персонализатор"?

Персонализатор Azure — это облачная служба API, которая позволяет выбирать оптимальную работу для своих пользователей, изучая их поведение в режиме реального времени.

* Предоставьте информацию о содержимом и пользователях, чтобы получить самое популярное действие, которое следует им показать. 
* Перед использованием Персонализатора нет необходимости очищать и маркировать данные.
* Оставляйте отзывы в Персонализатор в удобное для вас время. 
* Просматривайте аналитику в режиме реального времени. 
* Используйте Персонализатор в совокупности с масштабными действиями по обработке и анализу данных для проверки проводящихся экспериментов.

## <a name="how-does-personalizer-work"></a>Принцип работы службы "Персонализатор"

Персонализатор использует модели машинного обучения для обнаружения действия, которому следует назначить наивысший приоритет в контексте. Ваше клиентское приложение предоставляет список возможных действий с информацией о них, а также сведения о контексте, которые могут содержать информацию о пользователе, устройстве и т. д. Персонализатор определяет действие, которое нужно предпринять. Как только ваше клиентское приложение использует выбранное действие, оно отправляет отзыв в Персонализатор в виде оценки вознаграждения. После получения отзыва Персонализатор автоматически обновляет свою модель, используемую для будущего ранжирования.

## <a name="how-do-i-use-the-personalizer"></a>Как использовать службу "Персонализатор"?

![Использование Персонализатора для выбора видео, которое следует показать пользователю](media/what-is-personalizer/personalizer-example-highlevel.png)

1. Выберите среду в своем приложении, которую необходимо персонализировать.
1. Создайте и настройте экземпляр службы персонализации на портале Azure. Каждый экземпляр представляет цикл Персонализатора.
1. Используйте пакет SDK для вызова Персонализатора с информацией (_функциями_) о пользователях и содержимом (_действиях_). Перед использованием службы "Персонализатор" нет необходимости предоставлять очищенные, маркированные данные. 
1. Покажите пользователю в клиентском приложении действие, выбранное службой "Персонализатор".
1. Используйте пакет SDK, чтобы оставить отзыв в Персонализаторе с указанием того, выбрал ли пользователь действие, предложенное службой. Это — _оценка вознаграждения_, которая обычно измеряется в диапазоне от –1 до 1.
1. Просмотрите аналитику на портале Azure, чтобы оценить работу системы и то, насколько ваши данные помогают персонализации.

## <a name="where-can-i-use-personalizer"></a>Где можно использовать службу "Персонализатор"?

Например, службу "Персонализатор" можно добавить в свое клиентское приложение для таких целей:

* персонализация выбора статей, которые будут выделены на новостном веб-сайте;    
* оптимизация размещения рекламы на веб-сайте;
* отображение персонализированного рекомендуемого товара в интернет-магазине;
* предложение элементов пользовательского интерфейса, например фильтров, для применения к определенной фотографии;
* выбор ответа бота для уточнения намерения пользователя или предложения действия;
* ранжирование предложений по поводу того, какой следующий шаг должен выполнить пользователь в бизнес-процессе.

## <a name="personalization-for-developers"></a>Персонализация для разработчиков

Служба "Персонализатор" имеет два интерфейса API:

* отправление информации (_функций_) о пользователях и содержимом (_действиях_) для персонализации; Персонализатор отправляет ответ с самым популярным действием;
* отправление отзыва в службу "Персонализатор" с оценкой того, насколько удачно сработало ранжирование, обычно в виде цифры от 0 до 1 (в предыдущем разделе — от –1 до 1). 

![Основная последовательность событий для персонализации](media/what-is-personalizer/personalization-intro.png)

## <a name="next-steps"></a>Дополнительная информация

* [Краткое руководство Personalize content using C#](csharp-quickstart-commandline-feedback-loop.md) (Персонализация содержимого с помощью C#)
* [Использование интерактивной демонстрации](https://personalizationdemo.azurewebsites.net/)
