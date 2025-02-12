---
title: Руководство по совместному использованию данных клиентами и партнерами с помощью предварительной версии Azure Data Share
description: Руководство по совместному использованию данных клиентами и партнерами с помощью предварительной версии Azure Data Share
author: joannapea
ms.service: data-share
ms.topic: tutorial
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 01888f3656765b922c1b646e7ca8e07d81e799f3
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67838421"
---
# <a name="tutorial-share-your-data-using-azure-data-share-preview"></a>Руководство по Совместное использование данных с помощью предварительной версии Azure Data Share

Из этого руководства вы узнаете, как настроить новый Azure Data Share и начать предоставлять свои данные клиентам и партнерам за пределами вашей организации Azure. 

Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * создать общий доступ к данным;
> * добавить наборы данных в общий доступ к данным;
> * включить расписание синхронизации для общего доступа к данным; 
> * добавить получателей для общего доступа к данным. 

## <a name="prerequisites"></a>Предварительные требования

* Подписка Azure. Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/free/), прежде чем начинать работу.
* Учетная запись хранилища Azure: Если у вас ее еще нет, вы можете создать [Учетную запись службі хранилища Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account)
* Разрешение на добавление назначения ролей в учетную запись хранения с разрешением *Microsoft.Authorization/role assignments/write*. Это разрешение назначено роли владельца. 
* Адрес электронной почты для входа получателей в Azure (псевдонимы электронной почты не поддерживаются).

## <a name="sign-in-to-the-azure-portal"></a>Вход на портал Azure

Войдите на [портале Azure](https://portal.azure.com/).

## <a name="create-a-data-share-account"></a>Создание учетной записи для общего доступа к данным

Создайте ресурс Azure Data Share в группе ресурсов Azure.

1. Нажмите кнопку **Создать ресурс** (+) в левом верхнем углу окна портала.

1. Найдите *Data Share*.

1. Выберите "Data Share (preview)" (Data Share (предварительная версия)) и выберите команду **Создать**.

1. Укажите основные сведения о ресурсе Azure Data Share, указав следующую информацию. 

     **Параметр** | **Рекомендуемое значение** | **Описание поля**
    |---|---|---|
    | ИМЯ | *datashareacount* | Укажите имя вашей учетной записи Data Share. |
    | Subscription | Ваша подписка | Выберите подписку Azure, которую нужно использовать для своей учетной записи Data Share.|
    | Resource group | *test-resource-group* | Воспользуйтесь существующей группой ресурсов или создайте новую. |
    | Location | *Восточная часть США 2* | Выберите регион для учетной записи Data Share.
    | | |

1. Выберите команду **Создать**, чтобы подготовить учетную запись Data Share. Подготовка новой учетной записи для общего доступа к данным обычно занимает около 2 минут или меньше. 

1. Когда развертывание будет завершено, выберите **Перейти ресурсу**.

## <a name="create-a-data-share"></a>Создание Data Share

1. Перейдите на страницу "Обзор Data Share".

    ![Совместное использование данных](./media/share-receive-data.png "совместное использование данных") 

1. Выберите команду **Начать совместное использование данных**.

1. Нажмите кнопку **Создать**.   

1. Введите сведения для Data Share. Укажите имя, описание содержимого и условия использования (необязательно). 

    ![EnterShareDetails](./media/enter-share-details.png "Введите сведения для Data Share") 

1. Выберите **Продолжить**

1. Чтобы добавить наборы данных для Data Share, выберите **Добавить наборы данных**. 

    ![Datasets](./media/datasets.png "Наборы данных")

1. Выберите тип набора данных, который хотите добавить. 

    ![AddDatasets](./media/add-datasets.png "Добавить наборы данных")    

1. Перейдите к объекту, доступ к которому нужно предоставить, и выберите "Добавить наборы данных". 

    ![SelectDatasets](./media/select-datasets.png "Выберите наборы данных")    

1. На вкладке "Получатели" введите адреса электронной почты получателей данных, выбрав команду "+Добавить получателя". 

    ![AddRecipients](./media/add-recipient.png "Добавить получателей") 

1. Выберите **Продолжить**

1. Если хотите, чтобы потребитель данных мог получать добавочные обновления ваших данных, включите создание моментальных снимков по расписанию. 

    ![EnableSnapshots](./media/enable-snapshots.png "Включить моментальные снимки") 

1. Выберите время начала и интервал повторения. 

1. Выберите **Продолжить**

1. На вкладке "Просмотр и создание" просмотрите содержимое пакета, параметры, получателей и параметры синхронизации. Нажмите кнопку **Создать**

Azure Data Share создан, и получатель Data Share готов принять ваше приглашение. 

## <a name="next-steps"></a>Дополнительная информация

Из этого руководства вы узнали, как создать Azure Data Share и пригласить получателей. Чтобы узнать больше о том, как потребитель данных может получить и принять Data Share, см. пособие о том, как [принять и получить данные](subscribe-to-data-share.md). 