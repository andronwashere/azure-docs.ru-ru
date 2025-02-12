---
title: Подготовка виртуальных машин Hyper-V для оценки и миграции в Azure с помощью службы "Миграция Azure" | Документация Майкрософт
description: Описание подготовки для оценки и миграции виртуальных машин Hyper-V в Azure с помощью службы "Миграция Azure".
author: rayne-wiselman
manager: carmonm
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 07/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 9e0d29770aa36f8e79bf08b7c5435ea2dbc4ae38
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67840369"
---
# <a name="prepare-for-assessment-and-migration-of-hyper-v-vms-to-azure"></a>Подготовка виртуальных машин Hyper-V к оценке и переносу в Azure

Из этой статьи можно узнать, как выполнить подготовку для оценки и миграции локальных виртуальных машин Hyper-V в Azure с помощью службы [Миграция Azure](migrate-services-overview.md).

Служба [Миграция Azure](migrate-overview.md) объединяет в себе инструменты, которые используются для поиска, оценки и переноса приложений, инфраструктуры и рабочих нагрузок в Microsoft Azure. Она включает в себя инструменты Миграции Azure и предложения независимых поставщиков программного обеспечения. 

Это руководство является первым в серии, в которой показано, как оценивать и переносить виртуальные машины Hyper-V в Azure. Из этого руководства вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Подготовьте Azure. Настройте разрешения для своей учетной записи Azure и ресурсов для работы с Миграцией Azure.
> * Подготовьте локальные хосты Hyper-V и виртуальные машины для оценки сервера.
> * Подготовьте локальные хосты Hyper-V и виртуальные машины для миграции сервера.


> [!NOTE]
> В руководствах показан простейший путь развертывания сценария, использование которого позволяет быстро настроить проверку концепции. В руководствах по возможности используются параметры по умолчанию и показаны не все возможные параметры и пути. Для получения подробных инструкций ознакомьтесь со статьей о выполнении оценки и миграции Hyper-V.


