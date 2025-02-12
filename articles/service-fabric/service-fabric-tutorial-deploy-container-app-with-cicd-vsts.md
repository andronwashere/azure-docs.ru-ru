---
title: Развертывание приложения-контейнера с помощью CI/CD в кластере Azure Service Fabric
description: В этом руководстве представлены общие сведения о том, как настроить непрерывные интеграцию и развертывание для приложения-контейнера Azure Service Fabric с помощью Visual Studio Azure DevOps.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/29/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: b686ceace3679d1541e8f1a74bca7e99b81ba932
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/29/2019
ms.locfileid: "68598894"
---
# <a name="tutorial-deploy-a-container-application-with-cicd-to-a-service-fabric-cluster"></a>Руководство по Развертывание приложения-контейнера с помощью CI/CD в кластере Service Fabric

Это второе руководство из цикла. В нем описано, как настроить непрерывные интеграцию и развертывание для приложения-контейнера Azure Service Fabric с помощью Visual Studio Azure DevOps.  Вам потребуется существующее приложение Service Fabric. В качестве примера используется приложение, созданное в руководстве [по развертыванию приложения .NET из контейнера Windows в Azure Service Fabric](service-fabric-host-app-in-a-container.md).

Из второй части цикла вы узнаете, как выполнять следующие задачи:

> [!div class="checklist"]
> * Добавление проекта в систему управления версиями
> * Создание определения сборки в Visual Studio Team Explorer
> * Создание определения выпуска в Visual Studio Team Explorer
> * Автоматическое развертывание и обновление приложения

## <a name="prerequisites"></a>Предварительные требования

Перед началом работы с этим руководством выполните следующие действия:

* использовать имеющийся кластер в Azure или [создать новый с помощью данного руководства](service-fabric-tutorial-create-vnet-and-windows-cluster.md);
* [развернуть в нем контейнерное приложение](service-fabric-host-app-in-a-container.md).

## <a name="prepare-a-publish-profile"></a>Подготовка профиля публикации

Завершив [развертывание приложения-контейнера](service-fabric-host-app-in-a-container.md), вы теперь готовы настроить непрерывную интеграцию.  Прежде всего подготовьте в приложении профиль публикации, который будет использоваться в процессе развертывания под управлением Azure DevOps.  Профиль публикации следует настроить для целевого кластера, который был создан ранее.  Запустите Visual Studio и откройте существующий проект приложения Service Fabric.  Щелкните правой кнопкой мыши приложение в **обозревателе решений** и выберите **Опубликовать...**

Выберите в проекте приложения целевой профиль, который будет использоваться для рабочего процесса непрерывной интеграции, например "Облако".  Укажите конечную точку подключения кластера.  Установите флажок **Обновлять приложение**, чтобы ваше приложение обновлялось при каждом развертывании в Azure DevOps.  Нажмите ссылку **Сохранить**, чтобы сохранить параметры профиля публикации, а затем нажмите кнопку **Отменить**, чтобы закрыть диалоговое окно.

![Принудительная отправка профиля][publish-app-profile]

## <a name="share-your-visual-studio-solution-to-a-new-azure-devops-git-repo"></a>Публикация решения Visual Studio в новом репозитории Azure DevOps Git

Поместите исходные файлы приложения в командный проект Azure DevOps, чтобы создавать сборки.

Создайте новый локальный репозиторий Git, выбрав **Добавить в систему управления версиями** -> **Git** в строке состояния в правом нижнем углу Visual Studio.

В представлении **Принудительная отправка** в **Team Explorer** выберите **Опубликовать репозиторий Git** в разделе **Принудительная отправка в Azure DevOps**.

![Принудительная отправка репозитория Git][push-git-repo]

Проверьте адрес электронной почты и выберите организацию в раскрывающемся списке **Учетная запись**. Если у вас нет организации, потребуется ее создать. Введите имя репозитория и выберите **Опубликовать репозиторий**.

![Принудительная отправка репозитория Git][publish-code]

При публикации репозитория в вашей учетной записи создается новый командный проект с тем же именем, что и у локального репозитория. Чтобы создать репозиторий в существующем командном проекте, щелкните **Расширенный** рядом с **именем репозитория** и выберите командный проект. Код можно просматривать в Интернете, выбрав **Просмотреть на веб-сайте**.

## <a name="configure-continuous-delivery-with-azure-pipelines"></a>Настройка непрерывной поставки с помощью Azure Pipelines

