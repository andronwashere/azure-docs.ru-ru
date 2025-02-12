---
title: Развертывание виртуальной машины Azure с помощью Chef | Документация Майкрософт
description: Узнайте, как использовать Chef для автоматизированного развертывания и настройки виртуальных машин в Microsoft Azure.
services: virtual-machines-windows
documentationcenter: ''
author: diegoviso
manager: gwallace
tags: azure-service-management,azure-resource-manager
editor: ''
ms.assetid: 0b82ca70-89ed-496d-bb49-c04ae59b4523
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 07/09/2019
ms.author: diviso
ms.openlocfilehash: 74b92c277b1d6eaa0984e55a70459bad59c2bf84
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2019
ms.locfileid: "67719276"
---
# <a name="automating-azure-virtual-machine-deployment-with-chef"></a>Автоматизация развертывания виртуальной машины Azure с помощью Chef

Chef — это отличное средство для автоматизации и для настройки требуемого состояния.

После последнего выпуска облачного API средство Chef обеспечивает прозрачную интеграцию с Azure, позволяя подготовить и развернуть состояния конфигурации одной командой.

В этой статье показано, как настроить среду Chef для подготовки виртуальных машин Azure, а также рассматривается пошаговая процедура создания политики или руководства, после чего данная инструкция применяется на практике к виртуальной машине Azure.

