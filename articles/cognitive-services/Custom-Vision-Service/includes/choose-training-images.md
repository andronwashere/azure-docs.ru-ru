---
author: PatrickFarley
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: include
ms.date: 07/17/2019
ms.author: pafarley
ms.openlocfilehash: ee2c1fd1fc1cad07b14a2c99318be20be30db9c5
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/23/2019
ms.locfileid: "68423587"
---
Мы рекомендуем включить в начальный обучающий набор не менее 30 изображений для каждого тега. Кроме того, вам потребуются несколько дополнительных изображений для тестирования обученной модели.

Чтобы обучение модели было эффективным, используйте разнообразные изображения. Изображения должны отличаться по следующим аспектам:
* угол обзора камеры;
* освещение;
* background
* стиль изображения;
* отдельные объекты и группы;
* size
* Тип

Также убедитесь, что все обучающие изображения соответствуют следующим критериям:
* файлы с расширением .jpg, .png или .bmp;
* размер не более 6 МБ (4 МБ для прогнозирования изображений);
* не менее 256 пикселей по короткой стороне (Пользовательская служба визуального распознавания автоматически увеличивает изображения меньшего размера).