Если у вас еще нет подписки Azure, [создайте бесплатную учетную запись Azure](https://azure.microsoft.com/pricing/free-trial/), прежде чем начинать работу.


## <a name="prepare-azure"></a>Подготовка Azure

### <a name="azure-permissions"></a>Разрешения Azure

Для развертывания Миграции Azure вам потребуется несколько разрешений:

- В учетной записи Azure необходимы разрешения на создание проекта миграции Azure для оценки и миграции. 
- В учетной записи Azure необходимы разрешения на регистрацию устройства Миграции Azure.
    - Для оценки Миграция Azure запускает облегченное устройство, которое обнаруживает виртуальные машины Hyper-V и отправляет метаданные виртуальной машины и данные о производительности обратно в службу Миграции Azure.
    - Во время регистрации устройства служба Миграции Azure создает два приложения Azure Active Directory (Azure AD), которые однозначно идентифицируют устройство.
        - Первое приложение взаимодействует с конечными точками службы "Миграция Azure".
        - Второе приложение обращается к хранилищу ключей Azure, созданному во время регистрации, для хранения информации приложения Azure AD и параметров конфигурации устройства.
    - Вы можете назначить разрешения для Миграции Azure для создания этих приложений Azure AD одним из следующих способов:
        - Администратор клиента и глобальный администратор могут предоставлять разрешения пользователям в клиенте для создания и регистрации приложений Azure AD.
        - Администратор клиента и глобальный администратор могут назначить роль разработчика приложений (которая уже имеет нужные разрешения).
    - Стоит отметить, что:
        - Приложения не имеют других разрешений на доступ к подписке, помимо описанных выше.
        - Эти разрешения требуются только для регистрации нового устройства. После настройки устройства разрешения можно удалить. 


### <a name="assign-permissions-to-create-project"></a>Назначение разрешений для создания проекта

Убедитесь, что у вас есть разрешения на создание приложений проекта Миграции Azure.

1. На портале Azure откройте подписку, а затем выберите **Управление доступом (IAM)** .
2. В разделе **Проверить доступ** найдите соответствующую учетную запись и щелкните ее, чтобы просмотреть разрешения.
3. Вы должны обладать разрешениями уровня **Участник** или **Владелец**.
    - Если вы создали бесплатную учетную запись Azure, вы являетесь владельцем подписки.
    - Если вы не являетесь владельцем подписки, назначьте эту роль совместно с владельцем.


### <a name="assign-permissions-to-register-the-appliance"></a>Назначение разрешений для регистрации устройства

Если вы развертываете устройство службы "Миграция Azure" для оценки виртуальных машин, вам необходимо зарегистрировать его.

- Во время регистрации устройства служба "Миграция Azure" создает два приложения Azure Active Directory (Azure AD), которые однозначно идентифицируют устройство.
    - Первое приложение взаимодействует с конечными точками службы "Миграция Azure".
    - Второе приложение обращается к хранилищу ключей Azure, созданному во время регистрации, для хранения информации приложения Azure AD и параметров конфигурации устройства.
- Вы можете назначить разрешения для Миграции Azure для создания этих приложений Azure AD одним из следующих способов:
    - Администратор клиента и глобальный администратор могут предоставлять разрешения пользователям в клиенте для создания и регистрации приложений Azure AD.
    - Администратор клиента и глобальный администратор могут назначить роль разработчика приложений (которая уже имеет нужные разрешения).

Стоит отметить, что:

- Приложения не имеют других разрешений на доступ к подписке, помимо описанных выше.
- Эти разрешения требуются только для регистрации нового устройства. После настройки устройства разрешения можно удалить. 


#### <a name="grant-account-permissions"></a>Предоставление разрешений учетной записи

Администратор клиента и глобальный администратор могут предоставить разрешения, выполнив следующие действия:

1. В Azure AD глобальному администратору или администратору клиента нужно перейти в раздел **Azure Active Directory** > **Пользователи** > **Параметры пользователей**.
2. Затем для параметра **Регистрация приложений** нужно выбрать вариант **Да**.

    ![Разрешения Azure AD](./media/tutorial-prepare-hyper-v/aad.png)

> [!NOTE]
> Это значение по умолчанию, которое разрешает выполнять регистрацию. [Узнайте больше](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).



#### <a name="assign-application-developer-role"></a>Назначение роли разработчика приложения 

Администратор клиента и глобальный администратор могут назначить учетной записи роль разработчика приложений. [Узнайте больше](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).


## <a name="prepare-for-hyper-v-assessment"></a>Подготовка к оценке Hyper-V

Чтобы подготовиться к оценке Hyper-V, необходимо проверить параметры хоста и виртуальной машины Hyper-V, а также параметры развертывания устройства.

### <a name="verify-hyper-v-host-settings"></a>Проверьте настройки узла Hyper-V

1. Проверьте [требования к узлу Hyper-V](migrate-support-matrix-hyper-v.md#assessment-hyper-v-host-requirements) для оценки сервера.
2. Убедитесь, что [необходимые порты](migrate-support-matrix-hyper-v.md#assessment-port-requirements) открыты на узлах Hyper-V.

### <a name="enable-powershell-remoting-on-hosts"></a>Включение удаленного взаимодействия PowerShell на узлах

Настройте удаленное взаимодействие PowerShell на каждом узле следующим образом:

1. На каждом узле откройте консоль PowerShell от имени администратора.
2. Выполните следующую команду:

    ```
    Enable-PSRemoting -force
    ```

### <a name="enable-credssp-on-hosts"></a>Включение CredSSP на узлах

Если диски виртуальной машины расположены на общих ресурсах SMB, выполните этот шаг на каждом соответствующем узле Hyper-V. Этот шаг используется для поиска информации о конфигурации виртуальных машин Hyper-V с дисками на общих ресурсах SMB. Если у вас нет дисков виртуальных машин на общих ресурсах SMB, вы можете пропустить этот шаг.

1. Определите узлы Hyper-V, на которых выполняются виртуальные машины Hyper-V с дисками на общих ресурсах SMB.
2. Выполните следующую команду на каждом указанном узле Hyper-V:

    ```
    Enable-WSManCredSSP -Role Server -Force
    ```

- Проверка подлинности CredSSP позволяет узлу Hyper-V делегировать учетные данные от имени клиента Миграции Azure.
- Вы можете запустить эту команду удаленно на всех узлах Hyper-V.
- Если вы добавляете новые главные узлы в кластер, они автоматически добавляются для обнаружения, но вам необходимо вручную включить CredSSP на новых узлах, если это необходимо.

### <a name="verify-appliance-settings"></a>Проверка настроек устройства

Перед настройкой устройства Миграции Azure и началом оценки из следующего руководства вам следует подготовиться к развертыванию устройства.

1. [Проверьте](migrate-support-matrix-hyper-v.md#assessment-appliance-requirements) требования к устройству.
2. [Просмотрите](migrate-support-matrix-hyper-v.md#assessment-appliance-url-access) URL-адреса Azure, к которым устройству потребуется доступ.
3. Просмотрите данные, собранные устройством во время обнаружения и оценки.
4. [Обратите внимание](migrate-support-matrix-hyper-v.md#assessment-port-requirements) на требования доступа к порту для устройства.


### <a name="set-up-an-account-for-vm-discovery"></a>Настройка учетной записи для обнаружения виртуальной машины

Миграции Azure необходимы разрешения для обнаружения локальных виртуальных машин.

- Настройте учетную запись домена или локального пользователя с правами администратора на узлах и в кластере Hyper-V.

    - Вам нужна единая учетная запись для всех узлов и кластеров, которые вы хотите включить в обнаружение.
    - Учетная запись может быть локальной или доменной. Мы советуем, чтобы учетной записи были назначены разрешения администратора на узлах или кластерах Hyper-V.
    - Кроме того, если вы не хотите назначать права администратора, необходимы следующие разрешения:
        - пользователи удаленного управления;
        - администраторы Hyper-V;
        - пользователи системного монитора.

### <a name="enable-integration-services-on-vms"></a>Включение Integration Services на виртуальных машинах

Integration Services должны быть включены на каждой виртуальной машине, чтобы Миграция Azure могла собирать информацию об операционной системе на виртуальной машине.

- На виртуальных машинах, которые вы хотите обнаружить и оценить, включите [Hyper-V Integration Services](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-integration-services) на каждой виртуальной машине. 

## <a name="prepare-for-hyper-v-migration"></a>Подготовка к миграции Hyper-V

1. [Просмотрите](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-requirements) требования к узлу Hyper-V для миграции.
2. [Просмотрите требования](migrate-support-matrix-hyper-v.md#migration-hyper-v-vm-requirements) для виртуальных машин Hyper-V, которые вы хотите перенести в Azure.
3. [Запишите](migrate-support-matrix-hyper-v.md#migration-hyper-v-host-url-access) URL-адреса Azure, к которым узлам и кластерам Hyper-V необходим доступ для миграции виртуальных машин.

## <a name="next-steps"></a>Дополнительная информация

Изучив это руководство, вы:
 
> [!div class="checklist"] 
> * Настроили разрешения для учетной записи Azure.
> * Подготовили виртуальные машины и узлы Hyper-V к оценке и переносу.

Перейдите к следующему руководству по созданию проекта миграции Azure и оцените виртуальные машины Hyper-V для миграции на Azure.

> [!div class="nextstepaction"] 
> [Оценка виртуальных машин Hyper-V с помощью средства "Оценка сервера" службы "Миграция Azure"](./tutorial-assess-hyper-v.md) 
