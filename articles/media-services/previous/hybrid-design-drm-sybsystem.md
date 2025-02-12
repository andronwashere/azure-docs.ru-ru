---
title: Гибридная структура подсистем DRM с использованием служб мультимедиа Azure | Документация Майкрософт
description: Здесь рассматривается гибридная структура подсистем управления цифровыми правами (DRM) с использованием служб мультимедиа Azure.
services: media-services
documentationcenter: ''
author: willzhan
manager: femila
editor: ''
ms.assetid: 18213fc1-74f5-4074-a32b-02846fe90601
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2019
ms.author: willzhan;juliako
ms.openlocfilehash: 5c86a49cd9dc26f724de12ed2e5e77e645e4ab53
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61466601"
---
# <a name="hybrid-design-of-drm-subsystems"></a>Гибридная структура подсистем DRM 

Здесь рассматривается гибридная структура подсистем управления цифровыми правами (DRM) с использованием служб мультимедиа Azure.

## <a name="overview"></a>Обзор

Службы мультимедиа Azure поддерживают три следующие системы DRM:

* PlayReady
* Widevine (модульная);
* FairPlay

Поддержка DRM включает в себя шифрование DRM (динамическое шифрование) и доставку лицензий, а Проигрыватель мультимедиа Azure поддерживает все 3 системы DRM как пакет SDK проигрывателя браузера.

Подробные сведения о проектировании и реализации подсистемы DRM с CENC см. в статье [CENC с несколькими DRM и управление доступом: справочное проектирование и реализация в Azure и службах мультимедиа Azure](media-services-cenc-with-multidrm-access-control.md).

Несмотря на то что мы обеспечиваем полную поддержку трех систем DRM, иногда следует использовать различные части собственной инфраструктуры или подсистемы вместе со службами мультимедиа Azure, чтобы создать гибридную подсистему DRM.

Ниже представлены некоторые часто задаваемые вопросы пользователей:

* Можно ли использовать собственные серверы лицензирования DRM? (В этом случае пользователи вложили средства в кластер серверов лицензирования DRM со встроенной бизнес-логикой.)
* Можно ли использовать доставку лицензий DRM в службах мультимедиа Azure без размещения содержимого в AMS?

## <a name="modularity-of-the-ams-drm-platform"></a>Модульность платформы AMS DRM

Как часть комплексной облачной видеоплатформы, DRM служб мультимедиа Azure имеет гибкую и модульную структуру. Службы мультимедиа Azure можно использовать вместе с любым сочетанием, представленным в следующей таблице (после таблицы идет объяснение). 

|**Размещение содержимого и источник**|**Шифрование содержимого**|**Доставка лицензий DRM**|
|---|---|---|
|AMS|AMS|AMS|
|AMS|AMS|Сторонний производитель|
|AMS|Сторонний производитель|AMS|
|AMS|Сторонний производитель|Сторонний производитель|
|Сторонний производитель|Сторонний производитель|AMS|

### <a name="content-hosting--origin"></a>Размещение содержимого и источник

* AMS: видеоресурсы размещаются в AMS, а потоковая передача ведется с помощью конечных точек потоковой передачи AMS (но не обязательно с динамической упаковкой).
* Сторонний производитель: видео размещается и доставляется с помощью платформы потоковой передачи стороннего производителя за пределами AMS.

### <a name="content-encryption"></a>Шифрование содержимого

* AMS: шифрование содержимого происходит динамически или по требованию с помощью динамического шифрования AMS.
* Сторонний производитель: шифрование содержимого выполняется за пределами AMS с использованием рабочего процесса предварительной обработки.

### <a name="drm-license-delivery"></a>Доставка лицензий DRM

* AMS: лицензию DRM доставляет служба доставки лицензий AMS.
* Сторонний производитель: лицензию DRM доставляет сервер лицензирования DRM стороннего производителя за пределами AMS.

## <a name="configure-based-on-your-hybrid-scenario"></a>Настройка на основе своего гибридного сценария

### <a name="content-key"></a>Ключ содержимого

Настроив ключ содержимого, можно управлять следующими атрибутами динамического шифрования AMS и службы доставки лицензий AMS:

* Ключ содержимого, используемый для динамического шифрования DRM.
* Содержимое лицензии DRM, которое будет доставляться с помощью службы доставки лицензий: права, ключ содержимого и ограничения.
* Тип **ограничения политики авторизации ключа содержимого**: открытое, IP-адрес или ограничение по токену.
* Если для **ограничения политики авторизации ключа содержимого** используется **токен**, перед выдачей лицензии необходимо выполнить условия **ограничения политики авторизации ключа содержимого**.

### <a name="asset-delivery-policy"></a>Политика доставки ресурсов

Настроив политику доставки ресурсов, можно контролировать следующие атрибуты, которые используются динамическим упаковщиком AMS и динамическим шифрованием конечной точки потоковой передачи AMS:

