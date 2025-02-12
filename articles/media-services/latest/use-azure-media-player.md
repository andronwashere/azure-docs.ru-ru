---
title: Воспроизведение с помощью Проигрывателя мультимедиа Azure | Документация Майкрософт
description: В этом разделе приводится краткий обзор Проигрывателя мультимедиа Azure.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 07/17/2019
ms.author: juliako
ms.openlocfilehash: 6157f59d96770298d947a1cf6ca28aa343e1a0d4
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311823"
---
# <a name="azure-media-player-overview"></a>Обзор Проигрывателя мультимедиа Azure

Проигрыватель мультимедиа Azure — это веб-проигрыватель, созданный для воспроизведения мультимедийного содержимого из Служб мультимедиа Microsoft Azure в разных браузерах и на разных устройствах. Проигрыватель мультимедиа Azure использует такие отраслевые стандарты, как HTML5, Media Source Extensions (MSE) и Encrypted Media Extensions (EME), для оптимальной адаптивной потоковой передачи. Если эти стандарты недоступны на устройстве или в браузере, Проигрыватель мультимедиа Azure использует Flash и Silverlight. Независимо от используемой технологии воспроизведения, разработчики получают единый интерфейс JavaScript для доступа к интерфейсам API. Это позволяет службам мультимедиа Azure обслуживать содержимое для воспроизведения на широком спектре устройств и в различных браузерах без дополнительных усилий.

Службы мультимедиа Microsoft Azure поддерживают форматы потоковой передачи HLS, DASH и Smooth Streaming для воспроизведения содержимого. Проигрыватель мультимедиа Azure учитывает эти различные форматы и автоматически воспроизводит наиболее подходящую ссылку в зависимости от возможностей платформы или браузера. Службы мультимедиа также позволяют динамически шифровать ресурсы с использованием PlayReady или 128-битного шифрования AES. Проигрыватель мультимедиа Azure поддерживает расшифровку содержимого, зашифрованного с помощью PlayReady и 128-битного алгоритма шифрования AES, если заданы соответствующие параметры. 

> [!NOTE]
> Для зашифрованного содержимого Widevine требуется воспроизведение HTTPS.

[Начните бесплатное пробное использование](https://azure.microsoft.com/pricing/free-trial/)

## <a name="use-azure-media-player-demo-page"></a>Использование демонстрационной страницы Проигрывателя мультимедиа Azure

### <a name="start-using"></a>Начать использовать

[Демонстрационную страницу Проигрывателя мультимедиа Azure](https://aka.ms/azuremediaplayer) можно использовать для воспроизведения примеров Служб мультимедиа Azure или собственного потока.  

Чтобы воспроизвести новое видео, вставьте другой URL-адрес и нажмите кнопку **Обновить**.

Чтобы настроить различные параметры воспроизведения (например, технологию, язык или шифрование), нажмите кнопку **Дополнительные параметры**.

![Проигрыватель мультимедиа Azure](./media/azure-media-player/home-page.png)

### <a name="monitor-diagnostics-of-a-video-stream"></a>Мониторинг диагностики видеопотока

Для отслеживания диагностики видеопотока можно использовать [демонстрационную страницу Проигрывателя мультимедиа Azure](https://aka.ms/azuremediaplayer). 

![Диагностика Проигрывателя мультимедиа Azure Media Player](./media/azure-media-player/diagnostics.png)

## <a name="set-up-azure-media-player-in-your-html"></a>Настройка Проигрывателя мультимедиа Azure в HTML

Проигрыватель мультимедиа Azure можно легко настроить. Для базового воспроизведения мультимедийного содержимого из учетной записи Служб мультимедиа потребуется всего несколько минут. Дополнительные сведения о настройке Проигрывателя мультимедиа Azure см. в [документации по Проигрывателю мультимедиа Azure](https://aka.ms/ampdocs). 

## <a name="next-steps"></a>Следующие шаги

- [Документация по Проигрывателю мультимедиа Azure](https://aka.ms/ampdocs)
- [Примеры Проигрывателя мультимедиа Azure](https://aka.ms/ampsamples)
