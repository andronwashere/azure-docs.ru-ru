---
title: Обеспечение высокого уровня доступности с помощью Apache Kafka в Azure HDInsight
description: Узнайте, как обеспечить высокий уровень доступности с помощью Apache Kafka в Azure HDInsight. Узнайте, как перераспределять реплики секций в Kafka, чтобы в случае сбоя они находились в разных доменах в регионе Azure, в котором размещена служба HDInsight.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/01/2018
ms.openlocfilehash: d570cdf32ccf0f7037fd772f71a4296904ba7921
ms.sourcegitcommit: fa45c2bcd1b32bc8dd54a5dc8bc206d2fe23d5fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2019
ms.locfileid: "67849096"
---
# <a name="high-availability-of-your-data-with-apache-kafka-on-hdinsight"></a>Обеспечение высокого уровня доступности данных с помощью Apache Kafka в HDInsight

Узнайте, как настроить реплики секций для разделов Apache Kafka, чтобы воспользоваться преимуществами конфигураций базовых аппаратных стоек. Эта конфигурация обеспечивает доступность данных, хранящихся в Apache Kafka в HDInsight.

## <a name="fault-and-update-domains-with-apache-kafka"></a>Домены сбоя и обновления с Apache Kafka

Домен сбоя — это логическое объединение базового оборудования в центре обработки данных Azure. Все домены сбоя используют общий источник питания и сетевой коммутатор. Виртуальные машины и управляемые диски, на которых реализуются узлы в кластере HDInsight, распределяются по этим доменам сбоя. Такая архитектура ограничивает потенциальное влияние сбоев физического оборудования.

В каждом регионе Azure есть определенное количество доменов сбоя. Список доменов и количество доменов сбоя в них см. в документации [о группах доступности](../../virtual-machines/windows/availability.md#availability-sets).

> [!IMPORTANT]  
> В Kafka нет сведений о доменах сбоя. При создании раздела в Kafka все реплики секций могут храниться в одном домене сбоя. Чтобы решить эту проблему, HDInsight предоставляет [средство перераспределения секций Kafka](https://github.com/hdinsight/hdinsight-kafka-tools).

## <a name="when-to-rebalance-partition-replicas"></a>Когда следует перераспределять реплики секций?

Чтобы обеспечить максимально высокий уровень доступности данных Kafka, следует перераспределять реплики секций для раздела в следующих случаях:

* при создании раздела или секции;

* при масштабировании кластера.

## <a name="replication-factor"></a>Коэффициент репликации

> [!IMPORTANT]  
> Мы рекомендуем использовать регион Azure с тремя доменами сбоя и коэффициент репликации 3.

Если необходимо указать регион с двумя доменами сбоя, используйте коэффициент репликации 4, чтобы равномерно распределить реплики на этих доменах.

Примеры создания разделов и настройки коэффициента репликации см. в статье [Краткое руководство по созданию Apache Kafka в кластере HDInsight](apache-kafka-get-started.md).

## <a name="how-to-rebalance-partition-replicas"></a>Как перераспределять реплики секций?

Воспользуйтесь [средством перераспределения секций Apache Kafka](https://github.com/hdinsight/hdinsight-kafka-tools), чтобы перераспределить выбранные разделы. Это средство следует запускать из сеанса SSH на главном узле кластера Kafka.

Дополнительные сведения о подключении к HDInsight с помощью SSH см. в статье [Подключение к HDInsight (Hadoop) с помощью SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="next-steps"></a>Следующие шаги

* [Настройка объема хранилища и уровня масштабируемости для Apache Kafka в HDInsight](apache-kafka-scalability.md)
* [Репликация разделов Apache Kafka с помощью Kafka в HDInsight и MirrorMaker](apache-kafka-mirroring.md)