В определении сборки Azure DevOps описывается рабочий процесс, состоящий из последовательных этапов сборки. Создайте определение сборки, которое создает пакет приложения Service Fabric и другие артефакты для развертывания в кластер Service Fabric. См. дополнительные сведения об [определениях сборки](https://www.visualstudio.com/docs/build/define/create) Azure DevOps. 

В определении выпуска Azure DevOps описывается рабочий процесс развертывания пакета приложения в кластере. При совместном использовании определение сборки и определение выпуска выполняют весь рабочий процесс начиная с исходных файлов и заканчивая запуском приложения в кластере. См. дополнительные сведения об [определениях выпуска](https://www.visualstudio.com/docs/release/author-release-definition/more-release-definition) Azure DevOps.

### <a name="create-a-build-definition"></a>Создание определения сборки

Откройте новый командный проект, перейдя в браузере по адресу https://dev.azure.com и выбрав свою организацию вслед за новым проектом. 

На панели слева выберите **Конвейеры**, а затем щелкните **Новый конвейер**.

>[!NOTE]
>Если шаблон определения сборки не отображается, выключите функцию **Новый интерфейс для создания конвейеров YAML**. Эта функция настраивается в разделе **Функции предварительной версии** учетной записи DevOps.

![Новый конвейер][new-pipeline]

Выберите **Azure Repos Git** в качестве источника, имя для командного проекта и **главную** ветвь по умолчанию или сборки, выполняемые вручную или по расписанию.  Затем нажмите кнопку **Продолжить**.

В области **Выбор шаблона** выберите шаблон **Приложение Azure Service Fabric с поддержкой Docker** и нажмите кнопку **Применить**.

![Выбор шаблона сборки][select-build-template]

В разделе **Задачи** выберите значение **Размещается в VS2017** для параметра **Пул агентов**.

![Выбор задач][task-agent-pool]

Щелкните **Tag images** (Добавление тегов к образам).

Для параметра **Тип реестра контейнеров** выберите **Реестр контейнеров Azure**. Выберите **подписку Azure**, затем щелкните **Авторизовать**. Выберите **Реестр контейнеров Azure**.

![Выбор команды Docker для добавления тегов к образам][select-tag-images]

Щелкните **Отправка образов**.

Для параметра **Тип реестра контейнеров** выберите значение **Реестр контейнеров Azure**. Выберите **подписку Azure**, затем щелкните **Авторизовать**. Выберите **Реестр контейнеров Azure**.

![Выбор команды Docker для отправки образов][select-push-images]

На вкладке **Триггеры** включите непрерывную интеграцию, установив флажок **Включить непрерывную интеграцию**. В **фильтрах ветвей** щелкните **+Добавить** и **спецификация ветви** будет по умолчанию назначена ветви **master**.

В диалоговом окне **Сохранение конвейера сборки и очереди** щелкните команду **Сохранить и поместить в очередь**, чтобы вручную запустить сборку.

![Выбор триггеров][save-and-queue]

Сборки также активируются после принудительного запуска или возврата. Чтобы проверить ход сборки, перейдите на вкладку **Сборки**.  Проверив, что сборка запускается успешно, создайте определение выпуска, которое развертывает приложение в кластер.

### <a name="create-a-release-definition"></a>Создание определения выпуска

Выберите **Конвейеры** на левой панели, затем **Выпуски** и **+ Новый конвейер**.  В области **Выбор шаблона** выберите шаблон **Развертывание Azure Service Fabric** в списке и нажмите кнопку **Применить**.

![Выбор шаблона выпуска][select-release-template]

Выберите **Задачи**, а затем **Среда 1** и **+ Создать**, чтобы добавить новое подключение к кластеру.

![Добавление подключения к кластеру][add-cluster-connection]

В представлении **Add new Service Fabric Connection** (Добавление нового подключения Service Fabric) выберите проверку подлинности **на основе сертификатов** или **Azure Active Directory**.  Укажите имя подключения "mysftestcluster" и конечную точку кластера "tcp://mysftestcluster.southcentralus.cloudapp.azure.com:19000" (или конечную точку кластера, куда выполняется развертывание).

Для проверки подлинности на основе сертификатов добавьте **отпечаток сертификата сервера**, используемого для создания кластера.  В поле **Сертификат клиента** добавьте файла сертификата клиента в кодировке Base-64. Сведения о том, как получить представление сертификата в кодировке Base-64, см. во вплывающем окне в этом поле. Добавьте также **пароль** для сертификата.  При отсутствии отдельного клиентского сертификата можно использовать сертификат кластера или сервера.

Для учетных данных Azure Active Directory добавьте **отпечаток сертификата сервера**, используемый для создания кластера, и учетные данные, которые необходимо использовать для подключения к кластеру, в полях **Имя пользователя** и **Пароль**.

Нажмите кнопку **Добавить**, чтобы сохранить подключение кластера.



В разделе "Этап агента" щелкните **Развернуть приложение Service Fabric**.
Щелкните **Параметры Docker**, а затем **Настройка параметров Docker**. Для параметра **Источник учетных данных реестра** выберите **Подключение к службе Azure Resource Manager**. Выберите **подписку Azure**.

![Агент конвейера выпуска][release-pipeline-agent]

Затем добавьте артефакт сборки в конвейер, чтобы для определения выпуска были доступны выходные данные сборки. Выберите **Конвейер** и **Артефакты**-> **+ Добавить**.  В поле **Источник (определение сборки)** выберите созданное ранее определение сборки.  Щелкните **Добавить**, чтобы сохранить артефакт сборки.

![Добавление артефакта][add-artifact]

Включите триггер непрерывного развертывания, чтобы выпуск создавался автоматически после завершения сборки. Щелкните значок молнии в артефакте, включите триггер и щелкните **Сохранить**, чтобы сохранить определение выпуска.

![Включение триггера][enable-trigger]

Чтобы создать выпуск вручную, последовательно выберите **+Выпуск** -> **Создать выпуск** -> **Создать**. Ход создания можно отслеживать на вкладке **Выпуски**.

Убедитесь, что развертывание выполнено успешно и приложение выполняется в кластере.  Откройте браузер и перейдите по ссылке [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Обратите внимание на версию приложения, в данном случае "1.0.0.20170616.3".

## <a name="commit-and-push-changes-trigger-a-release"></a>Фиксация и отправка изменений, создание выпуска

Чтобы проверить работу конвейера непрерывной интеграции, отправьте некоторые изменения кода в Azure DevOps.

При написании кода в Visual Studio изменения отслеживаются автоматически. Зафиксировать изменения в локальном репозитории Git можно, щелкнув значок ожидающих изменений (![Ожидает][pending]) в строке состояния в нижнем правом углу.

В представлении **Изменения** в Team Explorer добавьте сообщение с описанием обновления и зафиксируйте изменения.

![Фиксация всех изменений][changes]

Щелкните значок неопубликованных изменений в строке состояния (![Неопубликованные изменения][unpublished-changes]) или в представлении "Синхронизация" в Team Explorer. Выберите **Отправить**, чтобы обновить код в Azure DevOps.

![Отправка изменений][push]

При отправке изменений в Azure DevOps автоматически запускается сборка.  После создания определения сборки автоматически создается выпуск и начинается обновление приложения в кластере.

Чтобы проверить ход сборки, перейдите на вкладку **Сборки** в **Team Explorer** в Visual Studio.  Проверив, что сборка запускается успешно, создайте определение выпуска, которое развертывает приложение в кластер.

Убедитесь, что развертывание выполнено успешно и приложение выполняется в кластере.  Откройте браузер и перейдите по ссылке [http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/](http://mysftestcluster.southcentralus.cloudapp.azure.com:19080/Explorer/).  Обратите внимание на версию приложения, в этом случае — 1.0.0.20170815.3.

![Service Fabric Explorer][sfx1]

## <a name="update-the-application"></a>Обновление приложения

Измените код в приложении.  Сохраните и зафиксируйте изменения, выполнив указанные выше действия.

После начала обновления вы сможете отслеживать ход обновления в Service Fabric Explorer:

![Service Fabric Explorer][sfx2]

Обновление приложения может занять несколько минут. После завершения обновления будет запущена новая версия приложения.  В этом примере — 1.0.0.20170815.4.

![Service Fabric Explorer][sfx3]

## <a name="next-steps"></a>Дальнейшие действия

Из этого руководства вы узнали, как выполнить следующие задачи:

> [!div class="checklist"]
> * Добавление проекта в систему управления версиями
> * Создание определения сборки
> * Создание определения выпуска
> * Автоматическое развертывание и обновление приложения

В следующей части руководства описывается настройка [мониторинга контейнера](service-fabric-tutorial-monitoring-wincontainers.md).

<!-- Image References -->
[publish-app-profile]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/PublishAppProfile.png
[push-git-repo]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/PublishGitRepo.png
[publish-code]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/PublishCode.png
[new-pipeline]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/NewPipeline.png
[select-build-template]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SelectBuildTemplate.png
[task-agent-pool]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/TaskAgentPool.png
[save-and-queue]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SaveAndQueue.png
[select-tag-images]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/DockerTagImages.png
[select-push-images]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/DockerPushImages.png
[select-release-template]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SelectReleaseTemplate.png
[set-continuous-integration]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SetContinuousIntegration.png
[add-cluster-connection]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/AddClusterConnection.png
[release-pipeline-agent]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/ReleasePipelineAgent.png
[add-artifact]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/AddArtifact.png
[enable-trigger]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/EnableTrigger.png
[sfx1]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SFX1.png
[sfx2]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SFX2.png
[sfx3]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/SFX3.png
[pending]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/Pending.png
[changes]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/Changes.png
[unpublished-changes]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/UnpublishedChanges.png
[push]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/Push.png
[continuous-delivery-with-VSTS]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/VSTS-Dialog.png
[new-service-endpoint]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/NewServiceEndpoint.png
[new-service-endpoint-dialog]: ./media/service-fabric-tutorial-deploy-container-app-with-cicd-vsts/NewServiceEndpointDialog.png
