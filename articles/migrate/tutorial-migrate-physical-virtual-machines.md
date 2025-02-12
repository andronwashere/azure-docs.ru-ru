---
title: Миграция локальных физических или виртуализированных машин в Azure с помощью средства "Миграция сервера" службы "Миграция Azure" | Документация Майкрософт
description: В этой статье описывается, как перенести локальные физические или виртуализированные машины в Azure с помощью средства "Миграция сервера" службы "Миграция Azure".
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/09/2019
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 728bf785edebcd17599b6a56edea1e26ed2d2fbc
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311793"
---
# <a name="migrate-physical-or-virtualized-servers-to-azure"></a>Перенос физических и виртуализированных серверов в Azure 

В этой статье показано, как перенести физические или виртуализированные серверы в Azure. Средство "Миграция сервера" службы "Миграция Azure" предлагает миграцию физических и виртуализированных серверов с использованием репликации на основе агентов. С помощью этого средства вы можете перенести в Azure самые разные компьютеры:

- Локальные физические серверы.
- Виртуальные машины, виртуализированные такими платформами, как Xen и KVM.
- Виртуальные машины Hyper-V или VMWare. Это полезно, если по какой-то причине вы не можете использовать стандартный поток миграции, который обеспечивает средство "Миграция сервера" службы "Миграция Azure" для миграции [Hyper-V](tutorial-migrate-hyper-v.md), [VMware без агента](tutorial-migrate-vmware.md) или [VMware на основе агента](tutorial-migrate-vmware-agent.md).
- Виртуальные машины, работающие в частных облаках.
- Виртуальные машины, работающие в общедоступных облаках, таких как Amazon Web Services (AWS) или Google Cloud Platform (GCP).


Служба [Миграция Azure](migrate-services-overview.md) предоставляет центральный концентратор для наблюдения за обнаружением, оценкой и миграцией локальных приложений и рабочих нагрузок, а также облачных экземпляров виртуальных машин в Azure. Эта служба предоставляет инструменты для оценки и миграции, а также предложения сторонних независимых поставщиков программного обеспечения (ISV).


Из этого руководства вы узнаете, как выполнять следующие задачи:
> [!div class="checklist"]
> * Подготовка Azure к миграции с помощью средства "Миграция сервера" службы "Миграция Azure".
> * Проверка требований к компьютерам, которые вы хотите перенести, и подготовка компьютера для устройства репликации Миграции Azure, которое используется для обнаружения и миграции компьютеров в Azure.
> * Добавление средства "Миграция сервера" службы "Миграция Azure" в концентратор Миграции Azure.
> * Настройка устройства репликации.
> * Установка службы мобильности на виртуальных компьютерах, которые нужно перенести.
> * Включите репликацию.
> * Выполнение тестовой миграции, позволяющей убедиться в правильной работе всех компонентов.
> * Выполнение полной миграции в Azure.

> [!NOTE]
> В руководствах показан простейший путь развертывания сценария, использование которого позволяет быстро настроить проверку концепции. В руководствах по возможности используются параметры по умолчанию и показаны не все возможные параметры и пути. Для получения подробных инструкций ознакомьтесь со статьями с инструкциями по Миграции Azure.

Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/pricing/free-trial/), прежде чем начинать работу.


## <a name="prerequisites"></a>Предварительные требования

Для работы с этим руководством вам потребуется:

1. [Просмотрите](migrate-architecture.md) архитектуру миграции.
2. Убедитесь, что учетной записи Azure назначена роль участника виртуальной машины, что позволит получить:

    - разрешение на создание виртуальной машины в выбранной группе ресурсов;
    - разрешение на создание виртуальной машины в выбранной виртуальной сети;
    - разрешение на запись на управляемый диск Azure. 

