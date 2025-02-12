---
title: Понятия службы подготовки устройств для Центра Интернета вещей Azure | Документация Майкрософт
description: Здесь описываются понятия подготовки в контексте подготовки устройств с помощью Службы подготовки устройств и Центра Интернета вещей
author: nberdy
ms.author: nberdy
ms.date: 04/04/2019
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 4a4f53f991355e634e8139f9e90bec6c508a527d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "60745814"
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>Понятия службы подготовки устройств для Центра Интернета вещей

Служба подготовки устройств для Центра Интернета вещей — это вспомогательная служба, позволяющая настроить полностью автоматическую подготовку устройства в указанном Центре Интернета вещей. Эта служба дает возможность [автоматически подготавливать](concepts-auto-provisioning.md) миллионы устройств и обеспечивает высокий уровень безопасности и масштабируемости.

Подготовка устройства состоит из двух этапов. На первом этапе устанавливается начальное подключение между устройством и решением Интернета вещей путем *регистрации* устройства. Второй этап включает применение правильной *конфигурации* на устройстве на основе определенных требований решения. После завершения обоих этапов устройство будет полностью *подготовлено*. Служба подготовки устройств автоматизирует оба этапа для обеспечения эффективной подготовки устройства.

В этой статье приводятся основные понятия подготовки, которые лучше всего подходят для управления *службой*. Сведения в этой статье касаются пользователей, участвующих в [этапе настройки облака](about-iot-dps.md#cloud-setup-step) по подготовке устройства для развертывания.

## <a name="service-operations-endpoint"></a>Конечная точка операций службы

Конечная точка операций службы — это конечная точка, предназначенная для управления параметрами службы и обслуживания списка регистрации. Эта конечную точку использует только администратор служб. Она не используется на устройствах.

## <a name="device-provisioning-endpoint"></a>Конечная точка подготовки устройства

Конечная точка подготовки устройства — это одна конечная точка, которую все устройства используют для автоматической подготовки. Все экземпляры службы подготовки используют один и тот же URL-адрес, что исключает необходимость перезаписывать в память устройств новые данные о подключении в сценариях цепочки поставок. Область идентификатора обеспечивает изоляцию клиентов.

## <a name="linked-iot-hubs"></a>Связанные Центры Интернета вещей

Служба подготовки устройств может подготавливать для Центров Интернета вещей только связанные с ней устройства. Связывание Центра Интернета вещей с экземпляром службы подготовки устройств предоставляет разрешения на чтение и запись к службе в реестре устройств Центра Интернета вещей. С помощью связывания служба подготовки устройств может зарегистрировать идентификатор устройства и задать начальную конфигурацию в двойнике устройства. Связанные Центры Интернета вещей могут находиться в любом регионе Azure. К службе подготовки можно привязать Центры в других подписках.

## <a name="allocation-policy"></a>Политика распределения

Параметр на уровне службы, определяющий назначение устройств службой подготовки устройств Центру Интернета вещей. Поддерживаются три политики распределения:

* **Evenly weighted distribution** (Равномерное распределение). В связанных Центрах Интернета вещей можно обеспечить равномерную подготовку устройств. Значение по умолчанию. При подготовке устройств только в одном Центре Интернета вещей можно оставить это значение.

* **Lowest latency** (Минимальная задержка). Устройства подготавливаются в Центре Интернета вещей с минимальной задержкой. Если несколько связанных Центров Интернета вещей обеспечивают одинаковую наименьшую задержку, служба подготовки хэширует устройства по этим Центрам.

* **Static configuration via the enrollment list** (Статическая конфигурация через список регистрации). Спецификация нужного Центра Интернета вещей в списке регистрации имеет приоритет над политикой распределения на уровне службы.

## <a name="enrollment"></a>Регистрация

Регистрация — это запись устройств или групп устройств, которые могут регистрироваться посредством автоматической подготовки. Запись регистрации содержит сведения об устройстве или группе устройств, в частности такие:
- используемый на устройстве [механизм аттестации](concepts-security.md#attestation-mechanism);
- предпочитаемая начальная конфигурация (необязательная информация);
- предпочитаемый Центр Интернета вещей;
- предпочитаемый идентификатор устройства.

Служба подготовки устройств поддерживает два типа регистрации.

### <a name="enrollment-group"></a>Группа регистраций

Это группа устройств, совместно использующих определенный механизм аттестации. Все устройства в группе регистраций предоставляют сертификаты X.509, подписанные одним корневым или промежуточным центром сертификации (CA). Группы регистраций могут использовать только механизм аттестации X.509. Имя группы регистрации и сертификата должно состоять из букв и цифр нижнего регистра и может содержать дефисы.

> [!TIP]
> Мы советуем использовать группу регистраций для большого количества устройств, имеющих необходимую начальную конфигурацию, или для устройств, предназначенных для одного и того же клиента.

### <a name="individual-enrollment"></a>Отдельная регистрация

Отдельная регистрация — это запись одного устройства, которое можно зарегистрировать. При аттестации таких устройств используются сертификаты X.509 конечных объектов или токены SAS (от физических или виртуальных доверенных платформенных модулей). Идентификатор регистрации в отдельной регистрации состоит из цифр и букв нижнего регистра и может содержать дефисы. Для отдельных регистраций можно указать нужный идентификатор устройства Центра Интернета вещей.

> [!TIP]
> Мы рекомендуем использовать отдельные регистрации для устройств, которым требуются уникальные начальные конфигурации, или для устройств, которые для аутентификации могут использовать только аттестацию с помощью доверенного платформенного модуля и токена SAS.

## <a name="registration"></a>Регистрация

Регистрация — это запись успешной регистрации или подготовки устройства в Центре Интернета вещей через службу подготовки устройств. Записи регистрации создаются автоматически. Вы можете их удалить, но не обновить.

## <a name="operations"></a>Операции

Операции представляют собой единицы выставления счетов Службы подготовки устройств. Операцией является успешное выполнение одной инструкции в службе. К операциям относятся регистрация и повторная регистрация устройств, а также изменения на стороне службы, например добавление записей в список регистрации и их обновление.
