---
title: Средства исследования и визуализации данных в Azure | Документация Майкрософт
description: Средства исследования и визуализации данных для виртуальной машины для обработки и анализа данных.
keywords: средства анализа и обработки данных, виртуальная машина для анализа и обработки данных, средства для анализа и обработки данных, анализ и обработка данных Linux
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: gokuma
ms.openlocfilehash: 693be80e493a0ba259d147f432dc9d6c07ba876d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "66427513"
---
# <a name="data-exploration-and-visualization-tools-on-the-data-science-virtual-machine"></a>Средства исследования и визуализации данных на виртуальной машине для обработки и анализа данных

Ключевым шагом в обработке и анализе данных является их понимание. Средства визуализации и исследования данных позволяют ускорить этот процесс. Ниже приведены некоторые средства, предоставляемые на DSVM, которые упрощают этой ключевой стадии. 

## <a name="apache-drill"></a>Apache Drill
|    |           |
| ------------- | ------------- |
| Что это такое?   | Обработчик SQL-запросов с открытым исходным кодом для больших данных    |
| Поддерживаемые версии виртуальных машин для обработки и анализа данных      | Windows, Linux  |
| Настройка и установка на DSVM      |  Установлено в `/dsvm/tools/drill*` лишь во встроенном режиме   |
| Распространенные способы применения      |  Просмотр данных на месте без использования процессов извлечения, преобразования и загрузки. Запрос разных источников и форматов данных, включая CSV, JSON, реляционные таблицы и Hadoop.     |
| Запуск и использование      | С помощью ярлыка на рабочем столе  <br/> [Drill in 10 Minutes](https://drill.apache.org/docs/drill-in-10-minutes/) (Начало работы с Drill за 10 минут)  |
| Дополнительные средства на виртуальной машине для обработки и анализа данных      |   Rattle, Weka, SQL Server Management Studio      |

## <a name="weka"></a>Weka
|    |           |
| ------------- | ------------- |
| Что это такое?   |  Weka — это коллекция алгоритмов машинного обучения для задач интеллектуального анализа данных. Алгоритмы можно применить непосредственно к набору данных или вызвать из кода Java. Weka содержит средства для предварительной обработки данных, классификации, регрессии, кластеризации, правил взаимосвязей и визуализации. |
| Поддерживаемые выпуски виртуальных машин для обработки и анализа данных (DSVM)     | Windows, Linux     |
| Распространенные способы применения      | Общее средство машинного обучения     |
| Запуск и использование      | В Windows выполните поиск Weka в меню "Пуск". В Linux, войдите с помощью X2Go, затем перейдите к приложений "->" разработки -> Weka. |
| Ссылки на примеры      | [Примеры Weka](https://www.cs.waikato.ac.nz/ml/weka/documentation.html) |
| Дополнительные средства на виртуальной машине для обработки и анализа данных      |LightGBM, Rattle, XGBoost   |

## <a name="rattle"></a>Rattle;
|    |           |
| ------------- | ------------- |
| Что это такое?   |   Графический пользовательский интерфейс для интеллектуального анализа данных с помощью R.   |
| Поддерживаемые выпуски виртуальных машин для обработки и анализа данных (DSVM)     | Windows, Linux     |
| Распространенные способы применения      | Универсальное средство пользовательского интерфейса для интеллектуального анализа данных для R    |
| Запуск и использование      | Средство пользовательского интерфейса. В Windows откройте окно командной строки, запустите R, а затем внутри R запустите `rattle()`. В Linux подключитесь к X2Go, запустите терминал, запустите R, а затем внутри R запустите `rattle()`. |
| Ссылки на примеры      | [Rattle](https://togaware.com/onepager/) |
| Дополнительные средства на виртуальной машине для обработки и анализа данных      |LightGBM, Weka, XGBoost   |

## <a name="power-bi-desktop"></a>Power BI Desktop 
|    |           |
| ------------- | ------------- |
| Что это такое?   | Средство интерактивной визуализации данных и бизнес-аналитики    |
| Поддерживаемые версии виртуальных машин для обработки и анализа данных      | Windows  |
| Распространенные способы применения      |  Визуализация данных и создание панелей мониторинга   |
| Запуск и использование      | С помощью ярлыка на рабочем столе (`C:\Program Files\Microsoft Power BI Desktop\bin\PBIDesktop.exe`)      |
| Дополнительные средства на виртуальной машине для обработки и анализа данных      |   2019 г. Visual Studio, Visual Studio Code, Juno      |

