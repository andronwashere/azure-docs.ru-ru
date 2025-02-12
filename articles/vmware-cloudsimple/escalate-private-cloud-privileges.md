---
title: Повышения прав частное облако — решение Azure VMware, CloudSimple
description: Описывает, как для повышения прав в вашем частном облаке, для выполнения административных функций в vCenter
author: sharaths-cs
ms.author: b-shsury
ms.date: 06/05/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d11c88b91b13cca13120a9203e376fdc2c3d6d8d
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2019
ms.locfileid: "67332580"
---
# <a name="escalate-private-cloud-vcenter-privileges-from-the-cloudsimple-portal"></a>Повышения прав vCenter частного облака на портале CloudSimple 

Для административного доступа к vCenter, частного облака могут временно сообщать о ваши права доступа CloudSimple.  С использованием повышенных привилегий, можно установить решения VMware, Добавление источников идентификации и управления пользователями.

Новых пользователей можно создать в домене единого входа vCenter и получает доступ к vCenter.  При создании новых пользователей, добавьте их в CloudSimple встроенные группы для доступа к vCenter.  Дополнительные сведения см. в разделе [модель разрешений CloudSimple частного облака из VMware vCenter](https://docs.azure.cloudsimple.com/learn-private-cloud-permissions/).

> [!CAUTION]
> Не вносите никаких изменений конфигурации для управления компонентами. Действия, предпринятые во время эскалированных привилегированном состоянии может отрицательно влиять на работу системы, или может привести к недоступности вашей системе.

## <a name="sign-in-to-azure"></a>Вход в Azure

Войдите на портал Azure по адресу [https://portal.azure.com](https://portal.azure.com).

## <a name="escalate-privileges"></a>Эскалация привилегий

1. Доступ [CloudSimple портала](access-cloudsimple-portal.md).

2. Откройте **ресурсы** выберите частное облако, для которого вы хотите повысить уровень прав.

3. В нижней части странице "Сводка" в разделе **изменить права vSphere**, нажмите кнопку **эскалировать**.

    ![Изменение привилегий vSphere](media/escalate-private-cloud-privilege.png)

4. Выберите тип пользователя vSphere.  Только **CloudOwner@cloudsimple.local** локального пользователя может быть расширена.

5. Выберите time interval эскалировать из раскрывающегося списка. Выберите кратчайший срок, который позволит вам для выполнения задачи.

6. Установите флажок, чтобы подтвердить, что вы понимаете риски.

    ![Повышения привилегий диалоговое окно](media/escalate-private-cloud-privilege-dialog.png)

7. Последовательно выберите **ОК**.

8. Распространение может занять несколько минут. По завершении нажмите кнопку **ОК**.

Повышение привилегий, начинается и продолжается до окончания выбранного периода времени.  Вы можете войти программу vCenter частного облака для выполнения административных задач.

> [!IMPORTANT]
> Только один пользователь может повышенными правами доступа.  Отзыв должен повышения прав пользователя, прежде чем вы можете повысить привилегии другого пользователя.

## <a name="extend-privilege-escalation"></a>Расширить повышение привилегий

Если требуется дополнительное время для выполнения задач, можно продлить период эскалации привилегий.  Выберите дополнительный укрупнить интервал времени, которая позволяет выполнять административные задачи.

1. На **ресурсы** > **частных облаков** на портале CloudSimple выберите частное облако, для которого вы хотите расширить повышения привилегий.

2. В нижней части вкладки сводки щелкните **расширить повышения привилегий**.

    ![Расширить повышение привилегий](media/de-escalate-private-cloud-privilege.png)

3. Выберите интервал времени эскалировать из раскрывающегося списка. Просмотрите новое время окончания эскалации.

4. Нажмите кнопку **Сохранить** для расширения интервал.

## <a name="de-escalate-privileges"></a>Отмена повышения прав

После завершения задач администрирования отмены нужно передать ваши права доступа.  

1. На **ресурсы** > **частных облаков** на портале CloudSimple выберите частное облако, для которого вы хотите отменить повышения прав.

2. Нажмите кнопку **отмены укрупнить**.

3. Последовательно выберите **ОК**.

> [!IMPORTANT]
> Чтобы избежать ошибок, выйдите из vCenter и войдите снова после отмены эскалация привилегий.

## <a name="next-steps"></a>Дальнейшие действия

* [Настройка источников идентификации vCenter для использования Active Directory](https://docs.azure.cloudsimple.com/set-vcenter-identity/)
* Установить решение для резервного копирования для [резервное копирование рабочей нагрузки виртуальных машин](https://docs.azure.cloudsimple.com/backup-workloads-veeam/)