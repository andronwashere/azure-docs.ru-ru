---
title: Тестирование производительности приложения в Хранилище дисков Azure — управляемые диски
description: Узнайте о процессе тестирования производительности приложения в Azure.
services: virtual-machines-linux,storage
author: roygara
ms.author: rogarana
ms.date: 01/11/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.subservice: disks
ms.openlocfilehash: 21ed4e9a6b1da10d0ae4c276612459506e13d94f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "61474374"
---
# <a name="benchmarking-a-disk"></a>Тестирование производительности диска

Тестирование производительности — это процесс моделирования различных рабочих нагрузок приложения и измерения его производительности при каждой из них. Выполнив действия, описанные в статье [Хранилище Azure класса Premium. Проектирование для обеспечения высокой производительности](premium-storage-performance.md), вы собрали требования к производительности приложения. Применив инструменты тестирования производительности в виртуальных машинах с приложением, можно определить уровень производительности, которого оно может достичь при использовании хранилища класса «Премиум». В этой статье приведены примеры тестирования производительности виртуальной машины серии DS14 (цен. категория "Стандартный"), для которой подготовлены диски хранилища класса Premium.

Мы использовали стандартные инструменты тестирования производительности Iometer для Windows и FIO для Linux. Эти инструменты порождают несколько потоков, моделирующих производительность при рабочей нагрузке, и измеряют производительность системы. С помощью этих инструментов можно задать такие параметры, как размер блока и длина очереди, которые обычно нельзя менять в приложениях. Это обеспечивает большую гибкость для достижения максимальной производительности в высокомасштабируемых виртуальных машинах с подготовленными дисками класса "Премиум" для различных рабочих нагрузок приложения. Дополнительные сведения о каждом инструменте измерения производительности см. на веб-сайте [Iometer](http://www.iometer.org/) и на странице [FIO](http://freecode.com/projects/fio).

Создайте виртуальную машину серии DS14 класса «Стандартный» и подключите к ней 11 дисков хранилища класса «Премиум» согласно примеру ниже. Для параметра кэширования узлов 10 из 11 дисков задайте значение None и чередуйте их в томе NoCacheWrites. Для параметра кэширования оставшегося диска задайте значение ReadOnly и создайте том CacheReads с этим диском. Благодаря такой настройке можно узнать максимальный уровень производительности виртуальной машины серии DS14 (цен. категория "Стандартный") при чтении и записи. Подробные инструкции по созданию виртуальной машины DS14 с использованием дисков класса Premium см. в статье [Хранилище Azure класса Premium. Проектирование для обеспечения высокой производительности](premium-storage-performance.md).

[!INCLUDE [virtual-machines-disks-benchmarking](../../../includes/virtual-machines-managed-disks-benchmarking.md)]

## <a name="next-steps"></a>Дальнейшие действия

Прочитайте статью о проектировании для высокой производительности. В ней описано, как создать для прототипа такой же контрольный список, как для существующего приложения. С помощью инструментов тестирования производительности можно смоделировать рабочую нагрузку и измерить производительность прототипа приложения. В результате вы сможете определить, какой из классов диска будет соответствовать требованиям к производительности приложения, а какой превосходить их. Затем те же указания можно применить и для рабочего приложения.

> [!div class="nextstepaction"]
> См. статью о начале [проектирования для обеспечения высокой производительности](premium-storage-performance.md).