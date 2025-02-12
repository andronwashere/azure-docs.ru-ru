---
title: Жизненный цикл процесса обработки и анализа данных группы
description: Порядок выполнения проектов по обработке и анализу данных
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 40d1af6e6258b5026853532f7963a76d4fc389db
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60837900"
---
# <a name="the-team-data-science-process-lifecycle"></a>Жизненный цикл процесса обработки и анализа данных группы

Процесс обработки и анализа данных группы (TDSP) выполняется в рамках рекомендуемого жизненного цикла, позволяя структурировать проекты по обработке и анализу данных. Этот жизненный цикл представляет стандартные этапы выполнения проектов, от самого начала и до завершения. Если вы используете другой жизненный цикл обработки и анализа данных, например межотраслевой стандартный процесс для исследования данных [(CRISP-DM)](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), обнаружение знаний в базах данных [(KDD)](https://wikipedia.org/wiki/Data_mining#Process) или пользовательский процесс вашей организации, вы можете продолжить использование TDSP на основе задач. 

Этот жизненный цикл предназначен для проектов обработки и анализа данных, которые входят в состав интеллектуальных приложений. Такие приложения развертывают модели машинного обучения или искусственного интеллекта для прогнозной аналитики. Этот процесс будет полезен при работе с исследовательскими проектами обработки и анализа данных, а также проектами на основе ad-hoc-аналитики. Однако для этих проектов некоторые шаги, описанные здесь, могут не понадобиться. 

## <a name="five-lifecycle-stages"></a>Пять этапов жизненного цикла

Жизненный цикл TDSP включает пять основных этапов, которые выполняются циклически. Эти этапы включают:

   1. [Коммерческий аспект](lifecycle-business-understanding.md).
   2. [Получение и анализ данных](lifecycle-data.md).
   3. [Моделирование](lifecycle-modeling.md)
   4. [Развертывание](lifecycle-deployment.md)
   5. [Прием клиентом](lifecycle-acceptance.md).

Визуальное представление жизненного цикла процесса обработки и анализа данных группы: 

![Жизненный цикл процесса обработки и анализа данных группы](./media/lifecycle/tdsp-lifecycle2.png) 


Жизненный цикл TDSP моделируется как циклическая последовательность этапов. По сути, это руководство по выполнению задач, необходимых для работы с прогнозными моделями. Модели прогнозирования развертываются в рабочей среде, которую планируется использовать для разработки интеллектуальных приложений. Жизненный цикл этого процесса помогает развивать проект по анализу и обработке данных до четко определенной конечной цели, предусматривающей его применение. Обработка и анализ данных — это исследовательская деятельность. Но если вы сможете доходчиво объяснить все ключевые моменты сотрудникам и клиентам с помощью хорошо определенного набора артефактов и стандартизированных шаблонов, вы сможете избежать недопонимания и существенно повысить вероятность успешной реализации сложного аналитического проекта.

Для каждого этапа предоставляется следующая информация.

   * **Цели** — определенные задачи.
   * **Способы** — структура конкретных задач и рекомендации по их выполнению.
   * **Артефакты** — конечные результаты и поддержка их реализации.

## <a name="next-steps"></a>Дальнейшие действия

Кроме того, предоставляются полные пошаговые руководства, которые демонстрируют все этапы процесса для конкретных сценариев. Статья [Пошаговые руководства по процессу обработки и анализа данных группы](walkthroughs.md) содержит список сценариев, ссылки и описания эскизов. В пошаговых руководствах показано, как объединить облачные, локальные инструменты и службы в единый рабочий процесс или конвейер, чтобы создать интеллектуальное приложение. 

Примеры выполнения шагов в процессе обработки и анализа данных группы, который использует среду "Студия машинного обучения Azure", см. в статье [Командный процесс обработки и анализа данных с использованием службы "Машинное обучение Azure"](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).
