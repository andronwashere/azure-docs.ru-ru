---
title: Создание рабочих процессов с помощью соединителя Azure IoT Central в Microsoft Flow | Документация Майкрософт
description: Использование IoT Central соединителя в Microsoft Flow для активации рабочих процессов и создать, получить, обновлять, удалять устройства и выполните команды в рабочих процессах.
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 04/25/2019
ms.topic: conceptual
ms.service: iot-central
manager: hegate
ms.openlocfilehash: e8bc8b8d4e3585ea4c0505f2e36abc6d1da7f8eb
ms.sourcegitcommit: 66237bcd9b08359a6cce8d671f846b0c93ee6a82
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67797708"
---
# <a name="build-workflows-with-the-iot-central-connector-in-microsoft-flow"></a>Создание рабочих процессов с помощью соединителя IoT Central в Microsoft Flow

*Этот раздел предназначен для разработчиков и администраторов.*

Microsoft Flow позволяет автоматизировать рабочие процессы во многих приложениях и службах, которые используют бизнес-пользователи. С помощью соединителя IoT Central в Microsoft Flow можно настроить запуск рабочих процессов при срабатывании правила в IoT Central. В рабочего процесса, запускаемого с IoT Central или любого другого приложения можно использовать действия в IoT Central connector:
- Создание устройства
- Получить сведения об устройстве
- Обновление свойств и параметров устройства
- Запуск команды на устройстве
- Удалить устройство.

