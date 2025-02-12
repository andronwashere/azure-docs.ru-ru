---
title: Вопросы и ответы о службе приложений на платформе Linux в Azure | Документация Майкрософт
description: 'Служба приложений Azure на платформе Linux: вопросы и ответы.'
keywords: служба приложений azure, веб-приложение, вопросы и ответы, linux, oss, веб-приложение для контейнеров, мультиконтейнер, многоконтейнерный
services: app-service
documentationCenter: ''
author: msangapu-msft
manager: stefsch
editor: ''
ms.assetid: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/30/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: ec571555415a912a31b094722bd47f67210a0372
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2019
ms.locfileid: "67617353"
---
# <a name="azure-app-service-on-linux-faq"></a>Служба приложений Azure на платформе Linux: вопросы и ответы

После выпуска службы приложений на платформе Linux мы работаем над добавлением функций и усовершенствованиями нашей платформы. В этой статье приведены ответы на вопросы, которые наши клиенты нам недавно задавали.

Если у вас есть вопросы, задайте их в комментарии к этой статье.

## <a name="built-in-images"></a>Встроенные образы

**Мне необходимо выполнить разветвление встроенных контейнеров Docker, предоставляемых этой платформой. Где находятся эти файлы?**

Все файлы Docker можно найти на портале [GitHub](https://github.com/azure-app-service). Все контейнеры Docker можно найти в репозитории [Docker Hub](https://hub.docker.com/u/appsvc/).

<a id="#startup-file"></a>

**Какие значения будут доступны в разделе "Загрузочный файл" при настройке стека времени выполнения?**

| Стек           | Ожидаемое значение                                                                         |
|-----------------|----------------------------------------------------------------------------------------|
| Java SE         | команда запуска приложения JAR-ФАЙЛ (например, `java -jar my-app.jar --server.port=80`) |
| Tomcat, Wildfly | расположение сценария для выполнения всех необходимых конфигураций (например, `/home/site/deployments/tools/startup_script.sh`)          |
| Node.js         | файл конфигурации PM2 или свой файл сценария                                |
| .NET Core.       | имя скомпилированной библиотеки DLL, как `dotnet <myapp>.dll`                                 |
| Ruby            | скрипт Ruby, который вы хотите инициализировать приложение                     |

Эти команды или скрипты выполняются после запуска встроенного контейнера Docker, но прежде чем приложение запускается код.

## <a name="management"></a>Управление

**Что происходит при нажатии кнопки перезагрузки на портале Azure?**

Это действие аналогично перезагрузке Docker.

**Можно ли использовать Secure Shell (SSH) для подключения к виртуальной машине контейнера приложения?**

Да, вы можете это сделать при помощи сайта системы управления версиями (SCM).

> [!NOTE]
> Можно также подключиться к контейнеру приложения непосредственно с локального компьютера разработки с помощью SSH, SFTP или Visual Studio Code (для динамической отладки приложений Node.js). См. дополнительные сведения об [удаленной отладке и SSH в службе приложений в Linux](https://aka.ms/linux-debug).
>

**Как создать план службы приложений Linux с помощью пакета SDK или шаблона Azure Resource Manager?**

Для поля **reserved** службы приложений необходимо указать значение *true*.

## <a name="continuous-integration-and-deployment"></a>Непрерывная интеграция и развертывание

**Мое веб-приложение по-прежнему использует старый образ контейнера Docker после обновления образа в Docker Hub. Поддерживается ли непрерывная интеграция и развертывание пользовательских контейнеров?**

Да. Сведения о настройке непрерывной интеграции и развертывания для реестра контейнеров Azure или Docker Hub см. в статье [Непрерывное развертывание с использованием платформы Azure "Веб-приложения для контейнеров"](./app-service-linux-ci-cd.md). Для частных реестров контейнер можно обновить, остановив и снова запустив свое веб-приложение. Либо измените (добавьте) пустой параметр приложения, чтобы принудительно обновить контейнер.

**Поддерживаются ли промежуточные среды?**

Да.

**Можно ли развернуть веб-приложение с помощью *WebDeploy или MSDeploy*?**

Да. Для параметра `WEBSITE_WEBDEPLOY_USE_SCM` приложения необходимо задать значение *false*.

**При использовании веб-приложения Linux произошел сбой развертывания Git моего приложения. Как обойти эту проблему?**

В случае сбоя развертывания Git для веб-приложения Linux можно выбрать следующие варианты развертывания кода приложения:

- Используйте возможность непрерывной поставки (предварительная версия). Исходный код вашего приложения можно хранить в репозитории Azure DevOps Git или репозиторий GitHub для использования непрерывной поставки в Azure. Дополнительные сведения см. в блоге [Use Azure portal to setup Continuous Delivery for Web App On Linux](https://blogs.msdn.microsoft.com/devops/2017/05/10/use-azure-portal-to-setup-continuous-delivery-for-web-app-on-linux/) (Настройка непрерывной доставки для веб-приложения на платформе Linux с помощью портала Azure).

- Используйте [API развертывания ZIP-файлов](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). Чтобы использовать этот API, [установите SSH-подключение к веб-приложению](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-ssh-support) и перейдите в папку, в которую требуется развернуть код. Выполните следующий код:

   ```bash
   curl -X POST -u <user> --data-binary @<zipfile> https://{your-sitename}.scm.azurewebsites.net/api/zipdeploy
   ```

   Если происходит ошибка и отображается сообщение о том, что команда `curl` не найдена, убедитесь, что вы установили curl с помощью команды `apt-get install curl`, прежде чем выполнили предыдущую команду `curl`.

## <a name="language-support"></a>Поддержка языков

**Я хочу использовать WebSocket в своем приложении Node.js. Нужно ли для этого выбрать какие-либо специальные параметры или настройки?**

Да. На сервере в коде Node.js нужно отключить `perMessageDeflate`. Например, если вы используете socket.io, примените следующий код:

```nodejs
const io = require('socket.io')(server,{
  perMessageDeflate :false
});
```

**Поддерживаются ли некомпилированные приложения .NET Core?**

Да.

**Поддерживает ли Composer диспетчер зависимостей для приложений PHP?**

Да. Во время развертывания Git сервер Kudu определяет развертывание приложения PHP (на основе файла composer.lock), после чего Kudu активирует установку Composer.

## <a name="custom-containers"></a>Пользовательские контейнеры

**Я использую собственный пользовательский контейнер. Мне нужно, чтобы платформа подключила общий ресурс SMB в каталог `/home/`.**

Если `WEBSITES_ENABLE_APP_SERVICE_STORAGE` параметр **неуказанных** равным или *true*, `/home/` directory **будут совместно использоваться** экземпляров масштабирования и файлы, записанные **сохранится** между перезагрузками. Явная установка `WEBSITES_ENABLE_APP_SERVICE_STORAGE` для *false* приведет к отключению подключения.

**Мой пользовательский контейнер долго запускается, и платформа перезапускает контейнер до завершения его запуска.**

Можно настроить для платформы время ожидания перед перезапуском контейнера. Это можно сделать, указав для параметра приложения `WEBSITES_CONTAINER_START_TIME_LIMIT` нужное значение. Значение по умолчанию — 230 секунд, а максимальное значение — 1800 секунд.

**Какой формат используется для URL-адреса сервера частного реестра?**

Нужно указать полный URL-адрес реестра, включая `http://` или `https://`.

**Какой формат используется для имени образа в частном реестре?**

Добавьте полное имя образа, включая URL-адрес частного реестра (например, myacr.azurecr.io/dotnet:latest). [На портале нельзя вводить](https://feedback.azure.com/forums/169385-web-apps/suggestions/31304650) имена образов, которые используют пользовательский порт. Чтобы задать `docker-custom-image-name`, воспользуйтесь [программой командной строки `az`](https://docs.microsoft.com/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set).

**Можно ли предоставить несколько портов в образе настраиваемого контейнера?**

Мы не поддерживаем, предоставление доступа к более чем один порт.

**Можно ли использовать собственное хранилище?**

Да, [функция использования собственного хранилища](https://docs.microsoft.com/azure/app-service/containers/how-to-serve-content-from-azure-storage) находится на этапе предварительной версии.

**Почему мне не удается просматривать файловую систему или выполняющиеся процессы моего пользовательского контейнера на сайте SCM?**

Сайт SCM выполняется в отдельном контейнере. Поэтому вы не можете проверить файловую систему или выполняющиеся процессы контейнера приложения.

**Мой настраиваемый контейнер ожидает передачи данных через порт, отличающийся от порта 80. Как настроить приложение для перенаправления запросов в этот порт?**

Мы используем автоматическое определение порта. Вы также можете указать параметр приложения *WEBSITES_PORT* и присвоить ему номер нужного порта. Ранее платформа использовала параметр приложения *PORT*. Мы планируем отказаться от этого параметра приложения и использовать исключительно *WEBSITES_PORT*.

**Нужно ли реализовать протокол HTTPS в своем настраиваемом контейнере?**

Нет, платформа обрабатывает завершение протокола HTTPS на общих внешних интерфейсах.

## <a name="multi-container-with-docker-compose"></a>Несколькими контейнерами с помощью Docker Compose

**Как настроить Реестр контейнеров Azure (ACR) для использования с мультиконтейнером?**

Для использования ACR с мультиконтейнером **все образы контейнеров** должны размещаться на одном сервере реестра ACR. Как только они на один и тот же сервер реестра, необходимо будет создавать параметры приложения и затем обновите файл конфигурации Docker Compose образа имя ACR.

Создайте следующие параметры приложений:

- DOCKER_REGISTRY_SERVER_USERNAME
- DOCKER_REGISTRY_SERVER_URL (полный URL-адрес, например: `https://<server-name>.azurecr.io`)
- DOCKER_REGISTRY_SERVER_PASSWORD (включить доступ с правами администратора в параметрах ACR)

В файле конфигурации укажите образ ACR, как в следующем примере:

```yaml
image: <server-name>.azurecr.io/<image-name>:<tag>
```

**Как узнать, какой контейнер доступен из Интернета?**

- Доступ может быть открыт только к одному контейнеру
- Доступны только порты 80 и 8080 (предоставляемые порты)

Ниже приведены правила определения доступного контейнера в порядке приоритета.

- Параметру приложения `WEBSITES_WEB_CONTAINER_NAME` присвоено имя контейнера
- Первый контейнер, который определяет порт 80 или 8080
- Если ни одно из приведенных выше правил не выполняется, доступным (предоставляемым) будет первый контейнер, определенный в файле

## <a name="pricing-and-sla"></a>Цены и соглашение об уровне обслуживания

**Сколько сейчас стоит общедоступная служба?**

При использовании обычной модели ценообразования службы приложений Azure с вас снимается плата за количество часов работы вашего приложения.

## <a name="other-questions"></a>Другие вопросы

**Какие символы поддерживаются в именах параметров приложения?**

Для параметров приложения можно использовать только буквы (A–Z, a–z), цифры (0–9) и символ подчеркивания (_).

**Где можно отправить запрос на новые возможности?**

Вы можете отправить свои идеи на [форум отзывов о веб-приложениях](https://aka.ms/webapps-uservoice). Добавьте [Linux] в заголовок своей идеи.

## <a name="next-steps"></a>Следующие шаги

- [Общие сведения о службе приложений на платформе Linux](app-service-linux-intro.md).
- [Настройка промежуточных сред в службе приложений Azure](../../app-service/deploy-staging-slots.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json)
- [Непрерывное развертывание с использованием платформы Azure "Веб-приложения для контейнеров"](./app-service-linux-ci-cd.md).