* Протокол потоковой передачи и комбинация шифрования DRM, например DASH в CENC (PlayReady и Widevine), Smooth Streaming в PlayReady, HLS в Widevine или PlayReady.
* URL-адреса лицензий доставки (встроенных и по умолчанию) для каждой используемой системы DRM.
* Содержат ли URL-адреса для приобретения лицензий (LA_URL) в DASH MPD или списке воспроизведения HLS строку запроса идентификатора ключа (KID) для Widevine и FairPlay соответственно.

## <a name="scenarios-and-samples"></a>Сценарии и примеры

Как описано в предыдущем разделе, в следующих пяти гибридных сценариях используются соответствующие сочетания конфигураций **ключа содержимого** -**политики доставки ресурсов** (примеры указаны в последнем столбце таблицы):

|**Размещение содержимого и источник**|**Шифрование DRM**|**Доставка лицензий DRM**|**Настройка ключа содержимого**|**Настройка политики доставки для ресурса-контейнера**|**Пример**|
|---|---|---|---|---|---|
|AMS|AMS|AMS|Да|Да|Пример 1|
|AMS|AMS|Сторонний производитель|Да|Да|Пример 2|
|AMS|Сторонний производитель|AMS|Да|Нет|Пример 3|
|AMS|Сторонний производитель|Внешняя|Нет|Нет|Пример 4|
|Сторонний производитель|Сторонний производитель|AMS|Да|Нет|    

В примерах защита PlayReady работает как для DASH, так и для Smooth Streaming. Ниже приведены URL-адреса видео, которые являются URL-адресами Smooth Streaming. Чтобы получить соответствующие URL-адреса DASH, просто добавьте "(format=mpd-time-csf)". Для проверки в браузере можно использовать [проигрыватель для тестирования мультимедиа Azure](https://aka.ms/amtest). Он позволяет настроить протокол потоковой передачи для использования с каждой технологией. Internet Explorer 11 и Microsoft Edge в Windows 10 поддерживают PlayReady через EME. Дополнительные сведения см. в записи блога [Azure Media Test Tool](https://blogs.msdn.microsoft.com/playready4/2016/02/28/azure-media-test-tool/) (Средство для тестирования мультимедиа Azure).

### <a name="sample-1"></a>Пример 1

* URL-адрес источника (базовый): https://willzhanmswest.streaming.mediaservices.windows.net/1efbd6bb-1e66-4e53-88c3-f7e5657a9bbd/RussianWaltz.ism/manifest 
* URL-адрес для приобретения лицензии PlayReady (DASH и Smooth Streaming): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 
* URL-адрес для приобретения лицензии Widevine (DASH): https://willzhanmswest.keydelivery.mediaservices.windows.net/Widevine/?kid=78de73ae-6d0f-470a-8f13-5c91f7c4 
* URL-адрес для приобретения лицензии FairPlay (HLS): https://willzhanmswest.keydelivery.mediaservices.windows.net/FairPlay/?kid=ba7e8fb0-ee22-4291-9654-6222ac611bd8 

### <a name="sample-2"></a>Пример 2

* URL-адрес источника (базовый): https://willzhanmswest.streaming.mediaservices.windows.net/1a670626-4515-49ee-9e7f-cd50853e41d8/Microsoft_HoloLens_TransformYourWorld_816p23.ism/Manifest 
* URL-адрес для приобретения лицензии PlayReady (DASH и Smooth Streaming): http://willzhan12.cloudapp.net/PlayReady/RightsManager.asmx 

### <a name="sample-3"></a>Пример 3

* URL-адрес источника: https://willzhanmswest.streaming.mediaservices.windows.net/8d078cf8-d621-406c-84ca-88e6b9454acc/20150807-bridges-2500.ism/manifest 
* URL-адрес для приобретения лицензии PlayReady (DASH и Smooth Streaming): https://willzhanmswest.keydelivery.mediaservices.windows.net/PlayReady/ 

### <a name="sample-4"></a>Пример 4

* URL-адрес источника: https://willzhanmswest.streaming.mediaservices.windows.net/7c085a59-ae9a-411e-842c-ef10f96c3f89/20150807-bridges-2500.ism/manifest 
* URL-адрес для приобретения лицензии PlayReady (DASH и Smooth Streaming): https://willzhan12.cloudapp.net/playready/rightsmanager.asmx 

## <a name="summary"></a>Сводка

Таким образом, компоненты DRM служб мультимедиа Azure являются гибкими. Их можно использовать в гибридных сценариях при правильной настройке ключа содержимого и политики доставки ресурсов, как описано в этой статье.

## <a name="next-steps"></a>Дальнейшие действия
Просмотрите схемы обучения работе со службами мультимедиа.

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Отзывы
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