## <a name="chef-basics"></a>Основы использования Chef
Перед началом [изучите основные принципы работы со средой Chef](https://www.chef.io/chef).

На приведенной ниже схеме показана высокоуровневая архитектура Chef.

![][2]

Среда Chef имеет три основных компонента архитектуры: сервер Chef, клиент Chef (узел) и рабочая станция Chef.

Сервер Chef является управляющим компонентом и доступен в двух вариантах: размещенное решение или локальное решение.

Клиент Chef (узел) — это агент, который находится на управляемых серверах.

Рабочая станция Chef — это рабочая станция администратора, где вы создаете политики и выполняете команды управления, и программный пакет инструментов Chef.

Как правило, вы видите _свою рабочую станцию_ в качестве расположения, в котором вы выполняете действия, а _рабочую станцию Chef_ — как пакет программного обеспечения.
Например, вы скачиваете команду knife как часть _рабочей станции Chef_, однако вы выполняете команды knife из _своей рабочей станции_, чтобы управлять инфраструктурой.

Chef также использует понятия руководств и инструкций, которые представляют собой политики, которые мы определяем и применяем к серверам.

## <a name="preparing-your-workstation"></a>Подготовка рабочей станции

Сначала подготовьте свою рабочую станцию, создав каталог для хранения файлов конфигурации и руководств Chef.

Создайте каталог с именем C:\Chef.

Скачайте и установите последнюю версию [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) версии на рабочей станции.

## <a name="configure-azure-service-principal"></a>Настройка субъекта-службы Azure

В простейшей терминов и субъект-службу Azure является учетной записью службы.   Мы будем использовать субъект-службу для создания ресурсов Azure на нашей рабочей станции Chef.  Для создания соответствующего субъекта-службы с необходимыми разрешениями, нужно выполнить следующие команды в PowerShell:
 
```powershell
Login-AzureRmAccount
Get-AzureRmSubscription
Select-AzureRmSubscription -SubscriptionName "<yourSubscriptionName>"
$myApplication = New-AzureRmADApplication -DisplayName "automation-app" -HomePage "https://chef-automation-test.com" -IdentifierUris "https://chef-automation-test.com" -Password "#1234p$wdchef19"
New-AzureRmADServicePrincipal -ApplicationId $myApplication.ApplicationId
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $myApplication.ApplicationId
```

Запишите идентификатор подписки, TenantID, ClientID и секрет клиента (пароль, установленный выше), он понадобится позже. 

## <a name="setup-chef-server"></a>Настройка сервера Chef

В этом руководстве предполагается, что вы зарегистрируетесь для получения размещенной учетной записи Chef.

Если вы еще не используете сервер Chef, вы можете:

* зарегистрироваться для [получения размещенной учетной записи Chef](https://manage.chef.io/signup), которая является самым быстрым способом приступить к работе с Chef;
* установить автономный сервер Chef на компьютере под управлением Linux, следуя [инструкциям по установке](https://docs.chef.io/install_server.html) из [документации Chef](https://docs.chef.io/).

### <a name="creating-a-hosted-chef-account"></a>Создание размещенной учетной записи Chef

Зарегистрируйте размещенную учетную запись Chef [здесь](https://manage.chef.io/signup).

В процессе регистрации вам будет предложено создать новую организацию.

![][3]

После создания организации скачайте начальный набор.

![][4]

> [!NOTE]
> Если появится предупреждение о том, что ключи будут сброшены, можно спокойно продолжать процедуру, так как у нас пока нет никакой настроенной инфраструктуры.
>

Этот ZIP-файл начального набора содержит файлы конфигурации и пользовательский ключ организации в каталоге `.chef`.

`organization-validator.pem` следует скачать отдельно, так как это закрытый ключ, а закрытые ключи не должны храниться на сервере Chef. Из [Chef управление](https://manage.chef.io/), перейдите в раздел «Администрирование» и выберите «Сбросить проверки ключ», который предоставляет файл для скачивания отдельно. Сохраните файл в каталоге C:\chef.

### <a name="configuring-your-chef-workstation"></a>Настройка рабочей станции Chef

Извлеките содержимое файла chef-starter.zip в каталог C:\chef.

Скопируйте все файлы из расположения chef-starter\chef-repo\.chef в каталог C:\chef.

Скопируйте файл `organization-validator.pem` в каталог C:\chef, в том случае, если он был сохранен в расположении C:\Downloads.

Теперь ваш каталог должен выглядеть, как показано ниже.

```powershell
    Directory: C:\Users\username\chef

Mode           LastWriteTime    Length Name
----           -------------    ------ ----
d-----    12/6/2018   6:41 PM           .chef
d-----    12/6/2018   5:40 PM           chef-repo
d-----    12/6/2018   5:38 PM           cookbooks
d-----    12/6/2018   5:38 PM           roles
-a----    12/6/2018   5:38 PM       495 .gitignore
-a----    12/6/2018   5:37 PM      1678 azuredocs-validator.pem
-a----    12/6/2018   5:38 PM      1674 user.pem
-a----    12/6/2018   5:53 PM       414 knife.rb
-a----    12/6/2018   5:38 PM      2341 README.md
```

Вы добавили пять файлов и четыре каталога, в том числе пустой каталог chef-repo, в корень C:\chef.

### <a name="edit-kniferb"></a>Изменение файла knife.rb

PEM-файлы содержат закрытые ключи администратора и организации для обеспечения связи, а файл knife.rb содержит конфигурацию knife. Нам потребуется изменить файл knife.rb.

Откройте файл knife.rb в редакторе по своему усмотрению. Неизмененный файл должен выглядеть следующим образом:

```rb
current_dir = File.dirname(__FILE__)
log_level           :info
log_location        STDOUT
node_name           "mynode"
client_key          "#{current_dir}/user.pem"
chef_server_url     "https://api.chef.io/organizations/myorg"
cookbook_path       ["#{current_dir}/cookbooks"]
```

Добавьте следующие сведения в файл knife.rb:

validation_client_name «myorg-проверяющий элемент управления»

validation_key           "#{current_dir}/myorg.pem"

knife[:azure_tenant_id] =         "0000000-1111-aaaa-bbbb-222222222222"

knife[:azure_subscription_id] =   "11111111-bbbbb-cccc-1111-222222222222"

knife[:azure_client_id] =         "11111111-bbbbb-cccc-1111-2222222222222"

knife[:azure_client_secret] =     "#1234p$wdchef19"


Эти строки будут убедитесь, что Knife ссылается на каталог подробных инструкций в разделе c:\chef\cookbooks, а также использует субъект-службу Azure, созданный во время операций Azure.

Теперь файл knife.rb должен выглядеть как в показанном ниже примере:

![][14]

<!--- Giant problem with this section: Chef 12 uses a config.rb instead of knife.rb
// However, the starter kit hasn't been updated
// So, I don't think this will even work on the modern Chef -->

<!--- update image [6] knife.rb -->

```rb
current_dir = File.dirname(__FILE__)
log_level                :info
log_location             STDOUT
node_name                "myorg"
client_key               "#{current_dir}/myorg.pem"
validation_client_name   "myorg-validator"
validation_key           "#{current_dir}/myorg-validator.pem"
chef_server_url          "https://api.chef.io/organizations/myorg"
cookbook_path            ["#{current_dir}/../cookbooks"]
knife[:azure_tenant_id] = "0000000-1111-aaaa-bbbb-222222222222"
knife[:azure_subscription_id] = "11111111-bbbbb-cccc-1111-222222222222"
knife[:azure_client_id] = "11111111-bbbbb-cccc-1111-2222222222222"
knife[:azure_client_secret] = "#1234p$wdchef19"
```

## <a name="install-chef-workstation"></a>Установка рабочей станции Chef

Затем [скачайте и установите](https://downloads.chef.io/chef-workstation/) рабочую станцию Chef.
Установите рабочую станцию Chef в расположение по умолчанию. Установка может занять несколько минут.

На рабочем столе вы увидите CW PowerShell, которая представляет собой рабочую среду, загруженную вместе со средством, необходимым для взаимодействия с продуктами Chef. CW PowerShell доступны новые специальные команды, такие как `chef-run` команд интерфейса командной строки также как традиционные Chef, таких как `chef`. Просмотрите установленную версию рабочей станции Chef и инструментов Chef с помощью `chef -v`. Вы также можете проверить версию рабочей станции, выбрав About Chef Workstation (О рабочей станции Chef) в приложении рабочей станции Chef.

Команда `chef --version` должна вернуть примерно следующее:

```
Chef Workstation: 0.4.2
  chef-run: 0.3.0
  chef-client: 15.0.300
  delivery-cli: 0.0.52 (9d07501a3b347cc687c902319d23dc32dd5fa621)
  berks: 7.0.8
  test-kitchen: 2.2.5
  inspec: 4.3.2
```

> [!NOTE]
> Порядок путей важен! Если пути opscode указаны в неправильном порядке, возникнут проблемы.
>

Прежде чем продолжить, перезагрузите рабочую станцию.

### <a name="install-knife-azure"></a>Установка Knife для Azure

В руководстве предполагается, что вы используете Azure Resource Manager для взаимодействия со своей виртуальной машиной.

Установите расширение Knife для Azure. Это предоставляет Knife "подключаемый модуль Azure".

Выполните следующую команду:

    chef gem install knife-azure ––pre

> [!NOTE]
> Благодаря аргументу –pre вы получите последнюю версию-кандидат подключаемого модуля Knife для Azure, предоставляющего доступ к актуальному набору API.
>
>

Вполне вероятно, что в это же время будет установлен набор зависимостей.

![][8]

Чтобы проверить правильность настройки, выполните следующую команду.

    knife azurerm server list

Если все настроено правильно, появится прокручиваемый список доступных образов Azure.

Поздравляем! Рабочая станция настроена.

## <a name="creating-a-cookbook"></a>Создание подробной инструкции

Подробная инструкция Chef используется для определения набора команд, которые необходимо выполнить на управляемом клиенте. Создание руководства не вызывает проблем, поэтому для создания шаблона руководства используйте команду **chef generate cookbook**. Это руководство предназначено для веб-сервера, который автоматически развертывает службы IIS.

В каталоге C:\Chef выполните следующую команду:

    chef generate cookbook webserver

Эта команда создает набор файлов в каталоге C:\Chef\cookbooks\webserver. Затем определите набор команд, которые клиент Chef будет выполнять на управляемой виртуальной машине.

Команды хранятся в файле default.rb. В этом файле мы определим набор команд, который устанавливает службы IIS, запускает их и копирует файл шаблона в папку wwwroot.

Измените файл C:\chef\cookbooks\webserver\recipes\default.rb и добавьте в него указанные ниже строки.

    powershell_script 'Install IIS' do
         action :run
         code 'add-windowsfeature Web-Server'
    end

    service 'w3svc' do
         action [ :enable, :start ]
    end

    template 'c:\inetpub\wwwroot\Default.htm' do
         source 'Default.htm.erb'
         rights :read, 'Everyone'
    end

После завершения работы сохраните файл.

## <a name="creating-a-template"></a>Создание шаблона
На этом шаге вы создадите файл шаблона, который будет использоваться в качестве страницы default.html.

Выполните следующую команду, чтобы создать шаблон:

    chef generate template webserver Default.htm

Перейдите к файлу `C:\chef\cookbooks\webserver\templates\default\Default.htm.erb`. Добавьте в него простой HTML-код Hello World и сохраните файл.

## <a name="upload-the-cookbook-to-the-chef-server"></a>Передача подробной инструкции на сервер Chef
На этом шаге вы создаете копию руководства, созданного на локальном компьютере, и отправляете ее на размещенный сервер Chef. После передачи руководство появится на вкладке **Policy** (Политика).

    knife cookbook upload webserver

![][9]

## <a name="deploy-a-virtual-machine-with-knife-azure"></a>Развертывание виртуальной машины с помощью Knife Azure
Разверните виртуальную машину Azure и примените подробную инструкцию из руководства Webserver, которая установит нашу веб-службу IIS и веб-страницу по умолчанию.

Чтобы сделать это, используйте **knife azurerm server создать** команды.

Пример этой команды приводится ниже.

    knife azurerm server create `
    --azure-resource-group-name rg-chefdeployment `
    --azure-storage-account store `
    --azure-vm-name chefvm `
    --azure-vm-size 'Standard_DS2_v2' `
    --azure-service-location 'westus' `
    --azure-image-reference-offer 'WindowsServer' `
    --azure-image-reference-publisher 'MicrosoftWindowsServer' `
    --azure-image-reference-sku '2016-Datacenter' `
    --azure-image-reference-version 'latest' `
    -x myuser -P myPassword123 `
    --tcp-endpoints '80,3389' `
    --chef-daemon-interval 1 `
    -r "recipe[webserver]"


Приведенный выше пример создаст Standard_DS2_v2 виртуальной машины с Windows Server 2016, установленные в западной части США. Подставьте свои переменные и запустите команду.

> [!NOTE]
> Мы автоматизируем работу правил сетевых фильтров конечной точки, используя параметр –tcp-endpoints в командной строке. Здесь открыты порты 80 и 3389, чтобы обеспечить доступ к веб-страницы и сеанс RDP.
>
>

После выполнения команды перейдите на портал Azure, где можно наблюдать за началом подготовки виртуальной машины.

![][15]

Откроется командная строка.

![][16]

После завершения развертывания будет отображаться общедоступный IP-адрес новой виртуальной машины на завершении развертывания, можно скопировать это и вставьте его в веб-браузер и просмотреть веб-сайт, который был развернут. Когда мы развернута виртуальная машина мы открыли порт 80, поэтому он должен быть доступен извне.   

![][11]

В этом примере используется изобретательный HTML-код.

Вы также можете просмотреть состояние узла [Chef управление](https://manage.chef.io/). 

![][17]

Не забывайте, что вы также можете подключиться через сеанс протокола удаленного рабочего стола с портала Azure через порт 3389.

Благодарим вас! Начинайте использовать подход "инфраструктура как код" прямо сейчас с помощью Azure!

<!--Image references-->
[2]: media/chef-automation/2.png
[3]: media/chef-automation/3.png
[4]: media/chef-automation/4.png
[5]: media/chef-automation/5.png
[6]: media/chef-automation/6.png
[7]: media/chef-automation/7.png
[8]: media/chef-automation/8.png
[9]: media/chef-automation/9.png
[10]: media/chef-automation/10.png
[11]: media/chef-automation/11.png
[13]: media/chef-automation/13.png
[14]: media/chef-automation/14.png
[15]: media/chef-automation/15.png
[16]: media/chef-automation/16.png
[17]: media/chef-automation/17.png


<!--Link references-->
