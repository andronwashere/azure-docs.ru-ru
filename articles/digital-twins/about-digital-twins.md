---
title: Обзор Azure Digital Twins | Документация Майкрософт
description: Сведения о службе Azure Digital Twins — решении Интернета вещей для пространственной аналитики.
author: julieseto
ms.author: jseto
ms.date: 05/31/2019
ms.topic: overview
ms.service: digital-twins
services: digital-twins
manager: bertvanhoof
ms.custom: mvc
ms.openlocfilehash: 0bbbb924cace93e44af84fd96926a2d7153dd5f8
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2019
ms.locfileid: "67459024"
---
# <a name="overview-of-azure-digital-twins"></a>Обзор Azure Digital Twins

Azure Digital Twins (предварительная версия) представляет собой службу Интернета вещей, с помощью которой можно создать комплексные модели физического окружения. С ее помощью можно создавать пространственные интеллектуальные графы для моделирования связей и взаимодействий между людьми, пространствами и устройствами.

Azure Digital Twins позволяет запрашивать данные из физического пространства, а не из многих разрозненных датчиков. С помощью этой службы можно создавать многоразовые, высокомасштабируемые, пространственно-ориентированные возможности, которые связывают потоковые данные в цифровом и реальном мире. Эти уникальные контекстно-зависимые функции оптимизируют ваши приложения. 

Azure Digital Twins применяется ко всем типам сред, таким как хранилища, офисы, школы, больницы и банки. Ее можно использовать даже для стадионов, фабрик, автостоянок, парков, интеллектуальных сетей и городов. Ниже приведены некоторые сценарии, где Azure Digital Twins может пригодиться.

- прогнозирование требований в техническом обслуживании для фабрики;
- анализ требований в энергии в реальном времени для электрической сети;
- оптимизация использования свободного места для офиса.
- отслеживание дневной температуры в разных штатах;
- мониторинг загруженных путей для полета дронов;
- определение автономных транспортных средств;
- анализ уровней заполняемости здания;
- поиск самого загруженного кассового аппарата в магазине.

Независимо от вашего реального бизнес-сценария вероятнее всего с помощью Digital Twins можно подготовить соответствующий цифровой экземпляр.

В следующем видеоролике служба Azure Digital Twins рассматривается более подробно.

