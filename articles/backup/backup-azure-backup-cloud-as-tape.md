---
title: Использование службы архивации Azure для замены ленточной инфраструктуры
description: Узнайте, как служба архивации Azure предоставляет ленточную семантику, которая дает возможность выполнять резервное копирование и восстановление данных в Azure
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 04/30/2017
ms.author: dacurwin
ms.openlocfilehash: d768f0fae9487a555f6ace12303f8a4bd7cb8bd1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65146021"
---
# <a name="move-your-long-term-storage-from-tape-to-the-azure-cloud"></a>Перенос долгосрочного хранения данных с магнитной ленты в облако Azure
Пользователи службы архивации Azure и System Center Data Protection Manager могут:

* создавать резервные копии данных по расписанию, которое лучше всего соответствует потребностям организации;
* хранить резервные копии данных в течение более длительного времени;
* сделать Azure одним из средств длительного хранения данных (вместо магнитных лент).

В этой статье объясняется, как клиенты могут активировать политики резервного копирования и хранения. У клиентов, использующих для длительного хранения магнитные ленты, теперь появилась мощная и эффективная альтернатива. Эта функция активирована в последнем выпуске службы архивации Azure (доступен [здесь](https://aka.ms/azurebackup_agent)). Клиентам, использующим решение System Center DPM, необходимо обновить его по крайней мере до DPM 2012 R2 UR5, прежде чем использовать DPM в службе архивации Azure.

## <a name="what-is-the-backup-schedule"></a>Что такое расписание резервного копирования
Расписание резервного копирования определяет частоту выполнения операции резервного копирования. Например, параметры на показанном ниже экране означают, что резервные копии будут создаваться каждый день в 18:00 и в полночь.

![Ежедневное расписание](./media/backup-azure-backup-cloud-as-tape/dailybackupschedule.png)

Создание резервных копий может быть и еженедельным. Например, параметры на показанном ниже экране означают, что резервные копии будут создаваться каждую среду и воскресенье в 09:30 и в 01:00.

![Еженедельное расписание](./media/backup-azure-backup-cloud-as-tape/weeklybackupschedule.png)

## <a name="what-is-the-retention-policy"></a>Что такое политика хранения?
Политика хранения определяет время, в течение которого должна храниться резервная копия. Вместо того, чтобы настраивать одну политику для всех точек резервного копирования, клиенты могут настроить разные политики хранения на основе времени создания резервной копии. Например, точка ежедневной архивации (которая служит точкой оперативного восстановления) хранится в течение 90 дней. Точка архивации, создаваемая в конце каждого квартала для аудита, хранится дольше.

![Политика хранения](./media/backup-azure-backup-cloud-as-tape/retentionpolicy.png)

Общее число точек хранения, указанное в этой политике, равно 90 (ежедневные точки) + 40 (одна для каждого квартала в течение 10 лет) = 130.

## <a name="example--putting-both-together"></a>Пример объединения обеих точек восстановления
![Образец экрана](./media/backup-azure-backup-cloud-as-tape/samplescreen.png)

1. **Ежедневная политика хранения**: Резервные копии, создаваемые ежедневно, хранятся в течение семи дней.
2. **Еженедельная политика хранения**: Резервные копии, создаваемые каждый день в полночь и 18: 00 субботы, хранятся в течение четырех недель
3. **Ежемесячная политика хранения**: Резервные копии, создаваемые в полночь и 18: 00 в последнюю субботу каждого месяца, хранятся в течение 12 месяцев
4. **Ежегодная политика хранения**: Резервные копии, создаваемые в полночь в последнюю субботу каждого марта, хранятся в течение 10 лет

Общее число точек хранения (точки, из которых клиент может восстановить данные) в приведенной выше диаграмме вычисляется следующим образом:

* две точки в день в течение 7 дней = 14 точек восстановления;
* две точки в неделю в течение 4 недель = 8 точек восстановления;
* две точки в месяц в течение 12 месяцев = 24 точки восстановления;
* одна точка в год в течение 10 лет = 10 точек восстановления.

Общее количество точек восстановления равно 56.

> [!NOTE]
> С помощью службы архивации Azure можно создать до 9999 точек восстановления на защищенный экземпляр. Защищенный экземпляр — это компьютер, сервер (физический или виртуальный) или рабочая нагрузка, для которых настроено резервное копирование данных в Azure.
>

## <a name="advanced-configuration"></a>Расширенная конфигурация
Щелкнув элемент **Изменить** на показанном выше экране, клиенты получат доступ к дополнительным возможностям для составления расписаний хранения.

![Изменить](./media/backup-azure-backup-cloud-as-tape/modify.png)

## <a name="next-steps"></a>Следующие шаги
Дополнительные сведения о службе архивации Azure см. в статьях:

* [Общие сведения о службе архивации Azure](backup-introduction-to-azure-backup.md)
* [Знакомство со службой архивации Azure](backup-try-azure-backup-in-10-mins.md)