3. [Настройте сеть Azure](../virtual-network/manage-virtual-network.md#create-a-virtual-network). При репликации в Azure созданные виртуальные машины Azure присоединяются к сети Azure, указанной при настройке миграции.


## <a name="prepare-azure"></a>Подготовка Azure

Настройте разрешения Azure, прежде чем вы сможете выполнить миграцию с помощью средства "Миграция сервера" службы "Миграция Azure".

- **Создайте проект**. Учетная запись Azure должна иметь разрешения на создание приложений проекта Миграции Azure. 
- **Зарегистрируйте устройство репликации в службе "Миграция Azure"** . Устройство репликации создает и регистрирует приложение Azure Active Directory в вашей учетной записи Azure. Для этого делегируйте разрешения.
- **Создание Key Vault**. Чтобы выполнить миграцию виртуальных машин Миграция Azure создает Key Vault в группе ресурсов, которые используются для управления ключами доступа учетной записи хранилища репликации в подписке. Чтобы создать хранилище, вам потребуются разрешения на назначение ролей в группе ресурсов, в которой находится проект Миграции Azure. 


### <a name="assign-permissions-to-create-project"></a>Назначение разрешений для создания проекта

1. На портале Azure откройте подписку, а затем выберите **Управление доступом (IAM)** .
2. В разделе **Проверить доступ** найдите соответствующую учетную запись и щелкните ее, чтобы просмотреть разрешения.
3. Вы должны обладать разрешениями уровня **Участник** или **Владелец**.
    - Если вы создали бесплатную учетную запись Azure, вы являетесь владельцем подписки.
    - Если вы не являетесь владельцем подписки, назначьте эту роль совместно с владельцем.

### <a name="assign-permissions-to-register-the-replication-appliance"></a>Назначение разрешений для регистрации устройства репликации

Для миграции на основе агентов делегируйте разрешения на средство "Миграция сервера" службы "Миграция Azure", чтобы создать и зарегистрировать приложение Azure AD в своей учетной записи. Это можно сделать с помощью одного из следующих методов:

- Администратор клиента и глобальный администратор могут предоставлять разрешения пользователям в клиенте для создания и регистрации приложений Azure AD.
- Администратор клиента и глобальный администратор могут назначить роль разработчика приложений (которая уже имеет нужные разрешения).

Стоит отметить, что:

- Приложения не имеют других разрешений на доступ к подписке, помимо описанных выше.
- Эти разрешения требуются только для регистрации нового устройства репликации. После настройки устройства репликации разрешения можно удалить. 


#### <a name="grant-account-permissions"></a>Предоставление разрешений учетной записи

Администратор клиента и глобальный администратор могут предоставить разрешения, выполнив следующие действия:

1. В Azure AD глобальному администратору или администратору клиента нужно перейти в раздел **Azure Active Directory** > **Пользователи** > **Параметры пользователей**.
2. Затем для параметра **Регистрация приложений** нужно выбрать вариант **Да**.

    ![Разрешения Azure AD](./media/tutorial-migrate-physical-virtual-machines/aad.png)

> [!NOTE]
> Это значение по умолчанию, которое разрешает выполнять регистрацию. [Узнайте больше](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

#### <a name="assign-application-developer-role"></a>Назначение роли разработчика приложения 

Администратор клиента и глобальный администратор могут назначить учетной записи роль разработчика приложений. [Узнайте больше](../active-directory/fundamentals/active-directory-users-assign-role-azure-portal.md).

## <a name="assign-permissions-to-create-key-vault"></a>Присвоение разрешений для создания Key Vault

Назначьте разрешения на назначение ролей в группе ресурсов, в которой находится проект Миграции Azure.

1. В группе ресурсов на портале Azure выберите **Управление доступом (IAM)** .
2. В разделе **Проверить доступ** найдите соответствующую учетную запись и щелкните ее, чтобы просмотреть разрешения. Вам необходимы права **Владелец** (или **Участник** и **Администратор доступа пользователей**).
3. Если у вас нет необходимых разрешений, запросите их у владельца группы ресурсов. 

## <a name="prepare-for-migration"></a>Подготовка к переносу

### <a name="check-machine-requirements-for-migration"></a>Проверка требований к компьютеру для миграции

Убедитесь, что компьютеры соответствуют требованиям для миграции в Azure. 

> [!NOTE]
> Миграция на основе агентов с помощью средства "Миграция сервера" службы "Миграция Azure" основана на функциях службы Azure Site Recovery. Некоторые требования могут ссылаться на документацию Site Recovery.

1. [Проверьте](migrate-support-matrix-vmware.md#agent-based-migration-vmware-server-requirements) соответствие требованиям сервера VMware.
2. [Проверьте](migrate-support-matrix-vmware.md#agent-based-migration-vmware-vm-requirements) требования к виртуальным машинам для миграции.
3. Проверьте настройки виртуальной машины. Локальные виртуальные машины, которые реплицируются в Azure, должны соответствовать [требованиям к виртуальным машинам Azure](migrate-support-matrix-vmware.md#azure-vm-requirements).


### <a name="prepare-a-machine-for-the-replication-appliance"></a>Подготовка компьютера для устройства репликации

Средство "Миграция сервера" службы "Миграция Azure" использует устройство репликации для репликации компьютеров в Azure. Устройство репликации запускает следующие компоненты.

- **Сервер конфигурации** Сервер конфигурации используется для управления обменом данными между локальным источником и Azure, а также репликацией данных.
- **Сервер обработки** Сервер обработки выступает в качестве шлюза репликации. Он получает данные репликации, оптимизирует их путем кэширования, сжатия и шифрования и отправляет их в учетную запись хранения кэша Azure. 

Перед началом необходимо подготовить компьютер под управлением Windows Server 2016 для размещения устройства репликации. Компьютер должен соответствовать [таким требованиям](migrate-support-matrix-vmware.md#agent-based-migration-replication-appliance-requirements).


## <a name="add-the-azure-migrate-server-migration-tool"></a>Добавление средства "Миграция сервера" службы "Миграция Azure"

Настройте проект Миграции Azure, а затем добавьте в него средство "Миграция сервера" службы "Миграция Azure".

1. На портал Azure выберите **Все службы** и найдите службу **Миграция Azure**.
2. В разделе **Службы** выберите **Миграция Azure**.
3. В разделе **Обзор** щелкните **Оценка и миграция серверов**.
4. В области **Обнаружение, оценка и перенос серверов** щелкните **Оценка и миграция серверов**.

    ![Обнаружение и оценка серверов](./media/tutorial-migrate-physical-virtual-machines/assess-migrate.png)

5. В разделе **Обнаружение, оценка и перенос серверов** щелкните **Add tools** (Добавить инструменты).
6. В разделе **Проект миграции** выберите подписку Azure и создайте группу ресурсов, если у вас ее нет.
7. В разделе **Сведения о проекте** укажите имя проекта и регион для создания проекта и нажмите кнопку **Далее**.

    ![Создание проекта службы "Миграция Azure"](./media/tutorial-migrate-physical-virtual-machines/migrate-project.png)

    Проект службы "Миграция Azure" можно создать в следующих географических регионах.

    **География** | **Регион**
    --- | ---
    Азия | Юго-Восточная Азия
    Европа | Северная или Западная Европа
    США | Восточная или центрально-западная часть США

    Метаданные, собранные с локальных виртуальных машин, хранятся только в регионе, который задан для проекта. При выполнении миграции можно выбрать любой целевой регион.
8. В разделе **Выберите инструмент оценки** щелкните **Пропустить добавление инструмента оценки** > **Далее**.
9. В области **Выберите инструмент миграции** выберите **Azure Migrate: Server Migration** (Миграция Azure: миграция сервера) > **Далее**.
10. В области **Review + add tools** (Проверка и добавление инструментов) проверьте параметры и щелкните **Add tools** (Добавить инструменты).
11. После добавления инструмента он появится в проекте Миграции Azure в разделе **Серверы** > **Средства миграции**.

## <a name="set-up-the-replication-appliance"></a>Настройка устройства репликации

Первым шагом миграции является настройка устройства репликации. Скачайте установочный файл для устройства и запустите его на [подготовленном компьютере](#prepare-a-machine-for-the-replication-appliance). После установки устройства вы зарегистрируете его с помощью средства "Миграция сервера" службы "Миграция Azure".


### <a name="download-the-replication-appliance-installer"></a>Скачивание установщика устройства репликации

1. В проекте "Миграция Azure" щелкните **Серверы**, в разделе ***Azure Migrate: Server Migration** (Миграция Azure: миграция сервера) щелкните **Обнаружить**.

    ![Обнаружение виртуальных машин](./media/tutorial-migrate-physical-virtual-machines/migrate-discover.png)

3. В разделе**Обнаружение компьютеров** > **Ваши компьютеры виртуализированы?** выберите **Not virtualized/Other** (Не виртуализированы/Другое).
4. В поле **Целевой регион** выберите регион Azure, в который необходимо перенести компьютеры.
5. Выберите параметр **Подтвердите, что целевым регионом для миграции является "имя-региона"** .
6. Щелкните **Создать ресурсы**. После этого в фоновом режиме создается хранилище Azure Site Recovery.
    - Если вы уже настроили миграцию с помощью средства "Миграция сервера" службы "Миграция Azure", целевой параметр нельзя настроить, так как ресурсы были настроены ранее.
    - После нажатия этой кнопки вы не сможете изменить целевой регион для этого проекта.
    - Все последующие миграции будут выполняться в этот регион.

7. В разделе **Do you want to install a new replication appliance?** (Вы хотите установить новое устройство репликации?) щелкните **Установить устройство репликации**.
9. В разделе **Download and install the replication appliance software** (Скачайте и установите ПО устройства репликации) скачайте установщик устройства и регистрационный ключ. Для регистрации устройства необходим ключ. Ключ действителен в течение пяти дней после скачивания.

    ![Скачивание поставщика](media/tutorial-migrate-physical-virtual-machines/download-provider.png)

10. Скопируйте файл установки устройства и файл ключа на компьютер под управлением Windows Server 2016, который вы создали для устройства.
11. Запустите файл установки устройства репликации, как описано в следующей процедуре.
12. После настройки и перезапуска устройства на вкладке **Обнаружение компьютеров** выберите новое устройство в разделе **Выберите сервер конфигурации** и нажмите кнопку **Finalize registration** (Завершить регистрацию). Завершение регистрации выполняет пару заключительных задач по подготовке устройства репликации.

    ![Завершение регистрации](./media/tutorial-migrate-physical-virtual-machines/finalize-registration.png)

После завершения регистрации может пройти до 15 минут, пока обнаруженные виртуальные машины не отобразятся в средстве "Миграция сервера" службы "Миграция Azure". При обнаружении виртуальных машин количество **обнаруженных серверов** увеличивается.

![Обнаруженные серверы](./media/tutorial-migrate-physical-virtual-machines/discovered-servers.png)


## <a name="install-the-mobility-service"></a>Установка службы Mobility

Установите службу мобильности на виртуальных компьютерах, которые нужно перенести. Установщики агента доступны на устройстве репликации. Найдите правильный установщик и установите агент на каждой машине, которую хотите перенести. Это можно сделать указанным ниже образом.

1. Войдите на устройство репликации.
2. Перейдите по ссылке **%ProgramData%\ASR\home\svsystems\pushinstallsvc\repository**.
3. Найдите версию установщика, которая соответствует ОС компьютера. Проверьте [поддерживаемые операционные системы](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines). 
4. Скопируйте файл установщика на компьютер, который вы хотите перенести.
5. Убедитесь, что у вас есть пароль, сгенерированный при развертывании устройства.
    - Сохраните файл во временный текстовый файл на компьютере.
    - На устройстве репликации можно получить парольную фразу. В командной строке выполните **C:\ProgramData\ASR\home\svsystems\bin\genpassphrase.exe -v**, чтобы просмотреть текущую парольную фразу.
    - Не создавайте парольную фразу повторно. Это нарушит соединение и вам придется перерегистрировать устройство репликации.


### <a name="install-on-windows"></a>Установка в Windows

1. Извлеките содержимое файла установщика в локальную папку (например, C:\Temp) на компьютере следующим образом:

    ```
    ren Microsoft-ASR_UA*Windows*release.exe MobilityServiceInstaller.exe
    MobilityServiceInstaller.exe /q /x:C:\Temp\Extracted
    cd C:\Temp\Extracted
    ```
2. Запустите установщик Службы мобильности:
    ```
   UnifiedAgent.exe /Role "MS" /Silent
    ```
3. Зарегистрируйте агент на устройстве репликации:
    ```
    cd C:\Program Files (x86)\Microsoft Azure Site Recovery\agent
    UnifiedAgentConfigurator.exe  /CSEndPoint <replication appliance IP address> /PassphraseFilePath <Passphrase File Path>
    ```

### <a name="install-on-linux"></a>Установка в Linux

1. Извлеките содержимое файла tarball установщика в локальную папку (например, /tmp/MobSvcInstaller) на компьютере следующим образом:
    ```
    mkdir /tmp/MobSvcInstaller
    tar -C /tmp/MobSvcInstaller -xvf <Installer tarball>
    cd /tmp/MobSvcInstaller
    ```
2. Запустите скрипт установщика.
    ```
    sudo ./install -r MS -q
    ```
3. Зарегистрируйте агент на устройстве репликации:
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <replication appliance IP address> -P <Passphrase File Path>
    ```

## <a name="replicate-machines"></a>Репликация компьютеров

1. В проекте службы "Миграция Azure" щелкните **Серверы** и в разделе **Azure Migrate: Server Migration** (Миграция Azure: миграция сервера) выберите **Репликация**.

    ![Репликация виртуальных машин](./media/tutorial-migrate-physical-virtual-machines/select-replicate.png)

2. В разделе **Реплицировать** выберите **Параметры исходного кода** > **Ваши компьютеры виртуализированы?** и **Yes, with VMware vSphere Hypervisor** (Да, с помощью гипервизора VMware vSphere).
3. В разделе **Локальное устройство** выберите имя устройства Миграции Azure, которое вы настроили.
4. В разделе **vCenter Server** укажите имя сервера vCenter, управляющего виртуальными машинами, или сервера vSphere, на котором размещены виртуальные машины.
5. В разделе **Сервер обработки** выберите имя устройства репликации.
6. В разделе **Учетные данные гостя** укажите учетную запись администратора виртуальной машины, которая будет использоваться для принудительной установки Службы мобильности. В этом руководстве объясняется, как установить Службу мобильности вручную, чтобы можно было добавить любую фиктивную учетную запись. Выберите **Next: Виртуальные машины**.

    ![Репликация виртуальных машин](./media/tutorial-migrate-physical-virtual-machines/source-settings.png)

7. В разделе **Виртуальные машины** на вкладке **Импортировать параметры миграции из оценки?** оставьте параметр по умолчанию **No, I'll specify the migration settings manually** (Нет, я укажу параметры миграции вручную).
8. Проверьте каждую виртуальную машину, которую требуется перенести. Выберите **Next: Целевые параметры**.

    ![Выбор виртуальных машин](./media/tutorial-migrate-physical-virtual-machines/select-vms.png)


9. В разделе **Целевые параметры** выберите подписку и целевой регион, в который вы хотите выполнить миграцию, а также укажите группу ресурсов, в которой будут находиться виртуальные машины Azure после миграции.
10. В разделе **Виртуальная сеть** выберите виртуальную сеть или подсеть Azure, к которой будут подключены виртуальные машины Azure после миграции.
11. Для параметра **Преимущество гибридного использования Azure**

    - выберите вариант **Нет**, если вы не хотите применять Преимущество гибридного использования Azure. Нажмите кнопку **Далее**.
    - Выберите вариант **Да**, если у вас есть компьютеры с Windows Server, на которые распространяются активные подписки Software Assurance или Windows Server, и вы хотите применить это преимущество к компьютерам, которые вы переносите. Нажмите кнопку **Далее**.

    ![Целевые параметры](./media/tutorial-migrate-physical-virtual-machines/target-settings.png)

12. В разделе **Вычисления** просмотрите имя виртуальной машины, размер, тип диска ОС и группу доступности. Виртуальные машины должны соответствовать [требованиям Azure](migrate-support-matrix-vmware.md#azure-vm-requirements).

    - **Размер виртуальной машины**. По умолчанию средство "Миграция сервера" службы "Миграция Azure" выбирает размер в зависимости от ближайшего совпадения в подписке Azure. В качестве альтернативы выберите размер вручную в разделе **Размер виртуальной машины Azure**. 
    - **Диск ОС**: Укажите загрузочный диск ОС для виртуальной машины. Диск ОС — это диск с загрузчиком операционной системы и установщиком. 
    - **Группа доступности**. Если виртуальная машина должна находиться в группе доступности Azure после миграции, укажите эту группу. Группа должна находиться в целевой группе ресурсов, которую вы указываете для миграции.

    ![Параметры вычислений](./media/tutorial-migrate-physical-virtual-machines/compute-settings.png)

13. В разделе **Диски** укажите, следует ли реплицировать диски виртуальных машин в Azure, и выберите тип диска (SSD или HDD (цен. категория "Стандартный") или управляемые диски (цен. категория "Премиум")) в Azure. Нажмите кнопку **Далее**.
    - Диски можно исключить из репликации.
    - Если вы исключите диски, они не будут присутствовать на виртуальной машине Azure после миграции. 

    ![Параметры дисков](./media/tutorial-migrate-physical-virtual-machines/disks.png)


14. В разделе **Review and start replication** (Просмотр и запуск репликации) просмотрите параметры и нажмите кнопку **Реплицировать**, чтобы начать первоначальную репликацию серверов.

> [!NOTE]
> Вы можете обновить параметры репликации в любое время до начала репликации в разделе **Управление** > **Репликация компьютеров**. Настройки невозможно изменить после начала репликации.



## <a name="track-and-monitor"></a>Отслеживание и мониторинг

- Когда вы нажимаете кнопку **Реплицировать**, выполняется задание "Запуск репликации". 
- После успешного завершения задания "Запуск репликации" компьютеры начинают первоначальную репликацию в Azure.
- После завершения начальной репликации начинается разностная репликация. Добавочные изменения локальных дисков периодически реплицируются на диски реплики в Azure.


Отслеживать состояние задания можно в уведомлениях портала.

Вы можете отслеживать состояние репликации, щелкнув **Репликация серверов** в разделе **Azure Migrate: Server Migration** (Миграция Azure: миграция сервера).
![Мониторинг репликации](./media/tutorial-migrate-physical-virtual-machines/replicating-servers.png)

## <a name="run-a-test-migration"></a>Выполнение тестовой миграции


Когда начинается разностная репликация, вы можете запустить тестовую миграцию для виртуальных машин перед выполнением полной миграции в Azure. Мы настоятельно рекомендуем сделать это хотя бы один раз для каждой машины перед ее миграцией.

- Запуск тестовой миграции позволяет проверить, что миграция будет работать должным образом, не затрагивая локальные машины, которые остаются работоспособными и продолжают репликацию. 
- Тестовая миграция моделирует миграцию путем создания виртуальной машины Azure с использованием реплицированных данных (обычно выполняется миграция в нерабочую виртуальную сеть в вашей подписке Azure).
- Вы можете использовать реплицированную тестовую виртуальную машину Azure для проверки миграции, выполнения тестирования приложений и решения любых проблем перед полной миграцией.

Выполните тестовую миграцию следующим образом:


1. Последовательно выберите разделы **Цели миграции** > **Серверы** > **Azure Migrate: Server Migration** (Миграция Azure: миграция сервера) щелкните **Тестовая миграция серверов выполнена**.

     ![Тестовая миграция серверов](./media/tutorial-migrate-physical-virtual-machines/test-migrated-servers.png)

2. Щелкните правой кнопкой мыши виртуальную машину, чтобы выполнить тест, и выберите **Тестовая миграция**.

    ![Тестовая миграция](./media/tutorial-migrate-physical-virtual-machines/test-migrate.png)

3. В разделе **Тестовая миграция** выберите виртуальную сеть Azure, в которой будет находиться виртуальная машина Azure после миграции. Мы рекомендуем использовать нерабочую виртуальную сеть.
4. Запустится задание **Тестовая миграция**. Отслеживайте задание и уведомления на портале.
5. После завершения миграции просмотрите перенастроенную виртуальную машину Azure в разделе **Виртуальные машины** на портале Azure. Имя машины содержит суффикс **-Test**.
6. После завершения теста щелкните правой кнопкой мыши виртуальную машину Azure в разделе **Репликация компьютеров** и выберите **Очистка тестовой миграции**.

    ![Очистка миграции](./media/tutorial-migrate-physical-virtual-machines/clean-up.png)


## <a name="migrate-vms"></a>Перенос виртуальных машин

Убедившись, что тестовая миграция работает должным образом, вы можете выполнить миграцию локальных компьютеров.

1. В проекте "Миграция Azure" щелкните **Серверы** > **Azure Migrate: Server Migration** (Миграция Azure: миграция сервера) и выберите **Репликация серверов**.

    ![Репликация серверов](./media/tutorial-migrate-physical-virtual-machines/replicate-servers.png)

2. В области **Репликация компьютеров** щелкните правой кнопкой мыши виртуальную машину и выберите **Миграция**.
3. В разделе **Миграция** > **Shut down virtual machines and perform a planned migration with no data loss** (Завершить работу виртуальных машин и выполнить запланированную миграцию без потери данных?) выберите вариант **Да** > **ОК**.
    - По умолчанию Миграция Azure выключает локальную виртуальную машину и запускает репликацию по требованию для синхронизации любых изменений виртуальной машины, которые произошли с момента последней репликации. Это гарантирует отсутствие потери данных.
    - Если вы не хотите выключать виртуальную машину, выберите вариант **Нет**.
4. Запустится задание миграции виртуальной машины. Отслеживайте задание в уведомлениях Azure.
5. После завершения работы вы можете просматривать виртуальную машину и управлять ею на странице **Виртуальные машины**.

## <a name="complete-the-migration"></a>Выполнение миграции

1. После завершения миграции щелкните правой кнопкой мыши виртуальную машину и выберите **Stop migration** (Остановка миграции). Это останавливает репликацию для локального компьютера и очищает информацию о состоянии репликации виртуальной машины.
2. Установите агент виртуальной машины Azure для [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-windows) или [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) на перенесенных компьютерах.
3. Выполните любые действия по настройке после миграции приложения, такие как обновление строк подключения к базе данных и конфигурация веб-сервера.
4. Выполните приемочное тестирование конечного приложения и миграции на перенесенном приложении, работающем в Azure.
5. Остановите трафик для перенесенного экземпляра виртуальной машины Azure.
6. Удалите локальные виртуальные машины из списка локальных виртуальных машин.
7. Удалите локальные виртуальные машины из локальных заданий резервного копирования.
8. Обновите внутренние документы и отразите в них новое расположение и IP-адреса виртуальных машин Azure. 

## <a name="post-migration-best-practices"></a>Рекомендации, выполняемые после миграции

- Для повышения устойчивости:
    - Обеспечьте безопасность данных путем резервного копирования виртуальных машин Azure с помощью службы Azure Backup. [Узнайте больше](../backup/quick-backup-vm-portal.md).
    - Обеспечьте непрерывную работу и постоянную доступность рабочих нагрузок за счет репликации виртуальных машин Azure в дополнительный регион с помощью Site Recovery. [Узнайте больше](../site-recovery/azure-to-azure-tutorial-enable-replication.md).
- Для повышения уровня безопасности:
    - Заблокируйте и ограничьте доступ входящего трафика с помощью [JIT-администрирования](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) центра безопасности Azure.
    - Ограничьте сетевой трафик конечными точками с помощью [групп безопасности сети](https://docs.microsoft.com/azure/virtual-network/security-overview).
    - Разверните [шифрование дисков Azure](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-overview), чтобы обеспечить безопасность дисков и защитить данные от кражи и несанкционированного доступа.
    - Ознакомьтесь с дополнительными сведениями о [защите ресурсов IaaS](https://azure.microsoft.com/services/virtual-machines/secure-well-managed-iaas/) и посетите [центр безопасности Azure](https://azure.microsoft.com/services/security-center/).
- Для мониторинга и управления:
    - Рассмотрите возможность развертывания [службы "Управление затратами Azure"](https://docs.microsoft.com/azure/cost-management/overview) для мониторинга использования ресурсов и затрат.


## <a name="next-steps"></a>Дополнительная информация

Проанализируйте процесс [миграции в облако](https://docs.microsoft.com/azure/architecture/cloud-adoption/getting-started/migrate) в Azure Cloud Adoption Framework.
