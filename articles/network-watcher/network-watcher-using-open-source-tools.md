---
title: Визуализация распределения сетевого трафика с помощью Наблюдателя за сетями Azure и средств с открытым кодом | Документация Майкрософт
description: На этой странице описывается, как использовать захват пакетов Наблюдателя за сетями со средством CapAnalysis для визуализации распределения входящего и исходящего трафика на виртуальных машинах.
services: network-watcher
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: kumud
ms.openlocfilehash: 3a0ae782d3fe97752ca8b9e786c3c2672f554277
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "64936008"
---
# <a name="visualize-network-traffic-patterns-to-and-from-your-vms-using-open-source-tools"></a>Визуализация распределения входящего и исходящего трафика на виртуальных машинах с помощью средств с открытым кодом

Захват пакетов содержит сетевые данные, которые позволяют выполнять экспертизу сети и тщательную проверку пакетов. Есть множество средств с открытым кодом, которые можно использовать для анализа захвата пакетов, чтобы получить ценную информацию о сети. В числе таких средств можно назвать средство с открытым кодом CapAnalysis, предназначенное для визуализации захвата пакетов. Визуализация данных захвата пакетов позволяет быстро получать информацию о тенденциях и аномалиях в сети. Также визуализация позволяет передавать такую информацию другим людям в простом и доступном формате.

Наблюдатель за сетями Azure позволяет собирать данные, предоставляя функцию захвата пакетов в сети. Эта статья содержит пошаговое руководство по визуализации и анализу данных захвата пакетов с помощью CapAnalysis и Наблюдателя за сетями.

## <a name="scenario"></a>Сценарий

На виртуальной машине в Azure развернуто простое веб-приложение, для которого вы хотите выполнить визуализацию сетевого трафика с помощью средств с открытым кодом, чтобы быстро понять распределение потоков и возможные аномалии. Наблюдатель за сетями позволяет получить захват пакетов конкретной сетевой среды и сохранить эти данные в учетной записи хранения. После этого CapAnalysis может принять захват пакетов непосредственно из BLOB-объекта хранилища и визуализировать его содержимое.

![сценарий][1]

## <a name="steps"></a>Действия

### <a name="install-capanalysis"></a>Установка CapAnalysis