Ознакомьтесь с [этими шаблонами Microsoft Flow](https://aka.ms/iotcentralflowtemplates), которые подключают IoT Central к другим службам, таким как мобильные уведомления и Microsoft Teams.

## <a name="prerequisites"></a>предварительные требования

- приложение с оплатой по мере использования;
- Microsoft персональных или рабочая или учебная учетная запись для использования Microsoft Flow ([Дополнительные сведения о планах Microsoft Flow](https://aka.ms/microsoftflowplans))
- Рабочая или учебная учетная запись для использования соединителя Azure IoT Central

## <a name="trigger-a-workflow"></a>Триггер рабочего процесса

В этом разделе показано, как активировать мобильное уведомление в мобильное приложение Flow при активации правила в IoT Central. Вы можете создать весь этот рабочий процесс в приложении IoT Central, использовании внедренного конструктора Microsoft Flow.

1. Начнем с [создавая правила в IoT Central](howto-create-telemetry-rules.md). После сохранения условия правила, выберите **действия Microsoft Flow** как новое действие. Диалоговое окно откроется для настройки рабочего процесса. Учетная запись пользователя IoT Central, которые вы вошли в будет использоваться для входа в Microsoft Flow.

    ![Создание действия Microsoft Flow](media/howto-add-microsoft-flow/createflowaction.png)

1. Вы увидите список рабочих процессов, которые имеют доступ к и вложенные в это правило IoT Central. Нажмите кнопку **ознакомьтесь с шаблонами** или **Создать > создать из шаблона** и можно выбрать любой из доступных шаблонов. 

    ![Доступные шаблоны Microsoft Flow](media/howto-add-microsoft-flow/flowtemplates1.png)

1. Вам будет предложено войти в соединители в выбранный шаблон. 

    > [!NOTE]
    > Чтобы использовать соединитель Azure IoT Central, необходимо войти с помощью учетной записи Azure Active Directory (рабочую или учебную учетную запись). Личной учетной записи, такие как abc@outlook.com или abc@live.com не поддерживаются с помощью Azure IoT Central соединителя.

    После входа в соединители, вы перейдете в конструкторе, чтобы создавать рабочие процессы. Рабочий процесс имеет триггер IoT Central, для которого уже указаны ваше приложение и правило.

1. Рабочий процесс можно настроить путем настройки сведения, передаваемые в действие и добавления новых действий. В этом примере действие — это **уведомления — отправить уведомления на мобильном**. Вы можете добавить *динамическое содержимое* из своего правила IoT Central, передав важные сведения, например имя устройства и метку времени для вашего уведомления.

    > [!NOTE]
    > Выберите **увеличить** текста в окне динамического содержимого для получения измерения и значения свойств, которые происходят в правило.

    ![Действие редактирования с открытой динамической панелью в приложение Flow](./media/howto-add-microsoft-flow/flowdynamicpane1.png)

1. После завершения редактирования действия, выберите **Сохранить**. Вы будете перенаправлены на страницу обзора рабочего процесса. Здесь вы можете просмотреть журнал выполнения и предоставить к нему доступ коллегам.

    > [!NOTE]
    > Если вы хотите, чтобы другие пользователи в вашем приложении IoT Central имели возможность редактировать это правило, нужно предоставить им доступ к нему в Microsoft Flow. Добавьте их учетные записи Microsoft Flow в качестве владельцев в рабочий процесс.

1. Если вернуться в приложение IoT Central, вы увидите, что это правило содержит действие Microsoft Flow в области действия.

Кроме того, можно создавать рабочие процессы, с помощью соединителя IoT Central непосредственно из Microsoft Flow. Затем можно выбрать какое приложение центра Интернета вещей для подключения к.

## <a name="create-a-device-in-a-workflow"></a>Создание устройства в рабочем процессе

В этом разделе показано, как создать новое устройство в IoT Central с помощью кнопки на мобильном устройстве, используя мобильное приложение Microsoft Flow. Вы можете использовать это действие в Flow для интеграции ERP-систем, таких как Dynamics, с IoT Central путем создания нового устройства, когда устройство добавлено в другое расположение.

1. Начните с создания пустого рабочего процесса в Microsoft Flow.

1. В качестве триггера найдите **кнопку потока для мобильных устройств**.

1. В этом триггере добавьте текстовое поле с именем **Имя устройства**. Измените текст описания на **Введите имя нового устройства**.

1. Добавьте новое действие. Найдите действие **Azure IoT Central - Create a device** (Azure IoT Central — Создать устройство).

1. Выберите приложение и шаблон устройства, чтобы создать устройство в раскрывающихся списках. Развернется действие, в котором будут указаны все свойства и параметры устройства.

1. Выберите поле "Имя устройства". На панели динамического содержимого выберите **Имя устройства**. Это значение передается из входных данных, пользователь вводит через мобильное приложение и имя нового устройства в центр Интернета вещей. В этом примере единственным обязательным полем является "Имя устройства", обозначенное красной звездочкой. Другой шаблон устройства может содержать несколько обязательных полей, которые необходимо заполнить для создания нового устройства.

    ![Динамическая панель действия создания устройства в приложении Flow](./media/howto-add-microsoft-flow/flowcreatedevice1.png)

1. (Необязательно.) Заполните другие поля, которые, по вашему мнению, необходимы для создания новых устройств.

1. Наконец, сохраните рабочий процесс.

1. Попробуйте запустить свой рабочий процесс в мобильном приложении Microsoft Flow. Перейдите на вкладку **Кнопки** в приложении. Вы должны увидеть область Button -> Create a new device workflow (Кнопка -> Создать рабочий процесс нового устройства). Введите имя своего нового устройства и проверьте, отображается ли оно в IoT Central.

    ![Снимок экрана создания мобильного устройства в приложении Flow](./media/howto-add-microsoft-flow/flowmobilescreenshot.png)

## <a name="update-a-device-in-a-workflow"></a>Обновление устройства в рабочем процессе

В этом разделе показано, как обновить параметры и свойства устройства в IoT Central с помощью кнопки на мобильном устройстве в мобильном приложении Microsoft Flow.

1. Начните с создания пустого рабочего процесса в Microsoft Flow.

1. В качестве триггера найдите **кнопку потока для мобильных устройств**.

1. В этом триггере добавьте входные данные, например **расположение**, которое соответствует параметру или свойству, которое вы хотите изменить. Измените описание.

1. Добавьте новое действие. Найдите действие **Azure IoT Central - Update a device** (Azure IoT Central — Обновить устройство).

1. Выберите свое приложение из раскрывающегося списка. Теперь вам потребуется идентификатор имеющегося устройства, которое вы хотите обновить. 

    > [!NOTE] 
    > **Необходимо использовать идентификатор которого находится в URL-адрес** на страницу сведений об устройстве, устройства нужно обновить. Идентификатор устройства, найти в обозреватель устройств список устройств не подходящий для использования в Microsoft Flow.

    ![Идентификатор центра Интернета вещей с URL-адреса](./media/howto-add-microsoft-flow/iotcdeviceidurl.png)

1. Вы можете обновить имя устройства. Чтобы обновить любые свойства и параметры устройства, выберите шаблон устройства, которое вы хотите обновить, в раскрывающемся списке **Device Template** (Шаблон устройства). Развернется плитка действия, на которой будут показаны все свойства и параметры, которые вы можете обновить.

    ![Обновление рабочего процесса устройства в Flow](./media/howto-add-microsoft-flow/flowupdatedevice1.png)

1. Выберите свойства и параметры, которые нужно обновить. На панели динамического содержимого выберите соответствующие входные данные из триггера. В этом примере значение "Расположение" перемещено вниз для обновления свойства расположения устройства.

1. Наконец, сохраните рабочий процесс.

1. Попробуйте запустить свой рабочий процесс в мобильном приложении Microsoft Flow. Перейдите на вкладку **Кнопки** в приложении. Вы должны увидеть область Button -> Update a device workflow (Кнопка -> Обновить рабочий процесс устройства). Введите входные данные и посмотрите, как устройство обновляется в IoT Central.

## <a name="get-device-information-in-a-workflow"></a>Получить сведения об устройстве в рабочем процессе

Можно получить сведения об устройстве с помощью его идентификатора **Azure IoT Central — получение устройства** действие. 
> [!NOTE] 
> **Необходимо использовать идентификатор которого находится в URL-адрес** на страницу сведений об устройстве, устройства нужно обновить. Идентификатор устройства, найти в обозреватель устройств список устройств не подходящий для использования в Microsoft Flow.

Можно получить сведения, такие как имя устройства, имя шаблона устройства, значения свойств и значений параметров, передаваемых последующими действиями в рабочем процессе. Ниже приведен пример рабочего процесса, передает значение свойства имени клиента с устройства в Microsoft Teams.

   ![Рабочий процесс Flow get устройств](./media/howto-add-microsoft-flow/flowgetdevice1.png)


## <a name="run-a-command-on-a-device-in-a-workflow"></a>Запуск команды на устройстве в рабочем процессе
Можно выполнить команду на устройстве с помощью его идентификатора **Azure IoT Central — выполнение команды** действие. 

> [!NOTE] 
> **Необходимо использовать идентификатор которого находится в URL-адрес** на страницу сведений об устройстве, устройства нужно обновить. Идентификатор устройства, найти в обозреватель устройств список устройств не подходящий для использования в Microsoft Flow.
    
Вы можете выбрать команду для запуска и передачи параметров команды через это действие. Ниже приведен пример рабочего процесса, которое выполняет команду перезагрузки устройства с помощью кнопки в мобильном приложении Microsoft Flow.

   ![Рабочий процесс Flow get устройств](./media/howto-add-microsoft-flow/flowrunacommand1.png)

## <a name="delete-a-device-in-a-workflow"></a>Удаление устройства в рабочем процессе

Вы сможете удалить устройство с помощью его идентификатора **Azure IoT Central — удаление устройства** действие. 
> [!NOTE] 
> **Необходимо использовать идентификатор которого находится в URL-адрес** на страницу сведений об устройстве, устройства нужно обновить. Идентификатор устройства, найти в обозреватель устройств список устройств не подходящий для использования в Microsoft Flow.

Ниже приведен пример рабочего процесса, который позволяет удалить устройство с помощью кнопки в мобильном приложении Microsoft Flow.

   ![Удаление рабочего процесса в приложении Flow](./media/howto-add-microsoft-flow/flowdeletedevice.png)

## <a name="troubleshooting"></a>Устранение неполадок

Ниже приведены несколько советов на случай, если вам не удается подключиться к соединителю Azure IoT Central.

1. Личные учетные записи Майкрософт (например, домены @hotmail.com, @live.com, @outlook.com) в настоящее время не поддерживаются. Необходимо использовать Azure Active Directory (AD) рабочую или учебную учетную запись.

2. Чтобы использовать соединитель IoT Central в Microsoft Flow, необходимо по крайней мере один раз войти в приложение IoT Central. В противном случае приложение не будет отображаться в раскрывающихся списках приложения.

3. Если ошибка возникает при использовании учетной записи Azure AD, попробуйте открыть Windows PowerShell и выполните следующие командлеты с правами администратора.

    ``` PowerShell
    Install-Module AzureAD
    Connect-AzureAD
    New-AzureADServicePrincipal -AppId 9edfcdd9-0bc5-4bd4-b287-c3afc716aac7 -DisplayName "Azure IoT Central"
    ```

## <a name="next-steps"></a>Следующие шаги

Теперь, когда вы узнали, как использовать Microsoft Flow для создания рабочих процессов, мы предлагаем — [управления устройствами](howto-manage-devices.md).