> [!VIDEO https://www.youtube.com/embed/TvN_NxpgyzQ]

## <a name="key-capabilities"></a>Ключевые возможности

Azure Digital Twins имеет следующие ключевые возможности.

### <a name="spatial-intelligence-graph"></a>Пространственный интеллектуальный граф

[*Пространственный интеллектуальный граф*](./concepts-objectmodel-spatialgraph.md#graph)( или *пространственный граф*) является виртуальным представлением физического окружения. С его помощью можно моделировать связи между пользователями, расположениями и устройствами.

Рассмотрим интеллектуальное приложение, предназначенное для коммунально-бытовых сценариев. Оно может включать несколько счетчиков потребления электроэнергии, подключенных в одном районе. Компания, использующая это приложение, должна точно отслеживать и прогнозировать потребление электроэнергии и выставление счетов. Каждое устройство и датчик необходимо смоделировать с контекстом о расположении и клиенте, которому будет выставлен счет. С помощью пространственного интеллектуального графа можно моделировать такие сложные взаимосвязи.

### <a name="digital-twin-object-models"></a>Цифровые модели объектов двойников

[Цифровые модели объектов двойников](./concepts-objectmodel-spatialgraph.md#model) — это предопределенные протоколы устройств и схема данных. Они представляют потребности определенной предметной области решения, чтобы ускорить и упростить разработку.

Например, приложение для заполнения комнаты может использовать заранее определенные типы пространства, такие как корпус, здание, этаж и комната.

### <a name="multiple-and-nested-tenants"></a>Множественные и вложенные клиенты

Вы можете создавать решения, которые безопасно масштабируются и которые можно повторно использовать для нескольких клиентов. Вы также можете создать несколько вложенных клиентов, к которым можно получить доступ и которые можно использовать изолированным и безопасным образом.

Например, приложение для использования пространства можно настроить для изоляции данных клиента от данных другого клиента в рамках одного здания. Можно также использовать приложение для объединения данных для одного клиента с несколькими зданиями.

### <a name="advanced-compute-capabilities"></a>Расширенные вычислительные возможности

С помощью [определяемых пользователем функций](./concepts-user-defined-functions.md) можно определять и запускать пользовательские функции в отношении входящих [данных устройства](./concepts-device-ingress.md) для отправки сигналов в заранее определенные конечные точки. Эта расширенная возможность улучшает процессы настройки и автоматизацию задач устройства.

Например, интеллектуальное приложение для сельского хозяйства, которое выполняет определяемую пользователем функцию для оценки показаний датчика влажности почвы, а также для прогнозирования погоды. Затем приложение отправляет сигналы о потребностях в орошении.

### <a name="built-in-access-control"></a>Встроенное управление доступом

Используя функции управления доступом и идентификацией, такие как [управление доступом на основе ролей](./security-role-based-access-control.md) и [Azure Active Directory](./security-authenticating-apis.md), вы можете безопасно контролировать доступ отдельных лиц и устройств.

Примером может выступать приложение для управления объектами, которое настроено таким образом, чтобы позволить людям в комнате устанавливать температуру в пределах указанного диапазона. Менеджеры объекта могут устанавливать любое значение температуры в любой комнате.

### <a name="ecosystem"></a>Экосистема

Экземпляр Azure Digital Twins можно подключить к множеству эффективных служб Azure. В число этих служб входят Azure Stream Analytics, служба ИИ Azure и служба хранилища Azure. Сюда также относятся Azure Maps, Microsoft Mixed Reality, Dynamics 365 или Office 365.

Примером может выступать интеллектуальное приложение для офисного здания, которое использует Digital Twins для представления команд и устройств, расположенных на многих этажах. Так как устройства потоком передают динамические данные в подготовленный экземпляр Digital Twin, Stream Analytics обрабатывает эти данные для предоставления эффективных практических сведений. Данные хранятся в службе хранилища Azure и преобразованы в формат файла с совместным доступом. Файл распределяется по всей организации с помощью Office 365.

## <a name="solutions-that-benefit-from-azure-digital-twins"></a>Преимущества использования Digital Twins

Служба Azure Digital Twins эффективна для представления реального мира и его многочисленных связей. Она упрощает моделирование Интернета вещей, обработку данных, событий и отслеживание устройств. Рассмотрим только несколько следующих сценариев, представляющих разные отрасли. Преимущества ее использования:

* Отображение для компаний по управлению объектами уровней заполняемости пространства с течением времени для получения сведений о лучших способах конфигурации офисного здания.
* Запуск бланков на заказ работы для мобильного приложения. Использование для управления отправкой охранников, планирования услуг по уборке и поддержанию чистоты и любыми другими услугами в торговом или спортивном центре.
* Отображение сведений о том, какие комнаты в помещении заняты (в режиме реального времени), и помощь в резервировании рабочего пространства в соответствии с потребностями.
* Отслеживание расположения ресурсов в пространстве.
* Оптимизация зарядки электрических транспортных средств путем моделирования предпочтений пользователей и ограничений энергосетей.

## <a name="azure-digital-twins-in-the-context-of-other-iot-services"></a>Azure Digital Twins в контексте других служб Интернета вещей

Digital Twins использует Центр Интернета вещей для подключения устройств и датчиков Интернета вещей, чтобы обеспечить соответствие всех процессов современному реальному миру. На следующей схеме показано, как служба Azure Digital Twins связана с другими службами Интернета вещей.

![Digital Twins — служба, созданная на основе Центра Интернета вещей][1]

Дополнительные сведения об Интернете вещей см. в статье [Технологии и решения Интернета вещей Azure: PaaS и SaaS](https://docs.microsoft.com/azure/iot-fundamentals/iot-services-and-technologies).

## <a name="next-steps"></a>Дополнительная информация

Перейдите к короткому демонстрационному видеоролику о службе Azure Digital Twins:

>[!div class="nextstepaction"]
>[Краткое руководство Поиск свободных помещений с помощью Azure Digital Twins](./quickstart-view-occupancy-dotnet.md)

Ознакомьтесь с более подробными сведениями о приложении управления объектами с помощью Digital Twins:

>[!div class="nextstepaction"]
>[Руководство. Развертывание Azure Digital Twins и настройка пространственного графа](./tutorial-facilities-setup.md)

Ознакомьтесь с основными понятиями Digital Twins:

>[!div class="nextstepaction"]
>[Основные сведения об объектных моделях и пространственном интеллектуальном графе в Digital Twins](./concepts-objectmodel-spatialgraph.md)

<!-- Images -->
[1]: media/overview/azure-digital-twins-in-iot-ecosystem.png