Чтобы установить CapAnalysis на виртуальной машине, воспользуйтесь официальной инструкцией, доступной по адресу https://www.capanalysis.net/ca/how-to-install-capanalysis.
Для удаленного доступа к CapAnalysis необходимо открыть на виртуальной машине порт 9877. Для этого добавьте новое правило безопасности для входящего трафика. Дополнительные сведения о создании правил в группах безопасности сети см. в разделе [Создание правил в существующей сетевой группе безопасности](../virtual-network/manage-network-security-group.md#create-a-security-rule). Когда правило будет добавлено, вы сможете получить доступ к CapAnalysis из `http://<PublicIP>:9877`.

### <a name="use-azure-network-watcher-to-start-a-packet-capture-session"></a>Запуск сеанса захвата пакетов в Наблюдателе за сетями Azure

Наблюдатель за сетями позволяет захватить все пакеты для отслеживания входящего и исходящего трафика на виртуальной машине. Инструкции о том, как начать сеанс захвата пакетов, есть в статье [Manage packet captures with Azure Network Watcher using the portal](network-watcher-packet-capture-manage-portal.md) (Управление захватом пакетов с помощью Наблюдателя за сетями на портале). Полученный захват пакетов можно сохранить в BLOB-объекте хранилища, к которому затем получит доступ CapAnalysis.

### <a name="upload-a-packet-capture-to-capanalysis"></a>Передача захвата пакетов в CapAnalysis
Вы можете напрямую отправить захват пакетов, полученный Наблюдателем за сетями, на вкладке "Импортировать с URL-адреса", где нужно указать ссылку на BLOB-объект хранилища, в котором хранится захват пакетов.

Передавая ссылку в CapAnalysis, не забудьте добавить маркер SAS к URL-адресу BLOB-объекта хранилища.  Чтобы создать этот маркер, откройте в учетной записи хранения подписанный URL-адрес, назначьте необходимые разрешения и нажмите кнопку "Создать SAS". Полученный маркер SAS затем можно добавить к URL-адресу захвата пакетов в BLOB-объекте хранилища.

Итоговый URL-адрес должен выглядеть следующим образом: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere


### <a name="analyzing-packet-captures"></a>Анализ захвата пакетов

CapAnalysis предлагает много возможностей для визуализации захвата пакетов, позволяющих выполнить анализ с разных точек зрения. Эти наглядные сводки помогают понять тенденции распределения сетевого трафика и быстро выявлять любые необычные действия. В следующем списке показаны некоторые из этих возможностей.

1. Таблицы потоков

    Эта таблица содержит список потоков, содержащихся в данных о пакетах, с указанием меток времени и различных протоколов, связанных с каждым потоком, а также IP-адресов источника и получателя.

    ![Страница потоков CapAnalysis][5]

1. Обзор протоколов

    Эта панель позволяет быстро просматривать распределение сетевого трафика по разным протоколам и географическим регионам.

    ![Обзор протоколов CapAnalysis][6]

1. Статистика

    Эта панель позволяет просматривать статистику сетевого трафика по таким параметрам, как количество отправленных и полученных байтов для IP-адресов источника и получателя, количество потоков для каждого IP-адреса источника и получателя, используемые в потоках протоколы и продолжительность потоков.

    ![Статистика CapAnalysis][7]

1. Географическая карта

    Эта панель содержит представление карты сетевого трафика, с помощью цветов закодирован объем трафика из каждой страны или региона. Вы можете выбрать выделенный странах и регионах для просмотра дополнительных потоках. Например долю данных отправленных и полученных с IP-адресов в этой страны или региона.

    ![Географическая карта][8]

1. Фильтры

    CapAnalysis предоставляет набор фильтров для быстрого анализа пакетов определенного типа. Например, можно фильтровать данные по протоколу, чтобы подробнее изучить распределение этого подмножества трафика.

    ![filters][11]

    Посетите [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about), чтобы узнать больше обо всех возможностях CapAnalysis.

## <a name="conclusion"></a>Заключение

Функция захвата пакетов Наблюдателя за сетями позволяет сохранять полезные данные для экспертизы сети и понимания характеристик сетевого трафика. На этом примере мы показали, как можно легко интегрировать захват пакетов Наблюдателя за сетями со средствами визуализации с открытым кодом. Используя средства с открытым кодом для визуализации захвата пакетов, например CapAnalysis, вы можете тщательно проверять сетевые пакеты и быстро выявлять тенденции трафика в сети.

## <a name="next-steps"></a>Дальнейшие действия

Информацию о журналах потоков для групп безопасности сети см. [в этой статье](network-watcher-nsg-flow-logging-overview.md).

Ознакомьтесь со статьей [Visualizing Network Security Group flow logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md) (Визуализация журналов потоков для групп безопасности сети с помощью Power BI).
<!--Image references-->

[1]: ./media/network-watcher-using-open-source-tools/figure1.png
[2]: ./media/network-watcher-using-open-source-tools/figure2.png
[3]: ./media/network-watcher-using-open-source-tools/figure3.png
[4]: ./media/network-watcher-using-open-source-tools/figure4.png
[5]: ./media/network-watcher-using-open-source-tools/figure5.png
[6]: ./media/network-watcher-using-open-source-tools/figure6.png
[7]: ./media/network-watcher-using-open-source-tools/figure7.png
[8]: ./media/network-watcher-using-open-source-tools/figure8.png
[9]: ./media/network-watcher-using-open-source-tools/figure9.png
[10]: ./media/network-watcher-using-open-source-tools/figure10.png
[11]: ./media/network-watcher-using-open-source-tools/figure11.png
