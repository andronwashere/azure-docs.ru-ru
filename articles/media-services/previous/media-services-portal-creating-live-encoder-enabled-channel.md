---
title: Потоковая трансляция с использованием служб мультимедиа Azure для создания потоков с разными скоростями с помощью портала Azure | Документация Майкрософт
description: В этом руководстве подробно описаны все этапы создания канала, который получает на вход динамический односкоростной поток данных и преобразует его в мультискоростной, с помощью портала Azure.
services: media-services
documentationcenter: ''
author: anilmur
manager: femila
editor: ''
ms.assetid: 504f74c2-3103-42a0-897b-9ff52f279e23
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: juliako
ms.openlocfilehash: daab3c3b2a5b756686a4867350478faaa1142279
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64726861"
---
# <a name="perform-live-streaming-using-media-services-to-create-multi-bitrate-streams-with-azure-portal"></a>Потоковая трансляция с использованием служб мультимедиа для создания потоков с разными скоростями с помощью портала Azure  
> [!div class="op_single_selector"]
> * [Портал](media-services-portal-creating-live-encoder-enabled-channel.md)
> * [.NET](media-services-dotnet-creating-live-encoder-enabled-channel.md)
> * [REST API](https://docs.microsoft.com/rest/api/media/operations/channel)
> 

> [!NOTE]
> В Cлужбы мультимедиа версии 2 больше не добавляются новые компоненты или функциональные возможности. <br/>Ознакомьтесь с новейшей версией Служб мультимедиа — [версией 3](https://docs.microsoft.com/azure/media-services/latest/). Кроме того, см. в разделе [руководство по миграции из версии 2 версии 3](../latest/migrate-from-v2-to-v3.md)

В этом руководстве подробно описаны все этапы создания **канала** , который получает на вход динамический односкоростной поток данных и преобразует его в мультискоростной.

Дополнительные сведения см. в статье [Потоковая трансляция с использованием служб мультимедиа Azure для создания потоков с разными скоростями](media-services-manage-live-encoder-enabled-channels.md).

## <a name="common-live-streaming-scenario"></a>Стандартный сценарий потоковой передачи в режиме реального времени
Ниже описываются основные этапы создания стандартных приложений, предназначенных для потоковой передачи в реальном времени.

> [!NOTE]
> Сейчас максимальная рекомендуемая продолжительность интерактивного события составляет 8 часов. Обратитесь по адресу amslived@microsoft.com, если необходимо запустить канал на более продолжительные отрезки времени.

1. Подключите видеокамеру к компьютеру. <br/>Чтобы установить оборудование, ознакомьтесь с записью блога [Simple and portable event video gear setup]( https://link.medium.com/KNTtiN6IeT) (Настройка простого портативного оборудования для сьемки).

    Если у вас нет доступа к камере, средства, такие как [Telestream Wirecast](https://www.telestream.net/wirecast/overview.htm) можно использовать создания динамического веб-канала из видеофайла.
1. Запустите и настройте локальный динамический кодировщик, который может выводить односкоростной поток по одному из следующих протоколов: RTMP или Smooth Streaming. Дополнительные сведения см. в статье о [поддержке протокола RTMP в службах мультимедиа Azure и о динамических кодировщиках](https://go.microsoft.com/fwlink/?LinkId=532824). <br/>Ознакомьтесь также с записью блога [Live streaming production with OBS](https://link.medium.com/ttuwHpaJeT) (Потоковая трансляция с помощью OBS).

    Это действие также можно выполнить после создания канала.
1. Создайте и запустите канал. 
1. Получите URL-адрес приема канала. 

    URL-адрес приема используется динамическим кодировщиком для отправки потока в канал.
1. Получите URL-адрес предварительного просмотра канала. 

    С его помощью можно убедиться в том, что канал надлежащим образом получает поток.
1. Создайте событие или программу (при этом также будет создан ресурс). 
1. Опубликуйте событие (при этом будет создан указатель OnDemand для соответствующего ресурса).    
1. Когда вы будете готовы начать потоковую передачу и архивацию, запустите событие.
1. При необходимости динамическому кодировщику можно дать сигнал начать показ рекламы. Реклама вставляется в выходной поток.
1. Чтобы остановить потоковую передачу и архивацию содержимого события, завершите работу события.
1. Удалите событие (и при необходимости ресурс).   

## <a name="prerequisites"></a>Технические условия

Ниже перечислены необходимые условия для выполнения действий, описанных в этом учебнике.

* Для работы с этим учебником требуется учетная запись Azure. Если ее нет, можно создать бесплатную пробную учетную запись всего за несколько минут. 
  Дополнительные сведения см. в разделе [Бесплатная пробная версия Azure](https://azure.microsoft.com/pricing/free-trial/).
* Учетная запись служб мультимедиа. Инструкции по созданию учетной записи служб мультимедиа см. в статье [Создание учетной записи служб мультимедиа Azure с помощью портала Azure](media-services-portal-create-account.md).
* Веб-камера и кодировщик, который передает динамический односкоростной поток данных.

## <a name="create-a-channel"></a>Создание канала

1. На [портале Azure](https://portal.azure.com/) щелкните "Службы мультимедиа", а затем — имя учетной записи служб мультимедиа.
2. Выберите **Live Streaming**(Потоковая трансляция).
3. Щелкните **Настраиваемое создание**. Этот параметр позволяет создать канал с поддержкой кодирования в реальном времени.

    ![Создание канала](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-channel.png)
4. Щелкните **Параметры**.

   1. Выберите тип канала с **кодированием в реальном времени**. Он указывает, что необходимо создать канал с поддержкой кодирования в режиме реального времени. Это означает, что входящий односкоростной поток отправляется в канал и преобразуется в мультискоростной поток на основе заданных параметров динамического кодировщика. Подробные сведения см. в статье [Потоковая трансляция с использованием служб мультимедиа Azure для создания потоков с разными скоростями](media-services-manage-live-encoder-enabled-channels.md). Нажмите кнопку ОК.
   2. Укажите имя канала.
   3. В нижней части экрана нажмите кнопку "ОК".
5. Выберите вкладку **Ingest** (Прием).

   1. На этой странице можно выбрать протокол потоковой передачи. Для типа канала **с кодированием в реальном времени** подходят такие протоколы:

      * Односкоростной фрагментированный MP4 (Smooth Streaming)
      * односкоростной RTMP;

        Подробное описание каждого из протоколов см. в статье [Потоковая трансляция с использованием служб мультимедиа Azure для создания потоков с разными скоростями](media-services-manage-live-encoder-enabled-channels.md).

        Во время работы канала или связанных с ним событий и программ протокол изменить нельзя. Если вам нужны другие протоколы потоковой передачи, создайте отдельный канал для каждого из них.  
   2. К приему данных можно применять ограничения IP-адресов. 

       Вы можете определить IP-адреса, с которых разрешено принимать видео в этом канале. Разрешенные IP-адреса можно указать в виде одного IP-адреса (например, 10.0.0.1), диапазона IP-адресов, заданного с помощью IP-адреса и маски подсети CIDR (например, 10.0.0.1/22), или диапазона IP-адресов, заданного с помощью IP-адреса и маски подсети в десятичной записи (например, 10.0.0.1(255.255.252.0)).

       Если не указаны IP-адреса и не определено правило, разрешенных IP-адресов нет. Чтобы разрешить все IP-адреса, создайте правило и задайте адрес 0.0.0.0/0.
6. На вкладке **Предварительный просмотр** укажите ограничения IP-адресов для предварительного просмотра.
7. На вкладке **Кодировка** укажите предустановки для кодирования. 

    В настоящее время можно выбрать только значение **По умолчанию 720p**. Чтобы указать пользовательские предустановки, создайте запрос в службу поддержки Майкрософт. Введите имя набора предустановок, созданного для вас. 

> [!NOTE]
> В настоящее время запуск канала может занять до 30 минут. Сброс канала может занимать до 5 минут.
> 
> 

После создания канала вы можете выбрать канал и открыть вкладку **Параметры** , на которой отображаются конфигурации каналов. 

Подробные сведения см. в статье [Потоковая трансляция с использованием служб мультимедиа Azure для создания потоков с разными скоростями](media-services-manage-live-encoder-enabled-channels.md).

## <a name="get-ingest-urls"></a>Получение URL-адресов приема
После создания канала можно получить URL-адреса приема, которые необходимо передать динамическому кодировщику. Он использует эти адреса для передачи динамического потока на вход.

![URL-адреса приема](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-ingest-urls.png)

## <a name="create-and-manage-events"></a>Создание событий и управление ими

### <a name="overview"></a>Обзор
Канал связан с событиями и программами, с помощью которых вы можете управлять публикацией и хранением сегментов динамического потока. Каналы управляют событиями и программами. Отношение между каналом и программой очень похоже на традиционные мультимедиа, где канал передает постоянный поток контента, а программа ограничена временным событием на этом канале.

Чтобы задать количество часов, в течение которых следует хранить записанное содержимое события, укажите длительность **окна архивирования** . Для него можно задать значение от 5 минут до 25 часов. Длина окна архивирования также определяет максимальный период, в пределах которого клиенты могут перемещаться назад во времени относительно текущей позиции в передаваемом потоке данных. События могут происходить в течение определенного времени, однако содержимое, выходящее за пределы этого периода, теряется. Значение этого свойства также определяет максимальный размер манифестов клиентов.

Каждое событие связано с ресурсом. Чтобы опубликовать событие, необходимо создать указатель OnDemand для соответствующего ресурса. С помощью этого указателя можно сформировать URL-адрес потоковой передачи данных, который предоставляется клиентам.

Канал поддерживает одновременную потоковую трансляцию максимум трех событий, поэтому можно создавать по несколько архивов одного и того же входящего потока. Благодаря этому можно публиковать и архивировать разные части транслируемого мероприятия. Предположим, согласно требованиям вашей компании, необходимо архивировать 6 часов события, а транслировать только последние 10 минут. Для этого необходимо создать два одновременно работающих события. Для одного из них настроено архивирование 6 часов события, но без публикации. Для другого события настроено архивирование 10 минут с публикацией.

Не используйте существующие программы повторно для новых мероприятий. Вместо этого создайте и запустите новую программу для каждого события.

Когда вы будете готовы начать потоковую передачу и архивацию, запустите событие или программу. Чтобы остановить потоковую передачу и архивацию содержимого события, завершите работу события. 

Чтобы удалить архивированное содержимое, остановите и удалите событие, а затем удалите связанный с ним ресурс. Ресурс невозможно удалить, пока он используется каким-либо событием. Сначала нужно удалить это событие. 

Даже после остановки и удаления события пользователи смогут запрашивать потоковую передачу архивированного видеосодержимого, пока не удален соответствующий ресурс.

Если вы хотите сохранить заархивированное содержимое, но при этом заблокировать возможность его потоковой передачи, удалите указатель.

### <a name="createstartstop-events"></a>Создание, запуск и остановка событий
Настроив передачу потока данных в канал, вы можете начать потоковую передачу мероприятия, создав ресурс, программу и указатель потоковой передачи. В результате вы сможете запустить архивирование потока и предложить его зрителям через конечную точку потоковой передачи. 

>[!NOTE]
>При создании учетной записи AMS в нее добавляется конечная точка потоковой передачи **по умолчанию** в состоянии **Остановлена**. Чтобы начать потоковую передачу содержимого и воспользоваться динамической упаковкой и динамическим шифрованием, конечная точка потоковой передачи, из которой необходимо выполнять потоковую передачу содержимого, должна находиться в состоянии **Выполняется**. 

Запустить мероприятие можно двумя способами. 

1. На странице **Канал** щелкните **Событие прямой трансляции**, чтобы добавить новое событие.

    Укажите название события и ресурса, окно архивирования и вариант кодирования.

    ![Создание программы](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-create-program.png)

    Если оставить установленным флажок **Publish this live event now** (Опубликовать это динамическое событие), для события будут созданы URL-адреса публикации.

    Когда вы будете готовы к потоковой передаче события, щелкните **Start**(Начать).

    После запуска события нажмите кнопку **Watch** (Просмотр), чтобы начать воспроизведение содержимого.
2. Можно также выполнить сразу все операции, нажав на странице **Канал** кнопку **Запустить**. В результате будут созданы ресурс по умолчанию, программа и указатель потоковой передачи.

    Событию будет присвоено название **default** , и для него будет задано окно архивирования 8 часов.

Опубликованное событие можно просмотреть на странице **Live Event** (Интерактивное событие). 

После нажатия кнопки **Off Air**(Прекратить трансляцию) трансляция всех событий остановится. 

## <a name="watch-the-event"></a>Просмотр события
Чтобы просмотреть событие, щелкните **Посмотреть** на портале Azure или скопируйте URL-адрес потоковой передачи и используйте проигрыватель по своему усмотрению. 

![Создано](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-play-event.png)

После остановки интерактивное событие автоматически преобразуется в содержимое по требованию.

## <a name="clean-up"></a>Очистка
После завершения потоковой передачи мероприятия вы можете удалить выделенные ранее ресурсы с помощью описанной ниже процедуры.

* Остановите трансляцию потока из кодировщика.
* Остановите работу канала. После остановки канала начисление платы прекращается. Если вам понадобится снова запустить его, вы можете воспользоваться тем же URL-адресом приема (перенастраивать кодировщик не потребуется).
* Вы можете остановить конечную точку потоковой передачи, если больше не собираетесь предоставлять доступ к архиву мероприятия в качестве потоковой передачи по требованию. Пока канал остановлен, начисление платы не осуществляется.

## <a name="view-archived-content"></a>Просмотр архивного содержимого
Даже после остановки и удаления события пользователи смогут запрашивать потоковую передачу архивированного видеосодержимого, пока не удален соответствующий ресурс. Ресурс невозможно удалить, пока он используется каким-либо событием: сначала нужно удалить это событие. 

Для управления ресурсами последовательно выберите **Параметры** и **Ресурсы**.

![Активы](./media/media-services-portal-creating-live-encoder-enabled-channel/media-services-assets.png)

## <a name="considerations"></a>Рекомендации
* Сейчас максимальная рекомендуемая продолжительность интерактивного события составляет 8 часов. Обратитесь по адресу amslived@microsoft.com, если необходимо запустить канал на более продолжительные отрезки времени.
* Убедитесь, что конечная точка потоковой передачи, из которой нужно передавать содержимое потоком, находится в состоянии **Работает**.

## <a name="next-step"></a>Дальнейшие действия
Просмотрите схемы обучения работе со службами мультимедиа.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

