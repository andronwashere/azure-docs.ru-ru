---
title: Advanced Threat Protection - база данных Azure для PostgreSQL — одном сервере
description: Система защиты от угроз обнаруживает подозрительные действия в базе данных, указывающие на наличие потенциальных угроз безопасности.
author: bolzmj
ms.author: mbolz
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: a69a8b651188b9566fdbec4bc6de09a8fec164c7
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65067462"
---
# <a name="advanced-threat-protection-for-azure-database-for-postgresql---single-server"></a>Расширенная защита от угроз для базы данных Azure для PostgreSQL: один сервер

Служба "Расширенная защита от угроз" для Базы данных Azure для PostgreSQL позволяет выявить подозрительную активность, указывающую на необычные и потенциально опасные попытки получить доступ к базам данных или воспользоваться ими.

Расширенная защита от угроз входит в состав предложения "Расширенная защита данных", которое представляет собой унифицированный пакет расширенных возможностей безопасности. Перейти к службе "Расширенная защита от угроз" и управлять ею можно на [портале Azure](https://portal.azure.com). Она сейчас доступна в предварительной версии.

> [!NOTE]
> Компонент "Расширенная защита от угроз" **недоступен** в следующих регионах Azure для национальных облаков и облаков для государственных организаций: US Gov (Техас), US Gov (Аризона), US Gov (Айова), US Gov (Вирджиния), восточный регион US DoD, центральный регион US DoD, Центральная Германия, Северная Германия, Восточный Китай, Восточный Китай — 2. Сведения об общей доступности продукта по регионам см. на [этой странице](https://azure.microsoft.com/global-infrastructure/services/).
>

> [!NOTE]
> Эта функция доступна во всех регионах Azure, где База данных Azure для PostgreSQL развернута для серверов общего назначения и серверов, оптимизированных для операций в памяти.

## <a name="set-up-threat-detection"></a>Настройка обнаружения угроз
1. Запустите портал Azure: [https://portal.azure.com](https://portal.azure.com).
2. Перейдите на страницу настройки сервера Базы данных Azure для PostgreSQL, который требуется защитить. В параметрах безопасности выберите **Расширенная защита от угроз (предварительная версия)** .
3. На странице настройки **Расширенная защита от угроз (предварительная версия)** сделайте следующее:

   - Включите расширенную защиту от угроз на сервере.
   - В **параметрах расширенной защиты от угроз** в текстовом поле **Кому отправлять оповещения** предоставьте список адресов электронной почты пользователей, которым будут отправляться оповещения системы безопасности об обнаружении аномальных действий в базе данных.
  
   ![Настройка обнаружения угроз](./media/howto-database-threat-protection-portal/set-up-threat-protection.png)

## <a name="explore-anomalous-database-activities"></a>Анализ подозрительной активности в базе данных

При обнаружении подозрительной активности в базе данных вы получите уведомление по электронной почте. В этом уведомлении содержится информация о подозрительном событии безопасности, в том числе характер подозрительных операций, имя базы данных, имя сервера, имя приложения, а также время, когда произошло событие. Кроме того, в уведомлении приводится информация о возможных причинах возникновения события, а также рекомендуемые действия по поиску и устранению потенциальной угрозы безопасности базы данных.
    
1. Щелкните ссылку **View recent alerts** (Просмотреть последние оповещения) в электронном сообщении, чтобы запустить портал Azure и открыть страницу оповещений центра безопасности Azure. На этой странице содержится обзор активных угроз, обнаруженных в базе данных SQL.
    
    ![Отчеты об аномальных действиях](./media/howto-database-threat-protection-portal/anomalous-activity-report.png)

    Просмотрите активные угрозы:

    ![Активные угрозы](./media/howto-database-threat-protection-portal/active-threats.png)

2. Щелкните определенное оповещение, чтобы отобразить дополнительные сведения и действия для изучения этой угрозы и устранения будущих угроз.
    
    ![Определенное оповещение](./media/howto-database-threat-protection-portal/specific-alert.png)

## <a name="explore-threat-detection-alerts"></a>Анализ оповещений об обнаружении угроз

Расширенная защита от угроз интегрируется с [центром безопасности Azure](https://azure.microsoft.com/services/security-center/). 

Щелкните **Оповещения безопасности** в разделе **Защита от угроз**, чтобы запустить страницу оповещений центра безопасности Azure и получить обзор активных угроз SQL, обнаруженных в базе данных.

  ![Повышение защиты от угроз](./media/howto-database-threat-protection-portal/threat-detection-alert-asc.png)

## <a name="next-steps"></a>Дальнейшие действия

* Узнайте больше о [центре безопасности Azure](https://docs.microsoft.com/azure/security-center/security-center-intro).
* Дополнительные сведения о ценах см. на [странице цен на Базу данных Azure для PostgreSQL](https://azure.microsoft.com/pricing/details/postgresql/).  
