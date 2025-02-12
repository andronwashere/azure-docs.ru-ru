---
title: Учебник по принятию приглашения и получению данных с помощью службы Azure Data Share (предварительная версия)
description: Учебник по принятию приглашения и получению данных с помощью службы Azure Data Share (предварительная версия)
author: joannapea
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: fc63c1a0b3b496de8e5ecea58f79f1db9d872e80
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67838440"
---
# <a name="tutorial-accept-and-receive-data-using-azure-data-share-preview"></a>Руководство по принятию приглашения и получению данных с помощью службы Azure Data Share (предварительная версия)

В этом учебнике вы узнаете, как принять приглашение в общий ресурс данных с помощью службы Azure Data Share (предварительная версия), а также как получить данные, к которым вам предоставили доступ и включить интервал регулярного обновления, чтобы всегда иметь последний моментальный снимок этих данных. 

> [!div class="checklist"]
> * Принятие приглашения от службы Azure Data Share (предварительная версия).
> * Создание учетной записи Azure Data Share (предварительная версия).
> * Указание места назначения данных.
> * Создание подписки на общий ресурс с данными для получения запланированного обновления.

## <a name="prerequisites"></a>Предварительные требования
Чтобы принять приглашение в общий ресурс данных, вам необходимо подготовить к работе ряд ресурсов Azure, которые перечислены ниже. 

Прежде чем принять приглашение в общий ресурс данных, выполните все предварительные требования. 

* Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.
* Учетная запись хранения Azure. Если у вас еще нет учетной записи хранения Azure, вы можете [создать ее](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account). 
* Приглашение Data Share. Приглашение от Microsoft Azure с темой "Приглашение Azure Data Share от **<yourdataprovider@domain.com>** ".
* Разрешение на добавление назначения ролей в учетную запись хранения с разрешением *Microsoft.Authorization/role assignments/write*. Это разрешение назначено роли владельца. 
* Регистрация поставщика ресурсов для Microsoft.DataShare. См. подробнее о [поставщиках ресурсов Azure](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-supported-services). 

> [!IMPORTANT]
> Чтобы получить данные Azure Data Share, сначала необходимо зарегистрировать поставщик ресурсов Microsoft.DataShare и стать владельцем учетной записи хранения, где вы будете получать данные. Следуйте инструкциям, изложенным в статье [Устранение распространенных неполадок в предварительной версии Azure Data Share](data-share-troubleshoot.md), чтобы зарегистрировать поставщик ресурсов для общего ресурса данных, а также добавить себя в качестве владельца учетной записи хранения. 

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портале Azure](https://portal.azure.com/).

## <a name="open-invitation"></a>Открытие приглашения

Проверьте папку "Входящие" на наличие приглашения от вашего поставщика данных. Это должно быть приглашение от Microsoft Azure с названием **Приглашение Azure Data Share от <yourdataprovider@domain.com>** . Запишите имя общего ресурса, чтобы принять правильную общую папку при наличии нескольких приглашений. 

Выберите **View invitation** (Просмотреть приглашение), чтобы увидеть ваше приглашение в Azure. После этого откроется представление полученных общих ресурсов.

![Приглашения](./media/invitations.png "Список приглашений") 

Выберите общий ресурс для просмотра. 

## <a name="accept-invitation"></a>Принятие приглашения
Просмотрите все поля, в том числе **Условия использования**. Если вы согласны с условиями использования, установите флажок для подтверждения своего согласия. 

![Условия использования](./media/terms-of-use.png "Условия использования") 

В разделе *Target Data Share Account* (Целевая учетная запись Data Share) выберите подписку и группу ресурсов, куда будете развертывать общий ресурс данных. 

В поле **учетной записи Data Share** выберите **Создать**, если у вас нет существующей учетной записи Data Share. В противном случае выберите учетную запись Data Share, где бы вы хотели принимать данные. 

В поле *Received Share Name* (Имя полученного общего ресурса) можно оставить значение по умолчанию, заданное поставщиком данных, или указать новое имя для полученного общего ресурса. 

![Целевая учетная запись общего ресурса данных](./media/target-data-share.png "Целевая учетная запись общего ресурса данных") 

Согласившись с условиями использования и указав расположение для общего ресурса, выберите *Accept and Configure* (Принять и настроить). Если вы выбрали эту команду, будет создана подписка на общий ресурс и на следующем экране вам нужно будет выбрать целевую учетную запись хранения, куда будут копироваться данные. 

![Варианты принятия](./media/accept-options.png "Варианты принятия") 

Если вы хотите принять приглашение сейчас, но настроить хранилище позднее, выберите *Accept and Configure later* (Принять и настроить позже). Этот вариант позволяет настроить вашу целевую учетную запись хранения позже. Сведения о возобновлении настройки общего ресурса данных и конфигурации хранилища см. [на этой странице](how-to-configure-mapping.md). 

Если вы не хотите принимать приглашение, выберите *Отклонить*. 

## <a name="configure-storage"></a>Настройка хранилища
В разделе *Target Storage Settings* (Параметры целевого хранилища) выберите подписку, группу ресурсов и учетную запись хранения, где вы хотите получать свои данные. 

![Параметры целевого хранилища](./media/target-storage-settings.png "Целевое хранилище") 

Чтобы получать регулярные обновления данных, включите настройки создания моментальных снимков. Обратите внимание, что вы сможете увидеть параметры расписания создания моментальных снимком только в том случае, если ваш поставщик данных включил расписание в общий ресурс. 

![Параметры создания моментальных снимков](./media/snapshot-settings.png "Параметры создания моментальных снимков") 

Щелкните *Сохранить*. 

## <a name="trigger-a-snapshot"></a>Активация создания моментальных снимков

Вы можете активировать создание моментальных снимков, выбрав в разделе Received Shares (Полученные общие ресурсы) вкладку "Подробности", а затем — **Trigger snapshot** (Активировать создание моментальных снимков). Здесь вы сможете активировать полные или добавочные моментальные снимки ваших данных. Если вы осуществляете прием данных от поставщика в первый раз, выберите полное копирование. 

![Активация создания моментальных снимков](./media/trigger-snapshot.png "Активация создания моментальных снимков") 

Если состояние последнего запуска является *успешным*, откройте учетную запись хранения для просмотра полученных данных. 

Чтобы проверить, какая учетная запись хранения использовалась, выберите **Наборы данных**. 

![Пользовательские наборы данных](./media/consumer-datasets.png "Сопоставление пользовательских наборов данных") 

## <a name="view-history"></a>Просмотр истории
Чтобы просмотреть журнал моментальных снимков, перейдите в раздел Received Shares (Полученные общие ресурсы) -> "Журнал". Здесь вы найдете журнал всех моментальных снимков за последние 60 дней. 

## <a name="next-steps"></a>Дополнительная информация
В этом учебнике вы узнали, как принять приглашение и получить данные Azure Data Share. Дополнительные сведения о понятиях Azure Data Share см. в[ этой статье](terminology.md).