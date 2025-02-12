---
title: Управление учетными записями лабораторий в Службе лабораторий Azure | Документация Майкрософт
description: Подробные сведения о том, как создавать, просматривать и удалять учетные записи лабораторий в подписке Azure.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: 6f283ce007e96547e01a01a3753ddcb60574bfc3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/13/2019
ms.locfileid: "65412804"
---
# <a name="manage-lab-accounts-in-azure-lab-services"></a>Управление учетными записями лабораторий в Службах лабораторий Azure 
В службах Azure лаборатории учетной записи лаборатории — это контейнер для лаборатории управляемых типов, таких как лабораторные занятия в аудитории. Администратор задает учетную запись лаборатории с помощью Служб лабораторий Azure и предоставляет доступ владельцам лабораторий, которые могут создать лаборатории в учетной записи. В этой статье содержатся подробные сведения о том, как создавать, просматривать и удалять учетные записи лабораторий.

## <a name="create-a-lab-account"></a>Создание учетной записи лаборатории
Ниже показано, как на портале Azure создать учетную запись лаборатории с помощью Служб лабораторий Azure. 

1. Войдите на [портале Azure](https://portal.azure.com).
2. В меню слева выберите **Все службы**. Выберите **Учетные записи лабораторий** в разделе **DEVOPS**. При выборе звездочки (`*`) рядом с полем **Учетные записи лабораторий**, добавляется в раздел **Избранное** в меню слева. В следующий раз выберите **Учетные записи лабораторий** в разделе **Избранное**.

    !["Все службы" -> "Учетные записи лабораторий"](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. На странице **Учетные записи лабораторий** на панели инструментов выберите **Добавить**. 

    ![Выбор "Добавить" на странице "Учетные записи лабораторий"](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. На странице **Учетная запись лаборатории** сделайте следующее: 
    1. В поле **Lab account name** (Имя учетной записи лаборатории) введите имя. 
    2. Выберите **подписку Azure**, в которой нужно создать учетную запись лаборатории.
    3. Для **группы ресурсов** выберите **Создать** и введите имя группы ресурсов.
    4. В поле **Расположение** выберите расположение или регион, в котором нужно создать учетную запись лаборатории. 
    5. Выберите существующую **общедоступную коллекцию образов** или создайте новый образ. Шаблон виртуальной машины можно сохранить в общедоступной коллекции образов, чтобы его могли использовать другие пользователи. Подробные сведения об общедоступных коллекциях образов см. в разделе об [использовании общедоступной коллекции образов в Службах лабораторий Azure](how-to-use-shared-image-gallery.md).
    6. В меню **Одноранговая виртуальная сеть** выберите одноранговую виртуальную сеть (VNet) для сети лаборатории. Лаборатории, созданные в этой учетной записи, подключаются к выбранной виртуальной сети и получают доступ к ресурсам в выбранной виртуальной сети. 
    7. Укажите **диапазон адресов** для виртуальных машин в лаборатории. Диапазон адресов должен быть указан в формате нотации бесклассовой междоменной маршрутизации (CIDR) (пример: 10.20.0.0/23). В этом диапазоне адресов будут создаваться виртуальные машины в лаборатории. Дополнительные сведения см. в разделе [Укажите диапазон адресов для виртуальных машин в лаборатории](how-to-configure-lab-accounts.md#specify-an-address-range-for-vms-in-the-lab).    
    8. В поле **Allow lab creator to pick lab location** (Разрешить создателю лаборатории выбирать расположение лаборатории) укажите, нужно ли предоставить создателям лабораторий возможность выбирать расположение для лаборатории. По умолчанию этот параметр отключен. В таком случае создатели лабораторий не могут указать расположение для лабораторий, которые они создают. Лаборатории создаются в географическом расположении, которое находится ближе всего к учетной записи лаборатории. Если этот параметр включен, создатель лаборатории может выбрать расположение при создании лаборатории.      
    9. Нажмите кнопку **Создать**. 

        ![Окно создания учетной записи лаборатории](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. На панели инструментов (**Уведомления**) выберите **значок колокольчика**, подтвердите, что развертывание прошло успешно, а затем выберите **Перейти к ресурсу**. 

    Кроме того, на странице **Учетные записи лабораторий** выберите **Обновить** и выберите созданную учетную запись лаборатории. 

    ![Окно создания учетной записи лаборатории](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Отобразится следующая страница **учетной записи лаборатории**:

    ![Страница учетной записи лаборатории](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="view-lab-accounts"></a>Просмотр учетных записей лаборатории
1. Войдите на [портале Azure](https://portal.azure.com).
2. В меню выберите **Все ресурсы**. 
3. Выберите **Учетные записи лабораторий** для **типа**. 
    Вы также можете выполнять фильтрацию по подписке, группе ресурсов, расположению и тегам. 

    ![Все ресурсы -> Учетные записи лабораторий](../media/how-to-manage-lab-accounts/all-resources-lab-accounts.png)

## <a name="view-and-manage-labs-in-the-lab-account"></a>Просмотр и управление лабораториями в учетной записи лаборатории

1. На странице **Учетная запись лаборатории** в меню слева выберите **Лаборатории**.

    ![Лаборатории учетной записи](../media/how-to-manage-lab-accounts/labs-in-account.png)
1. В учетной записи вы увидите **список лабораторий** с указанной следующей информацией: 
    1. Имя лаборатории.
    2. Дата создания лаборатории. 
    3. Адрес электронной почты пользователя, создавшего лабораторию. 
    4. Максимальное число пользователей, которые могут работать в лаборатории. 
    5. Статус лаборатории. 

## <a name="delete-a-lab-in-the-lab-account"></a>Удаление лаборатории в учетной записи лаборатории
Следуйте инструкциям в предыдущем разделе, чтобы просмотреть список лаборатории в учетной записи лаборатории.

1. Выберите **... (многоточие)** и выберите **Удалить**. 

    ![Кнопка удаления лаборатории](../media/how-to-manage-lab-accounts/delete-lab-button.png)
2. Нажмите в предупреждающем сообщении **Да**. 

    ![Подтверждение удаления лаборатории](../media/how-to-manage-lab-accounts/confirm-lab-delete.png)

## <a name="delete-a-lab-account"></a>Удаление учетной записи лаборатории
Следуйте инструкциям из предыдущего раздела, в котором учетные записи лаборатории отображаются в виде списка. Чтобы удалить учетную запись лаборатории, сделайте следующее: 

1. Выберите **учетную запись лаборатории**, которую нужно удалить. 
2. На панели инструментов выберите **Удалить**. 

    ![Учетные записи лабораторий -> кнопка "Удалить"](../media/how-to-manage-lab-accounts/delete-button.png)
1. Для подтверждения действия щелкните **Да**.
1. Нажмите кнопку **Удалить**. 

    ![Подтверждение удаления учетной записи лаборатории](../media/how-to-manage-lab-accounts/delete-lab-account-confirmation.png)


## <a name="next-steps"></a>Дальнейшие действия
Ознакомьтесь со следующей статьей: [Настройка учетных записей лабораторий](how-to-configure-lab-accounts.md).